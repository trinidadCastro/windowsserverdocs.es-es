---
title: Mover la configuración y los datos de Windows SBS 2003 al servidor de destino para la migración a Windows Server Essentials
description: Obtenga información acerca de cómo migrar los datos y la configuración de Windows SBS 2003 al servidor de destino para la migración a Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: be7447c74c90e85522b166656fbf2c82b9d52668
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810738"
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración y los datos de Windows SBS 2003 al servidor de destino para la migración a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para mover la configuración y los datos al servidor de destino, haga lo siguiente:

1. [Copiar los datos en el servidor de destino](#copy-data-to-the-destination-server)

2. [Importar Active Directory cuentas de usuario en el panel de Windows Server Essentials (opcional)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [Quitar los scripts de inicio de sesión antiguos (opcional)](#remove-old-logon-scripts)

4. [Quitar los objetos de directiva de grupo de Active Directory heredados (opcional)](#remove-legacy-active-directory-group-policy-objects)

5. [Configurar la red](#configure-the-network)

6. [Asignar los equipos permitidos a cuentas de usuario](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>Copiar los datos en el servidor de destino
Antes de copiar los datos del servidor de origen en el servidor de destino, realice las siguientes tareas:

- Revise la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Cree o personalice las carpetas en el servidor de destino para que coincidan con la estructura de carpetas que va a migrar desde el servidor de origen.

- Revise el tamaño de cada carpeta y asegúrese de que el servidor de destino tiene suficiente espacio de almacenamiento.

- Asigne permisos de solo lectura a las carpetas compartidas en el servidor de origen para todos los usuarios. De esta forma, no se escribirá en la unidad mientras se estén copiando archivos al servidor de destino.

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino:

1. Inicie sesión en el servidor de destino como administrador del dominio.

2. Click **Start**, type **cmd** in the search box, and then press ENTER.

3. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

Donde:
 - \<SourceServerName\> es el nombre del servidor de origen.
 - \<SharedSourceFolderName\> es el nombre de la carpeta compartida en el servidor de origen.
 - \<DestinationServerName\> es el nombre del servidor de destino.
 - \<SharedDestinationFolderName\> es la carpeta compartida en el servidor de destino en la que se copiarán los datos.

4. Repita el paso anterior para cada carpeta compartida que vaya a migrar desde el servidor de origen.

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>Importar Active Directory cuentas de usuario en el panel de Windows Server Essentials
 De forma predeterminada, todas las cuentas de usuario creadas en el servidor de origen se migran automáticamente al panel en Windows Server Essentials. Sin embargo, si algunas de las propiedades no cumplen los requisitos de migración, se producirá un error en la migración automática de una cuenta de usuario de Active Directory. Puede usar el siguiente cmdlet de Windows PowerShell para importar usuarios de Active Directory.

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar una cuenta de usuario Active Directory en el panel de Windows Server Essentials

1. Inicie sesión en el servidor de destino como administrador del dominio.

2. Abra Windows PowerShell como administrador.

3. Ejecute el siguiente cmdlet, donde `[AD username]` es el nombre de la cuenta de usuario de Active Directory que quiere importar:

    `Import-WssUser SamAccountName [AD username]`

## <a name="remove-old-logon-scripts"></a>Quitar scripts de inicio de sesión antiguos
Windows SBS 2003 usa scripts de inicio de sesión para algunas tareas, como por ejemplo la instalación de software y la personalización de escritorios. Windows Server Essentials reemplaza los scripts de inicio de sesión de Windows SBS 2003 por una combinación de scripts de inicio de sesión y objetos de directiva de grupo.

> [!NOTE]
> Si ha modificado los scripts de inicio de sesión de Windows SBS 2003 debe cambiar el nombre de los scripts para conservar las personalizaciones.
>
> Los scripts de inicio de sesión de Windows SBS 2003 solo se aplican a las cuentas de usuario que se han agregado mediante el **Asistente para agregar nuevos usuarios**.

#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Para quitar los scripts de inicio de sesión de Windows SBS 2003

1. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**.

2. En **Usuarios y equipos de Active Directory**, expanda la red y haga clic en **Usuarios**.

3. Haga clic con el botón derecho en el nombre de un usuario, haga clic en **Propiedades** y, a continuación, haga clic en la pestaña **Perfil**.

4. Elimine el contenido del cuadro de texto **Script de inicio de sesión** y haga clic en **Aceptar**.

5. Repita los pasos 3 y 4 para cada usuario.

## <a name="remove-legacy-active-directory-group-policy-objects"></a>Quitar objetos de directiva de grupo de Active Directory heredados
Los objetos de directiva de grupo (GPO) se actualizan para Windows Server Essentials. Son un superconjunto de los GPO de Windows SBS 2003. En Windows Server Essentials, se deben eliminar de forma manual una serie de los GPO de Windows SBS 2003 y los filtros de Instrumental de administración de Windows (WMI) para evitar conflictos con los GPO y los filtros WMI de Windows Server Essentials.

> [!NOTE]
> Si ha modificado los objetos de directiva de grupo originales de Windows SBS 2003 debe guardar copias en una ubicación diferente y eliminarlos de Windows SBS 2003.

#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Para quitar objetos antiguos de directiva de grupo de Windows SBS 2003

1. Inicie sesión en el servidor de origen con una cuenta de administrador.

2. Haga clic en **Inicio** y, después, en **Administración de servidores**.

3. En el panel de navegación, haga clic en **Administración avanzada**, en **Administración de directiva de grupo** y, a continuación, en **bosque:** _<sudominio \>_.

4. Haga clic en **dominios**, haga clic en *<\> sudominio* y, a continuación, haga clic en **Directiva de grupo objetos**.

5. Haga clic con el botón secundario en **Directiva de auditoría de Small Business Server**, en **Eliminar** y, a continuación, en **Aceptar**.

6. Repita el paso 5 para eliminar los siguientes GPO que se aplican a la red:

 - Equipo cliente de Small Business Server

 - Directiva de contraseña de dominio de Small Business Server

Se recomienda configurar la Directiva de contraseñas en Windows Server Essentials para aplicar contraseñas seguras. Para configurar la directiva de contraseñas use el panel, que escribe la configuración en la directiva de dominio predeterminada. La configuración de la directiva de contraseña no se escribe en el objeto de directiva de contraseña de dominio de Small Business Server, como ocurría en Windows SBS 2003.

 - Firewall de conexión a Internet de Small Business Server

 - Directiva de bloqueo de Small Business Server

 - Directiva de asistencia remota de Small Business Server

 - Firewall de Windows de Small Business Server

 - Directiva de equipo cliente de servicios de actualización de Small Business Server

 Se presentará este GPO si efectúa la migración desde Windows SBS 2003 R2.

 - Directiva de configuración común de servicios de actualización de Small Business Server

 Se presentará este GPO si efectúa la migración desde Windows SBS 2003 R2.

 - Directiva de equipo de servidor de servicios de actualización de Small Business Server

 Se presentará este GPO si efectúa la migración desde Windows SBS 2003 R2.

7. Compruebe que se hayan eliminado todos los GPO.

#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Para quitar los filtros WMI de Windows SBS 2003

1. Inicie sesión en el servidor de origen con una cuenta de administrador.

2. Haga clic en **Inicio** y, después, en **Administración de servidores**.

3. En el panel de navegación, haga clic en **Administración avanzada**, en **Administración de directiva de grupo** y, a continuación, en **bosque:** _<nombredominiored \>_

4. Haga clic en **dominios**, haga clic en *<\> nombredominiored* y, a continuación, en **filtros WMI**.

5. Haga clic con el botón secundario en **PostSP2**, haga clic en **Eliminar** y, a continuación, haga clic en **Sí**.

6. Haga clic con el botón secundario en **PreSP2**, haga clic en **Eliminar** y, a continuación, haga clic en **Sí**.

7. Compruebe que se hayan eliminado los tres filtros WMI.

## <a name="configure-the-network"></a>Configuración de la red

#### <a name="to-configure-the-network"></a>Para configurar la red

1. En el servidor de destino, abra el panel.

2. En la página **Inicio** del panel, haga clic en **CONFIGURACIÓN**, haga clic en **Configurar Acceso desde cualquier lugar** y, después, elija la opción **Haga clic para configurar el Acceso desde cualquier lugar**.

3. Siga las instrucciones del asistente **Configurar Acceso desde cualquier lugar** para configurar el nombre del enrutador y del dominio.

 Si el enrutador no es compatible con el entorno UPnP, o si este se ha deshabilitado, puede aparecer un icono de advertencia amarillo junto al nombre del enrutador. Asegúrese de que los puertos siguientes están abiertos y dirigidos a la dirección IP del servidor de destino:

- Puerto 80: tráfico web HTTP

- Puerto 443: tráfico web HTTPS

> [!NOTE]
> Si ha configurado un servidor local de Exchange en un segundo servidor debe asegurarse de que el puerto 25 (para SMTP) también está abierto y de que se redirige a la dirección IP del servidor local de Exchange.

## <a name="map-permitted-computers-to-user-accounts"></a>Asignar los equipos permitidos a cuentas de usuario
 En Windows SBS 2003, si un usuario se conecta a Acceso Web remoto, se muestran todos los equipos de la red. Puede incluir los equipos a los que el usuario no puede acceder. En Windows Server Essentials, un usuario debe estar asignado explícitamente a un equipo para que se muestre en acceso Web remoto. Todas las cuentas de usuario que se migren desde Windows SBS 2003 deben asignarse a uno o más equipos.

#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario a equipos:

1. En el servidor de destino, abra el panel de Windows Server Essentials.

2. En la barra de navegación, haga clic en **Usuarios**.

3. En la lista de cuentas de usuario, haga clic con el botón secundario en una cuenta de usuario y, con el botón primario, en **Ver las propiedades de la cuenta**.

4. Haga clic en la pestaña **Acceso desde cualquier lugar** y, a continuación, haga clic en **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.

5. Haga clic en **Carpetas compartidas**, en **Equipos**, en **Enlaces a la página de inicio** y en **Aplicar**.

6. Haga clic en la pestaña **Acceso al equipo** y haga clic en el nombre del equipo al que quiere permitir el acceso.

7. Repita los pasos 3, 4, 5 y 6 para cada una de las cuentas de usuario.

> [!NOTE]
> No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.
>
> Tras completar la migración, si se produce algún problema al crear la primera cuenta de usuario nueva en el servidor de destino, quite la cuenta de usuario que ha agregado y créela de nuevo.
