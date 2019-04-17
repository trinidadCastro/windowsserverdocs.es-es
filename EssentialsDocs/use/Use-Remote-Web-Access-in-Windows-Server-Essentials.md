---
title: Usar el acceso Web remoto en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac384b545fe61b4a832debdc8aedcc81a75c669a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Usar el acceso Web remoto en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
  Acceso Web remoto le ayuda a estar conectado a la red de Windows Server Essentials cuando estás fuera. Cuando inicies sesión en acceso Web remoto, puede conectarse a los equipos de la red de Windows Server Essentials, abra el panel para administrar la red de Windows Server Essentials y obtener acceso a todos los archivos multimedia y las carpetas compartidos en el servidor.  
  
 Este tema incluye las siguientes secciones:  
  

-   [Conectarse al acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Compartir archivos y carpetas](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Conectar desde un dispositivo móvil](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Conectarse al acceso Web remoto  
  
-   [Iniciar sesión en acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Acceso remoto a tu equipo](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [Conectarse al acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Compartir archivos y carpetas](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Conectar desde un dispositivo móvil](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Conectarse al acceso Web remoto  
  
-   [Iniciar sesión en acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Acceso remoto a tu equipo](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>Iniciar sesión en acceso Web remoto  
 Al iniciar sesión acceso Web remoto desde un equipo local o remoto, puede acceder a recursos en el servidor que ejecuta Windows Server Essentials y equipos de la red.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Para iniciar sesión en acceso Web remoto desde un equipo de red  
  
1.  Abre un explorador Web, tipo **https://***< YourServerName\ >***/remoto** en la barra de direcciones y, a continuación, presione ENTRAR.  
  
    > [!NOTE]
    >  Asegúrate de que incluya el s en https.  
  
2.  En la página de inicio de sesión de acceso Web remoto, escribe tu nombre de usuario y contraseña en los cuadros de texto y, a continuación, haz clic en la flecha.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Para iniciar sesión en acceso Web remoto desde un equipo remoto  
  
1.  Abre un explorador Web, tipo **https://***< YourDomainName\ >***/remoto** en la barra de direcciones y, a continuación, presione ENTRAR.  
  
    > [!NOTE]
    >  Puedes obtener la información del nombre de dominio desde el Administrador de red. Asegúrate de que incluya el s en https.  
  
2.  En la página de inicio de sesión de acceso Web remoto, escribe tu nombre de usuario y contraseña en los cuadros de texto y, a continuación, haz clic en la flecha.  
  
###  <a name="BKMK_1.5"></a>Acceso remoto a tu equipo  
 Cuando estás lejos de la oficina, puedes usar el explorador Web para iniciar sesión en el sitio de acceso Web remoto para tener acceso remoto a tu escritorio de Windows Server Essentials, las carpetas compartidas y equipos de la red.  
  
 Cuando se conecta al panel, puedes administrar Windows Server Essentials como lo harías si estuvieras en la oficina. Puedes realizar todas las tareas administrativas habituales, como agregar cuentas de usuario, agregar carpetas compartidas, configuración de acceso a la carpeta compartida y así sucesivamente. Cuando se conecta a equipos de la red, puede acceder a sus equipos de escritorio como si estuviera sentado delante de ellas en la oficina.  
  
 La **estado** columna muestra si puedes conectarte a un equipo en la red y pueden incluir los siguientes valores:  
  
-   **Disponible**  
  
     El equipo está activado y está disponible para una conexión remota. Incluso si ves este estado, aún no podrás para conectarse a este equipo si un firewall de terceros bloquea la conexión.  
  
-   **Sin conexión o durmiendo**  
  
     El equipo está apagado o está en modo de suspensión o hibernación. Si un equipo está sin conexión o en modo de suspensión, el estado se actualiza en tiempo real para que sepa cuando el equipo esté disponible.  
  
-   **Sistema operativo no compatible**  
  
     El sistema operativo en el equipo no admite escritorio remoto. Puede tardar hasta 6 horas para este estado se actualice en el servidor si se produce un cambio.  
  
-   **Conexión está deshabilitada**  
  
     La conexión del equipo está bloqueada por un firewall o el escritorio remoto está deshabilitado en el equipo o por la directiva de grupo. Puede tardar hasta 6 horas para este estado se actualice en el servidor si se produce un cambio.  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>Para conectarse a un equipo de la red  
 En la **dispositivos** pestaña, haz clic en el nombre del equipo. Puedes seleccionar solo los equipos con un **disponible** estado.  
  
#### <a name="to-connect-to-the-server-dashboard"></a>Para conectarse al servidor de escritorio  
 En la **dispositivos** pestaña, haz clic en el nombre del servidor. Puedes seleccionar solo los equipos con un **disponible** estado. Debe ser capaz de proporcionar una cuenta de usuario de administrador y una contraseña en el servidor para usar el panel.  
  
##  <a name="BKMK_SharedFolders"></a>Compartir archivos y carpetas  
  

-   [Cargar y descargar archivos en acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Crea, cambia el nombre, mover, eliminar o copiar archivos y carpetas en acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Cargar y descargar archivos en acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Crea, cambia el nombre, mover, eliminar o copiar archivos y carpetas en acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>Cargar y descargar archivos en acceso Web remoto  
 En el acceso Web remoto **carpetas compartidas** ficha, puedes hacer lo siguiente:  
  
-   Carga archivos (envío) desde el equipo a Windows Server Essentials.  
  
    > [!NOTE]
    >  Puedes cargar solo los archivos y carpetas no al acceso Web remoto. Si quieres tener la misma jerarquía de archivos y carpetas la **carpetas compartidas** en el servidor en el equipo, debe crear las carpetas en el servidor de acceso Web remoto y, a continuación, carga los archivos en la carpeta que creaste. Para obtener información sobre la creación de carpetas del servidor, consulte [agregar o mover una carpeta del servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
-   Descargar (recepción) archivos y carpetas de Windows Server Essentials en el equipo.  
  
-   Crear una carpeta dentro de una carpeta compartida en Windows Server Essentials.  
  

-   Mover, eliminar y cambiar el nombre de los archivos y carpetas en Windows Server Essentials. Para obtener más información, consulta [crear, cambiar el nombre, mover, eliminar o copiar los archivos y carpetas en acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

-   Mover, eliminar y cambiar el nombre de los archivos y carpetas en Windows Server Essentials. Para obtener más información, consulta [crear, cambiar el nombre, mover, eliminar o copiar los archivos y carpetas en acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

  
#### <a name="upload-files"></a>Cargar archivos  
  
###### <a name="to-upload-files"></a>Para cargar archivos  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  En la lista de carpetas compartidas de los archivos y carpetas, haz clic en la carpeta donde quieres cargar el archivo y, a continuación, haz clic en **cargar**.  
  
3.  Si aún no se ha cargado la herramienta de carga estándar, haz clic en **usa el método de carga estándar**.  
  
4.  Haz clic en **examinar** para buscar un archivo en el equipo.  
  
5.  Desplazarse por las carpetas en el equipo para buscar el archivo que quieres cargar y, a continuación, haz clic en **abrir**.  
  
6.  Repite los pasos 2 y 3 para cada archivo que quieres cargar.  
  
7.  Cuando hayas agregado todos los archivos que quieres cargar, haz clic en **cargar**.  
  
 La herramienta de carga de archivos fácil simplifica el proceso de carga de archivos en el servidor que ejecuta Windows Server Essentials. Puedes agregar todos los archivos que quieres la herramienta de carga de archivos fácilmente mediante la característica de arrastrar y colocar y luego cargarlos en las carpetas compartidas en el servidor.  
  
> [!NOTE]
>  Carga de varios archivos es compatible de forma nativa en exploradores web que son compatibles con HTML5. Esta herramienta solo es necesaria cuando el explorador web no es compatible con HTML5.  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Para cargar archivos mediante la herramienta de carga de archivos sencillo  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  En la lista de carpetas compartidas de los archivos y carpetas, haz clic en la carpeta donde quieres cargar los archivos y, a continuación, haz clic en **cargar**. Si la carpeta que quieres cargar existe, haz clic en **nueva carpeta**, escriba el nombre de la nueva carpeta en el cuadro de diálogo y, a continuación, haz clic en **Aceptar**.  
  
3.  Debes ejecutar el complemento de soluciones de Windows Server. Si es así, haga clic en la Tira amarilla en la parte superior de la pantalla, haz clic en ejecutar **complemento**y, a continuación, haz clic en **ejecutar** en el cuadro de diálogo.  
  
4.  Si aún no se ha cargado la herramienta fácil carga de archivos, haz clic en **usar la herramienta de carga de archivos fácil**.  
  
5.  Puedes arrastrar y colocar archivos desde el Explorador de Windows a la herramienta de carga de archivos sencillo, o haz clic en **Examinar para seleccionar archivos**.  
  
6.  Cuando termines de agregar los archivos que quieres cargar en la carpeta seleccionada, haz clic en **cargar**.  
  
7.  Cuando los archivos se cargaron correctamente, haz clic en **cerrar**.  
  
#### <a name="download-files-or-folders"></a>Descargar archivos o carpetas  
  
###### <a name="to-download-a-single-file"></a>Para descargar un solo archivo  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  Desde la lista de archivos de la carpeta compartida, haz clic en la casilla de verificación junto al archivo que quieres descargar en el equipo doméstico.  
  
3.  Haz clic en **descargar** para iniciar la descarga.  
  
4.  En la **descarga de archivos** cuadro de diálogo, haz clic en **guardar** para guardar el archivo en el equipo.  
  
5.  En la **Guardar como** cuadro de diálogo, seleccione la ubicación para guardar el archivo y, a continuación, haz clic en **guardar**. Un único archivo no está comprimido antes de descargarlos.  
  
 Existen dos opciones para la descarga de varios archivos o carpetas. Elija la opción que se ajuste a tus necesidades:  
  
> [!NOTE]
>  Estas opciones están disponibles solo cuando se descarga varios archivos o carpetas en el equipo.  
  
-   **Archivo ejecutable autoextraíble (.exe)**  
  
    > [!NOTE]
    >   En esta sección se aplica a un servidor que ejecuta Windows Server Essentials.  
  
     Un archivo ejecutable autoextraíble es un archivo que puedes descargar que combina el programa de descompresión (ejecutable) con los archivos comprimidos. Cuando se ejecuta el programa ejecutable, descomprime automáticamente los archivos comprimidos (autoextraíble). Esto es un método habitual para distribuir los datos comprimidos sin preocuparte por si el destinatario tiene la utilidad de descompresión derecho.  
  
    > [!NOTE]
    >  Esta opción admite caracteres Unicode.  
  
-   **Carpeta de Windows comprimido (.zip)**  
  
     Comprimir un archivo, crea una versión comprimida del archivo que es menor que el archivo original. La versión comprimida del archivo tiene una extensión de nombre de archivo .zip. Tipos de archivo que se reducen el máximo por comprimir son tipos de archivo orientados a texto, como .txt, .doc, .xls, y ese tipos de archivo sin comprimir de uso como .bmp de archivos de gráficos. Algunos archivos de gráficos, como archivos .jpg y .gif, ya usan compresión, y se reduce el tamaño del archivo muy poco por comprimir. Además, un documento de Word que contiene una gran cantidad de gráficos no se reduce tanto como un documento que es principalmente texto.  
  
    > [!NOTE]
    >  Esta opción proporciona compatibilidad limitada con nombres de archivo internacional en Windows Server Essentials.  
  
 Antes de que comience la descarga real, se crea el archivo exe o zip. Según el número de archivos y el tamaño total de los archivos se descarguen, esto puede tardar varios minutos. Después de crea el archivo de descarga, descargar el archivo se produce en segundo plano. Esto te permite seguir trabajando mientras se completa el proceso de descarga.  
  
###### <a name="to-download-multiple-files-or-folders"></a>Para descargar varios archivos o carpetas  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  Desde la lista de archivos de la carpeta compartida, haz clic en la casilla junto a los archivos o carpetas que quieres descargar en el equipo doméstico.  
  
3.  Haz clic en **descargar** para iniciar la descarga.  
  
4.  En la **elige un formato de descargar** cuadro de diálogo, haz clic para seleccionar la opción de formato de descarga que prefieras y, a continuación, haz clic en **Aceptar**. El archivo comprimido está preparado en la opción de formato que has seleccionado.  
  
5.  En la **descarga de archivos** cuadro de diálogo, haz clic en **guardar** para guardar el archivo en el equipo.  
  
6.  En la **Guardar como** cuadro de diálogo, seleccione la ubicación para guardar el archivo y, a continuación, haz clic en **guardar**.  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Recuperar archivos comprimidos descargados en el equipo  
  
> [!NOTE]
>   En esta sección se aplica a un servidor que ejecuta Windows Server Essentials.  
  
 Si seleccionas varios archivos o carpetas para descargar, puede recibir un archivo ejecutable comprimido autoextraíble (.exe) o un archivo comprimido (.zip).  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Para recuperar un archivo desde el archivo comprimido (.exe)  
  
1.  En el equipo, haz doble clic para abrir el archivo comprimido.  
  
2.  Sigue las instrucciones para extraer los archivos a una carpeta en el equipo.  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Para recuperar un archivo desde el archivo comprimido (.zip)  
  
1.  En el equipo, haz doble clic para abrir el archivo comprimido.  
  
2.  Selecciona los archivos que quieres recuperar y, a continuación, arrastra los archivos a una carpeta en el equipo donde quieres de almacenarlos.  
  
    > [!NOTE]
    >  Si usas un programa de compresión de archivos de terceros, sigue los procedimientos de ese programa para extraer los archivos desde el archivo comprimido.  
  
###  <a name="BKMK_2"></a>Crea, cambia el nombre, mover, eliminar o copiar archivos y carpetas en acceso Web remoto  
 Puedes usar acceso Web remoto para crear carpetas nuevas en una carpeta compartida existente, para cambiar el nombre de los archivos y carpetas, mover y copiar archivos y carpetas y eliminar archivos y carpetas del servidor.  
  
> [!NOTE]
>  Para agregar nuevas carpetas compartidas en un servidor que ejecuta Windows Server Essentials, debes usar el panel. Para conectarse a la consola del servidor de acceso Web remoto, en la **equipos** , haz clic en el nombre del servidor, haga clic **conectar**y, a continuación, sigue las instrucciones para iniciar sesión en el servidor. Para obtener información sobre cómo crear carpetas compartidas, vea [agregar o mover una carpeta del servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
##### <a name="to-create-a-new-folder"></a>Para crear una nueva carpeta  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  En la barra de tareas, haz clic en **nueva carpeta**.  
  
3.  Escribe un nombre para la carpeta y, a continuación, haz clic en **Aceptar**.  
  
##### <a name="to-rename-a-file-or-folder"></a>Para cambiar el nombre de un archivo o carpeta  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  Haz clic en el archivo o carpeta que desea cambiar el nombre y, a continuación, haz clic en **cambiar el nombre de**.  
  
3.  Escribe un nombre nuevo en el cuadro de texto y, a continuación, haz clic en **Aceptar**.  
  
##### <a name="to-move-files-or-folders"></a>Para mover archivos o carpetas  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  Selecciona la casilla junto a los archivos o carpetas que quieras mover, haz clic en uno de los archivos o carpetas seleccionados y, a continuación, haz clic en **cortar**.  
  
3.  Haz clic en la carpeta que quieres mover los archivos o carpetas para y, a continuación, haz clic en **pegar**.  
  
##### <a name="to-delete-a-file-or-folder"></a>Para eliminar un archivo o carpeta  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  Selecciona la casilla junto a los archivos o carpetas que quieras eliminar, haz clic en uno de los archivos o carpetas seleccionados y, a continuación, haz clic en **eliminar**.  
  
3.  Para confirmar que quieres eliminar los archivos y carpetas seleccionados, haz clic en **Sí**.  
  
##### <a name="to-copy-files-or-folders"></a>Para copiar archivos o carpetas  
  
1.  En acceso Web remoto, haga clic en el **carpetas compartidas** pestaña y, a continuación, haz clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y carpetas en la carpeta compartida.  
  
2.  Selecciona la casilla junto a los archivos o carpetas que quieres copiar, haz clic en uno de los archivos o carpetas seleccionados y, a continuación, haz clic en **copia**.  
  
3.  Haz clic en la carpeta que quieres copiar los archivos o carpetas para y, a continuación, haz clic en **pegar**.  
  
##  <a name="BKMK_ConnectMobile"></a>Conectar desde un dispositivo móvil  
  

-   [Usar el acceso Web remoto desde un dispositivo móvil](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Admite los exploradores Web para dispositivos móviles](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Usar el acceso Web remoto desde un dispositivo móvil](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Admite los exploradores Web para dispositivos móviles](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>Usar el acceso Web remoto desde un dispositivo móvil  
 Puede iniciar sesión acceso Web remoto desde el teléfono inteligente para ver los archivos y carpetas en las carpetas compartidas en el servidor.  
  
> [!NOTE]
>  También puedes descargar y usar la aplicación de mi servidor para Windows Server Essentials desde el [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) para acceder a los archivos de medios y carpetas compartidos que se almacenan en el servidor.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Para iniciar sesión en acceso Web remoto desde un dispositivo móvil  
  
1.  Abre un explorador Web y escribe **https://***< YourDomainName\ >***/remoto** en la barra de direcciones.  Asegúrate de que incluya el s en https.  
  
2.  En la página de inicio de sesión de acceso Web remoto, escribe tu nombre de usuario y contraseña en los cuadros de texto y, a continuación, haz clic en la flecha. Has iniciado sesión con la versión móvil de acceso Web remoto.  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Para cambiar a la versión de escritorio de acceso Web remoto  
  
1.  Abre un explorador Web y escribe **https://***< YourDomainName\ >***/remoto** en la barra de direcciones.  Asegúrate de que incluya el s en https.  
  
2.  En la página de inicio de sesión de acceso Web remoto, escribe tu nombre de usuario y contraseña en los cuadros de texto, haz clic en **permite ver la versión escritorio**y, a continuación, haz clic en la flecha. Has iniciado sesión en la versión de escritorio de acceso Web remoto.  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Para volver a la versión móvil de acceso Web remoto  
  
1.  Cierra la sesión.  
  
2.  Abre un explorador Web y escribe **https://***< YourDomainName\ >***/control remoto/m** en la barra de direcciones. Asegúrate de que incluya el s en https.  
  
3.  Se muestra la versión móvil de acceso Web remoto. En la página de inicio de sesión de acceso Web remoto, escribe tu nombre de usuario y contraseña en los cuadros de texto y, a continuación, haz clic en la flecha. Has iniciado sesión con la versión móvil de acceso Web remoto.  
  
 Puede buscar archivos y carpetas en las carpetas compartidas en el servidor.  
  
###  <a name="BKMK_9"></a>Admite los exploradores Web para dispositivos móviles  
 Incluyen los exploradores web admitidos para dispositivos móviles:  
  
-   Internet Explorer Mobile 6.0 o posterior  
  
-   Safari  
  
-   BlackBerry  
  
-   Symbian 6.0 o posterior  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar el acceso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [Trabajar de forma remota](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [Trabajar de forma remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

