---
title: Introducción al cliente de iOS
description: Aprenda a configurar el cliente de Escritorio remoto para iOS
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: Heidilohr
manager: lizross
ms.author: helohr
date: 02/11/2020
ms.localizationpriority: medium
ms.openlocfilehash: ef13227a9f7b83f01786bbb11498da912c86581b
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370698"
---
# <a name="get-started-with-the-ios-client"></a>Introducción al cliente de iOS

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2

El cliente Escritorio remoto para iOS se puede usar para trabajar con aplicaciones, recursos y equipos de escritorio de Windows desde un dispositivo iOS (iPhone e iPad).

Usa la siguiente información para comenzar. Asegúrate de consultar la sección de [Preguntas frecuentes](remote-desktop-client-faq.md) si te surge alguna duda.

> [!NOTE]
> - ¿Tienes curiosidad acerca de las nuevas versiones para el cliente iOS? Consulta [Novedades de Escritorio remoto en iOS](ios-whatsnew.md)
> - El cliente iOS admite dispositivos que usen iOS 6.x, y cualquier versión posterior.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obtención del cliente de Escritorio remoto y su primer uso

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Descarga del cliente de Escritorio remoto de la tienda iOS

Sigue estos pasos para empezar a usar Escritorio remoto en un dispositivo iOS:

