---
title: Solucionar problemas del historial de archivos en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed062945-27e9-4572-b1bb-6c8cf1b9c2f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f080bed5714ae4426cc6d0ca8edb5fab2d3c65b2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432474"
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Solucionar problemas del historial de archivos en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Solucionar problemas relacionados con las copias de seguridad del historial de archivos del usuario  
 Pueden producirse los siguientes problemas al administrar las copias de seguridad del historial de archivos de un usuario o de un equipo agregados a un servidor que ejecuta Windows Server Essentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>Los datos del historial de archivos no se eliminan automáticamente  
 Los datos del historial de archivos podrían no eliminarse automáticamente si:  
  
- Al eliminar una cuenta de usuario, decide no eliminar la cuenta de usuario s de datos de historial de archivos y optar por eliminar los datos manualmente.  
  
- Al intentar eliminar los datos del historial de archivos, otro proceso está usando los datos del historial de archivos.  
  
  Para resolver este problema, debe eliminar manualmente el historial de archivos siguiendo el procedimiento que se indica a continuación:  
  
####  <a name="BKMK_manuallyDelete"></a> Para eliminar manualmente las copias de seguridad del historial de archivos para un usuario o un equipo  
  
1.  Inicie sesión en el servidor como administrador.  
  
2.  Ejecute el Explorador de archivos como administrador.  
  
3.  Vaya a la carpeta de copias de seguridad del historial de archivos. La ubicación predeterminada es C:\ServerFolders\File History Backups.  
  
4.  Elimine la carpeta compartida que almacena la copia de seguridad del historial de archivos:  
  
    -   Para eliminar el historial de archivos para un usuario, elimine la carpeta de copia de seguridad secundario de historial de archivos que tiene el nombre de usuario s.  
  
    -   Para eliminar el historial de archivos de un equipo, elimine la carpeta secundaria de copia de seguridad del historial de archivos que tiene el nombre del equipo. Por ejemplo, si un usuario retiró < MyComputer01\> después de que empezó a trabajar en su equipo portátil nuevo, < MyComputer02\>, debe eliminar C:\ServerFolders\File History Backups\\< MyAccount\> \\ < MyComputer01\> después de comprobar con el usuario que se le ha transferido todos los archivos y carpetas de su equipo portátil nuevo y no tiene necesidad de para el historial de archivos en el futuro.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>No se puede aplicar la configuración del historial de archivos a un nuevo usuario  
 Si agrega un usuario nuevo con un nombre idéntico al nombre de un usuario que se eliminó de Windows Server Essentials, puede producirse un error en la configuración del historial de archivos del usuario nuevo, debido a un conflicto de nomenclatura cuando Windows Server Essentials intenta crear una carpeta para almacenar el historial de archivos del usuario nuevo. Para resolver este problema, puede cambiar el nombre de la carpeta del historial de archivos del usuario eliminado.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Para buscar el historial de archivos del usuario en el servidor  
  
1.  Inicie sesión en el servidor como administrador.  
  
2.  En el panel de Windows Server Essentials, haga clic en **Almacenamiento**.  
  
3.  En la pestaña **Carpetas del servidor**, anote la ubicación de la carpeta de copias de seguridad del historial de archivos. La ubicación predeterminada es %SystemDrive%\ServerFolders\File History Backups\\.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Para resolver problemas del historial de archivos de un usuario nuevo con un conflicto de nomenclatura  
  
1.  Inicie sesión en el servidor como administrador.  
  
2.  Ejecute el Explorador de archivos como administrador.  
  
3.  Vaya a la carpeta de copias de seguridad del historial de archivos. La ubicación predeterminada es C:\ServerFolders\File History Backups.  
  
     La carpeta de copias de seguridad del historial de archivos tiene una subcarpeta para cada cuenta de usuario que se agregó a Windows Server Essentials. Por ejemplo, el historial de archivos del usuario John Smith se almacenaría en la subcarpeta File History Backups\JohnSmith.  
  
4.  Cambiar el nombre de la subcarpeta para el usuario que se eliminó, por ejemplo,  **< *UserName*> _Deleted**. Si ya no necesita historial de archivos del usuario, puede eliminar la carpeta.  
  

5.  Ahora puede agregar el usuario nuevo. ¿Para obtener instrucciones, consulte Agregar una cuenta de usuario? en [administrar cuentas de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Se quitó una cuenta de usuario, pero sigue estando de historial de archivos del usuario  
 En algunos casos, el administrador de red puede decidir quitar un usuario o un equipo del servidor, pero conservar la copia de seguridad del historial de archivos para usarlo en el futuro. Cuando ya no necesite el historial de archivos, quite de las carpetas compartidas del servidor la carpeta de copias de seguridad del historial de archivos del usuario o del equipo. Para ello, consulte [To manually delete File History backups for a user or a computer](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5. Ahora puede agregar el usuario nuevo. ¿Para obtener instrucciones, consulte Agregar una cuenta de usuario? en [administrar cuentas de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Se quitó una cuenta de usuario, pero sigue estando de historial de archivos del usuario  
 En algunos casos, el administrador de red puede decidir quitar un usuario o un equipo del servidor, pero conservar la copia de seguridad del historial de archivos para usarlo en el futuro. Cuando ya no necesite el historial de archivos, quite de las carpetas compartidas del servidor la carpeta de copias de seguridad del historial de archivos del usuario o del equipo. Para ello, consulte [To manually delete File History backups for a user or a computer](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Vea también  
  
-   [Administrar copias de seguridad de cliente](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Soporte técnico de Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Soporte técnico de Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

