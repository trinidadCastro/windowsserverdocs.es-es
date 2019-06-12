---
title: Actualización a AD FS en Windows Server 2016 con SQL Server
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0a3db2a095d1a31f55bd1c8bfc5bf3c9f6bb65b8
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687395"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Actualización a AD FS en Windows Server 2016 con SQL Server


> [!NOTE]  
> Solo puede iniciar una actualización con un período de tiempo definitivo planeado para la finalización. No se recomienda tener AD FS en un estado de modo mixto durante un largo período de tiempo, como ADFS dejando en un estado de modo mixto pueden causar problemas con la granja de servidores.


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Movimiento de una granja de servidores de Windows Server 2012 R2 AD FS a una granja de servidores de AD FS en Windows Server 2016  
El siguiente documento describe cómo actualizar la granja de servidores de AD FS Windows Server 2012 R2 a AD FS en Windows Server 2016, cuando se usa un servidor SQL Server para la base de datos de AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Actualizar AD FS a Windows Server 2016 FBL  
La característica de nivel de comportamiento de granja de servidores (FBL) es nueva en AD FS para Windows Server 2016.   Esta característica es toda la granja y determina las características que puede usar la granja de servidores de AD FS.   De forma predeterminada, el FBL en una granja de servidores de Windows Server 2012 R2 AD FS está en el FBL Windows Server 2012 R2.  

Un servidor de AD FS en Windows Server 2016 se puede agregar a una granja de servidores de Windows Server 2012 R2 y funcionando en el mismo FBL como un Windows Server 2012 R2.  Cuando haya un servidor de Windows Server 2016 AD FS funciona de esta forma, se dice que la granja de servidores "mixed".  Sin embargo, no podrá aprovechar las ventajas de las nuevas características de Windows Server 2016 hasta que se genere el FBL a Windows Server 2016.  Con una granja de servidores mixto:  

-   Los administradores pueden agregar nuevos servidores de federación de Windows Server 2016 a una granja existente de Windows Server 2012 R2.  Como resultado, la granja de servidores está en "modo mixto" y funciona en el nivel de comportamiento de granja de servidores de Windows Server 2012 R2.  Para garantizar un comportamiento coherente en la granja de servidores, nuevas características de Windows Server 2016 no pueden ser configuradas o utilizadas en este modo.  

-   Una vez que se han quitado todos los servidores de federación de Windows Server 2012 R2 de la granja de servidores de modo mixto y, en el caso de una granja WID, uno de los nuevos servidores de federación de Windows Server 2016 se ha promovido a la función del nodo principal, el administrador, a continuación, puede elevar la FBL de Win Windows Server 2012 R2 a Windows Server 2016.  Como resultado, a continuación, se pueden configurar y usar las nuevas características de AD FS Windows Server 2016.  

-   Como resultado de la característica de granja de servidores mixto, AD FS Windows Server 2012 R2, las organizaciones que deseen para actualizar a Windows Server 2016 no tendrá que implementar una granja de servidores completamente nuevo, exportar e importar datos de configuración.  En su lugar, puede agregar nodos de Windows Server 2016 a una granja existente mientras está en línea y solo procesos implicados en raise FBL el tiempo de inactividad relativamente breve.  

Tenga en cuenta que en el modo mixto de la granja de servidores, la granja de servidores de AD FS no es capaz de características nuevas ni funcionalidad presentada en AD FS en Windows Server 2016.  Esto significa que las organizaciones que desean probar las nuevas características no pueden hacerlo hasta que se genere el FBL.  Por lo que si su organización está considerando para probar las nuevas características antes de rasing el FBL, deberá implementar una granja de servidores independiente para hacer esto.  

El resto de la es el documento proporciona los pasos para agregar un servidor de federación de Windows Server 2016 en un entorno de Windows Server 2012 R2 y, a continuación, generar el FBL a Windows Server 2016.  Estos pasos se realizaron en un entorno de pruebas que se describen en el diagrama arquitectónico siguiente.  

> [!NOTE]  
> Antes de poder mover a AD FS en Windows Server 2016 FBL, debe quitar todos los nodos de Windows 2012 R2.  Simplemente no se puede actualizar un sistema operativo de Windows Server 2012 R2 a Windows Server 2016 y hacer que se convertirá en un nodo de 2016.  Debe quitarlo y reemplazarlo con un nuevo nodo de 2016.  

En el siguiente diagrama de arquitectura muestra la configuración que se usó para validar y grabar los pasos siguientes.

