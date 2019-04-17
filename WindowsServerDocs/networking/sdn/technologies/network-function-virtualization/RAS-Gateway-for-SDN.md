---
title: Puerta de enlace RAS para SDN
description: Puedes usar este tema para obtener información sobre puerta de enlace de RAS, que está basado en software, multitenant de enrutador capaz de protocolo de puerta de enlace de borde (BGP) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 052911dcd52df82ef4e259de0c64078c54f00195
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-for-sdn"></a>Puerta de enlace RAS para SDN

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre la puerta de enlace de RAS, que es un protocolo de puerta de enlace de borde (BGP) basado en software y para varios inquilinos, compatible con enrutador en Windows Server 2016 está diseñada para proveedores de servicios de nube (CSP) y las empresas que hospedar varias redes virtuales inquilino usando la virtualización de Hyper-V red.  
  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas de puerta de enlace de RAS.  
>   
> -   [Novedades de puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Arquitectura de implementación de puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS puerta de enlace alta disponibilidad](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Protocolo de enlace de borde & #40; BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Referencia de comandos de PowerShell de Windows BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
En Windows Server 2016, puerta de enlace de RAS enruta el tráfico de red entre la física de red y recursos de red de la máquina virtual, independientemente de dónde se encuentran los recursos. Puedes usar la puerta de enlace RAS para enrutar el tráfico de red entre redes físicas y virtuales en la misma ubicación física o en varias ubicaciones físicas diferentes en Internet.  
  
Varias empresas es la capacidad de una infraestructura de nube para admitir las cargas de trabajo de la máquina virtual de varios inquilinos, aún las aislar entre ellas, mientras que todas las cargas de trabajo se ejecutan en la misma infraestructura. Varias cargas de trabajo de un inquilino individual pueden interconexión y administradas de forma remota, pero estos sistemas no interconexión con las cargas de trabajo de otros inquilinos ni otros inquilinos remotamente administrarlas.  
  
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Requisitos previos para instalar la puerta de enlace de RAS para SDN  
No puedes usar la interfaz de Windows para instalar el acceso remoto cuando quieras implementar RAS puerta de enlace en el modo multiempresa para su uso con SDN. En su lugar, debes usar Windows PowerShell.  
  
Pero antes de instalar la puerta de enlace RAS mediante Windows PowerShell, debes usar Windows PowerShell para agregar la **acceso remoto** característica de Windows. Para ello, ejecuta el siguiente comando en el símbolo del sistema de Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Agrega este comando la **acceso remoto** características y los comandos de Windows PowerShell para la característica.  
  
Después de haber agregado **acceso remoto** al servidor, puedes instalar acceso remoto como una puerta de enlace de RAS con el modo multiempresa y protocolo de puerta de enlace de borde (BGP).  
  
Para obtener más información, consulta el tema de referencia de Windows PowerShell [Install-acceso remoto](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Características de puerta de enlace RAS  
Siguientes son características de puerta de enlace de RAS en Windows Server 2016. Puedes implementar RAS puerta de enlace en grupos de alta disponibilidad que usan todas estas características al mismo tiempo.  
  
-   **VPN de sitio a sitio**. Esta característica de puerta de enlace de RAS te permite conectar dos redes en distintas ubicaciones físicas a través de Internet mediante una conexión VPN de sitio a sitio. De CSP que hospedan a varios inquilinos en su centro de datos, puerta de enlace de RAS proporciona una solución de puerta de enlace multiempresa que permite los inquilinos tener acceso y administrar sus recursos a través de conexiones de VPN de sitio a sitio desde sitios remotos, y que permita el flujo de tráfico de red entre los recursos virtuales en el centro de datos y su red física.  
  
-   **VPN de punto a sitio**. Esta característica de puerta de enlace de RAS permite que los empleados de la organización o los administradores para conectarte a la red de su organización desde ubicaciones remotas.  Para implementaciones multiempresa, los administradores de red de inquilino pueden usar conexiones VPN punto al sitio para acceder a recursos de red virtual en el centro de datos CSP.  
  
-   **Túnel GRE**. Encapsulación de enrutamiento genérico (GRE) basa túneles Habilitar conectividad entre redes virtuales del inquilino y redes externas. Dado que el protocolo GRE es ligero y soporte técnico para GRE está disponible en la mayoría de los dispositivos de red se convierte en idóneo túnel donde no se requiere el cifrado de datos. Soporte técnico GRE en los túneles (S2S) de un sitio a otro solucionó el problema de reenvío entre redes virtuales de inquilino y redes externas inquilino con una puerta de enlace de inquilino múltiples, tal como se describe más adelante en este tema.  
  
-   **Enrutamiento dinámico con el protocolo de puerta de enlace de borde (BGP)**. BGP reduce la necesidad de configuración manual de ruta en los enrutadores, dado que es un protocolo de enrutamiento dinámico y aprende automáticamente rutas entre sitios que están conectados a través de conexiones de VPN de sitio a sitio. Si tu organización tiene varios sitios que están conectados mediante el uso de enrutadores compatibles con BGP como puerta de enlace de RAS, BGP permite los enrutadores automáticamente calcular y usar rutas válidas entre sí en el caso de interrupción de la red o el error. Para obtener más información, consulta [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


