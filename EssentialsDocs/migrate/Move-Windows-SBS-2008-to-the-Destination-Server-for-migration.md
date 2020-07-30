---
title: Mover la configuración y los datos de Windows SBS 2008 al servidor de destino para la migración a Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 4950469d-d800-430d-8d10-53bafc4a9932
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5f9d240bb7467c0248b69bc65d7f26a66ecec038
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180581"
---
# <a name="move-windows-sbs-2008-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración y los datos de Windows SBS 2008 al servidor de destino para la migración a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para mover la configuración y los datos al servidor de destino, haga lo siguiente:

1. [Copiar los datos en el servidor de destino](#copy-data-to-the-destination-server)

2. [Importar Active Directory cuentas de usuario en el panel de Windows Server Essentials (opcional)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [Mover el rol de servidor DHCP del servidor de origen al enrutador](#move-the-dhcp-server-role-from-the-source-server-to-the-router)

4. [Configurar la red](#configure-the-network)

5. [Quitar objetos de directiva de grupo de Active Directory heredados (opcional)](#remove-legacy-active-directory-group-policy-objects)

6. [Asignar los equipos permitidos a cuentas de usuario](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>Copiar los datos en el servidor de destino
Antes de copiar los datos del servidor de origen en el servidor de destino, realice las siguientes tareas:

- Revise la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Cree o personalice las carpetas en el servidor de destino para que coincidan con la estructura de carpetas que va a migrar desde el servidor de origen.

- Revise el tamaño de cada carpeta y asegúrese de que el servidor de destino tiene suficiente espacio de almacenamiento.

- Asigne permisos de solo lectura a las carpetas compartidas en el servidor de origen para todos los usuarios. De esta forma, no se escribirá en la unidad mientras se estén copiando archivos al servidor de destino.

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino:

1. Inicie sesión en el servidor de destino como un administrador de dominio y, a continuación, abra una ventana de comandos.

2. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

 Donde:
 - \<SourceServerName\> es el nombre del servidor de origen.
 - \<SharedSourceFolderName\> es el nombre de la carpeta compartida en el servidor de origen.
 - \<DestinationServerName\>es el nombre del servidor de destino.
 - \<SharedDestinationFolderName\>es la carpeta compartida en el servidor de destino en la que se copiarán los datos.

3. Repita el paso anterior para cada carpeta compartida que vaya a migrar desde el servidor de origen.

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>Importar Active Directory cuentas de usuario en el panel de Windows Server Essentials
 De forma predeterminada, todas las cuentas de usuario creadas en el servidor de origen se migran automáticamente al panel en Windows Server Essentials. Sin embargo, si algunas de las propiedades no cumplen los requisitos de migración, se producirá un error en la migración automática de una cuenta de usuario de Active Directory. Puede usar el siguiente cmdlet de Windows PowerShell para importar usuarios de Active Directory.

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar una cuenta de usuario Active Directory en el panel de Windows Server Essentials

1. Inicie sesión en el servidor de destino como administrador del dominio.

2. Abra Windows PowerShell como administrador.

3. Ejecute el siguiente cmdlet, donde `[AD username]` es el nombre de la cuenta de usuario de Active Directory que quiere importar:

 `Import-WssUser SamAccountName [AD username]`

## <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a>Mover el rol de servidor DHCP del servidor de origen al enrutador
 Si el servidor de origen ejecuta la función DHCP, efectúe los siguientes pasos para mover la función DHCP al enrutador.

#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Para mover la función DHCP del servidor de origen al enrutador

1. Desactive el servicio DHCP en el servidor de origen del siguiente modo:

    1. En el servidor de origen, haga clic en **Inicio**, haga clic en **Herramientas administrativas** y, después, haga clic en **Servicios**.

    2. En la lista de servicios que se están ejecutando, haga clic con el botón derecho en **Servidor DHCP** y, después, haga clic en **Propiedades**.

    3. En **Tipo de inicio**, seleccione **Deshabilitado**.

    4. Detenga el servicio.

2. Active la función DHCP en el enrutador.

    1. Siga las instrucciones de la documentación del enrutador para activar la función DHCP en el enrutador.

    2. Para asegurarse de que las direcciones IP emitidas por el servidor de origen sean las mismas, siga las instrucciones de la documentación del enrutador para configurar que el intervalo de DHCP del enrutador sea el mismo que el intervalo de DHCP del servidor de origen.

 > [!IMPORTANT]
 > Si no ha configurado reservas de DHCP o una dirección IP estática en el enrutador para el servidor de destino y el intervalo de DHCP no es el mismo que el servidor de origen, es posible que el enrutador emita una nueva dirección IP para el servidor de destino. Si ocurre, restablezca las reglas de reenvío del puerto del enrutador para hacer un reenvío a la nueva dirección IP del servidor de destino.

## <a name="configure-the-network"></a>Configurar la red
 Después de mover el rol de DHCP al enrutador, configure la red en el servidor de destino.

#### <a name="to-configure-the-network"></a>Para configurar la red

1. En el servidor de destino, abra el panel.

2. En la página **Inicio** del panel, haga clic en **CONFIGURACIÓN**, haga clic en **Configurar Acceso desde cualquier lugar** y, después, elija la opción **Haga clic para configurar el Acceso desde cualquier lugar**.

3. Siga las instrucciones del asistente para configurar el enrutador y los nombres de dominio.

 Si el enrutador no es compatible con el entorno UPnP, o si este se ha deshabilitado, puede aparecer un icono de advertencia amarillo junto al nombre del enrutador. Asegúrese de que los puertos siguientes están abiertos y dirigidos a la dirección IP del servidor de destino:

- Puerto 80: tráfico web HTTP

- Puerto 443: tráfico web HTTPS

> [!NOTE]
> Si ha configurado un servidor local de Exchange en un segundo servidor debe asegurarse de que el puerto 25 (para SMTP) también está abierto y de que se redirige a la dirección IP del servidor local de Exchange.

## <a name="remove-legacy-active-directory-group-policy-objects"></a>Quitar objetos de directiva de grupo de Active Directory heredados
Los objetos de directiva de grupo (GPO) se actualizan para Windows Server Essentials. Son un superconjunto de los GPO de Windows SBS 2008. En Windows Server Essentials, se deben eliminar de forma manual una serie de los GPO de Windows SBS 2008 y los filtros de Instrumental de administración de Windows (WMI) para evitar conflictos con los GPO y los filtros WMI de Windows Server Essentials.

> [!NOTE]
> Si ha modificado los objetos de directiva de grupo originales de Windows SBS 2008 debe guardar copias en una ubicación diferente y eliminarlos de Windows SBS 2008.

#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2008"></a>Para quitar objetos antiguos de directiva de grupo de Windows SBS 2008

1. Inicie sesión en el servidor de origen con una cuenta de administrador.

2. Haga clic en **Inicio** y, después, en **Administración de servidores**.

3. En el panel de navegación, haga clic en **Administración avanzada**, en **Administración de directiva de grupo**y, a continuación, en **bosque:** _<sudominio \> _.

4. Haga clic en **dominios**, haga clic en *<\> sudominio*y, a continuación, haga clic en **Directiva de grupo objetos**.

5. Haga clic con el botón secundario en **Directiva de auditoría de Small Business Server**, en **Eliminar** y, a continuación, en **Aceptar**.

6. Repita el paso 5 para eliminar los siguientes GPO que se aplican a la red:

 - Equipo cliente de Small Business Server

 - Directiva de contraseña de dominio de Small Business Server

Se recomienda configurar la Directiva de contraseñas en Windows Server Essentials para aplicar contraseñas seguras. Para configurar la directiva de contraseñas use el panel, que escribe la configuración en la directiva de dominio predeterminada. La configuración de la directiva de contraseña no se escribe en el objeto de directiva de contraseña de dominio de Small Business Server, como ocurría en Windows SBS 2008.

 - Firewall de conexión a Internet de Small Business Server

 - Directiva de bloqueo de Small Business Server

 - Directiva de asistencia remota de Small Business Server

 - Firewall de Windows de Small Business Server

 - Directiva de equipo cliente de servicios de actualización de Small Business Server

 Este GPO estará disponible si efectúa una migración desde Windows SBS 2008 R2.

 - Directiva de configuración común de servicios de actualización de Small Business Server

 Este GPO estará disponible si efectúa una migración desde Windows SBS 2008 R2.

 - Directiva de equipo de servidor de servicios de actualización de Small Business Server

 Este GPO estará disponible si efectúa una migración desde Windows SBS 2008 R2.

7. Compruebe que se hayan eliminado todos los GPO.

#### <a name="to-remove-wmi-filters-from-windows-sbs-2008"></a>Para quitar los filtros WMI de Windows SBS 2008

1. Inicie sesión en el servidor de origen con una cuenta de administrador.

2. Haga clic en **Inicio** y, después, en **Administración de servidores**.

3. En el panel de navegación, haga clic en **Administración avanzada**, en **Administración de directiva de grupo**y, a continuación, en **bosque:** _<nombredominiored \> _

4. Haga clic en **dominios**, haga clic en *<\> nombredominiored*y, a continuación, en **filtros WMI**.

5. Haga clic con el botón secundario en **PostSP2**, haga clic en **Eliminar** y, a continuación, haga clic en **Sí**.

6. Haga clic con el botón secundario en **PreSP2**, haga clic en **Eliminar** y, a continuación, haga clic en **Sí**.

7. Compruebe que se hayan eliminado los tres filtros WMI.

## <a name="map-permitted-computers-to-user-accounts"></a>Asignar los equipos permitidos a cuentas de usuario
En Windows SBS 2008, si un usuario se conecta a Acceso Web remoto, se muestran todos los equipos de la red. Puede incluir los equipos a los que el usuario no puede acceder. En Windows Server Essentials, un usuario debe estar asignado explícitamente a un equipo para que se muestre en acceso Web remoto. Todas las cuentas de usuario que se migren desde Windows SBS 2008 deben asignarse a uno o más equipos.

#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario a equipos:

1. Abra el panel de Windows Server Essentials.

2. En la barra de navegación, haga clic en **Usuarios**.

3. En la lista de cuentas de usuario, haga clic con el botón secundario en una cuenta de usuario y, con el botón primario, en **Ver las propiedades de la cuenta**.

4. Haga clic en la pestaña **Acceso desde cualquier lugar** y, a continuación, haga clic en **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.

5. Seleccione **Carpetas compartidas**, **Equipos**, **Enlaces a la página de inicio** y, a continuación, haga clic en **Aplicar**.

6. Haga clic en la pestaña **Acceso al equipo** y en el nombre del equipo al que desea permitir el acceso.

7. Repita los pasos 3, 4, 5 y 6 para cada una de las cuentas de usuario.

> [!NOTE]
> No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.
>
> Tras completar la migración, si se produce algún problema al crear la primera cuenta de usuario nueva en el servidor de destino, quite la cuenta de usuario que ha agregado y créela de nuevo.
