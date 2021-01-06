---
description: Más información acerca de la actualización a AD FS en Windows Server 2016 con SQL Server
title: Actualización a AD FS en Windows Server 2016 con SQL Server
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.author: billmath
ms.openlocfilehash: 5fcc31c0b0f0482a545d17135603efaa97e174d7
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948531"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Actualización a AD FS en Windows Server 2016 con SQL Server


> [!NOTE]
> Solo se inicia una actualización con un período de tiempo definitivo planeado para su finalización. No se recomienda mantener AD FS en un estado de modo mixto durante un largo período de tiempo, ya que si se deja AD FS en un estado de modo mixto, pueden producirse problemas con la granja.


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Pasar de una granja de servidores de AD FS de Windows Server 2012 R2 a una granja de servidores de Windows Server 2016 AD FS
En el documento siguiente se describe cómo actualizar la AD FS granja de Windows Server 2012 R2 a AD FS en Windows Server 2016 cuando se usa un SQL Server para la base de datos de AD FS.

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Actualizar AD FS a Windows Server 2016 FBL
Novedad de AD FS para Windows Server 2016 es la característica de nivel de comportamiento de la granja de servidores (FBL).   Estas características están en todo el conjunto y determinan las características que puede usar la granja de AD FS.   De forma predeterminada, FBL en una granja de servidores de Windows Server 2012 R2 AD FS está en el FBL de Windows Server 2012 R2.

Se puede Agregar un servidor de AD FS de Windows Server 2016 a una granja de servidores de Windows Server 2012 R2 y funcionará en el mismo FBL que Windows Server 2012 R2.  Cuando tiene un servidor de AD FS de Windows Server 2016 que funciona de esta manera, se dice que la granja está "mixta".  Sin embargo, no podrá beneficiarse de las nuevas características de Windows Server 2016 hasta que se genere FBL en Windows Server 2016.  Con una granja mixta:

-   Los administradores pueden agregar nuevos servidores de Federación de Windows Server 2016 a una granja existente de Windows Server 2012 R2.  Como resultado, la granja está en "modo mixto" y funciona el nivel de comportamiento de la granja de servidores de Windows Server 2012 R2.  Para garantizar un comportamiento coherente en la granja, las nuevas características de Windows Server 2016 no se pueden configurar ni usar en este modo.

-   Una vez que todos los servidores de Federación de Windows Server 2012 R2 se han quitado de la granja de servidores de modo mixto y, en el caso de una granja de servidores WID, uno de los nuevos servidores de Federación de Windows Serve 2016 se ha promocionado al rol de nodo principal, el administrador puede elevar el FBL de Windows Server 2012 R2 a Windows Server 2016.  Como resultado, se pueden configurar y usar las nuevas características AD FS Windows Server 2016.

-   Como resultado de la característica de granja mixta, AD FS las organizaciones de Windows Server 2012 R2 que desean actualizar a Windows Server 2016 no tendrán que implementar una granja completamente nueva, exportar e importar los datos de configuración.  En su lugar, pueden agregar nodos de Windows Server 2016 a una granja existente mientras están en línea y solo incurren en un tiempo de inactividad relativamente breve relacionado con el FBL.

Tenga en cuenta que, en el modo de granja mixta, la granja de AD FS no es capaz de ofrecer nuevas características o funciones introducidas en AD FS en Windows Server 2016.  Esto significa que las organizaciones que deseen probar nuevas características no podrán hacerlo hasta que se genere el FBL.  Por lo tanto, si su organización desea probar las nuevas características antes de generar FBL, deberá implementar una granja independiente para hacerlo.

El resto del documento is proporciona los pasos para agregar un servidor de Federación de Windows Server 2016 a un entorno de Windows Server 2012 R2 y, a continuación, elevar el FBL a Windows Server 2016.  Estos pasos se realizaron en un entorno de prueba que se describe en el diagrama arquitectónico siguiente.

> [!NOTE]
> Antes de poder pasar a AD FS en Windows Server 2016 FBL, debe quitar todos los nodos de Windows 2012 R2.  No se puede actualizar un sistema operativo Windows Server 2012 R2 a Windows Server 2016 y hacer que se convierta en un nodo 2016.  Tendrá que quitarlo y reemplazarlo por un nuevo nodo 2016.

En el diagrama arquitectónico siguiente se muestra la configuración que se usó para validar y grabar los pasos siguientes.

