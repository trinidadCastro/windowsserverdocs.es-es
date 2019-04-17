---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: "Crear una aplicación web con OpenID conectarse con AD FS de 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f8040b19576ac9de4ced43e6313cad69276a3d27
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>Crear una aplicación web con OpenID conectarse con AD FS de 2016

>Se aplica a: Windows Server 2016

Ampliar la compatibilidad de Oauth inicial de AD FS en Windows Server 2012 R2, AD FS 2016 incorpora compatibilidad para usar el inicio de sesión en OpenId conectar.  
  
## <a name="pre-requisites"></a>Requisitos previos  
La siguiente es una lista de requisitos previos necesarios antes de completar este documento. Este documento se supone que se ha instalado AD FS y se ha creado un conjunto de AD FS.  
  
-   Una suscripción a Azure AD (una prueba gratuita es correcto)  
  
-   Herramientas de cliente de GitHub  
  
-   AD FS de Windows Server 2016 TP4 o versiones posteriores  
  
-   Visual Studio 2013 o posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Crear un grupo de aplicaciones en AD FS de 2016  
La siguiente sección describe cómo configurar el grupo de aplicaciones en AD FS de 2016.  
  
#### <a name="create-application-group"></a>Crear el grupo de aplicaciones  
  
1.  En la administración de AD FS, haz clic en los grupos de aplicaciones y selecciona **Agregar grupo de aplicaciones**.  
  
2.  En el Asistente de grupo de la aplicación, como el nombre escribe **ADFSSSO** y, en **aplicaciones independientes**selecciona la **aplicación de servidor o el sitio Web** plantilla.  Haz clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  Copia el **identificador de cliente** valor.  Se usará más tarde como el valor de ida: ClientId en el archivo de web.config de aplicaciones.  
  
4.  Escribe lo siguiente para **URI de redireccionamiento:** - **https://localhost:44320/**.  Haz clic en **agregar**. Haz clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  En la **configurar credenciales de la aplicación** de pantalla, coloca una comprobación en **generar un secreto compartido** y copia la clave secreta. Haz clic en **siguiente**  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  En la **resumen** de pantalla, haz clic en **siguiente**.  
  
7.  En la **completado** de pantalla, haz clic en **cerrar**.  
  
8.  Ahora, en el nuevo grupo de aplicación con el botón derecho y selecciona **propiedades**.  
  
9. En la **ADFSSSO propiedades** haga clic en **Agregar aplicación**.  
  
10. En la **agregar una nueva aplicación de la aplicación de ejemplo** selecciona **API Web** y haz clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. En la **configurar Web API** de pantalla, escribe lo siguiente para **identificador** - **https://contoso.com/WebApp**.  Haz clic en **agregar**. Haz clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
12. En la **elegir la directiva de Control de acceso** pantalla, selecciona **permitir que todos** y haz clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. En la **configurar permisos de la aplicación** de pantalla, asegúrate de que **openid** está seleccionado y haz clic en **siguiente**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. En la **resumen** de pantalla, haz clic en **siguiente**.  
  
15. En la **completado** de pantalla, haz clic en **cerrar**.  
  
16. En la **propiedades de la aplicación de muestra** haga clic en **Aceptar**.  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>Descargar y modificar la aplicación de MVP para autenticar a través de OpenId conectarse y AD FS  
En esta sección se describe cómo descargar la muestra de Web API y modificarla en Visual Studio.   Vamos a usar la muestra de Azure AD que se [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  
  
Para descargar el proyecto de ejemplo, usar Git Bash y escribe lo siguiente:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>Para modificar la aplicación  
  
1.  Abre la muestra mediante Visual Studio.  
  
2.  Compila la aplicación para que todos los que faltan NuGets se restauran.  
  
3.  Abre el archivo de web.config.  Modificar los siguientes valores para el aspecto de las siguientes acciones:  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  Abre el archivo Startup.Auth.cs y realiza los siguientes cambios:  
  
    -   Comentar las siguientes acciones:  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
    -   Ajustar la lógica de inicialización de software intermedio OpenId conectar con los siguientes cambios:  
  
        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  
  
        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  
  
    -   Más abajo, modificar las opciones de software intermedio OpenId conectarse al igual que en las siguientes acciones:  
  
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
  
        Cambiando el ejemplo anterior que hacemos lo siguiente:  
  
        -   En lugar de usar la autoridad para comunicar datos sobre el emisor de confianza, especificamos la ubicación de documentos del detección directamente a través de MetadataAddress  
  
        -   Azure AD no obliga a la presencia de un redirect_uri en la solicitud, pero no de ADFS. Por lo tanto, debemos agregar aquí  
  
## <a name="verify-the-app-is-working"></a>Comprueba que la aplicación funciona  
Una vez que se realizaron los cambios mencionados, presionar la tecla F5.  Esto hará que se abra la página de ejemplo.  Haz clic en iniciar sesión.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  
  
Se redirigen a la página de inicio de sesión de AD FS.  Pasar a iniciar sesión.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  
  
Una vez que esto es correcta verás que ahora se registran en.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  
  
## <a name="next-steps"></a>Pasos siguientes
[AD FS desarrollo](../../ad-fs/AD-FS-Development.md)  

