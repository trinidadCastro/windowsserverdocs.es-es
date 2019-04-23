---
title: Rendimiento de puerta de enlace de Windows Server de 2019
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: a6530b29ce7ffb0d18e0266e70cb2ca45188915c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845636"
---
# <a name="windows-server-2019-gateway-performance"></a>Rendimiento de puerta de enlace de Windows Server de 2019

>Se aplica a: Windows Server


En Windows Server 2016, una de las preocupaciones del cliente era la incapacidad de puerta de enlace SDN para satisfacer los requisitos de rendimiento de las redes modernas. El rendimiento de la red de los túneles IPsec y GRE tenía limitaciones con el rendimiento de conexión único para la conectividad de IPsec que se va a aproximadamente 300 Mbps y para la conectividad GRE que se va a Gbps aproximadamente 2,5.

Hemos mejorado significativamente en Windows Server 2019, con los números de alcanzar 1,8 Gbps y 15 GB/s para las conexiones de IPsec y GRE, respectivamente. Todo esto, con reducciones importantes en los ciclos de CPU / por bytes, lo que proporciona rendimiento alto rendimiento con mucho menos uso de CPU.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Habilitar el alto rendimiento con las puertas de enlace en Windows Server 2019

Para **conexiones GRE**, una vez implementación y actualización a Windows Server 2019 se basa en la puerta de enlace de máquinas virtuales, debería ver automáticamente el rendimiento mejorado. No es los responsables ningún paso manual.

Para **conexiones IPsec**, de forma predeterminada, al crear la conexión para las redes virtuales, obtener los números de rendimiento y la ruta de acceso de datos de Windows Server 2016. Para habilitar la ruta de acceso de datos de Windows Server 2019, realice lo siguiente:

   1. En una puerta de enlace SDN máquina virtual, vaya a **servicios** consola (services.msc).
   2. Busque el servicio denominado **servicio de puerta de enlace de Azure**y establezca el tipo de inicio en **automática**.
   3. Reinicie la máquina virtual de puerta de enlace.
      Las conexiones activas en esta puerta de enlace de conmutación por error a una máquina virtual de puerta de enlace redundante.
   4. Repita los pasos anteriores para el resto de la máquinas virtuales de puerta de enlace.

>[!TIP]
>Para obtener los mejores resultados de rendimiento, asegúrese de que el cipherTransformationConstant y authenticationTransformConstant en la configuración de modo rápido de los usos de la conexión IPsec el **GCMAES256** los conjuntos de cifrado.
>
>Para obtener el máximo rendimiento, el hardware del host de puerta de enlace debe admitir AES-NI y conjuntos de instrucciones de CPU PCLMULQDQ. Están disponibles en todas las CPU de Intel más adelante, excepto en los modelos que se han deshabilitado AES-NI y Westmere (32nm). Puede consultar la documentación del proveedor de hardware para ver si la CPU es compatible con AES-NI y CPU PCLMULQDQ instrucción establece.

A continuación es un ejemplo REST de conexión de IPsec con algoritmos de seguridad óptima:

```PowerShell
# NOTE: The virtual gateway must be created before creating the IPsec connection. More details here.
# Create a new object for Tenant Network IPsec Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   

# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 2000000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 2000000  

# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "111_aaa"   

$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 3600   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   

$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 28800
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   

# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   

# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "<<On premise subnet that must be reachable over the VPN tunnel. Ex: 10.0.0.0/24>>"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   

# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "<<Public IP address of the On-Premise VPN gateway. Ex: 192.168.3.4>>"   

# Add the new Network Connection for the tenant. Note that the virtual gateway must be created before creating the IPsec connection. $uri is the REST URI of your deployment and must be in the form of “https://<REST URI>”  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force
```

## <a name="testing-results"></a>Los resultados de pruebas

Lo que hemos hecho una amplia pruebas de rendimiento para las puertas de enlace SDN en nuestros laboratorios de pruebas. En las pruebas, hemos hemos comparado el rendimiento de red de puerta de enlace en escenarios SDN y escenarios que no son de SDN con Windows Server 2019. Puede encontrar los resultados y los detalles de configuración capturados en el artículo del blog de prueba [aquí](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---