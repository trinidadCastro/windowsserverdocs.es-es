---
title: Restaurar o reparar el servidor que ejecuta Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 26610c591d7bf81e493cf540599d665b37b02dee
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310614"
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Restaurar o reparar el servidor que ejecuta Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 En este tema se proporciona información general y procedimientos para restaurar o reparar un servidor que ejecuta Windows Server Essentials, e incluye las siguientes secciones:  
  
-   [Información general de las restauraciones del sistema de servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Restaurar o reparar la unidad del sistema](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [Restaurar archivos y carpetas en el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="overview-of-server-system-restores"></a><a name="BKMK_Overview"></a>Información general de las restauraciones del sistema de servidor  
 El estado del servidor al realizar una restauración afecta al método de restauración que se encuentre disponible y a la cantidad de datos que puede restaurar.  
  
 Las razones más comunes para restaurar un servidor son las siguientes:  
  
- No es posible inmovilizar o eliminar un virus en el servidor.  
  
- Hay errores en las opciones de configuración del servidor y no se puede iniciar el servidor.  
  
- Se ha remplazado la unidad del sistema.  
  
- Abandona el servidor y desea restaurar a un nuevo servidor.  
  
  O bien puede restaurar el servidor desde una copia de seguridad o restaurar el servidor a la configuración predeterminada de fábrica.  
  
- [Restaurar el servidor a partir de una copia de seguridad](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
- [Restablecer la configuración predeterminada de fábrica del servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="restoring-the-server-from-a-backup"></a><a name="BKMK_RestoreFromBackup"></a>Restaurar el servidor a partir de una copia de seguridad  
 Esta sección proporciona instrucciones sobre qué tipo de copia de seguridad puede elegir.  
  
 Si hay disponible una copia de seguridad, la mejor opción para restaurar el servidor es usar los medios de instalación del fabricante para restaurar a partir de una copia de seguridad externa. La restauración recuperará las carpetas y la configuración del servidor de la copia de seguridad que elija. Solo tiene que configurar las opciones y restaurar los datos que se crean después de la copia de seguridad.  
  
 Si decide recuperar el servidor desde una copia de seguridad anterior, debe elegir la copia de seguridad específica que quiera restaurar y debe tener un archivo de copia de seguridad válido en un disco duro externo que esté conectado directamente al servidor:  
  
- **Si tiene una copia de seguridad reciente y en buen estado del servidor** y sabe que contiene todos los datos críticos, su elección es bastante sencilla. Solo tendrá que recrear los datos que se crearon después de la última copia de seguridad correcta y volver a configurar los cambios de configuración realizados después de la copia de seguridad.  
  
- **Si va a restaurar el servidor debido a un virus**, seleccione una copia de seguridad que sepa que se produjo antes de recibir el virus. Necesitará retroceder varios días para seleccionar una copia de seguridad limpia.  
  
- **Si va a restaurar el servidor debido a una configuración incorrecta**, seleccione una copia de seguridad que sepa que se realizó del cambio de configuración que está causando el problema en el servidor.  
  
  Al restaurar una copia de seguridad, el proceso exacto y el seguimiento necesario dependen del número de unidades de disco duro en el servidor y de si se reemplaza la unidad del sistema:  
  
- **Si el servidor tiene una sola unidad de disco duro y no se reemplaza la unidad**, se conserva la información de partición de la unidad cuando se restaura el servidor. Se restaura el volumen del sistema y se conservan los datos en el volumen restante.  
  
- **Si el servidor tiene una sola unidad de disco duro y se reemplaza la unidad**, se restaura el volumen del sistema y, a continuación, debe restaurar manualmente las carpetas en el volumen de datos. Las carpetas compartidas no predeterminadas deben crearse porque no se crean cuando se vuelve a crear el almacenamiento del servidor.  
  
- **Si el servidor tiene varias unidades de disco duro y no se reemplaza la unidad 0 (que contiene el volumen del sistema)** , se conserva la información de partición de la unidad cuando se restaura el servidor. Se restaura el volumen del sistema y se conservan los datos en todos los volúmenes restantes.  
  
- **Si el servidor tiene varias unidades de disco duro y se reemplaza la unidad 0 (que contiene el volumen del sistema)** , se restaura el volumen del sistema y, a continuación, debe restaurar manualmente las carpetas compartidas previamente almacenadas en la unidad 0.  
  
###  <a name="resetting-the-server-to-factory-default-settings"></a><a name="BKMK_FactoryReset"></a>Restablecer la configuración predeterminada de fábrica del servidor  
 Si no tiene una copia de seguridad desde la que restaurar o, por algún motivo, quiere o necesita realizar una restauración completa del sistema sin restaurar la configuración anterior del servidor, puede realizar una restauración que reinicie el servidor a la configuración predeterminada de fábrica con los medios de instalación o recuperación del fabricante del hardware de servidor.  
  
 Al restaurar el servidor restableciendo la configuración predeterminada de fábrica, se elimina toda la configuración existente y las aplicaciones instaladas en el servidor. En este caso, debe volver a configurar el servidor. Después de restablecer el servidor a la configuración de fábrica, se reinicia el servidor.  
  
 Cuando realiza un restablecimiento de fábrica, puede conservar sus datos o eliminarlos, con los siguientes efectos:  
  
-   Si mantiene todos los datos, los datos del volumen del sistema se eliminan, pero se conservan los datos en otros volúmenes.  
  
    > [!CAUTION]
    >  Si la configuración de disco no coincide con la configuración predeterminada, se eliminarán todos los datos en un disco. Si reemplaza el disco del sistema, el nuevo disco debe ser mayor que el volumen del sistema del disco original.  
    >   
    >  Si la información de partición de una unidad del sistema es ilegible o se reemplaza la unidad del sistema, se quitarán todos los datos de la unidad del sistema, incluso si elige mantener todos los datos.  
  
-   Si va a retirar o reasignar el servidor, elimine todos los datos. Además de la configuración del servidor, otras opciones de configuración y los datos en el volumen del sistema, el resto de datos se elimina, y se vuelven a formatear todos los discos duros en el servidor.  
  
> [!NOTE]
>  Si Espacios de almacenamiento está configurado en el servidor, antes de realizar un restablecimiento de fábrica, debe utilizar la sección **Avanzadas** de la consola **Administrar espacios de almacenamiento** para quitar manualmente todos los espacios de almacenamiento.  
  
 Después de restablecer una fábrica, debe realizar las siguientes tareas:  
  
-   **Volver a configurar el servidor.** En el servidor, use el asistente de configuración del servidor para volver a especificar las opciones de configuración. Para configurar un servidor de Windows Server Essentials administrado de forma remota desde un equipo cliente, abra un explorador Web y escriba **http://** _< nombreservidor\>_ en la barra de direcciones.  
  
-   **Volver a conectar equipos cliente al servidor.** Si un equipo se conectó previamente al servidor, debe desinstalar el software del conector de Windows Server Essentials del equipo antes de volver a conectar el equipo al servidor. Para obtener más información, vea cómo [desinstalar el software del Conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) y [conectar equipos al servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="restore-or-repair-the-system-drive"></a><a name="BKMK_Restore"></a>Restaurar o reparar la unidad del sistema  
 Restaurar o reparar la unidad del sistema del servidor es el primer paso en la restauración del servidor. Después de restaurar la unidad del sistema, debe hacer lo necesario para restaurar las unidades de datos en el servidor y restaurar todo lo que compartía y se perdió en la restauración.  
  
 Existen tres métodos para realizar la restauración:  
  
-   [Restaurar o reparar el servidor mediante medios de instalación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Use los medios de instalación del fabricante del servidor para restaurar una copia de seguridad.  
  
-   **Use los medios de instalación para restaurar el servidor a su configuración predeterminada de fábrica**. Si quiere saber cómo hacerlo, consulte la documentación del fabricante del servidor.  
  
-   [Restaurar o restablecer el servidor desde un equipo cliente mediante el DVD de recuperación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Si necesita restaurar un servidor administrado de forma remota que ejecuta Windows Server Essentials, debe realizar la restauración desde un equipo cliente mediante el DVD de restauración del fabricante del servidor.  
  
###  <a name="restore-or-repair-your-server-using-installation-media"></a><a name="BKMK_Restore_1"></a>Restaurar o reparar el servidor mediante medios de instalación  
 El siguiente procedimiento describe cómo restaurar la unidad del sistema del servidor desde una copia de seguridad mediante el medio de instalación de Windows Server Essentials. Para obtener información sobre cómo usar los medios de instalación para restaurar la configuración predeterminada de fábrica, consulte la documentación del fabricante del servidor.  
  
> [!NOTE]
>  Si el servidor usa espacios de almacenamiento y va a restaurar los datos a un nuevo servidor, debe recuperar primero la unidad del sistema y, a continuación, iniciar sesión en el panel de Windows Server Essentials, configurar espacios de almacenamiento de forma similar a como se hace en el servidor anterior y, a continuación, recuperar el volúmenes de datos.  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Para restaurar la unidad del sistema del servidor desde una copia de seguridad mediante los medios de instalación  
  
1.  Inserte el DVD de instalación de Windows Server Essentials en la unidad de DVD del servidor, reinicie el servidor y, a continuación, presione cualquier tecla para iniciar desde el DVD.  
  
    > [!NOTE]
    >  Si no se inicia automáticamente el proceso de restauración, compruebe la configuración del BIOS del servidor para asegurarse de que la unidad de DVD aparezca la primera en el menú de arranque.  
  
     O bien:  
  
     Si el fabricante había cargado los medios de instalación en el servidor, presione F8 durante el inicio para arrancar desde los medios de instalación.  
  
2.  Una vez que Windows Server cargue los archivos, elija su idioma y otras preferencias y, a continuación, haga clic en **Siguiente**.  
  
3.  En la página siguiente del asistente, haga clic en **Reparar el equipo**.  
  
    > [!CAUTION]
    >  No elija la opción **Instalar ahora**. Esta opción le guía a través de una instalación completa del sistema que elimina todas las configuraciones y todos los datos de la unidad del sistema.  
  
4.  En la página **Elegir una opción**, haga clic en **Solucionar problemas**.  
  
5.  En la página **recuperación de imagen del sistema** , seleccione el sistema actual, ya sea **Windows Server Essentials** o **Windows Server Essentials**.  
  
     Se abrirá el asistente para crear una nueva imagen de su equipo.  
  
6.  En la página **Seleccionar una copia de seguridad de imagen del sistema**, puede usar la copia de seguridad más reciente o seleccionar una copia de seguridad anterior. El sistema se restaurará al estado que tenía en el momento de la copia de seguridad que elija para restaurar o reparar el servidor. Se deben recrear los datos que se agregaron y los cambios de configuración realizados después de la copia de seguridad.  
  
     Seleccione una de las siguientes opciones y, a continuación, haga clic en **Siguiente**:  
  
    -   **Usar la imagen de sistema más reciente disponible (recomendado)**  
  
    -   **Seleccionar una imagen del sistema**  
  
    > [!NOTE]
    >  Si tiene una copia de seguridad reciente y en buen estado del servidor y sabe que contiene todos los datos críticos, su elección es bastante sencilla. Solo tendrá que recrear los datos que se crearon después de la última copia de seguridad correcta y volver a configurar los cambios de configuración realizados después de la copia de seguridad.  
    >   
    >  Si va a restaurar el servidor debido a un virus, seleccione una copia de seguridad que sepa que se produjo antes de recibir el virus. Necesitará retroceder varios días para seleccionar una copia de seguridad limpia.  
    >   
    >  Si va a restaurar el servidor debido a una configuración incorrecta, seleccione una copia de seguridad que sepa que se realizó antes del cambio de configuración que está causando el problema en el servidor.  
  
7.  Siga las instrucciones del asistente para completar la restauración del sistema.  
  
8.  Cuando el servidor se restaure correctamente, quite el DVD de instalación si ha usado uno y, a continuación, reinicie el servidor.  
  
> [!NOTE]
>  Para restaurar y compartir carpetas en el servidor, debe realizar pasos adicionales. Para obtener más información, vea cómo [restaurar archivos y carpetas en el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
###  <a name="restore-or-reset-your-server-from-a-client-computer-using-the-recovery-dvd"></a><a name="BKMK_Restore_2"></a>Restaurar o restablecer el servidor desde un equipo cliente mediante el DVD de recuperación  
 En Windows Server Essentials, puede iniciar el servidor desde una unidad flash USB de arranque que cree y, a continuación, recuperar el servidor desde un equipo cliente mediante el DVD de recuperación que recibió del fabricante del servidor. El equipo cliente debe estar en la misma red que el servidor. Este método no está disponible en Windows Server Essentials.  
  
 El siguiente procedimiento incluye pasos generales para realizar una restauración del servidor. Los pasos pueden aplicarse también para restaurar desde una copia o restaurar a la configuración predeterminada de fábrica. Para obtener instrucciones más específicas, consulte la documentación del fabricante del servidor.  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Restaurar o restablecer el servidor desde un equipo cliente mediante el DVD de recuperación  
  
1.  Inserte los medios de recuperación del servidor de Windows Server Essentials que recibió del fabricante del servidor en un equipo cliente.  
  
     Se abrirá el asistente para recuperar el servidor.  
  
2.  Siga las instrucciones del asistente para crear una unidad flash USB de arranque para iniciar el servidor en modo de recuperación.  
  
3.  Una vez que el asistente para recuperar el servidor prepare la unidad flash USB de arranque, inserte la unidad flash en el servidor y, a continuación, inicie el servidor en modo de recuperación. Para obtener información sobre cómo iniciar el servidor en modo de recuperación, consulte la documentación del fabricante del hardware del servidor.  
  
     Tras iniciar el servidor en modo de recuperación, el asistente para recuperar el servidor busca el servidor y, a continuación, establece una conexión.  
  
4.  Siga las instrucciones del asistente para terminar la restauración.  
  
> [!NOTE]
>  Este método de recuperación del servidor omite los dispositivos de almacenamiento externos que están conectados al servidor durante la recuperación. Si desea borrar los datos en un dispositivo de almacenamiento externo, debe hacerlo manualmente.  
  
> [!NOTE]
>  Si ha creado carpetas compartidas adicionales en el servidor, después de restaurar los datos de la copia de seguridad, es posible que el servidor no reconozca las carpetas compartidas adicionales. Debe compartir de nuevo esas carpetas. Para obtener más información, vea cómo [restaurar archivos y carpetas en el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
##  <a name="restore-files-and-folders-on-the-server"></a><a name="BKMK_RestoreFilesAndFolders"></a>Restaurar archivos y carpetas en el servidor  
 En función del método que use para restaurar o reparar el servidor y el tipo de almacenamiento del servidor, es posible que necesite recuperar los volúmenes de datos después de restaurar la unidad del sistema. En algunos casos, deberá compartir carpetas existentes de nuevo para que el servidor las reconozca.  
  
 Estos son algunos casos en los que es posible que necesite restaurar archivos y carpetas:  
  
-   [Restaurar archivos y carpetas desde una copia de seguridad en línea](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Si reemplaza el disco del sistema, o si la información de las particiones en el disco del sistema es ilegible, puede restaurar el sistema, pero no puede restaurar datos desde otros volúmenes en este disco. Para restaurar archivos y carpetas desde otros volúmenes de datos, debe usar el asistente para restaurar archivos y carpetas.  
  
-   [Restore shared folders on the server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Si ha creado carpetas compartidas adicionales en el servidor, después de restaurar la copia de seguridad de la unidad del sistema, las carpetas compartidas permanecen en la partición de datos o se restauraron en la partición de datos, pero puede que el servidor no las reconozca. Debe compartir de nuevo esas carpetas.  
  
###  <a name="restore-files-and-folders-from-a-server-backup"></a><a name="BKMK_RestoreFilesFromBackup"></a>Restaurar archivos y carpetas desde una copia de seguridad del servidor  
 El asistente para restaurar archivos y carpetas le ayuda a proteger los datos si el disco duro deja de funcionar o si los archivos se borran accidentalmente. Con copias de seguridad de Windows Server Essentials, puede crear una copia de todos los datos en el disco duro y almacenar los datos en un dispositivo de almacenamiento externo. Si los datos originales del disco duro se borran accidentalmente, se sobrescriben o se vuelven inaccesibles debido a un funcionamiento incorrecto, puede restaurar los datos de la copia de seguridad. La restauración de archivos o carpetas asistente le ayudará a restaurar un solo archivo o carpeta, varios archivos o carpetas o todo un disco duro desde una copia de seguridad existente.  
  
 Después de una restauración del sistema, es posible que deba usar el asistente para restaurar archivos y carpetas que no se conservaron durante la restauración. Por ejemplo, si se reemplaza el disco del sistema, o si la información de las particiones en el disco del sistema es ilegible, no podrá restaurar datos desde otros volúmenes en el disco del sistema.  
  
> [!NOTE]
>  No puede usar el asistente para restaurar archivos y carpetas para restaurar la unidad del sistema completamente. Para obtener información sobre cómo restaurar el sistema completo, consulte cómo [restaurar o reparar el servidor usando medios de instalación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) o [restaurar o reiniciar el servidor desde un equipo cliente usando el DVD de recuperación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar archivos y carpetas desde una copia de seguridad del servidor  
  
1.  Abra el panel de Windows Server Essentials y, a continuación, haga clic en la pestaña **dispositivos** .  
  
2.  Haga clic en el nombre del servidor y, a continuación, haga clic en **Restaurar los archivos o las carpetas del servidor** en el panel **Tareas**.  
  
     Se abrirá el asistente de restauración de archivos o carpetas.  
  
3.  Siga las instrucciones del asistente para restaurar los archivos o carpetas.  
  
> [!WARNING]
>  Para obtener más información sobre la copia de seguridad y la restauración de archivos y carpetas, vea [administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
###  <a name="restore-shared-folders-on-the-server"></a><a name="BKMK_ConfigreSharedFolders"></a>Restaurar carpetas compartidas en el servidor  
 Después de restaurar la unidad del sistema del servidor, si las carpetas compartidas siguen en la partición de datos o se restauraron en la partición de datos, puede que tenga que volver a configurar las carpetas compartidas para que el servidor reconozca las carpetas. El siguiente procedimiento describe cómo agregar carpetas compartidas que se han compartido antes.  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Para agregar una carpeta existente a las carpetas compartidas del servidor  
  
1.  En el Explorador de archivos, busque la carpeta compartida en el disco duro.  
  
2.  En la carpeta compartida, haga clic con el botón secundario en **Propiedades**, haga clic en **Compartir** y, a continuación, anote los permisos de carpeta.  
  
3.  Inicie sesión en el panel de Windows Server Essentials, haga clic en la pestaña **almacenamiento** y, a continuación, haga clic en **Agregar una carpeta** en el panel **tareas de carpetas del servidor** .  
  
     Se abrirá el asistente para agregar carpetas.  
  
4.  Escriba un nombre para el recurso compartido en el cuadro **Nombre**.  
  
5.  Haga clic en **examinar**, vaya a *< unidad\>\\< ServerName\>* \carpetasdeservidor (por ejemplo, *d:\Contoso\ServerFolders*), seleccione la carpeta que desea compartir y, a continuación, haga clic en **Aceptar**.  
  
6.  Haga clic en **Siguiente**.  
  
7.  Especifique los permisos que anotó en el paso 2 y, a continuación, haga clic en **Agregar una carpeta**.  
  
    > [!IMPORTANT]
    >  Estos permisos reemplazarán los permisos existentes que no se agregaron a la carpeta mediante el panel de Windows Server Essentials.  
  
> [!IMPORTANT]
>  Cuando agregue las carpetas a la lista de carpetas compartidas, asegúrese de que estas se incluyan en la copia de seguridad del servidor. Para obtener información sobre cómo agregar carpetas a la copia de seguridad del servidor, consulte cómo [establecer una copia de seguridad o personalizar la copia de seguridad de servidor](Set-up-or-customize-server-backup.md).  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
