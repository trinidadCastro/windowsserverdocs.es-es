---
title: Personalización de notificaciones para que se emitan en id_token al usar OpenID Connect o OAuth con AD FS 2016 o posterior.
description: Información general técnica sobre los conceptos del token de identificador personalizado en AD FS 2016 o posterior.
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 04/29/2020
ms.topic: article
ms.reviewer: anandy
ms.openlocfilehash: bd9ed9d44c18eade653bfea6daa5cf6aa89e06ea
ms.sourcegitcommit: 7674bbe49517bbfe0e2c00160e08240b60329fd9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98603453"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personalización de notificaciones para que se emitan en id_token al usar OpenID Connect o OAuth con AD FS 2016 o posterior.

## <a name="overview"></a>Introducción

En este [artículo se](native-client-with-ad-fs.md) muestra cómo compilar una aplicación que usa AD FS para el inicio de sesión de OpenID Connect. Sin embargo, de forma predeterminada solo hay un conjunto fijo de notificaciones disponibles en el id_token. AD FS 2016 y versiones posteriores tienen la capacidad de personalizar el id_token en escenarios de OpenID Connect.

## <a name="when-are-custom-id-tokens-used"></a>¿Cuándo se usan los tokens de identificador personalizado?

En algunos escenarios, es posible que la aplicación cliente no tenga un recurso al que intente tener acceso. Por lo tanto, realmente no necesita un token de acceso. En tales casos, la aplicación cliente solo necesita un token de identificador, pero con algunas notificaciones adicionales para ayudar en la funcionalidad.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>¿Cuáles son las restricciones en la obtención de notificaciones personalizadas en el token de identificador?

### <a name="scenario-1"></a>Escenario 1

