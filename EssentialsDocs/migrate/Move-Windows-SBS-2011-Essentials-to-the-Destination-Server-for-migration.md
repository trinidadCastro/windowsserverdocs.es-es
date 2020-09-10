---
title: Mover la configuración y los datos de Windows SBS 2011 Essentials al servidor de destino para la migración a Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 47548994-9fa0-42e0-afa4-c2ccbd063acb
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5ab8e54fe94fa2f733e28dd461b7d35988589864
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625697"
---
# <a name="move-windows-sbs-2011-essentials-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración y los datos de Windows SBS 2011 Essentials al servidor de destino para la migración a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para mover la configuración y los datos al servidor de destino, haga lo siguiente:


1.  [Copiar los datos en el servidor de destino](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_CopyData)

2.  [Importar Active Directory cuentas de usuario en el panel de Windows Server Essentials (opcional)](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_ImportADaccounts)

3.  [Configurar la red](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_Network)

4.  [Asignar los equipos permitidos a cuentas de usuario](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_MapPermittedComputers)

##  <a name="copy-data-to-the-destination-server"></a><a name="BKMK_CopyData"></a> Copiar datos en el servidor de destino
 Antes de copiar los datos del servidor de origen en el servidor de destino, realice las siguientes tareas:

-   Revise la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Cree o personalice las carpetas en el servidor de destino para que coincidan con la estructura de carpetas que va a migrar desde el servidor de origen.

-   Revise el tamaño de cada carpeta y asegúrese de que el servidor de destino tiene suficiente espacio de almacenamiento.

-   Asigne permisos de solo lectura a las carpetas compartidas en el servidor de origen para todos los usuarios. De esta forma, no se escribirá en la unidad mientras se estén copiando archivos al servidor de destino.

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino:

1.  Inicie sesión en el servidor de destino como administrador de dominio y, después, abra una ventana de comandos.

2.  En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

     Donde:
     - \<SourceServerName\> es el nombre del servidor de origen.
     - \<SharedSourceFolderName\> es el nombre de la carpeta compartida en el servidor de origen.
     - \<DestinationServerName\> es el nombre del servidor de destino.
     - \<SharedDestinationFolderName\> es la carpeta compartida en el servidor de destino en la que se copiarán los datos.

3.  Repita el paso anterior para cada carpeta compartida que vaya a migrar desde el servidor de origen.

##  <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard-optional"></a><a name="BKMK_ImportADaccounts"></a> Importar Active Directory cuentas de usuario en el panel de Windows Server Essentials (opcional)
 De forma predeterminada, todas las cuentas de usuario creadas en el servidor de origen se migran automáticamente al panel en Windows Server Essentials. Sin embargo, si las propiedades no cumplen los requisitos de migración, se producirá un error en la migración automática de una cuenta de usuario de Active Directory. Puede usar el siguiente cmdlet de Windows PowerShell para importar usuarios de Active Directory.

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar una cuenta de usuario Active Directory en el panel de Windows Server Essentials

1.  Inicie sesión en el servidor de destino como administrador del dominio.

2.  Abra Windows PowerShell como administrador.

3.  Ejecute el siguiente cmdlet, donde `[AD username]` es el nombre de la cuenta de usuario de Active Directory que quiere importar:

     `Import-WssUser  SamAccountName [AD username]`

##  <a name="configure-the-network"></a><a name="BKMK_Network"></a> Configurar la red

#### <a name="to-configure-the-network"></a>Para configurar la red

1. En el servidor de destino, abra el panel.

2. En la página **Inicio** del panel, haga clic en **CONFIGURACIÓN**, haga clic en **Configurar Acceso desde cualquier lugar** y, después, elija la opción **Haga clic para configurar el Acceso desde cualquier lugar**.

3. Siga las instrucciones del asistente para configurar el enrutador y los nombres de dominio.

   Si el enrutador no es compatible con el entorno UPnP, o si este se ha deshabilitado, puede aparecer un icono de advertencia amarillo junto al nombre del enrutador. Asegúrese de que los puertos siguientes están abiertos y dirigidos a la dirección IP del servidor de destino:

-   Puerto 80: tráfico web HTTP

-   Puerto 443: tráfico web HTTPS

##  <a name="map-permitted-computers-to-user-accounts"></a><a name="BKMK_MapPermittedComputers"></a> Asignar equipos permitidos a cuentas de usuario
 Todas las cuentas de usuario que se migren desde Windows Small Business Server 2011 Essentials deben asignarse a uno o más equipos.

#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario a equipos:

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Usuarios**.

3.  En la lista de cuentas de usuario, haga clic con el botón secundario en una cuenta de usuario y, con el botón primario, en **Ver las propiedades de la cuenta**.

4.  Haga clic en la pestaña **Acceso desde cualquier lugar** y en **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.

5.  Seleccione **Carpetas compartidas**, **Equipos**, **Enlaces a la página de inicio** y, a continuación, haga clic en **Aplicar**.

6.  Haga clic en la pestaña **Acceso al equipo** y en el nombre del equipo al que desea permitir el acceso.

7.  Repita los pasos 3, 4, 5 y 6 para cada una de las cuentas de usuario.

> [!NOTE]
>  No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.

> [!NOTE]
>  Tras completar la migración, si se produce algún problema al crear la primera cuenta de usuario nueva en el servidor de destino, quite la cuenta de usuario que ha agregado y créela de nuevo.
