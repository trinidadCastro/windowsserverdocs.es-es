---
title: Instrucciones generales para la solución de problemas de DHCP
description: Este artilce presenta instrucciones generales para solucionar problemas de DHCP.
ms.service: na
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 92b76748153f19419733c32c08a24d48e53d5647
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970052"
---
# <a name="general-guidance-to-troubleshoot-dhcp"></a>Instrucciones generales para la solución de problemas de DHCP

Antes de empezar a solucionar problemas, compruebe los elementos siguientes. Pueden ayudarle a encontrar la causa principal del problema.

## <a name="checklist"></a>Lista de comprobación

  - ¿Cuándo comenzó el problema?

  - ¿Hay algún mensaje de error?

  - ¿El servidor DHCP funcionaba anteriormente o no funcionó nunca?
    Si funcionaba anteriormente, cualquier cambio antes del inicio del problema. Por ejemplo, ¿se instaló una actualización? ¿Se realizó un cambio en la infraestructura?

  - ¿El problema es persistente o intermitente? Si es intermitente, ¿Cuándo se produjo por última vez?

  - ¿Se producen errores en la concesión de direcciones para todos los clientes o solo para clientes específicos, como una subred de un solo ámbito?

  - ¿Hay clientes en la misma subred de red que el servidor DHCP?

  - Si los clientes residen en la misma subred de red, ¿pueden obtener direcciones IP?

  - Si los clientes no están en la misma subred de red, ¿los enrutadores o conmutadores VLAN están configurados correctamente para tener agentes de retransmisión DHCP (también conocidos como aplicaciones auxiliares de IP)?

  - ¿El servidor DHCP es independiente o está configurado para alta disponibilidad, como la conmutación por error DHCP o de ámbito dividido?

  - Compruebe los dispositivos intermedios en busca de características como VRRP/HSRP, inspección de ARP dinámica o supervisión de DHCP que se sabe que causan problemas.