![Architecture](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Unir el servidor de AD FS de Windows 2016 a la granja de AD FS

1.  Usar Administrador del servidor instalar el rol de Servicios de federación de Active Directory (AD FS) en Windows Server 2016

2.  Mediante el Asistente para configuración de AD FS, una el nuevo servidor de Windows Server 2016 a la granja de AD FS existente.  En la pantalla de **bienvenida** , haga clic en **siguiente**.
![Captura de pantalla que muestra la pantalla de bienvenida del Asistente para configuración de AD FS.](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)
3.  En la pantalla **conectar a Active Directory Domain Services** , s **pecifique una cuenta de administrador** con permisos para realizar la configuración de servicios de Federación y haz clic en **siguiente**.
4.  En la pantalla **especificar granja de servidores** , escriba el nombre de la instancia de SQL Server y, a continuación, haga clic en **siguiente**.
![Captura de pantalla que muestra la pantalla especificar granja en el Asistente para configuración de AD FS.](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  En la pantalla **especificar certificado SSL** , especifique el certificado y haga clic en **siguiente**.
![Unirse a una granja](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  En la pantalla **especificar cuenta de servicio** , especifique la cuenta de servicio y haga clic en **siguiente**.
7.  En la pantalla **Opciones de revisión** , revise las opciones y haga clic en **siguiente**.
8.  En la pantalla **comprobaciones de requisitos** previos, asegúrese de que se han superado todas las comprobaciones de requisitos previos y haga clic en **configurar**.
9.  En la pantalla **resultados** , asegúrese de que el servidor se haya configurado correctamente y haga clic en **cerrar**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Quitar el servidor de AD FS de Windows Server 2012 R2

>[!NOTE]
>No es necesario establecer el servidor de AD FS principal con Set-AdfsSyncProperties rol cuando se usa SQL como base de datos.  Esto se debe a que todos los nodos se consideran principales en esta configuración.

1.  En el servidor de AD FS de Windows Server 2012 R2 en Administrador del servidor use **quitar roles y características** en **administrar**.
![Captura de pantalla que resalta la opción de menú quitar roles y características.](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  En la pantalla **Antes de comenzar**, haz clic en **Siguiente**.
3.  En la pantalla **selección de servidor** , haga clic en **siguiente**.
4.  En la pantalla **roles de servidor** , desactive la casilla situada junto a **servicios de Federación de Active Directory (AD FS)** y haga clic en **siguiente**.
![Quitar servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  En la pantalla **características** , haga clic en **siguiente**.
6.  En la pantalla **confirmación** , haga clic en **quitar**.
7.  Cuando se complete, reinicie el servidor.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Elevar el nivel de comportamiento de la granja de servidores (FBL)
Antes de este paso, debe asegurarse de que se han ejecutado ForestPrep y DomainPrep en el entorno de Active Directory y que Active Directory tiene el esquema 2016 de Windows Server.  Este documento se inició con un controlador de dominio de Windows 2016 y no requirió su ejecución porque se ejecutaba cuando se instaló AD.

>[!NOTE]
>Antes de iniciar el proceso siguiente, asegúrese de que Windows Server 2016 está actualizado ejecutando Windows Update desde la configuración.  Continúe con este proceso hasta que no se necesiten más actualizaciones. Además, asegúrese de que la cuenta de la cuenta del servicio ADFS tiene los permisos administrativos en el servidor SQL Server y en cada servidor de la granja de AD FS.

1. Ahora, en el servidor de Windows Server 2016, abra PowerShell y ejecute lo siguiente: **$CRED = Get-Credential** y presione Entrar.
2. Escriba las credenciales que tienen privilegios de administrador en el SQL Server.
3. Ahora en PowerShell, escriba lo siguiente: **Invoke-AdfsFarmBehaviorLevelRaise-Credential $CRED**
2. Cuando se le solicite, escriba **Y**.  Esto comenzará a elevar el nivel.  Una vez completado esto, habrá generado correctamente el FBL.
![Finalizar actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Ahora, si va a la administración de AD FS, verá los nuevos nodos que se han agregado para AD FS en Windows Server 2016
4. Del mismo modo, puede usar el cmdlet de PowerShell: Get-AdfsFarmInformation para mostrar el FBL actual.
![Captura de pantalla que muestra cómo usar el cmdlet Get-AdfsFarmInformation para mostrar la F B actual.](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Actualizar la versión de configuración de los servidores WAP existentes
1. En cada proxy de aplicación Web, vuelva a configurar el WAP ejecutando el siguiente comando de PowerShell en una ventana con privilegios elevados:
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
    ```
2. Quite los servidores antiguos del clúster y mantenga solo los servidores WAP que ejecuten la versión más reciente del servidor, que se han reconfigurado anteriormente, mediante la ejecución del siguiente comando de PowerShell.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Compruebe la configuración de WAP ejecutando el comando Get-WebApplicationProxyConfiguration. El ConnectedServersName reflejará el servidor que se ejecuta desde el comando anterior.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Para actualizar el ConfigurationVersion de los servidores WAP, ejecute el siguiente comando de PowerShell.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Compruebe que ConfigurationVersion se ha actualizado con el comando de PowerShell Get-WebApplicationProxyConfiguration.
