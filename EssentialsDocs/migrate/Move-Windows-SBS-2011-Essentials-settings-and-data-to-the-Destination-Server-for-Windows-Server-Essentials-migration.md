---
title: Mover la configuración y los datos de Windows SBS 2011 Essentials al servidor de destino para la migración a Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47548994-9fa0-42e0-afa4-c2ccbd063acb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 80ef8a196f70a77e149dc8defaacf20a82d576e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859326"
---
# <a name="move-windows-sbs-2011-essentials-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración y los datos de Windows SBS 2011 Essentials al servidor de destino para la migración a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para mover la configuración y los datos al servidor de destino, haga lo siguiente:  
  

1.  [Copiar datos en el servidor de destino](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importar cuentas de usuario de Active Directory al panel de Windows Server Essentials (opcional)](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Configurar la red](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
4.  [Asignar los equipos permitidos a cuentas de usuario](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a> Copiar datos en el servidor de destino  
 Antes de copiar los datos del servidor de origen en el servidor de destino, realice las siguientes tareas:  
  
-   Revise la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Cree o personalice las carpetas en el servidor de destino para que coincidan con la estructura de carpetas que va a migrar desde el servidor de origen.  
  
-   Revise el tamaño de cada carpeta y asegúrese de que el servidor de destino tiene suficiente espacio de almacenamiento.  
  
-   Asigne permisos de solo lectura a las carpetas compartidas en el servidor de origen para todos los usuarios. De esta forma, no se escribirá en la unidad mientras se estén copiando archivos al servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino:  
  
1.  Inicie sesión en el servidor de destino como administrador de dominio y, después, abra una ventana de comandos.  
  
2.  En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Donde:
     - \<Nombreservidororigen\> es el nombre del servidor de origen
     - \<Nombredecarpetadeorigencompartida\> es el nombre de la carpeta compartida en el servidor de origen
     - \<NombreDeServidorDeDestino\> es el nombre del servidor de destino,
     - \<Nombredecarpetadedestinocompartida\> es la carpeta compartida del servidor de destino al que se copiarán los datos.  
        
3.  Repita el paso anterior para cada carpeta compartida que vaya a migrar desde el servidor de origen.  
  
##  <a name="BKMK_ImportADaccounts"></a> Importar cuentas de usuario de Active Directory al panel de Windows Server Essentials (opcional)  
 De forma predeterminada, todas las cuentas de usuario creadas en el servidor de origen se migran automáticamente al panel en Windows Server Essentials. Sin embargo, si las propiedades no cumplen los requisitos de migración, se producirá un error en la migración automática de una cuenta de usuario de Active Directory. Puede usar el siguiente cmdlet de Windows PowerShell para importar usuarios de Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar una cuenta de usuario de Active Directory en el panel de Windows Server Essentials  
  
1.  Inicie sesión en el servidor de destino como administrador del dominio.  
  
2.  Abra Windows PowerShell como administrador.  
  
3.  Ejecute el siguiente cmdlet, donde `[AD username]` es el nombre de la cuenta de usuario de Active Directory que quiere importar:  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="BKMK_Network"></a> Configurar la red  
  
#### <a name="to-configure-the-network"></a>Para configurar la red  
  
1.  En el servidor de destino, abra el panel.  
  
2.  En la página **Inicio** del panel, haga clic en **CONFIGURACIÓN**, haga clic en **Configurar Acceso desde cualquier lugar** y, después, elija la opción **Haga clic para configurar el Acceso desde cualquier lugar**.  
  
3.  Siga las instrucciones del asistente para configurar el enrutador y los nombres de dominio.  
  
 Si el enrutador no es compatible con el entorno UPnP, o si este se ha deshabilitado, puede aparecer un icono de advertencia amarillo junto al nombre del enrutador. Asegúrese de que los puertos siguientes están abiertos y dirigidos a la dirección IP del servidor de destino:  
  
-   Puerto 80: Tráfico web HTTP  
  
-   Puerto 443: Tráfico web HTTPS  
  
##  <a name="BKMK_MapPermittedComputers"></a> Asignar los equipos permitidos a cuentas de usuario  
 Todas las cuentas de usuario que se migren desde Windows Small Business Server 2011 Essentials deben asignarse a uno o más equipos.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario a equipos:  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haga clic en **Usuarios**.  
  
3.  En la lista de cuentas de usuario, haga clic con el botón secundario en una cuenta de usuario y, con el botón primario, en **Ver las propiedades de la cuenta**.  
  
4.  Haga clic en la pestaña **Acceso desde cualquier lugar** y en **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.  
  
5.  Seleccione **Carpetas compartidas**, **Equipos**, **Enlaces a la página de inicio**y, a continuación, haga clic en **Aplicar**.  
  
6.  Haga clic en la pestaña **Acceso al equipo** y en el nombre del equipo al que desea permitir el acceso.  
  
7.  Repita los pasos 3, 4, 5 y 6 para cada una de las cuentas de usuario.  
  
> [!NOTE]
>  No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.  
  
> [!NOTE]
>  Tras completar la migración, si se produce algún problema al crear la primera cuenta de usuario nueva en el servidor de destino, quite la cuenta de usuario que ha agregado y créela de nuevo.
