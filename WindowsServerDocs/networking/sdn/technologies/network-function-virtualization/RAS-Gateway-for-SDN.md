---
title: Puerta de enlace RAS para SDN
description: Puede usar este tema para obtener información sobre la puerta de enlace RAS, que es un enrutador compatible con Protocolo de puerta de enlace de borde (BGP) basado en software y multiinquilino en Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 3059da769e8ab5719be657c82d3f075a686f6691
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859598"
---
# <a name="ras-gateway-for-sdn"></a>Puerta de enlace RAS para SDN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 # # puerta de enlace RAS para SDN  


La puerta de enlace RAS es un enrutador compatible con software, multiempresa, Protocolo de puerta de enlace de borde (BGP) diseñado para proveedores de servicios en la nube (CSP) y empresas que hospedan varias redes virtuales de inquilinos mediante virtualización de red de Hyper-V. Las puertas de enlace RAS enrutan el tráfico de red entre la red física y los recursos de red de VM, independientemente de la ubicación. Puede enrutar el tráfico de red en la misma ubicación física o en muchas ubicaciones diferentes.   

Multiinquilino es la capacidad de una infraestructura de nube para admitir las cargas de trabajo de máquinas virtuales de varios inquilinos, pero aislarlas entre sí, mientras que todas las cargas de trabajo se ejecutan en la misma infraestructura. Las distintas cargas de trabajo de un inquilino individual pueden interconectarse y administrarse de manera remota, pero estos sistemas no se interconectan con las cargas de trabajo de los demás inquilinos, ni tampoco pueden los demás inquilinos administrarlas de manera remota.

  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas de puerta de enlace RAS.  
>   
> -   [Novedades de puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Arquitectura de implementación de puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Alta disponibilidad de puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Protocolo de puerta de enlace de borde &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Referencia de comandos de Windows PowerShell de BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Requisitos previos para la instalación de la puerta de enlace RAS para SDN  
No se puede usar la interfaz de Windows para instalar el acceso remoto cuando se desea implementar la puerta de enlace de RAS en modo multiempresa para su uso con SDN. En su lugar, debe usar Windows PowerShell.  
  
Pero antes de poder instalar la puerta de enlace RAS mediante Windows PowerShell, debe usar Windows PowerShell para agregar la característica de Windows **RemoteAccess** . Para ello, ejecute el siguiente comando en el símbolo del sistema de Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Este comando agrega la característica **RemoteAccess** y los comandos de Windows PowerShell para la característica.  
  
Después **de haber agregado el** acceso remoto al servidor, puede instalar el acceso remoto como una puerta de enlace de ras con el modo multiinquilino y Protocolo de puerta de enlace de borde (BGP).  
  
Para obtener más información, vea el tema de referencia de Windows PowerShell [install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Características de puerta de enlace RAS  
A continuación se indican las características de puerta de enlace de RAS en Windows Server 2016. Puede implementar la puerta de enlace RAS en grupos de alta disponibilidad que usan todas estas características al mismo tiempo.  
  
-   **VPN de sitio a sitio**. Esta característica de puerta de enlace RAS le permite conectar dos redes en diferentes ubicaciones físicas a través de Internet mediante una conexión VPN de sitio a sitio. Para los CSP que hospedan muchos inquilinos en su centro de recursos, la puerta de enlace RAS proporciona una solución de puerta de enlace multiinquilino que permite a los inquilinos tener acceso a sus recursos y administrarlos a través de conexiones VPN de sitio a sitio desde sitios remotos y que permite el flujo de tráfico de red entre recursos virtuales en su centro de Datacenter y su red física.  
  
-   **VPN de punto a sitio**. Esta característica de puerta de enlace RAS permite que los empleados o administradores de la organización se conecten a la red de su organización desde ubicaciones remotas.  En el caso de las implementaciones multiinquilino, los administradores de red de inquilinos pueden usar conexiones VPN de punto a sitio para tener acceso a los recursos de red virtual en el centro de recursos de CSP.  
  
-   **Tunelización de GRE**. Los túneles basados en encapsulación de enrutamiento genérico (GRE) permiten la conectividad entre redes virtuales de inquilinos y redes externas. Dado que el protocolo GRE es ligero y la mayoría de los dispositivos de red lo admiten, se convierte en una opción ideal para el túnel donde no se requiere el cifrado de datos. La compatibilidad con GRE en túneles de sitio a sitio (S2S) soluciona el problema de reenvío entre redes virtuales de inquilinos y redes externas de inquilinos mediante una puerta de enlace de varios inquilinos, como se describe más adelante en este tema.  
  
-   **Enrutamiento dinámico con protocolo de puerta de enlace de borde (BGP)** . BGP reduce la necesidad de configuración de enrutamiento manual en los enrutadores, ya que es un protocolo de enrutamiento dinámico y aprende automáticamente las rutas entre sitios que están conectados mediante conexiones VPN de sitio a sitio. Si su organización tiene varios sitios que están conectados mediante enrutadores habilitados para BGP, como la puerta de enlace RAS, BGP permite a los enrutadores calcular y usar rutas válidas entre sí automáticamente en caso de que se produzcan interrupciones o errores en la red. Para obtener más información, consulte [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


