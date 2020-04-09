---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Compilación de una aplicación web con OpenID Connect con AD FS 2016 y versiones posteriores
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 49d952a49cf474708f57a0ae2a7760d2470af607
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857498"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>Compilación de una aplicación web con OpenID Connect con AD FS 2016 y versiones posteriores

## <a name="pre-requisites"></a>Requisitos previos  
A continuación se muestra una lista de los requisitos previos necesarios antes de completar este documento. En este documento se supone que se ha instalado AD FS y se ha creado una granja de AD FS.  

-   Herramientas de cliente de GitHub  

-   AD FS en Windows Server 2016 TP4 o posterior  

-   Visual Studio 2013 o posterior.  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>Crear un grupo de aplicaciones en AD FS 2016 y versiones posteriores
En la siguiente sección se describe cómo configurar el grupo de aplicaciones en AD FS 2016 y versiones posteriores.  

#### <a name="create-application-group"></a>Crear grupo de aplicaciones  

1.  En administración de AD FS, haga clic con el botón derecho en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.  

2.  En el Asistente para grupos de aplicaciones, en nombre, escriba **ADFSSSO** y **, en aplicaciones cliente-servidor** , seleccione el **explorador Web que tiene acceso a una plantilla de aplicación web** .  Haga clic en **Siguiente**.

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  Copie el valor del **identificador de cliente** .  Se usará más adelante como valor de ida: ClientId en el archivo Web. config de aplicaciones.  

4.  Escriba lo siguiente en **URI de redirección:**  -  **https://localhost:44320/** .  Haga clic en **Agregar**. Haga clic en **Siguiente**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  En la pantalla **Resumen** , haga clic en **siguiente**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  En la pantalla **completa** , haga clic en **cerrar**.  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>Descargue y modifique la aplicación de ejemplo para autenticarse a través de OpenID Connect y AD FS  
En esta sección se describe cómo descargar la aplicación Web de ejemplo y modificarla en Visual Studio.   Vamos a usar el ejemplo Azure AD que se encuentra [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Para descargar el proyecto de ejemplo, use git bash y escriba lo siguiente:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>Para modificar la aplicación  

1.  Abra el ejemplo con Visual Studio.  

2.  Vuelva a compilar la aplicación para que se restauren todos los paquetes Nuget que faltan.  

3.  Abra el archivo Web. config.  Modifique los valores siguientes para que tengan el aspecto siguiente:  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  Abra el archivo Startup.Auth.cs y realice los cambios siguientes:  

    -   Comente lo siguiente:  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Retoque la lógica de inicialización del middleware de OpenId Connect con los siguientes cambios:  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   Más adelante, modifique las opciones de middleware de OpenId Connect como se indican a continuación:  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  

        Al cambiar lo anterior, haremos lo siguiente:  

        -   En lugar de usar la autoridad para comunicar datos sobre el emisor de confianza, se especifica la ubicación del documento de detección directamente a través de MetadataAddress  

        -   Azure AD no exige la presencia de un redirect_uri en la solicitud, pero ADFS sí lo hace. Por lo tanto, es necesario agregarlo aquí  

## <a name="verify-the-app-is-working"></a>Comprobar que la aplicación funciona  
Una vez realizados los cambios anteriores, presione F5.  Se abrirá la página de ejemplo.  Haga clic en iniciar sesión.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

Se le redirigirá a la página de inicio de sesión de AD FS.  Continúe e inicie sesión.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

Una vez que esto se realice correctamente, verá que ya ha iniciado sesión.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
