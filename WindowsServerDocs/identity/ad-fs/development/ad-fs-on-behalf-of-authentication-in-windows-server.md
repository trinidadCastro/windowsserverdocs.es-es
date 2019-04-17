---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: "Crear una aplicación de varios niveles con On-Behalf-Of (OBO) con OAuth con AD FS de 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8940cde2b78ce3ead499263e6fba0fbe28aae695
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016"></a>Crear una aplicación de varios niveles con On-Behalf-Of (OBO) con OAuth con AD FS de 2016

>Se aplica a: Windows Server 2016

Este tutorial proporciona instrucciones para implementar una autenticación (OBO) en nombre de uso de AD FS en Windows Server 2016 TP5.  O más información acerca de la autenticación OBO lee [escenarios de AD FS para desarrolladores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>Advertencia: El ejemplo que se puede generar aquí es solo con fines educativos. Estas instrucciones son para la implementación más simple, más mínima posible exponer los elementos necesarios del modelo. El ejemplo, no puedes incluir todos los aspectos del control de errores y otros relacionados con la funcionalidad y se centra solo obteniendo una autenticación OBO correcta.

## <a name="overview"></a>Introducción

En este ejemplo crearemos un flujo de autenticación que un cliente accederá a un servicio Web de nivel intermedio y el servicio web, a continuación, se actuar en nombre del cliente para obtener un token de acceso autenticado.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.PNG)

A continuación te mostramos el flujo de autenticación que logran la muestra
1. Cliente se autentica en el extremo de autorización de AD FS y solicita un código de autorización
2. Devuelve extremo de autorización de código de autenticación de cliente
3. Usa el código de autenticación de cliente y presenta hacia el extremo de token de AD FS al token de acceso de la solicitud de servicio de Web de nivel intermedio como WebAPI
4. AD FS devuelve el token de acceso al servicio Web de nivel medio. Para obtener funcionalidad adicional necesita acceso a la Backend WebAPI central de nivel de servicio
5. Cliente usa el token de acceso para usar el servicio de nivel intermedio.
6. Servicio de nivel intermedio proporciona el token de acceso al extremo de token de AD FS y solicitudes para acceder a token Backend WebAPI en nombre de usuario autenticado
7. AD FS devuelve el token de acceso para el back-end WebAPI al servicio de nivel intermedio actiing como cliente
8. Central de nivel de servicio usa el token de acceso proporcionado por AD FS en el paso 7 para acceder al back-end WebAPI como cliente y realizar las funciones necesarias

## <a name="sample-structure"></a>Estructura de ejemplo

Muestra se componen de tres módulos


Módulo | Descripción
-------|------------
ToDoClient | Cliente nativo con los que el usuario interactúa
ToDoService | Web API que actúa como un cliente para el back-end WebAPI de nivel en el medio
WebAPIOBO | Back-end de web api que se usa para realizar la operación necesaria cuando el usuario agrega un ToDoItem ToDoService




## <a name="setting-up-the-development-box"></a>Configurar el cuadro de desarrollo

