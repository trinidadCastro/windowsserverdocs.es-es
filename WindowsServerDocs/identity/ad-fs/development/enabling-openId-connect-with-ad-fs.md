---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Compilar una aplicación web con OpenID Connect con AD FS 2016 y posterior
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbd42941f8952fc7f649636d2f3645f941240d49
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190416"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>Compilar una aplicación web con OpenID Connect con AD FS 2016 y posterior

## <a name="pre-requisites"></a>Requisitos previos  
La siguiente es una lista de requisitos previos necesarios antes de completar este documento. Este documento se supone que se ha instalado AD FS y se ha creado una granja de AD FS.  

-   Herramientas de cliente de GitHub  

-   AD FS en Windows Server 2016 TP4 o posterior  

-   Visual Studio 2013 o posterior.  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>Crear un grupo de aplicaciones en AD FS 2016 y versiones posteriores
La siguiente sección describe cómo configurar la aplicación en el grupo de AD FS 2016 y versiones posterior.  

#### <a name="create-application-group"></a>Crear grupo de aplicaciones  

1.  Administración de AD FS, haga doble clic en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.  

2.  En el Asistente para grupo de aplicaciones, como nombre escriba **ADFSSSO** y en **aplicaciones cliente / servidor** seleccione la **explorador Web acceso a una aplicación web** plantilla.  Haz clic en **Siguiente**.

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  Copia el **identificador de cliente** valor.  Se usará más adelante como el valor de ida: ClientId en el archivo web.config de aplicaciones.  

4.  Escriba lo siguiente para **URI de redirección:**  -  **https://localhost:44320/** .  Haz clic en **Agregar**. Haz clic en **Siguiente**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  En el **resumen** pantalla, haga clic en **siguiente**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  En el **completar** pantalla, haga clic en **cerrar**.  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>Descargue y modifique la aplicación de ejemplo para autenticarse mediante OpenID Connect y AD FS  
En esta sección se explica cómo descargar el ejemplo de aplicación Web y modificarla en Visual Studio.   Vamos a usar el ejemplo de Azure AD que se [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Para descargar el proyecto de ejemplo, use Git Bash y escriba lo siguiente:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>Para modificar la aplicación  

1.  Abra el ejemplo mediante Visual Studio.  

2.  Vuelva a compilar la aplicación para que se restauran todos los paquetes de NuGet que faltan.  

3.  Abra el archivo web.config.  Modifique los valores siguientes para el aspecto siguiente:  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  Abra el archivo Startup.Auth.cs y realice los cambios siguientes:  

    -   Comente la siguiente:  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Ajustar la lógica de inicialización de middleware de OpenId Connect con los cambios siguientes:  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   Más abajo, modificar las opciones de middleware de OpenId Connect como se muestra en la siguiente:  

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

        Cambiando el anterior que estamos realizando lo siguiente:  

        -   En lugar de la autoridad para comunicar datos sobre el emisor de confianza, se especifique la ubicación de documento de descubrimiento directamente a través de MetadataAddress  

        -   Azure AD no exige la presencia de un URI de redirección en la solicitud, pero sí de ADFS. Por lo tanto, es necesario agregarla aquí  

## <a name="verify-the-app-is-working"></a>Compruebe que la aplicación funciona  
Una vez realizados los cambios anteriores, presione la tecla F5.  Se abrirá la página de ejemplo.  Haga clic en Inicio de sesión.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

Se redirige a la página de inicio de sesión de AD FS.  Continúe e inicie sesión.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

Cuando esto se realice correctamente, verá que ahora ha iniciado sesión.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
