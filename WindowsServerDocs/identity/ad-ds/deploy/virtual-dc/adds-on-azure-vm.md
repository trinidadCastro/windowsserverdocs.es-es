---
title: Instalación de Active Directory Domain Services en una máquina virtual de Azure
description: Cómo crear un nuevo bosque de Active Directory en una máquina virtual (VM) en una máquina virtual de Azure.
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 04/11/2019
ms.topic: article
ms.openlocfilehash: 98725e194226f048de5bc8332c02ec54c7525ee1
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88940125"
---
# <a name="install-a-new-active-directory-forest-using-azure-cli"></a>Install a new Active Directory forest using Azure CLI (Instalación de un nuevo bosque de Active Directory en la CLI de Azure)

AD DS se pueden ejecutar en una máquina virtual (VM) de Azure de la misma manera que se ejecuta en muchas instancias locales. Este artículo le guiará a través de la implementación de un nuevo bosque de AD DS, en dos controladores de dominio nuevos, en un conjunto de disponibilidad de Azure mediante los Azure Portal y CLI de Azure. Muchos clientes encuentran esta guía útil a la hora de crear un laboratorio o de preparar la implementación de controladores de dominio en Azure.

## <a name="components"></a>Componentes

* Un grupo de recursos en el que colocar todo.
* Un [Virtual Network de Azure](/azure/virtual-network/virtual-networks-overview.md), una subred, un grupo de seguridad de red y una regla para permitir el acceso RDP a las máquinas virtuales.
* Un [conjunto de disponibilidad](/azure/virtual-machines/windows/regions-and-availability#availability-sets) de máquinas virtuales de Azure para colocar dos controladores de dominio de Active Directory Domain Services (AD DS) en.
* Dos máquinas virtuales de Azure para ejecutar AD DS y DNS.

### <a name="items-that-are-not-covered"></a>Elementos no incluidos

* [Creación de una conexión VPN de sitio a sitio](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) desde una ubicación local
* [Protección del tráfico de red en Azure](/azure/security/azure-security-network-security-best-practices.md)
* [Diseño de la topología de sitio](../../plan/designing-the-site-topology.md)
* [Planeación de la ubicación del rol de maestro de operaciones](../../plan/planning-operations-master-role-placement.md)
* [Implementación de Azure AD Connect para sincronizar identidades con Azure AD](/azure/active-directory/hybrid/how-to-connect-install-express)

## <a name="build-the-test-environment"></a>Compilar el entorno de prueba

Usamos el [Azure portal](https://portal.azure.com) y [CLI de Azure](/cli/azure/overview?view=azure-cli-latest) para crear el entorno.

La CLI de Azure se usa para crear y administrar recursos de Azure desde la línea de comandos o en scripts. En este tutorial se detalla el uso de la CLI de Azure para implementar máquinas virtuales que ejecutan Windows Server 2019. Una vez completada la implementación, nos conectaremos a los servidores e instalaremos AD DS.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

El siguiente script automatiza el proceso de creación de dos máquinas virtuales con Windows Server 2019, con el fin de crear controladores de dominio para un nuevo bosque de Active Directory en Azure. Un administrador puede modificar las variables siguientes para que se adapten a sus necesidades y, a continuación, completar, como una operación. El script crea el grupo de recursos necesario, el grupo de seguridad de red con una regla de tráfico para Escritorio remoto, la red virtual y la subred y el grupo de disponibilidad. Cada una de las máquinas virtuales se compila con un disco de datos de 20 GB con el almacenamiento en caché deshabilitado para la instalación de AD DS.

El siguiente script se puede ejecutar directamente desde el Azure Portal. Si decide instalar y usar la CLI localmente, para esta guía de inicio rápido es preciso que ejecute la CLI de Azure versión 2.0.4 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).

| Nombre de variable | Propósito |
| :---: | :--- |
| AdminUsername | Nombre de usuario que se configurará en cada máquina virtual como administrador local. |
| AdminPassword | Contraseña de texto no cifrado que se configurará en cada máquina virtual como la contraseña de administrador local. |
| ResourceGroupName | Nombre que se va a usar para el grupo de recursos. No debe duplicar un nombre existente. |
| Location | Nombre de ubicación de Azure en el que desea realizar la implementación. Muestra las regiones admitidas para la suscripción actual mediante `az account list-locations` . |
| VNetName | Nombre para asignar la red virtual de Azure no debe duplicar un nombre existente. |
| VNetAddress | Ámbito de IP que se usará para las redes de Azure. No debe duplicar un intervalo existente. |
| SubnetName | Nombre para asignar la subred IP. No debe duplicar un nombre existente. |
| SubnetAddress | Dirección de subred para los controladores de dominio. Debe ser una subred dentro de la red virtual. |
| Conjunto | Nombre del conjunto de disponibilidad al que se unirán las máquinas virtuales del controlador de dominio. |
| VMSize | Tamaño de máquina virtual de Azure estándar disponible en la ubicación para la implementación. |
| Subdisks | Tamaño en GB del disco de datos en el que se instala AD DS. |
| DomainController1 | Nombre del primer controlador de dominio. |
| DC1IP | Dirección IP del primer controlador de dominio. |
| DomainController2 | Nombre del segundo controlador de dominio. |
| DC2IP | Dirección IP del segundo controlador de dominio. |

```azurecli
#Update based on your organizational requirements
Location=westus2
ResourceGroupName=ADonAzureVMs
NetworkSecurityGroup=NSG-DomainControllers
VNetName=VNet-AzureVMsWestUS2
VNetAddress=10.10.0.0/16
SubnetName=Subnet-AzureDCsWestUS2
SubnetAddress=10.10.10.0/24
AvailabilitySet=DomainControllers
VMSize=Standard_DS1_v2
DataDiskSize=20
AdminUsername=azureuser
AdminPassword=ChangeMe123456
DomainController1=AZDC01
DC1IP=10.10.10.11
DomainController2=AZDC02
DC2IP=10.10.10.12

# Create a resource group.
az group create --name $ResourceGroupName \
                --location $Location

# Create a network security group
az network nsg create --name $NetworkSecurityGroup \
                      --resource-group $ResourceGroupName \
                      --location $Location

# Create a network security group rule for port 3389.
az network nsg rule create --name PermitRDP \
                           --nsg-name $NetworkSecurityGroup \
                           --priority 1000 \
                           --resource-group $ResourceGroupName \
                           --access Allow \
                           --source-address-prefixes "*" \
                           --source-port-ranges "*" \
                           --direction Inbound \
                           --destination-port-ranges 3389

# Create a virtual network.
az network vnet create --name $VNetName \
                       --resource-group $ResourceGroupName \
                       --address-prefixes $VNetAddress \
                       --location $Location \

# Create a subnet
az network vnet subnet create --address-prefix $SubnetAddress \
                              --name $SubnetName \
                              --resource-group $ResourceGroupName \
                              --vnet-name $VNetName \
                              --network-security-group $NetworkSecurityGroup

# Create an availability set.
az vm availability-set create --name $AvailabilitySet \
                              --resource-group $ResourceGroupName \
                              --location $Location

# Create two virtual machines.
az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController1 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC1IP \
    --no-wait

az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController2 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC2IP

```

## <a name="dns-and-active-directory"></a>DNS y Active Directory

Si las máquinas virtuales de Azure creadas como parte de este proceso serán una extensión de una infraestructura de Active Directory local existente, se debe cambiar la configuración de DNS de la red virtual para que incluya los servidores DNS locales antes de la implementación. Este paso es importante para permitir que los controladores de dominio recién creados en Azure resuelvan los recursos locales y permitan que se produzca la replicación. Puede encontrar más información sobre DNS, Azure y cómo configurar las opciones en la sección resolución de [nombres que usa su propio servidor DNS](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

Después de promocionar los nuevos controladores de dominio en Azure, tendrán que establecerse en los servidores DNS principal y secundario de la red virtual, y los servidores DNS locales se degradarán a terciario y más. Puede encontrar más información sobre cómo cambiar los servidores DNS en el artículo [creación, cambio o eliminación de una red virtual](/azure/virtual-network/manage-virtual-network#change-dns-servers).

Puede encontrar información sobre cómo extender una red local a Azure en el artículo creación de [una conexión VPN de sitio a sitio](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

## <a name="configure-the-vms-and-install-active-directory-domain-services"></a>Configurar las máquinas virtuales e instalar Active Directory Domain Services

Una vez completado el script, busque el [Azure portal](https://portal.azure.com)y, a continuación, **máquinas virtuales**.

### <a name="configure-the-first-domain-controller"></a>Configuración del primer controlador de dominio

Conéctese a AZDC01 con las credenciales que proporcionó en el script.

* Inicialice y formatee el disco de datos como F:
   * Abra el menú Inicio y vaya a **Administración de equipos** .
   * Vaya a **Storage**  >  **Disk Management**
   * Inicializar el disco como MBR
   * Cree un nuevo volumen simple y asigne la letra de unidad F: puede proporcionar una etiqueta de volumen si lo desea.
* Instalación de Active Directory Domain Services mediante Administrador del servidor
* Promover el controlador de dominio como el primero de un bosque nuevo
   * Dejar el servidor de sistema de nombres de dominio (DNS) y el catálogo global (GC) comprobados en la página Opciones del controlador de dominio
   * Especifique una contraseña de Modo de restauración de servicios de directorio en función de los requisitos de su organización
   * Cambie las rutas de acceso de C: para que apunten a la unidad F: que creamos cuando se le solicitó su ubicación
   * Revise las selecciones realizadas en el asistente y elija **siguiente** .

> [!NOTE]
> La comprobación de requisitos previos le advertirá de que el adaptador de red físico no tiene asignada una dirección IP estática. puede omitir esta opción de forma segura a medida que se asignan direcciones IP estáticas en la red virtual de Azure.

* Elija **instalar** .

Cuando el asistente complete el proceso de instalación, se reiniciará la máquina virtual.

Cuando se haya completado el reinicio de la máquina virtual, vuelva a iniciar sesión con las credenciales usadas antes, pero esta vez como miembro del dominio que creó.

   > [!NOTE]
   > El primer inicio de sesión después de la promoción a un controlador de dominio puede tardar más de lo normal y esto es correcto. Captar una taza de té, café, agua u otra bebida de elección.

Las [redes virtuales de Azure ahora admiten IPv6](/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) , pero en caso de que quiera configurar las máquinas virtuales para que prefieran IPv4 a través de IPv6, puede encontrar información sobre cómo completar esta tarea en el artículo de Knowledge base [Guía para configurar IPv6 en Windows para usuarios avanzados](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users).

### <a name="configure-the-second-domain-controller"></a>Configurar el segundo controlador de dominio

Conéctese a AZDC02 con las credenciales que proporcionó en el script.

* Inicialice y formatee el disco de datos como F:
   * Abra el menú Inicio y vaya a **Administración de equipos** .
   * Vaya a **Storage**  >  **Disk Management**
   * Inicializar el disco como MBR
   * Cree un nuevo volumen simple y asigne la letra de unidad F: (puede proporcionar una etiqueta de volumen si lo desea)
* Instalación de Active Directory Domain Services mediante Administrador del servidor
* Promover el controlador de dominio
   * Agregar un controlador de dominio a un dominio existente: CONTOSO.com
   * Proporcionar credenciales para realizar la operación
   * Cambie las rutas de acceso de C: para que apunten a la unidad F: que creamos cuando se le solicitó su ubicación
   * Asegurarse de que el servidor de sistema de nombres de dominio (DNS) y el catálogo global (GC) están comprobados en la página Opciones del controlador de dominio
   * Especifique una contraseña de Modo de restauración de servicios de directorio en función de los requisitos de su organización
   * Revise las selecciones realizadas en el asistente y elija **siguiente** .

> [!NOTE]
> La comprobación de requisitos previos le advertirá de que el adaptador de red físico no tiene asignada una dirección IP estática. Puede pasarlo por alto sin ningún riesgo, ya que las direcciones IP estáticas se asignan en la red virtual de Azure.

* Elija **instalar** .

Cuando el asistente complete el proceso de instalación, se reiniciará la máquina virtual.

Cuando se haya completado el reinicio de la máquina virtual, vuelva a iniciar sesión con las credenciales usadas antes, pero esta vez como miembro del dominio CONTOSO.com

Las [redes virtuales de Azure ahora admiten IPv6](/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) , pero en caso de que quiera configurar las máquinas virtuales para que prefieran IPv4 a través de IPv6, puede encontrar información sobre cómo completar esta tarea en el artículo de Knowledge base [Guía para configurar IPv6 en Windows para usuarios avanzados](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users).

### <a name="configure-dns"></a>Configurar el DNS

Después de promocionar los nuevos controladores de dominio en Azure, tendrán que establecerse en los servidores DNS principal y secundario de la red virtual, y los servidores DNS locales se degradarán a terciario y más. Puede encontrar más información sobre cómo cambiar los servidores DNS en el artículo [creación, cambio o eliminación de una red virtual](/azure/virtual-network/manage-virtual-network#change-dns-servers).

### <a name="wrap-up"></a>Encapsulado

En este momento, el entorno tiene un par de controladores de dominio y se ha configurado la red virtual de Azure para que se puedan agregar más servidores al entorno. En este momento, se deben completar las tareas posteriores a la instalación de Active Directory Domain Services, como la configuración de sitios y servicios, auditoría, copia de seguridad y protección de la cuenta de administrador integrada.

## <a name="removing-the-environment"></a>Quitar el entorno

Para quitar el entorno, cuando haya completado las pruebas, se puede eliminar el grupo de recursos que se creó anteriormente. Este paso quita todos los componentes que forman parte de ese grupo de recursos.

### <a name="remove-using-the-azure-portal"></a>Quitar mediante el Azure Portal

En el Azure Portal, vaya a **grupos de recursos** y elija el grupo de recursos que hemos creado (en este ejemplo, ADonAzureVMs) y, luego, seleccione **Eliminar grupo de recursos**. El proceso solicita confirmación antes de eliminar todos los recursos incluidos en el grupo de recursos.

### <a name="remove-using-the-azure-cli"></a>Quitar mediante el CLI de Azure

En CLI de Azure ejecute el siguiente comando:

```azurecli
az group delete --name ADonAzureVMs
```

## <a name="next-steps"></a>Pasos siguientes

* [Virtualización segura de Servicios de dominio de Active Directory (AD DS)](../../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)
* [Azure AD Connect](/azure/active-directory/connect/active-directory-aadconnect-get-started-express)
* [Copia de seguridad y recuperación](/azure/virtual-machines/windows/backup-recovery)
* [Conectividad VPN de sitio a sitio](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Supervisión](/azure/virtual-machines/windows/monitor)
* [Seguridad y directivas](/azure/virtual-machines/windows/security-policy)
* [Mantenimiento y actualizaciones](/azure/virtual-machines/windows/maintenance-and-updates)
