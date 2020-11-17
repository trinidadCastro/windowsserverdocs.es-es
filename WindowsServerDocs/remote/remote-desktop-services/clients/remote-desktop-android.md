---
title: Introducción al cliente de Android
description: Información general acerca del cliente de Android.
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 09/17/2020
ms.localizationpriority: medium
ms.openlocfilehash: 1f59c00e375ab142c4e3dadc480c648cdd8e2396
ms.sourcegitcommit: 877d6db73d9520e3a23738d6528016235493cff3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90779249"
---
# <a name="get-started-with-the-android-client"></a>Introducción al cliente de Android

>Se aplica a: Android 4.1 y versiones posteriores

El cliente de Escritorio remoto para Android se puede usar con aplicaciones de Windows y escritorios directamente desde un dispositivo Android o Chromebook que sea compatible con Google Play Store.

En este artículo se muestra cómo empezar a usar el cliente. Si tiene alguna pregunta adicional, asegúrese de consultar las [preguntas más frecuentes](remote-desktop-client-faq.md).

> [!NOTE]
> - ¿Tienes curiosidad acerca de las nuevas versiones para el cliente Android? Consulta las [Novedades del cliente de Android](android-whatsnew.md).
> - El cliente de Android es compatible con dispositivos que ejecutan Android 4.1 y versiones posteriores, así como Chromebooks con ChromeOS 53 y versiones posteriores. Obtenga más información sobre las aplicaciones Android en Chrome en [Sistemas Chrome OS que admiten aplicaciones Android](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="download-the-remote-desktop-client"></a>Descarga del cliente de Escritorio remoto

A continuación, te mostramos cómo configurar el cliente de Escritorio remoto en tu dispositivo Android:

1. [Descargue el cliente de Escritorio remoto de Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) de Google Play.
2. Inicia **el cliente de Escritorio remoto** desde la lista de aplicaciones.
3. Agrega una [conexión a Escritorio remoto](#add-a-remote-desktop-connection) o [recursos remotos](#add-remote-resources). Las conexiones de Escritorio remoto permiten conectarse directamente a un PC Windows y a los recursos remotos a fin de acceder a las aplicaciones y los escritorios que publica un administrador para usted.

## <a name="add-a-remote-desktop-connection"></a>Adición de una conexión a Escritorio remoto

Ahora que tiene el cliente en el dispositivo, puede agregar conexiones a Escritorio remoto para acceder a los recursos remotos.

Antes de agregar una conexión, [configure el PC para que acepte conexiones remotas](remote-desktop-allow-access.md) si aún no lo ha hecho.

Para agregar una conexión a Escritorio remoto:

1. En Connection Center (Centro de conexión), pulsa **+** y, después, pulsa **Escritorio**.
2. Escribe el nombre del equipo remoto en **Nombre de PC**. Puede ser un nombre de PC Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, MyDesktop:3389 o 10.0.0.1:3389). Este campo es el único obligatorio.
3. Selecciona el **nombre de usuario** que utilizas para acceder al equipo remoto.

   - Selecciona **Escribir siempre** para que el cliente te pida las credenciales cada vez que te conectes al equipo remoto.
   - Selecciona **Agregar cuenta de usuario** para guardar una cuenta que usas con frecuencia. Así, no tendrás que escribir las credenciales cada vez que inicies sesión en esta. Para obtener más información acerca de las cuentas de usuario, consulte [Administración de cuentas de usuario](#manage-your-user-accounts).

4. También puedes pulsar **Show additional options** (Mostrar opciones adicionales) para configurar los siguientes parámetros opcionales:

   - En **Nombre descriptivo**, puedes escribir un nombre fácil de recordar para el equipo al que te conectas. Si no especificas ningún nombre descriptivo, se mostrará el nombre del equipo en su lugar.
   - **Puerta de enlace** es la puerta de enlace del Escritorio remoto que usarás para conectarte a un equipo desde una red externa. Para más información, ponte en contacto con el administrador del sistema.
   - **Sonido** selecciona el dispositivo que usa la sesión remota para el audio. Puedes elegir reproducir el sonido en un dispositivo local, en el dispositivo remoto o no reproducirlo.
   - La opción **Customize display resolution** (Personalizar la resolución de la pantalla) establece la resolución de la sesión remota. Cuando esta opción está desactivada, se usa la resolución especificada en la configuración global.
   - La opción **Intercambiar botones del mouse** cambia los comandos enviados por los movimientos del mouse a la derecha y a la izquierda. Ideal para los usuarios zurdos.
   - La opción **Conectar a sesión del administrador** te permite conectarte a una sesión de administrador en el equipo remoto.
   - La opción **Redirect local storage** (Redirigir el almacenamiento local) permite redireccionar el almacenamiento local. Esta opción está deshabilitada de forma predeterminada.

5. Cuando termines, pulsa **Guardar**.

¿Es necesario editar esta configuración? Pulsa en el menú **Más opciones** ( **...** ) junto al nombre del dispositivo de escritorio y, a continuación, en **Editar**.

¿Quieres quitar la conexión? Vuelve a pulsar en el menú **Más opciones** ( **...** ) y, a continuación, pulsa en **Quitar**.

>[!TIP]
> Si aparece un error "0xf07" con un mensaje similar a "No hemos podido conectar con el PC remoto porque la contraseña asociada a la cuenta de usuario ha expirado", pruebe de nuevo con una nueva contraseña.

## <a name="add-remote-resources"></a>Agregar recursos remotos

Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados por el administrador. El cliente de Android admite los recursos publicados desde **Servicios de Escritorio remoto** y las implementaciones de **Windows Virtual Desktop**.

Para agregar recursos remotos:

1. En Connection Center (Centro de conexión), pulsa **+** y, después, **Remote Resources Feed** (Fuente de recursos remotos).
2. Escribe la **Dirección URL de fuente**. Puede ser una URL o una dirección de correo electrónico:
   - La **dirección URL** es el servidor de acceso web de Escritorio remoto que te proporciona el administrador. Si accede a los recursos de Windows Virtual Desktop, puede usar una de las siguientes direcciones URL, según la versión que esté usando:
     - Si usa Windows Virtual Desktop (clásico), utilice: `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`.
     - Si usa Windows Virtual Desktop, utilice: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`.
   - Si tienes previsto usar **Correo electrónico**, escribe tu dirección de correo electrónico en este campo. Al rellenar este campo, se indica al cliente que busque un servidor de acceso web de Escritorio remoto asociado a su dirección de correo electrónico si el administrador lo ha configurado.
3. Pulsa **Siguiente**.
4. Escriba la información de inicio de sesión cuando se le pida. Las credenciales que debe usar pueden variar en función de la implementación y pueden incluir:
   - El **Nombre de usuario** que tiene permiso para acceder a los recursos.
   - La **Contraseña** asociada con el nombre de usuario.
   - **Additional factor** (Factor adicional), que se te puede pedir si el administrador ha configurado la autenticación de este modo.
5. Cuando termines, pulsa **Guardar**.

Los recursos remotos se mostrarán en el Centro de conexión.

## <a name="remove-remote-resources"></a>Eliminación de los recursos remotos

Para quitar los recursos remotos:

1. En el Centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del recurso remoto.
2. Pulsa **Quitar**.
3. Confirme que ha quitado el recurso.

## <a name="pin-a-connection-to-your-home-screen"></a>Anclaje de una conexión a la pantalla principal

El cliente de Escritorio remoto admite el uso de la característica de widgets de Android para anclar las conexiones a la pantalla de inicio. El proceso de adición de los widgets depende del tipo de dispositivo Android y de la versión del sistema operativo Android que esté usando.

Para agregar un widget:

1. Pulsa **Aplicaciones** para iniciar el menú de aplicaciones.
2. Pulsa **Widgets**.
3. Desliza el dedo por los widgets y busca el icono de Escritorio remoto con la descripción: Pin Remote Desktop (Anclar a Escritorio remoto).
4. Mantén pulsado el widget de Escritorio remoto y muévelo a la pantalla de inicio.
5. Al soltar el icono, verás los escritorios remotos guardados. Elige la conexión que quieres guardar en la pantalla de inicio.

Ya puedes iniciar la conexión a Escritorio remoto directamente desde la pantalla de inicio pulsando en ella.

> [!NOTE]
> Si cambias el nombre de la conexión de escritorio en el cliente de Escritorio remoto, la etiqueta anclada no se actualizará.

## <a name="manage-general-app-settings"></a>Administración de la configuración general de aplicaciones

Para cambiar la configuración general de la aplicación, ve al Centro de conexión, pulsa en **Configuración** y, a continuación, en **General**.

Puedes configurar las siguientes opciones generales:

- **Mostrar vistas previas de escritorio**, que permite obtener una vista previa de un escritorio en Connection Center (Centro de conexión) antes de conectarse a él. Esta opción está habilitada de forma predeterminada.
- **Pinch to zoom remote session** (Gesto de reducir o ampliar la sesión remota), que permite usar gestos de reducir o ampliar para aplicar zoom. Si la aplicación que estás usando a través de Escritorio remoto admite la funcionalidad multitáctil (incluida en Windows 8), desactiva esta característica.
- Habilita la opción **Use scancode input when available** (Usar entrada de código de tecla cuando esté disponible) si la aplicación remota no responde correctamente a la entrada de teclado enviada como código de tecla. La entrada se envía como Unicode cuando esta opción está deshabilitada.
- **Ayudar a mejorar Escritorio remoto** envía datos anónimos sobre cómo usas el Escritorio remoto para Android a Microsoft. Estos datos se usan para mejorar el cliente. Para obtener más información sobre nuestra directiva de privacidad y los tipos de datos que recopilamos, consulta la [Declaración de privacidad de Microsoft](https://privacy.microsoft.com/privacystatement). Esta opción está habilitada de forma predeterminada.

## <a name="manage-display-settings"></a>Administración de la configuración de pantalla

Para cambiar la configuración de pantalla, pulsa **Configuración** y, después, **Pantalla** en Connection Center (Centro de conexión).

Puedes configurar las siguientes opciones de configuración de la pantalla:

- **Orientación**, que establece la orientación preferida (horizontal o vertical) de la sesión.

  >[!NOTE]
  > Si te conectas a un equipo con Windows 8 o anterior, la sesión no se escalará correctamente cuando se cambie la orientación del dispositivo. Para facilitar el correcto escalado del cliente, desconéctate  y, después, vuelve a conectarte con la orientación que quieras usar. También puedes garantizar el escalado correcto mediante un equipo con Windows 10.

- **Resolución**, que establece la resolución remota que se quiere usar para las conexiones de escritorio globalmente. Si ya estableciste una resolución personalizada para una conexión individual, este valor no la cambiará.

  >[!NOTE]
  >Al cambiar la configuración de pantalla, los cambios solo se aplican a las nuevas conexiones que realices después de cambiar la configuración. Para aplicar los cambios a la sesión a la que estás conectado actualmente, desconéctate y vuelve a conectarte para actualizar la sesión.

## <a name="manage-your-rd-gateways"></a>Administración de puertas de enlace de Escritorio remoto

Las puerta de enlace de Escritorio remoto te permite conectarte a un equipo remoto de una red privada desde cualquier lugar de Internet. Para crear y administrar dichas puertas de enlace, usa el cliente de Escritorio remoto.

Para configurar una nueva puerta de enlace de Escritorio remoto:

1. En Connection Center (Centro de conexión), pulsa **Configuración** y, luego,  **Puertas de enlace**.
2. Pulsa **+** para agregar una nueva puerta de enlace.
3. Escribe la siguiente información:
   - Escribe el nombre del equipo que quieres usar como puerta de enlace en el campo **Nombre del servidor**. Puede ser un nombre de PC Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: RDGateway:443 o 10.0.0.1:443).
   - Selecciona la **cuenta de usuario** que usarás para acceder a la puerta de enlace de Escritorio remoto.
     - Selecciona **Use desktop user account** (Usar la cuenta de usuario de escritorio) para usar las mismas credenciales que especifiques para el equipo remoto.
     - Selecciona **Agregar cuenta de usuario** para guardar una cuenta que usas con frecuencia. Así, no tendrás que escribir las credenciales cada vez que inicies sesión en esta. Para obtener más información, consulta [Administración de cuentas de usuario](#manage-your-user-accounts).

Para eliminar una puerta de enlace de Escritorio remoto:

1. En Connection Center (Centro de conexión), pulsa **Configuración** y, luego,  **Puertas de enlace**.
2. Mantén pulsada una puerta de enlace en la lista para seleccionarla. Puedes seleccionar varias puertas de enlace a la vez.
3. Pulsa la papelera para eliminar la puerta de enlace seleccionada.

## <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Puedes guardar las cuentas de usuario para usarlas cuando te conectes a un escritorio remoto o a recursos remotos.

Para guardar una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración** y luego pulsa  **Cuenta de usuario**.
2. Pulsa **+** para agregar una cuenta de usuario nueva.
3. Escribe la siguiente información:
   - El **nombre de usuario** que se guarda para usarlo con una conexión remota. El nombre de usuario se puede escribir en cualquiera de los siguientes formatos: nombre_de_usuario, dominio\nombre_de_usuario o user_name@domain.com.
   - La **contraseña** del usuario que has especificado. Todas las cuentas de usuario que quieras guardar para usarlas para las conexiones remotas deben tener una contraseña asociada.
4. Cuando termines, pulsa **Guardar**.

Para eliminar una cuenta de usuario guardada:

1. En el Centro de conexión, pulsa **Configuración** y luego pulsa  **Cuenta de usuario**.
2. Mantén pulsada una cuenta de usuario en la lista para seleccionarla. Puedes seleccionar varios usuarios a la vez.
3. Pulsa la Papelera para eliminar el usuario seleccionado.

## <a name="start-a-remote-desktop-connection"></a>Inicio de una conexión a Escritorio remoto

Ahora que ha configurado el cliente de Escritorio remoto de Android, se explicará cómo iniciar una sesión de Escritorio remoto.

Para iniciar una sesión:

1. Pulsa en **el nombre de la conexión de Escritorio remoto** para iniciar la sesión.
2. Si se te pide que verifiques el certificado del escritorio remoto, pulsa **Conectar**. También puedes seleccionar **No volver a preguntarme sobre conexiones a este equipo** para aceptar siempre el certificado de forma predeterminada.

## <a name="use-the-connection-bar"></a>Uso de la barra de conexión

La barra de conexión te permite acceder a más controles de navegación. De manera predeterminada, la barra de conexión se coloca en la parte central de la parte superior de la pantalla. Arrastra la barra hacia la izquierda o derecha para moverla.

- **Pan Control** (Control de panorámica): el control de panorámica permite ampliar la pantalla y moverse alrededor de ella. El control de panorámica solo está disponible para el toque directo.
  - Para mostrar el control de panorámica, pulsa el icono de panorámica de la barra de conexión y aplica zoom a la pantalla. Vuelve a pulsar el icono de panorámica para ocultar el control y devolver la pantalla a su tamaño original.
  - Para usar el control de panorámica, mantenlo pulsado y arrástralo en la dirección que quieres mover la pantalla.
  - Para mover el control de panorámica, pulsa dos veces y mantén pulsado para mover el control por la pantalla.
- **Opciones adicionales**: pulsa el icono de opciones adicionales para mostrar la barra de selección y la barra de comandos de la sesión.
- **Teclado**: pulsa el icono del teclado para mostrar u ocultar el teclado. El control de panorámica se muestra automáticamente cuando se muestra el teclado.

## <a name="use-the-session-selection-bar"></a>Uso de la barra de selección de sesión

Puede tener varias conexiones abiertas en equipos diferentes al mismo tiempo. Pulse la barra de conexión para mostrar la barra de selección de sesión en el lado izquierdo de la pantalla. La barra de selección de sesión te permite ver las conexiones abiertas y cambiar de una a otra.

Cuando estás conectado a recursos remotos, para cambiar entre las aplicaciones de esa sesión, pulsa el menú del expansor ( **>** ) y elige un elemento de la lista de elementos disponibles.

Para iniciar una nueva sesión desde dentro de la conexión actual, pulsa **Iniciar nueva** y, después, elige un elemento de la lista de elementos disponibles.

Para desconectar una sesión, pulse **X** en el lado izquierdo del icono de la sesión.

## <a name="use-the-command-bar"></a>Uso de la barra de comandos

Pulse la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla. En la barra de comandos, puedes cambiar entre los modos del mouse (toque directo y puntero del mouse) o pulsar el botón Inicio para volver a Connection Center (Centro de conexión). También puedes pulsar el botón Atrás para volver a Connection Center (Centro de conexión). Al volver a Connection Center (Centro de conexión), no se desconectará la sesión activa.

## <a name="touch-gestures-and-mouse-modes"></a>Gestos de toque y modos de mouse

El cliente de Escritorio remoto para Android usa gestos táctiles estándar. También se pueden usar gestos táctiles para replicar acciones del ratón en el escritorio remoto. En la tabla siguiente se explica qué gestos coinciden con las acciones del mouse en cada modo del mouse.

> [!NOTE]
> Los gestos táctiles nativos se admiten en el modo de toque directo de Windows 8 o versiones posteriores.

| Modo del mouse    | Acción del mouse         | Gesto                                                                 |
|---------------|----------------------|-------------------------------------------------------------------------|
| Toque directo  | Clic con el botón izquierdo           | Pulsar con un dedo                                                     |
| Toque directo  | Haga clic con el botón secundario en          | Mantener pulsado con un dedo y soltar después                              |
| Puntero | Zoom                 | Usa dos dedos y acércalos para alejar o sepáralos para acercar. |
| Puntero | Clic con el botón izquierdo           | Pulsar con un dedo                                                     |
| Puntero | Hacer clic con el botón izquierdo y arrastrar  | Pulsar dos veces con un dedo y mantener pulsado y, a continuación, arrastrar                          |
| Puntero | Haga clic con el botón secundario en          | Pulsar con dos dedos                                                    |
| Puntero | Clic con el botón derecho y arrastrar | Pulsar dos veces con dos dedos y mantener pulsado y, a continuación, arrastrar                         |
| Puntero | Rueda del ratón          | Mantener pulsado con dos dedos y, a continuación, arrastrar hacia arriba o hacia abajo.                     |

## <a name="join-the-beta-channel"></a>Unión al canal beta

Si quiere ayudarnos a probar nuevas compilaciones o a buscar problemas en las próximas actualizaciones de versión antes de que se publiquen, debería unirse a nuestro canal beta. Los administradores de la organización pueden usar el canal beta para validar las nuevas versiones del cliente de Android para sus usuarios.

Para unirse a la beta, [descargue el cliente de la beta](https://play.google.com/apps/testing/com.microsoft.rdc.androidx) y dé su consentimiento para acceder a las versiones preliminares y descargar el cliente. Recibirás versiones preliminares directamente a través de Google Play Store.
