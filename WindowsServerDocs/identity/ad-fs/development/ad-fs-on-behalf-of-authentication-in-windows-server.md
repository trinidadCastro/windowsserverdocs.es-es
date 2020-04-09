---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: Compilar una aplicación de varios niveles mediante "en nombre de" (OBO) mediante OAuth con AD FS 2016 o posterior.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ed8bb6300360553e0809f4a30cec38bc37777ae9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858848"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>Compilar una aplicación de varios niveles mediante "en nombre de" (OBO) mediante OAuth con AD FS 2016 o posterior.


En este tutorial se proporcionan instrucciones para implementar una autenticación en nombre de (OBO) mediante AD FS en Windows Server 2016 TP5 o posterior. Para obtener más información acerca de la autenticación de OBO, lea [AD FS los flujos de OpenID Connect/OAuth y los escenarios de aplicación](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)

>ADVERTENCIA: el ejemplo que se puede compilar aquí es solo con fines educativos. Estas instrucciones son para la implementación más sencilla y mínima posible para exponer los elementos necesarios del modelo. Es posible que el ejemplo no incluya todos los aspectos del control de errores y otras funciones de relación y se Centre solo en obtener una autenticación OBO correcta.

## <a name="overview"></a>Información general

En este ejemplo, vamos a crear un flujo de autenticación donde un cliente tendrá acceso a un servicio Web de nivel intermedio y el servicio Web actuará en nombre del cliente autenticado para obtener un token de acceso.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

A continuación se muestra el flujo de autenticación que obtendrá el ejemplo
1. El cliente se autentica en AD FS extremo de autorización y solicita un código de autorización
2. El punto de conexión de autorización devuelve el código de autenticación al cliente
3. El cliente utiliza el código de autenticación y lo presenta al extremo del token de AD FS para solicitar el token de acceso para el servicio Web de nivel intermedio como WebAPI
4. AD FS devuelve el token de acceso al servicio Web de nivel intermedio. Para obtener funcionalidad adicional, el servicio de nivel intermedio necesita acceso al servidor WebAPI de back-end
5. El cliente utiliza el token de acceso para usar el servicio de nivel intermedio.
6. El servicio de nivel intermedio proporciona el token de acceso al punto de conexión del token de AD FS y solicita el token de acceso para el back-end de WebAPI en nombre del usuario autenticado.
7. AD FS devuelve el token de acceso para el back-end de WebAPI al servicio de nivel intermedio que se está deteniendo como cliente
8. El servicio de nivel intermedio usa el token de acceso proporcionado por AD FS en el paso 7 para tener acceso al servicio WebAPI de back-end como cliente y realizar las funciones necesarias.

## <a name="sample-structure"></a>Estructura de ejemplo

El ejemplo consta de tres módulos


Módulo | Descripción
-------|------------
ToDoClient | Cliente nativo con el que interactúa el usuario
ToDoService | API Web de nivel intermedio que actúa como cliente de la WebAPI de back-end
WebAPIOBO | API Web de back-end que usa ToDoService para realizar la operación necesaria cuando el usuario agrega un ToDoItem




## <a name="setting-up-the-development-box"></a>Configuración del cuadro de desarrollo

