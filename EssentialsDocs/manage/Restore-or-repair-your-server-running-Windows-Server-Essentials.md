---
title: Restaurar o reparar el servidor que ejecuta Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1ebc539928d13b0d34dfe5a0ee57ce6e98088e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Restaurar o reparar el servidor que ejecuta Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Este tema proporciona una introducción y la compatibilidad con los procedimientos para restaurar o reparar un servidor que ejecuta Windows Server Essentials e incluye las siguientes secciones:  
  
-   [Información general sobre la restauración del sistema de servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Restaurar o reparar la unidad del sistema](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [Restaurar archivos y carpetas en el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a>Información general sobre la restauración del sistema de servidor  
 El estado del servidor al realizar una restauración afecta al método de restauración que está disponible y cómo completa una restauración puede llevar a cabo.  
  
 Las razones más habituales para restaurar un servidor son:  
  
-   Un virus en el servidor no puede ser inoculado o eliminado.  
  
-   Las opciones de configuración del servidor son incorrectas y no se puede iniciar el servidor.  
  
-   Reemplaza la unidad del sistema.  
  
-   Vaya a retirar el servidor, y que deseas restaurar a un servidor nuevo.  
  
 O bien puede restaurar el servidor desde una copia de seguridad, o puedes restaurar el servidor a la configuración predeterminada de fábrica.  
  
-   [Restaurar el servidor desde una copia de seguridad](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
-   [Restablecimiento del servidor para la configuración predeterminada de fábrica](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a>Restaurar el servidor desde una copia de seguridad  
 Esta sección contiene instrucciones sobre qué tipo de copia de seguridad para elegir.  
  
 Si hay disponible una copia de seguridad, tu mejor opción para restaurar el servidor es usar los medios de instalación de s fabricante para restaurar una copia de seguridad externo. La restauración recuperará la configuración del servidor y carpetas de la copia de seguridad que elijas. Solo debes establecer la configuración y restaurar los datos que creamos después de la copia de seguridad.  
  
 Cuando eliges recuperar el servidor mediante la restauración desde una copia de seguridad anterior, debes elegir la copia de seguridad específico que desee restaurada y debe tener un archivo de copia de seguridad válido en un disco duro externo que está conectado directamente al servidor:  
  
-   **Si tienes una copia de seguridad correcta muy reciente del servidor**, y se sabe que contiene la copia de seguridad de todos los datos críticos, su elección es bastante sencilla. Solo deberás volver a crear los datos que se crean después de las última copia de seguridad y reconfigure configuración cambios positivos después de la copia de seguridad.  
  
-   **Si va a restaurar el servidor debido a un virus**, selecciona una copia de seguridad que sabes que se ha producido antes de recibir el virus. Es posible que debas volver varios días para seleccionar una copia de seguridad que está limpio.  
  
-   **Si va a restaurar el servidor debido a la configuración de la configuración incorrecta**, selecciona una copia de seguridad que sabes que se ha producido antes de la configuración de cambio que está causando el problema en el servidor.  
  
 Cuando se restaura desde una copia de seguridad, el proceso exacto y el seguimiento necesario dependen el número de unidades de disco duro en el servidor y si se reemplaza la unidad del sistema:  
  
-   **Si el servidor tiene una sola unidad de disco dura y no se reemplaza la unidad**, queda intacta la información de particiones de disco cuando se restaura el servidor. Se restaura el volumen del sistema, y se conservan los datos en el volumen restante.  
  
-   **Si el servidor tiene una sola unidad de disco dura y se reemplaza la unidad**, se restaura el volumen del sistema y, a continuación, debe restaurar manualmente carpetas en el volumen de datos. Todas las carpetas compartidas predeterminadas no deben crearse porque no se crean cuando se vuelve a crear el almacenamiento del servidor.  
  
-   **Si el servidor tiene varias unidades de disco duro y unidad 0 (contiene el volumen del sistema) no se reemplaza**, queda intacta la información de particiones de disco cuando se restaura el servidor. Se restaura el volumen del sistema, y se conservan los datos de todos los volúmenes restantes.  
  
-   **Si el servidor tiene varias unidades de disco duro y se ha reemplazado el disco 0 (que contiene el volumen del sistema)**, se restaura el volumen del sistema y, a continuación, debe restaurar manualmente todas las carpetas compartidas que anteriormente se almacenaban en el disco 0.  
  
###  <a name="BKMK_FactoryReset"></a>Restablecimiento del servidor para la configuración predeterminada de fábrica  
 Si no tienes una copia de seguridad que se puede restaurar desde, o por algún motivo que desee o necesite realizar una restauración completa del sistema sin restaurar la configuración del servidor anterior, puedes realizar una restauración que restablece el servidor para la configuración predeterminada de fábrica mediante la instalación o los medios de recuperación del fabricante del hardware de servidor.  
  
 Al restaurar el servidor al restablecer la configuración predeterminada de fábrica, se eliminan todas las opciones de configuración existentes y las aplicaciones instaladas en el servidor, y debe configurar el servidor de nuevo. Después de restablece una fábrica, reinicia el servidor.  
  
 Al realizar un restablecimiento de fábrica, puedes elegir mantener los datos o eliminarlo, con los siguientes efectos:  
  
-   Si eliges mantener todos los datos, todos los datos en el volumen del sistema se elimina, pero se conservan los datos de otros volúmenes.  
  
    > [!CAUTION]
    >  Si la configuración de disco no coincide con la configuración predeterminada, se eliminarán todos los datos en un disco. Si has reemplazado el disco del sistema, el nuevo disco debe ser mayor que el volumen del sistema de s disco original.  
    >   
    >  Si la información de partición de una unidad del sistema no es legible o reemplazar la unidad del sistema, se eliminarán todos los datos de la unidad del sistema, incluso si opta por mantener todos los datos.  
  
-   Si tienes previsto retiran o reutilizan el servidor, elegir eliminar todos los datos. Además de la configuración del servidor, otras opciones de configuración y los datos en el volumen del sistema, se elimina todos los demás datos y se cambian todas las unidades de disco duro en el servidor.  
  
> [!NOTE]
>  Si los espacios de almacenamiento está configurado en el servidor, antes de realizar un restablecimiento de fábrica, debes usar el **avanzadas** sección de la **administrar espacios de almacenamiento** consola para quitar todos los espacios de almacenamiento de forma manual.  
  
 Después de restablece una fábrica, deberás realizar las siguientes tareas:  
  
-   **Volver a configurar el servidor.** En el servidor, usa al Asistente para configurar servidor volver a introducir valores de configuración. Para configurar un servidor de Windows Server Essentials administrado de forma remota desde un equipo cliente, abre un explorador web y, a continuación, escribe **http://***< YourServerName\ >* en la barra de direcciones.  
  
-   **Vuelve a conectar los equipos cliente al servidor.** Si un equipo estaba conectado anteriormente en el servidor, debe desinstalar el software del conector de Windows Server Essentials desde el equipo antes de conectarte de nuevo el equipo al servidor. Para obtener más información, consulta [desinstalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) y [conectar equipos con el servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_Restore"></a>Restaurar o reparar la unidad del sistema  
 Es el primer paso para la restauración del servidor restaurar o reparar la unidad del sistema de servidor. Después de restaurar la unidad del sistema, que hará lo que es necesario para restaurar las unidades de datos en el servidor y restauración compartir que se ha perdido en la restauración.  
  
 Existen tres métodos para realizar la restauración:  
  
-   [Restaurar o reparar el servidor con medios de instalación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Usar los medios de instalación del fabricante del servidor para restaurar una copia de seguridad.  
  
-   **Usar los medios de instalación para restaurar el servidor en la configuración de fábrica predeterminado**. Para saber cómo hacerlo en el servidor, consulta la documentación del fabricante del servidor.  
  
-   [Restaurar o restablecer tu servidor desde un equipo cliente mediante el DVD de recuperación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Si necesitas restaurar un servidor de administración remota que se ejecuta Windows Server Essentials, debes realizar la restauración desde un equipo cliente mediante el uso de la restauración DVD desde el fabricante del servidor.  
  
###  <a name="BKMK_Restore_1"></a>Restaurar o reparar el servidor con medios de instalación  
 El siguiente procedimiento describe cómo restaurar la unidad del sistema server desde una copia de seguridad mediante los medios de instalación de Windows Server Essentials. (Para obtener información sobre cómo usar los medios de instalación para restaurar en la configuración predeterminada de fábrica, consulta la documentación del fabricante del servidor).  
  
> [!NOTE]
>  Si el servidor usa espacios de almacenamiento y va a restaurar los datos a un servidor nuevo, debes recuperar la unidad del sistema en primer lugar y, a continuación, iniciar sesión en el escritorio de Windows Server Essentials, configurar espacios de almacenamiento de una forma similar como en el servidor antiguo y después recuperar los volúmenes de datos.  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Para restaurar la unidad del sistema de servidor de una copia de seguridad con medios de instalación  
  
1.  Inserte el DVD de instalación de Windows Server Essentials en la unidad de DVD de servidor, reinicie el servidor y, a continuación, presione cualquier tecla para iniciar desde el DVD.  
  
    > [!NOTE]
    >  Si el proceso de restauración no se inicia automáticamente, comprueba la configuración del BIOS de tu servidor para asegurarse de que la unidad de DVD aparece en primer lugar en el menú de arranque.  
  
     - O bien-  
  
     Si el fabricante precargadas los medios de instalación en el servidor, presiona F8 durante el inicio para iniciar desde el medio de instalación.  
  
2.  El servidor de Windows cargar archivos, elige tu idioma y otras preferencias y, a continuación, haz clic en **siguiente**.  
  
3.  En la siguiente página del asistente, haz clic en **reparar el equipo**.  
  
    > [!CAUTION]
    >  No elige la **instalar ahora** opción. Esta opción le guiará a través de una instalación completa del sistema que elimina todas las opciones de configuración y todos los datos en la unidad del sistema.  
  
4.  En la **elige una opción** página, haz clic en **solucionar**.  
  
5.  En la **recuperación de imagen del sistema** página, selecciona el sistema actual? cualquier **Windows Server Essentials** o **Windows Server Essentials**.  
  
     Abre el Asistente para crear una nueva imagen su equipo.  
  
6.  En la **selecciona una copia de seguridad de imagen de sistema** página, puedes elegir utilizar la copia de seguridad más reciente o puedes seleccionar una copia de seguridad anterior. El sistema se restaurará al estado que estaba en el momento de la copia de seguridad que elijas para restaurar o reparar el servidor. Los datos que se ha agregado o cambios en la configuración que se realizaron después de guarda la copia de seguridad deben recrearse.  
  
     Selecciona una de las siguientes opciones y, a continuación, haz clic en **siguiente**:  
  
    -   **Usar la imagen de sistema más reciente disponible (recomendada)**  
  
    -   **Selecciona una imagen del sistema**  
  
    > [!NOTE]
    >  Si tienes una copia de seguridad correcta muy reciente del servidor y se sabe que la copia de seguridad contiene todos los datos críticos, su elección es bastante sencilla. Solo deberás volver a crear los datos que se crean después de las última copia de seguridad y reconfigure configuración cambios positivos después de la copia de seguridad.  
    >   
    >  Si va a restaurar el servidor debido a un virus, selecciona una copia de seguridad que sabes que se ha producido antes de recibir el virus. Es posible que debas volver varios días para seleccionar una copia de seguridad que está limpio.  
    >   
    >  Si va a restaurar el servidor debido a la configuración de la configuración incorrecta, selecciona una copia de seguridad que sabes que se ha producido antes del cambio de opción de configuración que está causando el problema en el servidor.  
  
7.  Sigue las instrucciones del Asistente para completar la restauración del sistema.  
  
8.  Después de que el servidor se restauró correctamente, quita el DVD de instalación si usaste una y, a continuación, reinicia el servidor.  
  
> [!NOTE]
>  Para restaurar y compartir carpetas en el servidor, debes realizar pasos adicionales. Para obtener más información, consulta [restaurar archivos y carpetas en el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
###  <a name="BKMK_Restore_2"></a>Restaurar o restablecer tu servidor desde un equipo cliente mediante el DVD de recuperación  
 En Windows Server Essentials, puede iniciar el servidor desde una unidad flash USB de arranque que crees y, a continuación, recuperación el servidor desde un equipo cliente mediante el uso de la recuperación de DVD que has recibido desde el fabricante del servidor. El equipo cliente debe estar en la misma red que el servidor. Este método no está disponible en Windows Server Essentials.  
  
 El siguiente procedimiento proporciona los pasos generales para realizar una restauración del servidor. Los pasos son igualmente aplicables para restaurar desde una copia o restaurar en la configuración predeterminada de fábrica. Para obtener instrucciones más específicas, consulta la documentación del fabricante del servidor.  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Para restaurar o restablecer el servidor desde un equipo cliente mediante el DVD de recuperación  
  
1.  Inserta los medios de recuperación del servidor de Windows Server Essentials que hayas recibido desde el fabricante del servidor en un equipo cliente.  
  
     Abre el Asistente para recuperar tu servidor.  
  
2.  Sigue las instrucciones del Asistente para crear una unidad flash USB de arranque que se usará para iniciar el servidor en modo de recuperación.  
  
3.  Después de que el Asistente para recuperar su servidor prepara la unidad flash USB de arranque, inserta la unidad flash en el servidor y, a continuación, inicia el servidor en modo de recuperación. Para obtener información sobre cómo iniciar el servidor en modo de recuperación, consulta la documentación del fabricante del hardware de servidor.  
  
     Después de iniciar el servidor en modo de recuperación, el Asistente para recuperar su servidor busca el servidor y, a continuación, Establece una conexión.  
  
4.  Sigue las instrucciones del Asistente para terminar de restaurar el servidor.  
  
> [!NOTE]
>  Este método de recuperación de servidor omite los dispositivos de almacenamiento externo que están conectados al servidor durante la recuperación. Si deseas eliminar los datos en un dispositivo de almacenamiento externo, debe hacerlo manualmente.  
  
> [!NOTE]
>  Si has creado adicionales carpetas compartidas en el servidor, después de restaurar los datos de la copia de seguridad, no se reconozcan las carpetas compartidas adicionales por el servidor. Debes volver a compartir esas carpetas. Para obtener más información, consulta [restaurar archivos y carpetas en el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a>Restaurar archivos y carpetas en el servidor  
 Según el método que usaste para restaurar o reparar el servidor, y usa el tipo de almacenamiento del servidor, es posible que debas recuperar los volúmenes de datos después de restaurar la unidad del sistema. En algunos casos, es posible que debas compartir carpetas existentes de nuevo para que el servidor reconoce.  
  
 A continuación es algunos ejemplos de cuando necesites restaurar archivos y carpetas:  
  
-   [Restaurar archivos y carpetas desde una copia de seguridad del servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Si has reemplazado el disco del sistema, o si la información de particiones en el disco del sistema es legible, puedes restaurar el sistema, pero no puede restaurar los datos de otros volúmenes en este disco. Para restaurar archivos y carpetas de otros volúmenes de datos, debes usar para restaurar archivos y carpetas asistente.  
  
-   [Restaurar carpetas compartidas en el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Si has creado adicionales carpetas compartidas en el servidor, después de restaurar la copia de seguridad de la unidad del sistema, las carpetas compartidas todavía están en la partición de datos o se restauraron a la partición de datos, pero no se reconozcan por el servidor. Debes volver a compartir esas carpetas.  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a>Restaurar archivos y carpetas desde una copia de seguridad del servidor  
 Para restaurar archivos y carpetas asistente ayuda a proteger los datos si el disco duro deja de funcionar o se borran accidentalmente los archivos. Con la copia de seguridad de Windows Server Essentials, puedes crear una copia de todos los datos en el disco duro y almacena los datos en un dispositivo de almacenamiento externo. Si los datos originales del disco duro se borran accidentalmente, se sobrescribe, o se vuelve inaccesibles debido a un error de funcionamiento, puedes restaurar los datos de la copia de seguridad. La restaurar archivos o carpetas asistente te ayuda a restaurar un solo archivo o carpeta, varios archivos o carpetas o una unidad de disco dura completa desde una copia de seguridad existente.  
  
 Después de Restaurar sistema, es posible que debas usar para restaurar archivos y carpetas Asistente para restaurar archivos y carpetas que no se conservan durante la restauración. Por ejemplo, si has reemplazado el disco del sistema, o si la información de particiones en el disco del sistema es legible, no se puede restaurar datos de otros volúmenes en el disco del sistema.  
  
> [!NOTE]
>  No puedes usar para restaurar archivos y carpetas Asistente para restaurar la unidad del sistema completo. Para obtener información sobre cómo restaurar el sistema completo, consulta [restaurar o reparar el servidor con medios de instalación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) o [restaurar o restablecer tu servidor desde un equipo cliente mediante el DVD de recuperación](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar archivos y carpetas desde una copia de seguridad del servidor  
  
1.  Abre el panel de Windows Server Essentials y, a continuación, haz clic en el **dispositivos** pestaña.  
  
2.  Haz clic en el nombre del servidor y, a continuación, haz clic en **restaurar archivos o carpetas del servidor** en la **tareas** panel.  
  
     Abre el Asistente de carpetas y restaurar archivos.  
  
3.  Sigue las instrucciones del Asistente para restaurar los archivos o carpetas.  
  
> [!WARNING]
>  Para obtener más información sobre cómo hacer copias de seguridad y restaurar archivos y carpetas, consulta [administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_ConfigreSharedFolders"></a>Restaurar carpetas compartidas en el servidor  
 Después de restaurar la unidad del sistema servidor s, si todavía están en la partición de datos de las carpetas compartidas o se restauraron a la partición de datos, debes configurar las carpetas compartidas de nuevo para el servidor reconocer las carpetas. El siguiente procedimiento describe cómo agregar carpetas compartidas que se han compartido antes.  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Para agregar una carpeta existente en el servidor de las carpetas compartidas  
  
1.  En el Explorador de archivos, busque la carpeta compartida en el disco duro.  
  
2.  En la carpeta compartida, haz clic **propiedades**, haz clic en el **compartir** pestaña y, a continuación, anota los permisos de carpetas.  
  
3.  Iniciar sesión en el escritorio de Windows Server Essentials, haz clic en el **almacenamiento** pestaña y, a continuación, haz clic en **agregar una carpeta** en la **tareas de carpetas de servidor** panel.  
  
     Agregar una carpeta se abrirá.  
  
4.  Escribe un nombre para el recurso compartido en la **nombre** cuadro.  
  
5.  Haz clic en **examinar**, navegar a *< unidad > compartida\ \\ < ServerName\ >*\ServerFolders (por ejemplo *d:\Contoso\ServerFolders*), selecciona la carpeta que desea compartir y, a continuación, haz clic en **Aceptar**.  
  
6.  Haz clic en **siguiente**.  
  
7.  Especifica los permisos que anotó en el paso 2 y, a continuación, haz clic en **Agregar carpeta**.  
  
    > [!IMPORTANT]
    >  Estos permisos reemplazará los permisos que no se han agregado a la carpeta mediante el panel de Windows Server Essentials.  
  
> [!IMPORTANT]
>  Cuando termines de agregar carpetas a la lista de carpetas compartidas, asegúrate de que se incluyen las carpetas en la copia de seguridad del servidor. Para obtener información sobre cómo agregar carpetas a la copia de seguridad del servidor, consulte [establecer hacia arriba o personalizar copias de seguridad del servidor](Set-up-or-customize-server-backup.md).  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar la copia de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
