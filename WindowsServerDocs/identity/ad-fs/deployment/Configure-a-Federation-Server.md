---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guía de implementación de AD FS en Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2f597994aa74f453903e09f7d3eefd83f26faba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192270"
---
# <a name="configure-a-federation-server"></a>Configurar un servidor de federación

Después de instalar los servicios de federación de Active Directory \(AD FS\) servicio de rol en el equipo, está listo para configurar este equipo para convertirse en un servidor de federación. Puede realizar una de las acciones siguientes:  
  
-   [Configurar el primer servidor de federación en una nueva granja de servidores de federación](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Agregar un servidor de federación a una granja de servidores de federación existente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurar el primer servidor de federación en una nueva granja de servidores de federación  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Para configurar el primer servidor de federación en una nueva granja de servidores de federación mediante el Asistente de configuración del servicio de federación de Active Directory  
  
> [!NOTE]  
> Asegúrese de que dispone de permisos de administrador de dominio o tener credenciales de administrador de dominio disponibles antes de realizar este procedimiento.  
  
1.  En la página **Panel** del Administrador del servidor, haz clic en la marca **Notificaciones** y, a continuación, en **Configurar el servicio de federación en el servidor**.  
  
    Se abre el **Asistente para configuración de Servicios de federación de Active Directory** .  
  
2.  En la **Página principal** , selecciona **Crear el primer servidor de federación en una granja de servidores de federación**y haz clic en **Siguiente**.  
  
3.  En el **conectarse a AD DS** , especifique una cuenta mediante el uso de permisos de administrador de dominio de Active Directory \(AD\) a la que este equipo está unido a un dominio y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **Especificar propiedades del servicio**, haz lo que se explica a continuación y, después, haz clic en **Siguiente**:  
  
    -   Importar el archivo .pfx que contiene la capa de sockets seguros \(SSL\) certificado y la clave que obtuviste anteriormente. En [paso 2: Inscribir un certificado SSL para AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), ha obtenido este certificado y copiarla en el equipo que desea configurar como un servidor de federación. Para importar el archivo .pfx a través del asistente, haga clic en **importar**y, a continuación, vaya a la ubicación del archivo. Cuando se le pida, escriba la contraseña para el archivo. pfx.  
  
    -   Proporcione un nombre para el servicio de federación. Por ejemplo, **fs.contoso.com**. Este nombre debe coincidir con uno del sujeto o nombres alternativos del sujeto del certificado.  
  
    -   Proporcione un nombre para mostrar al servicio de federación. Por ejemplo, **Contoso Corporation**. Los usuarios ven este nombre en los servicios de federación de Active Directory \(AD FS\) sesión\-en la página.  
  
5.  En el **especificar cuenta de servicio** , especifique una cuenta de servicio. Puede crear o usar una cuenta de servicio administrada de grupo existente \(gMSA\) o usar una cuenta de usuario de dominio existente. Si selecciona la opción para crear una nueva cuenta de gMSA, especifique un nombre para la nueva cuenta. Si selecciona la opción de usar una cuenta de dominio o gMSA existente, haga clic en **seleccione** para seleccionar una cuenta.  
  
    > [!NOTE]  
    > La ventaja de usar una cuenta de gMSA es su auto\-negocia la característica de actualización de contraseña.  
  
    > [!WARNING]  
    > Si desea usar una cuenta de gMSA, debe tener al menos un controlador de dominio en su entorno que se está ejecutando el sistema operativo Windows Server 2012.  
    >   
    > Si la opción de gMSA está deshabilitada y verá un mensaje de error **cuentas de servicio administradas de grupo no están disponibles porque no se estableció la clave raíz de KDS**, puedes habilitar gMSA en tu dominio ejecutando el siguiente Windows Comando de PowerShell en un controlador de dominio que ejecuta Windows Server 2012 o posterior, en el dominio de Active Directory: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. A continuación, vuelva al asistente, haga clic en **anterior**y, a continuación, haga clic en **siguiente** a re\-introduzca el **especificar cuenta de servicio** página. Ahora debe habilitarse la opción de gMSA. Puede seleccionarla y escriba un nombre de cuenta de gMSA que desea usar.  
  
6.  En el **especificar base de datos de configuración** , especifique una base de datos de configuración de AD FS y, a continuación, haga clic en **siguiente**. Puede crear una base de datos en este equipo mediante el uso de Windows Internal Database \(WID\), o bien puede especificar la ubicación y el nombre de instancia de Microsoft SQL Server.  
  
    Para obtener más información, consulta [Rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012 y SQL Server 2014.  
  
7.  En la página **Revisar opciones**, comprueba la configuración que has seleccionado y haz clic en **Siguiente**.  
  
8.  En el **Pre\-comprobaciones de requisitos previos** , comprueba que se han realizado correctamente todas las comprobaciones de requisitos previos y, a continuación, haga clic en **configurar**.  
  
9. En el **resultados** , revise los resultados y comprueba que la configuración se ha completado correctamente y, a continuación, haga clic en **pasos necesarios para completar la implementación del servicio de federación**. Para obtener más información, consulte [pasos siguientes para completar la instalación de AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Haga clic en **Cerrar** para salir del asistente.  
  
### <a name="BKMK_3"></a>Para configurar el primer servidor de federación en una nueva granja de servidores de federación a través de Windows PowerShell  
Puede crear una nueva granja de servidores de federación mediante el uso de una cuenta gMSA nueva o existente o una cuenta de usuario de dominio existente.  
  
-   **Si desea crear un nuevo servidor de federación mediante el uso de una cuenta gMSA nueva, realice lo siguiente:**  
  
    > [!IMPORTANT]  
    > Debe tener permisos de administrador de dominio para crear el primer servidor de federación en una nueva granja de servidores de federación.  
  
    1.  En el equipo que desea configurar como un servidor de federación, asegúrese de que se ha importado el certificado SSL requerido en el **ordenador\\My Store** directory. Puede comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de comandos de Windows PowerShell: `dir Cert:\LocalMachine\My`. Aparece el certificado mediante su huella digital en el **ordenador\\My Store** directory.  
  
    2.  En el controlador de dominio, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando para comprobar si la clave raíz de KDS se ha creado en el dominio: `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Si no se ha creado para que el resultado no muestra ninguna información, ejecute el siguiente comando para crear la clave: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  En el equipo que desea configurar como un servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > El `$` se requiere inicio de sesión al final del comando anterior.  
  
        Para obtener el valor de `<certificate_thumbprint>`, ejecute `dir Cert:\LocalMachine\My`y, a continuación, seleccionar la huella digital del certificado SSL. El valor de `<federation_service_name>` es el nombre del servicio de federación, por ejemplo, **fs.contoso.com**.  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecute este comando, agregue el `OverwriteConfiguration` parámetro.  
  
        > [!NOTE]  
        > El comando anterior crea una granja WID. Si desea crear una granja de servidores de SQL Server, debe tener una instancia de SQL Server ya instalada y operativa.  
        >   
        > Puede usar el comando siguiente para crear el primer servidor de federación en una nueva granja que utiliza una instancia de SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` donde **< SQL\_Host\_nombre >** es el nombre del servidor en el que SQL Se está ejecutando el servidor, y **< SQL\_instancia\_nombre >** es el nombre de la instancia de SQL Server. Si usa la instancia predeterminada de SQL Server, use un **SQLConnectionString** valor de "**origen de datos\=< SQL\_Host\_nombre >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012.  
  
-   **Si desea crear un nuevo servidor de federación mediante el uso de una cuenta de usuario de dominio existente, realice lo siguiente:**  
  
    1.  En el equipo que desea configurar como un servidor de federación, asegúrese de que se ha importado el certificado SSL requerido en el **ordenador\\My Store** directory. Puede comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de comandos de Windows PowerShell: `dir Cert:\LocalMachine\My`. Aparece el certificado mediante su huella digital en el **ordenador\\My Store** directory.  
  
    2.  En el equipo que desea configurar como un servidor de federación, abra la ventana de comandos de Windows PowerShell y, a continuación, ejecute el siguiente comando: `$fscred = Get-Credential`. Escriba las credenciales de cuenta de usuario de dominio que desea usar para la cuenta de servicio de federación en el dominio de formato\\nombre de usuario.  
  
    3.  En la misma ventana de Windows PowerShell, ejecute el siguiente comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Para obtener el valor de **< certificado\_huella digital >** , ejecute `dir Cert:\LocalMachine\My`y, a continuación, seleccionar la huella digital del certificado SSL. El valor de **< federación\_servicio\_nombre >** es el nombre del servicio de federación, por ejemplo, fs.contoso.com.  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecute este comando, agregue el `OverwriteConfiguration` parámetro.  
  
        > [!NOTE]  
        > El comando anterior crea una granja WID. Si desea crear una granja de servidores de SQL Server, debe tener la instancia de SQL Server ya instalada y operativa.  
        >   
        > Puede usar el comando siguiente para crear el primer servidor de federación en una nueva granja que utiliza una instancia de SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` donde **SQL\_Host\_nombre** es el nombre del servidor en el que está SQL Server está ejecutando, y **SQL\_instancia\_nombre** es el nombre de la instancia de SQL Server. Si usa la instancia predeterminada de SQL Server, use un **SQLConnectionString** valor de "**origen de datos\=< SQL\_Host\_nombre >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012 y SQL Server 2014.  
  
## <a name="BKMK_2"></a>Agregar un servidor de federación a una granja de servidores de federación existente  
  
> [!IMPORTANT]  
> Asegúrese de que ha completado [paso 3: Instalar el servicio de rol de AD FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), antes de iniciar cualquiera de los procedimientos en esta sección.  
  
> [!IMPORTANT]  
> Asegúrese de que ha obtenido a un servidor SSL válido el certificado de autenticación antes de completar este procedimiento.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Para agregar un servidor de federación a una granja de servidores de federación existente mediante el Asistente de configuración del servicio de federación de Active Directory  
  
1.  En la página **Panel** del Administrador del servidor, haz clic en la marca **Notificaciones** y, a continuación, en **Configurar el servicio de federación en el servidor**.  
  
    Se abre el **Asistente para configuración de Servicios de federación de Active Directory** .  
  
2.  En el **bienvenida** página, seleccione **agregar un servidor de federación a una granja de servidores de federación**y, a continuación, haga clic en **siguiente**.  
  
3.  En el **conectarse a AD DS** página, especifique una cuenta con permisos de administrador de dominio para el dominio de AD al que está unido este equipo y, a continuación, haga clic en **siguiente**.  
  
4.  En el **especificar granja de servidores** página, proporcione el nombre del servidor de federación principal en una granja de servidores que se usa WID o especifique el nombre de host de la base de datos y el nombre de instancia de base de datos de una granja de servidores de federación existente que usa SQL Server.  
  
    > [!WARNING]  
    > En Windows Server® 2012 R2, hay una solución alternativa para especificar la instancia predeterminada de SQL Server. La solución alternativa es no utilizar la interfaz de usuario. En su lugar, use los pasos descritos en [para configurar el primer servidor de federación en una nueva granja de servidores de federación a través de Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012.  
  
5.  En el **especificar certificado SSL** página, importe el archivo .pfx que contiene el certificado SSL y la clave que obtuviste anteriormente. Este certificado es el certificado de autenticación de servicio necesario. En [paso 2: Inscribir un certificado SSL para AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), ha obtenido este certificado y copiado en el equipo que desea configurar como un servidor de federación. Para importar el archivo .pfx a través del asistente, haga clic en **importar** y vaya a la ubicación del archivo. Cuando se le pida, escriba la contraseña para el archivo. pfx.  
  
6.  En el **especificar cuenta de servicio** , especifique la misma cuenta de servicio que configuró al crear el primer servidor de federación en la granja de servidores. Puede usar una cuenta de servicio administrada de grupo existente o una cuenta de usuario de dominio existente.  
  
    > [!IMPORTANT]  
    > La cuenta que especifique debe ser la misma cuenta que la cuenta que se usó en el servidor de federación principal en esta granja de servidores.  
  
7.  En la página **Revisar opciones**, comprueba la configuración que has seleccionado y haz clic en **Siguiente**.  
  
8.  En el **Pre\-comprobaciones de requisitos previos** , comprueba que se han realizado correctamente todas las comprobaciones de requisitos previos y, a continuación, haga clic en **configurar**.  
  
9. En el **resultados** , revise los resultados y comprueba que la configuración se ha completado correctamente y, a continuación, haga clic en **pasos necesarios para completar la implementación del servicio de federación**. Para obtener más información, consulte [pasos siguientes para completar la instalación de AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Haga clic en **Cerrar** para salir del asistente.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Para agregar un servidor de federación a una granja de servidores de federación existente a través de Windows PowerShell  
Puede agregar un servidor de federación a una granja existente mediante el uso de una cuenta de gMSA existente o una cuenta de usuario de dominio existente.  
  
-   Si desea unir un servidor de federación a una granja de servidores mediante el uso de una cuenta de gMSA existente, realice lo siguiente:  
  
    1.  En el equipo que desea configurar como un servidor de federación, asegúrese de que se ha importado el certificado SSL requerido en el **ordenador\\My Store** directory. Puede comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de comandos de Windows PowerShell: `dir Cert:\LocalMachine\My`. Aparece el certificado mediante su huella digital en el **ordenador\\My Store** directory.  
  
    2.  En el equipo que desea configurar como un servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` es el dominio de AD y el nombre de la cuenta de gMSA en ese dominio. `<first_federation_server_hostname>` es el nombre de host del servidor de federación principal de esta granja de servidores existente.  
  
        Puede obtener el valor de `<certificate_thumbprint>` ejecutando `dir Cert:\LocalMachine\My` en el paso anterior.  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecute este comando, agregue el `OverwriteConfiguration` parámetro.  
  
        > [!NOTE]  
        > El comando anterior crea un nodo de granja WID. Si desea crear un nodo de granja de servidores de los equipos que ejecutan SQL Server, debe tener la instancia de SQL Server ya instalada y operativa.  
        >   
        > Puede usar el siguiente comando para agregar un servidor de federación a una granja existente que está usando una instancia de SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` donde **SQL\_Host\_nombre** es el nombre del servidor en el que está SQL Server está ejecutando, y **SQL\_instancia\_nombre** es el nombre de la instancia de SQL Server. Si usa la instancia predeterminada de SQL Server, use un **SQLConnectionString** valor de "**origen de datos\=< SQL\_Host\_nombre >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012 y SQL Server 2014.  
  
-   Si desea unir un servidor de federación a una granja de servidores mediante el uso de una cuenta de usuario de dominio existente, realice lo siguiente:  
  
    1.  En el equipo que desea configurar como un servidor de federación, abra la ventana de Windows PowerShellcommand y, a continuación, ejecute el siguiente comando: `$fscred = get-credential`. Escriba las credenciales de cuenta de usuario de dominio que desea usar para la cuenta de servicio de federación en el dominio de formato\\nombre de usuario.  
  
    2.  En el equipo que desea configurar como un servidor de federación, asegúrese de que se ha importado el certificado SSL requerido en el **ordenador\\My Store** directory. Puede comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de Windows PowerShellcommand: `dir Cert:\LocalMachine\My`. Aparece el certificado mediante su huella digital en el **ordenador\\My Store** directory.  
  
    3.  En la misma ventana de comandos de Windows PowerShell, ejecute el siguiente comando.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecute este comando, agregue el `OverwriteConfiguration` parámetro.  
  
        > [!NOTE]  
        > El comando anterior crea un nodo de granja WID. Si desea crear un nodo de granja de servidores de los equipos que ejecutan SQL Server, debe tener la instancia de SQL Server ya instalada y operativa. Puede usar el siguiente comando para agregar un servidor de federación a una granja existente mediante una instancia de SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` donde **SQL\_Host\_nombre** es el nombre del servidor en el que la instancia de SQL Se está ejecutando el servidor, y **SQL\_instancia\_nombre** es el nombre de la instancia de SQL Server. Si usa la instancia predeterminada de SQL Server, use un **SQLConnectionString** valor de "**origen de datos\=< SQL\_Host\_nombre >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012 y SQL Server 2014.  
  
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

