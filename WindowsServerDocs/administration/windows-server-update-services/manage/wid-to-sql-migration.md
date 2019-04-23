---
title: Migrar la base de datos WSUS (Windows Internal Database) WID para SQL
description: Tema de Windows Server Update Service (WSUS) - cómo migrar la base de datos WSUS (SUSDB) de una instancia de Windows Internal Database para una instancia Local o remota de SQL Server.
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: ed6f695947fc17d2e96b5282b3a67a221bb0140d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858036"
---
>Se aplica a: Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>Migrar la base de datos WSUS desde WID a SQL

Utilice los pasos siguientes para migrar la base de datos WSUS (SUSDB) desde una instancia de Windows Internal Database para una instancia Local o remota de SQL Server.

## <a name="prerequisites"></a>Requisitos previos

- Instancia de SQL. Esto puede ser el valor predeterminado **MSSQLServer** o una instancia personalizada.
- SQL Server Management Studio
- WSUS con la función WID instalada
- (Esto se suele incluir cuando instale WSUS mediante Administrador del servidor) de IIS. Ya no está instalado, deberá ser.

## <a name="migrating-the-wsus-database"></a>Migrar la base de datos WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Detenga los servicios IIS y WSUS en el servidor WSUS

En PowerShell (con privilegios elevado), ejecute:

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Desasociar SUSDB desde la base de datos interna de Windows

#### <a name="using-sql-management-studio"></a>Mediante SQL Management Studio

1. Haga clic en **SUSDB** - &gt; **tareas** - &gt; haga clic en **Detach**: ![image1](images/image1.png)
2. Comprobar **quitar conexiones existentes** y haga clic en **Aceptar** (opcional, si hay conexiones activas).
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>Uso de la ventana del símbolo del sistema

