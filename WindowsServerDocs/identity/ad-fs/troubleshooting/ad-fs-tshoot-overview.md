---
title: Solución de problemas de AD FS
description: En este documento se describe cómo solucionar varios aspectos de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5fc5c2384a6d8d13f807a1ce99c5db78bee5b108
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950137"
---
# <a name="troubleshooting-ad-fs"></a>Solución de problemas de AD FS
AD FS tiene muchas piezas móviles, toca muchas cosas diferentes y tiene muchas dependencias diferentes.  Como es lógico, esto puede dar lugar a diversos problemas.  Este documento está diseñado para ayudarle a empezar a solucionar estos problemas.  En este documento se presentan las áreas típicas en las que se debe centrar, cómo habilitar características para obtener información adicional y diversas herramientas que se pueden usar para realizar un seguimiento de los problemas.  

>[!NOTE]
>Para obtener más información, consulte la [ayuda de ADFS](https://adfshelp.microsoft.com) , que proporciona herramientas eficaces en un solo lugar que facilita a los usuarios y administradores la resolución de problemas de autenticación a un ritmo más rápido. 


## <a name="what-to-check-first"></a>Qué comprobar primero
Antes de profundizar en la solución de problemas detallada, hay algunas cosas que debe comprobar en primer lugar.  Estos son:
- **Configuración de DNS** : ¿se puede resolver el nombre del servicio de Federación?  Esto debe resolverse en la dirección IP del equilibrador de carga o en la dirección IP de uno de los servidores AD FS de la granja.  Para obtener más información [, consulte solución de problemas de AD FS: DNS](ad-fs-tshoot-dns.md).
- **AD FS puntos de conexión** : ¿puede examinar los puntos de conexión de AD FS?  Al ir a este paso, puede determinar si el servidor Web AD FS responde a las solicitudes.  Si puede obtener acceso a este archivo, sabrá que AD FS está atendiendo solicitudes en 443.  Para obtener más información [, consulte solución de problemas de AD FS: puntos de conexión](ad-fs-tshoot-endpoints.md).
- **Inicio de sesión iniciado por IDP** : ¿se puede iniciar sesión y autenticarse a través de la página de inicio de sesión iniciada por IDP?  Debe asegurarse de que esta página se ha habilitado porque está deshabilitada de forma predeterminada.  Use `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` para habilitar la página.  Si puede iniciar sesión y autenticarse, sabrá que AD FS funciona en esta área.  Para obtener más información [, consulte solución de problemas de AD FS de inicio de sesión](ad-fs-tshoot-initiatedsignon.md).
  ##  <a name="common-troubleshooting-areas"></a>Áreas de solución de problemas comunes

|Nombre|Descripción|
|-----|-----|
|[Registro y auditoría de eventos](ad-fs-tshoot-logging.md)|Use los registros de eventos de Windows para ver la información de nivel alto y bajo de nivel a través de los registros de seguimiento y de administración.  También se puede usar para ver la auditoría de seguridad.|
|[Conectividad de SQL](ad-fs-tshoot-sql.md)|Información sobre cómo probar la conectividad entre los servidores de AD FS y las bases de datos SQL de back-end|
|[Emisión de notificaciones](ad-fs-tshoot-claims-issuance.md)|Información sobre cómo determinar si AD FS emite notificaciones correctamente.|
|[Detección de bucles](ad-fs-tshoot-loop.md)|Información sobre cómo determinar y evitar que los usuarios se devuelvan entre el IDP y el RP.|
|[Certificados](ad-fs-tshoot-certs.md)|Problemas de certificados Typcial que pueden surgir|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Información sobre cómo instalar y usar Fiddler|
|[WS-Federation con Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Seguimiento detallado de Fiddler de una interacción de WS-Federation|
|[Reglas de notificaciones](ad-fs-tshoot-claims-rules.md)|Información sobre la solución de problemas de reglas de notificaciones y su sintaxis|
|[Autenticación integrada de Windows](ad-fs-tshoot-iwa.md)|Información sobre la solución de problemas de la autenticación integrada.|
|[Azure AD](ad-fs-tshoot-azure.md)|Información sobre la solución de problemas de interacción AD FS con Azure AD.|
|[Analizador de diagnósticos de AD FS](ad-fs-diagnostics-analyzer.md)|AD FS el analizador de diagnósticos de ayuda puede ayudar a realizar comprobaciones básicas de AD FS mediante el módulo diagnósticos de PowerShell. 