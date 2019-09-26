---
title: Introducción al cliente de Android
description: Información general acerca del cliente de Android.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/16/2019
ms.localizationpriority: medium
ms.openlocfilehash: f9e8eb861961dc714a964012960e8742b721d4de
ms.sourcegitcommit: 081661f50d6dafb77180149956a02e679270c710
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71037599"
---
# <a name="get-started-with-the-android-client"></a>Introducción al cliente de Android

>Se aplica a: Android 4.1 y versiones posteriores

El cliente de Escritorio remoto para Android se puede usar con aplicaciones de Windows y escritorios directamente desde un dispositivo Android o Chromebook que sea compatible con Google Play Store.

Usa la siguiente información para comenzar. Asegúrate de consultar la sección de [Preguntas frecuentes](remote-desktop-client-faq.md) si te surge alguna duda.

> [!NOTE]
> - ¿Tienes curiosidad acerca de las nuevas versiones para el cliente Android? Consulta las [Novedades del cliente de Android](android-whatsnew.md).
> - El cliente de Android es compatible con dispositivos que ejecutan Android 4.1 y versiones posteriores, así como Chromebooks con ChromeOS 53 y versiones posteriores. [Aquí](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps) encontrará más información acerca de aplicaciones Android en Chrome.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obtención del cliente de Escritorio remoto y su primer uso

### <a name="download-the-remote-desktop-client-from-the-google-play-store"></a>Descarga del cliente de Escritorio remoto desde Google Play Store

A continuación, te mostramos cómo configurar el cliente de Escritorio remoto en tu dispositivo Android:

