---
title: Novedades de Puerta de enlace de RAS
description: Puede utilizar este tema para obtener información sobre las nuevas características para la puerta de enlace de RAS, que está basado en software, para varios inquilinos, enrutador capaz de Border Gateway Protocol (BGP) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5cc7d8bab3f2783750dbd723da745b1df3c2e462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863026"
---
# <a name="whats-new-in-ras-gateway"></a>Novedades de Puerta de enlace de RAS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre las nuevas características para la puerta de enlace de RAS, que está basado en software, para varios inquilinos, enrutador capaz de Border Gateway Protocol (BGP) en Windows Server 2016. El enrutador BGP Multiinquilino de puerta de enlace de RAS está diseñado para empresas que hospedan varias redes virtuales de inquilino mediante la virtualización de red de Hyper-V y proveedores de servicios en la nube (CSP).  
  
> [!NOTE]  
> En Windows Server 2012 R2, puerta de enlace de RAS se denomina puerta de enlace RRAS; y en System Center Virtual Machine Manager, la puerta de enlace de RAS se denomina puerta de enlace de Windows Server.  
  
En este tema se incluyen las siguientes secciones.  
  
-   [Opciones de conectividad de sitio a sitio](#bkmk_s2s)  
  
-   [Grupos de puerta de enlace](#bkmk_pools)  
  
-   [Escalabilidad del grupo de servidores de puerta de enlace](#bkmk_gps)  
  
-   [Redundancia de grupo de servidores de puerta de enlace M+N](#bkmk_m)  
  
-   [Reflector de ruta](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Opciones de conectividad de sitio a sitio  
Puerta de enlace RAS ahora admite tres tipos de conexiones de sitio a sitio VPN:  Intercambio de claves por red versión 2 (IKEv2) túneles de encapsulación de enrutamiento genérico (GRE), VPN de nivel 3 (L3) y la red privada virtual (VPN) de sitio a sitio.  
  
Para obtener más información acerca de GRE, consulte [túneles GRE en Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Grupos de puerta de enlace  
En Windows Server 2016, puede crear grupos de puerta de enlace de tipos diferentes. Los grupos de puerta de enlace contienen muchas instancias de puerta de enlace de RAS y enrutar el tráfico de red entre redes físicas y virtuales. Los grupos de puerta de enlace pueden realizar cualquiera de las funciones de puerta de enlace individuales: intercambio de claves por red versión 2 (IKEv2) túneles de redes privadas virtuales (VPN) de sitio a sitio, VPN de nivel 3 (L3) y la encapsulación de enrutamiento genérico (GRE) - o el grupo puede realizar todos estos las funciones y actuar como un grupo mixto.  
  
Puede crear grupos de puerta de enlace mediante cualquier lógica que prefiera en función de los requisitos de infraestructura. Por ejemplo, puede crear grupos de puerta de enlace basados en cualquiera de las siguientes características.  
  
-   Tipos de túneles (IKEv2 VPN, VPN L3, GRE VPN)  
  
-   Capacidad  
  
-   Nivel de redundancia (confiabilidad según el plan de facturación para los inquilinos)  
  
-   Separación personalizado para los clientes  
  
Para obtener más información, consulte [alta disponibilidad de puerta de enlace de RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Escalabilidad del grupo de servidores de puerta de enlace  
Puede escalar fácilmente un grupo de servidores de puerta de enlace hacia arriba o hacia abajo agregando o quitando máquinas virtuales de puerta de enlace en el grupo. Eliminación o adición de puertas de enlace no interrumpe los servicios proporcionados por un grupo. También puede agregar y quitar grupos completos de las puertas de enlace.  
  
Para obtener más información, consulte [alta disponibilidad de puerta de enlace de RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>Redundancia de grupo de servidores de puerta de enlace M+N  
Cada grupo de servidores de puerta de enlace es con redundancia M+N. Esto significa que un ' m ' número de máquinas virtuales de puerta de enlace activo (VM) está respaldado por un número de "n" de las máquinas virtuales de puerta de enlace en espera. M + N redundancia proporciona más flexibilidad para determinar el nivel de confiabilidad que requieren al implementar la puerta de enlace RAS. En lugar de usar solo las puerta de enlace de RAS en espera por active de máquina virtual de puerta de enlace de RAS, que es la única opción de configuración con Windows Server 2012 R2, ahora puede configurar máquinas virtuales de reserva tantos como sea necesario. La característica Administrador de servicios de puerta de enlace de controlador de red utiliza eficazmente la capacidad de máquina virtual de puerta de enlace de RAS en espera para proporcionar conmutación por error confiable si un activo se produce un error o pierde la conexión de máquina virtual de puerta de enlace de RAS.  
  
Para obtener más información, consulte [alta disponibilidad de puerta de enlace de RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Reflector de ruta  
El Reflector de rutas Border Gateway Protocol (BGP) ya está incluido con la puerta de enlace RAS y proporciona una alternativa a la topología de malla completa de BGP que es necesario para la sincronización de la ruta entre enrutadores. Con la sincronización de malla completa, deben conectar todos los enrutadores BGP con todos los demás enrutadores de la topología de enrutamiento. Cuando se usa la ruta Reflector, sin embargo, el Reflector de ruta es el único que se conecta con todos los demás enrutadores, llama a los clientes BGP, simplificando la sincronización de la ruta, por tanto y reduce el tráfico de red. El Reflector de ruta aprende todas las rutas, calcula mejores rutas y redistribuye las rutas mejor a sus clientes BGP.  
  
Con Windows Server 2016, puede configurar túneles de acceso remoto de un inquilino individual para terminar en más de una máquina virtual de puerta de enlace de RAS. Esto proporciona mayor flexibilidad para proveedores de servicios en la nube cuando se enfrenta circunstancias donde no puede cumplir una máquina virtual de puerta de enlace de RAS todos los requisitos de ancho de banda de las conexiones del inquilino.  
  
Esta funcionalidad, sin embargo, presenta la complejidad adicional de administración de la ruta y sincronización efectiva de rutas entre los sitios remotos de inquilinos y sus recursos virtuales en el centro de datos en la nube. Proporcionar inquilinos con conexiones a varias puertas de enlace de RAS también aporta una complejidad adicional en la configuración al final de empresa, donde cada sitio de inquilino tendrá los vecinos de enrutamientos independientes.  
  
Un Reflector de rutas BGP en el plano de control aborda estos problemas y hace que la implementación de fabric interno CSP transparente a los inquilinos de la empresa. Estos son algunos puntos clave sobre el Reflector de rutas BGP que se incluye con la puerta de enlace RAS e integrarse con controladora de red.  
  
-   Una ruta Reflector en una implementación de redes definidas por Software es una entidad lógica que se encuentra en el plano de control entre las puertas de enlace de RAS y la controladora de red. No, sin embargo, participa en el enrutamiento de plano de datos.  
  
-   Cuando se agrega un nuevo inquilino a su centro de datos, controladora de red configura automáticamente el inquilino de la primera puerta de enlace de RAS como una ruta de Reflector.  
  
-   Cada inquilino tiene un espejo de ruta correspondiente, y reside en una de las máquinas virtuales de puerta de enlace de RAS que están asociados a ese inquilino.  
  
-   Un inquilino ruta Reflector actúa como el Reflector de ruta para todas las máquinas virtuales de puerta de enlace de RAS que están asociadas con el inquilino. Puertas de enlace de inquilino que no sea el Reflector de ruta de puerta de enlace de RAS son los clientes de Reflector de ruta. El Reflector de ruta realiza la sincronización de la ruta entre todos los clientes de Reflector de ruta para que puedan realizarse el enrutamiento de la ruta de acceso de datos reales.  
  
-   Una ruta Reflector no proporciona servicios de reflector de ruta para la puerta de enlace de RAS en el que está configurado.  
  
-   Una ruta Reflector actualiza la controladora de red con las rutas de empresa que corresponden a los sitios de empresa del inquilino. Esto permite que la controladora de red configurar las directivas de virtualización de red de Hyper-V necesarias en la red virtual de inquilino para el acceso a la ruta de acceso de datos de extremo a otro.  
  
-   Si los clientes de empresa usan el enrutamiento de BGP en el espacio de direcciones de cliente, el Reflector de ruta de puerta de enlace de RAS es externo solo vecino BGP (eBGP) para todos los sitios de inquilino correspondiente. Esto es cierto independientemente de los puntos de terminación de túnel del inquilino de empresa. En otras palabras, con independencia de qué máquina virtual de puerta de enlace de RAS en el CSP datacenter finaliza el túnel VPN de sitio a sitio para un sitio de inquilino, el BGP del mismo nivel para todos los sitios de inquilino es el Reflector de ruta.  
  
Para obtener más información, consulte [arquitectura de implementación de puerta de enlace de RAS](RAS-Gateway-Deployment-Architecture.md) y la solicitud de Internet Engineering Task Force (IETF) para el tema de comentarios [RFC 4456 BGP Route reflexión: Una alternativa al completo de la malla BGP interno (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

