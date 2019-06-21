---
title: Protocolo de seguridad de la capa de transporte
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 0a3b241fe0d2a61361d551b7f515507ad55d71cd
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284238"
---
# <a name="transport-layer-security-protocol"></a>Protocolo de seguridad de la capa de transporte

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

Este tema para profesionales de TI describe cómo el protocolo de capa de transporte (TLS) funciona y proporciona vínculos a las RFC de IETF para TLS 1.0, TLS 1.1 y TLS 1.2.

Los protocolos TLS (y SSL) se encuentran entre la capa del protocolo de aplicación y la capa de TCP/IP, donde pueden proteger y enviar datos de la aplicación a la capa de transporte. Dado que los protocolos funcionan entre la capa de aplicación y la capa de transporte, TLS y SSL pueden admitir varios protocolos de capa de aplicación.

TLS y SSL se suponen que el transporte orientado a conexiones, normalmente TCP, está en uso. El protocolo permite que las aplicaciones cliente y servidor detectar los riesgos de seguridad siguientes:

-   Manipulación del mensaje

-   Intercepción de mensajes

-   Falsificación de mensajes

Los protocolos TLS y SSL se pueden dividir en dos capas. La primera capa consta de protocolo de aplicación y los tres protocolos de enlace: el protocolo de enlace, el protocolo de especificación de cifrado de cambio y el protocolo de alerta. El segundo nivel es el protocolo de registro. La siguiente imagen ilustra las distintas capas y sus elementos.

**Capas de protocolo TLS y SSL**


Schannel SSP implementa los protocolos TLS y SSL sin ninguna modificación. El protocolo SSL es propietario, pero Internet Engineering Task Force genera las especificaciones públicas de TLS. Para obtener información sobre qué TLS o SSL versión es compatible con las versiones de Windows, consulte [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). En la tabla siguiente se enumera las especificaciones para cada versión TLS. Cada especificación contiene información sobre:

-   El protocolo de registro de TLS

-   Los protocolos de protocolo de enlace TLS: \- Cambiar cipher spec protocolo \- protocolo de alerta

-   Cálculos criptográficos

-   Conjuntos de cifrado obligatorio

-   Protocolo de datos de aplicación

[RFC 5246: The Transport Layer Security (TLS) Protocol Version 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346: The Transport Layer Security (TLS) Protocol Version 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246: The TLS Protocol Version 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Reanudación de la sesión TLS
Se introdujo en Windows Server 2012 R2, el SSP de Schannel implementa la parte del servidor de reanudación de la sesión TLS. La implementación de cliente de RFC 5077 se agregó en Windows 8.

Los dispositivos que conectan TLS a servidores necesitan volver a conectarse con frecuencia. Reanudación de la sesión TLS reduce el costo de establecer conexiones TLS porque implica un protocolo de enlace TLS abreviado. Esto facilita más intentos de reanudación al permitir que a un grupo de servidores TLS reanuden sesiones TLS del otro. Esta modificación proporciona los siguientes ahorros para cualquier cliente TLS que es compatible con RFC 5077, incluidos los dispositivos Windows Phone y Windows RT:

-   Un uso de recursos reducido en el servidor

-   Un ancho de banda reducido, lo que mejora la eficacia de las conexiones de cliente

-   Reduce el tiempo empleado para el protocolo de enlace TLS debido a reanudaciones de la conexión

Para obtener información acerca de la reanudación de la sesión TLS sin estado, consulte el documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negociación del protocolo de aplicación
 Windows Server 2012 R2 y Windows 8.1 introdujeron la compatibilidad que permite la negociación de protocolo de aplicación de TLS de cliente. Las aplicaciones puedan aprovechar los protocolos como parte del desarrollo estándar de HTTP 2.0 y los usuarios pueden acceder a los servicios en línea como Google y Twitter mediante el uso de aplicaciones que se ejecutan el protocolo SPDY.

Para obtener información sobre cómo funciona la negociación del protocolo de aplicación, consulte [extensión de negociación del protocolo de seguridad de capa de transporte (TLS) aplicación capa](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Soporte TLS para extensiones de indicación de nombre de servidor
La característica Indicación de nombre de servidor (SNI) extiende los protocolos SSL y TLS para permitir la identificación adecuada del servidor cuando se ejecuta una gran cantidad de imágenes virtuales en un solo servidor. En un escenario de hospedaje virtual, se hospedan varios dominios (cada uno con su propio certificado potencialmente diferente) en un servidor. En este caso, el servidor no tiene ninguna manera de saber de antemano qué certificado para enviar al cliente. SNI permite que el cliente informar al dominio de destino anteriormente en el protocolo, y esto permite que el servidor seleccione correctamente el certificado adecuado.

Esta funcionalidad adicional:

-   Le permite hospedar varios sitios Web SSL en una combinación de puerto y protocolo de Internet único

-   Reduce el uso de memoria cuando se hospedan varios sitios web SSL en un servidor web único.

-   Permite que varios usuarios se conecten a sitios Web SSL simultáneamente



