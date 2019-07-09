---
title: Introducción al cliente de Escritorio remoto en Android
description: Los pasos básicos para la configuración del cliente de Escritorio remoto para Android.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b4b188eb8148b2f4e5c6672b07884af8fdcd0c60
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446742"
---
# <a name="get-started-with-remote-desktop-on-android"></a>Introducción al cliente de Escritorio remoto en Android

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2

El cliente de Escritorio remoto para Android se puede usar con aplicaciones de Windows y escritorios directamente desde un dispositivo Android.

Usa la siguiente información para comenzar. Asegúrate de consultar la sección de [Preguntas frecuentes](remote-desktop-client-faq.md) si te surge alguna duda.

> [!NOTE]
> - ¿Tienes curiosidad acerca de las nuevas versiones para el cliente Android? Consulta [Novedades de Escritorio remoto en Android](android-whatsnew.md)
> El cliente se puede ejecutar en Android 4.1, y dispositivos más recientes, así como en Chromebooks con ChromeOS 53 instalado. [Aquí](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps) encontrará más información acerca de aplicaciones Android en Chrome.

## <a name="get-the-rd-client-and-start-using-it"></a>Obtención del cliente de Escritorio remoto y primer uso

Sigue estos pasos para empezar a usar Escritorio remoto en un dispositivo Android:

1. Descarga del cliente de Escritorio remoto de [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configura el equipo para que acepte conexiones remotas](remote-desktop-allow-access.md).
3. Agrega una conexión a Escritorio remoto o un recurso remoto. Usa una conexión para conectarse directamente a un equipo Windows y un recurso remoto para usar un programa de RemoteApp, un escritorio basado en sesión o un escritorio virtual publicado de forma local. 
4. Crea un widget para poder acceder a Escritorio remoto rápidamente.

> [!NOTE]
> Si deseas probar las nuevas características con anterioridad, es aconsejable que descargue la aplicación [Microsoft Remote Desktop Beta](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) de Google Play Store. 

### <a name="add-a-remote-desktop-connection"></a>Adición de una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En el Centro de conexión, pulsa **+** y, después, pulsa **Escritorio**.
2. Escribe la siguiente información del equipo al que quieres conectarte:
   - **Nombre del equipo**: el nombre del equipo. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nombre de usuario**: el nombre de usuario que se utiliza para acceder al equipo remoto. Puedes usar los siguientes formatos: *nombre_de_usuario*, *dominio\nombre_de_usuario* o <em>user_name@domain.com</em>. También puedes especificar si deseas que se soliciten el nombre de usuario y la contraseña.
3. También puedes establecer las siguientes opciones adicionales:
   - **Nombre descriptivo**: un nombre fácil de recordar para el equipo al que se conecta. Puedes utilizar cualquier cadena, pero si no especificas un nombre descriptivo, se muestra el nombre del equipo.
   - **Puerta de enlace**: la puerta de enlace de Escritorio remoto que quieres usar para conectarte a escritorios virtuales, programas RemoteApp y escritorios basados en sesión de una red corporativa interna. La información acerca de la puerta de enlace te la puede proporcionar el administrador del sistema.
    ¿Necesita configurar una puerta de enlace de Escritorio remoto?
   - **Sonido**: selecciona el dispositivo que se usa para audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, en el dispositivo remoto o no reproducirlo.
   - **Personalizar la resolución de pantalla**: establezca una resolución personalizada para una conexión mediante la habilitación de este valor. Cuando fuera de la resolución se aplica lo que ha definido en la configuración global de la aplicación.
   - **Intercambiar botones del mouse**: esta opción se usa para cambiar las funciones del botón izquierdo del ratón al botón derecho. (es especialmente útil si el equipo remoto está configurado para usuarios zurdos, pero usa un ratón para diestros).
   - **Conectar a sesión del administrador**: esta opción se utiliza para conectarse a una sesión de consola para administrar un servidor de Windows.
   - **Redirigir a almacenamiento local**: monta el almacenamiento local como un sistema de archivos remoto en el equipo remoto.
4. Pulsa **Guardar**.

¿Es necesario editar esta configuración? Pulsa el menú de desbordamiento ( **...** ) junto al nombre del escritorio y, después, pulsa **Editar**.

¿Quieres eliminar la conexión? Vuelve a pulsar el menú de desbordamiento ( **...** ) y, después, pulsa **Quitar**.

>[!TIP]
> Si aparece el error 0xf07, que tiene relación con una contraseña incorrecta ["We couldn't connect to the remote PC because the password associated with the user account has expired" (No ha sido posible conectarse al equipo remoto porque la contraseña asociada a la cuenta de usuario ha expirado)], cambie la contraseña y pruebe de nuevo.

### <a name="add-a-remote-resource"></a>Adición de recursos remotos
Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados mediante Conexión de RemoteApp y Escritorio.

Para agregar un recurso remoto:

