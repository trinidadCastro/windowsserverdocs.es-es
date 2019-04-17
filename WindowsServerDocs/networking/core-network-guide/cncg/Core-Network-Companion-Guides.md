---
title: Guías de complemento de red principal
description: En este tema proporciona una descripción general de las guías de complemento a la Guía de red principal de Windows Server 2016
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c272c51cc69017b75e50e79e58186c0ea7c6391
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-companion-guides"></a>Guías de complemento de red principal

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Mientras el equipo con Windows Server 2016 [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) proporciona instrucciones sobre cómo implementar un nuevo directorio Active&reg; bosque con un nuevo dominio raíz y la infraestructura de red soporte complementario guías proporcionarte la capacidad de agregar características a la red.

Cada guía de inicio le permite lograr un objetivo específico después de implementar la red principal. En algunos casos, hay que varias complementario las guías que, cuando implementado entre sí y en el orden correcto, te permiten lograr objetivos muy complejos de forma razonable, rentable y medida.

Si implementaste la red principal y de dominio de Active Directory antes de encontrar a la guía básica de red, todavía puede utilizar a las guías de complemento a agregar características a la red. Simplemente usar a la guía básica de red como una lista de requisitos previos y saber para implementar características adicionales con las guías complementario, la red debe cumplir los requisitos previos proporcionados por la guía básica de red.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guía básica de complemento de red: Implementar certificados de servidor para 802.1X implementaciones inalámbricas y con cables 

Esta guía de inicio explica cómo se estructuran la red principal mediante la implementación de certificados de servidor para equipos que ejecutan el servidor de directivas de red \(NPS\), servicio de acceso remoto \(RAS\) o ambos.

Certificados de servidor se requieren al implementar métodos de autenticación basada en certificados con el protocolo de autenticación Extensible \(EAP\) y \(PEAP\) EAP protegido para la autenticación de acceso de red. Implementación de certificados de servidor con servicios de certificados de Active Directory \(AD CS\) para métodos de autenticación basada en certificados EAP y PEAP ofrece las siguientes ventajas:

- Enlazar la identidad del servidor NPS o RAS a una clave privada
- Un método rentable y seguro para inscribir automáticamente los certificados en los servidores miembro del dominio NPS y RAS
- Un método eficaz para administrar los certificados y las entidades de certificación
- Seguridad proporcionada por la autenticación basada en certificados
- La capacidad para expandir el uso de certificados para fines adicionales
  
Para obtener instrucciones sobre cómo implementar los certificados de servidor, consulta [implementar certificados de servidor para implementaciones de conexión inalámbrica y cableadas 802.1X](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guía básica de complemento de red: Implementar basada en contraseña autenticado acceso inalámbrico 802.1X

Esta guía de inicio explica cómo se estructuran la red principal al proporcionar instrucciones sobre cómo implementar Instituto de electricidad y electrónica ingenieros \(IEEE\) 802.1X\-autenticado con la versión 2 de protegido Extensible autenticación Protocol\ – Microsoft protocolo de autenticación de acceso inalámbrico IEEE 802.11 \ (v2\ PEAP\-MS\-CHAP).

El método de autenticación PEAP\-MS\-CHAPv2 requiere que la autenticación de servidores que ejecutan el servidor de directivas de red \(NPS\) clientes inalámbricos presentes con un certificado de servidor para demostrar la identidad del servidor NPS al cliente, sin embargo autenticación de usuario no se realiza mediante un certificado, en su lugar, los usuarios proporcionar su nombre de usuario de dominio y la contraseña.

Dado que PEAP\-MS\-CHAPv2 requiere que los usuarios que proporcionen las credenciales basadas en contraseña, en lugar de un certificado durante el proceso de autenticación, es suele ser más fácil y económica de implementar que EAP\ TLS o PEAP\ TLS.

Antes de usar a esta guía para implementar el acceso inalámbrico con el método de autenticación de PEAP\-MS\-CHAP v2, haz lo siguiente:

1. Sigue las instrucciones de la guía básica de red para implementar la infraestructura de red principal o ya han las tecnologías presentadas en esa guía implementada en la red.
2. Sigue las instrucciones en el núcleo red complementaria guía implementar los certificados de servidor para implementaciones de conexión inalámbrica y cableadas 802.1X o ya han las tecnologías presentadas en esa guía implementada en la red.

Para obtener instrucciones sobre cómo implementar acceso inalámbrico con PEAP\-MS\-CHAPv2, consulta [basada en contraseña implementar 802.1X X autenticados acceso inalámbrico](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guía básica de complemento de red: Implementar el modo de caché hospedada BranchCache

Esta guía de inicio explica cómo implementar BranchCache en modo de caché hospedada en sucursales uno o más.

BranchCache es una tecnología de optimización de ancho de banda (WAN) de red de área extensa que se incluye en algunas ediciones de los sistemas operativos Windows Server 2016 y Windows 10, así como en las versiones anteriores de Windows y Windows Server.

Cuando implementas BranchCache en modo de caché hospedada, la memoria caché de contenido en una sucursal es hospedada en uno o más equipos de servidor, que se denominan servidores de caché. Servidores de la memoria caché hospedada pueden ejecutar cargas de trabajo además de la memoria caché, lo que permite usar el servidor para varios propósitos en la oficina de rama de hospedaje.

El modo de caché BranchCache hospedado aumenta la eficacia de la memoria caché porque el contenido está disponible, incluso si el cliente que originalmente solicitado y almacena en caché los datos sin conexión. Como el servidor de la memoria caché hospedada siempre está disponible, se almacena en caché más contenido, proporcionar mayor ahorro de ancho de banda WAN, y mejora la eficacia de BranchCache.

Cuando implementas el modo de caché hospedada, todos los clientes en una sucursal varios subred pueden acceder a una memoria caché único, que se almacena en el servidor de la memoria caché hospedada, incluso si los clientes están en diferentes subredes.

Para obtener instrucciones sobre cómo implementar BranchCache en modo de caché hospedada, consulta [implementar BranchCache hospedado modo de caché](bc-hcm/1-Deploy-Bc-Hcm.md).
