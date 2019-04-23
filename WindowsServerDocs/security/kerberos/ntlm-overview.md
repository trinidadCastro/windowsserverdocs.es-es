---
title: Información general de NTLM
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 523fb71304ae55d17203cab4d1c5a17551bf8fdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890646"
---
# <a name="ntlm-overview"></a>Información general de NTLM

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se describe NTLM y los cambios en la funcionalidad y proporciona vínculos a recursos técnicos para la autenticación de Windows y NTLM para Windows Server 2012 y versiones anteriores.

## <a name="BKMK_OVER"></a>Descripción de la característica
La autenticación NTLM es una familia de protocolos de autenticación que se incluyen en el Msv1 Windows\_0.dll. Los protocolos de autenticación NTLM incluyen las versiones 1 y 2 de LAN Manager y, además, las versiones 1 y 2 de NTLM. Los protocolos de autenticación NTLM autentican usuarios y equipos según un desafío\/mecanismo de respuesta que le demuestra a un servidor o un controlador de dominio que un usuario conoce la contraseña asociada con una cuenta. Cuando se emplea el protocolo NTLM, un servidor de recursos debe realizar una de las siguientes acciones para comprobar la identidad de un equipo o usuario cada vez que se necesita un nuevo token de acceso:

-   Si la cuenta es una cuenta de dominio, póngase en contacto con un servicio de autenticación de dominio en el controlador de dominio para el dominio de la cuenta del usuario o del equipo.

-   Si la cuenta es una cuenta local, busque la cuenta del usuario o del equipo en la base de datos de la cuenta local.

## <a name="BKMK_APP"></a>Aplicaciones actuales
La autenticación NTLM aún se admite y se debe usar para la autenticación de Windows con sistemas configurados como miembros de un grupo de trabajo. También se usa la autenticación NTLM para la autenticación de inicio de sesión local en que no sean\-controladores de dominio. La autenticación Kerberos versión 5 es el método de autenticación preferido para entornos de Active Directory, pero sin\-Microsoft o aplicación todavía podría utilizar NTLM.

Reducir el uso del protocolo NTLM en un entorno de TI requiere el conocimiento de los requisitos de la aplicación implementada en NTLM y las estrategias y los pasos necesarios para configurar entornos informáticos para usar otros protocolos. Se han agregado nuevas herramientas y opciones para ayudarle a descubrir cómo se usa NTLM para restringir de forma selectiva el tráfico NTLM. Para obtener información sobre cómo analizar y restringir el uso de NTLM en sus entornos, vea [Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para tener acceso a la Guía de auditoría y restricción del uso de NTLM.

## <a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
No hay ningún cambio en la funcionalidad de NTLM para Windows Server 2012.

## <a name="BKMK_DEP"></a>Funcionalidad desusada o eliminada
No hay ninguna funcionalidad desusada o eliminada para NTLM para Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Información del administrador del servidor
NTLM no se puede configurar desde el Administrador del servidor. Puede usar la configuración de la directiva de seguridad o las directivas de grupo para administrar el uso de la autenticación NTLM entre sistemas de equipos. En un dominio, Kerberos es el protocolo de autenticación predeterminado.

## <a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente, se enumeran los recursos relevantes para NTLM y otras tecnologías de autenticación de Windows.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Cambios en la autenticación NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planeamiento**|[Guía de modelado de amenazas de infraestructura de TI](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Amenazas y contramedidas: Configuración de seguridad en Windows Server 2003 y Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 y Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 R2 y Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Implementación**|[Protección ampliada para la autenticación](https://support.microsoft.com/kb/968389)<br /><br />[Auditoría y restricción de la Guía de uso NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Pregunte al equipo de servicios de directorio: Bloqueo de NTLM y usted: Análisis de la aplicación metodologías y auditoría en Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog de autenticación de Windows](https://blogs.technet.com/authentication/)<br /><br />[Configuración de MaxConcurrentAPI para la fase de NTLM\-mediante la autenticación](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Desarrollo**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: NT LAN Manager \(NTLM\) especificación del protocolo de autenticación](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: NT LAN Manager \(NTLM\) autenticación: Protocolo de transferencia de noticias de red \(NNTP\) extensión](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: NTLM a través de la especificación del protocolo HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Solución de problemas**|No disponible aún|
|**Recursos de la comunidad**|[Es la entrada de blog sobre cuellos: Cuellos de botella de NTLM y el tiempo de ejecución RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



