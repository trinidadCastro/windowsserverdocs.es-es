---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Crear una aplicación web con OpenID Connect con AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74a493e6568b71a05116140ec67586d36f439aa8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882666"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>Crear una aplicación web con OpenID Connect con AD FS 2016

>Se aplica a: Windows Server 2016

Basándose en la compatibilidad de Oauth inicial de AD FS en Windows Server 2012 R2, AD FS 2016 introduce la compatibilidad para usar el inicio de sesión en OpenId Connect.  
  
## <a name="pre-requisites"></a>Requisitos previos  
La siguiente es una lista de requisitos previos necesarios antes de completar este documento. Este documento se supone que se ha instalado AD FS y se ha creado una granja de AD FS.  
  
-   Herramientas de cliente de GitHub  
  
-   AD FS en Windows Server 2016 TP4 o posterior  
  
-   Visual Studio 2013 o posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Crear un grupo de aplicaciones en AD FS 2016  
La siguiente sección describe cómo configurar el grupo de aplicaciones en AD FS 2016.  
  
#### <a name="create-application-group"></a>Crear grupo de aplicaciones  
  
1.  Administración de AD FS, haga doble clic en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.  
  
2.  En el Asistente para grupo de aplicaciones, como nombre escriba **ADFSSSO** y en **aplicaciones independientes** seleccione la **aplicación de servidor o un sitio Web** plantilla.  Haz clic en **Siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  Copia el **identificador de cliente** valor.  Se usará más adelante como el valor de ida: ClientId en el archivo web.config de aplicaciones.  
  
4.  Escriba lo siguiente para **URI de redirección:** - **https://localhost:44320/**.  Haz clic en **Agregar**. Haz clic en **Siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  En el **configurar credenciales de la aplicación** pantalla, coloque una marca de verificación **generar un secreto compartido** y copie el secreto. Haga clic en **Siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  En el **resumen** pantalla, haga clic en **siguiente**.  
  
7.  En el **completar** pantalla, haga clic en **cerrar**.  
  
8.  Ahora, en el botón secundario del nuevo grupo de aplicación y seleccione **propiedades**.  
  
9. En el **ADFSSSO propiedades** haga clic en **Agregar aplicación**.  
  
10. En el **agregar una nueva aplicación a aplicación de ejemplo** seleccione **API Web** y haga clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. En el **configurar Web API** pantalla, escriba lo siguiente para **identificador** - **https://contoso.com/WebApp**.  Haz clic en **Agregar**. Haz clic en **Siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG) 
    
12. En el **elegir directiva de Control de acceso** pantalla, seleccione **permitir todos los usuarios** y haga clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. En el **configurar permisos de la aplicación** pantalla, asegúrese de que **openid** está seleccionada y haga clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. En el **resumen** pantalla, haga clic en **siguiente**.  
  
15. En el **completar** pantalla, haga clic en **cerrar**.  
  
16. En el **propiedades de la aplicación de ejemplo** haga clic en **Aceptar**.  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>Descargue y modifique la aplicación de MVP para autenticarse mediante OpenId Connect y AD FS  
En esta sección se explica cómo descargar el ejemplo de API Web y modificarla en Visual Studio.   Vamos a usar el ejemplo de Azure AD que se [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  
  
Para descargar el proyecto de ejemplo, use Git Bash y escriba lo siguiente:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>Para modificar la aplicación  
  
1.  Abra el ejemplo mediante Visual Studio.  
  
2.  Compile la aplicación para que se restauran todos los paquetes de NuGet que faltan.  
  
3.  Abra el archivo web.config.  Modifique los valores siguientes para el aspecto siguiente:  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  Abra el archivo Startup.Auth.cs y realice los cambios siguientes:  
  
    -   Comente la siguiente:  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
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
                RedirectUri = postLogoutRedirectUri,  
                PostLogoutRedirectUri = postLogoutRedirectUri 
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
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  

