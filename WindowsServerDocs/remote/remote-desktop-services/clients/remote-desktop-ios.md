---
title: Introducción a Escritorio remoto en iOS
description: Obtenga información sobre cómo configurar el cliente de escritorio remoto para iOS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 01/13/2017
ms.localizationpriority: medium
ms.openlocfilehash: 1a1939c7fd6d25d756369c85e4adaa6c15195b37
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889636"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>Introducción a Escritorio remoto en iOS

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Puede usar al cliente de escritorio remoto para iOS para trabajar con aplicaciones, recursos y equipos de escritorio de Windows desde el dispositivo iOS (iPhone e iPad).

Use la siguiente información para empezar a trabajar. Asegúrese de consultar la [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tiene alguna pregunta.

> [!NOTE]
> - ¿Tiene curiosidad acerca de las nuevas versiones para el cliente de iOS? ¿Consulte [Novedades de escritorio remoto en iOS?](ios-whatsnew.md)
> - El cliente de iOS es compatible con dispositivos que ejecutan iOS 6.x y versiones más recientes.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obtener al cliente de escritorio remoto y empezar a usar

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Descargar al cliente de escritorio remoto desde la tienda de iOS
Siga estos pasos para empezar a trabajar con Escritorio remoto en el dispositivo iOS:

1. Descargue el cliente de escritorio remoto de Microsoft desde [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configuración de su PC para aceptar conexiones remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Agregar un [conexión a Escritorio remoto](#add-a-remote-desktop-connection) o un [recurso remoto](#add-a-remote-resource). Usar una conexión para conectarse a una a un equipo Windows y un recurso remoto para usar un programa RemoteApp, basados en sesión de escritorio o un escritorio virtual publica directamente en el entorno local mediante la conexión de RemoteApp y escritorio. Esta característica está disponible normalmente en entornos corporativos.

### <a name="download-the-remote-desktop-ios-beta-client"></a>Descargue al cliente de escritorio remoto de iOS versión Beta
En el dispositivo iOS, siga [estas instrucciones](https://aka.ms/rdiosbeta) para descargar el cliente de escritorio remoto de iOS versión Beta.

### <a name="add-a-remote-desktop-connection"></a>Agregar una conexión a Escritorio remoto

Para crear una conexión a escritorio remota: 
1. En la derivación de centro de conexiones **+** y, a continuación, puntee en **Agregar PC o servidor**.
2. Escriba la siguiente información para la conexión a escritorio remota:
  - **Nombre de equipo** : el nombre del equipo. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede anexar información del puerto en el nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Nombre de usuario** : el nombre de usuario a usar para obtener acceso al equipo remoto. Puede usar los siguientes formatos: *user_name*, *DOMINIO\nombre_de_usuario*, o *user_name@domain.com*. También puede especificar si se le solicitará un nombre de usuario y una contraseña.
3. También puede establecer las siguientes opciones adicionales:
  - **Nombre descriptivo (opcional)** : un nombre fácil de recordar para el equipo se conecta a. Puede utilizar cualquier cadena, pero si no especifica un nombre descriptivo, se muestra el nombre de equipo.
  - **(Opcional) de la puerta de enlace** : puerta de enlace de escritorio remoto el que desea usar para conectarse a escritorios virtuales, programas RemoteApp y escritorios basados en una red corporativa interna. Obtenga la información acerca de la puerta de enlace desde el administrador del sistema.
  - **Sonido** : seleccione el dispositivo que se usará para audio durante la sesión remota. Puede reproducir un sonido en los dispositivos locales, el dispositivo remoto, o no en absoluto.
  - **Intercambiar botones del mouse** : cada vez que un gesto del mouse tendría que enviar un comando con el botón primario del mouse, envía el mismo comando con el botón secundario del mouse en su lugar. Esto es necesario si el equipo remoto está configurado para el modo del mouse zurdo.
  - **Modo de administrador** -conectarse a una sesión de administración en un servidor que ejecuta Windows Server 2003 o posterior.
4. Pulse **guardar**.

¿Es necesario modificar esta configuración? Mantenga presionado el escritorio que desea editar y, a continuación, puntee en el icono de configuración. 

### <a name="add-a-remote-resource"></a>Agregar un recurso remoto
Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados utilizando la conexión de RemoteApp y escritorio.

- La dirección URL muestra el vínculo en el servidor de acceso Web de escritorio remoto que proporciona acceso a conexión de RemoteApp y escritorio.
- Se enumeran los configurado conexiones de RemoteApp y escritorio.

Para agregar un recurso remoto:

1. En la pantalla del centro de conexiones, pulse **+** y, a continuación, puntee en **agregar recursos remotos**. 
2. Escriba la información para el recurso remoto:
   - **Dirección URL de fuente** -la dirección URL del servidor de acceso Web de escritorio remoto. También puede especificar la cuenta de correo electrónico corporativo en este campo: Esto indica al cliente para buscar el servidor de acceso Web de escritorio remoto asociado con su dirección de correo electrónico.
   - **Nombre de usuario** -el nombre de usuario que se usará para el servidor de acceso Web de escritorio remoto que se conecta.
   - **Contraseña** -la contraseña que se usará para el servidor de acceso Web de escritorio remoto que se conecta.
3. Pulse **guardar**.

Los recursos remotos se mostrará en el centro de conexiones.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectarse a una puerta de enlace de escritorio remoto para tener acceso a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de RD) le permite conectarse a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puede crear y administrar las puertas de enlace mediante el cliente de escritorio remoto.

Para configurar una puerta de enlace:

1. En el centro de conexiones, pulse **configuración > puertas de enlace**. 
2. Pulse **puerta de enlace de escritorio remoto agregar**.
3. Escriba la siguiente información:
  - **Nombre del servidor** : el nombre del equipo que desea usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede agregar información de puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
  - **Nombre de usuario** -el nombre de usuario y la contraseña que se usará para la puerta de enlace de escritorio remoto que se conecta. También puede seleccionar **usar las credenciales de conexión** para usar el mismo nombre de usuario y la contraseña que se utilizaron para la conexión a escritorio remota.


## <a name="manage-your-user-accounts"></a>Administrar las cuentas de usuario 

Cuando se conecta a un recursos remotos o de escritorio, puede guardar las cuentas de usuario para seleccionar de nuevo. Puede administrar sus cuentas de usuario mediante el cliente de escritorio remoto.

Para crear una nueva cuenta de usuario:

1. En el centro de conexiones, pulse **configuración**y, a continuación, puntee en **los nombres de usuario**.
2. Pulse **Agregar cuenta de usuario**.
3. Escriba la siguiente información:
   - **Nombre de usuario** -el nombre del usuario para guardar para su uso con una conexión remota. Puede escribir el nombre de usuario en cualquiera de los siguientes formatos: user_name, DOMINIO\nombre_de_usuario, o user_name@domain.com.
   - **Contraseña** -la contraseña para el usuario especificado. Cada cuenta de usuario que desea guardar si desea para utilizar para las conexiones remotas debe tener una contraseña asociada con él.
4. Pulse **guardar**y, a continuación, puntee en **configuración**.
5. Pulse **realiza** para guardar la nueva configuración.

Para eliminar una cuenta de usuario:

1. En el centro de conexiones, pulse **configuración > los nombres de usuario**.
2. Deslice el dedo hacia la fila de derecha a izquierda para seleccionar el usuario.
3. Pulse **eliminar**.



## <a name="navigate-the-remote-desktop-session"></a>Navegar por la sesión de escritorio remoto
Al iniciar una sesión de escritorio remoto, hay herramientas disponibles que puede usar para navegar por la sesión.

### <a name="start-a-remote-desktop-connection"></a>Iniciar una conexión a escritorio remota

1. Puntee en la conexión a escritorio remota para iniciar la sesión de escritorio remota. 
2. Si se le pide que compruebe el certificado para el escritorio remoto, pulse **Accept**. Puede elegir Aceptar siempre deslizando el **no volver a preguntar para las conexiones a este equipo** alternar a **ON**. 

### <a name="connection-bar"></a>Barra de conexión

La conexión barra da como resultado que el acceso a los controles de navegación adicionales. 

- **Control de panorámica**: El control de panorámica permite a se amplía y se mueven por la pantalla. Tenga en cuenta que el control de panorámica solo está disponible mediante toque directo con.
   - Habilitar o deshabilitar el control de panorámica: Pulse el icono de panorámica en la barra de conexión para mostrar el control de panorámica y zoom en la pantalla. Pulse el icono de panorámica en la barra de conexión nuevo para ocultar el control y devolver la pantalla para su resolución original.
   - Use el control de panorámica: Pulse y mantenga el control de panorámica y, a continuación, arrastre en la dirección que desea mover la pantalla.
   - Mueva el control de panorámica: Double pulse y mantenga el control de panorámica para mover el control en la pantalla.
- **Nombre de la conexión**: Se muestra el nombre de la conexión actual. Pulse el nombre de conexión para mostrar la barra de selección de la sesión.
- **Teclado**: Pulse el icono de teclado para mostrar u ocultar el teclado. El control de panorámica se muestra automáticamente cuando se muestra el teclado.
- **Mueva la barra de conexión**: Pulse y mantenga presionada la barra de conexión y, a continuación, arrastre y coloque en una nueva ubicación en la parte superior de la pantalla.

### <a name="session-selection"></a>Selección de la sesión
Puede tener varias conexiones abra en equipos diferentes al mismo tiempo. Puntee en la barra de conexión para mostrar la barra de selección de la sesión en el lado izquierdo de la pantalla. La barra de selección de sesión permite ver sus conexiones abiertas y cambiar entre ellas. 

- Cambiar entre las aplicaciones en una sesión de abrir el recurso remoto.

    Cuando se conectan a recursos remotos, puede cambiar entre las aplicaciones abiertas dentro de esa sesión, pulse el menú del botón de expansión y elegir en la lista de elementos disponibles.
- Inicie una nueva sesión

  Puede iniciar nuevas aplicaciones o las sesiones de escritorio desde dentro de la conexión actual: pulse **iniciar nuevo**y, a continuación, elija en la lista de elementos disponibles.

- Desconexión de una sesión

  Para desconectar una derivación de la sesión X en el lado izquierdo del icono de la sesión.

### <a name="command-bar"></a>Barra de comandos

La barra de comandos reemplaza la utilidad de la barra a partir de la versión 8.0.1. Puede cambiar entre los modos de mouse y volver al centro de conexión de la barra de comandos.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Use gestos táctiles y los modos de mouse (ratón) en una sesión remota

El cliente utiliza gestos táctiles estándar. También puede usar gestos táctiles para replicar las acciones del mouse en el escritorio remoto. Los modos de mouse disponibles se definen en la tabla siguiente.

> [!NOTE]
> Interactúan con Windows 8 o versiones más recientes se admiten los gestos táctiles nativas en toque directo con el modo. Para obtener más información sobre Windows 8 Consulte gestos [Touch: Deslice el dedo, pulse y mucho más](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Modo de mouse    | Operación del mouse      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Toque directo con  | El botón primario           | Pulse 1 dedo                                               |
| Toque directo con  | Hacer clic con el botón derecho          | 1 tap dedo y suspensión                                      |
| Puntero del mouse | El botón primario           | Pulse 1 dedo                                               |
| Puntero del mouse | Hacer clic con el botón primario y arrastrar  | pulsar dos veces 1 dedo y mantener presionado, arrastre                    |
| Puntero del mouse | Hacer clic con el botón derecho          | Pulse dedo 2                                               |
| Puntero del mouse | Hacer clic con el botón derecho y arrastrar | 2 pulsar dos veces dedo y mantener presionado, arrastre                    |
| Puntero del mouse | Rueda del mouse          | dedo 2 pulse y mantenga presionado, y luego arrastre hacia arriba o hacia abajo                |
| Puntero del mouse | Zoom                 | Alejar 2 dedos para acercar o distribuir 2 dedos para alejar |

## <a name="supported-input-devices"></a>Admite dispositivos de entrada

El [de cliente de escritorio remoto iOS beta](https://aka.ms/rdiosbeta) es compatible con los ratones Swiftpoint GT y ProPoint físicos. Swiftpoint está ofreciendo un [descuento exclusivo](https://www.swiftpoint.com/microsoft/) en el GT para los usuarios del cliente de iOS versión beta.

El cliente de iOS actualmente solo admite Swiftpoint mice. Hacer referencia a la [cuáles son las novedades en el cliente de iOS](ios-whatsnew.md) página y el [iOS App Store](https://aka.ms/rdios) para noticias sobre la compatibilidad con otros dispositivos en el futuro.

## <a name="use-a-keyboard-in-a-remote-session"></a>Usar un teclado en una sesión remota

Se puede usar una que aparecen en pantalla o teclado físico en una sesión remota.

En la pantalla teclados, use el botón en el borde derecho de la barra encima del teclado para cambiar entre el teclado estándar y adicional.

Si es compatible con Bluetooth para el dispositivo iOS, el cliente detecta automáticamente el teclado Bluetooth.

Tenga en cuenta que, debido a limitaciones en el sistema operativo, las teclas especiales, como Ctrl, la opción y la función no funcionará según lo previsto con un teclado Bluetooth. El trabajo de las claves siguientes:

- Teclas alfanuméricas
- Teclas de dirección
- Pestaña: Pestaña funciona, pero no funciona Mayús + Tab
- Inicio / Pos1: Alt+flecha izquierda = Home
- Fin: Alt+flecha derecha = final
- RE PÁG: Alt+flecha arriba = RE PÁG
- AV PÁG: Alt+flecha abajo = AV PÁG
- Seleccionar todo: Comando + A = CTRL+a (Seleccionar todo en la mayoría de los programas)
- Cortar: Comando + X = CTRL+x (Cortar en la mayoría de los programas)
- Copiar: Comando + C = CTRL+c (copiar en la mayoría de los programas)
- Pegar: Comando + V = CTRL+v (pegar en la mayoría de los programas)
- Símbolos: Teclas ALT + alfanumérica generará símbolos diferentes dependiendo del idioma configurado

> [!TIP]
> Preguntas y comentarios siempre son bienvenidos. Sin embargo, no publique una solicitud de ayuda para solucionar problemas con la característica de comentario al final de este artículo. En su lugar, vaya a la [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene alguna sugerencia de característica? Díganos en el [foro de uservoice cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

