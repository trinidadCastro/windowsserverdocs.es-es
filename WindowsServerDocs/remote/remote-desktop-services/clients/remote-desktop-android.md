---
title: Introducción a Escritorio remoto en Android
description: Basic pasos de configuración para el cliente de escritorio remoto para Android.
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446742"
---
# <a name="get-started-with-remote-desktop-on-android"></a>Introducción a Escritorio remoto en Android

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puede usar al cliente de escritorio remoto para Android para trabajar con equipos de escritorio y aplicaciones de Windows directamente desde un dispositivo Android.

Use la siguiente información para empezar a trabajar. Asegúrese de consultar la [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tiene alguna pregunta.

> [!NOTE]
> - ¿Tiene curiosidad acerca de las nuevas versiones del cliente Android? ¿Consulte [cuáles son las novedades de escritorio remoto en Android?](android-whatsnew.md)
> Puede ejecutar al cliente Android 4.1 y dispositivos más recientes, así como en los Chromebooks con 53 ChromeOS instalado. Más información sobre las aplicaciones de Android en Chrome [aquí](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="get-the-rd-client-and-start-using-it"></a>Obtener al cliente de escritorio remoto y empezar a usar

Siga estos pasos para empezar a trabajar con Escritorio remoto en el dispositivo Android:

1. Descargue el cliente de escritorio remoto desde [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configuración de su PC para aceptar conexiones remotas](remote-desktop-allow-access.md).
3. Agregar una conexión a Escritorio remoto o un recurso remoto. Usar una conexión para conectarse directamente a un equipo de Windows y un recurso remoto para utilizar un programa RemoteApp, basados en sesión de escritorio o escritorio virtual publicada en el entorno local. 
4. Crear un widget para que pueda empezar rápidamente a Escritorio remoto.

> [!NOTE]
> Si lo desea para las nuevas características de vuelo anteriormente le recomendamos que descargue nuestra [Microsoft Remote Desktop Beta](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) aplicación desde Google Play store. 

### <a name="add-a-remote-desktop-connection"></a>Agregar una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En la derivación de centro de conexiones **+** y, a continuación, puntee en **Desktop**.
2. Escriba la siguiente información para el equipo que desea conectarse:
   - **Nombre de equipo** : el nombre del equipo. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede anexar información del puerto en el nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nombre de usuario** : el nombre de usuario a usar para obtener acceso al equipo remoto. Puede usar los siguientes formatos: *user_name*, *DOMINIO\nombre_de_usuario*, o <em>user_name@domain.com</em>. También puede especificar si se le solicitará un nombre de usuario y una contraseña.
3. También puede establecer las siguientes opciones adicionales:
   - **Nombre descriptivo** : un nombre fácil de recordar para el equipo se conecta a. Puede utilizar cualquier cadena, pero si no especifica un nombre descriptivo, se muestra el nombre de equipo.
   - **Puerta de enlace** : puerta de enlace de escritorio remoto el que desea usar para conectarse a escritorios virtuales, programas RemoteApp y escritorios basados en una red corporativa interna. Obtenga la información acerca de la puerta de enlace desde el administrador del sistema.
    ¿Debe configurar una puerta de enlace de escritorio remoto?
   - **Sonido** : seleccione el dispositivo que se usará para audio durante la sesión remota. Puede reproducir un sonido en los dispositivos locales, el dispositivo remoto, o no en absoluto.
   - **Personalizar la resolución de pantalla** -establecer una resolución personalizada para una conexión si habilita esta configuración. Cuando se aplica fuera de la resolución que ha definido en la configuración global de la aplicación.
   - **Intercambiar botones del mouse** : Use esta opción para cambiar el primario del mouse funciones del botón para el botón secundario del mouse. (Esto es especialmente útil si el equipo remoto está configurado para un usuario zurdo pero usa un mouse diestro).
   - **Conectarse a la sesión de administrador** -Utilice esta opción para conectarse a una sesión de consola para administrar un servidor de Windows.
   - **Redirigir a almacenamiento local** : el almacenamiento local se monta como un sistema de archivos remoto en el equipo remoto.
4. Pulse **guardar**.

¿Es necesario modificar esta configuración? Puntee en el menú de desbordamiento ( **...** ) junto al nombre del escritorio y, a continuación, en **editar**.

¿Desea eliminar la conexión? De nuevo, puntee en el menú de desbordamiento ( **...** ) y, a continuación, puntee en **quitar**.

>[!TIP]
> Si se produce el error 0xf07 acerca de una contraseña incorrecta ("se puede conectar con el equipo remoto porque ha caducado la contraseña asociada con la cuenta de usuario"), cambiar la contraseña y vuelva a intentarlo.

### <a name="add-a-remote-resource"></a>Agregar un recurso remoto
Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados utilizando la conexión de RemoteApp y escritorio.

Para agregar un recurso remoto:

1. En la pantalla del centro de conexiones, pulse **+** y, a continuación, puntee en **fuente de los recursos remotos**. 
2. Escriba la información para el recurso remoto:
   - **Dirección URL o correo electrónico** -la dirección URL del servidor de acceso Web de escritorio remoto. También puede especificar la cuenta de correo electrónico corporativo en este campo: Esto indica al cliente para buscar el servidor de acceso Web de escritorio remoto asociado con su dirección de correo electrónico.
   - **Nombre de usuario** -el nombre de usuario que se usará para el servidor de acceso Web de escritorio remoto que se conecta.
   - **Contraseña** -la contraseña que se usará para el servidor de acceso Web de escritorio remoto que se conecta.
3. Pulse **guardar**.

Los recursos remotos se mostrará en el centro de conexiones.


Para eliminar los recursos remotos:

1. En el centro de conexiones, pulse el menú de desbordamiento ( **...** ) junto al recurso remoto.
2. Pulse **quitar**.
3. Confirme la eliminación.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Widgets: ancle un escritorio guardado en la pantalla principal

Las aplicaciones de escritorio remoto admiten conexiones ancladas a la pantalla principal mediante la característica de widget de Android. La manera en que agregar un widget depende del tipo de dispositivo Android que está usando y su sistema operativo. Aquí es la manera más común para agregar un widget: 

1. Pulse **aplicaciones** para iniciar el menú de aplicaciones.
2. Pulse **widgets**.
3. Desplácese a través de los widgets y busque el icono de escritorio remoto con la descripción, "Pin de escritorio remoto".
4. Pulse y mantenga ese widget de escritorio remoto y muévalo a la pantalla principal.
5. Al soltar el icono, verá los escritorios remotos guardados. Elija la conexión que desea guardar en la pantalla principal.

Ahora puede iniciar la conexión a escritorio remota directamente desde la pantalla principal, puntee en él.

> [!NOTE]
> Si cambia el nombre de la conexión de escritorio en la aplicación de escritorio remoto, no se actualizará la etiqueta de este anclado escritorio remoto.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectarse a una puerta de enlace de escritorio remoto para tener acceso a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de RD) le permite conectarse a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puede crear y administrar las puertas de enlace mediante el cliente de escritorio remoto.

Para configurar una puerta de enlace:

1. En el centro de conexiones, pulse **configuración > puertas de enlace**. Pulse **+** para agregar una nueva puerta de enlace.
2. Escriba la siguiente información:
   - **Nombre del servidor** : el nombre del equipo que desea usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede agregar información de puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Nombre de usuario** -el nombre de usuario y la contraseña que se usará para la puerta de enlace de escritorio remoto que se conecta. También puede seleccionar **usar cuenta de usuario de escritorio** para usar las mismas credenciales que se utilizaron para la conexión a escritorio remota.

## <a name="manage-your-user-accounts"></a>Administrar las cuentas de usuario

Cuando se conecta a un recursos remotos o de escritorio, puede guardar las cuentas de usuario para seleccionar de nuevo. También puede definir las cuentas de usuario en el cliente, en lugar de guardar los datos de usuario cuando se conecta a un equipo de escritorio.

Para crear una nueva cuenta de usuario:

1. En el centro de conexiones, pulse **configuración**y, a continuación, puntee en **cuentas de usuario**.
2. Pulse **+** para agregar una nueva cuenta de usuario.
3. Escriba la siguiente información:
   - **Nombre de usuario** -el nombre del usuario para guardar para su uso con una conexión remota. Puede escribir el nombre de usuario en cualquiera de los siguientes formatos: user_name, DOMINIO\nombre_de_usuario, o user_name@domain.com.
   - **Contraseña** -la contraseña para el usuario especificado. Cada cuenta de usuario que desea guardar si desea para utilizar para las conexiones remotas debe tener una contraseña asociada con él.
4. Pulse **guardar**.

Para eliminar una cuenta de usuario:

1. En el centro de conexiones, pulse **configuración > cuentas de usuario**.
2. Pulse y mantenga una cuenta de usuario en la lista para seleccionarla. Puede seleccionar varios usuarios.
3. Pulse la Papelera para eliminar el usuario seleccionado.

## <a name="navigate-the-remote-desktop-session"></a>Navegar por la sesión de escritorio remoto
Cuando se inicia una conexión a escritorio remota, hay herramientas disponibles que puede usar para navegar por la sesión.

### <a name="start-a-remote-desktop-connection"></a>Iniciar una conexión a Escritorio remoto

1. Puntee en la conexión a Escritorio remoto para iniciar la sesión. 
2. Si se le pide que compruebe el certificado para el escritorio remoto, pulse **Connect**. También puede seleccionar **no volver a preguntar para las conexiones a este equipo** para aceptar siempre el certificado.

### <a name="manage-global-app-settings"></a>Administrar la configuración de aplicación global

Puede establecer la configuración global siguiente en el cliente de Android:

- **Mostrar vistas previas de escritorio** -le permite obtener una vista previa de un escritorio en el centro de conexiones antes de conectarse a él. De forma predeterminada, se establece en **en**.
- **Acercar el zoom** -le permite usar gestos de acercar para alejar. Si la aplicación usa a través de escritorio remoto es compatible con multitoque (introducida en Windows 8), Active esta configuración **desactivar**.
- **Ayuda a mejorar el escritorio remoto** -envía datos anónimos a Microsoft. Usamos estos datos para mejorar al cliente. Puede obtener más información sobre cómo se tratan estos datos anónimos, privados, consulte el [declaración de privacidad de cliente de escritorio remoto](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). De forma predeterminada, esta configuración es **en**.
- **Mostrar** -hay dos configuraciones globales para su presentación:
  - **Orientación** -establece la orientación preferida (horizontal o vertical) para la sesión. 
    >[!NOTE]
    > Si se conecta a un equipo que ejecuta Windows 8 o una versión anterior de Windows, la sesión no escala correctamente. La mejor opción es desconectar desde el equipo y, a continuación, volver a conectar en la orientación que desee utilizar. Una mejor opción consiste en actualizar el equipo al menos Windows 8.1.

  - **Resolución** -establece la resolución que desee utilizar para las conexiones de escritorio globalmente. Si ya ha establecido una resolución personalizada para una aplicación individual o una conexión, esta opción no cambia.
    >[!NOTE]
    >Cuando se cambia una de las opciones de presentación, solo se aplican a las nuevas conexiones desde ese punto en. Para ver el cambio en una sesión ya está conectado para desconectar y, a continuación, volver a conectar.

### <a name="connection-bar"></a>Barra de conexión

La conexión barra da como resultado que el acceso a los controles de navegación adicionales. De forma predeterminada, la barra de conexión se coloca en la parte central en la parte superior de la pantalla. Doble punteo y arrastre la barra hacia la izquierda o derecha para moverlo.

- **Control de panorámica**: El control de panorámica permite a se amplía y se mueven por la pantalla. Tenga en cuenta que el control de panorámica solo está disponible mediante toque directo con.
   - Habilitar o deshabilitar el control de panorámica: Pulse el icono de panorámica en la barra de conexión para mostrar el control de panorámica y zoom en la pantalla. Pulse el icono de panorámica en la barra de conexión nuevo para ocultar el control y devolver la pantalla para su resolución original.
   - Use el control de panorámica: Pulse y mantenga el control de panorámica y, a continuación, arrastre en la dirección que desea mover la pantalla.
   - Mueva el control de panorámica: Double pulse y mantenga el control de panorámica para mover el control en la pantalla.
- **Opciones adicionales**: Pulse el icono de opciones adicionales para mostrar la selección de la sesión de la barra y comando barra (ver abajo).
- **Teclado**: Pulse el icono de teclado para mostrar u ocultar el teclado. El control de panorámica se muestra automáticamente cuando se muestra el teclado.
- **Mueva la barra de conexión**: Pulse y mantenga presionada la barra de conexión y, a continuación, arrastre y coloque en una nueva ubicación en la parte superior de la pantalla.


### <a name="command-bar"></a>Barra de comandos

Puntee en la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla. Puede cambiar entre los modos del ratón (puntero del Mouse y toque directo). Utilice el botón de inicio para volver al centro de conexión de la barra de comandos. También puede usar el botón Atrás para la misma acción. No se desconectará la sesión activa. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso directo touch gestos y modos de mouse (ratón) en una sesión remota

El cliente utiliza gestos táctiles estándar. También puede usar gestos táctiles para replicar las acciones del mouse en el escritorio remoto. Los modos de mouse disponibles se definen en la tabla siguiente.

> [!NOTE]
> Interactúan con Windows 8 o versiones más recientes se admiten los gestos táctiles nativas en toque directo con el modo. 

| Modo de mouse    | Operación del mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque directo con  | El botón primario           | Pulse 1 dedo                                                          |
| Toque directo con  | Hacer clic con el botón derecho          | 1 tap dedo y suspensión                                                 |
| Puntero del mouse | Zoom                 | Utilice 2 dedos y acerca los dedos para acercar o mover separar los dedos para alejar. |
| Puntero del mouse | El botón primario           | Pulse 1 dedo                                                          |
| Puntero del mouse | Hacer clic con el botón primario y arrastrar  | pulsar dos veces 1 dedo y mantener presionado, arrastre                               |
| Puntero del mouse | Hacer clic con el botón derecho          | Pulse dedo 2                                                          |
| Puntero del mouse | Hacer clic con el botón derecho y arrastrar | 2 pulsar dos veces dedo y mantener presionado, arrastre                               |
| Puntero del mouse | Rueda del mouse          | dedo 2 pulse y mantenga presionado, y luego arrastre hacia arriba o hacia abajo                           |

> [!TIP]
> Preguntas y comentarios siempre son bienvenidos. Sin embargo, no publique una solicitud de ayuda para solucionar problemas con la característica de comentario al final de este artículo. En su lugar, vaya a la [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene alguna sugerencia de característica? Díganos en el [foro de uservoice cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
