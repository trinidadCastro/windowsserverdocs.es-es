---
title: Guías complementarias de red principal
description: En este tema proporciona información general sobre las guías complementarias a la Guía de red de Windows Server 2016 Core
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b757e1914ee263a041f39e9767d3cb8af38403dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816806"
---
# <a name="core-network-companion-guidance"></a>Orientación complementaria de red principal

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Aunque Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) proporciona instrucciones sobre cómo implementar un nuevo directorio activo&reg; bosque con un nuevo dominio raíz y la infraestructura de red subyacente, las guías complementarias le proporcionan con la capacidad de agregar características a la red.

Cada guía complementaria le permite cumplir un objetivo específico después de haber implementado la red principal. En algunos casos, existen varias guías complementarias que, cuando se implementan juntas y en el orden correcto, permiten alcanzar objetivos muy complejos de una manera medida, rentable y razonable.

Si implementó el dominio y la red principal de Active Directory antes de encontrar la guía de red principal, puede usar las guías complementarias de todas maneras para agregar características a su red. Simplemente, use la guía de red principal como una lista de requisitos previos y sepa que, para implementar otras características con las guías complementarias, la red debe cumplir con los requisitos previos que proporciona la guía de red principal.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guía complementaria de red principal: Implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1X 

En esta guía complementaria se explica cómo basarse en la red principal mediante la implementación de certificados de servidor para equipos que ejecutan el servidor de directivas de red \(NPS\), servicio de acceso remoto \(RAS\), o ambos.

Certificados de servidor son necesarios al implementar métodos de autenticación basada en certificados con protocolo de autenticación Extensible \(EAP\) y EAP protegido \(PEAP\) para la autenticación de acceso de red. Implementación de certificados de servidor con servicios de certificados de Active Directory \(AD CS\) para la autenticación basada en certificados EAP y PEAP métodos ofrece las siguientes ventajas:

- Enlazar la identidad del servidor NPS o RAS con una clave privada
- Un método rentable y seguro para inscribir automáticamente certificados en servidores miembros del dominio NPS y RAS
- Método eficaz para administrar certificados y entidades de certificación
- Seguridad proporcionada por autenticación basada en certificados
- Capacidad de ampliar el uso de certificados para otros propósitos
  
Para obtener instrucciones sobre cómo implementar certificados de servidor, consulte [implementar certificados de servidor para las implementaciones inalámbricas y cableadas 802.1X](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guía complementaria de red principal: Implementación del acceso inalámbrico autenticado mediante 802.1X basado en contraseña

En esta guía complementaria se explica cómo basarse en la red principal proporcionando instrucciones sobre cómo implementar Institute of Electrical and Electronics Engineers \(IEEE\) 802.1X\-autenticado IEEE 802.11 inalámbrica obtener acceso mediante la versión 2 de Protected Extensible Authentication Protocol\ – Microsoft protocolo de autenticación \(PEAP\-MS\-CHAP v2\).

El método de autenticación PEAP\-MS\-CHAP v2 requiere que autenticar los servidores que ejecutan el servidor de directivas de red \(NPS\) presentar a los clientes inalámbricos con un certificado de servidor para comprobar la identidad NPS para el cliente, sin embargo, no se realiza con un certificado - autenticación de usuario en su lugar, los usuarios proporcionar su nombre de usuario de dominio y contraseña.

Dado que PEAP\-MS\-CHAP v2 requiere que los usuarios proporcionen credenciales basadas en contraseña en lugar de un certificado durante el proceso de autenticación, se suele ser más fácil y menos costoso de implementar que EAP\-TLS o PEAP \-TLS.

Antes de utilizar esta guía para implementar el acceso inalámbrico con el PEAP\-MS\-método de autenticación CHAP v2, debe hacer lo siguiente:

1. Siga las instrucciones de la Guía de red principal para implementar la infraestructura de red principal o, ya que las tecnologías presentado en esa guía implementada en la red.
2. Siga las instrucciones en el Core Network Companion guía implementar los certificados de servidor para las implementaciones inalámbricas y cableadas 802.1X o, ya que las tecnologías presentado en esa guía implementada en la red.

Para obtener instrucciones sobre cómo implementar el acceso inalámbrico con PEAP\-MS\-CHAP v2, consulte [basado en contraseña implementar autentica el acceso inalámbrico 802.1X](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guía complementaria de red principal: Implementar el modo de caché hospedada de BranchCache

En esta guía complementaria se explica cómo implementar BranchCache en modo de caché hospedada en uno o más sucursales.

BranchCache es una tecnología de optimización de ancho de banda (WAN) de red de área extensa que se incluye en algunas ediciones de los sistemas operativos Windows Server 2016 y Windows 10, así como en versiones anteriores de Windows y Windows Server.

Cuando implementa BranchCache en modo Caché hospedada, la memoria caché de contenido en la sucursal se hospeda en uno o más equipos servidores que se denominan servidores de caché hospedada. Servidores de caché hospedada pueden ejecutar las cargas de trabajo además de hospedar la memoria caché, lo que le permite usar el servidor para fines diferentes en la sucursal.

Modo de caché hospedada de BranchCache aumenta la eficacia de la memoria caché porque el contenido está disponible incluso si el cliente que originalmente solicitó los datos almacenados en caché está desconectado. Dado que el servidor de caché hospedada está siempre disponible, se almacena más contenido en caché, lo cual ofrece más ahorro de ancho de banda WAN y se mejora la eficiencia de BranchCache.

Al implementar el modo de caché hospedada, todos los clientes de una sucursal de varias subredes pueden tener acceso a una memoria caché única, que se almacena en el servidor de caché hospedada, incluso si los clientes están en subredes diferentes.

Para obtener instrucciones sobre cómo implementar BranchCache en modo caché hospedada, consulte [modo de caché hospedada de BranchCache de implementar](bc-hcm/1-Deploy-Bc-Hcm.md).
