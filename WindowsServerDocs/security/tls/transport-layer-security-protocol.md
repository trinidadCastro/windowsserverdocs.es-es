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
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 94c81a79e5f3fd8fc22eafde3de8bd3d0bdd73f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# Protocolo de seguridad de la capa de transporte

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI se describe cómo el protocolo de seguridad de la capa de transporte (TLS) funciona y proporciona vínculos a IETF RFC para TLS 1.2, TLS 1.1 y TLS 1.0.

Los protocolos TLS (y SSL) se encuentran entre el nivel de protocolo de aplicación y la capa de TCP/IP, donde pueden seguro y enviar datos de la aplicación a la capa de transporte. Dado que los protocolos funcionan entre la capa de la aplicación y la capa de transporte, TLS y SSL pueden admitir varios protocolos de nivel de aplicación.

TLS y SSL suponen que el transporte orientado a la conexión, por lo general TCP, está en uso. El protocolo permite que las aplicaciones de cliente y servidor detectar los riesgos de seguridad siguientes:

-   Manipulación de mensajes

-   Intercepción de mensajes

-   Falsificación de mensajes

Los protocolos TLS y SSL se pueden dividir en dos niveles. La primera capa está formado por el protocolo de aplicación y los tres protocolos de protocolo: el protocolo de protocolo de enlace, el protocolo de especificaciones de cifrado de cambio y el protocolo de alerta. La segunda capa es el protocolo de registro. La siguiente imagen muestra las distintas capas y sus elementos.

**Niveles de protocolo TLS y SSL**


El SSP Schannel implementa los protocolos TLS y SSL sin modificaciones. El protocolo SSL es propietario, pero el grupo de trabajo de ingeniería de Internet produce las especificaciones de TLS públicas. Para obtener información sobre qué TLS o SSL versión se admite en versiones de Windows, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). La siguiente tabla enumera las especificaciones para cada versión TLS. Cada especificación contiene información sobre:

-   El protocolo de registro de TLS

-   Los protocolos de protocolo de enlace TLS: \-protocolo especificaciones de cambio de cifrado \-alertar protocolo

-   Cálculos criptográficos

-   Conjuntos de cifrado obligatorios

-   Protocolo de datos de aplicación

[RFC 5246 - la versión del protocolo de transporte capa seguridad (TLS) 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346 - la versión del protocolo de seguridad (TLS) de transporte capa 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246 - la versión del protocolo TLS 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Reanudación de la sesión TLS
Introducidas en Windows Server 2012 R2, el SSP Schannel implementan la parte del lado servidor de reanudación de la sesión TLS. La implementación de cliente de RFC 5077 se agregó en Windows 8.

Dispositivos que se conectan con frecuencia TLS con los servidores deben volver a conectar. Reanudación de la sesión TLS reduce el costo de establecer conexiones TLS porque reanudación implica un protocolo de enlace TLS abreviado. Esto facilita más intentos de reanudación al permitir que a un grupo de servidores TLS reanudar las sesiones TLS del otro. Esta modificación proporciona los siguientes ahorros para cualquier cliente TLS que admite RFC 5077, incluidos los dispositivos de Windows Phone y Windows RT:

-   Uso de recursos reducido en el servidor

-   Reduce el ancho de banda, lo que mejora la eficacia de las conexiones de cliente

-   Reduce el tiempo invertido para el protocolo de enlace TLS debido a reanudaciones de la conexión

Para obtener información acerca de reanudación de la sesión TLS sin estado, consulta el documento IETF [5077 RFC.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negociación del protocolo de aplicación
 Windows Server 2012 R2 y Windows 8.1 comenzó a admitir que permite la negociación del protocolo de aplicación de cliente TLS. Las aplicaciones pueden aprovechar protocolos como parte del desarrollo HTTP 2.0 estándar y los usuarios pueden acceder a los servicios en línea como Google y Twitter mediante el uso de aplicaciones que se ejecutan en el protocolo SPDY.

Para obtener información sobre cómo funciona la negociación del protocolo de aplicación, consulta [extensión de negociación del protocolo de capa de seguridad de la capa de transporte (TLS) aplicación](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Compatibilidad con TLS las extensiones de indicación del nombre de servidor
La característica de indicación de nombre de servidor (SNI) amplía los protocolos SSL y TLS para permitir la identificación correcta del servidor al muchas imágenes virtual se ejecutan en un solo servidor. En un escenario de hospedaje virtual, varios dominios (cada una con su propio certificado potencialmente diferentes) se hospedan en un servidor. En este caso, el servidor no tiene ninguna manera de saber de antemano que certificados para enviar al cliente. SNI permite al cliente informar el dominio de destino anteriormente en el protocolo y permite al servidor seleccionar correctamente el certificado correcto.

Esta funcionalidad adicional:

-   Permite hospedar varios sitios Web SSL en un único protocolo de Internet y la combinación de puerto

-   Reduce el uso de memoria cuando se hospedan varios sitios Web SSL en un único servidor web

-   Permite a los usuarios más conectarte simultáneamente a sitios Web SSL



