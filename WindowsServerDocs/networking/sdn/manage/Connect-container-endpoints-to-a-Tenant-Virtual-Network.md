---
title: Conexión de puntos de conexión de contenedor a una red virtual de inquilino
description: En este tema, se muestra cómo conectar los puntos de conexión de contenedor a una red virtual existente de inquilino creada a través de SDN. Utilice la l2bridge (y opcionalmente l2tunnel) controlador de red disponible con el complemento de Windows libnetwork para Docker crear una red de contenedor en la máquina virtual del inquilino.
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
ms.date: 08/24/2018
ms.openlocfilehash: 1968a4db9231459fe5858d9a0f3ba5e8f317ed1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872746"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Conexión de puntos de conexión de contenedor a una red virtual de inquilino

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se muestra cómo conectar los puntos de conexión de contenedor a una red virtual existente de inquilino creada a través de SDN. Usa el *l2bridge* (y, opcionalmente, *l2tunnel*) controlador de red disponible con el complemento de Windows libnetwork para Docker crear una red de contenedor en la máquina virtual del inquilino.

En el [controladores de red de contenedor](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies) tema, analizamos varios controladores de red están disponibles a través de Docker en Windows. Para SDN, use el *l2bridge* y *l2tunnel* controladores. Para ambos controladores, cada punto de conexión de contenedor está en la misma subred virtual que la máquina virtual de host (inquilino) de contenedor. 

El Host a redes servicio (SNP), mediante el complemento de nube privada, se asigna dinámicamente las direcciones IP para los puntos de conexión de contenedor. Los puntos de conexión de contenedor tienen direcciones IP únicas, pero comparten la misma dirección MAC de la máquina de virtual de host (inquilino) contenedor debido a la traducción de direcciones de nivel 2. 

Directiva de red (ACL, encapsulación y QoS) para estos puntos de conexión de contenedor se aplica en el host de Hyper-V físico como recibidos por la controladora de red y se definen en sistemas de administración de la capa superior. 

La diferencia entre el *l2bridge* y *l2tunnel* controladores son:

| l2bridge | l2tunnel |
| --- | --- |
|Puntos de conexión de contenedor que residen en: <ul><li>El mismo contenedor host de máquina virtual y en la misma subred que tiene todo el tráfico de red con puente en el conmutador virtual de Hyper-V. </li><li>Contenedor diferente hospedar las máquinas virtuales o en subredes diferentes tiene su tráfico reenviado para el host de Hyper-V físico. </li></ul>No obtener aplica la directiva de red puesto que el tráfico de red entre contenedores en el mismo host y en la misma subred no pasan el host físico. Directiva de red aplica el tráfico de red de contenedor solo a entre hosts o entre subredes. | *Todos los* se reenvía el tráfico de red entre dos puntos de conexión de contenedor para el host de Hyper-V físico independientemente de host o la subred. Directiva de red se aplica al tráfico de red entre subredes y entre hosts. |
---

>[!NOTE]
>Estos modos de redes no funcionan para conectar puntos de conexión de contenedor de windows a una red virtual de inquilino en la nube pública de Azure.


## <a name="prerequisites"></a>Requisitos previos
-  Una infraestructura de SDN implementada con la controladora de red.
-  Se ha creado una red virtual del inquilino.
-  Una máquina virtual de inquilino implementado con la característica de contenedor de Windows habilitado, Docker instalado y la característica de Hyper-V habilitado. La característica de Hyper-V es necesaria para instalar varios archivos binarios para redes l2bridge y l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>[Virtualización anidada](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting) y exponer las extensiones de virtualización no es necesario a menos que el uso de contenedores de Hyper-V. 


## <a name="workflow"></a>Flujo de trabajo

[1. Agregar varias configuraciones de IP a un recurso de NIC de VM existente a través de la controladora de red (Host de Hyper-V)](#1-add-multiple-ip-configurations)
[2. Habilitar el proxy de red en el host para asignar direcciones IP de entidad emisora de certificados para puntos de conexión de contenedor (Host de Hyper-V) ](#2-enable-the-network-proxy) 
 [3. Instalar el complemento para asignar direcciones IP de la entidad emisora de certificados para puntos de conexión de contenedor (contenedor Host de VM) de nube privada ](#3-install-the-private-cloud-plug-in) 
 [4. Crear un *l2bridge* o *l2tunnel* red mediante docker (máquina virtual de Host de contenedor) ](#4-create-an-l2bridge-container-network)
 
>[!NOTE]
>No se admite varias configuraciones de IP en recursos de VM NIC creados a través de System Center Virtual Machine Manager. Se recomienda para estos tipos de implementaciones que cree el recurso de NIC de VM fuera de banda mediante PowerShell de controlador de red.

### <a name="1-add-multiple-ip-configurations"></a>1. Agregar varias configuraciones de IP
En este paso, se supone la NIC de VM de la máquina virtual de inquilino tiene una configuración de IP con la dirección IP de 192.168.1.9 y se adjunta a un identificador de recurso de red virtual de "VNet1" y el recurso de subred de máquina virtual de 'Subnet1' en la subred IP 192.168.1.0/24. Agregamos 10 direcciones IP para los contenedores de 192.168.1.101 - 192.168.1.110.

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

### <a name="2-enable-the-network-proxy"></a>2. Habilitar al proxy de red
En este paso, se habilita el proxy de red asignar varias direcciones IP para la máquina virtual de host de contenedor. 

Para habilitar el proxy de red, ejecute el [ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) de script en el **Host de Hyper-V** hospeda la máquina virtual de host (inquilino) de contenedor.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. Instalar el complemento de nube privada
En este paso, se instala un complemento para permitir el SNP para comunicarse con el proxy de red en el Host de Hyper-V.

Para instalar el complemento, ejecute el [InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) script dentro de la **máquina virtual del host (inquilino) contenedor**.


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. Crear un *l2bridge* red de contenedor
En este paso, utilizará el `docker network create` comando el **máquina virtual del host (inquilino) contenedor** para crear una red l2bridge. 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>No se admite la asignación de IP estática con *l2bridge* o *l2tunnel* redes de contenedor cuando se usa con la pila de SDN de Microsoft.

## <a name="more-information"></a>Más información
Para obtener más información sobre cómo implementar una infraestructura de SDN, consulte [implementar una infraestructura de red definida por Software](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

