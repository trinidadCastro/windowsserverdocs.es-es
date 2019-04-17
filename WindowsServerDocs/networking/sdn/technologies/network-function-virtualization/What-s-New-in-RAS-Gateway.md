---
title: Novedades de puerta de enlace RAS
description: Puedes usar este tema para obtener información sobre las nuevas características de puerta de enlace de RAS, que está basado en software, multitenant de enrutador capaz de protocolo de puerta de enlace de borde (BGP) en Windows Server 2016.
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
ms.openlocfilehash: 1a9ac762f6cd80d3889cf72478b7a7f8ce9e5cb7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ras-gateway"></a>Novedades de puerta de enlace RAS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre las nuevas características de puerta de enlace de RAS, que está basado en software, multitenant de enrutador capaz de protocolo de puerta de enlace de borde (BGP) en Windows Server 2016. El enrutador RAS puerta de enlace Multitenant BGP está diseñado para proveedores de servicios de nube (CSP) y las empresas que hospedar varias redes virtuales inquilino usando la virtualización de Hyper-V red.  
  
> [!NOTE]  
> En Windows Server 2012 R2, puerta de enlace de RAS se denomina puerta de enlace de RRAS; y en System Center Virtual Machine Manager, RAS puerta de enlace que se denomina la puerta de enlace de servidor de Windows.  
  
Este tema contiene las siguientes secciones.  
  
