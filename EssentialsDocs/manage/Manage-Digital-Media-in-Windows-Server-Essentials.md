---
title: Administrar elementos multimedia Digital en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6906c1dafc6d4131e07c008b9db47ebebe9b7770
ms.sourcegitcommit: 5c8d8acc59d61756377c9c4e459a47a9ab555b39
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2018
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Administrar elementos multimedia Digital en Windows Server Essentials

>Se aplica a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Los temas siguientes describen el características de tu servidor de streaming de multimedia y explican cómo configurar y usar medios de transmisión por secuencias de la red.  
  
> [!NOTE]
>  De manera predeterminada, la funcionalidad de transmisión por secuencias de multimedia no está disponible en Windows Server Essentials y Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado. Para agregar la funcionalidad en estas versiones, de streaming de multimedia [descargar e instalar el paquete de medios de Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40837) desde Microsoft Download Center.  
  
-   [Información general de multimedia digital](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Administrar el servidor multimedia con el panel](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Cómo funciona la transmisión por secuencias de multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Activar o desactivar de streaming multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Agregar archivos multimedia digitales en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Permitir o restringir el acceso a una biblioteca multimedia en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Cambiar el nombre de la biblioteca multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Dejar de compartir contenido multimedia digital](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Habilitar los dispositivos de medios que usan el protocolo bloque de mensajes de servidor (SMB) para acceder a archivos compartidos en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Procesadores comunes y son compatibles con los perfiles de vídeo](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [Problemas conocidos con tipos de archivos multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>Información general de multimedia digital  
 Contenido multimedia digital se refiere a audio, vídeo y contenido de la foto que se ha codificado (comprimido digitalmente). Contenido de codificación implica convertir la entrada de audio y vídeo a un archivo multimedia digital, como un archivo multimedia de Windows. Después de que se codifica multimedia digital, puede ser fácilmente manipular, distribuido, equipos y fácilmente se transmiten a través de redes de equipos.  
  
 Algunos ejemplos de tipos de medios digitales: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG y AVI. Para obtener información sobre los tipos de archivos multimedia digitales que son compatibles con el Reproductor de Windows Media, consulte [tipos de archivo compatibles con el Reproductor de Windows Media](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>¿Por qué querría transmitir mi multimedia digital?  
 Muchas personas almacenan música, vídeo e imágenes en carpetas compartidas en Windows Server Essentials. Puede que a veces, cuando quieras hacer lo siguiente:  
  
-   **Ver vídeos**. El servidor puede usarse para almacenar y transmitir por secuencias grandes colecciones de vídeos y TV grabada muestra a los equipos o la reproducción de otra dispositivos de la red. Puedes transmitir vídeos en una consola Xbox 360 o en un equipo con el Reproductor de Windows Media.  
  
-   **Reproducir música**. Al activar uso compartido de multimedia para el **música** carpeta compartida, puedes acceder a tu música de los dispositivos que admiten la conexión de medios de Windows. No es necesario habilitar o configurar las cuentas de usuario para hacer streaming desde el **música** carpeta compartida después de uso compartido está activado.  
  
-   **Presentar presentaciones de diapositivas de foto**. Puedes almacenar fotos digitales en la **fotos** compartido carpeta en el servidor y, a continuación, acceder a ellos desde cualquier equipo o desde una consola Xbox 360 que está conectado a un Televisor en su casa o de office. Puedes ver presentaciones de diapositivas de foto, que es similar a convertir el Televisor en un marco de imagen grande.  
  
###  <a name="BKMK_1.5"></a>Uso compartido de multimedia protegida copia  
  Windows Server Essentials no admite el uso compartido de multimedia protegidos contra copia. Esto incluye la música que se ha comprado a través de una tienda de música en línea.  
  
 Copy-protected multimedia puede reproducir solo en el equipo o dispositivo que usaste para adquirirlo. La protección contra copia impide la reproducción multimedia en más de un equipo o dispositivo, incluso si copia los medios en el servidor y reproducirlo desde allí. Sin embargo, puedes almacenar los medios protegidos contra copia en Windows Server Essentials y continuar a reproducir los medios en el equipo o dispositivo que usaste para adquirirlo.  
  
##  <a name="BKMK_2"></a>Administrar el servidor multimedia con el panel  
  Windows Server Essentials hace posible realizar tareas de administración comunes mediante el panel de Windows Server Essentials. La **multimedia** ficha del servidor **configuración** página del panel proporciona las siguientes acciones:  
  
|Sección|Funcionalidad|  
|-------------|-------------------|  
|Servidor multimedia|La **activar / desactivar** botón permite activar o desactivar de streaming multimedia.|  
|Calidad de transmisión por secuencias de vídeo|La flecha desplegable te permite elegir la calidad de los vídeos que se reproducen desde el servidor de streaming de vídeo.|  
|Biblioteca multimedia|Muestra el nombre de la biblioteca multimedia. El nombre predeterminado de la biblioteca se denomina **biblioteca multimedia Digital**, que se crea al activar la transmisión por secuencias de multimedia. Para cambiar el nombre de la biblioteca multimedia, consulta [cambiar el nombre de la biblioteca multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). Puedes hacer clic en **personalizar** para personalizar las carpetas que se comparten en la biblioteca multimedia.|  
  
 Para obtener más información, consulta [permitir o restringir el acceso a una biblioteca multimedia en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) y [uso compartido de multimedia protegida copia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).  
  
##  <a name="BKMK_3"></a>Cómo funciona la transmisión por secuencias de multimedia  
 Los medios de transmisión por secuencias de la característica de Windows Server Essentials hace que sea posible para los equipos en red y algunos dispositivos multimedia digital en red para reproducir archivos multimedia digitales que se almacenan en el servidor.  
  
 Cuando activas el servidor multimedia, contenido que compartes en las bibliotecas multimedia estará disponible para reproducir en dispositivos de la red que son capaces de recibir transmisiones multimedia desde el servidor. Puede transmitir por secuencias casi todos los tipos de archivos multimedia digitales. Algunos de los tipos de archivos que puede transmitir por secuencias más habituales son:  
  
-   Formatos de medios de Windows (.asf, .wma, .wmv, .wm)  
  
-   Audio y vídeo de intercalación (.avi)  
  
-   Las imágenes en movimiento Experts Group (.mpeg, .mpg,. mp3)  
  
-   Audio de Windows (.wav)  
  
-   CD pista de Audio (.cda)  
  
 Para reproducir un archivo, simplemente encontrar una canción, vídeo o imagen en una carpeta compartida, haz doble clic en el archivo, y el contenido se transmitir desde el servidor en el equipo y reproducir. Para obtener información acerca de cómo buscar y reproducir los archivos multimedia digitales que se almacenan en el servidor, consulte [reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
 Para transmitir por secuencias multimedia, necesitarás el siguiente hardware:  
  
-   Una red privada con cable o inalámbrica  
  
-   Cualquier otro equipo en la red o un dispositivo conocido como un receptor de multimedia digital (también conocido como un reproductor multimedia digital en red). ¿Receptores de multimedia digital son dispositivos de hardware conectados a la red con cable o inalámbrica que puedes controlar con tu equipo? incluso si el equipo esté en otra habitación.  
  
 Para obtener más información, consulta [activar o desactivar de streaming multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
##  <a name="BKMK_4"></a>Activar o desactivar de streaming multimedia  
 Puedes compartir música, vídeos e imágenes de Windows Server Essentials por streaming de archivos a cualquier receptor de multimedia digital compatibles (DMR), como equipos, teléfonos móviles, televisores, receptores de multimedia digital, extenders para Windows Media Center (incluida la Xbox 360) y otros dispositivos electrónicos personales.  
  
 Para obtener una lista de dispositivos de medios digitales que son compatibles con Windows Server Essentials, consulte la [centro de compatibilidad de Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
### <a name="enabling-media-sharing"></a>Habilitar el uso compartido de multimedia  
 Para compartir los medios que se almacenan en Windows Server Essentials, debes activar el streaming de multimedia. Transmisión por secuencias de multimedia está desactivada de manera predeterminada.  
  
####  <a name="BKMK_2.5"></a>Para activar o desactivar de streaming multimedia  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  Haz clic en **configuración**, haz clic en **multimedia**, y, a continuación, realiza una de las siguientes acciones:  
  
    -   Haz clic en **activar** para empezar a compartir todos los archivos que se almacenan en la biblioteca multimedia del servidor.  
  
    -   Haz clic en **desactivar** para dejar de compartir todos los archivos que se almacenan en la biblioteca multimedia del servidor.  
  
3.  Si desea compartir carpetas adicionales en la biblioteca multimedia, haz clic en **personalizar**y, a continuación, selecciona **Sí** para cada carpeta compartida que quieres incluir en la biblioteca multimedia.  
  
4.  Haz clic en **Aceptar** para guardar los cambios.  
  
 Para obtener información sobre los tipos de archivos multimedia digitales compatibles con Windows Media Player, consulte [tipos de archivo compatibles con el Reproductor de Windows Media](https://support.microsoft.com/kb/316992).  
  
 Para obtener más información, consulta [permitir o restringir el acceso a una biblioteca multimedia en el servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).  
  
##  <a name="BKMK_5"></a>Agregar archivos multimedia digitales en el servidor  
 El administrador del servidor puede agregar multimedia a las carpetas compartidas en la biblioteca multimedia acceder directamente al servidor, o mediante el sitio de acceso Web remoto para iniciar sesión el panel. Otros usuarios pueden agregar archivos multimedia en el servidor mediante la **carpetas compartidas** conexión en el Launchpad, mediante el sitio de acceso Web remoto o mediante la aplicación de mi servidor para Windows Phone. Para obtener información acerca de la reproducción multimedia, consulta [reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  También puedes cargar archivos multimedia en el servidor mediante la aplicación de mi servidor para Windows Phone. Puedes descargar la aplicación Mi servidor desde el [tienda de Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Para obtener más información acerca de la aplicación de mi servidor para Windows phone, consulta la entrada de blog [aplicación de teléfono de mi servidor para Windows Server Essentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Para agregar archivos multimedia digitales a las carpetas compartidas en el servidor  
  
1.  Utilice uno de los siguientes métodos para iniciar sesión en el servidor:  
  
    1.  Para obtener información sobre cómo iniciar sesión en acceso Web remoto, vea [iniciar sesión en acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).  
  
    2.  Para obtener información sobre el inicio de sesión con el Launchpad, consulta [Launchpad Introducción](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Buscar y haz clic en la carpeta para los medios que vas a agregar.  
  
3.  Copiar y pegar o arrastrar y colocar los archivos multimedia que quieras agregan a la carpeta compartida correspondiente en el servidor.  
  
##  <a name="BKMK_6"></a>Permitir o restringir el acceso a una biblioteca multimedia en el servidor  
  
-   Cuando activas el uso compartido de multimedia, se crea cuatro carpetas predefinidas: música, imágenes, vídeos y TV grabada. Si cualquiera de estas carpetas ya exista en el servidor, se reutiliza la carpeta existente como una carpeta compartida para uso compartido de multimedia. Todos los la carpeta s multimedia contenido y usuario permisos existentes se conservan y se comparten con todos los usuarios de red.  
  
-   Antes de activar el uso compartido de bibliotecas de medios para una carpeta compartida, debes saber que la biblioteca de uso compartido de multimedia omite cualquier tipo de acceso a la cuenta de usuario que se establece para la carpeta compartida. Por ejemplo, supongamos que Active biblioteca de uso compartido de multimedia para el **fotos** carpeta compartida, y estableces la **fotos** carpeta compartida para **sin acceso** para un usuario cuenta denominada Bobby. Bobby puede continuar transmitiendo los medios digitales desde el **vídeos** carpeta compartida a cualquier reproductor de medios digitales compatibles o DMR. Si tienes un medio digital que no quieras transmitir por secuencias de esta manera, guarda los archivos en una carpeta que no tiene medios biblioteca activado el uso compartido.  
  
-   Si activas biblioteca de uso compartido de multimedia para una carpeta compartida, cualquier reproductor de medios digitales compatibles o DMR que puede tener acceso a la red de Windows Server Essentials también puede acceder a sus archivos multimedia digitales en la carpeta compartida. Por ejemplo, si tienes una red inalámbrica y no se ha protegido, cualquiera dentro del alcance de su red inalámbrica puede tener acceso a sus archivos multimedia digitales en esa carpeta. Antes de activar uso compartido de la biblioteca multimedia, asegúrate de proteger la red inalámbrica. Para obtener más información, consulta la documentación del punto de acceso inalámbrico.  
  
##  <a name="BKMK_8"></a>Cambiar el nombre de la biblioteca multimedia  
 El nombre predeterminado de la biblioteca multimedia es **servidor multimedia Digital**. Se crea cuando se activa en medios de transmisión por secuencias en Windows Server Essentials. Para obtener más información sobre cómo activar la transmisión por secuencias de multimedia, consulta [para activar o desactivar de streaming multimedia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). Puedes modificar el nombre de la biblioteca multimedia en cualquier momento mediante el panel del servidor.  
  
#### <a name="to-rename-the-media-library"></a>Para cambiar el nombre de la biblioteca multimedia  
  
1.  Abre el panel del servidor. Para abrir el panel desde el Launchpad, haz clic en **panel**, escribe la contraseña del servidor y, a continuación, haz clic en la flecha para iniciar sesión.  
  
2.  En el panel del servidor, haz clic en **configuración**.  
  
3.  En la **configuración** página, haz clic en el **multimedia** pestaña.  
  
4.  En la **biblioteca multimedia** sección de la **configuración multimedia** página, haz clic en el nombre de la biblioteca multimedia.  
  
5.  En la **cambiar el nombre de la biblioteca multimedia** cuadro de diálogo, escribe un nombre nuevo para la biblioteca multimedia y, a continuación, haz clic en **Aceptar**.  
  
##  <a name="BKMK_9"></a>Dejar de compartir contenido multimedia digital  
 El administrador del servidor puede dejar de compartir contenido multimedia digital que se almacena en carpetas compartidas en un servidor que ejecuta Windows Server Essentials.  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>Para dejar de compartir contenido multimedia en las carpetas compartidas  
  
1.  Abre el panel del servidor.  
  
2.  En el panel **Home** página, haz clic en **instalación**, haz clic en **configurar el servidor multimedia**y, a continuación, haz clic en **haga clic para configurar el servidor multimedia**.  
  
3.  En la **multimedia** página de configuración, puedes elegir una de las siguientes opciones:  
  
    -   Haz clic en **desactivar** para dejar de compartir todos los archivos que se almacenan en el servidor.  
  
    -   Haz clic en **personalizar**y, a continuación, selecciona **No** para carpetas específicas que desea dejar de compartir.  
  
4.  Haz clic en **aplicar** o **Aceptar** para guardar los cambios.  
  
##  <a name="BKMK_10"></a>Habilitar los dispositivos de medios que usan el protocolo bloque de mensajes de servidor (SMB) para acceder a archivos compartidos en el servidor  
 Dispositivos que usan el bloque de mensajes de servidor (SMB) para el acceso de archivo y el recurso compartido de red en lugar de DLNA (para el streaming de multimedia) requieren que la cuenta Invitado activarse. Esto permite que cualquier usuario o dispositivo de la red para ver el contenido de las carpetas compartidas sin autenticación.  
  
> [!CAUTION]
>  Al habilitar la cuenta Invitado, cualquier persona puede acceder a los recursos compartidos en el servidor de manera predeterminada.  
  
##  <a name="BKMK_CommonProcessors"></a>Procesadores comunes y son compatibles con los perfiles de vídeo  
 Para transmitir multimedia desde el servidor de Windows Server Essentials, puedes usar un equipo que ejecuta el sistema operativo Windows 7 o Windows 8 otros dispositivos en red (por ejemplo, reproductores de medios digitales) o Media Center extenders (por ejemplo, Xbox 360). Cuando estés fuera de la red, usa el Reproductor de medios de acceso Web remoto para reproducir archivos que están almacenados en el servidor.  
  
 Necesitas una velocidad de transferencia de datos entre 200 KBps y 10 MBps. Debes usar los formatos de medios que el equipo y dispositivos puedan reconocer y reproducir. No todos los dispositivos admiten los mismos formatos de medios, debe haber una forma para PC y dispositivos para reproducir los archivos multimedia que tienes.  
  
  Windows Server Essentials contiene compatibilidad de transcodificación que determina la capacidad del equipo o el dispositivo y, a continuación, se convierte un archivo de vídeo no admitido en dinámicamente en uno compatible. En general, si el Reproductor de Windows Media 12 puede reproducir el contenido en un equipo que ejecuta Windows 7 o Windows 8, se reproducirá el contenido en el servidor en el dispositivo conectado a la red.  
  
 La velocidad de bits y de formato elegida para la transcodificación depende en gran medida el rendimiento del procesador del servidor. El rendimiento del procesador se identifica como parte de la evaluación de la experiencia de Windows. Para determinar la puntuación de rendimiento de tu servidor, realiza una de las siguientes acciones:  
  
-   En un equipo de red que ejecuta Windows 7 o Windows 8 que tiene el mismo procesador como el servidor, ve a la **Panel de Control**, haz clic en **rendimiento información y herramientas**y, a continuación, revisa la información sobre la **velocidad y mejorar el rendimiento del equipo s** página.  
  
-   Ponte en contacto con el fabricante del procesador.  
  
 Para obtener la mejor experiencia de usuario, elige un calidad de resolución que sea apropiada para el procesador del servidor de streaming de vídeo. El servidor ajustará automáticamente la velocidad de bits a uno de estos valores:  
  
-   **Bajo** si la puntuación del procesador es inferior a 3.6.  
  
-   **Mediano** si la puntuación del procesador es mayor que 3.6 y 4.2 inferiores.  
  
-   **Alto** si la puntuación del procesador es mayor que 4.2 y menos de 6.0.  
  
-   **Mejor** si la puntuación del procesador es mayor que 6.0.  
  
 Si eliges un resolución que requiere más energía de procesamiento que tiene el servidor de streaming de vídeo, puede experimentar búferes y se detiene durante la transmisión por secuencias multimedia desde el servidor.  
  
> [!NOTE]
>  Para hacer streaming de alta definición vídeo a través de acceso Web remoto, necesitas un procesador con una puntuación de al menos 6.0.  
  
##  <a name="BKMK_KnownIssues"></a>Problemas conocidos con tipos de archivos multimedia  
 La característica de transmisión por secuencias de multimedia en acceso Web remoto usa el servicio de uso compartido de Windows Media Player 12 red. Streaming de multimedia de acceso Web remoto admite el audio, vídeo y tipos de archivo de imagen que son compatibles con el Reproductor de Windows Media 12 y Silverlight 4.  
  
 La siguiente tabla enumera los tipos de archivo (formatos) admitidos por streaming de multimedia de acceso Web remoto. Si hay tipos en el servidor que no están incluidos en la tabla de archivos multimedia, que no puedes transmitir por streaming de multimedia de acceso Web remoto.  
  
|Tipo de archivo|Extensión de nombre de archivo|  
|---------------|-------------------------|  
|Archivos 3GPP|. 3gp,. 3GPP, .3g 2 y. 3gp2|  
|Archivos de audio audio de secuencia de transporte de datos (ADTS)|.ADTS y .adt|  
|Archivos de audio AU|.au y .snd|  
|Archivos de audio de audio Interchange File Format (AIFF)|.AIF, .aifc y .aiff|  
|Archivos AVCHD|. m2ts,. m2t y .mts|  
|Disco de audio CD|.cda|  
|Disco de vídeo DVD|.vob|  
|Archivos de imagen JPEG|.jpg|  
|Archivos de audio MP3|. mp3 y. m3u|  
|Archivos de vídeo MPEG|.MPEG, .mpg,. m1v, .mpa, .mpe,. mp4,. MP4V,. m4v,. m4a y .mov|  
|Archivos de audio y vídeo de Windows|.avi y .wav|  
|Archivos de audio y vídeos de Windows Media|.asf, .asx, .wax, .wm, .wma, .wmd, .wmp, .wmv, .wmx, .wpl y .wvx|  
  
> [!NOTE]
>  Si no puedes usar un tipo de archivo que se enumera en esta tabla, el archivo también puede codificarse con un códec que no es compatible con el Reproductor de Windows Media.  
  
 Para obtener más información acerca de los formatos de archivo admitidos, consulta [tipos de archivo compatibles con el Reproductor de Windows Media](https://go.microsoft.com/fwlink/p/?LinkID=196118) y [admite formatos de medios, protocolos y campos de registro](https://go.microsoft.com/fwlink/p/?LinkId=203339) para Silverlight.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