Este tutorial usa Visual Studio 2015. El proyecto usa mucho la biblioteca de autenticación de Active Directory (ADAL). Para obtener información sobre ADAL lee [Active Directory autenticación biblioteca .NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

La muestra usa también v11.0 LocalDB SQL. Instala el LocalDB SQL antes de trabajar en la muestra.

## <a name="setting-up-the-environment"></a>Configurar el entorno
Trabajaremos con una instalación básica de:

1. **DC**: el controlador de dominio del dominio en el que se hospedará AD FS
2. **Servidor de AD FS**: el servidor de AD FS para el dominio
3. **Equipo de desarrollo**: equipo donde tienen instalado Visual Studio y se pueden desarrollar nuestra muestra

Puedes, si quieres, usar solo dos equipos. Uno de DC/ADFS y el otro para el desarrollo de la muestra.

Cómo configurar el controlador de dominio y AD FS queda fuera del ámbito de este artículo. Para información de implementación adicional, consulta:

- [Implementación de AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Implementación de AD FS](../AD-FS-Deployment.md)

La muestra se basa en el ejemplo OBO existente en Azure creado por Vittorio Bertocci y disponible [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Sigue las instrucciones para clonar el proyecto en el equipo de desarrollo y crear una copia de la muestra para comenzar a trabajar con.

## <a name="clone-or-download-this-repository"></a>Clonar o descargar este repositorio

Desde el shell o una línea de comandos:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modificar la muestra

En cuanto se abre la solución WebAPI-OnBehalfOf-DotNet.sln, observarás que tiene dos proyectos en la solución:

* **ToDoListClient**: servirá el cliente OpenID que el usuario interactúa con
* **ToDoListService**: Esto se la aplicación de servidor Web de nivel intermedio / servicio que interactuarán con otro back-end WebAPI OBO del usuario autenticado

Como puedes ver, debemos agregar otro proyecto más adelante que actuará como el recurso que tendrán acceso a la ToDoListService nivel intermedio.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configurar AD FS para el cliente y la aplicación de servidor Web

En el formulario actual de la muestra, la autenticación está configurada para hacer frente a Azure AD. Queremos cambiar el mecanismo de autenticación y directa que hacia AD FS implemente local. Para ello, debemos configurar AD FS para reconocer al cliente y tenemos de App de servidor Web en la muestra.

**Crear un grupo de aplicaciones**

Abre la administración de AD FS MMC y agrega un nuevo grupo de aplicaciones. Selecciona la plantilla de aplicación-nativo-WebAPI.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Haz clic en siguiente y aparecerá la página para proporcionar información acerca de la aplicación cliente. Asigna un nombre apropiado para el cliente de la aplicación en AD FS. Copia el identificador de cliente y Guárdalo en algún lugar puedes acceder a una versión posterior como esto será necesario en la configuración de la aplicación en visual studio.

>Nota: El URI de redireccionamiento puede ser cualquier URI arbitraria como realmente no se utiliza en el caso de los clientes nativos

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Haz clic en siguiente y aparecerá la página para proporcionar información acerca de WebAPI. Dar un nombre para la entrada de AD FS adecuado para el WebAPI y escribe el URI de redirección como el URI que ves en Visual Studio para la ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Haz clic en siguiente y verás la página de directivas de Control de acceso de elegir. Asegúrate de que veas "Permitir todo el mundo" en la sección de directiva.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Haz clic en siguiente y aparecerá la página de configuración de permisos de la aplicación. En esta página, selecciona los ámbitos permitidos como openid (opción predeterminada) y user_impersonation. El ámbito 'user_impersonation' es necesario poder correctamente solicitar un token de acceso en nombre de AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Haz clic en Siguiente aparecerá la página de resumen. Ve a través del resto del asistente y finalizar la configuración.

Para habilitar la autenticación en nombre de otra persona, necesitamos asegurarse de que AD FS devuelve un token de acceso con ámbito user_impersonation al cliente. Modifica la emisión de notificaciones para ToDoListServiceWebApi incluir las siguientes tres reglas personalizadas:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue open id scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "openid");

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Agregar ToDoListService como un cliente en el grupo de aplicaciones**

En esta etapa, necesitamos crear una entrada adicional en AD FS para el servidor Web App para que actúe como un cliente y no solo como un recurso. Abre el grupo de la aplicación que acabas de crear y haz clic en Agregar aplicación.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

Aparecerá la página de "Agregar una nueva aplicación para MySampleGroup". En esa página, selecciona "Servidor aplicación o sitio Web" como la aplicación independiente

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Haz clic en siguiente y aparecerá la página para proporcionar los detalles de la aplicación. Proporcionar un nombre adecuado para la entrada de configuración en la sección de nombre. Asegúrate de que el identificador de cliente es el mismo que el identificador de la ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Haz clic en siguiente y aparecerá la página para configurar las credenciales de la aplicación. Haz clic en "Generar un secreto compartido". Aparecerá una clave secreta que se genera automáticamente. Copia la clave secreta en algún lugar que será necesario mientras se configura la ToDoListService en visual studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Haz clic en siguiente y completa al asistente.

### <a name="modifying-the-todolistclient-code"></a>Modificar el código ToDoListClient

#### <a name="modify-the-application-config"></a>Modificar la configuración de la aplicación

Ve a la estás el proyecto ToDoListClient en WebAPI-OnBehalfOf-DotNet solución. Abre el archivo App.config y realizar las siguientes modificaciones

* La entrada de la clave de ida: inquilino de comentario
* La ida: RedirectURI, escriba el URI arbitrario que proporcionaste al configurar el MySampleGroup_ClientApplication en AD FS.
* Para la clave de ida: ClientID, proporcionar al cliente ID identificador que AD FS le dio al configurar el MySampleGroup_ClientApplication.
* Para la ida: ToDoListResourceID proporcionar el identificador de recursos que se le dio al configurar el ToDoListServiceWebApi en AD FS
* Comentario de la clave de ida: AADInstance
* Para la ida: ToDoListBaseAddress escribe el identificador de recursos de la ToDoListServiceWebApi. Esto se usará al llamar a la ToDoList WebAPI.
* Agregar una clave de ida: entidad y proporcionar el valor como el URI de AD FS.

Tu **appSettings** en App.Config debería ser similar al siguiente:

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>Modificar el código

**MainWindow.xaml.cs**

Comenta la línea leer la información de inquilino de la configuración de la aplicación

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Cambia el valor de autoridad de cadena

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Cambiar el código para leer los valores correctos de ToDoListResourceId y ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

En la función MainWindow() cambie la inicialización de authcontext como:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Agrega un recurso de back-end

Para completar el flujo en nombre de otra persona, debes crear un recurso de back-end que tendrán acceso la ToDoListService en nombre de usuario autenticado. La elección del recurso back-end puede variar según el requisito, pero con el fin de esta muestra se puede crear un WebAPI básica.

* Haga clic con el botón secundario en la solución 'WebAPI-OnBehalfOf-DotNet' en el Explorador de soluciones y selecciona Agregar -> Nuevo proyecto
* Elige la plantilla de aplicación Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* En el símbolo del sistema siguiente, haz clic en 'Cambiar autenticación'
* Selecciona 'Trabajo y cuentas de centro docente' y en la lista desplegable derecha en la misma local
* Escribe la ruta de acceso federationmetadata.xml para la implementación de AD FS y proporcionar un URI de aplicación (proporcionar cualquier URI por ahora, y se cambia más adelante) y haz clic en Aceptar para agregar el proyecto a la solución.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Haga clic en los controladores en el Explorador de soluciones en el nuevo proyecto creado. Selecciona Agregar -> controlador
* En la selección de plantilla, selecciona 'Web API 2 controlador - vaciar' y haz clic en Aceptar.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Asigna el nombre de controlador adecuado

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Agrega el siguiente código en el controlador


        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Http;
        namespace WebAPIOBO.Controllers
        {
            public class WebAPIOBOController : ApiController
            {
                public IHttpActionResult Get()
                {
                    return Ok("WebAPI via OBO");
                }
            }
        }

Este código simplemente devolverá la cadena cuando cualquier usuario ponga una solicitud Get para la WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Agregar el nuevo back-end WebAPI a AD FS

Abra el grupo de la aplicación MySampleGroup. Haz clic en seleccionar la plantilla de Web API y las aplicaciones de agregar y haz clic en siguiente.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

En la API para Web Configurar página proporciona un nombre adecuado para la entrada de WebAPI y el identificador. El identificador debe ser el valor de la dirección URL de SSL WebAPIOBO proyecto en visual studio (similar a lo que hicimos para BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Continúe con el resto del asistente iguales como cuando se configura la ToDoListService WebAPI. Al final del grupo de la aplicación debe ser similar al siguiente:

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modificar el código ToDoListService

#### <a name="modifying-the-application-config"></a>Modificar la configuración de la aplicación

* Abre el archivo de Web.config
* Modificar las siguientes claves

| Clave | Valor |
|:-----|:-------|
|ida: público| Id. de la ToDoListService como dispone de AD FS al configurar el ToDoListService WebAPI, por ejemplo, https://localhost:44321/|
|ida: ClientID| Id. de la ToDoListService como dispone de AD FS al configurar el ToDoListService WebAPI, por ejemplo, https://localhost:44321/ </br>**Es muy importante que el público: ida y ida: ClientID coinciden entre sí**|
|ida: ClientSecret| Este es el secreto que AD FS se generan cuando se configura el cliente ToDoListService en AD FS|
|ida: ADFSMetadata| Esta es la dirección URL a los metadatos de AD FS para https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml p. ej.|
|ida: OBOWebAPIBase| Esta es la dirección de base que vamos a usar para llamar a la API de back-end para https://localhost:44300 p. ej.|
|Entidad de ida:| Esta es la dirección URL para el servicio de AD FS, https://fs.anandmsft.com/adfs/ de ejemplo|


Todos los demás ida: XXXXXXX teclas en el **appsettings** nodo puede comenta o eliminado

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Cambio de la autenticación de Azure AD a AD FS

* Abre el archivo Startup.Auth.cs
* Quitar el siguiente código

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

con

        app.UseActiveDirectoryFederationServicesBearerAuthentication(
            new ActiveDirectoryFederationServicesBearerAuthenticationOptions
            {
                MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
                TokenValidationParameters = new TokenValidationParameters()
                {
                    SaveSigninToken = true,
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                }
            });

#### <a name="modifying-the-todolistcontroller"></a>Modificar el ToDoListController

Agrega la referencia System.Web. Extensiones. Modificar a los miembros de clase reemplazando el siguiente código

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The App Key is a credential used by the application to authenticate to Azure AD.
    // The Tenant is the name of the Azure AD tenant in which this application is registered.
    // The AAD Instance is the instance of Azure, for example public Azure or Azure China.
    // The Authority is the sign-in URL of the tenant.
    //
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string appKey = ConfigurationManager.AppSettings["ida:AppKey"];

    //
    // To authenticate to the Graph API, the app needs to know the Grah API's App ID URI.
    // To contact the Me endpoint on the Graph API we need the URL as well.
    //
    private static string graphResourceId = ConfigurationManager.AppSettings["ida:GraphResourceId"];
    private static string graphUserUrl = ConfigurationManager.AppSettings["ida:GraphUserUrl"];
    private const string TenantIdClaimType = "https://schemas.microsoft.com/identity/claims/tenantid";

con

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**Modificar la reclamación utilizada para nombre**

Desde AD FS hemos emitido la reclamación Nmae, pero no hemos emitido NameIdentifier reclamación. La muestra usa NameIdentifier a exclusivamente clave en los elementos de ToDo. Por cuestiones de simplicidad, puede quitar el NameIdentifier con la notificación de nombre en el código. Buscar y reemplazar todas las apariciones de NameIdentifier con nombre.

**Modificar la rutina de Post y CallGraphAPIOnBehalfOfUser()**

Copia y pega el siguiente código en ToDoListController.cs y reemplaza el código para registrar e CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
        if (!ClaimsPrincipal.Current.FindFirst("https://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
        {
            throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found" });
        }

        //
        // Call the WebAPIOBO On Behalf Of the user who called the To Do list web API.
        //
        string augmentedTitle = null;
        string custommessage = await CallGraphAPIOnBehalfOfUser();
        if (custommessage != null)
        {
            augmentedTitle = String.Format("{0}, Message: {1}", todo.Title, custommessage);
        }
        else
        {
            augmentedTitle = todo.Title;
        }

        if (null != todo && !string.IsNullOrWhiteSpace(todo.Title))
        {
            db.TodoItems.Add(new TodoItem { Title = augmentedTitle, Owner = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value });
            db.SaveChanges();
        }
    }

    public static async Task<string> CallGraphAPIOnBehalfOfUser()
    {
        string accessToken = null;
        AuthenticationResult result = null;
        AuthenticationContext authContext = null;
        HttpClient httpClient = new HttpClient();
        string custommessage = "";

        //
        // Use ADAL to get a token On Behalf Of the current user.  To do this we will need:
        //      The Resource ID of the service we want to call.
        //      The current user's access token, from the current request's authorization header.
        //      The credentials of this application.
        //      The username (UPN or email) of the user calling the API
        //
        ClientCredential clientCred = new ClientCredential(clientId, clientSecret);
        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        string userName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn) != null ? ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value : ClaimsPrincipal.Current.FindFirst(ClaimTypes.Email).Value;
        string userAccessToken = bootstrapContext.Token;
        UserAssertion userAssertion = new UserAssertion(bootstrapContext.Token, "urn:ietf:params:oauth:grant-type:jwt-bearer", userName);

        string userId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
        authContext = new AuthenticationContext(authority, false);

        // In the case of a transient error, retry once after 1 second, then abandon.
        // Retrying is optional.  It may be better, for your application, to return an error immediately to the user and have the user initiate the retry.
        bool retry = false;
        int retryCount = 0;

        do
        {
            retry = false;
            try
            {
                result = authContext.AcquireToken(OBOWebAPIBase, clientCred, userAssertion);
                accessToken = result.AccessToken;
            }
            catch (AdalException ex)
            {
                if (ex.ErrorCode == "temporarily_unavailable")
                {
                    // Transient error, OK to retry.
                    retry = true;
                    retryCount++;
                    Thread.Sleep(1000);
                }
            }
        } while ((retry == true) && (retryCount < 1));

        if (accessToken == null)
        {
            // An unexpected error occurred.
            return (null);
        }

        // Once the token has been returned by ADAL, add it to the http authorization header, before making the call to access the To Do list service.
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

        // Call the WebAPIOBO.
        HttpResponseMessage response = await httpClient.GetAsync(OBOWebAPIBase + "/api/WebAPIOBO");


        if (response.IsSuccessStatusCode)
        {
            // Read the response and databind to the GridView to display To Do items.
            string s = await response.Content.ReadAsStringAsync();
            JavaScriptSerializer serializer = new JavaScriptSerializer();
            custommessage = serializer.Deserialize<string>(s);
            return custommessage;
        }
        else
        {
            custommessage = "Unsuccessful OBO operation : " + response.ReasonPhrase;
        }
        // An unexpected error occurred calling the Graph API.  Return a null profile.
        return (null);
    }

## <a name="running-the-solution"></a>Ejecuta la solución


De manera predeterminada visual studio está configurado para ejecutarse un proyecto cuando llegas depuración para ejecutarse.

* Haga clic con el botón secundario en la solución y seleccionadas propiedades.
* En la página Propiedades seleccione Inicio varios proyectos y cambiar la acción de inicio para las tres entradas.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Presionar la tecla F5 y ejecutar la solución

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Haz clic en el botón de inicio de sesión. Se te pedirá que inicie sesión con AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Después de iniciar sesión, agrega un elemento ToDo en la lista. En segundo plano, vamos a realizar una operación Post a la ToDoListService que va a realizar más una publicación en la web WebAPIOBO API.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

En la operación correcta verás que el elemento se ha agregado a la lista con el mensaje adicional desde el back-end de Web API que se tuvo acceso con el flujo de autenticación OBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

También puedes ver los seguimientos detallados en Fiddler. Inicia Fiddler y habilitar descifrado HTTPS. Puedes ver que creamos dos solicitudes al extremo /adfs/oautincludes.
En la primera interacción, se presenta el código para el extremo del token de acceso y obtener un acceso token https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

En la segunda interacción con el extremo del token, puedes ver que tenemos **requested_token_use** establecer como **on_behalf_of** y que estamos usando el token de acceso que se obtienen del servicio web de nivel intermedio, es decir, https://localhost:44321/ como la aserción para obtener el token en nombre de.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Pasos siguientes
[AD FS desarrollo](../../ad-fs/AD-FS-Development.md)  