-   [Opciones de conectividad de sitio a sitio](#bkmk_s2s)  
  
-   [Grupos de puerta de enlace](#bkmk_pools)  
  
-   [Puerta de enlace escalabilidad de grupo](#bkmk_gps)  
  
-   [Redundancia del grupo de puerta de enlace M + N](#bkmk_m)  
  
-   [Ruta espejo](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Opciones de conectividad de sitio a sitio  
Puerta de enlace de RAS ahora admite tres tipos de conexiones VPN de sitio a sitio: intercambio de claves de Internet versión 2 (IKEv2) túneles sitio a sitio redes privadas virtuales (VPN), nivel 3 (L3) VPN y encapsulación de enrutamiento genérico (GRE).  
  
Para obtener más información sobre GRE, consulta [GRE túnel en Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Grupos de puerta de enlace  
En Windows Server 2016, puedes crear grupos de puerta de enlace de diferentes tipos. Grupos de puerta de enlace contienen muchas instancias de puerta de enlace de RAS y enrutan el tráfico de red entre redes físicas y virtuales. Grupos de puerta de enlace pueden realizar cualquiera de las funciones individuales de la puerta de enlace - intercambio de claves de Internet versión 2 (IKEv2) túneles sitio a sitio redes privadas virtuales (VPN), nivel 3 (L3) VPN y encapsulación de enrutamiento genérico (GRE) - o el grupo puede realizar todas estas funciones y actuar como un grupo mixto.  
  
Puedes crear grupos de puerta de enlace usando cualquier lógica que prefieras en función de los requisitos de infraestructura. Por ejemplo, puedes crear grupos de puerta de enlace basados en cualquiera de las siguientes características.  
  
-   Tipos de túnel (IKEv2 VPN, L3 VPN, GRE VPN)  
  
-   Capacidad  
  
-   Nivel de redundancia (confiabilidad en función de tu plan de facturación para inquilinos)  
  
-   Separación personalizada para los clientes  
  
Para obtener más información, consulta [RAS puerta de enlace de alta disponibilidad](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Puerta de enlace escalabilidad de grupo  
Puede escalar fácilmente un grupo de puerta de enlace hacia arriba o hacia abajo, agregando o quitando máquinas virtuales de puerta de enlace en el grupo. Retirada o la adición de puertas de enlace no interrumpir los servicios proporcionados por un grupo. También puedes agregar y quitar todos grupos de puertas de enlace.  
  
Para obtener más información, consulta [RAS puerta de enlace de alta disponibilidad](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>Redundancia del grupo de puerta de enlace M + N  
Cada grupo de puerta de enlace es M + N redundante. Esto significa que un estoy ' número de máquinas virtuales (VM) de puerta de enlace active copia un número de ' n ' de máquinas virtuales en modo de espera de la puerta de enlace. M + N redundancia ofrece más flexibilidad para determinar el nivel de confiabilidad que requieren cuando implementas RAS puerta de enlace. En lugar de usar un solo Gateway RAS en modo de espera por activo de máquina virtual de puerta de enlace de RAS, que es la única opción de configuración con Windows Server 2012 R2, ahora puedes configurar tantas máquinas virtuales en modo de espera como sea necesario. La característica de administrador de servicios de puerta de enlace de controlador de red usa eficazmente la capacidad de máquina virtual de puerta de enlace de RAS en modo de espera para proporcionar confiable conmutación por error si se activa RAS VM de puerta de enlace se produce un error o pierde la conexión.  
  
Para obtener más información, consulta [RAS puerta de enlace de alta disponibilidad](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Ruta espejo  
El espejo de ruta de protocolo de puerta de enlace de borde (BGP) ya está incluido con la puerta de enlace de RAS y ofrece una alternativa a topología de malla completa BGP que se necesita para la sincronización de la ruta entre enrutadores. Con la sincronización de malla completa, todos los enrutadores BGP deben conectarse con todos los demás enrutadores de la topología de enrutamiento. Cuando usas el espejo de ruta, sin embargo, espejo ruta es el único que se conecta con todos los demás enrutadores, llamados a los clientes BGP, lo que simplifica la sincronización de la ruta y reduce el tráfico de red. Ruta espejo aprende todas las rutas, calcula rutas mejor y redistribuye las rutas mejor a sus clientes BGP.  
  
Con Windows Server 2016, puedes configurar túneles de acceso remoto de un inquilino individuales para terminar en más de una máquina virtual de puerta de enlace de RAS. Esto proporciona una mayor flexibilidad para proveedores de servicios de nube cuando se enfrentan las circunstancias donde no cumple una VM de puerta de enlace de RAS todos los requisitos de ancho de banda de las conexiones de inquilino.  
  
Esta funcionalidad, sin embargo, presenta la complejidad adicional de administración de la ruta y la sincronización eficaz de rutas entre los sitios remotos de inquilino y sus recursos virtuales en el centro de datos de la nube. Proporcionar inquilinos con conexiones a varias puertas de enlace de RAS, también incorpora complejidad adicional en la configuración al final de empresa, donde cada sitio inquilino tendrá vecinos enrutamiento independientes.  
  
Un espejo de ruta de BGP en el plano control soluciona estos problemas y hace que la implementación de la estructura interna de CSP transparente para los inquilinos de empresa. A continuación ofrecemos algunos puntos claves sobre BGP ruta espejo que se incluye con la puerta de enlace de RAS e integrada con el controlador de red.  
  
-   Una ruta espejo en una implementación de Software definido de redes es una entidad lógica que se encuentra en el plano de control entre las puertas de enlace de RAS y el controlador de red. No, no obstante, participa en el enrutamiento de plano de datos.  
  
-   Cuando agregas un inquilino nuevo en el centro de datos, el controlador de red configura automáticamente la primera RAS puerta de enlace de inquilino como un espejo de la ruta.  
  
-   Cada inquilino no tiene un espejo de ruta correspondiente y reside en una de las máquinas virtuales de puerta de enlace de RAS que están asociados a ese inquilino.  
  
-   Un ruta espejo inquilino actúa como espejo ruta para todas las máquinas virtuales de puerta de enlace de RAS que están asociados con el inquilino. Puertas de enlace de inquilino que no sean el espejo de ruta de puerta de enlace de RAS son los clientes de espejo de la ruta. Ruta espejo realiza la sincronización de la ruta entre todos los clientes de espejo de ruta para que el enrutamiento de ruta de acceso de los datos reales puede producirse.  
  
-   Una ruta espejo no proporcionan servicios de espejo de ruta para la puerta de enlace de RAS en el que está configurado.  
  
-   Una ruta espejo actualiza el controlador de red con las rutas de empresa que corresponden a los sitios de empresa del inquilino. Esto permite que el controlador de red configurar las directivas de virtualización de Hyper-V red necesarias de la red virtual del inquilino obtener acceso a la ruta de acceso de datos de principio a fin.  
  
-   Si los clientes de empresa utilizan BGP enrutamiento en el espacio de direcciones de cliente, el espejo de ruta de puerta de enlace de RAS es solo externo vecino BGP (eBGP) para todos los sitios del inquilino correspondiente. Esto sucede independientemente de puntos de terminación de túnel del inquilino de la empresa. En otras palabras, sin importar qué VM de puerta de enlace de RAS en el CSP datacenter termina el túnel VPN de sitio a sitio para un sitio de inquilinos, el sistema del mismo nivel eBGP para todos los sitios de inquilino es espejo ruta.  
  
Para obtener más información, consulta [arquitectura de implementación de puerta de enlace de RAS](RAS-Gateway-Deployment-Architecture.md) y la solicitud de ingeniería de Internet (IETF) para el tema de comentarios [RFC 4456 BGP ruta reflexión: una alternativa a completa de malla interno BGP (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