![Arquitectura](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Unir el servidor de AD FS de Windows 2016 a la granja de AD FS

1.  Usar el administrador del servidor de instalar el rol de servicios de federación de Active Directory en Windows Server 2016  

2.  Mediante el Asistente para configuración de AD FS, combinar el nuevo servidor de Windows Server 2016 a la granja de AD FS existente.  En el **bienvenida** pantalla clic **siguiente**.
 ![Unirse a granja](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  En el **conectar con Active Directory Domain Services** pantalla, s**especificar una cuenta de administrador** con permisos para realizar la configuración de servicios de federación y haga clic en **siguiente**.
4.  En el **especificar granja de servidores** pantalla, escriba el nombre del servidor SQL y la instancia y, a continuación, haga clic en **siguiente**.
![Unirse a granja](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  En el **especificar certificado SSL** pantalla, especifique el certificado y haga clic en **siguiente**.
![Unirse a granja](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  En el **especificar cuenta de servicio** pantalla, especifique la cuenta de servicio y haga clic en **siguiente**.
7.  En el **Revisar opciones** pantalla, revise las opciones y haga clic en **siguiente**.
8.  En el **comprobaciones de requisitos previos** pantalla, asegúrese de que todas las comprobaciones de requisitos previos han pasado y haga clic en **configurar**.
9.  En el **resultados** pantalla, asegúrese de que el servidor se configuró correctamente y haga clic en **cerrar**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Quitar el servidor de Windows Server 2012 R2 AD FS

>[!NOTE]
>No es necesario establecer el servidor de AD FS principal mediante Set-AdfsSyncProperties-rol cuando se usa SQL como la base de datos.  Esto es porque todos los nodos se consideran principales en esta configuración.

1.  En el servidor de Windows Server 2012 R2 AD FS en el administrador del servidor uso **quitar Roles y características** en **administrar**.
![Quitar servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  En la pantalla **Antes de comenzar**, haz clic en **Siguiente**.
3.  En el **selección de servidor** pantalla, haga clic en **siguiente**.
4.  En el **Roles de servidor** pantalla, quitar la marca de verificación junto a **Active Directory Federation Services** y haga clic en **siguiente**.
![Quitar servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  En el **características** pantalla, haga clic en **siguiente**.
6.  En el **confirmación** pantalla, haga clic en **quitar**.
7.  Una vez que se complete, reinicie el servidor.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Elevar el nivel de comportamiento de la granja de servidores (FBL)
Antes de este paso, deberá asegurarse de que se han ejecutado forestprep y domainprep en su entorno de Active Directory y que Active Directory tiene el esquema de Windows Server 2016.  Este documento a trabajar con un controlador de dominio de Windows 2016 y no era necesario ejecutar estos porque se ejecutan cuando se instaló AD.

>[!NOTE]
>Antes de comenzar este procedimiento, asegúrese de que Windows Server 2016 está actualizada ejecutando Windows Update desde configuración.  Continúa este proceso hasta que no se necesiten actualizaciones.

1. Ahora en el servidor de Windows Server 2016, abra PowerShell y ejecute lo siguiente: **$cred = Get-Credential** y presione ENTRAR.
2. Escriba las credenciales que tienen privilegios de administrador en el servidor SQL Server.
3. Ahora en PowerShell, escriba lo siguiente: **Invoke-AdfsFarmBehaviorLevelRaise -Credential $cred**
2. Cuando se le solicite, escriba **Y**.  Esto comenzará la elevación del nivel.  Cuando se complete correctamente que han surgido la FBL.  
![Finalizar actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Ahora, si va a administración de AD FS, verá los nuevos nodos que se han agregado para AD FS en Windows Server 2016  
4. Del mismo modo, puede usar el cmdlet de PowerShell:  Get-AdfsFarmInformation para mostrar el FBL actual.  
![Finalizar actualización](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Actualizar la versión de configuración de los servidores WAP existentes
1. En cada Proxy de aplicación Web, volver a configurar el WAP ejecutando el siguiente comando de PowerShell en una ventana con privilegios elevados:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. Quitar los antiguos servidores del clúster y conservar sólo los servidores WAP ejecutando la versión más reciente del servidor, que se vuelve a configurar anteriormente, ejecutando el siguiente cmdlet de Powershell.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Compruebe la configuración de WAP ejecutando el commmandlet Get-WebApplicationProxyConfiguration. El ConnectedServersName reflejará el servidor que ejecute el comando anterior.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Para actualizar la versión de configuración de los servidores WAP, ejecute el siguiente comando de Powershell.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Compruebe que la versión de configuración se ha actualizado con el comando de Powershell Get-WebApplicationProxyConfiguration.
