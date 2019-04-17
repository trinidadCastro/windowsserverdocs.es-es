---
title: "Información general sobre el Control de cuentas de usuario"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 90ce72cb3d1850563d16a12d09a6872d107c0690
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="user-account-control-overview"></a>Información general sobre el Control de cuentas de usuario
Usuario Control de cuentas \(UAC\) es un componente fundamental de la visión global de seguridad de Microsoft.  UAC te ayuda a mitigar el impacto de un programa malintencionado.

## <a name="BKMK_OVER"></a>Descripción de la característica
UAC permite a los usuarios iniciar sesión en sus equipos con una cuenta de usuario estándar. Procesos iniciados mediante un token de usuario estándar pueden realizar tareas usando los derechos de acceso concedidos a un usuario estándar. Por ejemplo, el Explorador de Windows hereda automáticamente los permisos de nivel de usuario estándar. Además, todos los programas que se ejecutan con Windows Explorer \ (por ejemplo, haciendo clic en el double\ shortcut\ de una aplicación) también se ejecutan con el conjunto de permisos de usuario estándar. Muchas aplicaciones, incluidas aquellas que están incluidos en el propio sistema operativo, están diseñadas para funcionar correctamente de este modo.

Otras aplicaciones, especialmente aquellos que no se diseñaron específicamente teniendo en mente, configuración de seguridad a menudo requieren permisos adicionales para ejecutarse correctamente. Estos tipos de programas se conocen como aplicaciones heredadas. Además, las acciones como instalar software nuevo y realizar cambios de configuración a programas como Firewall de Windows, requieren más permisos de lo que está disponible para una cuenta de usuario estándar.

Cuando un necesita aplicaciones ejecutar con derechos de usuario más de estándar, UAC puede restaurar grupos de usuarios adicionales en el token. Esto permite que el usuario tenga control explícito de los programas que están realizando cambios en el sistema a su equipo o dispositivo.

## <a name="BKMK_APP"></a>Aplicaciones prácticas
Modo de aprobación de administrador de UAC ayuda a impedir la instalación silenciosa sin conocimiento del Administrador de programas malintencionados. También ayuda a evitar cambios inadvertidos de todo el FAT32\. Por último, se puede usar para aplicar un mayor nivel de cumplimiento donde los administradores deben consentimiento o proporcionar credenciales para cada proceso administrativo activamente.



