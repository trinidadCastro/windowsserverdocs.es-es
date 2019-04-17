---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: "Guía de implementación de Windows Server 2012 R2 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-federation-server"></a>Configurar un servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Después de instalar el servicio de rol de servicios de federación de Active Directory \(AD FS\) en el equipo, estás listo para configurar el equipo para convertirse en un servidor de federación. Puedes realizar una de las siguientes acciones:  
  
-   [Configurar el primer servidor de federación en un nuevo conjunto de servidor de federación](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Agregar un servidor de federación a un conjunto de servidor de federación existente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurar el primer servidor de federación en un nuevo conjunto de servidor de federación  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Para configurar el primer servidor de federación en un nuevo conjunto de servidor de federación mediante el Asistente para la configuración de los servicios de federación de Active Directory  
  
> [!NOTE]  
> Asegúrate de que tiene los permisos de administrador de dominio o credenciales de administrador de dominio disponibles antes de realizar este procedimiento.  
  
1.  En el administrador del servidor **panel** página, haz clic en el **notificaciones** marcar y, a continuación, haz clic en **configurar el servicio de federación en el servidor**.  
  
    La **Asistente para la configuración a servicios de Active Directory federación** se abre.  
  
2.  En la **bienvenida** página, seleccione **crea el primer servidor de federación en una granja de servidores de federación**y, a continuación, haz clic en **siguiente**.  
  
3.  En la **conectarse a AD DS** página, especificar una cuenta con permisos de administrador de dominio para el dominio de Active Directory \(AD\) a la que está unido a este equipo y, a continuación, haz clic en **siguiente**.  
  
4.  En la **especificar propiedades del servicio** página, haz lo siguiente y, a continuación, haz clic en **siguiente**:  
  
    -   Importar el archivo pfx que contenga el certificado de capa de sockets seguros \(SSL\) y la clave que has obtenido anteriormente. En [paso 2: inscribir un certificado SSL de AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), ha obtenido este certificado y lo has copiado en el equipo que quieres configurar como un servidor de federación. Para importar el archivo .pfx mediante el asistente, haz clic en **importar**y, a continuación, busca la ubicación del archivo. Cuando se te pida, escribe la contraseña para el archivo pfx.  
  
    -   Proporciona un nombre para el servicio de federación. Por ejemplo, **fs.contoso.com**. Este nombre debe coincidir con uno de los asunto o nombres alternativos de firmante del certificado.  
  
    -   Proporcionar un nombre para el servicio de federación. Por ejemplo, **Contoso Corporation**. Los usuarios verán este nombre en el \(AD FS\) sign\ los servicios de federación de Active Directory-en la página.  
  
5.  En la **especificar la cuenta de servicio** página, especifica una cuenta de servicio. Puede crear o usar una existente grupo cuenta de servicio administrada \(gMSA\) o utilizar una cuenta de usuario de dominio existente. Si seleccionas la opción de crear una nueva cuenta gMSA, especifica un nombre para la nueva cuenta. Si seleccionas la opción de usar una cuenta de dominio o gMSA existente, haz clic en **selecciona** para seleccionar una cuenta.  
  
    > [!NOTE]  
    > La ventaja de usar una cuenta de gMSA es su característica de actualización de contraseña negocia auto\.  
  
    > [!WARNING]  
    > Si quieres usar una cuenta de gMSA, debe tener al menos un controlador de dominio en el entorno que está ejecutando el sistema operativo Windows Server 2012.  
    >   
    > Si se deshabilita la opción gMSA y ves un mensaje de error **cuentas de servicio administradas de grupo no están disponibles porque no se estableció la clave raíz de KDS**, puedes habilitar gMSA en tu dominio ejecutando el siguiente comando de Windows PowerShell en un controlador de dominio, que se ejecuta Windows Server 2012 o posterior, en el dominio de Active Directory:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. A continuación, vuelva al asistente, haz clic en **anterior**y, a continuación, haz clic en **siguiente** a re\-escribe la **especificar la cuenta de servicio** página. La opción gMSA ahora debe habilitarse. Puedes seleccionarla y escribe un nombre de cuenta gMSA que quieras usar.  
  
6.  En la **especificar base de datos de configuración** página, especificar una base de datos de configuración de AD FS y, a continuación, haz clic en **siguiente**. Puede crear una base de datos en este equipo mediante el uso de Windows Internal Database \(WID\), o puedes especificar la ubicación y el nombre de la instancia de Microsoft SQL Server.  
  
    Para obtener más información, consulta [el rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluidos SQL Server 2012 y SQL Server 2014.  
  
7.  En la **opciones de revisión** página, compruebe las selecciones de configuración y, a continuación, haz clic en **siguiente**.  
  
8.  En la **Pre\ requisito comprobaciones** página, comprueba que todas las comprobaciones de requisitos previos se ha completado correctamente y, a continuación, haz clic en **configurar**.  
  
9. En la **resultados** página, revisar los resultados y si la configuración se realizó correctamente y, a continuación, haz clic en **siguiente pasos necesarios para completar la implementación de servicios de federación**. Para obtener más información, consulta [pasos siguientes para completar la instalación de AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Haz clic en **cerrar** para salir del asistente.  
  
### <a name="BKMK_3"></a>Para configurar el primer servidor de federación en un nuevo conjunto de servidor de federación mediante Windows PowerShell  
Puedes crear un nuevo conjunto de servidor de federación mediante una cuenta nueva o existente gMSA o una cuenta de usuario de dominio existente.  
  
-   **Si quieres crear un servidor de federación de nuevo con una nueva cuenta gMSA, haz lo siguiente:**  
  
    > [!IMPORTANT]  
    > Debes tener permisos de administrador de dominio para crear el primer servidor de federación en un nuevo conjunto de servidor de federación.  
  
    1.  En el equipo que quieres configurar como un servidor de federación, asegúrate de que se ha importado el certificado SSL necesario en el **almacén Local de PC\\Mis** directorio. Puedes comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de comandos de Windows PowerShell:`dir Cert:\LocalMachine\My`. Aparece el certificado de su huella digital en la **almacén Local de PC\\Mis** directorio.  
  
    2.  En el controlador de dominio, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando para comprobar si se ha creado la clave raíz de KDS en tu dominio:`Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Si no se ha creado para que el resultado muestra ninguna información, ejecuta el siguiente comando para crear la clave:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  En el equipo que quieres configurar como un servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > La `$`inicio de sesión al final del comando anterior, es necesario.  
  
        Para obtener el valor de `<certificate_thumbprint>`, ejecuta `dir Cert:\LocalMachine\My`y, a continuación, selecciona la huella digital del certificado SSL. El valor de `<federation_service_name>`es el nombre de los servicios de federación de, por ejemplo, **fs.contoso.com**.  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecutes este comando, agregar el `OverwriteConfiguration`parámetro.  
  
        > [!NOTE]  
        > El comando anterior crea una granja de servidores WID. Si quieres crear una granja de servidores de SQL Server, debes tener una instancia de SQL Server ya instalado y en funcionamiento.  
        >   
        > Puedes usar el siguiente comando para crear el primer servidor de federación en un nuevo conjunto que use una instancia de SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`donde **< SQL\_Host\_Name >** es el nombre del servidor en el que se ejecuta SQL Server, y **< SQL\_instance\_name >** es el nombre de la instancia de SQL Server. Si usas la instancia predeterminada de SQL Server, usa un **SessionState** valor de "**datos Source\ = < SQL\_Host\_Name >; Security\ integrado = True**".  
  
        > [!IMPORTANT]  
        > Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluido SQL Server 2012.  
  
-   **Si quieres crear un servidor de federación de nuevo con una cuenta de usuario de dominio existente, haz lo siguiente:**  
  
    1.  En el equipo que quieres configurar como un servidor de federación, asegúrate de que se ha importado el certificado SSL necesario en el **almacén Local de PC\\Mis** directorio. Puedes comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de comandos de Windows PowerShell:`dir Cert:\LocalMachine\My`. Aparece el certificado de su huella digital en la **almacén Local de PC\\Mis** directorio.  
  
    2.  En el equipo que quieres configurar como un servidor de federación, abre la ventana de comandos de Windows PowerShell y, a continuación, ejecuta el siguiente comando:`$fscred = Get-Credential`. Escribe las credenciales de cuenta de usuario de dominio que quieras usar para la cuenta de servicio de federación en el nombre del formato dominio\\usuario.  
  
    3.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Para obtener el valor de **< certificate\_thumbprint >**, ejecuta `dir Cert:\LocalMachine\My`y, a continuación, selecciona la huella digital del certificado SSL. El valor de **< federation\_service\_name >** es el nombre de los servicios de federación de, por ejemplo, fs.contoso.com.  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecutes este comando, agregar el `OverwriteConfiguration`parámetro.  
  
        > [!NOTE]  
        > El comando anterior crea una granja de servidores WID. Si quieres crear una granja de SQL Server, debes tener la instancia de SQL Server ya instalado y en funcionamiento.  
        >   
        > Puedes usar el siguiente comando para crear el primer servidor de federación en un nuevo conjunto que use una instancia de SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`donde **SQL\_Host\_Name** es el nombre del servidor en el que se ejecuta SQL Server, y **SQL\_instance\_name** es el nombre de la instancia de SQL Server. Si usas la instancia predeterminada de SQL Server, usa un **SessionState** valor de "**datos Source\ = < SQL\_Host\_Name >; Security\ integrado = True**".  
  
        > [!IMPORTANT]  
        > Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluidos SQL Server 2012 y SQL Server 2014.  
  
## <a name="BKMK_2"></a>Agregar un servidor de federación a un conjunto de servidor de federación existente  
  
> [!IMPORTANT]  
> Asegúrate de que has completado [paso 3: instalar el servicio de rol de AD FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), antes de empezar a cualquiera de los procedimientos descritos en esta sección.  
  
> [!IMPORTANT]  
> Asegúrate de que has obtenido a un servidor SSL válido certificado de autenticación para completar este procedimiento.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Para agregar un servidor de federación a un conjunto de servidor de federación existente mediante el Asistente para la configuración de los servicios de federación de Active Directory  
  
1.  En el administrador del servidor **panel** página, haz clic en el **notificaciones** marcar y, a continuación, haz clic en **configurar el servicio de federación en el servidor**.  
  
    La **Asistente para la configuración a servicios de Active Directory federación** se abre.  
  
2.  En la **bienvenida** página, seleccione **agregar un servidor de federación a una granja de servidores de federación**y, a continuación, haz clic en **siguiente**.  
  
3.  En la **conectarse a AD DS** página, especificar una cuenta con permisos de administrador de dominio para el dominio de AD a la que está unido a este equipo y, a continuación, haz clic en **siguiente**.  
  
4.  En la **especificar granja** página y proporcionar el nombre del servidor de federación principal de un conjunto que utiliza WID o especificar el nombre de host de la base de datos y el nombre de la instancia de base de datos de un conjunto de servidor de federación existente que usa SQL Server.  
  
    > [!WARNING]  
    > En Windows Server® 2012 R2, no hay una solución alternativa para especificar la instancia predeterminada de SQL Server. La solución consiste en no usar la interfaz de usuario. En su lugar, usa los pasos de [para configurar el primer servidor de federación en un nuevo conjunto de servidor de federación mediante Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluido SQL Server 2012.  
  
5.  En la **especificar el certificado SSL** página, importar el archivo pfx que contenga el certificado SSL y la clave que has obtenido anteriormente. Este certificado es el certificado de autenticación del servicio requerido. En [paso 2: inscribir un certificado SSL de AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), ha obtenido este certificado y lo has copiado en el equipo que quieres configurar como un servidor de federación. Para importar el archivo .pfx mediante el asistente, haz clic en **importar** y busca la ubicación del archivo. Cuando se te pida, escribe la contraseña para el archivo pfx.  
  
6.  En la **especificar la cuenta de servicio** página, especifica la misma cuenta de servicio que configuraste cuando crea el primer servidor de federación de la batería. Puedes usar un grupo existente administrado cuenta de servicio o una cuenta de usuario de dominio existente.  
  
    > [!IMPORTANT]  
    > La cuenta que especifiques debe ser la misma cuenta que la cuenta que se usó en el servidor de federación principal de servidores.  
  
7.  En la **opciones de revisión** página, compruebe las selecciones de configuración y, a continuación, haz clic en **siguiente**.  
  
8.  En la **Pre\ requisito comprobaciones** página, comprueba que todas las comprobaciones de requisitos previos se ha completado correctamente y, a continuación, haz clic en **configurar**.  
  
9. En la **resultados** página, revisar los resultados y si la configuración se realizó correctamente y, a continuación, haz clic en **siguiente pasos necesarios para completar la implementación de servicios de federación**. Para obtener más información, consulta [pasos siguientes para completar la instalación de AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Haz clic en **cerrar** para salir del asistente.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Para agregar un servidor de federación a un conjunto de servidor de federación existente mediante Windows PowerShell  
Puedes agregar un servidor de federación a un conjunto existente mediante una cuenta de gMSA existente o una cuenta de usuario de dominio existente.  
  
-   Si quieres unirte a un servidor de federación a un conjunto de con una cuenta de gMSA existente, haz lo siguiente:  
  
    1.  En el equipo que quieres configurar como un servidor de federación, asegúrate de que se ha importado el certificado SSL necesario en el **almacén Local de PC\\Mis** directorio. Puedes comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de comandos de Windows PowerShell:`dir Cert:\LocalMachine\My`. Aparece el certificado de su huella digital en la **almacén Local de PC\\Mis** directorio.  
  
    2.  En el equipo que quieres configurar como un servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` Es tu dominio de Active Directory y el nombre de tu cuenta gMSA en ese dominio. `<first_federation_server_hostname>` Es el nombre de host del servidor de federación principal de este conjunto existente.  
  
        Puedes obtener el valor de `<certificate_thumbprint>`ejecutando `dir Cert:\LocalMachine\My`en el paso anterior.  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecutes este comando, agregar el `OverwriteConfiguration`parámetro.  
  
        > [!NOTE]  
        > El comando anterior, crea un nodo de granja de servidores WID. Si quieres crear un nodo de granja de servidores de equipos que ejecutan SQL Server, debes tener la instancia de SQL Server ya instalado y en funcionamiento.  
        >   
        > Puedes usar el siguiente comando para agregar un servidor de federación a un conjunto existente que está usando una instancia de SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`donde **SQL\_Host\_Name** es el nombre del servidor en el que se ejecuta SQL Server, y **SQL\_instance\_name** es el nombre de la instancia de SQL Server. Si usas la instancia predeterminada de SQL Server, usa un **SessionState** valor de "**datos Source\ = < SQL\_Host\_Name >; Security\ integrado = True**".  
  
        > [!IMPORTANT]  
        > Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluidos SQL Server 2012 y SQL Server 2014.  
  
-   Si quieres unirte a un servidor de federación a un conjunto de mediante el uso de una cuenta de usuario de dominio existente, haz lo siguiente:  
  
    1.  En el equipo que quieres configurar como un servidor de federación, abre la ventana PowerShellcommand de Windows y, a continuación, ejecuta el siguiente comando:`$fscred = get-credential`. Escribe las credenciales de cuenta de usuario de dominio que quieras usar para la cuenta de servicio de federación en el nombre del formato dominio\\usuario.  
  
    2.  En el equipo que quieres configurar como un servidor de federación, asegúrate de que se ha importado el certificado SSL necesario en el **almacén Local de PC\\Mis** directorio. Puedes comprobar si se ha importado el certificado SSL ejecutando el comando siguiente en la ventana de Windows PowerShellcommand:`dir Cert:\LocalMachine\My`. Aparece el certificado de su huella digital en la **almacén Local de PC\\Mis** directorio.  
  
    3.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Si esto no es la primera vez que ejecutes este comando, agregar el `OverwriteConfiguration`parámetro.  
  
        > [!NOTE]  
        > El comando anterior, crea un nodo de granja de servidores WID. Si quieres crear un nodo de granja de servidores de equipos que ejecutan SQL Server, debes tener la instancia de SQL Server ya instalado y en funcionamiento. Puedes usar el siguiente comando para agregar un servidor de federación a un conjunto existente mediante una instancia de SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`donde **SQL\_Host\_Name** es el nombre del servidor en el que se ejecuta la instancia de SQL Server, y **SQL\_instance\_name** es el nombre de la instancia de SQL Server. Si usas la instancia predeterminada de SQL Server, usa un **SessionState** valor de "**datos Source\ = < SQL\_Host\_Name >; Security\ integrado = True**".  
  
        > [!IMPORTANT]  
        > Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluidos SQL Server 2012 y SQL Server 2014.  
  
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar un conjunto de servidor de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

