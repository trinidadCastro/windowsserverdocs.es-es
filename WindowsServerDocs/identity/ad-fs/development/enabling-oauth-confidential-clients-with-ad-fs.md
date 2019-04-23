---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Compilar una aplicación de lado de servidor mediante clientes confidenciales de OAuth con AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869586"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>Compilar una aplicación de lado de servidor mediante clientes confidenciales de OAuth con AD FS 2016

>Se aplica a: Windows Server 2016

Basándose en la compatibilidad de Oauth inicial de AD FS en Windows Server 2012 R2, AD FS 2016 introduce la compatibilidad con clientes capaces de mantener su propio secreto, como una aplicación o servicio que se ejecuta en un servidor web.  Estos clientes se conocen como clientes confidenciales.    
A continuación es un esquema de una aplicación web que se ejecutan en un servidor web y que actúa como un cliente confidencial a AD FS:  
  
## <a name="pre-requisites"></a>Requisitos previos  
La siguiente es una lista de requisitos previos necesarios antes de completar este documento. Este documento se supone que se ha instalado AD FS y se ha creado una granja de AD FS.  
  
-   Una suscripción de Azure AD (una evaluación gratuita es correcto)  
  
-   Herramientas de cliente de GitHub  
  
-   AD FS en Windows Server 2016 TP4 o posterior  
  
-   Visual Studio 2013 o posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Crear un grupo de aplicaciones en AD FS 2016  
La siguiente sección describe cómo configurar el grupo de aplicaciones en AD FS 2016.  
  
#### <a name="create-the-application-group"></a>Crear el grupo de aplicaciones  
  
1.  Administración de AD FS, haga doble clic en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.  
  
2.  En el Asistente para grupo de aplicaciones, como nombre escriba **ADFSOAUTHCC** y en **aplicaciones independientes** seleccione la **aplicación de servidor o un sitio Web** plantilla.  Haz clic en **Siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  Copia el **identificador de cliente** valor.  Se usará más adelante como el valor de **ida: ClientId** en el archivo web.config de aplicaciones.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  Escriba lo siguiente para **URI de redirección:** - **https://localhost:44323**.  Haz clic en **Agregar**. Haz clic en **Siguiente**.  
  
5.  En el **configurar credenciales de la aplicación** pantalla, coloque una marca de verificación **generar un secreto compartido**y copie el secreto.  Esto se utilizará más adelante como el valor de **ida: AppKey** en el archivo web.config de aplicaciones.  Haz clic en **Siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  En el **resumen** pantalla, haga clic en **siguiente**.  
  
7.  En el **completar** pantalla, haga clic en **cerrar**.  
  
8.  Ahora, en el botón secundario del nuevo grupo de aplicación y seleccione **propiedades**.  
  
9. En **ADFSOAUTHCC propiedades** haga clic en **Agregar aplicación**.  
  
10. En el **agregar una nueva aplicación a aplicación de ejemplo** seleccione **API Web**y haga clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. En el **configurar Web API** pantalla, escriba lo siguiente para **identificador** - **https://contoso.com/WebApp**.  Haz clic en **Agregar**. Haz clic en **Siguiente**.  Este valor se usará más adelante para **ida: GraphResourceId** en el archivo web.config de aplicaciones.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. En el **elegir directiva de Control de acceso** pantalla, seleccione **permitir todos los usuarios** y haga clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. En el **configurar permisos de la aplicación** pantalla, asegúrese de que**user_impersonation** está seleccionada y haga clic en **siguiente**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. En el **resumen** pantalla, haga clic en **siguiente**.  
  
15. En el **completar** pantalla, haga clic en **cerrar**.  
  
16. En el **ADFSOAUTHCC propiedades** haga clic en **Aceptar**.  
  
## <a name="upgrade-the-database"></a>Actualizar la base de datos  
En la creación de este tutorial, se usó Visual Studio 2015.   Para obtener el ejemplo funciona con Visual Studio 2015 deberá actualizar el archivo de base de datos.  Utiliza el siguiente procedimiento para hacerlo.  
  
