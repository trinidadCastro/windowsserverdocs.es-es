---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Compilar una aplicación del lado servidor con clientes confidenciales de OAuth con AD FS 2016 o posterior
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5b2bf036de1de8300e36c3413c551e51d408a4d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407869"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016-or-later"></a>Compilar una aplicación del lado servidor con clientes confidenciales de OAuth con AD FS 2016 o posterior


AD FS 2016 y versiones posteriores proporcionan compatibilidad con clientes que pueden mantener su propio secreto, como una aplicación o un servicio que se ejecuta en un servidor Web.  Estos clientes se conocen como clientes confidenciales.
A continuación se muestra un esquema de una aplicación web que se ejecuta en un servidor Web y que actúa como cliente confidencial para AD FS:  

## <a name="pre-requisites"></a>Requisitos previos  
A continuación se muestra una lista de los requisitos previos necesarios antes de completar este documento. En este documento se supone que se ha instalado AD FS.  

-   Herramientas de cliente de GitHub  

-   AD FS en Windows Server 2016 TP4 o posterior  

-   Visual Studio 2013 o posterior.  

## <a name="create-an-application-group-in-ad-fs-2016-or-later"></a>Crear un grupo de aplicaciones en AD FS 2016 o posterior
En la siguiente sección se describe cómo configurar el grupo de aplicaciones en AD FS 2016 o posterior.  

#### <a name="create-the-application-group"></a>Crear el grupo de aplicaciones  

1.  En administración de AD FS, haga clic con el botón derecho en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.  

2.  En el Asistente para grupos de aplicaciones, en **nombre** , escriba **ADFSOAUTHCC** y en **aplicaciones cliente-servidor** seleccione la plantilla **aplicación de servidor que tiene acceso a una API Web** .  Haz clic en **Siguiente**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  

3.  Copie el valor del **identificador de cliente** .  Se usará más adelante como valor de **ida: ClientID** en el archivo Web. config de aplicaciones.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  

4.  Escriba lo siguiente para el **URI de redirección:**  -  **https://localhost:44323** .  Haz clic en **Agregar**. Haz clic en **Siguiente**.  

5.  En la pantalla **configurar credenciales de aplicación** , active la casilla **generar un secreto compartido** y copie el secreto.  Se usará más adelante como valor de **ida: ClientSecret** en el archivo Web. config de aplicaciones.  Haz clic en **Siguiente**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)   

6. En la pantalla **configurar API Web** , escriba lo siguiente para -  **https://contoso.com/WebApp** Identifier.  Haz clic en **Agregar**. Haz clic en **Siguiente**.  Este valor se usará más adelante para **ida: GraphResourceId** en el archivo Web. config de las aplicaciones.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  

7. En la pantalla **aplicar Directiva de Access Control** , seleccione **permitir todos** y haga clic en **siguiente**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  

8. En la pantalla **configurar permisos de aplicación** , asegúrese de que **OpenID** y **user_impersonation** están seleccionados y haga clic en **siguiente**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  

9. En la pantalla **Resumen** , haga clic en **siguiente**.  

10. En la pantalla **completa** , haga clic en **cerrar**.  

## <a name="upgrade-the-database"></a>Actualizar la base de datos  
Visual Studio 2015 se usó en la creación de este tutorial.   Para obtener el ejemplo que funciona con Visual Studio 2015, deberá actualizar el archivo de base de datos.  Utiliza el siguiente procedimiento para hacerlo.  