> [!IMPORTANT]
> Estos pasos muestran cómo desasociar la base de datos WSUS (SUSDB) de la instancia de base de datos interna de Windows mediante el uso de la **sqlcmd** utilidad. Para obtener más información sobre la **sqlcmd** utilidad, vea [utilidad sqlcmd](https://go.microsoft.com/fwlink/?LinkId=81183).
1. Abra un símbolo del sistema con privilegios elevados
2. Ejecute el siguiente comando SQL para desasociar la base de datos WSUS (SUSDB) de la instancia de base de datos interna de Windows mediante el uso de la **sqlcmd** utilidad:

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>Copie los archivos SUSDB en el servidor SQL Server

1. Copia **SUSDB.mdf** y **SUSDB\_log.ldf** desde la carpeta de datos WID (**% SystemDrive %**\** Windows\WID\Data **) a los datos de la instancia SQL Carpeta.

> [!TIP]
> Por ejemplo, si su carpeta de la instancia de SQL es **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**, y es la carpeta de datos de WID **C:\Windows\WID\Data,** copie los archivos SUSDB de **C:\Windows\WID\Data** a **C:\Program Files\Microsoft SQL Server \MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Adjunte SUSDB a la instancia SQL

1. En **SQL Server Management Studio**, en el **instancia** nodo, haga clic en **bases de datos**y, a continuación, haga clic en **adjuntar**.
    ![image3](images/image3.png)
2. En el **adjuntar bases de datos** cuadro de **bases de datos para adjuntar**, haga clic en el **agregar** y busque el **SUSDB.mdf** archivo (copiado de la Carpeta WID) y, a continuación, haga clic en **Aceptar**.
    ![image4](images/image4.png) ![image5](images/image5.png)

> [!TIP]
> Esto también es capaz de hacerse mediante Transact-Sql.  Consulte la [documentación para adjuntar una base de datos SQL](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) para sus instrucciones.
>
> Ejemplo (con las rutas de acceso del ejemplo anterior):
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

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Compruebe que SQL Server y los inicios de sesión de base de datos y permisos

#### <a name="sql-server-login-permissions"></a>Permisos de inicio de sesión SQL Server

Después de adjuntar el SUSDB, compruebe que **NT AUTHORITY\NETWORK SERVICE** tiene permisos de inicio de sesión para la instancia de SQL Server haciendo lo siguiente:

1. Vaya a SQL Server Management Studio
2. Apertura de la instancia
3. Haga clic en **seguridad**
4. Haga clic en **inicios de sesión**

El **NT AUTHORITY\NETWORK SERVICE** cuenta debería aparecer. Si no es así, deberá agregarlo al agregar el nuevo nombre de inicio de sesión.

> [!IMPORTANT]
> Si la instancia de SQL está en un equipo diferente de WSUS, cuenta de equipo del servidor WSUS debe aparecer en el formato **[FQDN]\\[WSUSComputerName] $**.  Si no, se pueden usar los pasos siguientes para agregarlo, reemplace **NT AUTHORITY\NETWORK SERVICE** con la cuenta de equipo del servidor WSUS (**[FQDN]\\[WSUSComputerName] $**) sería ***además*** conceder derechos a **NT AUTHORITY\NETWORK SERVICE**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Adición de NT AUTHORITY\NETWORK SERVICE y concédale derechos

1. Haga clic en **inicios de sesión** y haga clic en **nuevo inicio de sesión...**
    ![image6](images/image6.png)
2. En el **General** página, rellene el **nombre de inicio de sesión** (**NT AUTHORITY\NETWORK SERVICE**) y establezca el **base de datos predeterminada** a SUSDB.
    ![image7](images/image7.png)
3. En el **Roles de servidor** , asegúrese de **pública** y **sysadmin** están seleccionadas.
    ![image8](images/image8.png)
4. En el **User Mapping** página:
    - En **usuarios asignados a este inicio de sesión**: seleccione **SUSDB**
    - Bajo **pertenencia al rol de la base de datos: SUSDB**, asegúrese de que se comprueban los siguientes:
        - **public**
        - **webService** ![image9](images/image9.png)
5. Haz clic en **Aceptar**.

Ahora debería ver **NT AUTHORITY\NETWORK SERVICE** en inicios de sesión.
![image10](images/image10.png)

#### <a name="database-permissions"></a>Permisos de base de datos

1. Haga clic en el SUSDB
2. Seleccione **propiedades**
3. Haga clic en **permisos**

El **NT AUTHORITY\NETWORK SERVICE** cuenta debería aparecer.

1. Si no es así, agregue la cuenta.
2. En el cuadro de texto Nombre de inicio de sesión, escriba la máquina WSUS en el formato siguiente:
    > [**FQDN]\\[WSUSComputerName] $**
3. Compruebe que la **base de datos predeterminada** está establecido en **SUSDB**.

    > [!TIP]
    > En el ejemplo siguiente, el FQDN es **Contosto.com** y es el nombre del equipo WSUS **WsusMachine**:
    >
    > ![Image11](images/image11.png)

4. En el **User Mapping** página, seleccione el **SUSDB** en la base de datos **"Usuarios asignados a este inicio de sesión"**
5. Comprobar **webservice** bajo el **"miembros del rol para la base de datos: SUSDB"**: ![image12](images/image12.png)
6. Haga clic en **Aceptar** para guardar la configuración.
    > [!NOTE]
    > Es posible que deba reiniciar el servicio de SQL para que los cambios surtan efecto.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Edite el registro para el punto de WSUS a la instancia de SQL Server

> [!IMPORTANT]
> Siga los pasos descritos en esta sección con cuidado. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756) en caso de que se producen problemas.

1. Haga clic en **Inicio**, luego en **Ejecutar**, escriba **regedit**y, a continuación, haga clic en **OK**.
2. Busque la siguiente clave: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. En el **valor** cuadro de texto, escriba **[ServerName]\\[nombreDeInstancia]** y, a continuación, haga clic en **Aceptar**. Si el nombre de instancia es la instancia predeterminada, escriba **[ServerName]**.
4. Busque la siguiente clave: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed rol Services\UpdateServices-WidDatabase** ![image13](images/image13.png)
5. Cambiar el nombre de la clave para **UpdateServices-base de datos** ![image41](images/image14.png)

    > [!NOTE]
    > Si no actualiza esta clave, a continuación, **WsusUtil** intentará la WID, en lugar de la instancia de SQL a la que se han migrado del servicio.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Inicie los servicios IIS y WSUS en el servidor WSUS

En PowerShell (con privilegios elevado), ejecute:

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Si usa la consola de WSUS, cierre y reinícielo.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Desinstalar el rol WID (no recomendado)

> [!WARNING]
> Quitar el rol WID también quita una carpeta de base de datos (**%SystemDrive%\Program programa\Update Services\Database**) que contiene los scripts necesarios por WSUSUtil.exe para las tareas posteriores a la instalación. Si decide desinstalar el rol de WID, asegúrese de copia de seguridad de la **%SystemDrive%\Program programa\Update Services\Database** carpeta con antelación.

Uso de PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Después de quita el rol WID, compruebe que la siguiente clave del registro está presente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed rol Services\UpdateServices bases de datos**