1. En la pantalla Centro de conexión, pulsa **+** y, después, pulsa **Remote Resources Feed** (Fuente de recursos remotos). 
2. Escribe información para el recurso remoto:
   - **Email or URL** (Correo electrónico o URL): la dirección URL del servidor de Acceso web a Escritorio remoto. En este campo también puedes escribir la cuenta de correo electrónico corporativa (esto indica al cliente que busque el servidor de Acceso web a Escritorio remoto asociado con la dirección de correo electrónico).
   - **Nombre de usuario**: el nombre de usuario que se usa para servidor de Acceso web a Escritorio remoto al que se conecta.
   - **Contraseña**la contraseña que se usa para servidor de Acceso web a Escritorio remoto al que se conecta.
3. Pulsa **Guardar**.

Los recursos remotos se mostrarán en el Centro de conexión.


Para eliminar los recursos remotos:

1. En el Centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del recurso remoto.
2. Pulsa **Quitar**.
3. Confirma la eliminación.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Widgets: ancla un escritorio guardado a la pantalla de inicio

Las aplicaciones de Escritorio remoto admiten el anclaje de conexiones a la pantalla de inicio mediante la característica de widgets de Android. La forma en que se agregan los widgets depende del tipo de dispositivo Android que se use y de su sistema operativo. Esta es la manera más común de agregar widgets: 

1. Pulsa **aplicaciones** para iniciar el menú de aplicaciones.
2. Pulsa **widgets**.
3. Desliza el dedo por los widgets y busca el icono de Escritorio remoto con la descripción, "Pin Remote Desktop" (Anclar a Escritorio remoto).
4. Mantén pulsado el widget de Escritorio remoto y muévelo a la pantalla de inicio.
5. Al soltar el icono, verás los escritorios remotos guardados. Elige la conexión que quieres guardar en la pantalla de inicio.

Ya puedes iniciar la conexión a Escritorio remoto directamente desde la pantalla de inicio pulsando en ella.

> [!NOTE]
> Si cambias el nombre de la conexión de escritorio en la aplicación Escritorio remoto, la etiqueta de este escritorio remoto anclado no se actualizará.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conexión a una puerta de enlace de Escritorio remoto para acceder a recursos internos

Las puertas de enlace de Escritorio remoto te permiten conectarte a equipos remotos de una red corporativa desde cualquier lugar de Internet. Para crear y administrar dichas puertas de enlace, usa el cliente de Escritorio remoto.

Para configurar una puerta de enlace nueva:

1. En el Centro de conexión, pulsa **Configuración > Puerta de enlace**. Pulsa **+** para agregar una nueva puerta de enlace.
2. Escribe la siguiente información:
   - **Nombre del servidor**: el nombre del equipo que quieres usar como puerta de enlace. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Nombre de usuario**: el nombre de usuario y la contraseña que se van a usar para la puerta de enlace de Escritorio remoto a la que se va a conectar. También puedes seleccionar **Usar la cuenta de usuario de escritorio** para usar las mismas credenciales que se utilizaron para la conexión a Escritorio remoto.

## <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando te conectas a un escritorio o a recursos remotos, puedes guardar las cuentas de usuario para volver a seleccionarlas más adelante. Las cuentas de usuario también se pueden definir en el propio cliente, en lugar de guardar los datos del usuario al conectarse a un escritorio.

Para crear una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**y luego pulsa  **Cuenta de usuario**.
2. Pulsa **+** para agregar una cuenta de usuario nueva.
3. Escribe la siguiente información:
   - **Nombre de usuario**: el nombre del usuario que se guarda para usarlo con una conexión remota. El nombre de usuario se puede escribir en cualquiera de los siguientes formatos: nombre_de_usuario, dominio\nombre_de_usuario user_name@domain.com.
   - **Contraseña**: la contraseña del usuario que has especificado. Todas las cuentas de usuario que quieras guardar para usarlas para las conexiones remotas deben tener una contraseña asociada.
4. Pulsa **Guardar**.

Para eliminar una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración > Cuenta de usuario**.
2. Mantén pulsada una cuenta de usuario en la lista para seleccionarla. Puede seleccionar varios usuarios.
3. Pulsa la Papelera para eliminar el usuario seleccionado.

## <a name="navigate-the-remote-desktop-session"></a>Desplazamiento a la sesión de Escritorio remoto
Al iniciar una conexión con Escritorio remoto, hay varias herramientas disponibles que se pueden usar para desplazarse por la sesión.

### <a name="start-a-remote-desktop-connection"></a>Inicio de una conexión a Escritorio remoto

1. Pulsa en la conexión con Escritorio remoto para iniciar la sesión. 
2. Si se te pide que compruebes el certificado del escritorio remoto, pulsa **Conectar**. También puedes seleccionar **No volver a preguntar las conexiones a este equipo** para aceptar siempre el certificado.

### <a name="manage-global-app-settings"></a>Administración de la configuración global de aplicaciones

Para establecer la siguiente configuración global en el cliente Android:

