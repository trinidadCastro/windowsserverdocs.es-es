---
title: Contenedores de conectarse a una red Virtual
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.openlocfilehash: 801cf4b8f71935eb72d820d47e523a310fa64562
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Conectar los extremos de contenedor a una red virtual del inquilino

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema muestra cómo conectarse a extremos de contenedor a una red virtual existente de inquilino creada a través de la pila de Microsoft Software definido de redes (SDN). Usaremos la *l2bridge* (y, opcionalmente, *l2tunnel*) disponibles con el complemento de libnetwork de Windows para Docker crear una red de contenedores en la máquina virtual de host (inquilino) de contenedor de controladores de red.

Como se indica en la [contenedor redes](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) tema en MSDN, existen varios controladores de red mediante Docker en Windows. Son los controladores más adecuados para SDN *l2bridge* y *l2tunnel *. Ambos controladores, cada extremo del contenedor se está en la misma subred virtual que la máquina virtual de host (inquilino) de contenedor. Las direcciones IP para los extremos de contenedor se asignan dinámicamente mediante el Host de red servicio (SNP) mediante el complemento de nube privada. Los extremos de contenedor tienen direcciones IP únicas pero comparten la misma dirección MAC del equipo host (inquilino) virtual contenedor debido a la traducción de direcciones de nivel 2. Directiva de red (por ejemplo: ACL, encapsulación y QoS) para estos extremos de contenedor se aplican en el host de Hyper-V físico como recibidos por el controlador de red y se definen en sistemas de administración de nivel superior. Hay una ligera diferencia entre el *l2bridge* y *l2tunnel* controladores que se explica a continuación.

- **Puente de L2** -extremos de contenedor que residen en la misma máquina virtual de host de contenedor y se encuentran en la misma subred tienen todo el tráfico de red en el conmutador de Hyper-V virtual. Extremos de contenedor que residen en el host de contenedor diferentes máquinas virtuales o que están en diferentes subredes tienen el tráfico que se reenvía al host de Hyper-V físico. Dado que el tráfico de red entre contenedores en el mismo host y en la misma subred no fluyan hacia el host físico, se aplicará ninguna directiva de red. Solo se aplica la directiva para el tráfico de red de contenedor entre hosts o entre subred.  
 
- **Túnel L2** - *todos los* el tráfico de red entre dos extremos de contenedor se reenvía al host de Hyper-V física independientemente de host o subred. Se aplica la directiva de red para el tráfico de red entre subred y entre hosts.   

>[!NOTE]
>Estos modos de red no funcionan para conectar extremos del contenedor de windows a una red virtual de inquilino en la nube pública de Azure

## <a name="prerequistes"></a>Requisitos previos
 * Se ha implementado una infraestructura SDN con el controlador de red
 * Se ha creado una red virtual del inquilino
 * Se ha implementado una máquina virtual de inquilino con la característica de contenedor de Windows habilitada, el Docker instalado y la característica de Hyper-V habilitado

>[!Note]
>[Virtualización anidada](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/user_guide/nesting) y exponer extensiones de virtualización no es necesario a menos que es necesario utilizar HyperV de Hyper-V contenedores la característica para instalar archivos binarios de varias redes l2bridge y l2tunnel

```powershell
# To install HyperV feature without checks for nested virtualization
dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
```

 

## <a name="workflow"></a>Flujo de trabajo

1. [Agregar varias configuraciones de IP a un recurso existente de VM NIC a través de controlador de red](#Add) (Host Hyper-V)
2. [Habilitar el proxy de red en el equipo host para asignar direcciones IP de entidad emisora de certificados para los extremos de contenedor](#Enable) (Host Hyper-V) 
3. [Instalar el complemento para asignar direcciones IP de entidad emisora de certificados a los extremos del contenedor de nube privada](#Install) (contenedor de máquinas virtuales) 
4. [Crear un *l2bridge* o *l2tunnel* red con docker](#Create) (contenedor de máquinas virtuales) 
 
>[!NOTE]
>No se admiten varias configuraciones de IP en recursos de VM NIC creados a través de System Center Virtual Machine Manager. Se recomienda para estos tipos de implementaciones que crees el recurso de VM NIC fuera de banda mediante PowerShell del controlador de red.

### <a name="Add"></a>1. agregar varias configuraciones de IP

Para este ejemplo, suponemos que ya tiene una configuración IP con la dirección IP de 192.168.1.9 la NIC VM de la máquina virtual de inquilino y está unida a un identificador de recurso VNet de 'VNet1' y recursos de subred de máquina virtual de 'Subred1' en la subred IP 192.168.1.0/24. Agregaremos 10 direcciones IP para contenedores de 192.168.1.101 - 192.168.1.110.

```powershell
Import-Module NetworkController

# Specify Network Controller REST IP or FQDN
$uri = "<NC REST IP or FQDN>"
$vnetResourceId = "VNet1"
$vsubnetResourceId = "Subnet1"

$vmnic= Get-NetworkControllerNetworkInterface -ConnectionUri $uri | where {$_.properties.IpConfigurations.Properties.PrivateIPAddress -eq "192.168.1.9" }
$vmsubnet = Get-NetworkControllerVirtualSubnet -VirtualNetworkId $vnetResourceId -ResourceId $vsubnetResourceId -ConnectionUri $uri

# For this demo, we will assume an ACL has already been defined; any ACL can be applied here
$allowallacl = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"


foreach ($i in 1..10)
{
    $newipconfig = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $props = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties

    $resourceid = "IP_192_168_1_1"
    if ($i -eq 10) 
    {
        $resourceid += "10"
        $ipstr = "192.168.1.110"
    }
    else
    {
        $resourceid += "0$i"
        $ipstr = "192.168.1.10$i"
    }
    
    $newipconfig.ResourceId = $resourceid
    $props.PrivateIPAddress = $ipstr    
    
    $props.PrivateIPAllocationMethod = "Static"
    $props.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $props.Subnet.ResourceRef = $vmsubnet.ResourceRef
    $props.AccessControlList = new-object Microsoft.Windows.NetworkController.AccessControlList
    $props.AccessControlList.ResourceRef = $allowallacl.ResourceRef

    $newipconfig.Properties = $props
    $vmnic.Properties.IpConfigurations += $newipconfig
}

New-NetworkControllerNetworkInterface -ResourceId $vmnic.ResourceId -Properties $vmnic.Properties -ConnectionUri $uri
```

### <a name="Enable"></a>2. activar al Proxy de red

[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1>)

Ejecutar este script el **Host Hyper-V** que hospeda la máquina de contenedor host (inquilino) virtual para habilitar el proxy de red asignar varias direcciones IP para la máquina virtual de host de contenedor.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="Install"></a>3. instala el complemento de nube privada

[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)

Ejecutar este script dentro de la **máquina virtual de host (inquilino) de contenedor** para permitir que el Host de red servicio (SNP) para comunicarse con el proxy de red en el Host de Hyper-V.

```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="Create"></a>4. crear una *l2bridge* contenedor red

En la **máquina virtual de host (inquilino) de contenedor** usar la `docker network create`comando para crear una red l2bridge

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>Asignación de IP estáticas no es compatible con *l2bridge* o *l2tunnel* contenedor redes cuando se usa con la pila de SDN de Microsoft.

## <a name="more-information"></a>Obtener más información
Para obtener infortation más acerca de cómo implementar una infraestructura SDN, consulta [implementar una infraestructura de red definido de Software](https://technet.microsoft.com/en-us/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

