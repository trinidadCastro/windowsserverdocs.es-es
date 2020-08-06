---
title: Administración de medios digitales en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c59fd121fdf628fc0943214b699599f2f20625b3
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837834"
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Administración de medios digitales en Windows Server Essentials

>Se aplica a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En los siguientes temas se abordan las características de transmisión por secuencias de multimedia del servidor y se explica cómo configurar y usar la transmisión por secuencias de multimedia en la red.

> [!NOTE]
>  De forma predeterminada, la funcionalidad de transmisión por secuencias de multimedia no está disponible en Windows Server Essentials y Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado. Para agregar la funcionalidad de streaming multimedia en estas versiones, [descargue e instale el Módulo de medios de Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40837) en el Centro de descarga de Microsoft.

-   [Información general de los medios digitales](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)

-   [Administrar el servidor multimedia mediante el panel](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)

-   [Cómo funciona la transmisión por secuencias de multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)

-   [Activar o desactivar la transmisión por secuencias](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)

-   [Agregar archivos multimedia digitales al servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)

-   [Permitir o restringir el acceso a una biblioteca multimedia en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)

-   [Cambiar el nombre de la biblioteca multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)

-   [Detener el uso compartido de medios digitales](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)

-   [Permitir que los dispositivos multimedia que usan el protocolo de bloque de mensajes del servidor (SMB) puedan tener acceso a los archivos compartidos en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)

-   [Procesadores comunes y perfiles de vídeo compatibles](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)

-   [Problemas conocidos de los tipos de archivos multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)

