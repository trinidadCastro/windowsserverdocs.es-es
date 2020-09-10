---
title: Introducción a Control de cuentas de usuario
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 2729bafe910db6814479464a007ce0e49f8b3205
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638855"
---
# <a name="user-account-control-overview"></a>Introducción a Control de cuentas de usuario
Control de cuentas \( de usuario UAC \) es un componente fundamental de la visión general de la seguridad de Microsoft.  UAC ayuda a atenuar los efectos de un programa malintencionado.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descripción de la característica
UAC permite a los usuarios iniciar sesión en sus equipos con una cuenta de usuario estándar. Los procesos que se inician con un token de usuario estándar pueden realizar tareas usando los derechos de acceso concedidos a un usuario estándar. Por ejemplo, el Explorador de Windows hereda automáticamente los permisos de nivel de usuario estándar. Además, los programas que se ejecutan mediante \( el explorador de Windows, por ejemplo, al \- hacer doble clic en el acceso directo de una aplicación \) también se ejecutan con el conjunto estándar de permisos de usuario. Muchas aplicaciones, incluidas las que se incluyen con el sistema operativo, están diseñadas para funcionar correctamente de esta manera.

Otras aplicaciones, especialmente aquellas que no se diseñaron específicamente con la configuración de seguridad en mente, suelen requerir permisos adicionales para ejecutarse correctamente. Estos tipos de programas se conocen como aplicaciones heredadas. Además, las acciones como la instalación de software nuevo y la realización de cambios de configuración en programas como el Firewall de Windows, requieren más permisos de los que están disponibles para una cuenta de usuario estándar.

Cuando una aplicación necesita ejecutarse con más derechos de usuario estándar, UAC puede restaurar grupos de usuarios adicionales en el token. Esto permite al usuario tener un control explícito de los programas que realizan cambios en el nivel del sistema en su equipo o dispositivo.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas
El modo de aprobación de administrador en UAC ayuda a evitar que programas malintencionados se instalen de forma silenciosa sin conocimiento del administrador. También ayuda a protegerse de cambios en todo el sistema involuntarios \- . Por último, se puede usar para aplicar un mayor nivel de cumplimiento donde los administradores deben consentir activamente cada proceso administrativo o proporcionar credenciales para ellos.



