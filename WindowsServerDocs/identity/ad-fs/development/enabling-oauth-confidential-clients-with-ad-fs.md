---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: "Crear una aplicación de servidor con OAuth confidenciales clientes con AD FS de 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>Crear una aplicación de servidor con OAuth confidenciales clientes con AD FS de 2016

>Se aplica a: Windows Server 2016

Ampliar la compatibilidad de Oauth inicial de AD FS en Windows Server 2012 R2, AD FS 2016 incorpora compatibilidad para los clientes capaces de mantener su propia clave secreta, como una aplicación o un servicio que se ejecuta en un servidor web.  Estos clientes se conocen como los clientes confidenciales.    
A continuación se incluye un esquema de una aplicación web ejecuta en un servidor web y que actúa como un cliente confidencial a AD FS:  
  
## <a name="pre-requisites"></a>Requisitos previos  
La siguiente es una lista de requisitos previos necesarios antes de completar este documento. Este documento se supone que se ha instalado AD FS y se ha creado un conjunto de AD FS.  
  
-   Una suscripción a Azure AD (una prueba gratuita es correcto)  
  
-   Herramientas de cliente de GitHub  
  
-   AD FS de Windows Server 2016 TP4 o versiones posteriores  
  
-   Visual Studio 2013 o posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Crear un grupo de aplicaciones en AD FS de 2016  
La siguiente sección describe cómo configurar el grupo de aplicaciones en AD FS de 2016.  
  
#### <a name="create-the-application-group"></a>Crear el grupo de aplicaciones  
  
1.  En la administración de AD FS, haz clic en los grupos de aplicaciones y selecciona **Agregar grupo de aplicaciones**.  
  
2.  En el Asistente de grupo de la aplicación, como el nombre escribe **ADFSOAUTHCC** y, en **aplicaciones independientes** selecciona la **aplicación de servidor o el sitio Web** plantilla.  Haz clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  Copia el **identificador de cliente** valor.  Se usará más tarde, como el valor de **ida: ClientId** en el archivo de web.config de aplicaciones.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  Escribe lo siguiente para **URI de redireccionamiento:** - **https://localhost:44323**.  Haz clic en **agregar**. Haz clic en **siguiente**.  
  
5.  En la **configurar credenciales de la aplicación** de pantalla, coloca una comprobación en **generar un secreto compartido**y copia la clave secreta.  Esto se usará más tarde, como el valor de **ida: AppKey** en el archivo de web.config de aplicaciones.  Haz clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  En la **resumen** de pantalla, haz clic en **siguiente**.  
  
7.  En la **completado** de pantalla, haz clic en **cerrar**.  
  
8.  Ahora, en el nuevo grupo de aplicación con el botón derecho y selecciona **propiedades**.  
  
9. En **ADFSOAUTHCC propiedades** haga clic en **Agregar aplicación**.  
  
10. En la **agregar una nueva aplicación de la aplicación de ejemplo** selecciona **API Web**y haz clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. En la **configurar Web API** de pantalla, escribe lo siguiente para **identificador** - **https://contoso.com/WebApp**.  Haz clic en **agregar**. Haz clic en **siguiente**.  Este valor se usará más tarde para **ida: GraphResourceId** en el archivo de web.config de aplicaciones.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. En la **elegir la directiva de Control de acceso** pantalla, selecciona **permitir que todos** y haz clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. En la **configurar permisos de la aplicación** de pantalla, asegúrate de que**user_impersonation** está seleccionado y haz clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. En la **resumen** de pantalla, haz clic en **siguiente**.  
  
15. En la **completado** de pantalla, haz clic en **cerrar**.  
  
16. En la **ADFSOAUTHCC propiedades** haga clic en **Aceptar**.  
  
## <a name="upgrade-the-database"></a>Actualizar la base de datos  
Visual Studio 2015 se usó en la creación de este tutorial.   Para obtener el ejemplo funciona con Visual Studio 2015, necesitarás actualizar el archivo de base de datos.  Usa el siguiente procedimiento para hacerlo.  
  
