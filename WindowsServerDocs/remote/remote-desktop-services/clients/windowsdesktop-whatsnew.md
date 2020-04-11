---
title: Novedades del cliente de escritorio de Windows
description: Obtén información sobre los cambios recientes en el cliente de Escritorio remoto para el escritorio de Windows
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 03/24/2020
ms.localizationpriority: medium
ms.openlocfilehash: 34f5fdb5a2826173edf471fd65248008761863dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861418"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novedades del cliente de escritorio de Windows

Puedes obtener más información detallada sobre el cliente de escritorio de Windows en [Introducción al cliente de escritorio de Windows](windowsdesktop.md). A continuación, encontrarás las actualizaciones más recientes del cliente.

## <a name="latest-client-versions"></a>Versiones más recientes del cliente

El cliente se puede configurar para distintos [grupos de usuarios](windowsdesktop-admin.md#configure-user-groups). En la tabla siguiente se enumeran las versiones actuales disponibles para cada grupo de usuarios:

|Grupo de usuarios |Version  |
|-----------|---------|
|Público     |1.2.790  |
|Insider    |1.2.790  |

## <a name="updates-for-version-12790"></a>Actualizaciones para la versión 1.2.790

*Fecha de publicación: 24/03/2020*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4siSh), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4siSi), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4sllb)

- Se ha cambiado el nombre de la acción "Actualizar" de las áreas de trabajo por "Actualizar" para mantener la coherencia con otros clientes de Escritorio remoto.
- Ahora puedes actualizar un área de trabajo directamente desde su menú contextual.
- La actualización manual de un área de trabajo ahora garantiza que se actualice todo el contenido local.
- Ahora puedes restablecer los datos de usuario del cliente desde la página Acerca de sin necesidad de desinstalar la aplicación.
- También puedes restablecer los datos de usuario del cliente mediante msrdcw.exe/reset con un parámetro /f opcional para omitir el mensaje.
- Ahora buscaremos automáticamente una actualización de cliente al navegar a la página Acerca de.
- Se ha actualizado el color de los botones para mantener la coherencia.

## <a name="updates-for-version-12675"></a>Actualizaciones de la versión 1.2.675

*Fecha de publicación: 25/02/2020*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qeak), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qm7h), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qm7g)

- Ahora se bloquean las conexiones con el escritorio virtual de Windows si falta la firma en el archivo RDP o si se ha modificado alguna de las propiedades de signscope.
- Cuando un área de trabajo está vacía o se ha quitado, el centro de conexión ya no parece estar vacío.
- Se ha agregado el identificador de actividad y el código de error en los mensajes de desconexión para mejorar la solución de problemas. Puedes copiar el mensaje del cuadro de diálogo con **Control + C**.
- Se corrigió un problema que hacía que la configuración de conexión del escritorio no detectase las pantallas.
- Las actualizaciones de cliente ya no reinician automáticamente el equipo.
- Los iconos sin ventanas ya no deberían aparecer en la barra de tareas.

## <a name="updates-for-version-12605"></a>Actualizaciones para la versión 1.2.605

*Fecha de publicación: 29/01/2020*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oHrD), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oJZs), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oXhD)

- Ahora puedes seleccionar las pantallas que se van a usar para las conexiones de escritorio. Para cambiar esta configuración, haz clic con el botón derecho en el icono de conexión a escritorio y selecciona **Configuración**.
- Se ha corregido un problema que provocaba que la configuración de conexión no mostrara los factores de escala disponibles correctos.
- Se ha corregido un problema que provocaba que el Narrador no pudiera leer el cuadro de diálogo mostrado mientras se iniciaba la conexión.
- Se ha corregido un problema que provocaba que se mostrara un nombre de usuario incorrecto cuando los nombres de Azure Active Directory y Active Directory no coincidían.
- Se corrigió un problema que provocaba que el cliente dejara de responder al iniciar una conexión mientras no estaba conectado a ninguna red.
- Se corrigió un problema que provocaba que el cliente dejara de responder al conectar un casco.

## <a name="updates-for-version-12535"></a>Actualizaciones para la versión 1.2.535

*Fecha de publicación: 04/12/2019*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jH), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jL), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k27O)

- Ahora puedes obtener acceso directo a la información sobre las actualizaciones, gracias al botón para obtener más opciones de la barra de comandos que está en la parte superior del cliente.
- Ahora puedes notificar los comentarios desde la barra de comandos del cliente.
- Ahora, la opción Comentarios solo se muestra si el Centro de opiniones está disponible.
- Ahora, la notificación de actualización no se muestra si las notificaciones se han deshabilitado mediante la directiva.
- Se corrigió un problema que impedía el inicio de algunos archivos RDP.
- Se corrigió un bloqueo al iniciar el cliente debido a daños en algunos valores de configuración persistentes.

## <a name="updates-for-version-12431"></a>Actualizaciones de la versión 1.2.431

*Fecha de publicación: 12/11/2019*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48kow), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48koA), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48zYj)

- Ya están disponibles las versiones de 32 bits y ARM64 del cliente.
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

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3LkSa)

- Se han mejorado los idiomas de reserva para la versión localizada. (Por ejemplo, FR-CA se mostrará correctamente en francés, en lugar de en inglés).
- Al quitar una suscripción, el cliente ahora quita correctamente las credenciales guardadas del Administrador de credenciales.
- El proceso de actualización del cliente ahora está desatendido una vez que se ha iniciado y el cliente se reiniciará una vez completado.
- El cliente ahora se puede usar en el modo S de Windows 10.
- Se corrigió un problema que hacía que se produjera un error en el proceso de actualización de usuarios con un espacio en su nombre de usuario.
- Se ha corregido un bloqueo que se producía al autenticarse durante una conexión.
- Se ha corregido un bloqueo que se producía al cerrar el cliente.
