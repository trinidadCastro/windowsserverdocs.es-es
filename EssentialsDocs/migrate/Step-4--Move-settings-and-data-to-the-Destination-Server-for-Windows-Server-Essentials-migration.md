---
title: 'Paso 4: Mover la configuración y los datos al servidor de destino para la migración a Windows Server Essentials'
description: Describe cómo usar Windows Server Essentials
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
ms.openlocfilehash: e5a8db44f80c333d589e0c1664174c394701f90d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835686"
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Paso 4: Mover la configuración y los datos al servidor de destino para la migración a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta sección proporciona información sobre cómo migrar los datos y la configuración desde el servidor de origen. Para mover la configuración y los datos al servidor de destino, haga lo siguiente:  
  
-   [Copiar datos en el servidor de destino](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [Configurar la red](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [Asignar los equipos permitidos a cuentas de usuario](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a> Copiar datos en el servidor de destino  
 Antes de copiar los datos del servidor de origen en el servidor de destino, realice las siguientes tareas:  
  
-   Revise la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Cree o personalice las carpetas en el servidor de destino para que coincidan con la estructura de carpetas que va a migrar desde el servidor de origen.  
  
-   Revise el tamaño de cada carpeta y asegúrese de que el servidor de destino tiene suficiente espacio de almacenamiento.  
  
-   Configure las carpetas compartidas en el servidor de origen como de solo lectura para todos los usuarios, de modo que no se pueda escribir en la unidad de disco duro mientras está copiando los archivos en el servidor de destino.  
  
-   La carpeta **Copia de seguridad de equipos cliente** no se puede migrar al servidor de destino. Antes de la migración de servidor, asegúrese de que todos los equipos cliente se encuentran en un estado correcto. Después de la migración, se recomienda configurar e iniciar copias de seguridad de los equipos cliente para garantizar que se realizará la copia de seguridad de los datos de todos los equipos cliente relevantes.  
  
-   El **copias de seguridad de historial de archivos** carpeta no se puede migrar directamente al servidor de destino debido a los cambios de metadatos de copia de seguridad y la estructura de carpetas en Windows Server Essentials. Sin embargo, sí se puede migrar la carpeta **Copias de seguridad del Historial de archivos** para un usuario específico, en un equipo concreto. Para ello, ubique la carpeta **Datos** en la carpeta **Copias de seguridad del Historial de archivos** correspondiente al usuario y al equipo específicos, y copie la carpeta **Datos** en la carpeta **Copias de seguridad del Historial de archivos** del servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino:  
  
1.  Inicie sesión en el servidor de destino como administrador de dominio y abra una ventana del símbolo del sistema o un símbolo del sistema de Windows PowerShell.  
  
2.  Si utiliza la ventana del símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     Donde:  
  
    -   \<Nombreservidororigen\> es el nombre del servidor de origen  
  
    -   \<Nombredecarpetadeorigencompartida\> es el nombre de la carpeta compartida en el servidor de origen  
  
    -   \<RutaDeAccesoDeDestino\> es la ruta de acceso absoluta donde desea mover la carpeta  
  
    -   \<Nombredecarpetadedestinocompartida\> es la carpeta del servidor de destino al que se copiarán los datos  
  
     Por ejemplo,  `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.  
  
3.  Si usa Windows PowerShell, escriba el siguiente comando y presione ENTRAR.  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  Repita este proceso para cada una de las carpetas compartidas que desee migrar desde el servidor de origen.  
  
##  <a name="BKMK_Network"></a> Configurar la red  
  
#### <a name="to-configure-the-network"></a>Para configurar la red  
  
1.  En el servidor de destino, abra el panel.  
  
2.  En la **página principal** del panel, haga clic en **Instalación** y en **Configurar Acceso desde cualquier lugar**, y elija la opción **Haga clic para configurar el Acceso desde cualquier lugar**.  
  
3.  Aparecerá el Asistente para configurar el Acceso desde cualquier lugar. Siga las instrucciones del asistente para configurar el enrutador y los nombres de dominio.  
  
 Si el enrutador no es compatible con el entorno UPnP, o si este se ha deshabilitado, puede aparecer un icono de advertencia amarillo junto al nombre del enrutador. Asegúrese de que los puertos siguientes están abiertos y dirigidos a la dirección IP del servidor de destino:  
  
-   Puerto 80: Tráfico web HTTP  
  
-   Puerto 443: Tráfico web HTTPS  
  
> [!NOTE]
>  Si desea configurar un nombre de dominio público en el servidor de destino, libere el nombre de dominio desde el servidor de origen para evitar la competición en la actualización dinámica de DNS.  
  
##  <a name="BKMK_MapPermittedComputers"></a> Asignar los equipos permitidos a cuentas de usuario  
 Todas las cuentas de usuario que se migran desde las versiones anteriores de Windows Small Business Server o Windows Server Essentials deben asignarse a uno o varios equipos.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario a equipos:  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haga clic en **Usuarios**.  
  
3.  En la lista de cuentas de usuario, haga clic con el botón secundario en una cuenta de usuario y, con el botón primario, en **Ver las propiedades de la cuenta**.  
  
4.  Haga clic en la pestaña **Acceso desde cualquier lugar** y en **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.  
  
5.  Haga clic en **Carpetas compartidas**, **Equipos**, **Enlaces a la página de inicio** y **Aplicar**.  
  
6.  Haga clic en la pestaña **Acceso al equipo** y en el nombre del equipo al que desea permitir el acceso.  
  
7.  Repita los pasos 3, 4, 5 y 6 para cada una de las cuentas de usuario.  
  
> [!NOTE]
>  No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.  
  
> [!NOTE]
>  Tras completar la migración, si se produce algún problema al crear la primera cuenta de usuario nueva en el servidor de destino, quite la cuenta de usuario que ha agregado y créela de nuevo.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Ya ha movido la configuración y los datos al servidor de destino. Ahora, vaya a [paso 5: Habilitar la redirección de carpetas en la migración del servidor de destino para Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Para ver todos los pasos, consulte [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

