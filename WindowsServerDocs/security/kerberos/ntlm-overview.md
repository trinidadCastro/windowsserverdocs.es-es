---
title: NTLM Overview
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: d12e61fac3ba6c44dbcac35ea3097e95db54752a
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766788"
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

Reducir el uso del protocolo NTLM en un entorno de TI requiere el conocimiento de los requisitos de la aplicación implementada en NTLM y las estrategias y los pasos necesarios para configurar entornos informáticos para usar otros protocolos. Se han agregado nuevas herramientas y opciones para ayudarle a descubrir cómo se usa NTLM para restringir de forma selectiva el tráfico NTLM. Para obtener información sobre cómo analizar y restringir el uso de NTLM en sus entornos, vea [Introducción a la restricción de la autenticación NTLM](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560653(v=ws.10)) para tener acceso a la Guía de auditoría y restricción del uso de NTLM.

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
|**Evaluación del producto**|[Introducción a la restricción de la autenticación NTLM](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560653(v=ws.10))<p>[Cambios en la autenticación NTLM](/previous-versions/windows/it-pro/windows-7/dd566199(v=ws.10))|
|**Planeamiento**|[Guía de modelado de amenazas de infraestructura de TI](/previous-versions/tn-archive/dd941826(v=technet.10))<p>[Amenazas y contramedidas: configuración de seguridad en Windows Server 2003 y Windows Vista](/previous-versions/tn-archive/dd162275(v=technet.10))<p>[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 y Windows Vista](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349791(v=ws.10))<p>[Guía de amenazas y contramedidas: Configuración de seguridad en Windows Server 2008 R2 y Windows 7](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125921(v=ws.10))|
|**Implementación**|[Protección ampliada para la autenticación](https://support.microsoft.com/kb/968389)<p>[Guía de auditoría y restricción del uso de NTLM](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/jj865674(v=ws.10))<p>[Pregunte al Equipo de Servicios de directorio Bloqueo de NTLM y usted: metodologías de análisis y auditoría de la aplicación en Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<p>[Blog de autenticación de Windows](https://blogs.technet.com/authentication/)<p>[Configuración de MaxConcurrentAPI para la autenticación de paso a través de NTLM](https://support.microsoft.com/help/2688798/how-to-do-performance-tuning-for-ntlm-authentication-by-using-the-maxc)|
|**Desarrollo**|[Microsoft NTLM \( Windows\)](/windows/win32/secauthn/microsoft-ntlm)<p>[\[MS \- NLMP \] : \( especificación del Protocolo de autenticación NTLM de NT LAN Manager \)](/openspecs/windows_protocols/ms-nlmp/b38c36ed-2804-4868-a9ff-8dd3182128e4)<p>[\[MS \- NNTP \] : NT LAN Manager \( NTLM \) autenticación: \( extensión NNTP de protocolo de transferencia de noticias de red \)](/openspecs/windows_protocols/ms-nntp/73ae7d96-30fe-4750-807c-bfe7c38b3a0a)<p>[\[\-Especificación del protocolo MS NTHT \] : NTLM sobre http](/openspecs/windows_protocols/ms-ntht/f09cf6e1-529e-403b-a8a5-7368ee096a6a)|
|**Solución de problemas**|No disponible todavía|
|**Recursos de la comunidad**|[Entrada de blog sobre cuellos de botella de NTLM y el tiempo de ejecución de RPC](https://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|