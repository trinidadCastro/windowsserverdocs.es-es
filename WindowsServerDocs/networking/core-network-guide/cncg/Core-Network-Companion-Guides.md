---
title: Guías complementarias de red principal
description: En este tema se proporciona información general sobre las guías complementarias de la guía de red principal de Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e7595cc91f1bf3c4b2b631398bc97accba8930cc
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997118"
---
# <a name="core-network-companion-guidance"></a>Orientación complementaria de red principal

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Aunque la [Guía de red principal](../core-network-guide.md) de Windows Server 2016 proporciona instrucciones sobre cómo implementar un nuevo &reg; bosque de Active Directory con un nuevo dominio raíz y la infraestructura de red compatible, las guías complementarias le ofrecen la posibilidad de agregar características a la red.

Cada guía complementaria le permite cumplir un objetivo específico después de haber implementado la red principal. En algunos casos, existen varias guías complementarias que, cuando se implementan juntas y en el orden correcto, permiten alcanzar objetivos muy complejos de una manera medida, rentable y razonable.

Si implementó el dominio y la red principal de Active Directory antes de encontrar la guía de red principal, puede usar las guías complementarias de todas maneras para agregar características a su red. Simplemente, use la guía de red principal como una lista de requisitos previos y sepa que, para implementar otras características con las guías complementarias, la red debe cumplir con los requisitos previos que proporciona la guía de red principal.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guía complementaria de red principal: implementación de certificados de servidor para implementaciones cableadas e inalámbricas de 802.1 X

En esta guía complementaria se explica cómo basarse en la red principal mediante la implementación de certificados de servidor para equipos que ejecutan NPS de servidor de directivas de redes \( \) , ras de servicio de acceso remoto \( \) o ambos.

Los certificados de servidor son necesarios al implementar métodos de autenticación basados en certificados con protocolo de autenticación extensible \( EAP \) y EAP \( PEAP protegido para la \) autenticación de acceso a la red. La implementación de certificados de servidor con Active Directory servicios de Certificate Server \( ad CS \) para los métodos de autenticación basados en certificados de EAP y PEAP ofrece las siguientes ventajas:

- Enlazar la identidad del servidor NPS o RAS a una clave privada
- Método rentable y seguro para inscribir automáticamente certificados en servidores NPS y RAS miembros del dominio.
- Método eficaz para administrar certificados y entidades de certificación
- Seguridad proporcionada por autenticación basada en certificados
- Capacidad de ampliar el uso de certificados para otros propósitos

Para obtener instrucciones sobre cómo implementar certificados de servidor, consulte [implementación de certificados de servidor para implementaciones cableadas e inalámbricas de 802.1 x](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guía complementaria de red principal: implementación del acceso inalámbrico autenticado mediante 802.1 X basado en contraseña

En esta guía complementaria se explica cómo basarse en la red principal proporcionando instrucciones sobre cómo implementar los ingenieros de Institute of Electrical and Electronics Engineers IEEE \( \) 802.1 x \- authenticated IEEE 802,11 Wireless Access mediante el protocolo de autenticación extensible protegido (Protocolo de autenticación por desafío mutuo de Microsoft versión 2 \( PEAP \- MS \- CHAP v2) \) .

El método de autenticación PEAP \- MS \- CHAP V2 requiere que las autenticaciones de los servidores que ejecutan el servidor de directivas de redes en \( NPS \) presenten clientes inalámbricos con un certificado de servidor para demostrar la identidad de NPS al cliente; sin embargo, la autenticación de usuarios no se realiza mediante un certificado; en su lugar, los usuarios proporcionan su nombre de usuario y contraseña de dominio.

Dado \- que PEAP MS \- CHAP V2 requiere que los usuarios proporcionen credenciales basadas en contraseña en lugar de un certificado durante el proceso de autenticación, normalmente es más fácil y menos costoso implementar que EAP \- TLS o PEAP \- TLS.

Antes de usar esta guía para implementar el acceso inalámbrico con el \- método de autenticación PEAP MS \- CHAP V2, debe hacer lo siguiente:

1. Siga las instrucciones de la guía de red principal para implementar la infraestructura de red principal, o ya tiene las tecnologías que se presentan en esa guía implementadas en la red.
2. Siga las instrucciones de la guía complementaria de red principal implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X, o ya tiene las tecnologías que se presentan en esa guía implementadas en la red.

Para obtener instrucciones sobre cómo implementar el acceso inalámbrico con PEAP \- MS \- CHAP V2, consulte [implementación de acceso inalámbrico autenticado mediante 802.1 x basado en contraseña](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guía complementaria de red principal: implementar el modo caché hospedada de BranchCache

En esta guía complementaria se explica cómo implementar BranchCache en modo caché hospedada en una o varias sucursales.

BranchCache es una tecnología de optimización del ancho de banda de la red de área extensa (WAN) que se incluye en algunas ediciones de los sistemas operativos Windows Server 2016 y Windows 10, así como en versiones anteriores de Windows y Windows Server.

Cuando implementa BranchCache en modo Caché hospedada, la memoria caché de contenido en la sucursal se hospeda en uno o más equipos servidores que se denominan servidores de caché hospedada. Los servidores de caché hospedada pueden ejecutar cargas de trabajo además de hospedar la memoria caché, lo que le permite usar el servidor para varios propósitos en la sucursal.

El modo caché hospedada de BranchCache aumenta la eficacia de la memoria caché porque el contenido está disponible incluso si el cliente que originalmente solicitó y almacenó en caché los datos está sin conexión. Dado que el servidor de caché hospedada está siempre disponible, se almacena más contenido en caché, lo cual ofrece más ahorro de ancho de banda WAN y se mejora la eficiencia de BranchCache.

Al implementar el modo caché hospedada, todos los clientes de una sucursal de varias subredes pueden acceder a una sola caché, que se almacena en el servidor de caché hospedada, incluso si los clientes se encuentran en subredes diferentes.

Para obtener instrucciones sobre cómo implementar BranchCache en modo caché hospedada, vea [implementar el modo caché hospedada de BranchCache](bc-hcm/1-Deploy-Bc-Hcm.md).