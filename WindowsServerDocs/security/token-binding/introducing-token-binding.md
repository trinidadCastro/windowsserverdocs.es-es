---
title: "Presentación de enlace de Token"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="introducing-token-binding"></a>Presentación de enlace de Token

>Se aplica a: Windows Server 2016 y Windows 10

El protocolo de enlace Token permite que las aplicaciones y servicios criptográficamente enlazar sus tokens de seguridad a la capa TLS para mitigar el robo de token y ataques de reproducción. Los enlaces de TLS [RFC5246] de identificación única, de larga duración pueden abarcar varias sesiones TLS y conexiones.

Compatibilidad con la versión:

- Windows 10, versión 1507 – desactivado de manera predeterminada
    - Token de protocolo de enlace agregado [[borrador-ietf-tokbind-protocolo-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - HTTP WinInet & HTTP.SYS admiten de enlace token a través de HTTP [[borrador-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versión 1511 y 1607 y Windows Server 2016: de forma predeterminada
    - Token de protocolo de enlace actualizado [[borrador-ietf-tokbind-protocolo-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extensión TLS negociación del token enlace agregado [[borrador-popov-tokbind-negociación-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - HTTP WinInet & HTTP.SYS admiten de enlace token sobre HTTP actualizado [[borrador-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, versión 1507 con la actualización de mantenimiento [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, versión 1511 con la actualización de mantenimiento [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows 10, versión 1607 y Windows Server 2016 con la actualización de mantenimiento [KB4034658](https://support.microsoft.com/kb/KB4034658) admite la versión del protocolo de enlace Token 0,10 – en predeterminada
    - Token de protocolo de enlace actualizado [[borrador-ietf-tokbind-protocolo-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensión TLS negociación del token enlace agregado [[borrador-ietf-tokbind-negociación-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - HTTP WinInet & HTTP.SYS admiten de enlace token sobre HTTP actualizado [[borrador-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versión 1703 admite la versión del protocolo de enlace Token 0,10 – en predeterminada
    - Token de protocolo de enlace actualizado [[borrador-ietf-tokbind-protocolo-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensión TLS negociación del token enlace agregado [[borrador-ietf-tokbind-negociación-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - HTTP WinInet & HTTP.SYS admiten de enlace token sobre HTTP actualizado [[borrador-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Dispositivos de Windows con la seguridad basada en virtualización habilitada conservará las claves de enlace token en un entorno protegido que está aislado del sistema operativo en ejecución

Información sobre la compatibilidad de .NET ASP puede encontrarse en el [origen de referencia de .NET Framework](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Para obtener información acerca de .NET Framework, consulta los siguientes temas:

- [Mejoras de la red](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Clase de .NET TokenBinding](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