En esta sección se describe cómo descargar la muestra de Web API y actualizar la base de datos en Visual Studio 2015.   Vamos a usar la muestra de Azure AD que se [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  
  
Para descargar el proyecto de ejemplo, usar Git Bash y escribe lo siguiente:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>Para actualizar el archivo de base de datos  
  
1.  Abre el proyecto en Visual Studio, habrá un elemento emergente que indica que la aplicación requiere 2102 de SQL Server Express o tendrás que actualizar la base de datos.  Haz clic en Aceptar.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  Próxima compilación de la aplicación mediante la selección de la compilación -> Generar solución en la parte superior.  Así restaurará todos los paquetes de NuGet.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  Ahora en la parte superior, selecciona **vista** -> **Explorador de servidores**.  Una vez que se abra, en **conexiones de datos**, haz clic en **DefaultConnection** y selecciona **modificar conexión**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  En **modificar conexión**, selecciona **avanzadas**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  En las propiedades avanzadas, busca el origen de datos y usar la lista desplegable para cambiar desde **(LocalDb\v11.0)** a **(LoaclDB) MSSQLLocalDB**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  Haz clic en Aceptar. Haz clic en Aceptar.  Haz clic en Sí para actualizar la base de datos.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  Cuando esto finalice, más a la derecha, copia el valor en el cuadro junto a **cadena de conexión.**  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  Ahora, abre el archivo de Web.config y reemplaza el valor que está en connectionString con el valor que copiaste anteriormente.  Guarda el archivo de Web.config.  
  
    > [!NOTE]  
    > Los pasos anteriores son necesarios para que podemos conseguir connectionString nuevo.  De lo contrario, cuando ejecutamos Update-Database aparecerá un error.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. En la parte superior de Visual Studio, selecciona **vista** -> **otras ventanas** -> **consola del Administrador de paquetes**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. En la parte inferior, en la consola del Administrador de paquetes ENTRAR: `Enable-Migrations`y presione ENTRAR.  
  
    > [!NOTE]  
    > Si recibes un error que dice Enable-Migrations no se reconoce como un cmdlet, escribe Entity Framework Install-Package para actualizar el Entity Framework.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. En la parte inferior, en la consola del Administrador de paquetes ENTRAR: `Add-Migration <anynamehere>`y presione ENTRAR.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. En la parte inferior, en la consola del Administrador de paquetes ENTRAR: `Update-Database`y presione ENTRAR.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>Modificar el WebApi en Visual Studio  
  
#### <a name="to-modify-the-sample-web-api"></a>Para modificar la API Web de muestra  
  
1.  Abre la muestra mediante Visual Studio.  
  
2.  Abre el archivo de web.config.  Modifica los siguientes valores:  
  
    -   ida: ClientId - escribe el valor de #3 anterior.  
  
    -   ida: AppKey - escribe el valor de #5 anterior.  
  
    -   ida: GraphResourceId - escribe el valor de 11 # anterior.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  Abre el archivo Startup.Auth.cs en App_Start y realiza los siguientes cambios:  
  
    -   Comentar las siguientes líneas:  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   En su lugar, agrega las siguientes acciones:  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        Donde < your_fsname > se reemplaza con la porción DNS de la dirección url del servicio de federación en adfs.contoso.com por ejemplo  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  Abre el archivo UserProfileController.cs y realiza los siguientes cambios:  
  
    -   Comentar las siguientes acciones:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   Reemplaza las repeticiones con lo siguiente:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   Comentar las siguientes acciones:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   Reemplaza las repeticiones con lo siguiente:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   Comenta ahora todas las instancias de las siguientes acciones:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   Reemplaza todas las apariciones con lo siguiente:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>Probar la solución  
En esta sección probamos la solución confidenciales del cliente.  Usa el siguiente procedimiento para probar la solución.  
  
#### <a name="testing-the-confidential-client-solution"></a>Probar la solución confidenciales del cliente  
  
1.  En la parte superior de Visual Studio, asegúrate de que Internet Explorer está seleccionado y haz clic en la flecha verde.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  Cuando aparezca la página ASP.Net, haga clic en **registrar**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  Escribe un nombre de usuario y una contraseña y, a continuación, haz clic en **registrar**.  Esto crea una cuenta local en la base de datos SQL.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  Aviso de ahora, el sitio ASP.NET dice Hello bsimon!.  Haz clic en **perfil**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  Esto aparece una página sin ninguna información y dice que nos debemos hacer clic aquí para el inicio de sesión.  Haz clic en **aquí**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  Ahora pedirá que inicia sesión en AD FS.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>Pasos siguientes
[AD FS desarrollo](../../ad-fs/AD-FS-Development.md)  

