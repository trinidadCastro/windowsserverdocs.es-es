---
title: "Paso 4: Mover la configuración y los datos a la migración de servidor de destino para Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: db1169a5415039498718a7988c711d068945b4b7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Paso 4: Mover la configuración y los datos a la migración de servidor de destino para Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta sección proporciona información sobre la migración de datos y opciones de configuración del servidor de origen. Mover la configuración y datos al servidor de destino como sigue:  
  
-   [Copiar los datos al servidor de destino](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [Configurar la red](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [Asignar equipos permitidos a cuentas de usuario](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>Copiar los datos al servidor de destino  
 Antes de copiar los datos del servidor de origen al servidor de destino, realice las siguientes tareas:  
  
-   Revisa la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Crear o personalizar las carpetas del servidor de destino para que coincida con la estructura de carpetas que vas a migrar desde el servidor de origen.  
  
-   Revisa el tamaño de cada carpeta y asegúrate de que el servidor de destino tenga suficiente espacio de almacenamiento.  
  
-   Hacer las carpetas compartidas en el servidor de origen de solo lectura para todos los usuarios para que no escritura pueda tener lugar en el disco duro mientras se está copiando archivos al servidor de destino.  
  
-   La **copia de seguridad de equipo cliente** carpeta no se pueden migrar al servidor de destino. Antes de la migración de servidor, asegúrate de que todos los equipos cliente están en buenas condiciones. Después de la migración de servidor, se recomienda que configurar y empezar a las copias de seguridad del equipo de cliente para garantizar que los datos es una copia de seguridad para todos los equipos cliente importante.  
  
-   La **copias de seguridad de historial de archivos** carpeta no se pueden migrar directamente al servidor de destino debido a los cambios de metadatos de copia de seguridad y la estructura de carpetas en Windows Server Essentials. Sin embargo, es posible migrar la **copias de seguridad de historial de archivos** carpeta para un usuario específico en un equipo específico. Para ello, debe buscar el **datos** carpeta en la **copias de seguridad de historial de archivos** carpeta para ese usuario y del equipo, a continuación, copie que **datos** carpeta a la **copias de seguridad de historial de archivos** carpeta en el servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino  
  
1.  Inicia sesión en el servidor de destino como un administrador de dominio y, a continuación, abre una ventana de símbolo del sistema o en un símbolo del sistema de Windows PowerShell.  
  
2.  Si usas la ventana de símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     Donde:  
  
    -   \ < SourceServerName\ > es el nombre del servidor de origen  
  
    -   \ < SharedSourceFolderName\ > es el nombre de la carpeta compartida en el servidor de origen  
  
    -   \ < PathOfTheDestination\ > es la ruta de acceso absoluta donde desea mover la carpeta  
  
    -   \ < SharedDestinationFolderName\ > es la carpeta del servidor de destino al que se copiarán los datos  
  
     Por ejemplo, `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.  
  
3.  Si usas Windows PowerShell, escribe el siguiente comando y, a continuación, presione ENTRAR.  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  Repite este proceso para cada carpeta compartida que vas a migrar desde el servidor de origen.  
  
##  <a name="BKMK_Network"></a>Configurar la red  
  
#### <a name="to-configure-the-network"></a>Para configurar la red  
  
1.  En el servidor de destino, abra el panel.  
  
2.  En el panel **Home** página, haz clic en **instalación**, haz clic en **configurar acceso desde cualquier lugar**y, a continuación, elige el **haga clic para configurar acceso desde cualquier lugar** opción.  
  
3.  Aparece el Asistente de acceso desde cualquier lugar de configurar. Siga las instrucciones del Asistente para configurar el enrutador y nombres de dominio.  
  
 Si el enrutador no admite el marco UPnP o, si se deshabilita el marco UPnP, puede que aparezca un icono de advertencia amarillo junto al nombre del enrutador. Asegúrate de que los siguientes puertos estén abiertos y que haber sido dirigidos a la dirección IP del servidor de destino:  
  
-   Puerto 80: El tráfico HTTP Web  
  
-   Puerto 443: El tráfico de Web HTTPS  
  
> [!NOTE]
>  Si quieres configurar un nombre de dominio público en el servidor de destino, debe liberar el nombre de dominio desde el servidor de origen para evitar la competencia de la actualización dinámica de DNS.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Asignar equipos permitidos a cuentas de usuario  
 Cada cuenta de usuario que se migra desde versiones anteriores de Windows Small Business Server o Windows Server Essentials debe asignarse a uno o más equipos.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario para equipos  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, haz clic en una cuenta de usuario y, a continuación, haz clic en **ver las propiedades de la cuenta**.  
  
4.  Haz clic en el **acceso desde cualquier lugar** pestaña y, a continuación, haz clic en **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web**.  
  
5.  Haz clic en **carpetas compartidas**, haz clic en**equipos**, haz clic en **vínculos de la página principal**y, a continuación, haz clic en **aplicar**.  
  
6.  Haz clic en el **acceso al equipo** pestaña y, a continuación, haz clic en el nombre del equipo al que quieres permitir el acceso.  
  
7.  Repite los pasos 3, 4, 5 y 6 para cada cuenta de usuario.  
  
> [!NOTE]
>  No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.  
  
> [!NOTE]
>  Después de completar la migración, si se produce un problema al crear la primera cuenta de usuario de nuevo en el servidor de destino, quitar la cuenta de usuario que has agregado y, a continuación, volver a crearla.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Ha movido tu configuración y los datos al servidor de destino. Ahora ve a [paso 5: habilitar la redirección de carpetas en la migración de servidor de destino para Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Para ver todos los pasos, consulta [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