##  <a name="digital-media-overview"></a><a name="BKMK_1"></a>Información general de los medios digitales
 Por medios digitales se hace referencia al audio, vídeo y contenido de fotos que se han codificado (comprimido digitalmente). La codificación de contenido implica convertir la entrada de audio y vídeo a un archivo de medios digitales, como por ejemplo un archivo de Windows Media. Una vez codificados los medios digitales, estos pueden manipularse, distribuirse y reproducirse en equipos con facilidad, y se transmiten fácilmente a través de redes de equipos.

 Ejemplos de tipos de medios digitales: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG y AVI. Para obtener información acerca de los tipos de medios digitales compatibles con el Reproductor de Windows Media, consulte [Tipos de archivos compatibles con el Reproductor de Windows Media](https://support.microsoft.com/kb/316992).

### <a name="why-would-i-want-to-stream-my-digital-media"></a>¿Por qué debería transmitir mis medios digitales?
 Muchas personas almacenan música, vídeo e imágenes en carpetas compartidas en Windows Server Essentials. A veces, puede desear hacer lo siguiente:

-   **Ver vídeos**. El servidor se puede usar para almacenar y transmitir grandes colecciones de vídeos y espectáculos de TV grabados en los equipos o en otros dispositivos de reproducción de la red. Puede transmitir vídeos a Xbox 360 o a un equipo con el Reproductor de Windows Media.

-   **Reproducir música**. Al activar el uso compartido de multimedia de la carpeta compartida **Música** puede acceder a la música desde dispositivos que admitan Windows Media Connect. No es necesario habilitar o configurar las cuentas de usuario para realizar transmisiones desde la carpeta compartida **Música** después de activar el uso compartido.

-   **Mostrar presentaciones fotográficas**. Puede almacenar fotos digitales en la carpeta compartida **Fotos** en el servidor y acceder a ellas desde cualquier equipo o desde una Xbox 360 que esté conectada a un televisor de su casa u oficina. Puede ver presentaciones fotográficas, que es como activar el televisor en un gran marco de fotos.

###  <a name="sharing-copy-protected-media"></a><a name="BKMK_1.5"></a>Uso compartido de medios protegidos contra copia
  Windows Server Essentials no admite el uso compartido de medios protegidos contra copia. Esto incluye la música adquirida a través de una tienda de música en línea.

 Los medios protegidos contra copia solo se pueden reproducir en el equipo o dispositivo que ha usado para comprarlos. La protección contra copia impide reproducir archivos multimedia en más de un equipo o dispositivo, incluso si copia los medios al servidor y los reproduce desde allí. Sin embargo, puede almacenar los medios protegidos contra copia en Windows Server Essentials y seguir reproduciendo el medio en el equipo o dispositivo que usó para comprarlo.

##  <a name="manage-media-server-using-the-dashboard"></a><a name="BKMK_2"></a>Administrar el servidor multimedia mediante el panel
  Windows Server Essentials permite realizar tareas administrativas comunes mediante el panel de Windows Server Essentials. En el panel, la pestaña **Multimedia** de la página **Configuración** del servidor ofrece las siguientes funciones:

|Sección|Funcionalidad|
|-------------|-------------------|
|Servidor multimedia|El botón **Activar o desactivar** le permite activar o desactivar la transmisión por secuencias.|
|Calidad de transmisión por secuencias de vídeo|La flecha desplegable le permite elegir la calidad de transmisión por secuencias de los vídeos que se reproducen desde el servidor.|
|Biblioteca multimedia|Muestra el nombre de la biblioteca multimedia. El nombre predeterminado de la biblioteca se denomina **Biblioteca multimedia digital**, que se crea al activar la transmisión por secuencias de multimedia. Para cambiar el nombre de la biblioteca multimedia, vea el tema sobre cómo [cambiar el nombre de la biblioteca multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). Puede hacer clic en **Personalizar** para personalizar las carpetas compartidas en la biblioteca multimedia.|

 Para obtener más información, vea [Permitir o restringir el acceso a una biblioteca multimedia en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) y [Compartir medios protegidos contra copia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).

##  <a name="how-media-streaming-works"></a><a name="BKMK_3"></a>Cómo funciona la transmisión por secuencias de multimedia
 La característica de transmisión por secuencias de multimedia de Windows Server Essentials permite que los equipos conectados en red y algunos dispositivos de medios digitales en red puedan reproducir archivos multimedia digitales que se almacenan en el servidor.

 Al activar el servidor multimedia, el contenido que comparta en las bibliotecas multimedia podrán reproducirse en los dispositivos de la red que puedan recibir medios de transmisión desde el servidor. Puede transmitir la mayoría de los tipos de archivos multimedia digitales. Algunos de los tipos de archivos más habituales que puede transmitir son:

- Formatos de Windows Media (.asf, .wma, .wmv, .wm)

- Audio Visual Interleave (.avi)

- Moving Pictures Experts Group (.mpeg, .mpg, .mp3)

- Audio para Windows (.wav)

- Pista de audio de CD (.cda)

  Para reproducir un archivo, busque una canción, un vídeo o una imagen en una carpeta compartida, haga doble clic en el archivo y el contenido se transmitirá del servidor al equipo y se reproducirá. Para obtener información acerca de cómo buscar y reproducir los archivos multimedia digitales almacenados en el servidor, vea [reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).

  Para transmitir los medios necesita el siguiente hardware:

- Una red privada con cable o inalámbrica

- Otro equipo de la red o un dispositivo conocido como receptor de medios digitales (también conocido como reproductor multimedia digital en red). Los receptores de medios digitales son dispositivos de hardware conectados a la red cableada o inalámbrica que puede controlar mediante el equipo, incluso si el equipo está en otra habitación.

  Para obtener más información, vea [activar o desactivar la transmisión por secuencias de multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).

##  <a name="turn-media-streaming-on-or-off"></a><a name="BKMK_4"></a>Activar o desactivar la transmisión por secuencias de multimedia
 Puede compartir música, vídeos e imágenes de Windows Server Essentials mediante el streaming de archivos en cualquier receptor de medios digitales (DMR) admitido, como equipos, teléfonos móviles, televisores, receptores de medios digitales, extensores para Windows Media Center (incluido Xbox 360) y otros dispositivos electrónicos personales.

 Para obtener una lista actualizada de los dispositivos de medios digitales compatibles con Windows Server Essentials, vea el [centro de compatibilidad de Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).

### <a name="enabling-media-sharing"></a>Habilitar el uso compartido de multimedia
 Para compartir los medios que se almacenan en Windows Server Essentials, debe activar la transmisión por secuencias de multimedia. La transmisión por secuencias de multimedia está desactivada de manera predeterminada.

####  <a name="to-turn-media-streaming-on-or-off"></a><a name="BKMK_2.5"></a>Para activar o desactivar la transmisión por secuencias de multimedia

1. Abra el panel de Windows Server Essentials.

2. Haga clic en **Configuración**, haga clic en **Multimedia** y, a continuación, realice una de las acciones siguientes:

   -   Haga clic en **Activar** para iniciar el uso compartido de todos los archivos almacenados en la biblioteca multimedia del servidor.

   -   Haga clic en **Desactivar** para detener el uso compartido de todos los archivos almacenados en la biblioteca multimedia del servidor.

3. Si quiere compartir carpetas adicionales en la biblioteca multimedia, haga clic en **Personalizar** y, a continuación, seleccione **Sí** en cada carpeta compartida que quiera incluir en la biblioteca multimedia.

4. Haga clic en **Aceptar** para guardar los cambios.

   Para obtener información acerca de los tipos de medios digitales compatibles con el Reproductor de Windows Media, consulte [Tipos de archivo compatibles con el Reproductor de Windows Media](https://support.microsoft.com/kb/316992).

   Para obtener más información, vea [Permitir o restringir el acceso a una biblioteca multimedia en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).

##  <a name="add-digital-media-files-to-the-server"></a><a name="BKMK_5"></a>Agregar archivos multimedia digitales al servidor
 El administrador del servidor puede agregar medios digitales a las carpetas compartidas en la biblioteca multimedia accediendo directamente al servidor o usando el sitio de acceso Web remoto para iniciar sesión en el panel. Otros usuarios pueden agregar archivos multimedia al servidor mediante la conexión de **carpetas compartidas** del Launchpad, mediante el sitio de acceso Web remoto o mediante la aplicación my server para Windows Phone. Para obtener información acerca de la reproducción de medios, vea [reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).

> [!NOTE]
>  También puede cargar archivos multimedia al servidor mediante la aplicación My Server para Windows Phone. Puede descargar la aplicación My Server desde la [Tienda de Windows Phone](https://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Para obtener más información acerca de la aplicación My Server para Windows Phone, consulte la entrada de blog sobre la [aplicación de teléfono My Server para Windows Server Essentials](/archive/blogs/sbs/my-server-phone-app-for-windows-server-2012-essentials).

#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Para agregar archivos multimedia a las carpetas compartidas en el servidor

1.  Para iniciar sesión en el servidor use uno de los métodos siguientes:

    1.  Para obtener información sobre cómo iniciar sesión en Acceso Web remoto, vea [Iniciar sesión en Acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).

    2.  Para obtener información sobre cómo iniciar sesión con Launchpad, consulte [Introducción a Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).

2.  Busque y haga clic en la carpeta de los archivos multimedia que va a agregar.

3.  Copie y pegue o arrastre y coloque los archivos multimedia que quiere agregar a la carpeta compartida adecuada del servidor.

##  <a name="allow-or-restrict-access-to-a-media-library-on-the-server"></a><a name="BKMK_6"></a>Permitir o restringir el acceso a una biblioteca multimedia en el servidor

-   Al activar el uso compartido de multimedia se crean cuatro carpetas predefinidas: Música, imágenes, vídeos y TV grabada. Si cualquiera de estas carpetas ya existe en el servidor, se volverá a usar la carpeta existente como carpeta compartida para el uso compartido de multimedia. Se conservan todos los permisos de usuario y el contenido multimedia de la carpeta existente, y se comparten con todos los usuarios de la red.

-   Antes de activar el uso compartido de la biblioteca multimedia para una carpeta compartida, debe saber que el uso compartido de la biblioteca multimedia omite cualquier tipo de acceso de cuenta de usuario que haya establecido para la carpeta compartida. Por ejemplo, supongamos que activa el uso compartido de la biblioteca multimedia para la carpeta compartida **Fotos** y establece dicha carpeta compartida **Fotos** en **Sin acceso** para una cuenta de usuario denominada Enrique. Enrique aún puede transmitir los medios digitales desde la carpeta compartida **Vídeos** a cualquier reproductor multimedia digital o DMR admitido. Si tiene elementos multimedia digitales que no quiere transmitir de esta manera debe almacenar los archivos en una carpeta que no tenga activado el uso compartido de la biblioteca multimedia.

-   Si activa el uso compartido de la biblioteca multimedia para una carpeta compartida, cualquier reproductor multimedia digital o DMR admitido que pueda tener acceso a la red de Windows Server Essentials también podrá tener acceso a los medios digitales de esa carpeta compartida. Por ejemplo, si tiene una red inalámbrica y no está protegida, cualquier usuario dentro del alcance de su red inalámbrica podrá tener acceso a sus medios digitales de esa carpeta. Antes de activar el uso compartido de la biblioteca multimedia, asegúrese de que su red inalámbrica esté protegida. Para obtener más información, vea la documentación del punto de acceso inalámbrico.

##  <a name="rename-the-media-library"></a><a name="BKMK_8"></a>Cambiar el nombre de la biblioteca multimedia
 El nombre predeterminado de la biblioteca multimedia es **Servidor de medios digitales**. Se crea al activar el streaming multimedia en Windows Server Essentials. Para obtener más información acerca de cómo activar la transmisión por secuencias de multimedia, consulte [activar o desactivar la transmisión por secuencias de multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). Puede modificar el nombre de la biblioteca multimedia en cualquier momento con el panel del servidor.

#### <a name="to-rename-the-media-library"></a>Para cambiar el nombre de la biblioteca multimedia

1.  Abra el panel del servidor. Para abrir el panel desde Launchpad, haga clic en **Panel**, escriba la contraseña del servidor y, a continuación, haga clic en la flecha para iniciar sesión.

2.  En el panel del servidor, haga clic en **Configuración**.

3.  En la página **Configuración**, haga clic en la pestaña **Multimedia**.

4.  En la sección **Biblioteca multimedia** de la página **Configuración multimedia**, haga clic en el nombre de la biblioteca multimedia.

5.  En el cuadro de diálogo **Cambiar el nombre de la biblioteca multimedia**, escriba un nombre nuevo para la biblioteca multimedia y, a continuación, haga clic en **Aceptar**.

##  <a name="stop-sharing-digital-media"></a><a name="BKMK_9"></a>Detener el uso compartido de medios digitales
 El administrador del servidor puede detener el uso compartido de los medios digitales que se almacenan en carpetas compartidas en un servidor que ejecuta Windows Server Essentials.

#### <a name="to-stop-sharing-media-in-shared-folders"></a>Para detener el uso compartido de multimedia en las carpetas compartidas

1.  Abra el panel del servidor.

2.  En la página **Inicio** del panel, haga clic en **Configuración**, haga clic en **Configurar el servidor multimedia** y, a continuación, haga clic en **Haga clic para configurar el Servidor multimedia**.

3.  En la página de configuración **Multimedia** puede elegir una de las siguientes opciones:

    -   Haga clic en **Desactivar** para detener el uso compartido de todos los archivos almacenados en el servidor.

    -   Haga clic en **Personalizar** y seleccione **No** en las carpetas concretas que quiera dejar de compartir.

4.  Haga clic en **Aplicar** o en **Aceptar** para guardar los cambios.

##  <a name="enable-media-devices-that-use-the-server-message-block-smb-protocol-to-access-shared-files-on-the-server"></a><a name="BKMK_10"></a>Habilitar dispositivos multimedia que usan el protocolo de bloque de mensajes del servidor (SMB) para tener acceso a los archivos compartidos en el servidor
 Los dispositivos que usan el bloque de mensajes del servidor (SMB) para tener acceso a archivos y recursos compartidos de red en lugar de DLNA (para la transmisión por secuencias de multimedia) requieren que se active la cuenta Invitado. Esto permite que cualquier dispositivo o usuario de la red pueda ver el contenido de las carpetas compartidas sin autenticación.

> [!CAUTION]
>  Al habilitar la cuenta Invitado, cualquier usuario puede tener acceso a los recursos compartidos del servidor de manera predeterminada.

##  <a name="common-processors-and-the-video-profiles-they-support"></a><a name="BKMK_CommonProcessors"></a>Procesadores comunes y perfiles de vídeo que admiten
 Para transmitir multimedia desde el servidor de Windows Server Essentials, puede usar un equipo que ejecute el sistema operativo Windows 7 o Windows 8 u otros dispositivos en red (como reproductores multimedia digitales) o Media Center Extender (como Xbox 360). Cuando esté fuera de la red puede usar el reproductor multimedia de Acceso Web remoto para reproducir archivos almacenados en el servidor.

 Necesita una velocidad de transferencia de datos de entre 200 KBps y 10 MBps. Debe usar formatos multimedia que el equipo y los dispositivos puedan reconocer y reproducir. No todos los dispositivos admiten los mismos formatos multimedia, por lo que debe haber un método para que el equipo y los dispositivos puedan reproducir sus archivos multimedia.

  Windows Server Essentials contiene compatibilidad de transcodificación que determina la capacidad del equipo o dispositivo y, a continuación, convierte dinámicamente un archivo de vídeo no compatible en uno compatible. En general, si el Reproductor de Windows Media 12 puede reproducir el contenido en un equipo que ejecuta Windows 7 o Windows 8, el contenido del servidor se reproducirá en el dispositivo conectado a la red.

 El formato y la velocidad de bits elegidos para la transcodificación dependen en gran medida del rendimiento del procesador del servidor. El rendimiento del procesador se identifica como parte de la evaluación de experiencia de Windows. Para determinar los resultados de rendimiento del servidor realice una de las acciones siguientes:

- En un equipo de red que ejecute Windows 7 o Windows 8 con el mismo procesador que el servidor, vaya al **Panel de control**, haga clic en **información de rendimiento y herramientas**y, a continuación, revise la información de la página **velocidad y mejorar el rendimiento del equipo** .

- Póngase en contacto con el fabricante del procesador.

  Para lograr la mejor experiencia de usuario, elija una calidad de resolución de transmisión por secuencias de vídeo que sea adecuada para el procesador del servidor. El servidor ajustará automáticamente la velocidad de bits a uno de estos valores:

- **Bajo** si la calificación del procesador es inferior a 3,6.

- **Medio** si la calificación del procesador se encuentra entre 3,6 y 4,2.

- **Alto** si la calificación del procesador se encuentra entre 4,2 y 6,0.

- **Excelente** si la calificación del procesador es superior a 6,0.

  Si elige una resolución de transmisión por secuencias de vídeo que requiere más capacidad de procesamiento de la que tiene el servidor puede experimentar búferes e interrupciones durante la transmisión por secuencias de multimedia desde el servidor.

> [!NOTE]
>  Para transmitir vídeos de alta definición a través de Acceso Web remoto necesita un procesador con una puntuación de al menos 6,0.

##  <a name="known-issues-with-media-file-types"></a><a name="BKMK_KnownIssues"></a>Problemas conocidos de los tipos de archivos multimedia
 La característica de transmisión por secuencias de multimedia de Acceso Web remoto usa el servicio de uso compartido de red de Windows Media Player 12. La transmisión por secuencias de multimedia de Acceso Web remoto admite los tipos de archivo de audio, vídeo e imagen compatibles con Windows Media Player 12 y Silverlight 4.

 En la siguiente tabla se enumeran los tipos de archivo (formatos) compatibles con la transmisión por secuencias de multimedia de Acceso Web remoto. Si hay tipos de archivo multimedia en el servidor que no se incluyen en la tabla, no podrá transmitirlos mediante la transmisión por secuencias de Acceso Web remoto.

|Tipo de archivo|Extensión de nombre de archivo|
|---------------|-------------------------|
|Archivos 3GPP|.3gp, .3gpp, .3g2 y .3gp2|
|Archivos de audio ADTS (Audio Data Transport Stream)|.adts y .adt|
|Archivos de audio AU|.au y .snd|
|Archivos de audio AIFF (Audio Interchange File Format)|.aif, .aifc y .aiff|
|Archivos AVCHD|.m2ts, .m2t y .mts|
|Disco de audio de CD|.cda|
|Disco de vídeo DVD|.vob|
|Archivos de imagen JPEG|.jpg|
|Archivos de audio MP3|.mp3 y .m3u|
|Archivos de vídeo MPEG|.mpeg, .mpg, .m1v, .mpa, .mpe, .mp4, .mp4v, .m4v, .m4a y .mov|
|Archivos de audio y vídeo de Windows|.avi y .wav|
|Archivos de audio y vídeo de Windows Media|.asf, .asx, .wax, .wm, .wma, .wmd, .wmp, .wmv, .wmx, .wpl y .wvx|

> [!NOTE]
>  Si no puede usar un tipo de archivo que aparece en esta tabla es posible que el archivo también esté codificado con un códec incompatible con el Reproductor de Windows Media.

 Para obtener información adicional acerca de los formatos de archivo compatibles, consulte [Tipos de archivo compatibles con el Reproductor de Windows Media](https://go.microsoft.com/fwlink/p/?LinkID=196118) y [Formatos multimedia, protocolos y campos de registro compatibles](https://go.microsoft.com/fwlink/p/?LinkId=203339) para Silverlight.

## <a name="additional-references"></a>Referencias adicionales

-   [Reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)