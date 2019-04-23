---
title: Introducción a Control de cuentas de usuario
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887676"
---
# <a name="user-account-control-overview"></a>Introducción a Control de cuentas de usuario
Control de cuentas de usuario \(UAC\) es un componente fundamental de la visión global de seguridad de Microsoft.  UAC ayuda a atenuar los efectos de un programa malintencionado.

## <a name="BKMK_OVER"></a>Descripción de la característica
UAC permite a los usuarios iniciar sesión en sus equipos con una cuenta de usuario estándar. Los procesos que se inician con un token de usuario estándar pueden realizar tareas usando los derechos de acceso concedidos a un usuario estándar. Por ejemplo, el Explorador de Windows hereda automáticamente los permisos de nivel de usuario estándar. Además, todos los programas que se ejecutan mediante el Explorador de Windows \(por ejemplo, haciendo doble\-al hacer clic en un acceso directo de la aplicación\) también ejecutar con el conjunto de permisos de usuario estándar. Muchas aplicaciones, las que se incluyen con el sistema operativo, incluidas están diseñadas para funcionar correctamente en este modo.

Otras aplicaciones, especialmente aquellos que no se diseñaron específicamente con la configuración de seguridad en mente, a menudo requieren permisos adicionales para ejecutarse correctamente. Estos tipos de programas se conocen como aplicaciones heredadas. Además, las acciones como instalar software nuevo y efectuar los cambios de configuración en programas como Firewall de Windows, requieren más permisos que lo que está disponible para una cuenta de usuario estándar.

Cuando un necesidades de aplicaciones para ejecutar con derechos de usuario estándar más de, UAC puede restaurar los grupos de usuarios adicionales al token. Esto permite al usuario que tenga el control explícito de los programas que se realicen cambios de nivel de sistema a su equipo o dispositivo.

## <a name="BKMK_APP"></a>Aplicaciones prácticas
Modo de aprobación de administrador en UAC ayuda a evitar que programas malintencionados instalar de forma silenciosa sin conocimiento del administrador. También ayuda a proteger contra el sistema involuntario\-amplios cambios. Por último, se puede usar para aplicar un mayor nivel de cumplimiento donde los administradores deben consentir activamente cada proceso administrativo o proporcionar credenciales para ellos.