En esta sección se explica cómo descargar el ejemplo de API Web y actualizar la base de datos en Visual Studio 2015.   Vamos a usar el ejemplo de Azure AD que se [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  
  
Para descargar el proyecto de ejemplo, use Git Bash y escriba lo siguiente:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>Para actualizar el archivo de base de datos  
  
1.  Abra el proyecto en Visual Studio, habrá un elemento emergente que le indica que la aplicación requiere SQL Server 2102 Express o tendrán que actualizar la base de datos.  Haga clic en Aceptar.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  Compilación siguiente de la aplicación seleccionando las compilación -> compilar solución en la parte superior.  Esto restaurará todos los paquetes de NuGet.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  Ahora, en la parte superior, seleccione **vista** -> **Explorador de servidores**.  Una vez que se abre, en **conexiones de datos**, haga clic en **DefaultConnection** y seleccione **modificar conexión**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  En **modificar conexión**, seleccione **avanzadas**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  En las propiedades avanzadas, busque el origen de datos y se usa la lista desplegable para cambiarlo de **(localdb\v11. 0)** a **MSSQLLocalDB (LoaclDB)**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  Haga clic en Aceptar. Haga clic en Aceptar.  Haga clic en Sí para actualizar la base de datos.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  Cuando se complete, más a la derecha, copie el valor en el cuadro junto a **cadena de conexión.**  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  Ahora, abra el archivo Web.config y reemplace el valor que se encuentra en connectionString con el valor que copió anteriormente.  Guarde el archivo Web.config.  
  
    > [!NOTE]  
    > Los pasos anteriores son necesarios para que podamos obtener la cadena de conexión nuevo.  En caso contrario, cuando se ejecuta Update-Database siguiente generará un error.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. En la parte superior de Visual Studio, seleccione **vista** -> **Other Windows** -> **Package Manager Console**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. En la parte inferior, en la consola de administrador de paquetes ENTRAR: `Enable-Migrations` y presione ENTRAR.  
  
    > [!NOTE]  
    > Si se produce un error que dice Enable-Migrations no se reconoce como un cmdlet, escriba Install-Package EntityFramework para actualizar el Entity Framework.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. En la parte inferior, en la consola de administrador de paquetes ENTRAR: `Add-Migration <anynamehere>` y presione ENTRAR.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. En la parte inferior, en la consola de administrador de paquetes ENTRAR: `Update-Database` y presione ENTRAR.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>Modificar la API Web en Visual Studio  
  
#### <a name="to-modify-the-sample-web-api"></a>Para modificar la API Web de ejemplo  
  
1.  Abra el ejemplo mediante Visual Studio.  
  
2.  Abra el archivo web.config.  Modifique los valores siguientes:  
  
    -   ida: ClientId - escriba el valor de #3 anterior.  
  
    -   ida: AppKey - escriba el valor de #5 anterior.  
  
    -   ida: GraphResourceId - escriba el valor de #11 anterior.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  Abra el archivo Startup.Auth.cs en App_Start y realice los cambios siguientes:  
  
    -   Comente las líneas siguientes:  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   En su lugar, agregue lo siguiente:  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        donde < your_fsname > se reemplaza con la parte del DNS de la url del servicio de federación, por ejemplo, adfs.contoso.com  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  Abra el archivo UserProfileController.cs y realice los cambios siguientes:  
  
    -   Comente la siguiente:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   Reemplazar ambas repeticiones con lo siguiente:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   Comente la siguiente:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   Reemplazar ambas repeticiones con lo siguiente:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   Comente ahora todas las instancias de las siguientes acciones:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   Reemplace todas las repeticiones con lo siguiente:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>Probar la solución  
En esta sección, probará la solución de cliente confidencial.  Utilice el siguiente procedimiento para probar la solución.  
  
#### <a name="testing-the-confidential-client-solution"></a>Probar la solución de cliente confidencial  
  
1.  En la parte superior de Visual Studio, asegúrese de que Internet Explorer está activada y haga clic en la flecha verde.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  Cuando aparezca la página de ASP.Net, haga clic en **registrar**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  Escriba un nombre de usuario y una contraseña y, a continuación, haga clic en **registrar**.  Esto crea una cuenta local en la base de datos SQL.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  Tenga en cuenta ahora, el sitio de ASP.NET dice Hello bsimon!.  Haga clic en **perfil**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  Esto abrirá una página sin información y dice que nos debemos haga clic aquí para iniciar sesión.  Haga clic en **aquí**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  Ahora se le solicitará iniciar sesión en AD FS.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  

