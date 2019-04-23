---
title: Reproducción de medios digitales en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f570492-ee21-471b-92c1-3fd9bfb84f55
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8228d0b17a58858ed893181ddceb465715ffdeb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874106"
---
# <a name="play-digital-media-in-windows-server-essentials"></a>Reproducción de medios digitales en Windows Server Essentials

>Se aplica a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Por medios digitales se hace referencia al audio, vídeo y contenido de fotos que se ha comprimido digitalmente.  Windows Server Essentials permite que los equipos en red y algunos dispositivos de medios digitales para reproducir archivos multimedia digitales que se almacenan en el servidor de red.  
  
 Los temas siguientes proporcionan información sobre el acceso y reproducir archivos multimedia digitales que se almacenan en Windows Server Essentials:  
  

-   [Información general de medios digitales](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Reproducir y compartir medios digitales](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Reproducir archivos multimedia digitales compartidos desde una ubicación remota](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Agregar archivos multimedia digitales en el servidor](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Opciones de formato de descarga](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Herramienta de carga fácil](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Ver y Examinar medios digitales compartidos](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Información general de medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Reproducir y compartir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Reproducir archivos multimedia digitales compartidos desde una ubicación remota](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Agregar archivos multimedia digitales en el servidor](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Opciones de formato de descarga](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Herramienta de carga fácil](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Ver y Examinar medios digitales compartidos](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

  
##  <a name="BKMK_1"></a> Información general de medios digitales  
 Por medios digitales se hace referencia al audio, vídeo y contenido de fotos que se han codificado (comprimido digitalmente). La codificación de contenido implica convertir la entrada de audio y vídeo a un archivo de medios digitales, como por ejemplo un archivo de Windows Media. Una vez codificados los medios digitales, estos pueden manipularse, distribuirse y reproducirse en equipos con facilidad, y se transmiten fácilmente a través de redes de equipos.  
  
 Ejemplos de tipos de medios digitales: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG y AVI. Para obtener información acerca de los tipos de medios digitales compatibles con el Reproductor de Windows Media, consulte [Tipos de archivos compatibles con el Reproductor de Windows Media](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>¿Por qué debería transmitir mis medios digitales?  
 Mucha gente almacena música, vídeo e imágenes en las carpetas compartidas en Windows Server Essentials. Puede darse cuando quiere hacer lo siguiente:  
  
-   **Ver vídeos**. El servidor se puede usar para almacenar y transmitir grandes colecciones de vídeos y espectáculos de TV grabados en los equipos o en otros dispositivos de reproducción de la red. Puede transmitir vídeos a Xbox 360 o a un equipo con el Reproductor de Windows Media.  
  
-   **Reproducir música**. Al activar el uso compartido de multimedia de la carpeta compartida **Música** puede acceder a la música desde dispositivos que admitan Windows Media Connect. No es necesario habilitar o configurar las cuentas de usuario para realizar transmisiones desde la carpeta compartida **Música** después de activar el uso compartido.  
  
-   **Mostrar presentaciones fotográficas**. Puede almacenar fotos digitales en la carpeta compartida **Fotos** en el servidor y acceder a ellas desde cualquier equipo o desde una Xbox 360 que esté conectada a un televisor de su casa u oficina. Puede ver presentaciones fotográficas, que es como activar el televisor en un gran marco de fotos.  
  
### <a name="sharing-copy-protected-media"></a>Uso compartido de multimedia protegido contra copia  
  Windows Server Essentials no admite el uso compartido de multimedia protegido contra copia. Esto incluye la música adquirida a través de una tienda de música en línea.  
  
 Los medios protegidos contra copia solo se pueden reproducir en el equipo o dispositivo que ha usado para comprarlos. La protección contra copia impide reproducir archivos multimedia en más de un equipo o dispositivo, incluso si copia los medios al servidor y los reproduce desde allí. Sin embargo, puede almacenar los medios protegidos contra copia en Windows Server Essentials y continuar reproduciéndolos en el equipo o dispositivo que ha usado para comprarlos.  
  
##  <a name="BKMK_2"></a> Reproducir y compartir medios digitales  
 Después de configurar la red y de conectar correctamente los equipos y los dispositivos multimedia a la red del servidor, puede buscar los archivos de medios digitales que almacena y comparte en el servidor.  
  
> [!NOTE]
>  Para obtener una lista actualizada de los dispositivos reproductores/archivos multimedia digitales que son compatibles con Windows Server Essentials, consulte el [centro de compatibilidad de Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
 Puede usar cualquiera de los siguientes métodos para buscar y reproducir archivos multimedia digitales que se almacenan en el servidor:  
  

-   [Buscar y reproducir archivos multimedia en Windows Server Essentials desde un equipo o Reproductor multimedia digital en la red](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Enviar archivos multimedia en Windows Server Essentials al Reproductor de Windows Media, Xbox 360 o a un reproductor multimedia digital en la red](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

-   [Buscar y reproducir archivos multimedia en Windows Server Essentials desde un equipo o Reproductor multimedia digital en la red](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Enviar archivos multimedia en Windows Server Essentials al Reproductor de Windows Media, Xbox 360 o a un reproductor multimedia digital en la red](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

  
###  <a name="BKMK_2.1"></a> Buscar y reproducir archivos multimedia en Windows Server Essentials desde un equipo o Reproductor multimedia digital en la red  
 Cuando el dispositivo se une a la red de Windows Server Essentials, puede buscar y reproducir archivos multimedia digitales de cualquiera de las maneras siguientes:  
  

-   [Buscar y reproducir archivos multimedia desde un equipo que ejecuta Windows Media Center](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Buscar y reproducir archivos multimedia desde un equipo que ejecuta Windows mediante el uso de Windows Media Player](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Buscar y reproducir archivos multimedia con Xbox 360](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Buscar y reproducir archivos multimedia con otros reproductores multimedia digitales o receptores que sean compatibles con Windows Server Essentials](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Buscar y reproducir archivos multimedia mediante la característica carpetas compartidas de Launchpad](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Buscar y reproducir archivos multimedia compartidos mediante acceso Web remoto](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

-   [Buscar y reproducir archivos multimedia desde un equipo que ejecuta Windows Media Center](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [Buscar y reproducir archivos multimedia desde un equipo que ejecuta Windows mediante el uso de Windows Media Player](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [Buscar y reproducir archivos multimedia con Xbox 360](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [Buscar y reproducir archivos multimedia con otros reproductores multimedia digitales o receptores que sean compatibles con Windows Server Essentials](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [Buscar y reproducir archivos multimedia mediante la característica carpetas compartidas de Launchpad](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Buscar y reproducir archivos multimedia compartidos mediante acceso Web remoto](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

  
####  <a name="BKMK_WMC"></a> Buscar y reproducir archivos multimedia desde un equipo que ejecuta Windows Media Center  
  
1.  Haga clic en **Inicio**, en **Todos los programas** y, a continuación, haga clic en **Windows Media Center**.  
  
2.  En la página **Windows Media Center** , desplácese hasta el tipo de medio que está buscando y haga clic en la biblioteca multimedia.  
  
3.  Busque manualmente el archivo en cuestión o haga clic en **Búsqueda** y escriba el nombre del archivo que quiere buscar.  
  
4.  Haga clic en la imagen del archivo multimedia para ver o reproducir el archivo.  
  
####  <a name="BKMK_MWP"></a> Buscar y reproducir archivos multimedia desde un equipo que ejecuta Windows mediante el uso de Windows Media Player  
  
-   En el equipo o dispositivo multimedia, abra el **Reproductor de Windows Media** y busque la biblioteca multimedia.  
  
    > [!NOTE]
    >  Los pasos de búsqueda pueden variar en función de la versión del Reproductor de Windows Media utilizada. Para obtener información detallada, vea la ayuda de la versión.  
  
####  <a name="BKMK_Xbox"></a> Buscar y reproducir archivos multimedia con Xbox 360  
  
1.  Conecte la consola Xbox 360 a la red doméstica mediante una conexión por cable o inalámbrica.  
  
2.  En el equipo que ejecuta Windows Server Essentials, activar el uso compartido de multimedia. Para obtener más información, vea el tema [activar o desactivar la transmisión por secuencias](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
3.  Para reproducir archivos multimedia digitales con la consola Xbox 360:  
  
    1.  Vaya a **Mi Xbox**y seleccione **Biblioteca de vídeos**, **Biblioteca de música**o **Biblioteca de imágenes**, según el tipo de medio que quiera ver o reproducir.  
  
    2.  Seleccione el nombre del servidor.  
  
        > [!NOTE]
        >  Si no aparece el nombre del servidor, seleccione **Equipo** y, a continuación, haga clic en **Probar conexión**.  
  
    3.  Busque en la lista de archivos y seleccione el elemento que quiere reproducir.  
  
####  <a name="BKMK_Other"></a> Buscar y reproducir archivos multimedia con otros reproductores multimedia digitales o receptores que sean compatibles con Windows Server Essentials  
  
1.  Vaya al [Centro de compatibilidad de Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home) y asegúrese de que el reproductor o el receptor de medios digitales aparezca en la lista de dispositivos compatibles.  
  
2.  Dado que los pasos de búsqueda pueden variar en función del reproductor multimedia utilizado, vea la ayuda de su dispositivo para obtener instrucciones detalladas.  
  
####  <a name="BKMK_SharedFolders"></a> Buscar y reproducir archivos multimedia mediante la característica carpetas compartidas de Launchpad  
  
1.  Inicie sesión en Launchpad de Windows Server Essentials.  
  
2.  En Launchpad, haga clic en **Carpetas compartidas**. Aparece una ventana del explorador de Windows, que muestra las carpetas compartidas del servidor.  
  
3.  En el cuadro **Búsqueda** , escriba el nombre de un archivo multimedia. Se mostrarán los resultados de la consulta de búsqueda.  
  
    > [!NOTE]
    >  Como opción, puede hacer doble clic en una carpeta compartida para examinar el contenido de la carpeta.  
  
####  <a name="BKMK_RWA2"></a> Buscar y reproducir archivos multimedia compartidos mediante acceso Web remoto  
  
1.  Inicie sesión en Acceso Web remoto.  
  
2.  Haga clic en **Carpetas compartidas**. La sección **Carpetas compartidas** de la página web muestra una lista de las carpetas compartidas en el servidor.  
  
3.  Haga doble clic en una carpeta para ver el contenido de la carpeta.  
  
###  <a name="BKMK_SendToDevice"></a> Enviar archivos multimedia en Windows Server Essentials al Reproductor de Windows Media, Xbox 360 o a un reproductor multimedia digital en la red  
 Use el **Reproductor de Windows Media** para buscar el archivo multimedia deseado. Haga clic con el botón derecho en el archivo multimedia y, a continuación, haga clic en **Reproducir en** para enviar el archivo multimedia a un dispositivo multimedia en red.  
  
##  <a name="BKMK_3"></a> Reproducir archivos multimedia digitales compartidos desde una ubicación remota  
 Puede reproducir los archivos multimedia cuando esté fuera de la red de Windows Server Essentials mediante acceso Web remoto. Puede usar un teléfono móvil, un equipo remoto o un reproductor multimedia digital para buscar y reproducir los archivos multimedia compartidos que almacena en el servidor.  
  
#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>Para reproducir archivos multimedia compartidos cuando está fuera de la red  
  
1.  Abra un explorador de Internet.  
  
2.  Vaya al sitio web de Acceso Web remoto. Tipo **https://<YourDomainName\>/remoto** en la barra de direcciones del explorador de Internet y, a continuación, presione ENTRAR.  
  
    > [!NOTE]
    >  *< Nombredesudominio\>*  es un marcador de posición. Será un nombre único para el servidor, por lo que la dirección que escriba será similar a **https://contoso.com/remote**. Si no conoce el nombre del dominio, solicíteselo al administrador que ha elegido el nombre del dominio al establecer la funcionalidad de acceso remoto en el servidor. Para obtener más información, consulte [Turn on Remote Web Access](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
3.  En la página de inicio de sesión de Acceso Web remoto, escriba el nombre de la cuenta de usuario y la contraseña y, a continuación, haga clic en la flecha.  
  
4.  Use el método que quiera para buscar el archivo multimedia que desea reproducir.  
  
    > [!NOTE]

    >  Para obtener información acerca de los diversos métodos de búsqueda, vea [buscar y reproducir archivos de medios en Windows Server Essentials desde un equipo o Reproductor multimedia digital en la red](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

    >  Para obtener información acerca de los diversos métodos de búsqueda, vea [buscar y reproducir archivos de medios en Windows Server Essentials desde un equipo o Reproductor multimedia digital en la red](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).  

  
5.  Cuando aparezca el nombre del archivo multimedia, haga clic en el nombre para reproducir el medio.  
  
##  <a name="BKMK_4"></a> Agregar archivos multimedia digitales en el servidor  

 El administrador del servidor puede agregar archivos multimedia digitales a las carpetas compartidas en la biblioteca multimedia accediendo directamente al servidor, o mediante el sitio de acceso Web remoto para iniciar sesión el panel. Otros usuarios pueden agregar archivos multimedia al servidor mediante el uso de la **carpetas compartidas** conexión del LaunchPad, mediante el sitio de acceso Web remoto, o mediante el uso de la aplicación My Server para Windows Phone. Para obtener información acerca de la reproducción de medios, vea [Reproducir y compartir medios digitales](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

 El administrador del servidor puede agregar archivos multimedia digitales a las carpetas compartidas en la biblioteca multimedia accediendo directamente al servidor, o mediante el sitio de acceso Web remoto para iniciar sesión el panel. Otros usuarios pueden agregar archivos multimedia al servidor mediante el uso de la **carpetas compartidas** conexión del LaunchPad, mediante el sitio de acceso Web remoto, o mediante el uso de la aplicación My Server para Windows Phone. Para obtener información acerca de la reproducción de medios, vea [Reproducir y compartir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).  

  
> [!NOTE]
>  También puede cargar archivos multimedia al servidor mediante la aplicación My Server para Windows Phone. Puede descargar la aplicación My Server desde la [Tienda de Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Para obtener más información acerca de la aplicación My Server para Windows Phone, consulte la entrada de blog [aplicación de teléfono My Server para Windows Server Essentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Para agregar archivos multimedia a las carpetas compartidas en el servidor  
  
1.  Para iniciar sesión en el servidor use uno de los métodos siguientes:  
  

    1.  Para obtener información sobre cómo iniciar sesión en acceso Web remoto, consulte [Use Remote Web Access](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

    1.  Para obtener información sobre cómo iniciar sesión en acceso Web remoto, consulte [Use Remote Web Access](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
    2.  Para obtener información sobre cómo iniciar sesión con Launchpad, consulte [Introducción a Launchpad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Busque y haga clic en la carpeta de los archivos multimedia que va a agregar.  
  
3.  Copie y pegue o arrastre y coloque los archivos multimedia que quiere agregar a la carpeta compartida adecuada del servidor.  
  
##  <a name="BKMK_5"></a> Opciones de formato de descarga  
 Hay dos opciones para descargar archivos. Estas opciones solo están disponibles cuando va a descargar varios archivos o una carpeta en un equipo basado en Windows.  
  
 Elija la opción que mejor se adapte a sus necesidades relativas a las descargas:  
  
-   **Archivo ZIP comprimido (.zip)**  
  
     Al comprimir un archivo se crea una versión comprimida del archivo, que es menor que el archivo original. La versión comprimida del archivo tiene la extensión de nombre de archivo .zip. Los tipos de archivo que se reducen al máximo al comprimirse son tipos de archivos de texto (como .txt, .doc y .xls) y archivos de gráficos que usan tipos de archivo no comprimidos (.bmp). Algunos archivos de gráficos, como por ejemplo los archivos .jpg y .gif, ya emplean la compresión, y el tamaño del archivo se reduce muy poco durante la compresión. Además, un documento de Word que contiene una gran cantidad de gráficos no se reduce tanto como un documento que está formado principalmente por texto.  
  
    > [!NOTE]
    >  Esta opción proporciona una compatibilidad limitada para los nombres de archivo internacionales.  
  
-   **Archivo ejecutable autoextraíble (.exe)**  
  
     Un archivo ejecutable autoextraíble es un archivo que se puede descargar y que combina el programa (ejecutable) de descompresión con los archivos comprimidos. Al ejecutar el programa ejecutable, este descomprime automáticamente los archivos comprimidos. Se trata de un método habitual de distribuir los datos comprimidos sin preocuparse de si el destinatario tiene la utilidad de descompresión adecuada.  
  
    > [!NOTE]
    >  Esta opción admite los caracteres Unicode.  
  
 Antes de que comience la descarga real se crea el archivo .exe o .zip. En función del número de archivos y del tamaño total de los archivos que se van a descargar, esta acción puede tardar varios minutos. Después de crearse el archivo de descarga, la descarga del archivo tiene lugar en segundo plano. Esto le permite seguir trabajando mientras se completa el proceso de descarga.  
  
##  <a name="BKMK_6"></a> Herramienta de carga fácil  
 La herramienta de carga fácil simplifica el proceso de carga de archivos en el servidor de Windows Server Essentials. Puede agregar tantos archivos como desee a la herramienta de carga fácil de archivos y, a continuación, cargarlos en las carpetas compartidas en el servidor de Windows Server Essentials en un único lote. Para obtener más información, consulte la entrada de blog sobre la [descripción del uso compartido de archivos en Acceso Web remoto](http://blogs.technet.com/b/sbs/archive/2012/04/19/understanding-remote-web-access-file-sharing.aspx).  
  
##  <a name="BKMK_7"></a> Ver y Examinar medios digitales compartidos  
 Puede ver o examinar los recursos mediante el panel, Launchpad, el sitio web de Acceso Web remoto o la aplicación My Server para Windows Phone.  
  
#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>Para ver y examinar elementos multimedia compartidos desde el panel  
  
1.  Abra el panel del servidor.  
  
2.  En la barra de navegación principal, haga clic en **ALMACENAMIENTO**.  
  
3.  Haga clic en la pestaña **Carpetas de servidor** . Aparecerá una lista de las carpetas del servidor.  
  
4.  Haga doble clic en una carpeta para ver el contenido de la carpeta.  
  
#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>Para ver y examinar los archivos multimedia compartidos con Launchpad desde un equipo de red  
  
1.  Inicie sesión en Launchpad.  
  
2.  Haga clic en **Carpetas compartidas**. El Explorador de Windows abre y muestra una lista de las carpetas compartidas del servidor.  
  
3.  Haga doble clic en una carpeta para ver el contenido de la carpeta.  
  
#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>Para ver y examinar los archivos multimedia compartidos mediante Acceso Web remoto  
  
1.  Inicie sesión en Acceso Web remoto.  
  
2.  Haga clic en **Carpetas compartidas**. La sección **Carpetas compartidas** de la página web muestra una lista de las carpetas compartidas en el servidor.  
  
3.  Haga doble clic en una carpeta para ver el contenido de la carpeta.  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar medios digitales](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)  
  

-   [Conéctate](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usar carpetas compartidas](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Trabajar de forma remota](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Conéctate](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usar carpetas compartidas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Trabajar de forma remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)