1. Descarga el cliente de Escritorio remoto de Microsoft desde [iOS App Store](https://aka.ms/rdios) o [iTunes](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configura tu equipo para que acepte conexiones remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Agrega una [conexión a Escritorio remoto](#add-a-remote-desktop-connection) o un [recurso remoto](#add-a-remote-resource). Usa una conexión para conectarte directamente a un equipo Windows y un recurso remoto para usar un programa de RemoteApp, un escritorio basado en sesión o un escritorio virtual publicado de forma local mediante Conexión de RemoteApp y Escritorio. Esta característica está disponible habitualmente en entornos corporativos.

### <a name="add-a-remote-desktop-connection"></a>Adición de una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En el Centro de conexión, pulsa **+** y luego **Agregar equipo o servidor**.
2. Escribe la siguiente información de la conexión a Escritorio remoto:
   - **Nombre del equipo**: el nombre del equipo. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Nombre de usuario**: el nombre de usuario que se utiliza para acceder al equipo remoto. Puedes usar los siguientes formatos: *nombre_de_usuario*, *dominio\nombre_de_usuario* o `user_name@domain.com`. También puedes especificar si deseas que se soliciten el nombre de usuario y la contraseña.
3. También puedes establecer las siguientes opciones adicionales:
   - **Nombre descriptivo (opcional)** : un nombre fácil de recordar para el equipo al que se conecta. Puedes utilizar cualquier cadena, pero si no especificas un nombre descriptivo, se muestra el nombre del equipo.
   - **Puerta de enlace (opcional)** : la puerta de enlace de Escritorio remoto que deseas usar para conectarte a escritorios virtuales, programas RemoteApp y escritorios basados en sesión de una red corporativa interna. La información acerca de la puerta de enlace te la puede proporcionar el administrador del sistema.
   - **Sonido**: selecciona el dispositivo que se usa para audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, en el dispositivo remoto o no reproducirlo.
   - **Intercambiar botones del ratón**: cada vez que un gesto del ratón enviaría un comando con el botón izquierdo, envía el mismo comando con el botón derecho. Es necesario si el equipo remoto está configurado para el modo de ratón para zurdos.
   - **Modo de administrador**: conéctate a una sesión de administración en un servidor que usa Windows Server 2003 o posterior.
4. Pulsa **Guardar**.

¿Es necesario editar esta configuración? Mantén presionado el escritorio que deseas editar y, a continuación, pulsa el icono de configuración.

### <a name="add-a-remote-resource"></a>Adición de recursos remotos

Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados mediante Conexión de RemoteApp y Escritorio.

- La dirección URL muestra el vínculo al servidor de Acceso web a Escritorio remoto que proporciona acceso a Conexión de RemoteApp y Escritorio.
- Se enumeran los configurado conexiones de Conexión de RemoteApp y Escritorio configuradas.

Para agregar un recurso remoto:

1. En la pantalla Centro de conexión, pulsa **+** y, después, pulsa  **Add Remote Resources (Agregar recursos remotos)** .
2. Escribe información para el recurso remoto:
   - **Dirección URL de fuente**: la dirección URL del servidor de Acceso web a Escritorio remoto. En este campo también puedes escribir la cuenta de correo electrónico corporativa (esto indica al cliente que busque el servidor de Acceso web a Escritorio remoto asociado con la dirección de correo electrónico).
   - **Nombre de usuario**: el nombre de usuario que se usa para servidor de Acceso web a Escritorio remoto al que se conecta.
   - **Contraseña**la contraseña que se usa para servidor de Acceso web a Escritorio remoto al que se conecta.
3. Pulsa **Guardar**.

Los recursos remotos se mostrarán en el Centro de conexión.

## <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando te conectas a un escritorio o a recursos remotos, puedes guardar las cuentas de usuario para volver a seleccionarlas más adelante.

Para crear una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**y luego pulsa **Cuentas de usuario**.
2. Pulsa **Agregar cuenta de usuario**.
3. Escribe la siguiente información:
   - **Nombre de usuario**: el nombre del usuario que se guarda para usarlo con una conexión remota. El nombre de usuario se puede escribir en cualquiera de los siguientes formatos: nombre_de_usuario, dominio\nombre_de_usuario o user_name@domain.com.
   - **Contraseña**: la contraseña del usuario que has especificado. Todas las cuentas de usuario que quieras guardar para usarlas para las conexiones remotas deben tener una contraseña asociada.
4. Pulsa **Guardar**.

Para eliminar una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**y luego pulsa **Cuentas de usuario**.
2. Selecciona la cuenta que quieres eliminar.
3. Pulsa **Eliminar**.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conexión a una puerta de enlace de Escritorio remoto para acceder a recursos internos

Las puertas de enlace de Escritorio remoto te permiten conectarte a equipos remotos de una red corporativa desde cualquier lugar de Internet. Para crear y administrar dichas puertas de enlace, usa el cliente de Escritorio remoto.

Para configurar una puerta de enlace nueva:

1. En el Centro de conexión, pulsa **Configuración** > **Puerta de enlace**.
2. Pulsa **Agregar puerta de enlace de Escritorio remoto**.
3. Escribe la siguiente información:
   - **Nombre del servidor**: el nombre del equipo que quieres usar como puerta de enlace. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Nombre de usuario**: el nombre de usuario y la contraseña que se van a usar para la puerta de enlace de Escritorio remoto a la que se va a conectar. También puedes seleccionar **Usar credenciales de conexión** para usar el mismo nombre de usuario y la misma contraseña que para la conexión a Escritorio remoto.

## <a name="navigate-the-remote-desktop-session"></a>Desplazamiento a la sesión de Escritorio remoto

Al iniciar una sesión de Escritorio remoto, hay varias herramientas disponibles que se pueden usar para desplazarse por la sesión.

### <a name="start-a-remote-desktop-connection"></a>Inicio de una conexión a Escritorio remoto

1. Pulsa en la conexión con Escritorio remoto para iniciar la sesión de escritorio remota.
2. Si se te pide que compruebes el certificado del escritorio remoto, pulsa **Aceptar**. Puedes elegir aceptar siempre, para lo que debes deslizar el botón de alternancia **No volver a preguntar las conexiones a este equipo** a **Activado**.

### <a name="connection-bar"></a>Barra de conexión

La barra de conexión te permite acceder a más controles de navegación.

- **Pan Control** (Control de panorámica): el control de panorámica permite ampliar la pantalla y moverse alrededor de ella. Ten en cuenta que el control de panorámica solo está disponible si se usa el toque directo.
   - Habilitar o deshabilitar el control de panorámica: pulsa el icono de panorámica de la barra de conexión para mostrar el control de panorámica y aplicar zoom a la pantalla. Vuelve a pulsar el icono de panorámica en la barra de conexión nuevo para ocultar el control y devolver la pantalla a su resolución original.
   - Usar el control de panorámica: mantén pulsado el control de panorámica y arrástralo en la dirección que deseas mover la pantalla.
   - Mover el control de panorámica: pulsa dos veces y mantén pulsado el control de panorámica para mover el control en la pantalla.
- **Nombre de la conexión**: se muestra el nombre de la conexión actual. Pulsa el nombre de conexión para mostrar la barra de selección de la sesión.
- **Teclado**: pulsa el icono del teclado para mostrar u ocultar el teclado. El control de panorámica se muestra automáticamente cuando se muestra el teclado.
- **Mover la barra de conexión**: Mantén pulsada la barra de conexión, arrástrala y colócala en una nueva ubicación en la parte superior de la pantalla.

### <a name="session-selection"></a>Selección de la sesión

Puede tener varias conexiones abiertas en equipos diferentes al mismo tiempo. Pulsa la barra de conexión para mostrar la barra de selección de sesión en el lado izquierdo de la pantalla. La barra de selección de sesión permite ver las conexiones abiertas y cambiar de una a otra.

- Cambia entre aplicaciones en una sesión de recursos remotos abierta.

    Cuando estás conectado a recursos remotos, para cambiar entre las distintas aplicaciones abiertas de la esa sesión, pulsa el menú del expansor y elige en la lista de elementos disponibles el que desees.
- Inicio de una nueva sesión

  Es posible iniciar nuevas aplicaciones o sesiones de escritorio desde dentro de la conexión actual: pulsa **Iniciar nuevo**y, después, elija en la lista de elementos disponibles el que desee.

- Desconexión de una sesión

  Para desconectar una sesión, pulsa X en el lado izquierdo del icono de la sesión.

### <a name="command-bar"></a>Barra de comandos

La barra de comandos ha reemplazado a la barra de utilidad, que comenzó en la versión 8.0.1. Desde la barra de comandos puede cambiar de un modo del ratón a otro y volver al centro de conexión.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso de gestos táctiles y de modos de ratón en una sesión remota

El cliente utiliza gestos táctiles estándar. También se pueden usar gestos táctiles para replicar acciones del ratón en el escritorio remoto. Los modos del ratón disponibles se definen en la tabla siguiente.

> [!NOTE]
> En la interacción con Windows 8, o las versiones más recientes, se admiten los gestos táctiles nativos en modo de toque directo. Para más información acerca de los gestos de Windows 8, consulta[Entrada táctil: deslizar el dedo, pulsar y mucho más](https://windows.microsoft.com/windows-8/touch-swipe-tap-beyond).

| Modo del mouse    | Operación del ratón      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Toque directo  | Hacer clic con el botón izquierdo           | Pulsación con un dedo                                               |
| Toque directo  | Hacer clic con el botón derecho          | Mantener pulsado con un dedo                                      |
| Puntero | Hacer clic con el botón izquierdo           | Pulsación con un dedo                                               |
| Puntero | Hacer clic con el botón izquierdo y arrastrar  | Pulsar dos veces con un dedo y mantener pulsado, y luego arrastrar                    |
| Puntero | Hacer clic con el botón derecho          | Pulsación con dos dedos                                               |
| Puntero | Hacer clic con el botón derecho y arrastrar | Pulsar dos veces con dos dedos y mantener pulsado, y luego arrastrar                    |
| Puntero | Rueda del ratón          | Pulsar con dos dedos y mantener pulsado, y luego arrastrar hacia arriba o abajo                |
| Puntero | Zoom                 | Acercar dos dedos para acercar la imagen o alejar dos dedos para alejar la imagen |

## <a name="supported-input-devices"></a>Dispositivos de entrada compatibles

Hay [compatibilidad básica con el ratón Bluetooth](https://support.apple.com/HT210546) disponible en iOS 13 e iPadOS como característica de accesibilidad. Está disponible una mayor integración del ratón en el cliente de escritorio remoto mediante los ratones Swiftpoint GT y ProPoint. Además, también se admiten los teclados externos que son compatibles con iOS e iPad.

Para obtener más información sobre la compatibilidad con dispositivos, consulta [Novedades del cliente de iOS](ios-whatsnew.md) e [iOS App Store](https://aka.ms/rdios).

> [!TIP]
> Swiftpoint ofrece un [descuento exclusivo en el ratón ProPoint](https://www.swiftpoint.com/microsoft) para los usuarios del cliente iOS.

## <a name="use-a-keyboard-in-a-remote-session"></a>Uso de un teclado en una sesión remota

En la sesión remota puede usar tanto un teclado en pantalla como un teclado físico.

En los teclados en pantalla, use el botón del borde derecho de la barra que hay encima del teclado para cambiar entre el teclado estándar y el adicional.

Si el Bluetooth está habilitado en el dispositivo iOS, el cliente detecta automáticamente el teclado Bluetooth.

Aunque es posible que algunas combinaciones de teclas no funcionen como se esperaba en una sesión remota, muchas de las combinaciones de teclas comunes de Windows, como CTRL+C, CTRL+V y ALT+TAB funcionarán.

> [!IMPORTANT]
> Tanto las preguntas como los comentarios son siempre bienvenidos. Sin embargo, NO publiques una solicitud de ayuda para solucionar problemas con la característica de comentario del final de este artículo. En su lugar, ve al [foro del cliente de Escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicia una nueva conversación. ¿Tienes alguna sugerencia de característica? Realízala en el [foro de usuarios de clientes](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
