---
title: Información general de NTLM
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b8dec2877646fd2bfe00da9d5c9047e8edfd6f1d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386258"
---
# <a name="ntlm-overview"></a>Información general de NTLM

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se describe NTLM, cualquier cambio en la funcionalidad y se proporcionan vínculos a recursos técnicos para la autenticación de Windows y NTLM para Windows Server 2012 y versiones anteriores.

## <a name="BKMK_OVER"></a>Descripción de la característica
La autenticación NTLM es una familia de protocolos de autenticación que se incluyen en Windows Msv1\_0.dll. Los protocolos de autenticación NTLM incluyen las versiones 1 y 2 de LAN Manager y, además, las versiones 1 y 2 de NTLM. Los protocolos de autenticación NTLM autentican a los usuarios y equipos en función de un mecanismo Challenge @ no__t-0response que demuestra a un servidor o a un controlador de dominio que un usuario conoce la contraseña asociada a una cuenta. Cuando se emplea el protocolo NTLM, un servidor de recursos debe realizar una de las siguientes acciones para comprobar la identidad de un equipo o usuario cada vez que se necesita un nuevo token de acceso:

-   Si la cuenta es una cuenta de dominio, póngase en contacto con un servicio de autenticación de dominio en el controlador de dominio para el dominio de la cuenta del usuario o del equipo.

-   Si la cuenta es una cuenta local, busque la cuenta del usuario o del equipo en la base de datos de la cuenta local.

## <a name="BKMK_APP"></a>Aplicaciones actuales
La autenticación NTLM aún se admite y se debe usar para la autenticación de Windows con sistemas configurados como miembros de un grupo de trabajo. La autenticación NTLM también se usa para la autenticación de inicio de sesión local en controladores que no son no__t-0domain. La autenticación Kerberos versión 5 es el método de autenticación preferido para entornos de Active Directory, pero una aplicación que no es @ no__t-0Microsoft o Microsoft podría seguir usando NTLM.

Reducir el uso del protocolo NTLM en un entorno de TI requiere el conocimiento de los requisitos de la aplicación implementada en NTLM y las estrategias y los pasos necesarios para configurar entornos informáticos para usar otros protocolos. Se han agregado nuevas herramientas y opciones para ayudarle a descubrir cómo se usa NTLM para restringir de forma selectiva el tráfico NTLM. Para obtener información sobre cómo analizar y restringir el uso de NTLM en sus entornos, vea [Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para tener acceso a la Guía de auditoría y restricción del uso de NTLM.

## <a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
No hay ningún cambio en la funcionalidad de NTLM para Windows Server 2012.

## <a name="BKMK_DEP"></a>Funcionalidad eliminada o desusada
No hay ninguna funcionalidad eliminada o desusada para NTLM para Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Información de Administrador del servidor
NTLM no se puede configurar desde el Administrador del servidor. Puede usar la configuración de la directiva de seguridad o las directivas de grupo para administrar el uso de la autenticación NTLM entre sistemas de equipos. En un dominio, Kerberos es el protocolo de autenticación predeterminado.

## <a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente, se enumeran los recursos relevantes para NTLM y otras tecnologías de autenticación de Windows.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Cambios en la autenticación NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planeamiento**|[Guía de modelado de amenazas de infraestructura de ti](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />@no__t 0Threats y contramedidas: Configuración de seguridad en Windows Server 2003 y Windows XP @ no__t-0<br /><br />[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 y Windows Vista @ no__t-0<br /><br />[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 R2 y Windows 7 @ no__t-0|
|**Implementación**|[protección ampliada para la autenticación](https://support.microsoft.com/kb/968389)<br /><br />[Guía de auditoría y restricción del uso de NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />@no__t 0Ask el equipo de servicios de directorio: Bloqueo de NTLM y usted: Metodologías de análisis y auditoría de la aplicación en Windows 7 @ no__t-0<br /><br />[Blog de autenticación de Windows](https://blogs.technet.com/authentication/)<br /><br />[Configuración de MaxConcurrentAPI para NTLM Pass @ no__t-1through Authentication](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**DevelopMentor**|[Microsoft NTLM \(Windows @ no__t-2](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[ @ NO__T-1 MS NO__T-2NLMP @ NO__T-3: NT LAN Manager \(NTLM @ no__t-1 Especificación del Protocolo de autenticación @ no__t-2<br /><br />[ @ NO__T-1 MS NO__T-2NNTP @ NO__T-3: NT LAN Manager \(NTLM @ no__t-1 autenticación: Protocolo de transferencia de noticias de red \(NNTP @ no__t-1 extensión @ no__t-2<br /><br />[ @ NO__T-1 MS NO__T-2NTHT @ NO__T-3: Especificación del Protocolo NTLM a través de HTTP @ no__t-0|
|**Solución de problemas**|No disponible aún|
|**Recursos de la comunidad**|@no__t: este caballo está inactivo todavía: Cuellos de botella de NTLM y el tiempo de ejecución de RPC @ no__t-0|