![Captura de pantalla que muestra el escenario 1 que usa el usuario de confianza para que sea igual al cliente D.](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1. `response_mode` se establece como `form_post`
2. Solo los clientes públicos pueden obtener notificaciones personalizadas en el token de identificador
3. El identificador del usuario de confianza (identificador de la API Web) debe ser el mismo que el identificador de cliente

### <a name="scenario-2"></a>Escenario 2

![Captura de pantalla que muestra el escenario 2 que usa el ámbito allatclaims.](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) o una actualización de seguridad posterior instalada en los servidores de AD FS
1. `response_mode` se establece como form_post
2. Tanto los clientes públicos como los confidenciales pueden obtener notificaciones personalizadas en el token de identificador
3. Asigne `allatclaims` el ámbito al par cliente – RP.

Puede asignar el ámbito mediante el `Grant-ADFSApplicationPermission` cmdlet, como se indica en el ejemplo siguiente:

```powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Crear y configurar una aplicación de OAuth para administrar notificaciones personalizadas en el token de identificador

Siga los pasos que se indican a continuación para crear y configurar la aplicación en AD FS para recibir el token de identificador con notificaciones personalizadas.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Crear y configurar un grupo de aplicaciones en AD FS 2016 o posterior

1. En administración de AD FS, haga clic con el botón derecho en grupos de aplicaciones y seleccione **Agregar grupo de aplicaciones**.
2. En el Asistente para grupos de aplicaciones, en nombre, escriba **ADFSSSO** y, en Client-Server aplicaciones, seleccione la **aplicación nativa que tiene acceso a una plantilla de aplicación web** . Haga clic en **Next**.

   ![Captura de pantalla de la Página principal del Asistente para agregar grupo de aplicaciones.](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Copie el valor del **identificador de cliente** .  Se usará más adelante como valor de ida: ClientId en el archivo de web.config de aplicaciones.
4. Escriba lo siguiente para el **URI de redirección:**  -  **https://localhost:44320/** .  Haga clic en **Agregar**. Haga clic en **Next**.

   ![Captura de pantalla de la página aplicación nativa del Asistente para agregar grupos de aplicaciones que muestra el redireccionamiento U R I.](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. En la pantalla **configurar API Web** , escriba lo siguiente para **Identifier**  -  **https://contoso.com/WebApp** .  Haga clic en **Agregar**. Haga clic en **Next**.  Este valor se usará más adelante para **ida: ResourceID** en el archivo de web.config de aplicaciones.

   ![Captura de pantalla de la página configurar API Web del Asistente para agregar grupo de aplicaciones que muestra el identificador correcto.](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. En la pantalla **elegir Directiva de Access Control** , seleccione **permitir todos** y haga clic en **siguiente**.

   ![Captura de pantalla de la página elegir Directiva de Access Control del Asistente para agregar grupo de aplicaciones que muestra la opción permitir todos resaltada.](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. En la pantalla **configurar permisos de aplicación** , asegúrese de que **OpenID** y **allatclaims** están seleccionados y haga clic en **siguiente**.

   ![Captura de pantalla de la página configurar permisos de aplicación del Asistente para agregar grupo de aplicaciones.](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.PNG)

8. En la pantalla **Resumen** , haga clic en **siguiente**.

   ![Captura de pantalla de la página Resumen del Asistente para agregar grupo de aplicaciones.](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.PNG)

9. En la pantalla **completa** , haga clic en **cerrar**.
10. En administración de AD FS, haga clic en grupos de aplicaciones para obtener una lista de todos los grupos de aplicaciones. Haga clic con el botón derecho en **ADFSSSO** y seleccione **propiedades**. Seleccione **ADFSSSO-Web API** y haga clic en **Editar..** .

    ![Captura de pantalla del cuadro de diálogo Propiedades de D F s s O que muestra la API Web que se muestra en la sección aplicaciones.](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.PNG)

11. En **la pantalla ADFSSSO-propiedades de la API Web** , seleccione la pestaña **reglas de transformación de emisión** y haga clic en **Agregar regla..** .

    ![Captura de pantalla del cuadro de diálogo Propiedades de D F s s O que muestra la pestaña reglas de transformación de emisión.](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.PNG)

12. En la pantalla **Asistente para agregar regla de notificación de transformación** , seleccione **enviar notificaciones mediante una regla personalizada** en la lista desplegable y haga clic en **siguiente** .

    ![Captura de pantalla de la página Seleccionar plantilla de regla del Asistente para agregar regla de notificación de transformación que muestra la opción enviar notificaciones mediante una regla personalizada seleccionada.](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.PNG)

13. En la pantalla del **Asistente para agregar regla de notificaciones de transformación** , escriba **ForCustomIDToken** en el nombre de la **regla de notificaciones** y la siguiente regla de notificaciones en **regla personalizada**. Haga clic en **Finish** (Finalizar).

    ```
    x:[]
    => issue(claim=x);
    ```

    ![Captura de pantalla de la página configurar regla del Asistente para agregar regla de notificaciones de transformación que muestra el nombre de la regla de notificaciones y los campos de texto de la regla personalizada rellenados.](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.PNG)

    > [!NOTE]
    > También puede usar PowerShell para asignar los `allatclaims` ámbitos y `openid` .

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
    ```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>Descargue y modifique la aplicación de ejemplo para emitir notificaciones personalizadas en id_token

En esta sección se describe cómo descargar la aplicación Web de ejemplo y modificarla en Visual Studio. Vamos a usar el ejemplo Azure AD que se encuentra [aquí](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC).

Para descargar el proyecto de ejemplo, use git bash y escriba lo siguiente:

```
git clone https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC
```

![Captura de pantalla de la ventana de Git bash que muestra los resultados del comando de clonación de Git.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Para modificar la aplicación

1. Abra el ejemplo con Visual Studio.
2. Vuelva a compilar la aplicación para restaurar todos los paquetes de NuGet que falten.
3. Abra el archivo de web.config.  Modifique los valores siguientes para que tengan el aspecto siguiente:

   ```xml
   <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />
   <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
   <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />
   <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />
   <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
   <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />
   ```

   ![Captura de pantalla del archivo de configuración web que muestra los valores modificados.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.png)

4. Abra el archivo Startup.Auth.cs y realice los cambios siguientes:

   - Retoque la lógica de inicialización del middleware de OpenId Connect con los siguientes cambios:

      ```cs
      private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
      //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
      //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
      private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
      private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
      private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];
      ```

   - Comente lo siguiente:

      ```cs
      //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);
      ```

      ![Captura de pantalla del archivo de autenticación de inicio que muestra las líneas de código comentadas.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.png)

   - Más adelante, modifique las opciones de middleware de OpenId Connect como se indican a continuación:

      ```cs
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

      ![Captura de pantalla del archivo de autenticación de inicio que muestra las opciones de middleware de Open I D Connect modificadas.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.png)

5. Abra el archivo HomeController.cs y realice los cambios siguientes:

   - Agregue la siguiente línea de código:

      ```cs
      using System.Security.Claims;
      ```

   - Actualice el `About()` método como se muestra a continuación:

      ```cs
      [Authorize]
      public ActionResult About()
      {
          ClaimsPrincipal cp = ClaimsPrincipal.Current;
          string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
          ViewBag.Message = String.Format("Hello {0}!", userName);
          return View();
      }
      ```

      ![Captura de pantalla del archivo del controlador de inicio con las actualizaciones de requred.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.png)

### <a name="test-the-custom-claims-in-id-token"></a>Prueba de las notificaciones personalizadas en el token de identificador

Una vez realizados los cambios anteriores, presione F5. Se abrirá la página de ejemplo. Haga clic en iniciar sesión.

![Captura de pantalla de la aplicación de ejemplo que se muestra en un explorador.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Se le redirigirá a la página de inicio de sesión de AD FS. Continúe e inicie sesión.

![Captura de pantalla de la página de inicio de sesión de D F S.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Una vez que se realiza correctamente, debería ver que ha iniciado sesión.

![Captura de pantalla de la aplicación de ejemplo que muestra que el usuario ha iniciado sesión.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.png)

Haga clic en el enlace Acerca de . Verá "Hello [username]", que se recupera de la claim de nombre de usuario en el token de identificador.

![Captura de pantalla de la página acerca de en la aplicación de ejemplo.](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.png)

## <a name="next-steps"></a>Pasos siguientes

[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)
