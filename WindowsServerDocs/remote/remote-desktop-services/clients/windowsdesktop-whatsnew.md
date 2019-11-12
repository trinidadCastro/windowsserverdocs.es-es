---
title: Novedades del cliente de escritorio de Windows
description: Obtén información sobre los cambios recientes en el cliente de Escritorio remoto para el escritorio de Windows
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 10/31/2019
ms.localizationpriority: medium
ms.openlocfilehash: 6a8e66398bc61a69250b84101a3cb66f2c8f3548
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73567071"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novedades del cliente de escritorio de Windows

Puedes obtener más información detallada sobre el cliente de escritorio de Windows en [Introducción al cliente de escritorio de Windows](windowsdesktop.md). A continuación, encontrarás las actualizaciones más recientes del cliente.

## <a name="latest-client-versions"></a>Versiones más recientes del cliente

El cliente se puede configurar para distintos [grupos de usuarios](windowsdesktop-admin.md#configure-user-groups). En la tabla siguiente se enumeran las versiones actuales disponibles para cada grupo de usuarios:

|Grupo de usuarios |Versión  |
|-----------|---------|
|Public     |1.2.247  |
|Insider    |1.2.428  |

## <a name="updates-for-version-12428"></a>Actualizaciones de la versión 1.2.428

*Fecha de publicación: 31/10/2019*

- Ya hay una vista previa disponible de las versiones de 32 bits y ARM64 del cliente.
- Ahora, el cliente guarda los cambios que realizas en la barra de conexión (como su posición, tamaño y estado anclado) y aplica los cambios en las sesiones.
- Se actualizaron los cuadros de diálogo del estado de conexión y la información de la puerta de enlace.
- Se solucionó un problema que hacía que se solicitaran dos credenciales al mismo tiempo al intentar establecer conexión después de que expirara el token de Azure Active Directory.
- En Windows 7, ahora se solicitan correctamente las credenciales a los usuarios si las habían guardado cuando el servidor no lo permite.
- La solicitud de Azure Active Directory ahora se muestra delante de la ventana de conexión cuando se vuelve a establecer conexión.
- Los elementos anclados en la barra de tareas ahora se actualizan cuando se actualiza una fuente.
- Se mejoró el desplazamiento en Connection Center al usar la función táctil.
- Se quitó la línea vacía del menú desplegable de resolución.
- Se quitaron las entradas innecesarias en el Administrador de credenciales de Windows.
- Las sesiones de escritorio ahora tienen el tamaño correcto al salir de la pantalla completa.
- El cuadro de diálogo de desconexión de RemoteApp ahora se muestra en primer plano al reanudar la sesión después de entrar en el modo de suspensión.
- Se solucionaron problemas de accesibilidad como la navegación con el teclado.

## <a name="updates-for-version-12247"></a>Actualizaciones de la versión 1.2.247

*Fecha de publicación: 17/09/2019*

- Se ha corregido un bloqueo que se producía al autenticarse durante una conexión.
- Se ha corregido un bloqueo que se producía al cerrar el cliente.

## <a name="updates-for-version-12246"></a>Actualizaciones de la versión 1.2.246

*Fecha de publicación: 28/08/2019*

- Se han mejorado los idiomas de reserva para la versión localizada. (Por ejemplo, FR-CA se mostrará correctamente en francés, en lugar de en inglés).
- Al quitar una suscripción, el cliente ahora quita correctamente las credenciales guardadas del Administrador de credenciales.
- El proceso de actualización del cliente ahora está desatendido una vez que se ha iniciado y el cliente se reiniciará una vez completado.
- El cliente ahora se puede usar en el modo S de Windows 10.
- Se corrigió un problema que hacía que se produjera un error en el proceso de actualización de usuarios con un espacio en su nombre de usuario.
