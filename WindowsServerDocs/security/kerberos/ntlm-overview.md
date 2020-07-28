---
title: NTLM Overview
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-kerberos
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 972b5b8eb5e25382c2c9b7841cf0d0fe4db6e647
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181881"
---
# <a name="ntlm-overview"></a>NTLM Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se describe NTLM, cualquier cambio en la funcionalidad y se proporcionan vínculos a recursos técnicos para la autenticación de Windows y NTLM para Windows Server 2012 y versiones anteriores.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
La autenticación NTLM es una familia de protocolos de autenticación que se incluyen en el0.dll de Windows Msv1 \_ . Los protocolos de autenticación NTLM incluyen las versiones 1 y 2 de LAN Manager y, además, las versiones 1 y 2 de NTLM. Los protocolos de autenticación NTLM autentican a los usuarios y equipos en función de un \/ mecanismo de respuesta a desafíos que demuestra a un servidor o a un controlador de dominio que un usuario conoce la contraseña asociada a una cuenta. Cuando se emplea el protocolo NTLM, un servidor de recursos debe realizar una de las siguientes acciones para comprobar la identidad de un equipo o usuario cada vez que se necesita un nuevo token de acceso:

-   Si la cuenta es una cuenta de dominio, póngase en contacto con un servicio de autenticación de dominio en el controlador de dominio para el dominio de la cuenta del usuario o del equipo.

-   Si la cuenta es una cuenta local, busque la cuenta del usuario o del equipo en la base de datos de la cuenta local.

## <a name="current-applications"></a><a name="BKMK_APP"></a>Aplicaciones actuales
La autenticación NTLM aún se admite y se debe usar para la autenticación de Windows con sistemas configurados como miembros de un grupo de trabajo. La autenticación NTLM también se usa para la autenticación de inicio de sesión local en controladores que no son de \- dominio. La autenticación Kerberos versión 5 es el método de autenticación preferido para entornos Active Directory, pero una aplicación que no es de \- Microsoft o Microsoft podría seguir usando NTLM.

Reducir el uso del protocolo NTLM en un entorno de TI requiere el conocimiento de los requisitos de la aplicación implementada en NTLM y las estrategias y los pasos necesarios para configurar entornos informáticos para usar otros protocolos. Se han agregado nuevas herramientas y opciones para ayudarle a descubrir cómo se usa NTLM para restringir de forma selectiva el tráfico NTLM. Para obtener información sobre cómo analizar y restringir el uso de NTLM en sus entornos, vea [Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para tener acceso a la Guía de auditoría y restricción del uso de NTLM.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidad nueva y modificada
No hay ningún cambio en la funcionalidad de NTLM para Windows Server 2012.

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>Funcionalidad eliminada o en desuso
No hay ninguna funcionalidad eliminada o desusada para NTLM para Windows Server 2012.

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Información sobre el Administrador del servidor
NTLM no se puede configurar desde el Administrador del servidor. Puede usar la configuración de la directiva de seguridad o las directivas de grupo para administrar el uso de la autenticación NTLM entre sistemas de equipos. En un dominio, Kerberos es el protocolo de autenticación predeterminado.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vea también
En la tabla siguiente, se enumeran los recursos relevantes para NTLM y otras tecnologías de autenticación de Windows.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653.aspx)<p>[Cambios en la autenticación NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planeamiento**|[Guía de modelado de amenazas de infraestructura de TI](https://technet.microsoft.com/library/dd941826.aspx)<p>[Amenazas y contramedidas: configuración de seguridad en Windows Server 2003 y Windows Vista](https://technet.microsoft.com/library/dd162275.aspx)<p>[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 y Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<p>[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 R2 y Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Implementación**|[Protección ampliada para la autenticación](https://support.microsoft.com/kb/968389)<p>[Guía de auditoría y restricción del uso de NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<p>[Pregunte al Equipo de Servicios de directorio Bloqueo de NTLM y usted: metodologías de análisis y auditoría de la aplicación en Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<p>[Blog de autenticación de Windows](https://blogs.technet.com/authentication/)<p>[Configuración de MaxConcurrentAPI para la autenticación de paso a través de NTLM](https://support.microsoft.com/help/2688798/how-to-do-performance-tuning-for-ntlm-authentication-by-using-the-maxc)|
|**Desarrollo**|[Microsoft NTLM \( Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<p>[\[MS \- NLMP \] : \( especificación del Protocolo de autenticación NTLM de NT LAN Manager \)](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<p>[\[MS \- NNTP \] : NT LAN Manager \( NTLM \) autenticación: \( extensión NNTP de protocolo de transferencia de noticias de red \)](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<p>[\[\-Especificación del protocolo MS NTHT \] : NTLM sobre http](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Solución de problemas**|No disponible todavía|
|**Recursos de la comunidad**|[Entrada de blog sobre cuellos de botella de NTLM y el tiempo de ejecución de RPC](https://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



