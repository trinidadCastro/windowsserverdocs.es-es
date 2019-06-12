---
title: Personalización de notificaciones emitido en id_token al usar OpenID Connect o OAuth con AD FS 2016 o posterior
description: Una introducción técnica de los conceptos de token de identificador personalizado en AD FS 2016 o posterior
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 04573aa13689a0e6744b01a0fbf8b11b622b2706
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445472"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personalización de notificaciones emitido en id_token al usar OpenID Connect o OAuth con AD FS 2016 o posterior

## <a name="overview"></a>Información general
El artículo [aquí](native-client-with-ad-fs.md) muestra cómo crear una aplicación que utiliza AD FS para OpenID Connect inicio de sesión. Sin embargo, de forma predeterminada hay solo un conjunto fijo de notificaciones disponibles en id_token. AD FS 2016 y versiones posteriores tienen la capacidad para personalizar el valor de id_token en escenarios de OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>¿Cuando están identificador personalizado token que se usa?
En determinados escenarios es posible que la aplicación cliente no tiene un recurso que está intentando obtener acceso. Por lo tanto, realmente no necesita un token de acceso. En tales casos, la aplicación cliente necesita básicamente solo un identificador token, pero con algunas notificaciones adicionales para ayudar en la funcionalidad.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>¿Cuáles son las restricciones sobre la obtención de notificaciones personalizadas en el identificador de token?

### <a name="scenario-1"></a>Escenario 1

![restringir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode se establece como form_post
2.  Solo los clientes públicos pueden obtener notificaciones personalizadas en el identificador de token
3.  Identificador del usuario autenticado (identificador de la API Web) debe coincidir con el identificador de cliente

### <a name="scenario-2"></a>Escenario 2

![restringir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) instalados en los servidores de AD FS
1.  response_mode se establece como form_post
2.  Los clientes públicos y confidenciales pueden obtener notificaciones personalizadas en el Id. de token
3.  Asignar allatclaims de ámbito para el cliente: un par RP.
Puede asignar el ámbito mediante el cmdlet Grant ADFSApplicationPermission tal como se indica en el ejemplo siguiente:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Crear y configurar una aplicación de OAuth para controlar las notificaciones personalizadas en el token de identificador
Siga estos pasos para crear y configurar la aplicación en AD FS para recibir el token de identificador con las notificaciones personalizadas.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Crear y configurar un grupo de aplicaciones en AD FS 2016 o posterior

1. Administración de AD FS, haga doble clic en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.

2. En el Asistente para grupo de aplicaciones, como nombre escriba **ADFSSSO** y en el cliente / servidor, seleccione las aplicaciones la **aplicación nativa accediendo a una aplicación web** plantilla. Haz clic en **Siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Copia el **identificador de cliente** valor.  Se usará más adelante como el valor de ida: ClientId en el archivo web.config de aplicaciones.

4. Escriba lo siguiente para **URI de redirección:**  -  **https://localhost:44320/** .  Haz clic en **Agregar**. Haz clic en **Siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. En el **configurar Web API** pantalla, escriba lo siguiente para **identificador** -  **https://contoso.com/WebApp** .  Haz clic en **Agregar**. Haz clic en **Siguiente**.  Este valor se usará más adelante para **ida: ResourceID** en el archivo web.config de aplicaciones.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. En el **elegir directiva de Control de acceso** pantalla, seleccione **permitir todos los usuarios** y haga clic en **siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. En el **configurar permisos de la aplicación** pantalla, asegúrese de que **openid** y **allatclaims** están seleccionadas y haga clic en **siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. En el **resumen** pantalla, haga clic en **siguiente**.  

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. En el **completar** pantalla, haga clic en **cerrar**.

10. Administración de AD FS, haga clic en grupos de aplicaciones para obtener la lista de todos los grupos de aplicaciones. Haga doble clic en **ADFSSSO** y seleccione **propiedades**. Seleccione **ADFSSSO - API Web** y haga clic en **editar...**

    ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. En **ADFSSSO - propiedades de la API Web** pantalla, seleccione **reglas de transformación de emisión** ficha y haga clic en **Agregar regla...**

    ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. En **transformar notificaciones Asistente para agregar regla** pantalla, seleccione **enviar notificaciones mediante regla personalizada** en la lista desplegable y haga clic en **siguiente**

    ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. En **transformar notificaciones Asistente para agregar regla** pantalla, escriba **ForCustomIDToken** en **nombre de la regla de notificación** y la siguiente regla de notificación **regla personalizada**. Haga clic en **finalizar**

    ```  
    x:[]
    => issue(claim=x);  
    ```

    ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.png)

```

>[!NOTE]
>You can also use PowerShell to assign the allatclaims and openid scopes
>``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-idtoken"></a>Descargue y modifique la aplicación de ejemplo para emitir notificaciones personalizadas en id_token

En esta sección se explica cómo descargar el ejemplo de aplicación Web y modificarla en Visual Studio.   Vamos a usar el ejemplo de Azure AD que se [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Para descargar el proyecto de ejemplo, use Git Bash y escriba lo siguiente:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Para modificar la aplicación

1.  Abra el ejemplo mediante Visual Studio.  

2.  Vuelva a compilar la aplicación para que se restauran todos los paquetes de NuGet que faltan.  

3.  Abra el archivo web.config.  Modifique los valores siguientes para el aspecto siguiente:  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />  
    <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />  
    ```  

    ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.PNG)  

4.  Abra el archivo Startup.Auth.cs y realice los cambios siguientes:  

    -   Ajustar la lógica de inicialización de middleware de OpenId Connect con los cambios siguientes:  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   Comente la siguiente:  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   Más abajo, modificar las opciones de middleware de OpenId Connect como se muestra en la siguiente:  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                Resource = resourceId,
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.PNG)

5.  Abra el archivo HomeController.cs y realice los siguientes cambios:  

    -   Agrega lo siguiente:  

            ```  
            using System.Security.Claims;  
            ```

    -   Actualice el método About () como se muestra a continuación:  

        ```  
        [Authorize]
        public ActionResult About()
        {
            ClaimsPrincipal cp = ClaimsPrincipal.Current;
            string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
            ViewBag.Message = String.Format("Hello {0}!", userName);
            return View();
        }
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.PNG)

### <a name="test-the-custom-claims-in-id-token"></a>Probar las notificaciones personalizadas en el token de identificador

Una vez realizados los cambios anteriores, presione la tecla F5. Se abrirá la página de ejemplo. Haga clic en Inicio de sesión.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Se redirige a la página de inicio de sesión de AD FS. Continúe e inicie sesión.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Cuando esto se realice correctamente, verá que ahora ha iniciado sesión.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

Haga clic en acerca de vínculo. Verá Hello [Username] que se recupera de la notificación de nombre de usuario en el token de identificador

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