En esta sección se describe cómo descargar la API Web de ejemplo y cómo actualizar la base de datos en Visual Studio 2015.   Vamos a usar el ejemplo Azure AD que se encuentra [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  

Para descargar el proyecto de ejemplo, use git bash y escriba lo siguiente:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  

![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  

#### <a name="to-upgrade-the-database-file"></a>Para actualizar el archivo de base de datos  

1.  Abra el proyecto en Visual Studio, aparecerá un elemento emergente en el que se indica que la aplicación requiere SQL Server Express 2012 o bien, deberá actualizar la base de datos.  Haga clic en Aceptar.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  

2.  Después, compile la aplicación seleccionando compilar > compilar solución en la parte superior.  Se restaurarán todos los paquetes de NuGet.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  

3.  Ahora, en la parte superior, seleccione **ver** -> **Explorador de servidores**.  Una vez que se abra, en **conexiones de datos**, haga clic con el botón secundario en **DefaultConnection** y seleccione **modificar conexión**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  

4.  En **modificar conexión**, en **nombre de archivo de base de datos (nuevo o existente)** , seleccione **examinar** y proporcione **path\filename.MDF**. Haga clic en **sí** en el cuadro de diálogo.

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)

5.  En **modificar conexión**, seleccione **avanzadas**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  

6.  En las propiedades avanzadas, busque origen de datos y use la lista desplegable para cambiarlo de **(LocalDb\v11.0)** a **(LocalDb) \MSSQLLocalDB**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  

7.  Haga clic en Aceptar. Haga clic en Aceptar.  Haga clic en sí para actualizar la base de datos.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  

8.  Cuando se complete, copie el valor en el cuadro situado junto a **cadena de conexión.**  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  

9.  Ahora, abra el archivo Web. config y reemplace el valor que se encuentra en connectionString por el valor que copió anteriormente.  Guarde el archivo Web. config.  

    > [!NOTE]  
    > Los pasos anteriores son necesarios para que podamos obtener la nueva connectionString.  De lo contrario, cuando ejecute UPDATE-Database debajo, se producirá un error.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  

10. En la parte superior de Visual Studio, seleccione **ver** ->  otra**consola del administrador de paquetes**de**Windows** -> .  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  

11. En la parte inferior, en la consola del administrador de paquetes, escriba: `Enable-Migrations` y presione Entrar.  

    > [!NOTE]  
    > Si recibe un error que indica que enable-Migrations no se reconoce como cmdlet, escriba Install-Package EntityFramework para actualizar el EntityFramework.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  

12. En la parte inferior, en la consola del administrador de paquetes, escriba: `Add-Migration <anynamehere>` y presione Entrar.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  

13. En la parte inferior, en la consola del administrador de paquetes, escriba: `Update-Database` y presione Entrar.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  

## <a name="modify-the-webapi-in-visual-studio"></a>Modificación de WebApi en Visual Studio  

#### <a name="to-modify-the-sample-web-api"></a>Para modificar la API Web de ejemplo  

1.  Abra el ejemplo con Visual Studio.  

2.  Abra el archivo Web. config.  Modifique los valores siguientes:  

    -   ida: ClientId: escriba el valor de #3 en la sección crear el grupo de aplicaciones anterior.  

    -   ida: ClientSecret: escriba el valor de #5 en la sección crear el grupo de aplicaciones anterior.  

    -   ida: GraphResourceId: escriba el valor de #6 en la sección creación del grupo de aplicaciones anterior.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  

3.  Abra el archivo Startup.Auth.cs en App_Start y realice los cambios siguientes:  

    -   Comente las siguientes líneas:  

        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Agregue lo siguiente en su lugar:  

        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  

        donde < > your_fsname se reemplaza por la parte de DNS de la dirección URL del servicio de Federación, por ejemplo, adfs.contoso.com  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  

4.  Abra el archivo UserProfileController.cs y realice los cambios siguientes:  

    -   Comente lo siguiente:  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  

    -   Reemplace ambas apariciones por lo siguiente:  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  

    -   Comente lo siguiente:  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  

    -   Reemplace ambas apariciones por lo siguiente:  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  

    -   Ahora, comente todas las instancias de lo siguiente:  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString() + "/OAuth");  
        ```  

    -   Reemplace todas las apariciones por lo siguiente:  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString());  
        ```  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  

## <a name="test-the-solution"></a>Prueba de la solución  
En esta sección se probará la solución de cliente confidencial.  Use el procedimiento siguiente para probar la solución.  

#### <a name="testing-the-confidential-client-solution"></a>Probar la solución de cliente confidencial  

1. En la parte superior de Visual Studio, asegúrese de que Internet Explorer está seleccionado y haga clic en la flecha verde.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  

2. Cuando aparezca la página ASP.Net, haga clic en **registrar** en la parte superior derecha de la página.  Escriba un nombre de usuario y una contraseña y haga clic en **el botón registrar** .  Esto crea una cuenta local en la base de datos SQL.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  

3. Observe ahora que el sitio de ASP.NET dice Hello abby@contoso.com!.  Haga clic en **perfil**.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  

4. Se abrirá una página sin información y se indicará que debemos hacer clic aquí para iniciar sesión.  Haga clic **aquí**.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  

5. Ahora se le pedirá que inicie sesión en AD FS.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
