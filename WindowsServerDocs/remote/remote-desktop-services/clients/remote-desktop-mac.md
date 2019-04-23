---
title: Introducción a Escritorio remoto en Mac
description: Obtenga información sobre cómo configurar el cliente de escritorio remoto para Mac
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
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: e8c5da1960d0e3129b5520e65c2d5ecf45eef778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886956"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>Introducción a Escritorio remoto en Mac

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Puede usar al cliente de escritorio remoto para Mac para trabajar con aplicaciones, recursos y equipos de escritorio de Windows desde el equipo Mac. Use la siguiente información para comenzar - y desproteger la [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tiene alguna pregunta.

>[!Note]
> - ¿Tiene curiosidad acerca de las nuevas versiones para el cliente de macOS? ¿Consulte [Novedades de escritorio remoto en Mac?](mac-whatsnew.md)
> - Se ejecuta el cliente Mac en equipos que ejecutan macOS 10.10 y versiones más recientes.
> - La información de este artículo se aplica principalmente a la versión completa del cliente Mac - la versión disponible en el App Store de Mac. Pruebe las nuevas características mediante la descarga aquí nuestra aplicación de vista previa: [notas de la versión beta cliente](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Obtener al cliente de escritorio remoto
Siga estos pasos para empezar a trabajar con Escritorio remoto en el equipo Mac:

1. Descargue el cliente de escritorio remoto de Microsoft desde el [Mac App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configuración de su PC para aceptar conexiones remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Si se omite este paso, no se puede conectar a su equipo.)
3. Agregar una conexión a Escritorio remoto o un recurso remoto. Usar una conexión para conectarse directamente a un equipo de Windows y un recurso remoto para utilizar un programa RemoteApp, basados en sesión de escritorio o un escritorio virtual publicada en el entorno local mediante la conexión de RemoteApp y escritorio. Esta característica está disponible normalmente en entornos corporativos.

## <a name="what-about-the-mac-beta-client"></a>¿Qué sucede con el cliente de Mac beta?
Vamos a probar las nuevas características en nuestro canal de versión preliminar en HockeyApp. ¿Desea echarle un vistazo? Vaya a [escritorio remoto de Microsoft para Mac](https://go.microsoft.com/fwlink/?LinkID=619698) y haga clic en **descargar**. No es necesario crear una cuenta o inicio de sesión en HockeyApp para descargar al cliente de la versión beta.

Si ya tiene el cliente, puede comprobar las actualizaciones para asegurarse de que tiene la versión más reciente. En el cliente de la versión beta, haga clic en **Microsoft Remote Desktop Beta** en la parte superior y, a continuación, haga clic en **buscar actualizaciones**. 

## <a name="add-a-remote-desktop-connection"></a>Agregar una conexión a Escritorio remoto
Para crear una conexión a escritorio remota:

1. En el centro de conexiones, haga clic en **+** y, a continuación, haga clic en **Desktop**.
2. Escriba la siguiente información:
   - **Nombre de equipo** -el nombre del equipo.
      - Esto puede ser un nombre de equipo de Windows (se encuentra en la **sistema** configuración), un nombre de dominio o una dirección IP.
      - También puede agregar información de puerto al final de este nombre, como *MyDesktop:3389*.
   - **Cuenta de usuario** -agregar la cuenta de usuario que usa para obtener acceso al equipo remoto.
      - Los equipos o cuentas locales, Unidos a Active Directory (AD), use uno de estos formatos: *user_name*, *DOMINIO\nombre_de_usuario*, o *user_name@domain.com*.
      - Para Azure Active Directory (AAD) unido los equipos, use uno de estos formatos: *AzureAD\user_name* o *AzureAD\user_name@domain.com*.
      - También puede elegir si se requiere una contraseña.
      - Al administrar varias cuentas de usuario con el mismo nombre de usuario, establezca un nombre descriptivo para diferenciar las cuentas.
      - Administrar sus cuentas de usuario guardadas en las preferencias de la aplicación. 

3. También puede establecer estos valores opcionales para la conexión:
   - Establezca un nombre descriptivo 
   - Agregar una puerta de enlace
   - Establezca la salida de sonido
   - Intercambiar botones del mouse
   - Habilitar el modo de administrador
   - Redirección de carpetas locales en una sesión remota
   - Impresoras locales hacia delante
   - Tarjetas inteligentes hacia delante
4. Haga clic en **Guardar**.

Para iniciar la conexión, doble clic en él. Lo mismo puede decirse de los recursos remotos.

### <a name="export-and-import-connections"></a>Exportar e importar las conexiones
Puede exportar una definición de conexión a Escritorio remoto y usarla en un dispositivo diferente. Escritorios remotos se guardan de forma independiente. Archivos RDP.

1. En el centro de conexiones, haga clic en el escritorio remoto.
2. Haga clic en **Exportar**.
3. Vaya a la ubicación donde desea guardar el escritorio remoto. Archivo RDP.
4. Haga clic en **Aceptar**.

Siga estos pasos para importar un escritorio remoto. Archivo RDP.

1. En la barra de menús, haga clic en **archivo > Importar**.
2. Vaya a la. Archivo RDP.
3. Haga clic en **Abrir**.

## <a name="add-a-remote-resource"></a>Agregar un recurso remoto
Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados utilizando la conexión de RemoteApp y escritorio.

- La dirección URL muestra el vínculo en el servidor de acceso Web de escritorio remoto que proporciona acceso a conexión de RemoteApp y escritorio.
- Se enumeran los configurado conexiones de RemoteApp y escritorio.

Para agregar un recurso remoto:

1. Haga clic en Centro de conexiones **+** y, a continuación, haga clic en **agregar recursos remotos**. 
2. Escriba la información para el recurso remoto:
   - **Dirección URL de fuente** -la dirección URL del servidor de acceso Web de escritorio remoto. También puede especificar la cuenta de correo electrónico corporativo en este campo: Esto indica al cliente para buscar el servidor de acceso Web de escritorio remoto asociado con su dirección de correo electrónico.
   - **Nombre de usuario** -el nombre de usuario que se usará para el servidor de acceso Web de escritorio remoto que se conecta.
   - **Contraseña** -la contraseña que se usará para el servidor de acceso Web de escritorio remoto que se conecta.
3. Haga clic en **Guardar**.


Los recursos remotos se mostrará en el centro de conexiones.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectarse a una puerta de enlace de escritorio remoto para tener acceso a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de RD) le permite conectarse a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puede crear y administrar las puertas de enlace en las preferencias de la aplicación o al configurar una nueva conexión de escritorio.

Para configurar una puerta de enlace en Preferencias:

1. En el centro de conexiones, haga clic en **Preferencias > puertas de enlace**. 
2. Haga clic en el **+** situado en la parte inferior de la tabla, escriba la siguiente información:
  - **Nombre del servidor** : el nombre del equipo que desea usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede agregar información de puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
  - **Nombre de usuario** -el nombre de usuario y la contraseña que se usará para la puerta de enlace de escritorio remoto que se conecta. También puede seleccionar **usar las credenciales de conexión** para usar el mismo nombre de usuario y la contraseña que se utilizaron para la conexión a escritorio remota.


## <a name="manage-your-user-accounts"></a>Administrar las cuentas de usuario

Cuando se conecta a un recursos remotos o de escritorio, puede guardar las cuentas de usuario para seleccionar de nuevo. Puede administrar sus cuentas de usuario mediante el cliente de escritorio remoto.

Para crear una nueva cuenta de usuario:

1. En el centro de conexiones, haga clic en **configuración** > **cuentas**.
2. Haga clic en **Agregar cuenta de usuario**.
3. Escriba la siguiente información:
   - **Nombre de usuario** -el nombre del usuario para guardar para su uso con una conexión remota. Puede escribir el nombre de usuario en cualquiera de los siguientes formatos: user_name, DOMINIO\nombre_de_usuario, o user_name@domain.com.
   - **Contraseña** -la contraseña para el usuario especificado. Cada cuenta de usuario que desea guardar si desea para utilizar para las conexiones remotas debe tener una contraseña asociada con él.
   - **Nombre descriptivo** : si usa la misma cuenta de usuario con contraseñas diferentes, establezca un nombre descriptivo para distinguir las cuentas de usuario.
4. Pulse **guardar**y, a continuación, puntee en **configuración**.

## <a name="customize-your-display-resolution"></a>Personalizar la resolución de pantalla
Puede especificar la resolución de pantalla de la sesión de escritorio remota.

1. En el centro de conexiones, haga clic en **preferencias**.
2. Haga clic en **resolución**. 
3. Haga clic en **+**.
4. Escriba una altura de resolución y el ancho y, a continuación, haga clic en **Aceptar.**

Para eliminar la resolución, selecciónelo y, a continuación, haga clic en **-**.

**Las pantallas tienen espacios separados** si está ejecutando Mac OS X 10.9 y deshabilitado **pantallas tienen espacios separados** en Mavericks (**preferencias del sistema > Centro de Control**), deberá configurar Esta configuración en el cliente de escritorio remoto con la misma opción.

### <a name="drive-redirection-for-remote-resources"></a>Redirección de unidad para los recursos remotos
Se admite redirección de unidad para los recursos remotos, para que pueda guardar los archivos creados con una aplicación remota localmente en el equipo Mac. La carpeta redirigida siempre es el directorio principal que se muestra como una unidad de red en la sesión remota.

> [!NOTE]
> Para poder usar esta característica, el administrador debe establecer la configuración adecuada en el servidor.


## <a name="use-a-keyboard-in-a-remote-session"></a>Usar un teclado en una sesión remota

Distribuciones de teclado de Mac difieren de las distribuciones de teclado de Windows. 

- La tecla de comando en el teclado de Mac es igual a la clave de Windows.
- Para llevar a cabo las acciones que use el botón de comando en el equipo Mac, deberá usar el botón de control de Windows (p. ej.: Copiar = Ctrl + C).
- Las teclas de función se pueden activar en la sesión además la tecla FN (p. ej.: FN + F1).
- La tecla Alt a la derecha de la barra espaciadora en el teclado de Mac es igual a la tecla Alt derecha/Gr de Alt en Windows.

De forma predeterminada, la sesión remota usará la misma configuración regional de teclado como el sistema operativo que se ejecuta en el cliente. (Si el equipo Mac se está ejecutando un en-us del sistema operativo, que se usará para las sesiones remotas, así. Si no se usa la configuración regional de teclado de sistema operativo, compruebe el teclado, establecer en el equipo remoto y cambiar la configuración manualmente. Consulte la [P+F del cliente de escritorio remoto](remote-desktop-client-faq.md) para obtener más información acerca de teclados y configuraciones regionales.


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Compatibilidad para la autorización y autenticación acoplable de puerta de enlace de escritorio remoto

Windows Server 2012 R2 introdujo la compatibilidad con un nuevo método de autenticación, autenticación acoplable de puerta de enlace de escritorio remoto y la autorización, que proporciona más flexibilidad para las rutinas de autenticación personalizada. Ahora puede este modelo de autenticación con el cliente de Mac. 

> [!IMPORTANT]
> No se admiten modelos personalizados de autenticación y autorización antes de Windows 8.1, aunque la información de este artículo se describe cómo ellos.

Para obtener más información sobre esta característica, consulte [ http://aka.ms/paa-sample ](http://aka.ms/paa-sample).


> [!TIP]
> Preguntas y comentarios siempre son bienvenidos. Sin embargo, no publique una solicitud de ayuda para solucionar problemas con la característica de comentario al final de este artículo. En su lugar, vaya a la [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene alguna sugerencia de característica? Díganos en el [foro de uservoice cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

