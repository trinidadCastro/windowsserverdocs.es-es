---
title: Solución de problemas de AD FS
description: Este documento describe cómo solucionar diversos aspectos de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 60ecf94b72e58aed4d3718b19f6007cdad1c9578
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840956"
---
# <a name="troubleshooting-ad-fs"></a>Solución de problemas de AD FS
AD FS tiene muchas piezas móviles, afecta a muchos factores diferentes y tiene muchas dependencias diferentes.  Naturalmente, esto puede dar lugar a problemas diversos.  Este documento está diseñado para ayudarle a comenzar sobre cómo solucionar estos problemas.  Este documento le presentará las áreas típicas que debe centrarse en cómo habilitar características para obtener información adicional y diversas herramientas que pueden usarse para realizar un seguimiento de problemas.  

>[!NOTE]
>Para obtener más información, consulte [ADFS ayuda](http://adfshelp.microsoft.com) que proporciona herramientas eficaces en uno que colocan resulta más fácil para los usuarios y administradores a solucionar problemas de autenticación a un ritmo más rápido. 


## <a name="what-to-check-first"></a>Qué se debe comprobar primero
Antes de profundizar en la solución de problemas detallada, hay algunas cosas que debe comprobar primero.  Estas sobrecargas son:
- **¿Configuración de DNS** -puede resolver el nombre del servicio de federación?  Esta acción debería resolver a la dirección IP de ya sea del equilibrador de carga o la dirección IP de uno de los servidores de AD FS en la granja de servidores.  Para obtener más información, consulte [AD FS solución de problemas: DNS](ad-fs-tshoot-dns.md).
- **¿Puntos de conexión de AD FS** -puede examinar los puntos de conexión de AD FS?  Examinando Esto puede determinar si el servidor web de AD FS está respondiendo a las solicitudes.  Si puede obtener este archivo, a continuación, sabrá que AD FS está atendiendo las solicitudes en el puerto 443 perfectamente.  Para obtener más información, consulte [AD FS solución de problemas: puntos de conexión](ad-fs-tshoot-endpoints.md).
- **¿Inicio de sesión iniciado por IDP en** -puede iniciar sesión y autenticarse a través de la página de inicio de sesión Idp-Initiated?  Deberá asegurarse de que se ha habilitado esta página porque está deshabilitado de forma predeterminada.  Use `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` para habilitar la página.  Si puede iniciar sesión y autenticarse, sabrá que AD FS funciona en esta área.  Para obtener más información, consulte [AD FS solución de problemas: inicio de sesión](ad-fs-tshoot-initiatedsignon.md).
##  <a name="common-troubleshooting-areas"></a>Áreas de solución de problemas comunes

|Nombre|Descripción|
|-----|-----|
|[Auditoría y registro de eventos](ad-fs-tshoot-logging.md)|Use los registros de eventos de Windows para ver el nivel y poca información de alto nivel a través de los registros de seguimiento y administración.  También puede usarse para ver la auditoría de seguridad.|
|[Conectividad de SQL](ad-fs-tshoot-sql.md)|Información sobre cómo probar la conectividad entre los servidores de AD FS y las bases de datos SQL de back-end|
|[Emisión de notificaciones](ad-fs-tshoot-claims-issuance.md)|Información acerca de cómo determinar si AD FS es emitir notificaciones correctamente.|
|[Detección de bucles](ad-fs-tshoot-loop.md)|Información sobre la determinación y evitar que los usuarios que se ha devuelto y hacia atrás entre el proveedor de identidades y un punto de reunión.|
|[Certificados](ad-fs-tshoot-certs.md)|Problemas de certificados típicos que pueden surgir|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Información sobre cómo instalar y usar Fiddler|
|[WS-Federation con Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Seguimiento de Fiddler detallados de una interacción de WS-Federation|
|[Reglas de notificación](ad-fs-tshoot-claims-rules.md)|Información sobre cómo solucionar problemas de las reglas de notificación y su sintaxis|
|[Autenticación integrada de Windows](ad-fs-tshoot-iwa.md)|Información sobre cómo solucionar problemas de autenticación integrada.|
|[Azure AD](ad-fs-tshoot-azure.md)|Información sobre cómo solucionar problemas de interacción de AD FS con Azure AD.|
|[Analizador de AD FS diagnósticos](ad-fs-diagnostics-analyzer.md)|Analizador de diagnóstico de Ayuda de AD FS puede ayudar a realizar comprobaciones básicas de AD FS mediante el módulo de PowerShell de diagnóstico. 