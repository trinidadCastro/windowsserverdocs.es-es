---
title: Introducción al cliente de iOS
description: Aprenda a configurar el cliente de Escritorio remoto para iOS
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: Heidilohr
manager: lizross
ms.author: helohr
ms.date: 07/16/2020
ms.localizationpriority: medium
ms.openlocfilehash: 723fa40e1c2d446381b333eee1289a25adefd5d8
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997369"
---
# <a name="get-started-with-the-ios-client"></a>Introducción al cliente de iOS

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2

El cliente Escritorio remoto para iOS se puede usar para trabajar con aplicaciones, recursos y equipos de escritorio de Windows desde un dispositivo iOS (iPhone e iPad).

Usa la siguiente información para comenzar. Asegúrate de consultar la sección de [Preguntas frecuentes](remote-desktop-client-faq.md) si te surge alguna duda.

> [!NOTE]
> - ¿Tienes curiosidad acerca de las nuevas versiones para el cliente iOS? Consulte [Novedades de Escritorio remoto en iOS](ios-whatsnew.md).
> - El cliente iOS admite dispositivos que usen iOS 6.x, y cualquier versión posterior.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obtención del cliente de Escritorio remoto y su primer uso

En esta sección se explica cómo descargar y configurar el cliente de Escritorio remoto para iOS.

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Descarga del cliente de Escritorio remoto de la tienda iOS

En primer lugar, deberá descargar el cliente y configurar el equipo para que se conecte a los recursos remotos.

Para descargar el cliente, haga lo siguiente:

