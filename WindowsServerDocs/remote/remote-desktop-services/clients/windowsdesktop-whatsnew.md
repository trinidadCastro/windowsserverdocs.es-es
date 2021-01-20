---
title: Novedades del cliente de escritorio de Windows
description: Obtén información sobre los cambios recientes en el cliente de Escritorio remoto para el escritorio de Windows
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 01/12/2021
ms.localizationpriority: medium
ms.openlocfilehash: 1d5dc9e76680b6c222f67b8f94c12bedd57af263
ms.sourcegitcommit: 56297d3b8aa8f4796cb74b736d599d433aeee339
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98134811"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Novedades del cliente de escritorio de Windows

Puedes obtener más información detallada sobre el cliente de escritorio de Windows en [Introducción al cliente de escritorio de Windows](windowsdesktop.md). En este artículo, encontrará las actualizaciones más recientes del cliente.

## <a name="supported-client-versions"></a>Versiones del cliente admitidas

El cliente se puede configurar para distintos [grupos de usuarios](windowsdesktop-admin.md#configure-user-groups). En la tabla siguiente se enumeran las versiones actuales disponibles para cada grupo de usuarios:

|Grupo de usuarios |La versión más reciente  |Versión mínima admitida |
|-----------|----------------|--------------------------|
|Público     |1.2.1525        |1.2.945                   |
|Insider    |1.2.1670        |1.2.945                   |

## <a name="updates-for-version-121670-insider"></a>Actualizaciones de la versión 1.2.1670 (Insider)

*Fecha de publicación: 12/1/2021*

Descarga: [Windows de 64 bits](https://go.microsoft.com/fwlink/?linkid=2139233), [Windows de 32 bits](https://go.microsoft.com/fwlink/?linkid=2139144), [ARM64 de Windows](https://go.microsoft.com/fwlink/?linkid=2139368)

- Se ha agregado compatibilidad con la característica de protección de captura de pantalla para puntos de conexión de Windows 10. Para obtener más información, consulte [Procedimientos recomendados de seguridad del host de sesión](/azure/virtual-desktop/security-guide#session-host-security-best-practices).
- Se ha agregado compatibilidad con los servidores proxy que requieren autenticación para la suscripción de la fuente.
- El cliente ahora muestra una notificación con una opción de reintento si una actualización no se descarga correctamente.

## <a name="updates-for-version-121525"></a>Actualizaciones para la versión 1.2.1525

*Fecha de publicación: 01/12/2020*

Descarga: [Windows de 64 bits](https://go.microsoft.com/fwlink/?linkid=2139369), [Windows de 32 bits](https://go.microsoft.com/fwlink/?linkid=2139456), [ARM64 de Windows](https://go.microsoft.com/fwlink/?linkid=2139370)

- Se ha agregado la vista de lista de los recursos remotos, para que los nombres de aplicación más largos sean legibles.
- Se ha agregado el icono de notificación que aparece cuando hay disponible una actualización para el cliente.

## <a name="updates-for-version-121446"></a>Actualizaciones para la versión 1.2.1446

*Fecha de publicación: 27/10/2020*

Descarga: [Windows de 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4Hq7C), [Windows de 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4HvgF), [ARM64 de Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4Ho64)

- Se ha agregado la característica de actualización automática, que permite al cliente instalar automáticamente las actualizaciones más recientes.
- El cliente ahora distingue entre fuentes diferentes del centro de conexiones.
- Se ha corregido un problema por el que la cuenta de suscripción no coincide con la cuenta con la que el usuario inició sesión.
- Se ha corregido un problema que impedía a algunos usuarios acceder a las aplicaciones remotas a través de un archivo descargado.
- Se ha corregido un problema con la redirección de tarjetas inteligentes.

## <a name="updates-for-version-121364"></a>Actualizaciones de la versión 1.2.1364

*Fecha de publicación: 22/09/2020*

- Se corrigió un problema por el que el inicio de sesión único (SSO) no funcionaba en Windows 7.
- Se corrigió el error de conexión que se producía al realizar una llamada de Teams o unirse a una mientras otra aplicación tenía una secuencia de audio abierta en modo exclusivo y estaba habilitada la optimización de medios para Teams.
- Se corrigió un error que se producía al enumerar los dispositivos de audio o vídeo en Teams cuando estaba habilitada la optimización de medios para Teams.
- A agregó un vínculo "¿Necesita ayuda con la configuración?" a la página de configuración del escritorio.
- Se corrigió un problema con el botón "Suscribirse" que se producía al usar los temas oscuros de contraste alto.

## <a name="updates-for-version-121275"></a>Actualizaciones de la versión 1.2.1275

*Fecha de publicación: 25/08/2020*

- Se agregó la funcionalidad para detectar automáticamente las nubes soberanas a partir de la identidad del usuario.
- Se agregó la funcionalidad para habilitar las suscripciones de URL personalizadas para todos los usuarios.
- Se corrigió un problema de anclaje de aplicaciones en la barra de tareas de la fuente.
- Se corrigió un bloqueo al suscribirse con una dirección URL.
- Se mejoró la experiencia al arrastrar ventanas de aplicaciones remotas con la función táctil o el lápiz.
- Se corrigió un problema con la localización.

## <a name="updates-for-version-121186"></a>Actualizaciones para la versión 1.2.1186

*Fecha de publicación: 28/07/2020*

- Ahora puede suscribirse a áreas de trabajo con varias cuentas de usuario, mediante la opción ( **...** ) del menú de desbordamiento en la barra de comandos de la parte superior del cliente. Para diferenciar las áreas de trabajo, los títulos del área de trabajo ahora incluyen el nombre de usuario, al igual que todos los títulos de acceso directo de las aplicaciones.
- Se ha agregado información adicional a los mensajes de error de suscripción para mejorar la solución de problemas.
- El estado contraído o expandido de las áreas de trabajo ahora se conserva durante una actualización.
- Se ha agregado un botón **Send Diagnostics and Close** (Enviar diagnóstico y cerrar) al cuadro de diálogo **Información de conexión**.
- Se ha corregido un problema con las teclas Ctrl+Mayús en las sesiones remotas.

## <a name="updates-for-version-121104"></a>Actualizaciones para la versión 1.2.1104

*Fecha de publicación: 23/06/2020*

- Se actualizó la lógica de detección automática para la opción **Subscribir** para admitir la versión integrada en Azure Resource Manager de Windows Virtual Desktop. Los clientes con solo recursos de Windows Virtual Desktop ya no tienen que proporcionar consentimiento para Windows Virtual Desktop (clásico).
- Se mejoró la compatibilidad para dispositivos con valores altos de PPP con un factor de escala de hasta un 400 %.
- Se corrigió un problema en el que el cuadro de diálogo de desconexión no aparecía.
- Se corrigió un problema en que la información sobre herramientas de la barra de comandos permanecía visible más tiempo del esperado.
- Se corrigió un bloqueo que se producía al intentar suscribirse inmediatamente después de una actualización.
- Se corrigió un bloqueo del análisis incorrecto de fecha y hora en algunos idiomas.

## <a name="updates-for-version-121026"></a>Actualizaciones para la versión 1.2.1026

*Fecha de publicación: 27/05/2020*

- Al suscribirte, ahora puedes elegir tu cuenta en lugar de escribir tu dirección de correo electrónico.
- Se agregó una nueva opción **Subscribe with URL** (Suscribirse con dirección URL) que te permite especificar la dirección URL del área de trabajo a la que te vas a suscribir, o aprovechar la [detección de correo electrónico](../rds-email-discovery.md) cuando esté disponible en los casos en los que tus recursos no se encuentren automáticamente. Esto es similar al proceso de suscripción en el resto de los clientes de Escritorio remoto. Se puede usar para suscribirse directamente a las áreas de trabajo de Windows Virtual Desktop.
- Se agregó compatibilidad para suscribirse a un área de trabajo mediante un nuevo [esquema de URI](remote-desktop-uri.md) que se puede enviar en un correo electrónico a los usuarios o agregarse a un sitio web de soporte técnico.
- Se ha agregado un nuevo cuadro de diálogo **Connection information** (Información de conexión), que proporciona detalles del cliente, la red y el servidor de las sesiones de escritorio y aplicación. Puedes acceder al cuadro de diálogo desde la barra de conexión en el modo de pantalla completa, o desde el menú del sistema cuando estés dentro de la ventana.
- Las sesiones de escritorio iniciadas en modo de ventana ahora se maximizan siempre en lugar de pasar al modo de pantalla completa al maximizar la ventana. Usa la opción **Pantalla completa** del menú sistema para entrar en el modo de pantalla completa.
- El aviso de cancelación de suscripción ahora muestra un icono de advertencia y muestra los nombres de área de trabajo como una lista con viñetas.
- Se ha agregado una sección de detalles a otros cuadros de diálogo de error para ayudar a diagnosticar problemas.
- Se ha agregado una marca de tiempo a la sección de detalles de los cuadros de diálogo de error.
- Se ha corregido un problema por el que la opción del archivo RDP **desktop size ID** no funcionaba correctamente.
- Se ha corregido un problema por el que la opción de pantalla **Update the resolution on resize** (Actualizar la resolución al cambiar de tamaño) no se aplicaba después de iniciar la sesión.
- Se han corregido los problemas de localización en el panel de configuración del escritorio.
- Se ha corregido el tamaño del recuadro de enfoque al pasar con el tabulador por los controles del panel de configuración del escritorio.
- Se ha corregido un problema que hacía que los nombres de los recursos resultaran difíciles de leer en el modo de contraste alto.
- Se ha corregido un problema que hacía que la notificación de actualización en el centro de actividades se mostrara más de una vez al día.

## <a name="updates-for-version-12945"></a>Actualizaciones de la versión 1.2.945

*Fecha de publicación: 28/04/2020*

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

- Se ha cambiado el nombre de la acción "Actualizar" de las áreas de trabajo por "Actualizar" para mantener la coherencia con otros clientes de Escritorio remoto.
- Ahora puedes actualizar un área de trabajo directamente desde su menú contextual.
- La actualización manual de un área de trabajo ahora garantiza que se actualice todo el contenido local.
- Ahora puedes restablecer los datos de usuario del cliente desde la página Acerca de sin necesidad de desinstalar la aplicación.
- También puedes restablecer los datos de usuario del cliente mediante msrdcw.exe/reset con un parámetro /f opcional para omitir el mensaje.
- Ahora buscaremos automáticamente una actualización de cliente al navegar a la página Acerca de.
- Se ha actualizado el color de los botones para mantener la coherencia.

## <a name="updates-for-version-12675"></a>Actualizaciones de la versión 1.2.675

*Fecha de publicación: 25/02/2020*

- Ahora se bloquean las conexiones con el escritorio virtual de Windows si falta la firma en el archivo RDP o si se ha modificado alguna de las propiedades de signscope.
- Cuando un área de trabajo está vacía o se ha quitado, el centro de conexión ya no parece estar vacío.
- Se ha agregado el identificador de actividad y el código de error en los mensajes de desconexión para mejorar la solución de problemas. Puedes copiar el mensaje del cuadro de diálogo con **Control + C**.
- Se corrigió un problema que hacía que la configuración de conexión del escritorio no detectase las pantallas.
- Las actualizaciones de cliente ya no reinician automáticamente el equipo.
- Los iconos sin ventanas ya no deberían aparecer en la barra de tareas.

## <a name="updates-for-version-12605"></a>Actualizaciones para la versión 1.2.605

*Fecha de publicación: 29/01/2020*

- Ahora puedes seleccionar las pantallas que se van a usar para las conexiones de escritorio. Para cambiar esta configuración, haz clic con el botón derecho en el icono de conexión a escritorio y selecciona **Configuración**.
- Se ha corregido un problema que provocaba que la configuración de conexión no mostrara los factores de escala disponibles correctos.
- Se ha corregido un problema que provocaba que el Narrador no pudiera leer el cuadro de diálogo mostrado mientras se iniciaba la conexión.
- Se ha corregido un problema que provocaba que se mostrara un nombre de usuario incorrecto cuando los nombres de Azure Active Directory y Active Directory no coincidían.
- Se corrigió un problema que provocaba que el cliente dejara de responder al iniciar una conexión mientras no estaba conectado a ninguna red.
- Se corrigió un problema que provocaba que el cliente dejara de responder al conectar un casco.

## <a name="updates-for-version-12535"></a>Actualizaciones para la versión 1.2.535

*Fecha de publicación: 04/12/2019*

- Ahora puedes obtener acceso directo a la información sobre las actualizaciones, gracias al botón para obtener más opciones de la barra de comandos que está en la parte superior del cliente.
- Ahora puedes notificar los comentarios desde la barra de comandos del cliente.
- Ahora, la opción Comentarios solo se muestra si el Centro de opiniones está disponible.
- Ahora, la notificación de actualización no se muestra si las notificaciones se han deshabilitado mediante la directiva.
- Se corrigió un problema que impedía el inicio de algunos archivos RDP.
- Se corrigió un bloqueo al iniciar el cliente debido a daños en algunos valores de configuración persistentes.

## <a name="updates-for-version-12431"></a>Actualizaciones de la versión 1.2.431

*Fecha de publicación: 12/11/2019*

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

- Se han mejorado los idiomas de reserva para la versión localizada. (Por ejemplo, FR-CA se mostrará correctamente en francés, en lugar de en inglés).
- Al quitar una suscripción, el cliente ahora quita correctamente las credenciales guardadas del Administrador de credenciales.
- El proceso de actualización del cliente ahora está desatendido una vez que se ha iniciado y el cliente se reiniciará una vez completado.
- El cliente ahora se puede usar en el modo S de Windows 10.
- Se corrigió un problema que hacía que se produjera un error en el proceso de actualización de usuarios con un espacio en su nombre de usuario.
- Se ha corregido un bloqueo que se producía al autenticarse durante una conexión.
- Se ha corregido un bloqueo que se producía al cerrar el cliente.
