---
title: Protocolo de seguridad de la capa de transporte
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 05/16/2018
ms.openlocfilehash: 5641d79c40edaa17fd6c8fddc8cd80cbd30a0951
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989483"
---
# <a name="transport-layer-security-protocol"></a>Protocolo de seguridad de la capa de transporte

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

En este tema para profesionales de TI se describe cómo funciona el protocolo de seguridad de la capa de transporte (TLS) y se proporcionan vínculos a las RFC de IETF para TLS 1,0, TLS 1,1 y TLS 1,2.

Los protocolos TLS (y SSL) se encuentran entre la capa de protocolo de aplicación y la capa de TCP/IP, donde pueden proteger y enviar datos de aplicación a la capa de transporte. Dado que los protocolos funcionan entre el nivel de aplicación y la capa de transporte, TLS y SSL pueden admitir varios protocolos de nivel de aplicación.

TLS y SSL suponen que un transporte orientado a la conexión, normalmente TCP, está en uso. El protocolo permite a las aplicaciones cliente y servidor detectar los riesgos de seguridad siguientes:

-   Manipulación de mensajes

-   Interceptación de mensajes

-   Falsificación de mensajes

Los protocolos TLS y SSL se pueden dividir en dos capas. La primera capa consta del Protocolo de aplicación y de los tres protocolos de protocolo de enlace: el protocolo de enlace, el protocolo de cambio de especificación de cifrado y el protocolo de alerta. La segunda capa es el protocolo de registro. En la imagen siguiente se muestran las distintas capas y sus elementos.

**Niveles de protocolo TLS y SSL**


Schannel SSP implementa los protocolos TLS y SSL sin modificaciones. El protocolo SSL es propietario, pero Internet Engineering Task Force genera las especificaciones de TLS públicas. Para obtener información sobre qué versión de TLS o SSL se admite en las versiones de Windows, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-). En la tabla siguiente se enumeran las especificaciones de cada versión de TLS. Cada especificación contiene información sobre:

-   Protocolo de registro de TLS

-   Protocolos de protocolo de enlace de TLS: \- cambiar el protocolo de alerta del Protocolo de especificación de cifrado \-

-   Cálculos criptográficos

-   Conjuntos de cifrado obligatorios

-   Protocolo de datos de aplicaciones

[RFC 5246: el protocolo de seguridad de la capa de transporte (TLS) versión 1,2](http://tools.ietf.org/html/rfc5246)

[RFC 4346: The Transport Layer Security (TLS) Protocol Version 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246: The TLS Protocol Version 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="tls-session-resumption"></a><a name="BKMK_SessionResumption"></a>Reanudación de la sesión de TLS
Introducido en Windows Server 2012 R2, Schannel SSP implementó la parte del servidor de la reanudación de la sesión TLS. La implementación de cliente de RFC 5077 se agregó en Windows 8.

Los dispositivos que conectan TLS a servidores necesitan volver a conectarse con frecuencia. La reanudación de la sesión TLS reduce el costo de establecer conexiones TLS porque la reanudación implica un protocolo de enlace TLS abreviado. Esto facilita más intentos de reanudación al permitir que un grupo de servidores TLS reanude las sesiones TLS de los demás. Esta modificación proporciona el siguiente ahorro para cualquier cliente TLS que admita RFC 5077, incluidos los dispositivos Windows Phone y Windows RT:

-   Un uso de recursos reducido en el servidor

-   Un ancho de banda reducido, lo que mejora la eficacia de las conexiones de cliente

-   Se ha reducido el tiempo invertido en el protocolo de enlace de TLS debido a la reanudación de la conexión.

Para obtener información acerca de la reanudación de la sesión TLS sin estado, consulte el documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="application-protocol-negotiation"></a><a name="BKMK_AppProtocolNego"></a>Negociación de protocolo de aplicación
 Windows Server 2012 R2 y Windows 8.1 introdujeron la compatibilidad que permite la negociación del Protocolo de aplicación TLS del lado cliente. Las aplicaciones pueden aprovechar los protocolos como parte del desarrollo estándar de HTTP 2,0 y los usuarios pueden acceder a servicios en línea como Google y Twitter mediante el uso de aplicaciones que ejecutan el protocolo SPDY.

Para obtener información sobre el funcionamiento de la negociación del protocolo de aplicación, consulte [Extensión de negociación del protocolo de la capa de aplicación de la seguridad de la capa de transporte (TLS)](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="tls-support-for-server-name-indication-extensions"></a><a name="BKMK_SNI"></a>Compatibilidad con TLS para extensiones de Indicación de nombre de servidor
La característica Indicación de nombre de servidor (SNI) extiende los protocolos SSL y TLS para permitir la identificación adecuada del servidor cuando se ejecuta una gran cantidad de imágenes virtuales en un solo servidor. En un escenario de hospedaje virtual, se hospedan varios dominios (cada uno con su propio certificado potencialmente distinto) en un servidor. En este caso, el servidor no tiene ninguna manera de conocer de antemano qué certificado enviar al cliente. SNI permite al cliente informar al dominio de destino anteriormente en el protocolo, y esto permite que el servidor seleccione correctamente el certificado adecuado.

Esta funcionalidad adicional:

-   Permite hospedar varios sitios Web SSL en una combinación de puerto y Protocolo de Internet única.

-   Reduce el uso de memoria cuando se hospedan varios sitios web SSL en un servidor web único.

-   Permite a más usuarios conectarse a sitios Web SSL simultáneamente