1. Descarga el cliente de Escritorio remoto de Microsoft desde [iOS App Store](https://aka.ms/rdios) o [iTunes](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configura tu equipo para que acepte conexiones remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).

### <a name="add-a-pc"></a>Adición de un equipo

Una vez que haya descargado el cliente y configurado el equipo para que acepte conexiones remotas, deberá agregar un equipo.

Para agregar un equipo:

1. En Connection Center (Centro de conexión), pulse **+** y, luego, **Add PC** (Agregar PC).
2. Escribe la siguiente información:
   - **Nombre del equipo**: el nombre del equipo. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nombre de usuario**: el nombre de usuario que utilizará para acceder al equipo remoto. Puedes usar los siguientes formatos: *nombre_de_usuario*, *dominio\nombre_de_usuario* o `user_name@domain.com`. También puedes elegir **Ask when required** (Preguntar cuando sea necesario) para que pida un nombre de usuario y una contraseña cuando sea necesario.
3. También puedes establecer las siguientes opciones adicionales:
   - **Nombre descriptivo (opcional)** : un nombre fácil de recordar para el equipo al que se conecta. Puedes utilizar cualquier cadena, pero si no especificas un nombre descriptivo, se muestra el nombre del equipo en su lugar.
   - **Puerta de enlace (opcional)** : la puerta de enlace de Escritorio remoto que deseas usar para conectarte a escritorios virtuales, programas RemoteApp y escritorios basados en sesión de una red corporativa interna. La información acerca de la puerta de enlace te la puede proporcionar el administrador del sistema.
   - **Sonido**: selecciona el dispositivo que se usa para audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, en el dispositivo remoto o no reproducirlo.
   - **Intercambiar botones del ratón**: cada vez que un gesto del ratón enviaría un comando con el botón izquierdo, envía el mismo comando con el botón derecho. Dicho intercambio es necesario si el equipo remoto está configurado para el modo de mouse para zurdos.
   - **Modo de administrador**: conéctate a una sesión de administración en un servidor que usa Windows Server 2003 o posterior.
   - **Portapapeles**: elige si quieres redirigir el texto y las imágenes del portapapeles a tu equipo.
   - **Almacenamiento**: elige si quieres redirigir el almacenamiento a tu equipo.
4. Pulsa **Guardar**.

¿Es necesario editar esta configuración? Mantenga presionado el escritorio que quiera editar y, a continuación, pulse el icono de configuración.

### <a name="add-a-workspace"></a>Adición de un área de trabajo

Para obtener una lista de recursos administrados a los que puedes acceder en iOS, agrega un área de trabajo; para ello, suscríbete a la fuente proporcionada por el administrador.

Para agregar un área de trabajo:

1. En el Centro de conexión, pulsa **+** y, después, pulsa **Agregar área de trabajo**.
2. En el campo Dirección URL de la fuente, escribe la dirección URL de la fuente que quieres agregar. Puede ser una URL o una dirección de correo electrónico.
   - Si eliges una dirección URL, usa la que te proporcionó el administrador.
      - Esta dirección URL suele ser una URL de Windows Virtual Desktop. La que use dependerá de la versión de Windows Virtual Desktop que esté usando.
        - En el caso de la versión de otoño de 2019, use `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`.
        - En el caso de la versión de primavera de 2020, use `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`.
   - Si usas una dirección de correo electrónico, escribe tu dirección de correo electrónico. Al escribirla, indicará al cliente que busque una dirección URL asociada a su dirección de correo electrónico si el administrador ha configurado así el servidor.
3. Pulsa **Siguiente**.
4. Proporciona las credenciales cuando se te pidan.
   - En **Nombre de usuario**, indica el nombre de usuario de una cuenta con permiso de acceso a los recursos.
   - En **Contraseña**, indica la contraseña de la cuenta.
   - También es posible que se te pida información adicional en función de cómo el administrador haya configurado la autenticación.
5. Pulsa **Guardar**.

Una vez finalice, en Connection Center (Centro de conexión) se mostrarán los recursos remotos.

Después de suscribirte a una fuente, el contenido de la fuente se actualizará automáticamente de forma periódica. Se pueden agregar, cambiar o quitar recursos en función de los cambios que realice el administrador.

## <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando te conectas a un equipo o un área de trabajo, puedes guardar las cuentas de usuario para volver a seleccionarlas más adelante.

Para crear una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**y luego pulsa **Cuentas de usuario**.
2. Pulsa **Agregar cuenta de usuario**.
3. Escribe la siguiente información:
   - **Nombre de usuario**: el nombre del usuario que se guarda para usarlo con una conexión remota. Puede escribir el nombre de usuario en cualquiera de los siguientes formatos: `user_name`, `domain\user_name` o `user_name@domain.com`.
   - **Contraseña**: la contraseña del usuario que has especificado.
4. Pulsa **Guardar**.

Para eliminar una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**y luego pulsa **Cuentas de usuario**.
2. Selecciona la cuenta que quieres eliminar.
3. Pulsa **Eliminar**.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conexión a una puerta de enlace de Escritorio remoto para acceder a recursos internos

Las puertas de enlace de Escritorio remoto te permiten conectarte a equipos remotos de una red corporativa desde cualquier lugar de Internet. Para crear y administrar dichas puertas de enlace, usa el cliente de Escritorio remoto.

Para configurar una puerta de enlace nueva:

1. En el Centro de conexión, pulsa **Configuración** > **Puerta de enlace**.
2. Pulsa **Agregar puerta de enlace**.
3. Escribe la siguiente información:
   - **Nombre de puerta de enlace**: el nombre del equipo que quieres usar como puerta de enlace. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Nombre de usuario**: el nombre de usuario y la contraseña que se van a usar para la puerta de enlace de Escritorio remoto a la que se va a conectar. También puede seleccionar **Use connection credentials** (Usar credenciales de la conexión) para utilizar el mismo nombre de usuario y la misma contraseña que para la Conexión a Escritorio remoto.

## <a name="navigate-the-remote-desktop-session"></a>Desplazamiento a la sesión de Escritorio remoto

En esta sección se describen las herramientas que puede usar para navegar por la sesión de Escritorio remoto.

### <a name="start-a-remote-desktop-connection"></a>Inicio de una conexión a Escritorio remoto

1. Pulsa en la conexión con Escritorio remoto para iniciar la sesión de escritorio remota.
2. Si se le pide que verifique el certificado de Escritorio remoto, pulse **Aceptar**. Para aceptarlo de forma predeterminada, establezca la opción **No volver a preguntarme sobre conexiones a este equipo** en **Activado**.

### <a name="connection-bar"></a>Barra de conexión

La barra de conexión te permite acceder a más controles de navegación.

- **Pan Control** (Control de panorámica): el control de panorámica permite ampliar la pantalla y moverse alrededor de ella. Solo está disponible si se usa el toque directo.
   - Para habilitarlo o deshabilitarlo, pulse el icono de panorámica de la barra de conexión a fin de mostrar dicho control. La pantalla se acercará mientras el control de panorámica esté activo. Vuelva a pulsar el icono de panorámica en la barra de conexión para ocultar el control y devolver la pantalla a su resolución original.
   - Para usar el control de panorámica, manténgalo presionado. Mientras lo mantiene presionado, arrastre los dedos en la dirección en la que quiera mover la pantalla.
   - Para mover el control de panorámica, púlselo dos veces y manténgalo pulsado a fin de mover el control en la pantalla.
- **Nombre de la conexión**: se muestra el nombre de la conexión actual. Pulsa el nombre de conexión para mostrar la barra de selección de la sesión.
- **Teclado**: pulsa el icono del teclado para mostrar u ocultar el teclado. El control de panorámica se muestra automáticamente cuando se muestra el teclado.
- **Mover la barra de conexión**: Mantenga pulsada la barra de conexión. Mientras la mantiene pulsada, arrástrela a su nueva ubicación. Suéltela para colocarla en la nueva ubicación.

### <a name="session-selection"></a>Selección de la sesión

Puede tener varias conexiones abiertas en equipos diferentes al mismo tiempo. Pulsa la barra de conexión para mostrar la barra de selección de sesión en el lado izquierdo de la pantalla. La barra de selección de sesión permite ver las conexiones abiertas y cambiar de una a otra.

A continuación, le indicamos lo que puede hacer con la barra de selección de la sesión:

- Para cambiar entre aplicaciones en una sesión de recursos remota abierta, pulse el menú expansor y elija una aplicación de la lista.
- Pulse **Iniciar nueva** para iniciar una sesión nueva y, a continuación, elija una sesión de la lista de sesiones disponibles.
- Pulse el icono **X** en el lado izquierdo del icono de la sesión para desconectarse de la sesión.

### <a name="command-bar"></a>Barra de comandos

La barra de comandos ha reemplazado a la barra de utilidad, que comenzó en la versión 8.0.1. Puede usar la barra de comandos para cambiar entre modos del mouse y volver al centro de conexión.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso de gestos táctiles y de modos de ratón en una sesión remota

El cliente utiliza gestos táctiles estándar. También se pueden usar gestos táctiles para replicar acciones del ratón en el escritorio remoto. Los modos del ratón disponibles se definen en la tabla siguiente.

> [!NOTE]
> En Windows 8 o las versiones posteriores, los gestos táctiles nativos se admiten en el modo de toque directo. Para obtener más información sobre los gestos de Windows 8, consulte[Entrada táctil: deslizar el dedo, pulsar y mucho más](https://windows.microsoft.com/windows-8/touch-swipe-tap-beyond).

| Modo del mouse    | Operación del ratón      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Toque directo  | Clic con el botón izquierdo           | Pulsar con un dedo                                               |
| Toque directo  | Haga clic con el botón secundario en          | Mantener pulsado con un dedo                                      |
| Puntero | Clic con el botón izquierdo           | Pulsar con un dedo                                               |
| Puntero | Hacer clic con el botón izquierdo y arrastrar  | Mantener pulsado con un dedo y, a continuación, arrastrar                   |
| Puntero | Haga clic con el botón secundario en          | Pulsar con dos dedos                                              |
| Puntero | Clic con el botón derecho y arrastrar | Pulsar dos veces con dos dedos y mantener pulsado y, a continuación, arrastrar                    |
| Puntero | Rueda del ratón          | Pulsar dos veces y mantener pulsado con dos dedos y, a continuación, arrastrar hacia arriba o hacia abajo                |
| Puntero | Zoom                 | Con dos dedos, acérquelos para alejar el zoom y sepárelos para acercar. |

## <a name="supported-input-devices"></a>Dispositivos de entrada compatibles

El cliente ofrece [compatibilidad con el mouse Bluetooth](https://support.apple.com/HT210546) en iOS 13 e iPadOS como característica de accesibilidad. Puede usar los dispositivos mouse Swiftpoint GT o ProPoint para integrar el mouse de una forma más completa. Además, el cliente admite los teclados externos que son compatibles con iOS e iPadOS.

Para obtener más información sobre la compatibilidad con dispositivos, consulta [Novedades del cliente de iOS](ios-whatsnew.md) e [iOS App Store](https://aka.ms/rdios).

> [!TIP]
> Swiftpoint ofrece un [descuento exclusivo en el ratón ProPoint](https://www.swiftpoint.com/microsoft) para los usuarios del cliente iOS.

## <a name="use-a-keyboard-in-a-remote-session"></a>Uso de un teclado en una sesión remota

En la sesión remota puede usar tanto un teclado en pantalla como un teclado físico.

En los teclados en pantalla, use el botón del borde derecho de la barra que hay encima del teclado para cambiar entre el teclado estándar y el adicional.

Si el Bluetooth está habilitado en el dispositivo iOS, el cliente detecta automáticamente el teclado Bluetooth.

Aunque es posible que algunas combinaciones de teclas no funcionen como se esperaba en una sesión remota, muchas de las combinaciones de teclas comunes de Windows, como CTRL+C, CTRL+V y ALT+TAB funcionarán.

> [!TIP]
> Tanto las preguntas como los comentarios son siempre bienvenidos. Sin embargo, si publica solicitudes de soporte técnico o comentarios sobre el producto en la sección Comentarios de este artículo, no podremos responder a sus comentarios. Si necesita ayuda o quiere solucionar problemas con su cliente, le recomendamos que vaya al [foro de clientes de Escritorio remoto](/answers/topics/windows-remote-desktop-client.html) e inicie una nueva conversación. Si tiene alguna sugerencia de características, puede indicárnosla mediante el [foro de clientes de UserVoice](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).