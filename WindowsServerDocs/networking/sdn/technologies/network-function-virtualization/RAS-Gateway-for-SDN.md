---
title: Puerta de enlace RAS para SDN
description: Puede utilizar este tema para obtener información acerca de la puerta de enlace de RAS, que está basado en software, para varios inquilinos, enrutador capaz de Border Gateway Protocol (BGP) en Windows Server 2016.
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
ms.openlocfilehash: 4f1ad0b3f0b5921a53faa8a45baae9f0b8711873
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856276"
---
# <a name="ras-gateway-for-sdn"></a>Puerta de enlace RAS para SDN

>Se aplica a: Windows Server (canal semianual), puerta de enlace de Windows Server 2016 ## RAS para SDN  


Puerta de enlace RAS es una multiinquilino basado en software, enrutador con protocolo de puerta de enlace de borde (BGP) diseñado para empresas y proveedores de servicios en la nube (CSP) que hospedan varias redes virtuales de inquilino mediante la virtualización de red de Hyper-V. Las puertas de enlace de RAS enruta el tráfico de red entre la red física y los recursos de red de máquina virtual, independientemente de la ubicación. Puede enrutar el tráfico de red en la misma ubicación física o en muchas ubicaciones diferentes.   

La arquitectura multiempresa es la capacidad de una infraestructura de nube para admitir las cargas de trabajo de máquina virtual de varios inquilinos, aún aislarlas entre sí, mientras que todas las cargas de trabajo se ejecutan en la misma infraestructura. Las distintas cargas de trabajo de un inquilino individual pueden interconectarse y administrarse de manera remota, pero estos sistemas no se interconectan con las cargas de trabajo de los demás inquilinos, ni tampoco pueden los demás inquilinos administrarlas de manera remota.

  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas de la puerta de enlace RAS.  
>   
> -   [Novedades de la puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Arquitectura de implementación de puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Alta disponibilidad de puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Protocolo de puerta de enlace de borde &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Referencia de comandos de PowerShell de Windows BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Requisitos previos para instalar la puerta de enlace RAS para SDN  
No se puede usar la interfaz de Windows para instalar acceso remoto cuando desea implementar la puerta de enlace de RAS en el modo multiempresa para su uso con SDN. En su lugar, debe usar Windows PowerShell.  
  
Pero antes de poder instalar la puerta de enlace de RAS mediante Windows PowerShell, debe usar Windows PowerShell para agregar la **RemoteAccess** característica de Windows. Para ello, ejecute el siguiente comando en el símbolo del sistema de Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Este comando agrega el **RemoteAccess** característica y los comandos de Windows PowerShell para la característica.  
  
Después de haber agregado **RemoteAccess** a su servidor, puede instalar acceso remoto como una puerta de enlace de RAS con el modo multiempresa y Border Gateway Protocol (BGP).  
  
Para obtener más información, vea el tema de referencia de Windows PowerShell [Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Características de la puerta de enlace RAS  
Estos son las características de la puerta de enlace de RAS en Windows Server 2016. Puede implementar la puerta de enlace de RAS en grupos de alta disponibilidad que use todas estas características al mismo tiempo.  
  
-   **VPN de sitio a sitio**. Esta característica de puerta de enlace RAS permite conectar dos redes en diferentes ubicaciones físicas a través de Internet mediante una conexión VPN de sitio a sitio. Para los CSP que hospedan a muchos inquilinos en su centro de datos, la puerta de enlace de RAS proporciona una solución de puerta de enlace multiempresa que permite a los inquilinos acceder y administrar sus recursos a través de conexiones de VPN de sitio a sitio desde sitios remotos y permite el flujo de tráfico de red entre recursos virtuales en su centro de datos y la red física.  
  
-   **Point-to-site VPN**. Esta característica de puerta de enlace RAS permite que los empleados de la organización o administradores para conectarse a la red de su organización desde ubicaciones remotas.  Para las implementaciones de varios inquilinos, los administradores de red de inquilinos pueden usar conexiones VPN de punto a sitio para acceder a recursos de red virtual en el centro de datos CSP.  
  
-   **Tunelización de GRE**. Encapsulación de enrutamiento genérico (GRE) en función de los túneles habilitan la conectividad entre redes virtuales de inquilinos y redes externas. Dado que el protocolo GRE es ligero y la mayoría de los dispositivos de red lo admiten, se convierte en una opción ideal para el túnel donde no se requiere el cifrado de datos. La compatibilidad con GRE en túneles de sitio a sitio (S2S) soluciona el problema de reenvío entre redes virtuales de inquilinos y redes externas de inquilinos mediante una puerta de enlace de varios inquilinos, como se describe más adelante en este tema.  
  
-   **Enrutamiento dinámico con el protocolo de puerta de enlace de borde (BGP)**. BGP reduce la necesidad de configuración de enrutamiento manual en los enrutadores, ya que es un protocolo de enrutamiento dinámico y aprende automáticamente las rutas entre sitios que están conectados mediante conexiones VPN de sitio a sitio. Si su organización tiene varios sitios que están conectados mediante enrutadores BGP habilitado como puerta de enlace RAS, BGP permite que los enrutadores calcular automáticamente y usar rutas válidas entre sí en caso de interrupciones de red o un error. Para obtener más información, consulte [4271 RFC](https://tools.ietf.org/html/rfc4271).  
  

  


