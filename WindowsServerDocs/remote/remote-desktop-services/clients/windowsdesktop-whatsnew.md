---
title: Novedades del cliente de escritorio de Windows
description: Obtén información sobre los cambios recientes en el cliente de Escritorio remoto para el escritorio de Windows
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 05/27/2020
ms.localizationpriority: medium
ms.openlocfilehash: 0d49c49def8b110f42a6d56354c73e5a75b04b7e
ms.sourcegitcommit: 4fec7d82f0772d03a9e8cac20092a4309b0f796e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84025516"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novedades del cliente de escritorio de Windows

Puedes obtener más información detallada sobre el cliente de escritorio de Windows en [Introducción al cliente de escritorio de Windows](windowsdesktop.md). A continuación, encontrarás las actualizaciones más recientes del cliente.

## <a name="latest-client-versions"></a>Versiones más recientes del cliente

El cliente se puede configurar para distintos [grupos de usuarios](windowsdesktop-admin.md#configure-user-groups). En la tabla siguiente se enumeran las versiones actuales disponibles para cada grupo de usuarios:

|Grupo de usuarios |Version  |
|-----------|---------|
|Público     |1.2.1026 |
|Insider    |1.2.1026 |

## <a name="updates-for-version-121026"></a>Actualizaciones para la versión 1.2.1026

*Fecha de publicación: 27/05/2020*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4xsGB), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4xd8P), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4xq7m)

- Al suscribirte, ahora puedes elegir tu cuenta en lugar de escribir tu dirección de correo electrónico.
- Se agregó una nueva opción **Subscribe with URL** (Suscribirse con dirección URL) que te permite especificar la dirección URL del área de trabajo a la que te vas a suscribir, o aprovechar la [detección de correo electrónico](../rds-email-discovery.md) cuando esté disponible en los casos en los que tus recursos no se encuentren automáticamente. Esto es similar al proceso de suscripción en el resto de los clientes de Escritorio remoto. Se puede usar para suscribirse directamente a las áreas de trabajo de la actualización de primavera de 2020 de WVD.
- Se agregó compatibilidad para suscribirse a un área de trabajo mediante un nuevo [esquema de URI](remote-desktop-uri.md) que se puede enviar en un correo electrónico a los usuarios o agregarse a un sitio web de soporte técnico.
- Se ha agregado un nuevo cuadro de diálogo **Connection information** (Información de conexión), que proporciona detalles del cliente, la red y el servidor de las sesiones de escritorio y aplicación. Puedes acceder al cuadro de diálogo desde la barra de conexión en el modo de pantalla completa, o desde el menú del sistema cuando estés dentro de la ventana.
- Las sesiones de escritorio iniciadas en modo de ventana ahora se maximizan siempre en lugar de pasar al modo de pantalla completa al maximizar la ventana. Usa la opción **Pantalla completa** del menú sistema para entrar en el modo de pantalla completa.
- El aviso de cancelación de suscripción ahora muestra un icono de advertencia y muestra los nombres de área de trabajo como una lista con viñetas.
- Se ha agregado una sección de detalles a otros cuadros de diálogo de error para ayudar a diagnosticar problemas.
- Se ha agregado una marca de tiempo a la sección de detalles de los cuadros de diálogo de error.
- Se ha corregido un problema por el que la opción del archivo RDP **desktop size id** no funcionaba correctamente.
- Se ha corregido un problema por el que la opción de pantalla **Update the resolution on resize** (Actualizar la resolución al cambiar de tamaño) no se aplicaba después de iniciar la sesión.
- Se han corregido los problemas de localización en el panel de configuración del escritorio.
- Se ha corregido el tamaño del recuadro de enfoque al pasar con el tabulador por los controles del panel de configuración del escritorio.
- Se ha corregido un problema que hacía que los nombres de los recursos resultaran difíciles de leer en el modo de contraste alto.
- Se ha corregido un problema que hacía que la notificación de actualización en el centro de actividades se mostrara más de una vez al día.

## <a name="updates-for-version-12945"></a>Actualizaciones de la versión 1.2.945

*Fecha de publicación: 28/04/2020*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4vhNM), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4vhNO), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4vuSV)

- Se han agregado nuevas opciones de configuración de pantalla para las conexiones de escritorio disponibles al hacer clic con el botón derecho en un icono de escritorio del centro de conexiones.
  - Ahora hay tres opciones de configuración de pantalla: **todas las pantallas**, **pantalla única** y **pantallas seleccionadas**.
  - Ahora solo se muestra la configuración disponible cuando se selecciona una configuración de pantalla.
  - En el modo de pantallas seleccionadas, la nueva opción **Maximize to current displays** (Maximizar a las pantallas actuales) te permite cambiar dinámicamente las pantallas que se usan para la sesión sin necesidad de volver a conectar. Cuando está habilitada, al maximizar la sesión, se activa la pantalla completa en todas las pantallas que toca la ventana de sesión.
  - Se ha agregado la nueva opción **Single display when windowed** (Pantalla única para varias ventanas) para los modos de todas las pantallas y pantallas seleccionadas. Esta opción cambia la sesión automáticamente a una pantalla única al salir del modo de pantalla completa y vuelve automáticamente a varias pantallas al maximizar la ventana.
- Hemos agregado un nuevo grupo de **Configuración de pantalla** al menú del sistema que aparece al hacer clic con el botón derecho en la barra de títulos de una sesión de escritorio en ventanas. Esto te permitirá cambiar algunas opciones de configuración de forma dinámica durante una sesión. Por ejemplo, puedes cambiar las nuevas opciones **Single display mode when windowed** (Modo de pantalla única para varias ventanas) y **Maximize to current displays** (Maximizar a pantallas actuales).
- Al salir de la pantalla completa, la ventana de sesión volverá a la que era su ubicación original cuando se activó por primera vez a la pantalla completa.
- La actualización en segundo plano de las áreas de trabajo se ha cambiado a cada cuatro horas en lugar de cada hora. Ahora se produce una actualización automáticamente al iniciar el cliente.
- Al restablecer los datos de usuario desde la página Acerca de, ahora se redirige al centro de conexión al finalizar, en lugar de cerrar el cliente.
- Se ha reordenado los elementos del menú del sistema de las conexiones de escritorio y el tema de ayuda ahora apunta a la documentación del cliente.
- Se han solucionado algunos problemas de accesibilidad con la navegación por pestañas y los lectores de pantalla.
- Se ha corregido un problema que hacía que el cuadro de diálogo de autenticación de Azure Active Directory apareciera detrás de la ventana de sesión.
- Se ha corregido un problema de parpadeo y reducción al arrastrar una ventana de sesión de escritorio entre las pantallas de distintos factores de escala.
- Se corrigió un error que se producía al redirigir las cámaras.
- Se han corregido varios bloqueos para mejorar la confiabilidad.

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
