---
title: Conexión de puntos de conexión de contenedor a una red virtual de inquilino
description: En este tema, le mostraremos cómo conectar los puntos de conexión de contenedor a una red virtual de inquilino existente creada mediante SDN. Use el controlador de red l2bridge (y, opcionalmente l2tunnel) disponible con el complemento libnetwork de Windows para Docker para crear una red de contenedor en la máquina virtual del inquilino.
manager: ravirao
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: lizross
author: jmesser81
ms.date: 08/24/2018
ms.openlocfilehash: 5673cb6f808f37fb7737e22cf93c3984073e4f48
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309836"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Conexión de puntos de conexión de contenedor a una red virtual de inquilino

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, le mostraremos cómo conectar los puntos de conexión de contenedor a una red virtual de inquilino existente creada mediante SDN. Use el controlador de red *l2bridge* (y, opcionalmente *l2tunnel*) disponible con el complemento Libnetwork de Windows para Docker para crear una red de contenedor en la máquina virtual del inquilino.

En el tema [controladores de red de contenedor](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies) , hemos explicado que hay varios controladores de red disponibles a través de Docker en Windows. En el caso de SDN, use los controladores *l2bridge* y *l2tunnel* . Para ambos controladores, cada punto de conexión de contenedor está en la misma subred virtual que la máquina virtual del host de contenedor (inquilino). 

El servicio de red de host (SNP), a través del complemento de nube privada, asigna dinámicamente las direcciones IP de los puntos de conexión del contenedor. Los puntos de conexión del contenedor tienen direcciones IP únicas pero comparten la misma dirección MAC de la máquina virtual del host de contenedor (inquilino) debido a la traducción de direcciones de nivel 2. 

La Directiva de red (ACL, encapsulación y QoS) para estos puntos de conexión de contenedor se aplican en el host de Hyper-V físico tal como lo recibe el controlador de red y se definen en los sistemas de administración de nivel superior. 

La diferencia entre los controladores *l2bridge* y *l2tunnel* son:


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Extremos de contenedor que residen en: <ul><li>La misma máquina virtual del host de contenedor y en la misma subred tienen todo el tráfico de red enlazado en el conmutador virtual de Hyper-V. </li><li>Las distintas máquinas virtuales de host de contenedor o en subredes diferentes tienen su tráfico reenviado al host de Hyper-V físico. </li></ul>No se aplica la Directiva de red porque el tráfico de red entre contenedores en el mismo host y en la misma subred no fluye al host físico. La Directiva de red se aplica solo al tráfico de red de contenedor entre subredes o entre hosts. | *Todo* el tráfico de red entre dos puntos de conexión de contenedor se reenvía al host de Hyper-V físico, independientemente del host o de la subred. La Directiva de red se aplica a todo el tráfico de red entre subredes y entre hosts. |

---

>[!NOTE]
>Estos modos de red no funcionan para conectar los puntos de conexión de contenedor de Windows a una red virtual de inquilino en la nube pública de Azure.


## <a name="prerequisites"></a>Requisitos previos
-  Una infraestructura de SDN implementada con la controladora de red.
-  Se ha creado una red virtual de inquilino.
-  Una máquina virtual de inquilino implementada con la característica de contenedor de Windows habilitada, Docker instalado y la característica de Hyper-V habilitada. La característica Hyper-V es necesaria para instalar varios archivos binarios para las redes l2bridge y l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>La [Virtualización anidada](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting) y la exposición de las extensiones de virtualización no son necesarias a menos que se usen contenedores de Hyper-V. 


## <a name="workflow"></a>Flujo de trabajo

[1. agregar varias configuraciones de IP a un recurso de NIC de máquina virtual existente a través de la controladora de red (host de Hyper-V)](#1-add-multiple-ip-configurations)
[2. Habilite el proxy de red en el host para asignar las direcciones IP de CA para los puntos de conexión de contenedor (host de Hyper-V)](#2-enable-the-network-proxy)
[3. Instale el complemento de nube privada para asignar direcciones IP de CA a puntos de conexión de contenedor (VM de host de contenedor)](#3-install-the-private-cloud-plug-in)
[4. Creación de una red de *l2bridge* o *l2tunnel* mediante Docker (máquina virtual de host de contenedor)](#4-create-an-l2bridge-container-network)

>[!NOTE]
>No se admiten varias configuraciones de IP en los recursos de NIC de máquina virtual creados mediante System Center Virtual Machine Manager. Se recomienda para estos tipos de implementaciones que crea el recurso de NIC de VM fuera de banda mediante el controlador de red PowerShell.

### <a name="1-add-multiple-ip-configurations"></a>1. agregar varias configuraciones de IP
En este paso, asumimos que la NIC de VM de la máquina virtual del inquilino tiene una configuración de IP con la dirección IP de 192.168.1.9 y está conectada a un identificador de recurso de red virtual de "VNet1" y un recurso de subred de VM de "Subnet1" en la subred IP 192.168.1.0/24. Agregamos 10 direcciones IP para los contenedores de 192.168.1.101-192.168.1.110.

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

### <a name="2-enable-the-network-proxy"></a>2. habilitar el proxy de red
En este paso, habilitará el proxy de red para asignar varias direcciones IP para la máquina virtual del host de contenedor. 

Para habilitar el proxy de red, ejecute el script [ConfigureMCNP. PS1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) en el **host de Hyper-V** que hospeda la máquina virtual del host de contenedor (inquilino).

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. instalar el complemento de nube privada
En este paso, instalará un complemento para permitir que el SNP se comunique con el proxy de red en el host de Hyper-V.

Para instalar el complemento, ejecute el script [InstallPrivateCloudPlugin. PS1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) dentro de la **máquina virtual del host de contenedor (inquilino)** .


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. creación de una red de contenedores de *l2bridge*
En este paso, usará el comando `docker network create` en la **máquina virtual del host de contenedor (inquilino)** para crear una red l2bridge. 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>No se admite la asignación de direcciones IP estáticas con las redes de contenedor de *l2bridge* o *l2tunnel* cuando se usa con la pila de SDN de Microsoft.

## <a name="more-information"></a>Más información
Para obtener más información sobre la implementación de una infraestructura de SDN, consulte [implementación de una infraestructura de red definida por software](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

