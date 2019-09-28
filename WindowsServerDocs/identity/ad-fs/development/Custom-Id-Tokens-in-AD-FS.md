---
title: Personalización de notificaciones para que se emitan en ID_token al usar OpenID Connect o OAuth con AD FS 2016 o posterior.
description: Información general técnica sobre los conceptos del token de identificador personalizado en AD FS 2016 o posterior.
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 88ae6837872c5a6cf6bb1d8533a0aa14b82ca573
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358911"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personalización de notificaciones para que se emitan en ID_token al usar OpenID Connect o OAuth con AD FS 2016 o posterior.

## <a name="overview"></a>Información general
En este [artículo se](native-client-with-ad-fs.md) muestra cómo compilar una aplicación que usa AD FS para el inicio de sesión de OpenID Connect. Sin embargo, de forma predeterminada solo hay un conjunto fijo de notificaciones disponibles en ID_token. AD FS 2016 y versiones posteriores tienen la capacidad de personalizar ID_token en escenarios de OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>¿Cuándo se usa el token de identificador personalizado?
En algunos escenarios, es posible que la aplicación cliente no tenga un recurso al que intente tener acceso. Por lo tanto, realmente no necesita un token de acceso. En tales casos, la aplicación cliente solo necesita un token de identificador, pero con algunas notificaciones adicionales para ayudar en la funcionalidad.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>¿Cuáles son las restricciones en la obtención de notificaciones personalizadas en el token de identificador?

### <a name="scenario-1"></a>Escenario 1

![impedir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode se establece como form_post
2.  Solo los clientes públicos pueden obtener notificaciones personalizadas en el token de identificador
3.  El identificador del usuario de confianza (identificador de la API Web) debe ser el mismo que el identificador de cliente

### <a name="scenario-2"></a>Escenario 2

![impedir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) instalado en los servidores de AD FS
1.  response_mode se establece como form_post
2.  Tanto los clientes públicos como los confidenciales pueden obtener notificaciones personalizadas en el token de identificador
3.  Asigne el ámbito allatclaims al par cliente-RP.
Puede asignar el ámbito mediante el cmdlet Grant-ADFSApplicationPermission como se indica en el ejemplo siguiente:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Crear y configurar una aplicación de OAuth para administrar notificaciones personalizadas en el token de identificador
Siga los pasos que se indican a continuación para crear y configurar la aplicación en AD FS para recibir el token de identificador con notificaciones personalizadas.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Crear y configurar un grupo de aplicaciones en AD FS 2016 o posterior

1. En administración de AD FS, haga clic con el botón derecho en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.

2. En el Asistente para grupos de aplicaciones, en nombre, escriba **ADFSSSO** y en aplicaciones cliente-servidor seleccione la plantilla **aplicación nativa que tiene acceso a una aplicación web** . Haz clic en **Siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Copie el valor del **identificador de cliente** .  Se usará más adelante como valor de ida: ClientId en el archivo Web. config de aplicaciones.

4. Escriba lo siguiente para el **URI de redirección:**  -  **https://localhost:44320/** .  Haz clic en **Agregar**. Haz clic en **Siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. En la pantalla **configurar API Web** , escriba lo siguiente para -  **https://contoso.com/WebApp** Identifier.  Haz clic en **Agregar**. Haz clic en **Siguiente**.  Este valor se usará más adelante para **ida: ResourceID** en el archivo Web. config de aplicaciones.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. En la pantalla **elegir Directiva de Access Control** , seleccione **permitir todos** y haga clic en **siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. En la pantalla **configurar permisos de aplicación** , asegúrese de que **OpenID** y **allatclaims** están seleccionados y haga clic en **siguiente**.

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. En la pantalla **Resumen** , haga clic en **siguiente**.  

   ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. En la pantalla **completa** , haga clic en **cerrar**.

10. En administración de AD FS, haga clic en grupos de aplicaciones para obtener una lista de todos los grupos de aplicaciones. Haga clic con el botón derecho en **ADFSSSO** y seleccione **propiedades**. Seleccione **ADFSSSO-Web API** y haga clic en **Editar..** .

    ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. En **la pantalla ADFSSSO-propiedades de la API Web** , seleccione la pestaña **reglas de transformación de emisión** y haga clic en **Agregar regla..** .

    ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. En la pantalla **Asistente para agregar regla de notificación de transformación** , seleccione **enviar notificaciones mediante una regla personalizada** en la lista desplegable y haga clic en **siguiente** .

    ![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. En la pantalla **Asistente para agregar regla de notificaciones de transformación** , escriba **ForCustomIDToken** en **nombre de regla de notificaciones** y la siguiente regla de notificaciones en **regla personalizada**. Haga clic en **Finalizar**

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

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>Descargar y modificar la aplicación de ejemplo para emitir notificaciones personalizadas en ID_token

En esta sección se describe cómo descargar la aplicación Web de ejemplo y modificarla en Visual Studio.   Vamos a usar el ejemplo Azure AD que se encuentra [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Para descargar el proyecto de ejemplo, use git bash y escriba lo siguiente:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Para modificar la aplicación

1.  Abra el ejemplo con Visual Studio.  

2.  Vuelva a compilar la aplicación para que se restauren todos los paquetes Nuget que faltan.  

3.  Abra el archivo Web. config.  Modifique los valores siguientes para que tengan el aspecto siguiente:  

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

    -   Retoque la lógica de inicialización del middleware de OpenId Connect con los siguientes cambios:  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   Comente lo siguiente:  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   Más adelante, modifique las opciones de middleware de OpenId Connect como se indican a continuación:  

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

5.  Abra el archivo HomeController.cs y realice los cambios siguientes:  

    -   Agrega lo siguiente:  

            ```  
            using System.Security.Claims;  
            ```

    -   Actualice el método about () como se muestra a continuación:  

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

### <a name="test-the-custom-claims-in-id-token"></a>Prueba de las notificaciones personalizadas en el token de identificador

Una vez realizados los cambios anteriores, presione F5. Se abrirá la página de ejemplo. Haga clic en iniciar sesión.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Se le redirigirá a la página de inicio de sesión de AD FS. Continúe e inicie sesión.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Una vez que esto se realice correctamente, verá que ya ha iniciado sesión.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

Haga clic en el vínculo acerca de. Verá Hola [nombre de usuario] que se recupera de la demanda de nombre de usuario en el token de identificador.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
