---
title: Migración de la base de datos de WSUS desde WID de (Windows Internal Database) a SQL
description: 'Tema de Windows Server Update Service (WSUS): Cómo migrar la base de datos de WSUS (SUSDB) de una instancia de Windows Internal Database a una instancia local o remota de SQL Server.'
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/25/2018
ms.openlocfilehash: 54f7eb0464d4454bd2929aace44eb37567973154
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624468"
---
# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>Migración de la base de datos de WSUS de WID a SQL

> Se aplica a: Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016

Siga estos pasos para migrar la base de datos de WSUS (SUSDB) de una instancia de Windows Internal Database a una instancia local o remota de SQL Server.

## <a name="prerequisites"></a>Requisitos previos

- Instancia de SQL. Puede ser el valor predeterminado **MSSQLSERVER** o una instancia personalizada.
- SQL Server Management Studio
- WSUS con el rol WID instalado
- IIS (normalmente se incluye al instalar WSUS a través de Administrador del servidor). Todavía no está instalado, deberá ser.

## <a name="migrating-the-wsus-database"></a>Migrar la base de datos de WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Detener los servicios IIS y WSUS en el servidor WSUS

Desde PowerShell (elevado), ejecute:

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Desasociar SUSDB de Windows Internal Database

#### <a name="using-sql-management-studio"></a>Usar SQL Management Studio

1. Haga clic con el botón derecho en **SUSDB** - &gt; **tareas** - &gt; haga clic en **desasociar**: ![ image1](images/image1.png)
2. Active **quitar conexiones existentes** y haga clic en **Aceptar** (opcional si existen conexiones activas).
    ![Imagen 2](images/image2.png)

#### <a name="using-command-prompt"></a>Uso de la ventana del símbolo del sistema

