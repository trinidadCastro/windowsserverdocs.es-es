---
title: Solucionar problemas de historial de archivos de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 34442565b54b089064c1fa19317a24f591e44fda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Solucionar problemas de historial de archivos de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Solucionar problemas con las copias de seguridad del historial de archivos de usuario  
 Los siguientes problemas pueden surgir mientras administrar copias de seguridad del historial de archivos para un usuario o un equipo que se ha agregado a un servidor que ejecuta Windows Server Essentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>Datos del historial de archivos no se eliminan automáticamente  
 Los datos del historial de archivos no se eliminan automáticamente si:  
  
-   Al eliminar una cuenta de usuario, puedes optar por no eliminar la cuenta de usuario s datos del historial de archivos y optar por eliminar los datos de forma manual.  
  
-   Al intentar eliminar los datos del historial de archivos, los datos del historial de archivos están en uso por otro proceso.  
  
 Para resolver este problema, debes eliminar manualmente el historial de archivos mediante el siguiente procedimiento:  
  
####  <a name="BKMK_manuallyDelete"></a>Para eliminar manualmente las copias de seguridad del historial de archivos para un usuario o un equipo  
  
1.  Iniciar sesión como administrador el servidor.  
  
2.  Ejecuta el Explorador de archivos como administrador.  
  
3.  Navega hasta la carpeta de copias de seguridad de historial de archivos. La ubicación predeterminada es copias de seguridad de C:\ServerFolders\File historial.  
  
4.  Eliminar la carpeta compartida que almacena la copia de seguridad del historial de archivos:  
  
    -   Para eliminar el historial de archivos para que un usuario, eliminar el historial de archivos copia de seguridad de las carpetas secundarias que tiene el nombre de usuario s.  
  
    -   Para eliminar el historial de archivos para un equipo, eliminar el historial de archivos copia de seguridad de las carpetas secundarias que tiene el nombre del equipo. Por ejemplo, si un usuario ha retirado < MyComputer01\ > después de ella empezó a trabajar en su nuevo portátil, < MyComputer02\ >, eliminar historial de C:\ServerFolders\File Backups\\ < MyAccount\ > \\ < MyComputer01\ > después de comprobar con el usuario que ha transferido todos los archivos y carpetas a su equipo portátil nuevo y no tiene necesidad para el historial de archivos en el futuro.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>No se puede aplicar la configuración del historial de archivos a un nuevo usuario  
 Si agregas un nuevo usuario cuyo nombre de usuario es idéntico al nombre de usuario de un usuario que se ha eliminado de Windows Server Essentials, puede producir un error en la configuración del historial de archivos para el nuevo usuario debido a un conflicto de nombres cuando Windows Server Essentials intenta crear una carpeta para almacenar el historial de archivos del nuevo usuario. Para resolver este problema, puede cambiar el nombre de la carpeta de historial de archivos para que el usuario eliminado.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Para buscar el historial de archivos de usuario en el servidor  
  
1.  Iniciar sesión como administrador el servidor.  
  
2.  En el panel de Windows Server Essentials, haz clic en **almacenamiento**.  
  
3.  En la **carpetas del servidor** pestaña, ten en cuenta la ubicación de la carpeta de copias de seguridad de historial de archivos. La ubicación predeterminada es %SystemDrive%\ServerFolders\File Backups\\ historial.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Para resolver problemas de historial de archivos para un usuario nuevo con un conflicto de nombres  
  
1.  Iniciar sesión como administrador el servidor.  
  
2.  Ejecuta el Explorador de archivos como administrador.  
  
3.  Navega hasta la carpeta de copias de seguridad de historial de archivos. La ubicación predeterminada es copias de seguridad de C:\ServerFolders\File historial.  
  
     La carpeta de copias de seguridad de historial de archivos tiene una subcarpeta para cada cuenta de usuario que se ha agregado a Windows Server Essentials. Por ejemplo, el historial de archivos para que el usuario John Smith se almacenará en la subcarpeta Backups\JohnSmith del historial de archivos.  
  
4.  Cambiar el nombre de la subcarpeta por el usuario que se ha eliminado, por ejemplo, * * < *UserName*> _Deleted **. Si ya no necesitas historial de archivos del usuario, puede eliminar la carpeta.  
  

5.  Ahora puedes agregar el nuevo usuario. ¿Para obtener instrucciones, consulta el tema Agregar una cuenta de usuario? en [administrar cuentas de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Se eliminó una cuenta de usuario, pero sigue siendo el historial de archivos del usuario  
 En algunos casos es posible que elija el Administrador de red para quitar un usuario o equipo desde el servidor, pero para mantener el historial de archivos copia de seguridad para uso futuro. Cuando ya no necesitas el historial de archivos, quitar la carpeta de copias de seguridad de historial de archivos para el usuario o el equipo de carpetas compartidas en el servidor. Para ello, consulta [eliminar manualmente las copias de seguridad del historial de archivos para un usuario o un equipo](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5.  Ahora puedes agregar el nuevo usuario. ¿Para obtener instrucciones, consulta el tema Agregar una cuenta de usuario? en [administrar cuentas de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Se eliminó una cuenta de usuario, pero sigue siendo el historial de archivos del usuario  
 En algunos casos es posible que elija el Administrador de red para quitar un usuario o equipo desde el servidor, pero para mantener el historial de archivos copia de seguridad para uso futuro. Cuando ya no necesitas el historial de archivos, quitar la carpeta de copias de seguridad de historial de archivos para el usuario o el equipo de carpetas compartidas en el servidor. Para ello, consulta [eliminar manualmente las copias de seguridad del historial de archivos para un usuario o un equipo](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar la copia de seguridad de cliente](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Compatibilidad con Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Compatibilidad con Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

