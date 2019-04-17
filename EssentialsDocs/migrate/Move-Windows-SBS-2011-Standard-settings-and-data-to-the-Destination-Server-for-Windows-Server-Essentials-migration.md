---
title: "Mover la configuración de Windows SBS 2011 estándar y los datos a la migración de servidor de destino para Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b24026-2fe3-4bd0-b82f-900e1564be99
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b0ee150be2452fdf4c31c6a2a372958640fa5e4a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2011-standard-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración de Windows SBS 2011 estándar y los datos a la migración de servidor de destino para Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mover la configuración y datos al servidor de destino como sigue:  
  

1.  [Copiar los datos al servidor de destino](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importar cuentas de usuario de Active Directory en el panel de Essentials de servidor de Windows (opcional)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Mover el rol de servidor DHCP desde el servidor de origen al enrutador](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MoveDHCP)  
  
4.  [Configurar la red](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
5.  [Quitar heredado grupo Directiva objetos de Active Directory (opcional)](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
6.  [Asignar equipos permitidos a cuentas de usuario](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
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
  
##  <a name="BKMK_ImportADaccounts"></a>Importar cuentas de usuario de Active Directory en el panel de Essentials de servidor de Windows (opcional)  
 De manera predeterminada, todas las cuentas de usuario creadas en el servidor de origen se migran automáticamente al escritorio en Windows Server Essentials. Sin embargo, la migración automática de una cuenta de usuario de Active Directory se producirá un error si algunas propiedades no cumplen los requisitos de migración. Puedes usar el siguiente cmdlet de Windows PowerShell para importar los usuarios de Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar una cuenta de usuario de Active Directory para el escritorio de Windows Server Essentials  
  
1.  Inicie sesión en el servidor de destino como un administrador de dominio.  
  
2.  Abre Windows PowerShell como administrador.  
  
3.  Ejecutar el cmdlet siguiente, donde `[AD username]`es el nombre de la cuenta de usuario de Active Directory que va a importar:  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="BKMK_MoveDHCP"></a>Mover el rol de servidor DHCP desde el servidor de origen al enrutador  
 Si tu servidor de origen está ejecutando la función de DHCP, realiza los siguientes pasos para mover el rol DHCP al enrutador.  
  
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Para mover el rol DHCP desde el servidor de origen al enrutador  
  
1.  Desactivar el servicio DHCP en el servidor de origen, como sigue:  
  
    1.  En el servidor de origen, haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **servicios**.  
  
    2.  En la lista de servicios en ejecución actualmente, haz clic en **servidor DHCP**y, a continuación, haz clic en **propiedades**.  
  
    3.  Para **tipo de inicio**, selecciona **deshabilitado**.  
  
    4.  Detener el servicio.  
  
2.  Activa la función de DHCP en el enrutador.  
  
    1.  Sigue las instrucciones de la documentación del enrutador para activar la función de DHCP en el enrutador.  
  
    2.  Para garantizar que las direcciones IP emitidas por el servidor de origen sea el mismo, sigue las instrucciones de la documentación del enrutador para configurar el intervalo de DHCP en el enrutador para ser el mismo que el rango DHCP en el servidor de origen.  
  
        > [!IMPORTANT]
        >  Si no has configurado un reservas de IP o DHCP estática en el enrutador para el servidor de destino y el intervalo DHCP no es el mismo que el servidor de origen, es posible que el enrutador emitirá una nueva dirección IP de servidor de destino. Si esto ocurre, restablecer las reglas del enrutador para reenviar a la nueva dirección IP del servidor de destino de enrutamiento de puertos.  
  
##  <a name="BKMK_Network"></a>Configurar la red  
 Después de mover el rol DHCP al enrutador, configurar la configuración de red en el servidor de destino.  
  
#### <a name="to-configure-the-network"></a>Para configurar la red  
  
1.  En el servidor de destino, abra el panel.  
  
2.  En la **Home** página, haz clic en **instalación**, haz clic en **configurar acceso desde cualquier lugar**y, a continuación, elige el **haga clic para configurar acceso desde cualquier lugar** opción.  
  
3.  Siga las instrucciones del Asistente para configurar el enrutador y nombres de dominio.  
  
 Si el enrutador no admite el marco UPnP o, si se deshabilita el marco UPnP, puede que aparezca un icono de advertencia amarillo junto al nombre del enrutador. Asegúrate de que los siguientes puertos estén abiertos y que haber sido dirigidos a la dirección IP del servidor de destino:  
  
-   Puerto 80: El tráfico HTTP Web  
  
-   Puerto 443: El tráfico de Web HTTPS  
  
> [!NOTE]
>  Si has configurado un servidor de Exchange local en un segundo servidor, debes asegurarte de puerto 25 (SMTP) también está abierto y que se redirige a la dirección IP del servidor de Exchange local.  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a>Quitar heredado grupo Directiva objetos de Active Directory (opcional)  
 Se actualizan los objetos de directiva de grupo (GPO) para Windows Server Essentials. Son un superconjunto de los GPO de Windows Small Business Server 2011. Para Windows Server Essentials, un número de los filtros de Instrumental de administración de Windows (WMI) y de Windows Small Business Server 2011 GPO se debe eliminar manualmente para evitar conflictos con los filtros WMI y Windows Server Essentials GPO.  
  
> [!NOTE]
>  Si se modifican los original Windows Small Business Server 2011 grupo objetos de directiva, debes guardar copias de ellos en una ubicación diferente y, a continuación, eliminarlos desde Windows Small Business Server 2011.  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-small-business-server-2011"></a>Para quitar los anteriores objetos de directiva de grupo de Windows Small Business Server 2011  
  
1.  Iniciar sesión en el servidor de origen con una cuenta de administrador.  
  
2.  Haz clic en **inicio**y, a continuación, haz clic en **administración del servidor**.  
  
3.  En el panel de navegación, haz clic en **administración avanzada de**, haz clic en **Group Policy Management**y, a continuación, haz clic en **bosque:***< YourDomainName\ >*.  
  
4.  Haz clic en **dominios**, haz clic en *< YourDomainName\ >*y, a continuación, haz clic en **objetos de directiva de grupo**.  
  
5.  Haz clic en **directiva de auditoría de Small Business Server**, haz clic en **eliminar**y, a continuación, haz clic en **Aceptar**.  
  
6.  Repite el paso 5 para eliminar los siguientes GPO que se aplican a la red:  
  
    -   Directiva de Windows Vista y de Windows SBS cliente Windows 7  
  
    -   Directiva de Windows SBS cliente Windows XP  
  
    -   Directiva de Windows SBS CSE  
  
    -   Directiva de usuario de Windows SBS  
  
    -   Actualizar la directiva de equipo del cliente de servicios  
  
    -   Actualizar la directiva de configuración comunes de servicios  
  
    -   Actualizar la directiva de equipo de servidor de servicios  
  
7.  Confirma que se eliminan todos los GPO.  
  
#### <a name="to-remove-wmi-filters-from-the-source-server"></a>Para quitar los filtros WMI del servidor de origen  
  
1.  Iniciar sesión en el servidor de origen con una cuenta de administrador.  
  
2.  Haz clic en **inicio**y, a continuación, haz clic en **administración del servidor**.  
  
3.  En el panel de navegación, haz clic en **características**, haz clic en **Group Policy Management**y, a continuación, haz clic en **bosque:***< YourNetworkDomainName\ >*  
  
4.  Haz clic en **dominios**, haz clic en *< YourNetworkDomainName\ >*y, a continuación, haz clic en **filtros WMI**.  
  
5.  Haz clic en **cliente de Windows SBS**, haz clic en **eliminar**y, a continuación, haz clic en **Sí**.  
  
6.  Haz clic en **cliente de Windows SBS Windows 7 y Windows Vista**, haz clic en **eliminar**y, a continuación, haz clic en **Sí**.  
  
7.  Haz clic en **cliente de Windows SBS Windows XP**, haz clic en **eliminar**y, a continuación, haz clic en **Sí**.  
  
8.  Confirma que se eliminan estos tres filtros WMI.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Asignar equipos permitidos a cuentas de usuario  
 En Windows Small Business Server 2011, si un usuario se conecta al acceso Web remoto, se muestran todos los equipos de la red. Esto puede incluir que el usuario no tiene permiso para acceder a los equipos. En Windows Server Essentials, un usuario debe asignarse explícitamente a un equipo para que se muestren en acceso Web remoto. Cada cuenta de usuario que se migra desde Windows Small Business Server 2011 debe asignarse a uno o más equipos.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario para equipos  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, haz clic en una cuenta de usuario y, a continuación, haz clic en **ver las propiedades de la cuenta**.  
  
4.  Haz clic en el **acceso desde cualquier lugar** pestaña y, a continuación, haz clic en **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web**.  
  
5.  Selecciona **carpetas compartidas**, selecciona **equipos**, selecciona **vínculos de la página principal**y, a continuación, haz clic en **aplicar**.  
  
6.  Haz clic en el **acceso al equipo** pestaña y, a continuación, haz clic en el nombre del equipo al que quieres permitir el acceso.  
  
7.  Repite los pasos 3, 4, 5 y 6 para cada cuenta de usuario.  
  
> [!NOTE]
>  No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.  
  
> [!NOTE]
>  Después de completar la migración, si se produce un problema al crear la primera cuenta de usuario de nuevo en el servidor de destino, quitar la cuenta de usuario que has agregado y, a continuación, volver a crearla.