> [!IMPORTANT]
> En estos pasos se muestra cómo desasociar la base de datos de WSUS (SUSDB) de la instancia de Windows Internal Database mediante la utilidad **sqlcmd** . Para obtener más información acerca de la utilidad **sqlcmd** , vea [sqlcmd (utilidad](https://go.microsoft.com/fwlink/?LinkId=81183)).
> 1. Abra un símbolo del sistema con privilegios elevados
> 2. Ejecute el siguiente comando SQL para desasociar la base de datos de WSUS (SUSDB) de la instancia de Windows Internal Database mediante la utilidad **sqlcmd** :

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>Copie los archivos SUSDB en el SQL Server

1. Copie **SUSDB. MDF** y **SUSDB \_ log. ldf** de la carpeta de datos de WID (**% SystemDrive%** \\ **Windows \\ WID \\ Data**) en la carpeta de datos de instancia de SQL.

> [!TIP]
> Por ejemplo, si la carpeta de instancia de SQL es **c:\Archivos de programa\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**, y la carpeta de datos WID es **C:\Windows\WID\Data,** copie los archivos SUSDB de **C:\Windows\WID\Data** a **c:\Archivos de programa\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Adjuntar SUSDB a la instancia de SQL

1. En **SQL Server Management Studio**, en el nodo **instancia** , haga clic con el botón secundario en **bases**de datos y, a continuación, haga clic en **asociar**.
    ![image3](images/image3.png)
2. En el cuadro **adjuntar bases de datos** , en **bases de datos que se van a adjuntar**, haga clic en el botón **Agregar** y busque el archivo **SUSDB. MDF** (copiado de la carpeta WID) y, a continuación, haga clic en **Aceptar**.
    ![archivo image4 ](images/image4.png) ![ image5](images/image5.png)

> [!TIP]
> Esto también se puede hacer mediante Transact-SQL.  Consulte la [documentación de SQL para adjuntar una base de datos](/sql/relational-databases/databases/attach-a-database) para obtener instrucciones.
>
> Ejemplo (uso de rutas de acceso del ejemplo anterior):
> ```sql
>    USE master;
>    GO
>    CREATE DATABASE SUSDB
>    ON
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\SUSDB.mdf'),
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Log\SUSDB_Log.ldf')
>        FOR ATTACH;
>    GO
>```

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Comprobar los inicios de sesión y permisos de SQL Server y base de datos

#### <a name="sql-server-login-permissions"></a>Permisos de inicio de sesión SQL Server

Después de adjuntar el SUSDB, compruebe que **NT Authority\Network Service** tiene permisos de inicio de sesión para la instancia de SQL Server mediante el procedimiento siguiente:

1. Entrar en SQL Server Management Studio
2. Abrir la instancia
3. Haga clic en **seguridad**
4. Haga clic en **inicios de sesión**

Se debe mostrar la cuenta **NT Authority\Network Service** . Si no es así, debe agregarlo agregando un nuevo nombre de inicio de sesión.

> [!IMPORTANT]
> Si la instancia de SQL está en un equipo diferente de WSUS, la cuenta de equipo del servidor WSUS debe aparecer en el formato **[FQDN] \\ [WSUSComputerName] $**.  Si no es así, se pueden usar los pasos siguientes para agregarlo, reemplazando **NT Authority\Network Service** con la cuenta de equipo del servidor de WSUS (**[FQDN] \\ [WSUSComputerName] $**) esto sería ***además de*** conceder derechos a **NT Authority\Network Service** .

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Incorporación de NT AUTHORITY\NETWORK SERVICE y concesión de derechos de ti

1. Haga clic con el botón derecho en **inicios de sesión** y seleccione **nuevo inicio de sesión...**
    ![image6](images/image6.png)
2. En la página **General** , rellene el **nombre de inicio de sesión** (**NT Authority\Network Service**) y establezca la **base de datos predeterminada** en SUSDB.
    ![imagen7](images/image7.png)
3. En la página **roles de servidor** , asegúrese de que está seleccionado **público** y **sysadmin** .
    ![imagen8](images/image8.png)
4. En la página **asignación de usuarios** :
    - En **usuarios asignados a este inicio de sesión**: seleccione **SUSDB**
    - En **pertenencia al rol de la base de datos para: SUSDB**, asegúrese de que está activada la siguiente:
        - **public**
        - **servicio Webservicio** ![ image9](images/image9.png)
5. Haga clic en **Aceptar**

Ahora debería ver **NT Authority\Network Service** en inicios de sesión.
![imagen10](images/image10.png)

#### <a name="database-permissions"></a>Permisos para la base de datos

1. Haga clic con el botón secundario en SUSDB
2. Seleccionar **propiedades**
3. Haga clic en **permisos**

Se debe mostrar la cuenta **NT Authority\Network Service** .

1. Si no es así, agregue la cuenta.
2. En el cuadro de texto nombre de inicio de sesión, escriba el equipo WSUS con el siguiente formato:
    > [**FQDN] \\ [WSUSComputerName] $**
3. Compruebe que la **base de datos predeterminada** está establecida en **SUSDB**.

    > [!TIP]
    > En el ejemplo siguiente, el FQDN es **Contosto.com** y el nombre del equipo WSUS es **WsusMachine**:
    >
    > ![imagen11](images/image11.png)

4. En la página **asignación de usuarios** , seleccione la base de datos **SUSDB** en **usuarios asignados a este inicio de sesión** .
5. Active **WebService** en la **pertenencia al rol de la base de datos para: SUSDB**:  ![ IMAGE12](images/image12.png)
6. Haga clic en  **Aceptar** para guardar la configuración.
    > [!NOTE]
    > Es posible que tenga que reiniciar el servicio SQL para que los cambios surtan efecto.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Modificar el registro para que WSUS apunte a la instancia de SQL Server

> [!IMPORTANT]
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/help/322756), por si se produjeran problemas.

1. Haga clic en **Inicio**, en **Ejecutar**, escriba **regedit&** y, después, haga clic en **Aceptar**.
2. Busque la clave siguiente: **HKEY_LOCAL_MACHINE \software\microsoft\updateservices\server\setup\sqlservername**
3. En el cuadro de texto **valor** , escriba **[ServerName] \\ [nombreDeInstancia]** y, a continuación, haga clic en **Aceptar**. Si el nombre de la instancia es la instancia predeterminada, escriba **[ServerName]**.
4. Busque la clave siguiente: **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed role Services\UpdateServices-WidDatabase** ![ image13](images/image13.png)
5. Cambie el nombre de la clave a **UpdateServices-Database** ![ image41](images/image14.png)

    > [!NOTE]
    > Si no actualiza esta clave, **WsusUtil** intentará atender el WID en lugar de la instancia de SQL a la que ha migrado.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Iniciar los servicios IIS y WSUS en el servidor WSUS

Desde PowerShell (elevado), ejecute:

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Si usa la consola de WSUS, ciérrela y reiníciela.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Desinstalación del rol WID (no recomendado)

> [!WARNING]
> Al quitar el rol WID también se quita una carpeta de base de datos (**%SystemDrive%\Archivos de Programa\update Services\Database**) que contiene los scripts requeridos por WSUSUtil.exe para las tareas posteriores a la instalación. Si decide desinstalar el rol WID, asegúrese de hacer una copia de seguridad de la carpeta **%SystemDrive%\Archivos de Programa\update Services\Database** con antelación.

Con PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Una vez quitado el rol WID, compruebe que la siguiente clave del registro está presente: **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed role Services\UpdateServices-Database**