1. Descarga el cliente de Escritorio remoto de Microsoft de [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android).
2. Inicia **el cliente de Escritorio remoto** desde la lista de aplicaciones.
3. Agrega una [conexión a Escritorio remoto](#add-a-remote-desktop-connection) o [recursos remotos](#add-remote-resources). La conexión se usa para que puedas conectarte directamente a un equipo Windows y a los recursos remotos a fin de acceder a las aplicaciones y los escritorios que publica un administrador para ti.

> [!NOTE]
> Si quieres probar las nuevas características de forma anticipada, te recomendamos que descargues el cliente [Microsoft Remote Desktop Beta](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) de Google Play Store.

### <a name="add-a-remote-desktop-connection"></a>Adición de una conexión a Escritorio remoto

[Configura el equipo para que acepte conexiones remotas](remote-desktop-allow-access.md) si aún no lo has hecho.

Para crear una conexión a Escritorio remoto:

1. En Connection Center (Centro de conexión), pulsa y, después, pulsa Escritorio.
2. Escribe el nombre del equipo remoto en **Nombre de PC**. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, MyDesktop:3389 o 10.0.0.1:3389). Este es el único campo obligatorio.
3. Selecciona el **nombre de usuario** que utilizas para acceder al equipo remoto.
   - Selecciona **Escribir siempre** para que el cliente te pida las credenciales cada vez que te conectes al equipo remoto.
   - Selecciona **Agregar cuenta de usuario** para guardar una cuenta que usas con frecuencia. Así, no tendrás que escribir las credenciales cada vez que inicies sesión en esta. Consulta [Administración de cuentas de usuario](#manage-your-user-accounts) para obtener más detalles.
4. También puedes pulsar **Show additional options** (Mostrar opciones adicionales) para configurar los siguientes parámetros opcionales:
   - En **Nombre descriptivo**, puedes escribir un nombre fácil de recordar para el equipo al que te conectas. Si no especificas ningún nombre descriptivo, se mostrará el nombre del equipo en su lugar.
   - **Puerta de enlace** es la puerta de enlace del Escritorio remoto que usarás para conectarte a un equipo desde una red externa. Para más información, ponte en contacto con el administrador del sistema.
   - **Sonido** selecciona el dispositivo que usa la sesión remota para el audio. Puedes elegir reproducir el sonido en un dispositivo local, en el dispositivo remoto o no reproducirlo.
   - La opción **Customize display resolution** (Personalizar la resolución de la pantalla) establece la resolución de la sesión remota. Cuando esta opción está desactivada, se usa la resolución especificada en la configuración global.
   - La opción **Intercambiar botones del mouse** cambia los comandos enviados por los movimientos del mouse a la derecha y a la izquierda. Ideal para los usuarios zurdos.
   - La opción **Conectar a sesión del administrador** te permite conectarte a una sesión de administrador en el equipo remoto.
   - La opción **Redirect local storage** (Redirigir el almacenamiento local) permite redireccionar el almacenamiento local. Esta opción está deshabilitada de forma predeterminada.
5. Cuando termines, pulsa **Guardar**.

¿Es necesario editar esta configuración? Pulsa el menú de desbordamiento ( **...** ) junto al nombre del escritorio y, después, pulsa **Editar**.

¿Quieres quitar la conexión? Vuelve a pulsar el menú de desbordamiento ( **...** ) y, después, pulsa **Quitar**.

>[!TIP]
> Si aparece el error 0xf07, que tiene relación con una contraseña incorrecta ["We couldn't connect to the remote PC because the password associated with the user account has expired" (No ha sido posible conectarse al equipo remoto porque la contraseña asociada a la cuenta de usuario ha expirado)], cambie la contraseña y pruebe de nuevo.

### <a name="add-remote-resources"></a>Agregar recursos remotos

Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados por el administrador. El cliente de Android admite los recursos publicados desde **Servicios de Escritorio remoto** y las implementaciones de **Windows Virtual Desktop**. Para agregar recursos remotos:

1. En Connection Center (Centro de conexión), pulsa **+** y, después, **Remote Resources Feed** (Fuente de recursos remotos).
2. Escribe la **Dirección URL de fuente**. Puede ser una dirección URL o una dirección de correo electrónico:
   - La **dirección URL** es el servidor de acceso web de Escritorio remoto que te proporciona el administrador. Si tienes acceso a recursos de Windows Virtual Desktop, puedes usar `https://rdweb.wvd.microsoft.com`.
   - Si tienes previsto usar **Correo electrónico**, escribe tu dirección de correo electrónico en este campo. Esto indica al cliente que busque un servidor de acceso web de Escritorio remoto asociado a tu dirección de correo electrónico si el administrador lo ha configurado.
3. Pulsa **Siguiente**.
4. Escribe la información de inicio de sesión cuando se te pida. Esto puede variar en función de la implementación y puede incluir:
   - El **Nombre de usuario** que tiene permiso para acceder a los recursos.
   - La **Contraseña** asociada con el nombre de usuario.
   - **Additional factor** (Factor adicional), que se te puede pedir si el administrador ha configurado la autenticación de este modo.
5. Cuando termines, pulsa **Guardar**.

Los recursos remotos se mostrarán en el Centro de conexión.

Para quitar los recursos remotos:

1. En el Centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del recurso remoto.
2. Pulsa **Quitar**.
3. Confirma la eliminación.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Widgets: ancla un escritorio guardado a la pantalla de inicio

El cliente de Escritorio remoto admite el anclaje de conexiones a la pantalla de inicio mediante la característica de widgets de Android. La forma en que se agregan los widgets depende del tipo de dispositivo Android que se use y de su sistema operativo. Esta es la manera más común de agregar widgets:

1. Pulsa **Aplicaciones** para iniciar el menú de aplicaciones.
2. Pulsa **Widgets**.
3. Desliza el dedo por los widgets y busca el icono de Escritorio remoto con la descripción, "Pin Remote Desktop" (Anclar a Escritorio remoto).
4. Mantén pulsado el widget de Escritorio remoto y muévelo a la pantalla de inicio.
5. Al soltar el icono, verás los escritorios remotos guardados. Elige la conexión que quieres guardar en la pantalla de inicio.

Ya puedes iniciar la conexión a Escritorio remoto directamente desde la pantalla de inicio pulsando en ella.

> [!NOTE]
> Si cambias el nombre de la conexión de escritorio en el cliente de Escritorio remoto, la etiqueta anclada no se actualizará.

## <a name="manage-general-app-settings"></a>Administración de la configuración general de aplicaciones

Para cambiar la configuración general de la aplicación, pulsa **Configuración** y, después, pulsa **General** en Connection Center (Centro de conexión).

Puedes configurar las siguientes opciones generales:

- **Mostrar vistas previas de escritorio**, que permite obtener una vista previa de un escritorio en Connection Center (Centro de conexión) antes de conectarse a él. Esta opción está habilitada de forma predeterminada.
- **Pinch to zoom remote session** (Gesto de reducir o ampliar la sesión remota), que permite usar gestos de reducir o ampliar para aplicar zoom. Si la aplicación que estás usando a través de Escritorio remoto admite la funcionalidad multitáctil (incluida en Windows 8), desactiva esta característica.
- Habilita la opción **Use scancode input when available** (Usar entrada de código de tecla cuando esté disponible) si la aplicación remota no responde correctamente a la entrada de teclado enviada como código de tecla. La entrada se envía como Unicode cuando esta opción está deshabilitada.
- **Ayudar a mejorar Escritorio remoto**, que envía datos anónimos a Microsoft. Estos datos se usan para mejorar el cliente. Puedes obtener más información acerca de cómo se tratan estos datos privados anónimos en la [declaración de privacidad de Microsoft](https://privacy.microsoft.com/privacystatement). Esta opción está habilitada de forma predeterminada.

## <a name="manage-display-settings"></a>Administración de la configuración de pantalla

Para cambiar la configuración de pantalla, pulsa **Configuración** y, después, **Pantalla** en Connection Center (Centro de conexión).

Puedes configurar las siguientes opciones de configuración de la pantalla:

- **Orientación**, que establece la orientación preferida (horizontal o vertical) de la sesión.
  >[!NOTE]
  > Si te conectas a un equipo con Windows 8 o cualquier versión anterior de Windows, la sesión no se escalará correctamente cuando se cambie la orientación del dispositivo. La mejor opción es desconectarse del equipo y, después, volverse a conectar en la orientación que se desea utilizar. Aunque una opción mejor sería actualizar el equipo a Windows 10.

- **Resolución**, que establece la resolución remota que se quiere usar para las conexiones de escritorio globalmente. Si ya estableciste una resolución personalizada para una conexión individual, este valor no la cambiará.
  >[!NOTE]
  >Cuando cambias uno de los valores de visualización, dicho cambio solo se aplica a las conexiones que se establecen desde ese momento. Para ver el cambio en una sesión en la que ya estés conectado, desconéctate y vuelve a conectarte.

## <a name="manage-your-rd-gateways"></a>Administración de puertas de enlace de Escritorio remoto

Las puerta de enlace de Escritorio remoto te permite conectarte a un equipo remoto de una red privada desde cualquier lugar de Internet. Para crear y administrar dichas puertas de enlace, usa el cliente de Escritorio remoto.

Para configurar una puerta de enlace nueva:

1. En Connection Center (Centro de conexión), pulsa **Configuración** y, luego,  **Puertas de enlace**.
1. Pulsa **+** para agregar una nueva puerta de enlace.
1. Escribe la siguiente información:
   - Escribe el nombre del equipo que quieres usar como puerta de enlace en el campo **Nombre del servidor**. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: RDGateway:443 o 10.0.0.1:443).
   - Selecciona la **cuenta de usuario** que usarás para acceder a la puerta de enlace de Escritorio remoto.
     - Selecciona **Use desktop user account** (Usar la cuenta de usuario de escritorio) para usar las mismas credenciales que especifiques para el equipo remoto.
     - Selecciona **Agregar cuenta de usuario** para guardar una cuenta que usas con frecuencia. Así, no tendrás que escribir las credenciales cada vez que inicies sesión en esta. Sigue estas instrucciones para [administrar las cuentas de usuario](#manage-your-user-accounts).

Para eliminar una puerta de enlace:

1. En Connection Center (Centro de conexión), pulsa **Configuración** y, luego,  **Puertas de enlace**.
2. Mantén pulsada una puerta de enlace en la lista para seleccionarla. Puedes seleccionar varias puertas de enlace a la vez.
3. Pulsa la papelera para eliminar la puerta de enlace seleccionada.

## <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando te conectas a un escritorio o a recursos remotos, puedes guardar las cuentas de usuario para volver a seleccionarlas más adelante.

Para crear una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**y luego pulsa  **Cuenta de usuario**.
2. Pulsa **+** para agregar una cuenta de usuario nueva.
3. Escribe la siguiente información:
   - El **nombre de usuario** que se guarda para usarlo con una conexión remota. El nombre de usuario se puede escribir en cualquiera de los siguientes formatos: nombre_de_usuario, dominio\nombre_de_usuario o user_name@domain.com.
   - La **contraseña** del usuario que has especificado. Todas las cuentas de usuario que quieras guardar para usarlas para las conexiones remotas deben tener una contraseña asociada.
4. Cuando termines, pulsa **Guardar**.

Para eliminar una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**y luego pulsa  **Cuenta de usuario**.
2. Mantén pulsada una cuenta de usuario en la lista para seleccionarla. Puede seleccionar varios usuarios.
3. Pulsa la Papelera para eliminar el usuario seleccionado.

## <a name="navigate-the-remote-desktop-session"></a>Desplazamiento a la sesión de Escritorio remoto

Al iniciar una conexión con Escritorio remoto, hay varias herramientas disponibles que se pueden usar para desplazarse por la sesión.

### <a name="start-a-remote-desktop-connection"></a>Inicio de una conexión a Escritorio remoto

1. Pulsa en la conexión con Escritorio remoto para iniciar la sesión.
2. Si se te pide que verifiques el certificado del escritorio remoto, pulsa **Conectar**. También puedes seleccionar **No volver a preguntar las conexiones a este equipo** para aceptar siempre el certificado.

### <a name="connection-bar"></a>Barra de conexión

La barra de conexión te permite acceder a más controles de navegación. De manera predeterminada, la barra de conexión se coloca en la parte central de la parte superior de la pantalla. Arrastra la barra hacia la izquierda o derecha para moverla.

- **Pan Control** (Control de panorámica): el control de panorámica permite ampliar la pantalla y moverse alrededor de ella. El control de panorámica solo está disponible para el toque directo.
  - Para mostrar el control de panorámica, pulsa el icono de panorámica de la barra de conexión y aplica zoom a la pantalla. Vuelve a pulsar el icono de panorámica para ocultar el control y devolver la pantalla a su tamaño original.
  - Para usar el control de panorámica, mantenlo pulsado y arrástralo en la dirección que quieres mover la pantalla.
  - Para mover el control de panorámica, pulsa dos veces y mantén pulsado para mover el control por la pantalla.
- **Opciones adicionales**: pulsa el icono de opciones adicionales para mostrar la barra de selección y la barra de comandos de la sesión.
- **Teclado**: pulsa el icono del teclado para mostrar u ocultar el teclado. El control de panorámica se muestra automáticamente cuando se muestra el teclado.

### <a name="session-selection-bar"></a>Barra de selección de sesión

Puede tener varias conexiones abiertas en equipos diferentes al mismo tiempo. Pulsa la barra de conexión para mostrar la barra de selección de sesión en el lado izquierdo de la pantalla. La barra de selección de sesión te permite ver las conexiones abiertas y cambiar de una a otra.

Cuando estás conectado a recursos remotos, para cambiar entre las aplicaciones de esa sesión, pulsa el menú del expansor **>** y elige un elemento de la lista de elementos disponibles.

Para iniciar una nueva sesión desde dentro de la conexión actual, pulsa **Iniciar nueva** y, después, elige un elemento de la lista de elementos disponibles.

Para desconectar una sesión, pulsa **X** en el lado izquierdo del icono de la sesión.

### <a name="command-bar"></a>Barra de comandos

Pulsa la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla. En la barra de comandos, puedes cambiar entre los modos del mouse (toque directo y puntero del mouse) o pulsar el botón Inicio para volver a Connection Center (Centro de conexión). También puedes pulsar el botón Atrás para volver a Connection Center (Centro de conexión). Al volver a Connection Center (Centro de conexión), no se desconectará la sesión activa.

### <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso de gestos táctiles y de modos de ratón en una sesión remota

El cliente utiliza gestos táctiles estándar. También se pueden usar gestos táctiles para replicar acciones del ratón en el escritorio remoto. En la tabla siguiente se explica qué gestos coinciden con las acciones del mouse en cada modo del mouse.

> [!NOTE]
> Los gestos táctiles nativos se admiten en el modo de toque directo de Windows 8 o versiones posteriores.

| Modo de mouse    | Acción del mouse         | Gesto                                                                 |
|---------------|----------------------|-------------------------------------------------------------------------|
| Toque directo  | Clic con el botón izquierdo           | Pulsar con un dedo                                                     |
| Toque directo  | Haz clic con el botón secundario          | Mantener pulsado con un dedo y soltar después                              |
| Puntero | Zoom                 | Usar dos dedos y acercarlos para acercar o separarlos para alejar |
| Puntero | Clic con el botón izquierdo           | Pulsar con un dedo                                                     |
| Puntero | Hacer clic con el botón izquierdo y arrastrar  | Pulsar dos veces con un dedo y mantener pulsado y, a continuación, arrastrar                          |
| Puntero | Haz clic con el botón secundario          | Pulsar con dos dedos                                                    |
| Puntero | Clic con el botón derecho y arrastrar | Pulsar dos veces con dos dedos y mantener pulsado y, a continuación, arrastrar                         |
| Puntero | Rueda del ratón          | Mantener pulsado con dos dedos y, a continuación, arrastrar hacia arriba o hacia abajo.                     |
