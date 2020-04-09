---
title: Información general de NTLM
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-kerberos
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 250b1e7e0fc3a49fc261c70673a5ad6aa15c97b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858858"
---
# <a name="ntlm-overview"></a>Información general de NTLM

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se describe NTLM, cualquier cambio en la funcionalidad y se proporcionan vínculos a recursos técnicos para la autenticación de Windows y NTLM para Windows Server 2012 y versiones anteriores.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
La autenticación NTLM es una familia de protocolos de autenticación que se incluyen en Windows Msv1\_0. dll. Los protocolos de autenticación NTLM incluyen las versiones 1 y 2 de LAN Manager y, además, las versiones 1 y 2 de NTLM. Los protocolos de autenticación NTLM autentican a los usuarios y equipos en función de un mecanismo de desafío\/respuesta que demuestra a un servidor o a un controlador de dominio que un usuario conoce la contraseña asociada a una cuenta. Cuando se emplea el protocolo NTLM, un servidor de recursos debe realizar una de las siguientes acciones para comprobar la identidad de un equipo o usuario cada vez que se necesita un nuevo token de acceso:

-   Si la cuenta es una cuenta de dominio, póngase en contacto con un servicio de autenticación de dominio en el controlador de dominio para el dominio de la cuenta del usuario o del equipo.

-   Si la cuenta es una cuenta local, busque la cuenta del usuario o del equipo en la base de datos de la cuenta local.

## <a name="current-applications"></a><a name="BKMK_APP"></a>Aplicaciones actuales
La autenticación NTLM aún se admite y se debe usar para la autenticación de Windows con sistemas configurados como miembros de un grupo de trabajo. La autenticación NTLM también se usa para la autenticación de inicio de sesión local en controladores de dominio que no son\-. La autenticación Kerberos versión 5 es el método de autenticación preferido para entornos de Active Directory, pero una aplicación de Microsoft o de Microsoft no\-puede seguir usando NTLM.

Reducir el uso del protocolo NTLM en un entorno de TI requiere el conocimiento de los requisitos de la aplicación implementada en NTLM y las estrategias y los pasos necesarios para configurar entornos informáticos para usar otros protocolos. Se han agregado nuevas herramientas y opciones para ayudarle a descubrir cómo se usa NTLM para restringir de forma selectiva el tráfico NTLM. Para obtener información sobre cómo analizar y restringir el uso de NTLM en sus entornos, vea [Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para tener acceso a la Guía de auditoría y restricción del uso de NTLM.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
No hay ningún cambio en la funcionalidad de NTLM para Windows Server 2012.

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>Funcionalidad eliminada o desusada
No hay ninguna funcionalidad eliminada o desusada para NTLM para Windows Server 2012.

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Información de Administrador del servidor
NTLM no se puede configurar desde el Administrador del servidor. Puede usar la configuración de la directiva de seguridad o las directivas de grupo para administrar el uso de la autenticación NTLM entre sistemas de equipos. En un dominio, Kerberos es el protocolo de autenticación predeterminado.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente, se enumeran los recursos relevantes para NTLM y otras tecnologías de autenticación de Windows.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653.aspx)<p>[Cambios en la autenticación NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planeamiento**|[Guía de modelado de amenazas de infraestructura de ti](https://technet.microsoft.com/library/dd941826.aspx)<p>[Amenazas y contramedidas: configuración de seguridad en Windows Server 2003 y Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<p>[Guía de amenazas y contramedidas: configuración de seguridad en Windows Server 2008 y Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<p>[Guía de amenazas y contramedidas: configuración de seguridad en Windows Server 2008 R2 y Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Implementación**|[protección ampliada para la autenticación](https://support.microsoft.com/kb/968389)<p>[Guía de auditoría y restricción del uso de NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<p>[Pregunte al equipo de servicios de directorio: bloqueo de NTLM y las metodologías de análisis y auditoría de la aplicación en Windows 7.](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<p>[Blog de autenticación de Windows](https://blogs.technet.com/authentication/)<p>[Configuración de MaxConcurrentAPI para el paso de NTLM\-a través de la autenticación](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**DevelopMentor**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<p>[\[MS\-NLMP\]: NT LAN Manager \(NTLM\) especificación del Protocolo de autenticación](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<p>[\[MS\-NNTP\]: NT LAN Manager \(NTLM\) Authentication: Protocolo de transferencia de noticias de red \(extensión NNTP\)](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<p>[\[MS\-NTHT\]: especificación del Protocolo NTLM a través de HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Solución de problemas**|No disponible aún|
|**Recursos de la comunidad**|[Este caballo está inactivo todavía: cuellos de botella de NTLM y el tiempo de ejecución de RPC](https://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



