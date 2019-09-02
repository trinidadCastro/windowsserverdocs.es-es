---
title: Introducción al cliente de macOS
description: Aprenda a configurar el cliente de Escritorio remoto para Mac.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8836ab500e97b68efbcdd0cd1ca5bcbe39d79334
ms.sourcegitcommit: 51eaab0f860312d97293fd90f3e632e7caee3df1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70150921"
---
# <a name="get-started-with-the-macos-client"></a>Introducción al cliente de macOS

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Puedes usar el cliente de Escritorio remoto para Mac para trabajar con aplicaciones, recursos y equipos de escritorio de Windows desde tu equipo Mac. Usa la siguiente información para empezar y revisa las [Preguntas más frecuentes](remote-desktop-client-faq.md) si tienes dudas.

>[!NOTE]
> - ¿Tienes curiosidad sobre las nuevas versiones para el cliente macOS? Consulta [Novedades de Escritorio remoto en Mac](mac-whatsnew.md).
> - El cliente Mac se ejecuta en equipos con macOS 10.10 y versiones más recientes.
> - La información de este artículo se aplica principalmente a la versión completa del cliente Mac, la versión disponible en Mac App Store. Para probar las características nuevas, descarga aquí la versión preliminar de la aplicación: [notas de la versión del cliente beta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Obtención del cliente de Escritorio remoto
Sigue estos pasos para empezar a usar Escritorio remoto en un equipo Mac:

1. Descarga el cliente de Escritorio remoto de Microsoft desde [Mac App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configura tu equipo para que acepte conexiones remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Si omites este paso, no te podrás conectar a tu equipo).
3. Agrega una conexión a Escritorio remoto o un recurso remoto. Usa una conexión para conectarte directamente a un PC Windows y un recurso remoto para usar un programa de RemoteApp, un escritorio basado en sesión o un escritorio virtual publicado de forma local mediante Conexión de RemoteApp y Escritorio. Esta característica está disponible habitualmente en entornos corporativos.

## <a name="what-about-the-mac-beta-client"></a>¿Qué pasa con el cliente beta de Mac?
Estamos probando características nuevas en el canal de versión preliminar en HockeyApp. ¿Quieres echarle un vistazo? Ve a [Escritorio remoto de Microsoft para Mac](https://go.microsoft.com/fwlink/?LinkID=619698) y haz clic en **Descargar**. No es necesario que crees una cuenta ni que inicies sesión en HockeyApp para descargar el cliente beta.

Si ya tienes el cliente, puedes comprobar las actualizaciones para asegurarte de que tienes la última versión. En el cliente beta, haz clic en **Microsoft Remote Desktop Beta** (Beta de Escritorio remoto de Microsoft) en la parte superior y, luego, haz clic en **Check for updates** (Buscar actualizaciones). 

## <a name="add-a-remote-desktop-connection"></a>Adición de una conexión a Escritorio remoto
Para crear una conexión a Escritorio remoto:

1. En Connection Center, haz clic en **+** y, luego, en **Desktop** (Escritorio).
2. Escribe la siguiente información:
   - **Nombre de equipo**: el nombre del equipo.
      - Puede ser el nombre de un equipo Windows (en la configuración **Sistema**), un nombre de dominio o una dirección IP.
      - También puedes agregar información del puerto al final de este nombre, como *MyDesktop:3389*.
   - **Cuenta de usuario**: agrega la cuenta de usuario que usas para acceder al equipo remoto.
     - En el caso de las cuentas locales o los equipos unidos a Active Directory (AD), usa uno de estos formatos: *nombre_usuario*, *dominio\nombre_usuario* o <em>user_name@domain.com</em>.
     - En el caso de los equipos unidos a Azure Active Directory (AAD), usa uno de estos formatos: *AzureAD\nombre_usuario* o <em>AzureAD\user_name@domain.com</em>.
     - También puedes elegir si requerir una contraseña.
     - Al administrar varias cuentas de usuario con el mismo nombre de usuario, establece un nombre descriptivo para diferenciar las cuentas.
     - Administra tus cuentas de usuario guardadas en las preferencias de la aplicación. 

3. También puedes establecer estas configuraciones opcionales para la conexión:
   - Establecer un nombre descriptivo 
   - Agregar una puerta de enlace
   - Establecer la salida de sonido
   - Intercambiar los botones del mouse
   - Habilitar el modo de administrador
   - Redirigir las carpetas locales a una sesión remota
   - Reenviar las impresoras locales
   - Reenviar las tarjetas inteligentes
4. Haga clic en **Guardar**.

Para empezar la conexión, simplemente debes hacer doble clic en ella. Lo mismo sucede para los recursos remotos.

### <a name="export-and-import-connections"></a>Exportación e importación de conexiones
Puedes exportar una definición de conexión a Escritorio remoto y usarla en otro dispositivo. Los escritorios remotos se guardan en archivos .RDP separados.

1. En Connection Center, haz clic con el botón derecho en el escritorio remoto.
2. Haga clic en **Exportar**.
3. Ve a la ubicación donde quieres guardar el archivo .RDP del escritorio remoto.
4. Haga clic en **Aceptar**.

Sigue estos pasos para importar un archivo .RDP de escritorio remoto.

1. En la barra de menús, haz clic en **Archivo** > **Importar**.
2. Ve al archivo .RDP.
3. Haga clic en **Abrir**.

## <a name="add-a-remote-resource"></a>Adición de recursos remotos
Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados mediante Conexión de RemoteApp y Escritorio.

- La dirección URL muestra el vínculo al servidor de Acceso web a Escritorio remoto que proporciona acceso a Conexión de RemoteApp y Escritorio.
- Se enumeran los configurado conexiones de Conexión de RemoteApp y Escritorio configuradas.

Para agregar un recurso remoto:

1. En Connection Center, haz clic en **+** y, luego, en **Agregar recursos remotos**. 
2. Escribe información para el recurso remoto:
   - **Dirección URL de fuente**: la dirección URL del servidor de Acceso web a Escritorio remoto. En este campo también puedes escribir la cuenta de correo electrónico corporativa (esto indica al cliente que busque el servidor de Acceso web a Escritorio remoto asociado con la dirección de correo electrónico).
   - **Nombre de usuario**: el nombre de usuario que se usa para servidor de Acceso web a Escritorio remoto al que se conecta.
   - **Contraseña**la contraseña que se usa para servidor de Acceso web a Escritorio remoto al que se conecta.
3. Haga clic en **Guardar**.


Los recursos remotos se mostrarán en Connection Center.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conexión a una puerta de enlace de Escritorio remoto para acceder a recursos internos

Las puertas de enlace de Escritorio remoto te permiten conectarte a equipos remotos de una red corporativa desde cualquier lugar de Internet. Puedes crear y administrar las puertas de enlace en las preferencias de la aplicación o mientras configuras una nueva conexión de escritorio.

Para configurar una puerta de enlace nueva en las preferencias:

1. En Connection Center, haz clic en **Preferencias > Puertas de enlace**. 
2. Haz clic en el botón **+** que aparece en la parte inferior de la tabla y escribe esta información:
   - **Nombre del servidor**: el nombre del equipo que desea usar como puerta de enlace. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Nombre de usuario**: el nombre de usuario y la contraseña que se van a usar para la puerta de enlace de Escritorio remoto a la que se va a conectar. También puedes seleccionar **Usar credenciales de conexión** para usar el mismo nombre de usuario y la misma contraseña que para la conexión a Escritorio remoto.


## <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando te conectas a un escritorio o a recursos remotos, puedes guardar las cuentas de usuario para volver a seleccionarlas más adelante. Las cuentas de usuario se pueden administrar mediante el cliente de Escritorio remoto.

Para crear una cuenta de usuario:

1. En Connection Center, haz clic en **Configuración** > **Cuentas**.
2. Haz clic en **Agregar cuenta de usuario**.
3. Escribe la siguiente información:
   - **Nombre de usuario**: el nombre del usuario que se guarda para usarlo con una conexión remota. El nombre de usuario se puede escribir en cualquiera de los siguientes formatos: nombre_de_usuario, dominio\nombre_de_usuario o user_name@domain.com.
   - **Contraseña**: la contraseña del usuario que has especificado. Todas las cuentas de usuario que quieras guardar para usarlas para las conexiones remotas deben tener una contraseña asociada.
   - **Nombre descriptivo**: si usas la misma cuenta de usuario con distintas contraseñas, establece un nombre descriptivo para distinguir dichas cuentas de usuario.
4. Pulsa **Guardar** y, después, **Configuración**.

## <a name="customize-your-display-resolution"></a>Personalización de la resolución de pantalla
Puedes especificar la resolución de pantalla de la sesión de Escritorio remoto.

1. En Connection Center, haz clic en **Preferencias**.
2. Haz clic en **Resolución**. 
3. Haz clic en **+** .
4. Escribe la altura y el ancho de una resolución y, luego, haz clic en **Aceptar**.

Para eliminar la resolución, selecciónala y haz clic en **-** .

**Displays have separate spaces** (Las pantallas tienen espacios separados). Si ejecutas Mac OS X 10.9 y deshabilitaste **Displays have separate spaces** (Las pantallas tienen espacios separados) en Mavericks (**Preferencias del sistema > Control de misiones**), debes establecer esta configuración en el cliente de Escritorio remoto con la misma opción.

### <a name="drive-redirection-for-remote-resources"></a>Redireccionamiento de unidades para los recursos remotos
El redireccionamiento de unidades se admite para los recursos remotos de modo que puedas guardar los archivos creados con una aplicación remota de manera local en tu equipo Mac. La carpeta redirigida siempre será el directorio principal que se muestra como una unidad de red en la sesión remota.

> [!NOTE]
> Para usar esta característica, el administrador debe establecer la configuración adecuada en el servidor.


## <a name="use-a-keyboard-in-a-remote-session"></a>Uso de un teclado en una sesión remota

Las distribuciones del teclado Mac son distintas de las distribución del teclado Windows. 

- La tecla de comando del teclado Mac es equivalente a la tecla Windows.
- Para llevar a cabo las acciones que usan el botón de comando en Mac, deberás usar el botón de control en Windows (por ejemplo, Copiar = Ctrl + C).
- Para activar las teclas de función en la sesión, debes presionar además la tecla FN (por ejemplo, FN + F1).
- La tecla Alt que está a la derecha de la barra espaciadora en el teclado Mac equivale a la tecla Alt Gr/Alt derecho en Windows.

De manera predeterminada, la sesión remota usará la misma configuración regional del teclado que usa el sistema operativo donde se ejecuta el cliente. Si el equipo Mac ejecuta un sistema operativo en en-us, esa configuración regional se usará también para las sesiones remotas. Si no se usa la configuración regional del teclado del sistema operativo, comprueba la configuración del teclado del equipo remoto y cámbiala manualmente. Consulta las [Preguntas más frecuentes del cliente de Escritorio remoto](remote-desktop-client-faq.md) para más información sobre los teclados y las configuraciones regionales.


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Compatibilidad con la autorización y autenticación acoplables de la puerta de enlace de Escritorio remoto

Windows Server 2012 R2 introdujo la compatibilidad con un nuevo método de autenticación, la autorización y autenticación acoplables de la puerta de enlace de Escritorio remoto, que proporciona más flexibilidad para las rutinas de autenticación personalizadas. Ahora puedes usar este modelo de autenticación con el cliente de Mac. 

> [!IMPORTANT]
> No se admiten los modelos personalizados de autenticación y autorización antes de Windows 8.1, a pesar de que se analizan en el artículo mencionado.

Para más información sobre esta característica, revisa [http://aka.ms/paa-sample](http://aka.ms/paa-sample).


> [!TIP]
> Tanto las preguntas como los comentarios son siempre bienvenidos. Sin embargo, NO publiques una solicitud de ayuda para solucionar problemas con la característica de comentario del final de este artículo. En su lugar, ve al [foro del cliente de Escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicia una nueva conversación. ¿Tienes alguna sugerencia de característica? Realízala en el [foro de usuarios de clientes](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

