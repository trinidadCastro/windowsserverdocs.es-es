---
title: "Mover la configuración y los datos a la migración de servidor de destino para Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b882e87-347a-4010-b7fd-9599d61198dd
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 97a9f7ec7a9710b66236d8eca05dea2432df04ba
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración y los datos a la migración de servidor de destino para Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mover la configuración y datos al servidor de destino como sigue:  
  

1.  [Copiar los datos al servidor de destino](Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Configurar la red](Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
3.  [Asignar equipos permitidos a cuentas de usuario](Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a>Copiar los datos al servidor de destino  
 Antes de copiar los datos del servidor de origen al servidor de destino, realice las siguientes tareas:  
  
-   Revisa la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Crear o personalizar las carpetas del servidor de destino para que coincida con la estructura de carpetas que vas a migrar desde el servidor de origen.  
  
-   Revisa el tamaño de cada carpeta y asegúrate de que el servidor de destino tenga suficiente espacio de almacenamiento.  
  
-   Asegúrate de las carpetas compartidas en el servidor de origen Read-only para colocar todos los usuarios por lo que no puede tardar escribir en la unidad mientras se está copiando archivos al servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino  
  
1.  Iniciar sesión en el servidor de destino como un administrador de dominio y, a continuación, abre una ventana de comandos.  
  
2.  En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Donde:
     - \ < SourceServerName\ > es el nombre del servidor de origen
     - \ < SharedSourceFolderName\ > es el nombre de la carpeta compartida en el servidor de origen
     - \ < DestinationServerName\ > es el nombre del servidor de destino,
     - \ < SharedDestinationFolderName\ > es la carpeta compartida en el servidor de destino al que se copiarán los datos.  
  
3.  Repite el paso anterior para cada carpeta compartida que vas a migrar desde el servidor de origen.  
  
##  <a name="BKMK_Network"></a>Configurar la red  
 Después de mover el rol DHCP al enrutador, configurar la configuración de red en el servidor de destino.  
  
#### <a name="to-configure-the-network"></a>Para configurar la red  
  
1.  En el servidor de destino, abra el panel.  
  
2.  En el panel **Home** página, haz clic en **instalación**, haz clic en **configurar acceso desde cualquier lugar**y, a continuación, elige el **haga clic para configurar acceso desde cualquier lugar** opción.  
  
3.  Siga las instrucciones del Asistente para configurar el enrutador y nombres de dominio.  
  
 Si el enrutador no admite el marco UPnP o, si se deshabilita el marco UPnP, puede que aparezca un icono de advertencia amarillo junto al nombre del enrutador. Asegúrate de que los siguientes puertos estén abiertos y que haber sido dirigidos a la dirección IP del servidor de destino:  
  
-   Puerto 80: El tráfico HTTP Web  
  
-   Puerto 443: El tráfico de Web HTTPS  
  
##  <a name="BKMK_MapPermittedComputers"></a>Asignar equipos permitidos a cuentas de usuario  
 Cada cuenta de usuario que se migra desde el servidor de origen debe asignarse a uno o más equipos.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario para equipos  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, haz clic en una cuenta de usuario y, a continuación, haz clic en **ver las propiedades de la cuenta**.  
  
4.  Haz clic en el **acceso desde cualquier lugar** pestaña y, a continuación, haz clic en **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web.**.  
  
5.  Selecciona **carpetas compartidas**, selecciona **equipos**, selecciona **vínculos de la página principal**y, a continuación, haz clic en **aplicar**.  
  
6.  Haz clic en el **acceso al equipo** pestaña y, a continuación, haz clic en el nombre del equipo al que quieres permitir el acceso.  
  
7.  Repite los pasos 3, 4, 5 y 6 para cada cuenta de usuario.  
  
> [!NOTE]
>  No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.  
  
> [!NOTE]
>  Después de completar la migración, si se produce un problema al crear la primera cuenta de usuario de nuevo en el servidor de destino, quitar la cuenta de usuario que has agregado y, a continuación, volver a crearla.
