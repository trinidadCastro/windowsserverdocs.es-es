---
title: Introducción a Escritorio remoto en Mac
description: Obtén información sobre cómo configurar el cliente de escritorio remoto para Mac
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
ms.sourcegitcommit: 6dc14d4315793132488494b5543ee83e3f562f09
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "4555722"
---
# Introducción a Escritorio remoto en Mac

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2 y Windows Server 2016

Puedes usar al cliente de escritorio remoto para Mac para trabajar con las aplicaciones, los recursos y los equipos de escritorio de Windows desde un equipo Mac. Usa la siguiente información para empezar a trabajar y echa un vistazo a las [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tienes preguntas.

>[!Note]
> - ¿Curiosidad acerca de las nuevas versiones de cliente de macOS? Echa un vistazo a [Novedades para escritorio remoto en Mac?](mac-whatsnew.md)
> - El cliente de Mac se ejecuta en los equipos que ejecutan macOS 10.10 y versiones más recientes.
> - La información de este artículo se aplica principalmente a la versión completa de cliente de Mac - la versión disponible en la AppStore Mac. Pruebe las nuevas características mediante la descarga de nuestra aplicación de vista previa aquí: [notas de la versión de cliente de la versión beta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## Obtener al cliente de escritorio remoto
Sigue estos pasos para empezar a trabajar con Escritorio remoto en un Mac:

1. Descargar al cliente de escritorio remoto de Microsoft de la [Tienda de la aplicación de Mac](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configurar tu PC para que acepte las conexiones remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Si se omite este paso, no puedes conectarte a tu PC.)
3. Agregar una conexión a Escritorio remoto o un recurso remoto. Usar una conexión para conectarse directamente a un equipo con Windows y un recurso remoto para usar un programa de RemoteApp, basada en la sesión de escritorio o un equipo de escritorio virtual publicado local con conexión de RemoteApp y escritorio. Esta característica es suele estar disponible en entornos corporativos.

## ¿Qué sucede con el cliente de la versión beta de Mac?
Estamos probando nuevas características en nuestro canal de vista previa en HockeyApp. ¿Quieres echa un vistazo a? Ve a [Escritorio remoto de Microsoft para Mac](https://go.microsoft.com/fwlink/?LinkID=619698) y haz clic en **Descargar**. No tienes que crear una cuenta o el inicio de sesión en HockeyApp para descargar al cliente de la versión beta.

Si ya tienes el cliente, puedes comprobar las actualizaciones de asegurarte de que tener la versión más reciente. En el cliente de la versión beta, haz clic en la **Versión Beta de escritorio remoto de Microsoft** en la parte superior y, a continuación, haz clic en **Buscar actualizaciones**. 

## Agregar una conexión a Escritorio remoto
Para crear una conexión a Escritorio remoto:

1. En el centro de conexión, haz clic en **+** y, a continuación, haz clic en el **escritorio**.
2. Escribe la siguiente información:
   - **Nombre de equipo** : el nombre del equipo.
      - Esto puede ser un nombre de equipo de Windows (que se encuentra en la configuración **del sistema** ), un nombre de dominio o una dirección IP.
      - También puedes agregar información de puerto al final de este nombre, como *MyDesktop:3389*.
   - **Cuenta de usuario** : agregar la cuenta de usuario que usas para acceder al equipo remoto.
      - Para los equipos o cuentas locales, Unidos a Active Directory (AD), usa uno de estos formatos: *nombre de usuario*, *DOMINIO\nombre_usuario*o *user_name@domain.com*.
      - Para los equipos unidos a la Azure Active Directory (AAD), usa uno de estos formatos: *AzureAD\user_name* o *AzureAD\ user_name@domain.com*.
      - También puedes elegir si se requiere una contraseña.
      - Al administrar varias cuentas de usuario con el mismo nombre de usuario, establece un nombre descriptivo para diferenciar las cuentas.
      - Administrar las cuentas de usuario guardado en las preferencias de la aplicación. 

3. También puedes establecer estas opciones de configuración opcionales para la conexión:
   - Establece un nombre descriptivo 
   - Agregar una puerta de enlace
   - Establece la salida de sonido
   - Botones del mouse de intercambio
   - Habilitar el modo de administrador
   - Redirección de carpetas locales en una sesión remota
   - Impresoras locales hacia delante
   - Tarjetas inteligentes adelantada
4. Haz clic en **Guardar**.

Para iniciar la conexión, doble clic en él. El mismo es true para recursos remotos.

### Exportar e importar las conexiones
Puede exportar una definición de la conexión a Escritorio remoto y usar en otro dispositivo. Equipos de escritorio remotos se guardan en diferentes. Archivos RDP.

1. En el centro de conexión, haz clic en el escritorio remoto.
2. Haz clic en **Exportar**.
3. Busca la ubicación donde quieres guardar el escritorio remoto. Archivo RDP.
4. Haz clic en **Aceptar**.

Usar los siguientes pasos para importar un escritorio remoto. Archivo RDP.

1. En la barra de menús, haz clic en **archivo > Importar**.
2. Busca el. Archivo RDP.
3. Haz clic en **Abrir**.

## Agregar un recurso remoto
Recursos remotos son los programas de RemoteApp, equipos de escritorio basada en la sesión y escritorios virtuales publicados con RemoteApp y escritorio.

- La dirección URL muestra el vínculo en el servidor de acceso Web de escritorio remoto que proporciona acceso a RemoteApp y escritorio.
- Se enumeran las conexiones de escritorio y RemoteApp configurado.

Para agregar un recurso remoto:

1. En haga clic en el centro de conexión **+** y, a continuación, haz clic en **Agregar recursos remotos**. 
2. Escribe la información de los recursos remotos:
   - **Dirección URL de fuente** - la dirección URL del servidor de acceso Web de escritorio remoto. También puedes escribir tu cuenta de correo electrónico corporativos en este campo, esto indica que el cliente para buscar el servidor de acceso Web de escritorio remoto asociado con la dirección de correo electrónico.
   - **Nombre de usuario** : el nombre de usuario que se usará para el servidor de acceso Web de escritorio remoto a que se conecta.
   - **Contraseña** : la contraseña que se usará para el servidor de acceso Web de escritorio remoto a que se conecta.
3. Haz clic en **Guardar**.


Los recursos remotos se mostrará en el centro de conexión.


## Conectarse a una puerta de enlace de escritorio remoto para acceder a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto) te permite conectarse a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puedes crear y administrar las puertas de enlace en las preferencias de la aplicación o al configurar una nueva conexión de escritorio.

Para configurar una puerta de enlace nuevo en las preferencias:

1. En el centro de conexión, haz clic en **Preferencias > puertas de enlace**. 
2. Haz clic en el **+** botón en la parte inferior de la tabla entrar la siguiente información:
  - **Nombre del servidor** : el nombre del equipo que quieras usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información de puerto para el nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
  - **Nombre de usuario** : el nombre de usuario y contraseña que se usará para la puerta de enlace de escritorio remoto a que se conecta. También puedes seleccionar **Usar credenciales de conexión** para usar el mismo nombre de usuario y la contraseña como los que se usan para la conexión a Escritorio remoto.


## Administrar las cuentas de usuario

Cuando se conecta a recursos de escritorio o control remoto, puedes guardar las cuentas de usuario se selecciona de nuevo. Puedes administrar las cuentas de usuario mediante el cliente de escritorio remoto.

Para crear una nueva cuenta de usuario:

1. En el centro de conexión, haz clic en **configuración** > **cuentas**.
2. Haz clic en **Agregar cuenta de usuario**.
3. Escribe la siguiente información:
   - **Nombre de usuario** : el nombre del usuario para guardar para su uso con una conexión remota. Puedes escribir el nombre de usuario en cualquiera de los siguientes formatos: nombre de usuario, DOMINIO\nombre_usuario, o user_name@domain.com.
   - **Contraseña** : la contraseña para el usuario especificado. Cada cuenta de usuario que quieres guardar para usar para las conexiones remotas debe tener una contraseña asociada con ella.
   - **Nombre descriptivo** : Si estás usando la misma cuenta de usuario con diferentes contraseñas, establece un nombre descriptivo para distinguir las cuentas de usuario.
4. Pulsa **Guardar**y, a continuación, pulsa **Opciones de configuración**.

## Personalizar la resolución de pantalla
Puedes especificar la resolución de pantalla de la sesión de escritorio remota.

1. En el centro de conexión, haz clic en **las preferencias**.
2. Haz clic en **resolución**. 
3. Haz clic en **+**.
4. Escribe un alto de resolución y el ancho y, a continuación, haz clic en **Aceptar.**

Para eliminar la resolución, selecciónalo y, a continuación, haz clic en **-**.

**Las pantallas tienen espacios independientes** Si estás ejecutando Mac OS X 10.9 y deshabilitado **las pantallas tienen espacios independientes** en Mavericks (**Preferencias del sistema > Control de misiones**), debes configurar esta configuración en el cliente de escritorio remoto con la opción de la misma.

### Redirección de unidad para recursos remotos
Redirección de la unidad es compatible con recursos remotos, por lo que puedes guardar los archivos creados con una aplicación remota localmente tu Mac. La carpeta redirigida siempre es el directorio principal que se muestra como una unidad de red en la sesión remota.

> [!NOTE]
> Para poder usar esta característica, el administrador necesita establecer la configuración adecuada en el servidor.


## Usar un teclado en una sesión remota

Diseños de teclado de Mac difieren de los diseños de teclado de Windows. 

- Las teclas de comando del teclado de Mac es igual a la clave de Windows.
- Para realizar acciones que usan el botón de comando en el Mac, tendrás que usar el botón de control de Windows (por ejemplo: copia = Ctrl + C).
- Se pueden activar las teclas de función en la sesión, además presionando la tecla FN (p. ej.: FN + F1).
- La tecla Alt a la derecha de la barra espaciadora en el teclado de Mac es igual a la tecla Alt Alt Gr/derecha en Windows.

De manera predeterminada, la sesión remota usará como el sistema operativo que estás ejecutando al cliente en la misma configuración regional de teclado. (Si tu Mac se está ejecutando una en-us del sistema operativo, que se usará para las sesiones de remoto. Si no se usa la configuración regional de teclado de sistema operativo, comprueba el teclado, la configuración en el equipo remoto y cambiar la configuración de forma manual. Consulta las [Preguntas frecuentes de cliente de escritorio remoto](remote-desktop-client-faq.md) para obtener más información acerca de los teclados y configuraciones regionales.


## Compatibilidad con autorización y autenticación conectable de puerta de enlace de escritorio remoto

Windows Server 2012 R2 introdujo compatibilidad para un nuevo método de autenticación, autenticación conectables de puerta de enlace de escritorio remoto y autorización, que proporciona más flexibilidad para las rutinas de autenticación personalizada. Ahora puedes este modelo de autenticación con el cliente de Mac. 

> [!IMPORTANT]
> No se admiten los modelos de autenticación y autorización personalizados antes de Windows 8.1, aunque el artículo anterior se describe cómo les.

Para obtener más información acerca de esta característica, echa un vistazo a [http://aka.ms/paa-sample](http://aka.ms/paa-sample).


> [!TIP]
> Preguntas y comentarios siempre son inicio de sesión. Sin embargo, no envíe una solicitud de ayuda para solucionar problemas mediante el uso de la característica de comentarios al final de este artículo. En su lugar, ve al [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene una sugerencia de la característica? Nos indicará en el [foro de voz del usuario de cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