- **Mostrar vistas previas de escritorio**: permite obtener una vista previa de un escritorio en el Centro de conexión antes de conectarse a él. De manera predeterminada, se establece en **Activado**.
- **Acercar los dedos para aplicar zoom**: le permite usar gestos de acercar los dedos para aplicar zoom. Si la aplicación que usa a través de Escritorio remoto admite un uso multitáctil (algo que se ha introducido en Windows 8), cambie este valor a**Desactivado**.
- **Ayudar a mejorar Escritorio remoto**: envía datos anónimos a Microsoft. Estos datos se usan para mejorar el cliente. Puede obtener más información acerca de cómo se tratan estos datos privados anónimos, consulte la [declaración de privacidad del cliente de Escritorio remoto](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). De manera predeterminada, este valor está en la posición **Activado**.
- **Mostrar**: hay dos configuraciones globales para la presentación:
  - **Orientación**: establece la orientación preferida (horizontal o vertical) de la sesión. 
    >[!NOTE]
    > Si se conecta a un equipo en que use Windows 8 o cualquier versión anterior de Windows, la sesión no se escalara correctamente. La mejor opción es desconectarse del equipo y, después, volverse a conectar en la orientación que se desea utilizar. Aunque una opción mejor aún consiste en actualizar el equipo a, al menos, Windows 8.1.

  - **Resolución**: establece la resolución que se desea utilizar para las conexiones de escritorio globalmente. Si ya has establecido una resolución personalizada para una aplicación individual o una conexión, este valor no cambia.
    >[!NOTE]
    >Cuando cambias uno de los valores de visualización, dicho cambio solo se aplica a las conexiones que se establecen desde ese momento. Para ver el cambio en una sesión en la que ya estés conectado, desconéctate y vuelve a conectarte.

### <a name="connection-bar"></a>Barra de conexión

La barra de conexión te permite acceder a más controles de navegación. De manera predeterminada, la barra de conexión se coloca en la parte central de la parte superior de la pantalla. Pulsa dos veces la barra y arrástrala hacia la izquierda o derecha para moverla.

- **Pan Control** (Control de panorámica): el control de panorámica permite ampliar la pantalla y moverse alrededor de ella. Ten en cuenta que el control de panorámica solo está disponible si se usa el toque directo.
   - Habilitar o deshabilitar el control de panorámica: pulsa el icono de panorámica de la barra de conexión para mostrar el control de panorámica y aplicar zoom a la pantalla. Vuelve a pulsar el icono de panorámica en la barra de conexión nuevo para ocultar el control y devolver la pantalla a su resolución original.
   - Usar el control de panorámica: mantén pulsado el control de panorámica y arrástralo en la dirección que deseas mover la pantalla.
   - Mover el control de panorámica: pulsa dos veces y mantén pulsado el control de panorámica para mover el control en la pantalla.
- **Opciones adicionales**: pulsa el icono de opciones adicionales para mostrar la barra de selección y la barra de comandos de la sesión (ver abajo).
- **Teclado**: pulsa el icono del teclado para mostrar u ocultar el teclado. El control de panorámica se muestra automáticamente cuando se muestra el teclado.
- **Mover la barra de conexión**: Mantén pulsada la barra de conexión, arrástrala y colócala en una nueva ubicación en la parte superior de la pantalla.


### <a name="command-bar"></a>Barra de comandos

Pulsa la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla. Puedes cambiar entre los modos de ratón (puntero y toque directo). Usa el botón de inicio para volver al Centro de conexión desde la barra de comandos. También puedes usar el botón Atrás para realizar la misma acción. La sesión activa no se desconectará. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso de gestos de toque directo y de modos de ratón en una sesión remota

El cliente utiliza gestos táctiles estándar. También se pueden usar gestos táctiles para replicar acciones del ratón en el escritorio remoto. Los modos del ratón disponibles se definen en la tabla siguiente.

> [!NOTE]
> En la interacción con Windows 8, o las versiones más recientes, se admiten los gestos táctiles nativos en modo de toque directo. 

| Modo de mouse    | Operación del ratón      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque directo  | Hacer clic con el botón izquierdo           | Pulsación con un dedo                                                          |
| Toque directo  | Hacer clic con el botón derecho          | Mantener pulsado con un dedo                                                 |
| Puntero | Zoom                 | Utiliza dos dedos y acércalos para acercar o sepáralos para alejar. |
| Puntero | Hacer clic con el botón izquierdo           | Pulsación con un dedo                                                          |
| Puntero | Hacer clic con el botón izquierdo y arrastrar  | Pulsar dos veces con un dedo y mantener pulsado, y luego arrastrar                               |
| Puntero | Hacer clic con el botón derecho          | Pulsación con dos dedos                                                          |
| Puntero | Hacer clic con el botón derecho y arrastrar | Pulsar dos veces con dos dedos y mantener pulsado, y luego arrastrar                               |
| Puntero | Rueda del ratón          | Pulsar con dos dedos y mantener pulsado, y luego arrastrar hacia arriba o abajo                           |

> [!TIP]
> Tanto las preguntas como los comentarios son siempre bienvenidos. Sin embargo, NO publiques una solicitud de ayuda para solucionar problemas con la característica de comentario del final de este artículo. En su lugar, vete al [foro del cliente de Escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicia una nueva conversación. ¿Tienes alguna sugerencia de característica? Realízala en el [foro de usuarios de clientes](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
