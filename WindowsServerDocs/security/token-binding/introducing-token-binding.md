---
title: Presentación de enlace de Token
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884236"
---
# <a name="introducing-token-binding"></a>Presentación de enlace de Token

>Se aplica a: Windows Server 2016 y Windows 10

El protocolo de enlace de Token permite a las aplicaciones y servicios criptográficamente enlazar sus tokens de seguridad a la capa TLS para mitigar el robo de tokens y ataques de reproducción. Los enlaces de TLS [RFC5246] identificables de forma única, de larga duración pueden abarcar varias conexiones y sesiones TLS.

Compatibilidad de versiones:

- Windows 10, versión 1507 – desactivado de forma predeterminada
    - Símbolo (token) de protocolo de enlace agregado [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Soporte técnico SYS de enlace de token a través de HTTP [[draft-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, las versiones 1511 y 1607 y Windows Server 2016: de forma predeterminada
    - Símbolo (token) de protocolo de enlace actualiza [[draft-ietf-tokbind-protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extensión TLS para la negociación de token de enlace agregada [[draft-popov-tokbind-negociación-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Soporte técnico SYS de enlace de token a través de HTTP actualiza [[draft-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, versión 1507, con la actualización de servicio [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, versión 1511 con actualización de servicio [KB4034660](https://support.microsoft.com/kb/KB4034660), Windows 10, versión 1607 y Windows Server 2016 con el servicio de actualización [KB4034658](https://support.microsoft.com/kb/KB4034658) admiten la versión de protocolo de enlace de Token 0,10 – en de forma predeterminada
    - Símbolo (token) de protocolo de enlace actualiza [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensión TLS para la negociación de token de enlace agregada [[draft-ietf-tokbind-negociación-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Soporte técnico SYS de enlace de token a través de HTTP actualiza [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versión 1703 admite la versión de protocolo de enlace de tokens 0,10 – en de forma predeterminada
    - Símbolo (token) de protocolo de enlace actualiza [[draft-ietf-tokbind-protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensión TLS para la negociación de token de enlace agregada [[draft-ietf-tokbind-negociación-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Soporte técnico SYS de enlace de token a través de HTTP actualiza [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Dispositivos de Windows con seguridad basada en virtualización habilitada mantendrá las claves del token de enlace en un entorno protegido que se aísla de sistema operativo en ejecución

Puede encontrar información sobre la compatibilidad con ASP .NET en el [.NET Framework Reference Source](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Para obtener información acerca de .NET Framework, vea los temas siguientes:

- [Mejoras de redes](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Clase de .NET TokenBinding](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
