---
title: Introducción al enlace de token
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 52ba35808b34eb07ecd6ac92819e9dc7a693b15b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403324"
---
# <a name="introducing-token-binding"></a>Introducción al enlace de token

>Se aplica a: Windows Server 2016 y Windows 10

El protocolo de enlace de token permite a las aplicaciones y servicios enlazar criptográficamente sus tokens de seguridad a la capa de TLS para mitigar los ataques de robo y reproducción de tokens. Los enlaces TLS de larga duración, identificables de forma única (RFC5246) pueden abarcar varias conexiones y sesiones de TLS.

Compatibilidad de versiones:

- Windows 10, versión 1507: desactivado de forma predeterminada
    - Protocolo de enlace de tokens agregado [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP. Compatibilidad de SYS con el enlace de tokens sobre HTTP [[draft-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10, versiones 1511 y 1607 y Windows Server 2016: activado de forma predeterminada
    - Protocolo de enlace de tokens actualizado [[draft-ietf-tokbind-Protocol-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - Extensión TLS para la negociación del enlace de tokens agregada [[draft-Popov-tokbind-negociación-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP. Compatibilidad de SYS del enlace de tokens sobre HTTP actualizado [[draft-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10, versión 1507 con actualización de mantenimiento [KB4034668](https://support.microsoft.com/kb/KB4034668), Windows 10, versión 1511 con actualización de mantenimiento [KB4034660](https://support.microsoft.com/kb/KB4034660), windows 10, versión 1607 y Windows Server 2016 con actualización de mantenimiento [KB4034658](https://support.microsoft.com/kb/KB4034658) protocolo de enlace de tokens versión 0,10: activado de forma predeterminada
    - Protocolo de enlace de tokens actualizado [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensión TLS para la negociación del enlace de tokens agregada [[draft-ietf-tokbind-negociación-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Compatibilidad de SYS del enlace de tokens sobre HTTP actualizado [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10, versión 1703 admite el protocolo de enlace de tokens versión 0,10: activado de forma predeterminada
    - Protocolo de enlace de tokens actualizado [[draft-ietf-tokbind-Protocol-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - Extensión TLS para la negociación del enlace de tokens agregada [[draft-ietf-tokbind-negociación-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP. Compatibilidad de SYS del enlace de tokens sobre HTTP actualizado [[draft-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - Los dispositivos Windows con la seguridad basada en la virtualización habilitada conservarán las claves de enlace de tokens en un entorno protegido que esté aislado del sistema operativo en ejecución.

Puede encontrar información sobre la compatibilidad con ASP .NET en el [origen de referencia de .NET Framework](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170). 

Para obtener información acerca de .NET Framework, vea los temas siguientes:

- [Mejoras de red](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [Clase TokenBinding de .NET](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
