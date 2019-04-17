---
title: "Mover la configuración de Windows SBS 2003 y los datos a la migración de servidor de destino para Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74375d65845a7a5e9c2d6752bdee49993b8cc791
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración de Windows SBS 2003 y los datos a la migración de servidor de destino para Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mover la configuración y datos al servidor de destino como sigue:

1.  [Copiar los datos al servidor de destino](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importar cuentas de usuario de Active Directory en el panel de Essentials de servidor de Windows (opcional)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Quitar los scripts de inicio de sesión anterior (opcionales)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveScripts)  
  
4.  [Quitar heredado grupo Directiva objetos de Active Directory (opcional)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
5.  [Configurar la red](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Configure)  
  
6.  [Asignar equipos permitidos a cuentas de usuario](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a>Copiar los datos al servidor de destino  
 Antes de copiar los datos del servidor de origen al servidor de destino, realice las siguientes tareas:  
  
-   Revisa la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Crear o personalizar las carpetas del servidor de destino para que coincida con la estructura de carpetas que vas a migrar desde el servidor de origen.  
  
-   Revisa el tamaño de cada carpeta y asegúrate de que el servidor de destino tenga suficiente espacio de almacenamiento.  
  
-   Asegúrate de las carpetas compartidas en el servidor de origen Read-only para colocar todos los usuarios por lo que no puede tardar escribir en la unidad mientras se está copiando archivos al servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino  
  
1.  Inicie sesión en el servidor de destino como un administrador de dominio.  
  
2.  Haz clic en **inicio**, tipo **cmd** en el cuadro de búsqueda y, a continuación, presiona ENTRAR.  
  
3.  En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Donde:
     - \ < SourceServerName\ > es el nombre del servidor de origen
     - \ < SharedSourceFolderName\ > es el nombre de la carpeta compartida en el servidor de origen
     - \ < DestinationServerName\ > es el nombre del servidor de destino,
     - \ < SharedDestinationFolderName\ > es la carpeta compartida en el servidor de destino al que se copiarán los datos.  
 
4.  Repite el paso anterior para cada carpeta compartida que vas a migrar desde el servidor de origen.  
  
##  <a name="BKMK_ImportADaccounts"></a>Importar cuentas de usuario de Active Directory en el panel de Essentials de servidor de Windows (opcional)  
 De manera predeterminada, todas las cuentas de usuario creadas en el servidor de origen se migran automáticamente al escritorio en Windows Server Essentials. Sin embargo, la migración automática de una cuenta de usuario de Active Directory se producirá un error si algunas propiedades no cumplen los requisitos de migración. Puedes usar el siguiente cmdlet de Windows PowerShell para importar los usuarios de Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar una cuenta de usuario de Active Directory para el escritorio de Windows Server Essentials  
  
1.  Inicie sesión en el servidor de destino como un administrador de dominio.  
  
2.  Abre Windows PowerShell como administrador.  
  
3.  Ejecutar el cmdlet siguiente, donde `[AD username]`es el nombre de la cuenta de usuario de Active Directory que va a importar:  
  
     `Import-WssUser SamAccountName [AD username]`  
  
##  <a name="BKMK_RemoveScripts"></a>Quitar los scripts de inicio de sesión anterior (opcionales)  
 Windows SBS 2003 usa scripts de inicio de sesión para tareas como la instalación de software y personalizar el escritorio.  Windows Server Essentials reemplaza los scripts de inicio de sesión de Windows SBS 2003 con una combinación de objetos de directiva de grupo y scripts de inicio de sesión.  
  
> [!NOTE]
>  Si se modifican los scripts de inicio de sesión de Windows SBS 2003, debes cambiar el nombre de los scripts para conservar las personalizaciones.  
  
> [!NOTE]
>  Scripts de inicio de sesión de Windows SBS 2003 se aplican solo a las cuentas de usuario que se agregaron mediante la **Asistente para agregar nuevos usuarios**.  
  
#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Para quitar los scripts de inicio de sesión de Windows SBS 2003  
  
1.  Haz clic en **inicio**, elija **herramientas administrativas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**.  
  
2.  En **equipos y usuarios de Active Directory**, expande la red y, a continuación, haz clic en **usuarios**.  
  
3.  En un nombre de usuario, haz clic **propiedades**y, a continuación, haz clic en el **perfil** pestaña.  
  
4.  Eliminar el contenido de la **script de inicio de sesión** cuadro de texto y, a continuación, haz clic en **Aceptar**.  
  
5.  Repite los pasos 3 y 4 para cada usuario.  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a>Quitar heredado grupo Directiva objetos de Active Directory (opcional)  
 Se actualizan los objetos de directiva de grupo (GPO) para Windows Server Essentials. Son un superconjunto de los GPO de Windows SBS 2003. Para Windows Server Essentials, un número de los filtros de Instrumental de administración de Windows (WMI) y de Windows SBS 2003 GPO se debe eliminar manualmente para evitar conflictos con los filtros WMI y de los GPO de Windows Server Essentials.  
  
> [!NOTE]
>  Si se modifican los objetos de directiva de Windows SBS 2003 grupo original, debes guardar copias de ellos en una ubicación diferente y, a continuación, eliminarlos de Windows SBS 2003.  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Para quitar los anteriores objetos de directiva de grupo de Windows SBS 2003  
  
1.  Iniciar sesión en el servidor de origen con una cuenta de administrador.  
  
2.  Haz clic en **inicio**y, a continuación, haz clic en **administración del servidor**.  
  
3.  En el panel de navegación, haz clic en **administración avanzada de**, haz clic en **Group Policy Management**y, a continuación, haz clic en **bosque:***< YourDomainName\ >*.  
  
4.  Haz clic en **dominios**, haz clic en *< YourDomainName\ >*y, a continuación, haz clic en **objetos de directiva de grupo**.  
  
5.  Haz clic en **directiva de auditoría de Small Business Server**, haz clic en **eliminar**y, a continuación, haz clic en **Aceptar**.  
  
6.  Repite el paso 5 para eliminar los siguientes GPO que se aplican a la red:  
  
    -   Equipo de cliente de Small Business Server  
  
    -   Directiva de contraseña de dominio de Small Business Server  
  
         Te recomendamos que configures la directiva de contraseñas en Windows Server Essentials para contraseñas seguras. Para configurar la directiva de contraseñas, usar el panel, que se escribe la configuración en la directiva de dominio predeterminada. La configuración de directiva de contraseña no se escribe en el Small Business Server dominio contraseña objeto de directiva, como era de Windows SBS 2003.  
  
    -   Firewall de conexión a Internet Small Business Server  
  
    -   Directiva de bloqueo de Small Business Server  
  
    -   Directiva de asistencia remota de Small Business Server  
  
    -   Firewall de Windows Small Business Server  
  
    -   Directiva de equipo de cliente de Small Business Server Update Services  
  
         Este GPO se encontrarán si vas a migrar de Windows SBS 2003 R2.  
  
    -   Directiva de configuración comunes de Small Business Server Update Services  
  
         Este GPO se encontrarán si vas a migrar de Windows SBS 2003 R2.  
  
    -   Directiva de equipo de servidor de Small Business Server Update Services  
  
         Este GPO se encontrarán si vas a migrar de Windows SBS 2003 R2.  
  
7.  Confirma que se eliminan todos los GPO.  
  
#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Para quitar los filtros WMI de Windows SBS 2003  
  
1.  Iniciar sesión en el servidor de origen con una cuenta de administrador.  
  
2.  Haz clic en **inicio**y, a continuación, haz clic en **administración del servidor**.  
  
3.  En el panel de navegación, haz clic en **administración avanzada de**, haz clic en **Group Policy Management**y, a continuación, haz clic en **bosque:***< YourNetworkDomainName\ >*  
  
4.  Haz clic en **dominios**, haz clic en *< YourNetworkDomainName\ >*y, a continuación, haz clic en **filtros WMI**.  
  
5.  Haz clic en **PostSP2**, haz clic en **eliminar**y, a continuación, haz clic en **Sí**.  
  
6.  Haz clic en **PreSP2**, haz clic en **eliminar**y, a continuación, haz clic en **Sí**.  
  
7.  Confirma que se eliminan estos tres filtros WMI.  
  
##  <a name="BKMK_Configure"></a>Configurar la red  
  
#### <a name="to-configure-the-network"></a>Para configurar la red  
  
1.  En el servidor de destino, abra el panel.  
  
2.  En el panel **Home** página, haz clic en **instalación**, haz clic en **configurar acceso desde cualquier lugar**y, a continuación, elige el **haga clic para configurar acceso desde cualquier lugar** opción.  
  
3.  Sigue las instrucciones de la **configurar acceso desde cualquier lugar** Asistente para configurar el enrutador y nombre de dominio.  
  
 Si el enrutador no admite el marco UPnP o, si se deshabilita el marco UPnP, puede que aparezca un icono de advertencia amarillo junto al nombre del enrutador. Asegúrate de que los siguientes puertos estén abiertos y que haber sido dirigidos a la dirección IP del servidor de destino:  
  
-   Puerto 80: El tráfico HTTP Web  
  
-   Puerto 443: El tráfico de Web HTTPS  
  
> [!NOTE]
>  Si has configurado un servidor de Exchange local en un segundo servidor, debes asegurarte de puerto 25 (SMTP) también está abierto y que se redirige a la dirección IP del servidor de Exchange local.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Asignar equipos permitidos a cuentas de usuario  
 En Windows SBS 2003, si un usuario se conecta al acceso Web remoto, se muestran todos los equipos de la red. Esto puede incluir que el usuario no tiene permiso para acceder a los equipos. En Windows Server Essentials, un usuario debe asignarse explícitamente a un equipo para que se muestren en acceso Web remoto. Cada cuenta de usuario que se migra desde Windows SBS 2003 debe asignarse a uno o más equipos.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario para equipos  
  
1.  En el servidor de destino, abre el escritorio de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, haz clic en una cuenta de usuario y, a continuación, haz clic en **ver las propiedades de la cuenta**.  
  
4.  Haz clic en el **acceso desde cualquier lugar** pestaña y, a continuación, haz clic en **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web.**.  
  
5.  Haz clic en **carpetas compartidas**, haz clic en **equipos**, haz clic en **vínculos de la página principal**y, a continuación, haz clic en **aplicar**.  
  
6.  Haz clic en el **acceso al equipo** pestaña y haz clic en el nombre del equipo al que quieres permitir el acceso.  
  
7.  Repite los pasos 3, 4, 5 y 6 para cada cuenta de usuario.  
  
> [!NOTE]
>  No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.  
  
> [!NOTE]
>  Después de completar la migración, si se produce un problema al crear la primera cuenta de usuario de nuevo en el servidor de destino, quitar la cuenta de usuario que has agregado y, a continuación, volver a crearla.
