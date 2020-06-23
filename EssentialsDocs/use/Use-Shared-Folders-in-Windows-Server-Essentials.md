---
title: Usar carpetas compartidas en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: cb7f3d7d-4225-409a-9f6b-34a106e8dd24
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0c0e72f37057d50f7d7164ea84c9c7771fca0de8
ms.sourcegitcommit: 56ac7cf3f4bbcc5175f140d2df5f37cc42ba76d1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85217575"
---
# <a name="use-shared-folders-in-windows-server-essentials"></a>Usar carpetas compartidas en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Windows Server Essentials proporciona una ubicación central para todos los datos y archivos a través de las carpetas compartidas que se encuentran en el servidor.  
  
 Hay varias maneras de acceder a las carpetas compartidas en Windows Server Essentials desde un dispositivo conectado al servidor:  
  

-   [Usar el Launchpad de Windows Server Essentials](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingLaunchpad)  
  
-   [Usar Acceso Web remoto](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingRWA)  
  
-   [Usar la aplicación Mi servidor para Windows Phone](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_Phone)  
  
-   [Usar la aplicación Mi servidor para Windows 8](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_App)  
  
##  <a name="using-the-windows-server-essentials-launchpad"></a><a name="BKMK_UsingLaunchpad"></a>Usar el Launchpad de Windows Server Essentials  
 Puede utilizar el Launchpad desde cualquier equipo que esté conectado al servidor usando el Asistente para conectar un equipo al servidor. Para obtener más información acerca de cómo conectar el equipo al servidor, consulte [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

-   [Usar el Launchpad de Windows Server Essentials](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingLaunchpad)  
  
-   [Usar Acceso Web remoto](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingRWA)  
  
-   [Usar la aplicación Mi servidor para Windows Phone](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_Phone)  
  
-   [Usar la aplicación Mi servidor para Windows 8](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_App)  
  
##  <a name="using-the-windows-server-essentials-launchpad"></a><a name="BKMK_UsingLaunchpad"></a>Usar el Launchpad de Windows Server Essentials  
 Puede utilizar el Launchpad desde cualquier equipo que esté conectado al servidor usando el Asistente para conectar un equipo al servidor. Para obtener más información acerca de cómo conectar el equipo al servidor, consulte [Conectar equipos al servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
 Después de conectar el equipo al servidor, se agregará un acceso directo al Launchpad en el área de notificación del escritorio. Haga doble clic en el icono del Launchpad y escriba las credenciales de red para tener acceso a las carpetas compartidas mediante el uso del Launchpad. Mediante el vínculo de carpetas compartidas del Launchpad, puede cargar o descargar archivos en cualquiera de las carpetas compartidas que aparezcan en la lista arrastrando y colocando los archivos del equipo local a las carpetas compartidas. Con las carpetas compartidas podrá retransmitir música y vídeos, reproducir presentaciones o grabar programas de TV en cualquier equipo que esté conectado al servidor, así como reproducir una presentación para ver las imágenes.  
  
 Para obtener más información acerca de Launchpad, consulte [Introducción a Launchpad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
###  <a name="copy-or-move-shared-files-or-folders-using-the-launchpad"></a><a name="BKMK_Launchpad"></a>Copiar o quitar archivos o carpetas compartidos con Launchpad  
 Cuando desee copiar o mover archivos compartidos en Windows Server Essentials usando el Launchpad, haga clic en la pestaña **Carpetas compartidas** del Launchpad.  
  
 Si desea mover un archivo o carpeta de una ubicación a otra en **Carpetas compartidas**, puede usar el método de arrastrar y colocar de la misma manera como haría al mover archivos y carpetas en el equipo. Abra la carpeta que contiene el archivo o carpeta que desea mover. Abra la carpeta a la que desea mover el archivo o carpeta en una ventana diferente. Coloque las ventanas en paralelo en el escritorio, de modo que pueda ver el contenido de ambas, y arrastre el archivo o carpeta desde la primera carpeta a la segunda.  
  
> [!NOTE]
>  Al utilizar el método de arrastrar y colocar, verá que el archivo o carpeta a veces se **copia** y otras veces se **mueve**. Si va a arrastrar un elemento entre dos carpetas almacenadas en el mismo disco duro, se moverá el elemento para que no se creen dos copias del mismo archivo o carpeta en la misma ubicación. Si arrastra el elemento a una carpeta que se encuentra en otra ubicación (por ejemplo, otro equipo) o a un medio extraíble, como una unidad flash USB, el elemento se copia.  
  
 Si desea copiar archivos o carpetas de una ubicación a otra en **Carpetas compartidas**, puede utilizar el método de copiar y pegar de la misma manera en la que copiaría archivos en su equipo. Abra la carpeta que contiene los archivos que se vayan a copiar. Haga clic con el botón derecho en los archivos que quiera copiar y haga clic en **Copiar**. Haga clic con el botón derecho en la carpeta donde desee pegar los archivos copiados y haga clic en **Pegar**.  
  
##  <a name="using-remote-web-access"></a><a name="BKMK_UsingRWA"></a>Usar acceso Web remoto  

 Puede acceder a los archivos y carpetas compartidas desde cualquier equipo remoto usando el sitio web de Acceso Web remoto. Desde un equipo de la red del servidor, para tener acceso al sitio web de acceso Web remoto, abra el explorador de Internet y escriba https://<ServerName \> /Remote. Con Acceso Web remoto puede ver y administrar archivos en las carpetas compartidas. Para obtener instrucciones paso a paso, consulte [usar acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

 Puede acceder a los archivos y carpetas compartidas desde cualquier equipo remoto usando el sitio web de Acceso Web remoto. Desde un equipo de la red del servidor, para tener acceso al sitio web de acceso Web remoto, abra el explorador de Internet y escriba https://<ServerName \> /Remote. Con Acceso Web remoto puede ver y administrar archivos en las carpetas compartidas. Para obtener instrucciones paso a paso, consulte [usar acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
> [!NOTE]
>  En el servidor debe estar activado el Acceso Web remoto para acceder al sitio web de Acceso Web remoto. Para obtener información acerca de cómo administrar el acceso Web remoto, consulte [administrar el acceso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="create-rename-move-delete-or-copy-files-and-folders-in-remote-web-access"></a><a name="BKMK_2"></a>Crear, cambiar el nombre, trasladar, eliminar o copiar archivos y carpetas en acceso Web remoto  

 Puede usar Acceso Web remoto para crear nuevas carpetas en una carpeta compartida existente, cambiar el nombre de archivos y carpetas, mover o copiar los archivos y carpetas, así como eliminar archivos y carpetas en el servidor. Para obtener más información, vea la sección crear, cambiar el nombre, trasladar, eliminar o copiar archivos y carpetas en acceso Web remoto. en el tema [uso de acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="upload-and-download-files-in-remote-web-access"></a><a name="BKMK_3"></a>Cargar y descargar archivos en acceso Web remoto  
 En la pestaña **Carpetas compartidas** de Acceso Web remoto, puede cargar y descargar archivos. Para obtener más información, vea la sección cargar y descargar archivos en acceso Web remoto. en el tema [uso de acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

 Puede usar Acceso Web remoto para crear nuevas carpetas en una carpeta compartida existente, cambiar el nombre de archivos y carpetas, mover o copiar los archivos y carpetas, así como eliminar archivos y carpetas en el servidor. Para obtener más información, vea la sección crear, cambiar el nombre, trasladar, eliminar o copiar archivos y carpetas en acceso Web remoto. en el tema [uso de acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="upload-and-download-files-in-remote-web-access"></a><a name="BKMK_3"></a>Cargar y descargar archivos en acceso Web remoto  
 En la pestaña **Carpetas compartidas** de Acceso Web remoto, puede cargar y descargar archivos. Para obtener más información, vea la sección cargar y descargar archivos en acceso Web remoto. en el tema [uso de acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
##  <a name="using-my-server-app-for-windows-phone"></a><a name="BKMK_Phone"></a>Usar la aplicación My Server para Windows Phone  
 Puede acceder a las carpetas compartidas mediante Windows Phone usando la aplicación Mi servidor para Windows Phone. Puede descargar esta aplicación desde el [Marketplace de Windows Phone](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a).  
  
##  <a name="using-my-server-app-for-windows-8"></a><a name="BKMK_App"></a>Usar la aplicación My Server para Windows 8  
 Puede acceder a las carpetas compartidas mediante Windows 8 usando la aplicación Mi servidor para Windows Phone. Puede descargar esta aplicación desde la [Tienda de aplicaciones de Windows 8](https://windows.microsoft.com/windows-8/apps).  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar carpetas de servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Administrar el almacenamiento de servidor](../manage/Manage-Server-Storage-in-Windows-Server-Essentials.md)  

-   [Conéctese](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Trabajar de forma remota](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Reproducir medios digitales](Play-Digital-Media-in-Windows-Server-Essentials.md)

