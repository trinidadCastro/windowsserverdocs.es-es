---
title: Rendimiento de puerta de enlace de Windows Server 2019
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: 34890a5d93d6e2e214e401f5566cbb0ffde37508
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855708"
---
# <a name="windows-server-2019-gateway-performance"></a>Rendimiento de puerta de enlace de Windows Server 2019

>Se aplica a: Windows Server


En Windows Server 2016, una de las preocupaciones del cliente era la incapacidad de la puerta de enlace de SDN para satisfacer los requisitos de rendimiento de las redes modernas. El rendimiento de red de los túneles de IPsec y GRE tenía limitaciones con el rendimiento de conexión única para la conectividad de IPsec de aproximadamente 300 Mbps y para la conectividad GRE de aproximadamente 2,5 Gbps.

Hemos mejorado significativamente en Windows Server 2019, con los números que se exponen a 1,8 Gbps y 15 Gbps para las conexiones de IPsec y GRE, respectivamente. Todo esto, con reducciones significativas en los ciclos de CPU/por byte, lo que proporciona un rendimiento de alto rendimiento con un uso de CPU mucho menor.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Habilitar el alto rendimiento con las puertas de enlace en Windows Server 2019

En el caso de **las conexiones GRE**, una vez que implemente o actualice a Windows Server 2019 compilaciones en las máquinas virtuales de puerta de enlace, debe ver automáticamente el rendimiento mejorado. No interviene ningún paso manual.

En el caso de **las conexiones IPSec**, de forma predeterminada, cuando se crea la conexión para las redes virtuales, se obtiene la ruta de datos y los números de rendimiento de Windows Server 2016. Para habilitar la ruta de datos de Windows Server 2019, haga lo siguiente:

   1. En una máquina virtual de puerta de enlace de SDN, vaya a la consola de **servicios** (Services. msc).
   2. Busque el servicio denominado **servicio de puerta de enlace de Azure**y establezca el tipo de inicio en **automático**.
   3. Reinicie la máquina virtual de puerta de enlace.
      Las conexiones activas en esta puerta de enlace conmutan por error a una máquina virtual de puerta de enlace redundante.
   4. Repita los pasos anteriores para el resto de las máquinas virtuales de puerta de enlace.

>[!TIP]
>Para obtener los mejores resultados de rendimiento, asegúrese de que cipherTransformationConstant y authenticationTransformConstant en la configuración de quickMode de la conexión IPsec utiliza el conjunto de cifrado de **GCMAES256** .
>
>Para obtener el máximo rendimiento, el hardware del host de puerta de enlace debe ser compatible con los conjuntos de instrucciones de CPU AES-o y PCLMULQDQ. Están disponibles en cualquier Westmere (32nm) y posterior CPU de Intel, excepto en los modelos en los que se ha deshabilitado AES-NI. Puede consultar la documentación del proveedor de hardware para ver si la CPU es compatible con los conjuntos de instrucciones de CPU AES-o y PCLMULQDQ.

A continuación se muestra un ejemplo de REST de conexión IPsec con algoritmos de seguridad óptimos:

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

## <a name="testing-results"></a>Resultados de pruebas

Hemos realizado pruebas de rendimiento exhaustivas para las puertas de enlace de SDN en nuestros laboratorios de pruebas. En las pruebas, se ha comparado el rendimiento de la red de puerta de enlace con Windows Server 2019 en escenarios de SDN y escenarios que no son de SDN. Puede encontrar los resultados y los detalles de configuración de las pruebas que se capturan en el artículo del blog [aquí](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---