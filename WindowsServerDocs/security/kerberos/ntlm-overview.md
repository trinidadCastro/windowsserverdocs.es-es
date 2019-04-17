---
title: "Introducción a NTLM"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ntlm-overview"></a>Introducción a NTLM

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI describe NTLM, cualquier cambio en la funcionalidad y proporciona vínculos a recursos técnicos para la autenticación de Windows y NTLM para Windows Server 2012 y versiones anteriores.

## <a name="BKMK_OVER"></a>Descripción de la característica
La autenticación NTLM es una familia de protocolos de autenticación que se incluyen en el Msv1\_0.dll de Windows. LAN Manager versión 1 y 2 y NTLM versión 1 y 2 incluyen los protocolos de autenticación NTLM. Los protocolos de autenticación NTLM autentican usuarios y equipos basados en un mecanismo de challenge\ y respuesta que demuestra a un servidor o un controlador de dominio que un usuario conozca la contraseña asociada con una cuenta. Cuando se usa el protocolo NTLM, un servidor de recursos debe tomar una de las siguientes acciones para comprobar la identidad de un equipo o usuario cada vez que se necesita un nuevo token de acceso:

-   Ponte en contacto con un servicio de autenticación de dominio en el controlador de dominio para el dominio de la cuenta del equipo o del usuario, si la cuenta es una cuenta de dominio.

-   Busca la cuenta del equipo o del usuario en la base de datos de la cuenta local, si la cuenta es una cuenta local.

## <a name="BKMK_APP"></a>Aplicaciones actuales
La autenticación NTLM sigue siendo compatible y debe usarse para la autenticación de Windows con sistemas configurados como un miembro de un grupo de trabajo. También se usa la autenticación NTLM para la autenticación de inicio de sesión local en controladores de dominio de non\. Autenticación de la versión 5 de Kerberos es el método de autenticación preferido para entornos de Active Directory, pero una aplicación de Microsoft o de Microsoft non\ todavía usen NTLM.

Reducir el uso del protocolo NTLM en un entorno de TI requiere el conocimiento de los requisitos de la aplicación implementada en NTLM y las estrategias y los pasos necesarios para configurar el entorno informático usar otros protocolos. Nuevas herramientas y opciones de configuración se agregaron para descubrir cómo se utiliza NTLM con el fin de forma selectiva restringir el tráfico NTLM. Para obtener información sobre cómo analizar y restringir el uso de NTLM en los entornos, consulta [introducir la restricción de autenticación NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para acceder a la auditoría y restringir la Guía de uso NTLM.

## <a name="BKMK_NEW"></a>Funcionalidades nuevas y modificadas
No hay ningún cambio en la funcionalidad de NTLM para Windows Server 2012.

## <a name="BKMK_DEP"></a>Funcionalidad desusada o eliminada
No hay ninguna funcionalidad eliminó o desusada para NTLM para Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Información del administrador del servidor
NTLM no se puede configurar desde el administrador del servidor. Puedes usar configuración de directiva de seguridad o las directivas de grupo para administrar el uso de autenticación NTLM entre sistemas informáticos. En un dominio, Kerberos es el protocolo de autenticación predeterminado.

## <a name="BKMK_LINKS"></a>Consulta también
La siguiente tabla enumera los recursos más importantes para NTLM y otras tecnologías de autenticación de Windows.

|Tipo de contenido|Referencias|
|--------|-------|
|**Evaluación del producto**|[Introducción a la restricción de autenticación NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Cambios en la autenticación NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planeación**|[Guía de modelado de amenazas de infraestructura de TI](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Amenazas y contramedidas: configuración de seguridad de Windows Server 2003 y Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guía de amenazas y contramedidas: configuración de seguridad de Windows Server 2008 y Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guía de amenazas y contramedidas: configuración de seguridad de Windows Server 2008 R2 y Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Implementación**|[Protección extendida para la autenticación](https://support.microsoft.com/kb/968389)<br /><br />[Restringir la Guía de uso NTLM y auditoría](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Pregunte al equipo de servicios de directorio: bloqueo de NTLM y: análisis de la aplicación y auditoría metodologías en Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog de autenticación de Windows](https://blogs.technet.com/authentication/)<br /><br />[Configurar MaxConcurrentAPI para autenticación pass\ a través NTLM](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Desarrollo**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: especificación del protocolo de autenticación de NT LAN Manager \(NTLM\)](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: autenticación de NT LAN Manager \(NTLM\): \(NNTP\) extensión del protocolo de transferencia de noticias de red](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: NTLM sobre la especificación del protocolo HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Solución de problemas**|Aún no está disponible|
|**Recursos de la Comunidad**|[Aún muertos este caballo: cuellos de botella de NTLM y el tiempo de ejecución](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