En este tutorial se usa Visual Studio 2015. El proyecto usa mucho Biblioteca de autenticación de Active Directory (ADAL). Para obtener información sobre ADAL, lea [biblioteca de autenticación de Active Directory .net](https://msdn.microsoft.com/library/azure/mt417579.aspx)

En el ejemplo también se usa SQL LocalDB v 11.0. Instale SQL LocalDB antes de trabajar en el ejemplo.

## <a name="setting-up-the-environment"></a>Configurar el entorno
Vamos a trabajar con una configuración básica de:

1. **DC**: controlador de dominio para el dominio en el que se hospedará AD FS
2. **AD FS Server**: el servidor de AD FS para el dominio
3. **Máquina de desarrollo**: equipo en el que tenemos instalado Visual Studio y que va a desarrollar nuestro ejemplo

Si lo desea, puede usar solo dos máquinas. Uno para DC/ADFS y otro para desarrollar el ejemplo.

Cómo configurar el controlador de dominio y AD FS está fuera del ámbito de este artículo. Para obtener información adicional sobre la implementación, consulte:

- [AD DS Deployment](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Implementación de AD FS](../AD-FS-Deployment.md)

El ejemplo se basa en el ejemplo OBO existente en Azure creado por Vittorio BERTOCCI y disponible [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Siga las instrucciones para clonar el proyecto en el equipo de desarrollo y crear una copia del ejemplo para empezar a trabajar con.

## <a name="clone-or-download-this-repository"></a>Clonar o descargar este repositorio

Desde el shell o la línea de comandos:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modificar el ejemplo

En cuanto Abra la solución WebAPI-OnBehalfOf-DotNet. sln, observará que tiene dos proyectos en la solución

* **ToDoListClient**: Esto servirá como el cliente OpenID con el que el usuario va a interactuar.
* **ToDoListService**: este será el servicio o la aplicación WebServer de nivel intermedio que interactuará con otro servidor WebAPI OBO el usuario autenticado.

Como puede ver, es necesario agregar otro proyecto más adelante, lo que actuará como el recurso al que tendrá acceso el ToDoListService de nivel intermedio.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configuración de AD FS para la aplicación cliente y WebServer

En el formulario actual del ejemplo, la autenticación se configura para realizarse en Azure AD. Queremos cambiar el mecanismo de autenticación y dirigirlo hacia AD FS implementado de forma local. Para ello, necesitamos configurar AD FS para reconocer la aplicación cliente y WebServer que tenemos en el ejemplo.

**Creación de un grupo de aplicaciones**

Abra el MMC de administración de AD FS y agregue un nuevo grupo de aplicaciones. Seleccione la plantilla Native-Application-WebAPI.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Haga clic en siguiente y aparecerá la página para proporcionar información sobre la aplicación cliente. Asigne un nombre adecuado a la aplicación cliente en AD FS. Copie el identificador de cliente y guárdelo en un lugar en el que pueda acceder más adelante, ya que será necesario en la configuración de la aplicación en Visual Studio.

>Nota: el URI de redirección puede ser cualquier URI arbitrario ya que realmente no se usa en el caso de clientes nativos.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Haga clic en siguiente y aparecerá la página para proporcionar información acerca de WebAPI. Asigne un nombre adecuado a la entrada AD FS de WebAPI y escriba el URI de redireccionamiento como el URI que ve en Visual Studio para ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Haga clic en siguiente y verá la página elegir Directiva de Access Control. Asegúrese de ver "permitir todos" en la sección Directiva.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Haga clic en siguiente y aparecerá la página configurar permisos de aplicación. En esta página, seleccione los ámbitos permitidos como OpenID (seleccionada de forma predeterminada) y user_impersonation. El ámbito ' user_impersonation ' es necesario para poder solicitar correctamente un token de acceso en nombre de AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Haga clic en siguiente para mostrar la página Resumen. Recorra el resto del asistente y finalice la configuración.

Para habilitar la autenticación en nombre de, es necesario asegurarse de que AD FS devuelve un token de acceso con el ámbito user_impersonation al cliente. Modifique la emisión de notificaciones para que ToDoListServiceWebApi incluya las siguientes tres reglas personalizadas:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "http://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Agregar ToDoListService como cliente en el grupo de aplicaciones**

En esta fase, es necesario realizar una entrada adicional en AD FS para que la aplicación WebServer actúe como un cliente y no como un recurso. Abra el grupo de aplicaciones que acaba de crear y haga clic en Agregar aplicación.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

Se le presentará la página "agregar una nueva aplicación a MySampleGroup". En esa página, seleccione "aplicación de servidor o sitio web" como aplicación independiente.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Haga clic en siguiente y se le presentará la página para proporcionar los detalles de la aplicación. Proporcione un nombre adecuado para la entrada de configuración en la sección nombre. Asegúrese de que el identificador de cliente es el mismo que el identificador de ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Haga clic en siguiente y se le presentará la página para configurar las credenciales de la aplicación. Haga clic en "generar un secreto compartido". Se le presentará un secreto que se genera automáticamente. Copie el secreto en alguna ubicación, ya que será necesario mientras configuramos ToDoListService en Visual Studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Haga clic en siguiente y complete el asistente.

### <a name="modifying-the-todolistclient-code"></a>Modificar el código de ToDoListClient

#### <a name="modify-the-application-config"></a>Modificación de la configuración de la aplicación

Vaya a su proyecto ToDoListClient en la solución WebAPI-Onbehalfof-DotNet. Abra el archivo app. config y realice las siguientes modificaciones:

* Comentario de la entrada de clave ida: tenant
* En el caso de ida: RedirectURI, escriba el URI arbitrario que proporcionó al configurar el MySampleGroup_ClientApplication en AD FS.
* Para la clave ida: ClientID, proporcione el identificador de identificador de cliente que AD FS concedió durante la configuración de la MySampleGroup_ClientApplication.
* En el caso de ida: ToDoListResourceID, proporcione el identificador de recurso que proporcionó durante la configuración de ToDoListServiceWebApi en AD FS
* Comente la clave ida: AADInstance
* En el caso de ida: ToDoListBaseAddress, escriba el identificador de recurso de ToDoListServiceWebApi. Se usará al llamar a ToDoList WebAPI.
* Agregue una clave ida: Authority y proporcione el valor como URI para AD FS.

El **appSettings** del archivo app. config debería tener un aspecto similar al siguiente:

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

Comentario la línea que lee la información del inquilino desde la configuración de la aplicación

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Cambiar el valor de la entidad de la cadena a

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Cambie el código para leer los valores correctos de ToDoListResourceId y ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

En la función MainWindow (), cambie la inicialización de authcontext como:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Agregar el recurso de back-end

Para completar el flujo en nombre de, debe crear un recurso de back-end al que el ToDoListService tendrá acceso en nombre del usuario autenticado. La elección del recurso de back-end puede variar según el requisito, pero para el propósito de este ejemplo puede crear una WebAPI básica.

* Haga clic con el botón derecho en la solución ' WebAPI-Onbehalfof-DotNet ' en el explorador de soluciones y seleccione Agregar > nuevo proyecto.
* Elección de la plantilla de aplicación Web de ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* En el siguiente símbolo del sistema, haga clic en "cambiar autenticación"
* Seleccione ' cuentas profesionales y educativas ' y, en la lista desplegable derecha, seleccione ' local '.
* Escriba la ruta de acceso de federationmetadata. XML de la implementación de AD FS y proporcione un URI de la aplicación (proporcione cualquier URI por ahora, y lo cambiará más adelante) y haga clic en Aceptar para agregar el proyecto a la solución.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Haga clic con el botón derecho en controladores en el explorador de soluciones, en el nuevo proyecto creado. Seleccionar controlador de > de complementos
* En la selección de plantilla, seleccione ' controlador de Web API 2-vacío ' y haga clic en Aceptar.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Asigne un nombre adecuado al controlador.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Agregue el código siguiente en el controlador:

```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http;
    namespace WebAPIOBO.Controllers
    {
        [Authorize]
        public class WebAPIOBOController : ApiController
        {
            public IHttpActionResult Get()
            {
                return Ok($"WebAPI via OBO (user: {User.Identity.Name}");
            }
        }
    }
```

Este código simplemente devolverá la cadena cuando alguien ponga una solicitud GET para el WebAPIOBO de WebAPI.

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Agregando el nuevo WebAPI de back-end a AD FS

Abra el grupo de aplicaciones MySampleGroup. Haga clic en Agregar aplicación y seleccione plantilla de API Web y haga clic en siguiente.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

En la página configurar API Web, proporcione un nombre adecuado para la entrada WebAPI y el identificador. El identificador debe ser la dirección URL de SSL de valor del proyecto WebAPIOBO en Visual Studio (similar a lo que hicimos para BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Continúe con el resto del asistente, igual que cuando se configuró ToDoListService WebAPI. Al final, el grupo de aplicaciones debe tener el siguiente aspecto:

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modificar el código de ToDoListService

#### <a name="modifying-the-application-config"></a>Modificación de la configuración de la aplicación

* Abra el archivo Web. config.
* Modificar las claves siguientes

| Key                      | Valor                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ida: audiencia             | IDENTIFICADOR de ToDoListService tal como se indica en AD FS al configurar ToDoListService WebAPI, por ejemplo, https://localhost:44321/                                                                                         |
| ida: ClientID             | IDENTIFICADOR de ToDoListService tal como se indica en AD FS al configurar ToDoListService WebAPI, por ejemplo, <https://localhost:44321/> </br>**Es muy importante que ida: Audience y ida: ClientID coincidan entre sí.** |
| ida: ClientSecret         | Este es el secreto que AD FS genera al configurar el cliente de ToDoListService en AD FS                                                                                                                   |
| ida: AdfsMetadataEndpoint | Esta es la dirección URL de los metadatos de AD FS, por ejemplo, https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| ida: OBOWebAPIBase        | Esta es la dirección base que se utilizará para llamar a la API de back-end, por ejemplo, https://localhost:44300                                                                                                                     |
| ida: autoridad            | Esta es la dirección URL del servicio AD FS, por ejemplo https://fs.anandmsft.com/adfs/                                                                                                                                          |

Todas las demás claves ida: XXXXXXX del nodo **appSettings** se pueden eliminar o comentar

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Cambiar la autenticación de Azure AD a AD FS

* Abra el archivo Startup.Auth.cs
* Quitar el siguiente código

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

por

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

#### <a name="modifying-the-todolistcontroller"></a>Modificar ToDoListController

Agregue una referencia a System. Web. Extensions. Modifique los miembros de clase reemplazando el código siguiente

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

por

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**Modificar la demanda usada para el nombre**

A partir de AD FS vamos a emitir la demanda de Nmae, pero no vamos a emitir notificaciones de NameIdentifier. En el ejemplo se usa NameIdentifier para clave única en los elementos de la lista de tareas. Para simplificar, puede quitar de forma segura el NameIdentifier con la demanda de nombre en el código. Busque y reemplace todas las apariciones de NameIdentifier por nombre.

**Modificar rutina post y CallGraphAPIOnBehalfOfUser ()**

Copie y pegue el código siguiente en ToDoListController.cs y reemplace el código de post y CallGraphAPIOnBehalfOfUser

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
        // The Resource ID of the service we want to call.
        // The current user's access token, from the current request's authorization header.
        // The credentials of this application.
        // The username (UPN or email) of the user calling the API
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
                    result = await authContext.AcquireTokenAsync(OBOWebAPIBase, clientCred, userAssertion);
                    //result = await authContext.AcquireTokenAsync(...);
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

## <a name="running-the-solution"></a>Ejecutar la solución


De forma predeterminada, Visual Studio está configurado para ejecutar un proyecto cuando se alcanza la depuración para ejecutarse.

* Haga clic con el botón derecho en la solución y seleccione Propiedades.
* En la página Propiedades, seleccione proyectos de inicio múltiples y cambie la acción a iniciar para las tres entradas.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Presione F5 y ejecute la solución.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Haga clic en el botón iniciar sesión. Se le pedirá que inicie sesión con AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Después de iniciar sesión, agregue un elemento ToDo en la lista. En segundo plano, vamos a realizar una operación post en el ToDoListService que realizará una post a la API Web de WebAPIOBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

Si la operación se realiza correctamente, verá que el elemento se ha agregado a la lista con el mensaje adicional de la API Web de back-end a la que se obtuvo acceso mediante el flujo de autenticación de OBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

También puede ver los seguimientos detallados de Fiddler. Inicie Fiddler y habilite el descifrado HTTPS. Puede ver que realizamos dos solicitudes al punto de conexión/ADFS/oautincludes.
En la primera interacción, presentamos el código de acceso al punto de conexión del token y obtenemos un token de acceso para https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

En la segunda interacción con el punto de conexión del token, puede ver que hemos **requested_token_use** establecido como **on_behalf_of** y estamos usando el token de acceso obtenido para el servicio Web de nivel intermedio, es decir, https://localhost:44321/ como la aserción para obtener el token en nombre de.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
