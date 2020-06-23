---
title: Administrar Acceso web remoto en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 17ed30b708ce76396d49b4dc1c3c4a4785b67fe8
ms.sourcegitcommit: 56ac7cf3f4bbcc5175f140d2df5f37cc42ba76d1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85217585"
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Administrar Acceso web remoto en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
  Acceso Web remoto es una característica de Windows Server Essentials que permite tener acceso a archivos, carpetas y equipos de la red a través de un explorador Web desde cualquier lugar con conectividad a Internet. 
  
  El Acceso web remoto le permite estar conectado a la red de Windows Server Essentials cuando lo utilice. Al iniciar sesión en acceso Web remoto, puede conectarse a los equipos de la red de Windows Server Essentials, abrir el panel para administrar la red de Windows Server Essentials y acceder a todas las carpetas compartidas y archivos multimedia del servidor.  
  
 Este tema incluye las siguientes secciones:  
  

-   [Conectarse al Acceso web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Share files and folders](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Conectarse desde un dispositivo móvil](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="connect-to-remote-web-access"></a><a name="BKMK_Connect"></a>Conectarse al acceso Web remoto  
  
-   [Iniciar sesión en acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Acceder de forma remota a su equipo](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [Conectarse al Acceso web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Share files and folders](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Conectarse desde un dispositivo móvil](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="connect-to-remote-web-access"></a><a name="BKMK_Connect"></a>Conectarse al acceso Web remoto  
  
-   [Iniciar sesión en acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Acceder de forma remota a su equipo](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="log-on-to-remote-web-access"></a><a name="BKMK_1"></a>Iniciar sesión en acceso Web remoto  
 Al iniciar sesión en el acceso Web remoto desde un equipo local o remoto, puede tener acceso a los recursos del servidor que ejecuta Windows Server Essentials y los equipos de la red.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Para iniciar sesión en el Acceso web remoto desde un equipo de red  
  
1.  Abra un explorador Web, escriba **https://** _<nombreservidor \> _**/Remote** en la barra de direcciones y, a continuación, presione Entrar.  
  
    > [!NOTE]
    >  Asegúrese de incluir s en https.  
  
2.  En la página inicio de sesión de acceso Web remoto, escriba el nombre de usuario y la contraseña en los cuadros de texto y, a continuación, haga clic en la flecha.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Para iniciar sesión en el Acceso web remoto desde un equipo remoto  
  
1.  Abra un explorador Web, escriba **https://** _<sudominio \> _**/Remote** en la barra de direcciones y, a continuación, presione Entrar.  
  
    > [!NOTE]
    >  El administrador de red puede proporcionarle la información del nombre de dominio. Asegúrese de incluir s en https.  
  
2.  En la página inicio de sesión de acceso Web remoto, escriba el nombre de usuario y la contraseña en los cuadros de texto y, a continuación, haga clic en la flecha.  
  
###  <a name="remotely-access-your-computer"></a><a name="BKMK_1.5"></a>Acceder de forma remota al equipo  
 Cuando esté fuera de la oficina, puede usar el explorador Web para iniciar sesión en el sitio de acceso Web remoto para acceder de forma remota al panel de Windows Server Essentials, las carpetas compartidas y los equipos de la red.  
  
 Cuando se conecte al panel, puede administrar Windows Server Essentials tal como lo haría si estuviera en la oficina. Puede realizar todas las tareas administrativas habituales, como agregar cuentas de usuario, agregar carpetas compartidas, configurar el acceso a carpetas compartidas, etc. Cuando se conecte a equipos en la red, puede tener acceso a sus escritorios como si estuviera frente a ellos en la oficina.  
  
 La columna **Estado** indica si puede conectarse a un equipo en la red, y puede incluir los siguientes valores:  
  
-   **Disponible**  
  
     El equipo está activado y está disponible para una conexión remota. Aunque vea este estado, es posible que aún no pueda conectarse a este equipo si un firewall de terceros bloquea la conexión.  
  
-   **Sin conexión o en suspensión**  
  
     El equipo se apaga o está en modo de hibernación o de suspensión. Si un equipo está sin conexión o en modo de suspensión, el estado se actualiza en tiempo real para que sepa cuándo está disponible el equipo.  
  
-   **Sistema operativo no compatible**  
  
     El sistema operativo en el equipo no es compatible con el Escritorio remoto. Este estado puede tardar hasta 6 horas en actualizarse en el servidor si se produce algún cambio.  
  
-   **La conexión está deshabilitada**  
  
     La conexión del equipo está bloqueada por un firewall o el escritorio remoto está deshabilitado en el equipo o por la directiva de grupo. Este estado puede tardar hasta 6 horas en actualizarse en el servidor si se produce algún cambio.  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>Para conectarse a un equipo de la red  
 En la pestaña **DISPOSITIVOS**, haga clic en el nombre del equipo. Puede seleccionar solo los equipos con estado **Disponible**.  
  
#### <a name="to-connect-to-the-server-dashboard"></a>Para conectarse al panel del servidor  
 En la pestaña **DISPOSITIVOS**, haga clic en el nombre de su servidor. Puede seleccionar solo los equipos con estado **Disponible**. Debe ser capaz de proporcionar una cuenta de usuario administrador y la contraseña en el servidor para usar el panel.  
  
##  <a name="share-files-and-folders"></a><a name="BKMK_SharedFolders"></a>Compartir archivos y carpetas  
  

-   [Cargar y descargar archivos en Acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Crear, cambiar el nombre, mover, eliminar o copiar archivos y carpetas Acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="upload-and-download-files-in-remote-web-access"></a><a name="BKMK_UploadRWA"></a>Cargar y descargar archivos en acceso Web remoto  
 En la pestaña **Carpetas compartidas** del Acceso web remoto, puede hacer lo siguiente:  
  
-   Cargar (enviar) archivos desde el equipo a Windows Server Essentials.  
  
    > [!NOTE]
    >  Solo puede cargar los archivos en el Acceso web remoto, pero no las carpetas. Si desea tener la misma jerarquía de archivos y carpetas de **Carpetas compartidas** en el servidor y en el equipo, debe crear las carpetas en el servidor del Acceso web remoto y, a continuación, cargar los archivos en la carpeta que creó. Para obtener información sobre cómo crear carpetas de servidor, consulte [agregar o mover una carpeta de servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
-   Descargar (recibir) archivos y carpetas desde Windows Server Essentials a su equipo.  
  
-   Cree una carpeta dentro de una carpeta compartida en Windows Server Essentials.  
  

-   Mueva, elimine y cambie nombres de archivos y carpetas en Windows Server Essentials. Para obtener más información, vea [crear, cambiar el nombre, trasladar, eliminar o copiar archivos y carpetas en acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

-   Mueva, elimine y cambie nombres de archivos y carpetas en Windows Server Essentials. Para obtener más información, vea [crear, cambiar el nombre, trasladar, eliminar o copiar archivos y carpetas en acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

  
#### <a name="upload-files"></a>Carga de archivos  
  
###### <a name="to-upload-files"></a>Para cargar archivos  
  
1. En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, a continuación, haga clic en un vínculo de carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2. En la lista de archivos y carpetas de la carpeta compartida, haga clic en la carpeta donde quiera cargar el archivo y, a continuación, haga clic en **Cargar**.  
  
3. Si aún no está cargada la herramienta de carga estándar, haga clic en **Use el método de carga estándar**.  
  
4. Haga clic en **Examinar**  para buscar un archivo en el equipo.  
  
5. Navegue a través de las carpetas en el equipo para buscar el archivo que desea cargar y, a continuación, haga clic en **Abrir**.  
  
6. Repita los pasos 2 y 3 para cada archivo que quiera cargar.  
  
7. Cuando haya agregado todos los archivos que quiere cargar, haga clic en **Cargar**.  
  
   La herramienta de carga fácil simplifica el proceso de carga de archivos en el servidor que ejecuta Windows Server Essentials. Puede agregar todos los archivos que quiera a la herramienta de carga fácil usando la característica de arrastrar y soltar y, a continuación, cargarlos en las carpetas compartidas del servidor.  
  
> [!NOTE]
>  La carga de varios archivos de forma nativa es compatible con los exploradores web que son compatibles con HTML5. Esta herramienta solo es necesaria cuando el explorador web no admite HTML5.  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Para cargar archivos con la herramienta de carga fácil  
  
1.  En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, a continuación, haga clic en un vínculo de carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2.  En la lista de archivos y carpetas de la carpeta compartida, haga clic en la carpeta donde quiera cargar los archivos y, a continuación, haga clic en **Cargar**. Si la carpeta que desea cargar no existe, haga clic en **Nueva carpeta**, escriba el nombre de la nueva carpeta en el cuadro de diálogo y, a continuación, haga clic en **Aceptar**.  
  
3.  Es posible que deba ejecutar el complemento de soluciones de Windows Server. Si es así, haga clic en la banda amarilla de la parte superior de la pantalla, haga clic en **Complemento** y, a continuación, haga clic en **Ejecutar** en el cuadro de diálogo.  
  
4.  Si aún no está cargada la herramienta de carga fácil, haga clic en **Use la herramienta de carga fácil**.  
  
5.  Puede arrastrar y colocar archivos del Explorador de Windows a la herramienta de carga fácil o hacer clic en **Busque para seleccionar archivos**.  
  
6.  Cuando termine de agregar los archivos que quiera cargar en la carpeta seleccionada, haga clic en **Cargar**.  
  
7.  Cuando los archivos se cargan correctamente, haga clic en **Cerrar**.  
  
#### <a name="download-files-or-folders"></a>Descargar los archivos o carpetas  
  
###### <a name="to-download-a-single-file"></a>Para descargar un solo archivo  
  
1. En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, a continuación, haga clic en un vínculo de carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2. En la lista de archivos de carpeta compartida, haga clic en la casilla de verificación junto al archivo que desea descargar en su equipo doméstico.  
  
3. Haga clic en **Descargar** para comenzar la descarga.  
  
4. En el cuadro de diálogo **Descarga de archivos**, haga clic en **Guardar** para guardar el archivo en el equipo.  
  
5. En el cuadro de diálogo **Guardar como**, seleccione la ubicación para guardar el archivo y, a continuación, haga clic en **Guardar**. No se comprime un solo archivo antes de descargarlo.  
  
   Hay dos opciones para descargar varios archivos o carpetas. Elija la opción que se adapte a sus necesidades:  
  
> [!NOTE]
>  Estas opciones solo están disponibles cuando se descarga varios archivos o carpetas en el equipo.  
  
- **Archivo ejecutable autoextraíble (.exe)**  
  
  > [!NOTE]
  >   Esta sección se aplica a un servidor que ejecuta Windows Server Essentials.  
  
   Un archivo ejecutable autoextraíble es un archivo que se puede descargar y que combina el programa (ejecutable) de descompresión con los archivos comprimidos. Al ejecutar el programa ejecutable, se descomprimen automáticamente los archivos comprimidos (autoextraíble). Se trata de un método habitual de distribuir los datos comprimidos sin preocuparse de si el destinatario tiene la utilidad de descompresión adecuada.  
  
  > [!NOTE]
  >  Esta opción admite los caracteres Unicode.  
  
- **Carpeta comprimida de Windows (.zip)**  
  
   Al comprimir un archivo se crea una versión comprimida del archivo, que es menor que el archivo original. La versión comprimida del archivo tiene la extensión de nombre de archivo .zip. Los tipos de archivo que se reducen al máximo al comprimirlos son los de texto, como .txt, .doc, .xls, y archivos de gráficos que usan tipos de archivo no comprimidos, como .bmp. Algunos archivos de gráficos, como por ejemplo los archivos .jpg y .gif, ya emplean la compresión, y el tamaño del archivo se reduce muy poco durante la compresión. Además, un documento de Word que contiene una gran cantidad de gráficos no se reduce tanto como un documento que está formado principalmente por texto.  
  
  > [!NOTE]
  >  Esta opción proporciona compatibilidad limitada para los nombres de archivo internacionales en Windows Server Essentials.  
  
  Antes de que comience la descarga real, se crea el archivo .exe o .zip. En función del número de archivos y del tamaño total de los archivos que se van a descargar, esta acción puede tardar varios minutos. Después de crearse el archivo de descarga, la descarga del archivo tiene lugar en segundo plano. Esto le permite seguir trabajando mientras se completa el proceso de descarga.  
  
###### <a name="to-download-multiple-files-or-folders"></a>Para descargar varios archivos o carpetas  
  
1.  En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, a continuación, haga clic en un vínculo de carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2.  En la lista de archivos de la carpeta compartida, haga clic en la casilla de los archivos o carpetas que quiera descargar en su equipo doméstico.  
  
3.  Haga clic en **Descargar** para comenzar la descarga.  
  
4.  En el cuadro de diálogo **Elija un formato de descarga**, haga clic para seleccionar la opción de formato de descarga que prefiera y, a continuación, haga clic en **Aceptar**. El archivo comprimido está preparado en la opción de formato que ha seleccionado.  
  
5.  En el cuadro de diálogo **Descarga de archivos**, haga clic en **Guardar** para guardar el archivo en el equipo.  
  
6.  En el cuadro de diálogo **Guardar como**, seleccione la ubicación para guardar el archivo y, a continuación, haga clic en **Guardar**.  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Recuperar archivos comprimidos descargados en su equipo  
  
> [!NOTE]
>   Esta sección se aplica a un servidor que ejecuta Windows Server Essentials.  
  
 Si selecciona varios archivos o carpetas para descargar, puede recibir un archivo ejecutable comprimido autoextraíble (.exe) o un archivo comprimido (.zip).  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Para recuperar un archivo del archivo comprimido (.exe)  
  
1.  En el equipo, haga doble clic para abrir el archivo comprimido.  
  
2.  Siga las instrucciones para extraer los archivos en una carpeta en el equipo.  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Para recuperar un archivo desde el archivo comprimido (.zip)  
  
1.  En el equipo, haga doble clic para abrir el archivo comprimido.  
  
2.  Seleccione los archivos que quiera recuperar y arrástrelos a una carpeta en el equipo donde quiera almacenarlos.  
  
    > [!NOTE]
    >  Si usa un programa de compresión de archivos de terceros, siga los procedimientos de ese programa para extraer los archivos del archivo comprimido.  
  
###  <a name="create-rename-move-delete-or-copy-files-and-folders-in-remote-web-access"></a><a name="BKMK_2"></a>Crear, cambiar el nombre, trasladar, eliminar o copiar archivos y carpetas en acceso Web remoto  
 Puede usar el Acceso web remoto para crear nuevas carpetas en una carpeta compartida existente, cambiar el nombre de archivos y carpetas, moverlos y copiarlos y eliminarlos del servidor.  
  
> [!NOTE]
>  Para agregar nuevas carpetas compartidas en un servidor que ejecuta Windows Server Essentials, debe usar el panel. Para conectarse a la consola del servidor desde el Acceso web remoto, en la pestaña **Equipos**, haga clic en el nombre del servidor, en **Conectar** y, a continuación, siga las instrucciones para iniciar sesión en el servidor. Para obtener información sobre cómo crear carpetas compartidas, vea cómo [agregar o mover una carpeta del servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
##### <a name="to-create-a-new-folder"></a>Para crear una nueva carpeta  
  
1.  En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, después, haga clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2.  En la barra de tareas, haga clic en **Nueva carpeta**.  
  
3.  Escriba un nombre para la carpeta y, a continuación, haga clic en **Aceptar**.  
  
##### <a name="to-rename-a-file-or-folder"></a>Para cambiar el nombre de un archivo o una carpeta  
  
1.  En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, después, haga clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2.  Haga clic en el archivo o carpeta que quiera cambiar y, a continuación, haga clic en **Cambiar nombre**.  
  
3.  Escriba un nombre nuevo en el cuadro de texto y, a continuación, haga clic en **Aceptar**.  
  
##### <a name="to-move-files-or-folders"></a>Para mover archivos o carpetas  
  
1.  En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, después, haga clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2.  Active la casilla junto a los archivos o carpetas que quiera mover. Haga clic en uno de los archivos o carpetas seleccionados y, a continuación, en **Cortar**.  
  
3.  Haga clic con el botón secundario en la carpeta a la que quiere mover los archivos o carpetas y, después, haga clic en **Pegar**.  
  
##### <a name="to-delete-a-file-or-folder"></a>Para eliminar un archivo o carpeta  
  
1.  En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, después, haga clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2.  Active la casilla junto a los archivos o carpetas que desea eliminar, haga clic en uno de los archivos o carpetas seleccionados y, a continuación, haga clic en **Eliminar**.  
  
3.  Para confirmar que quiere eliminar los archivos y carpetas seleccionados, haga clic en **Sí**.  
  
##### <a name="to-copy-files-or-folders"></a>Para copiar archivos o carpetas  
  
1.  En el Acceso web remoto, haga clic en la pestaña **Carpetas compartidas** y, después, haga clic en un vínculo de la carpeta compartida. Se muestra una lista de los archivos y las carpetas de esa carpeta compartida.  
  
2.  Active la casilla junto a los archivos o carpetas que quiera copiar. Haga clic en uno de los archivos o carpetas seleccionados y, a continuación, haga clic en **Copiar**.  
  
3.  Haga clic con el botón secundario en la carpeta a la que quiera copiar los archivos o carpetas y, a continuación, haga clic en **Pegar**.  
  
##  <a name="connect-from-a-mobile-device"></a><a name="BKMK_ConnectMobile"></a>Conectarse desde un dispositivo móvil  
  

-   [Usar el Acceso web remoto desde un dispositivo móvil](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Exploradores web compatibles para dispositivos móviles](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)   

  
###  <a name="use-remote-web-access-from-a-mobile-device"></a><a name="BKMK_8"></a>Usar el acceso Web remoto desde un dispositivo móvil  
 Puede iniciar sesión en el Acceso web remoto desde el smartphone para ver los archivos y las carpetas de las carpetas compartidas en el servidor.  
  
> [!NOTE]
>  También puede descargar y usar la aplicación My Server para Windows Server Essentials desde el [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) para acceder a sus carpetas y archivos multimedia compartidos que se almacenan en el servidor.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Para iniciar sesión en el Acceso web remoto desde un dispositivo móvil  
  
1.  Abra un explorador Web y escriba **https://** _<sudominio \> _**/Remote** en la barra de direcciones.  Asegúrese de incluir s en https.  
  
2.  En la página inicio de sesión de acceso Web remoto, escriba el nombre de usuario y la contraseña en los cuadros de texto y, a continuación, haga clic en la flecha. Ha iniciado sesión en la versión móvil del Acceso web remoto.  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Para cambiar a la versión de escritorio del Acceso web remoto  
  
1.  Abra un explorador Web y escriba **https://** _<sudominio \> _**/Remote** en la barra de direcciones.  Asegúrese de incluir s en https.  
  
2.  En la página inicio de sesión de acceso Web remoto, escriba su nombre de usuario y contraseña en los cuadros de texto, haga clic en **Ver versión de escritorio**y, a continuación, haga clic en la flecha. Ha iniciado sesión en la versión de escritorio del acceso Web remoto.  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Para volver a la versión móvil del Acceso web remoto  
  
1. Cierre la sesión.  
  
2. Abra un explorador Web y escriba **https://** _<sudominio \> _**/remoto/m** en la barra de direcciones. Asegúrese de incluir s en https.  
  
3. Se muestra la versión móvil del acceso Web remoto. En la página inicio de sesión de acceso Web remoto, escriba el nombre de usuario y la contraseña en los cuadros de texto y, a continuación, haga clic en la flecha. Ha iniciado sesión en la versión móvil del acceso Web remoto.  
  
   Puede buscar archivos y carpetas en las carpetas compartidas en el servidor.  
  
###  <a name="supported-web-browsers-for-mobile-devices"></a><a name="BKMK_9"></a>Exploradores Web compatibles para dispositivos móviles  
 Entre los exploradores web compatibles para dispositivos móviles se incluyen los siguientes:  
  
-   Internet Explorer Mobile 6.0 o posterior  
  
-   Safari  
  
-   BlackBerry  
  
-   Symbian 6.0 o posterior  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar Acceso web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Trabajar de forma remota](Work-Remotely-in-Windows-Server-Essentials.md)
  
-   [Usar Windows Server Essentials](Use-Windows-Server-Essentials.md)

