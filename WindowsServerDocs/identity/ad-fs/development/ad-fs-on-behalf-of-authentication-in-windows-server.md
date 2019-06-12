---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: Crear una aplicación de varios niveles utilizando en nombre de (OBO) mediante OAuth con AD FS 2016 o posterior
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 047f297cfaabff3cbbd45057a4198e2fd2e747de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445452"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>Crear una aplicación de varios niveles utilizando en nombre de (OBO) mediante OAuth con AD FS 2016 o posterior


Este tutorial proporciona instrucciones para implementar una on-behalf-of (OBO) la autenticación mediante AD FS en Windows Server 2016 TP5 o posterior. Para obtener más información sobre la autenticación delegada, lea [escenarios de AD FS para desarrolladores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>ADVERTENCIA: El ejemplo que se puede generar aquí es solo con fines educativos. Estas instrucciones son para la implementación más sencilla y más mínima posible exponer los elementos necesarios del modelo. El ejemplo no puede incluir todos los aspectos del control de errores y otros se relacionan con la funcionalidad y se centra sólo en obtener una autenticación delegada correcta.

## <a name="overview"></a>Información general

En este ejemplo vamos a crear un flujo de autenticación que un cliente tendrá acceso a un servicio Web de nivel intermedio y, a continuación, el servicio web actúa en nombre del cliente para obtener un token de acceso autenticado.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

A continuación se presenta el flujo de autenticación que logran el ejemplo
1. Cliente se autentica en el punto de conexión de autorización de AD FS y solicita un código de autorización
2. Extremo de autorización devuelve código de autenticación de cliente
3. Utiliza código de autenticación de cliente y presenta para el extremo de token de AD FS al token de acceso de solicitud de servicio de Web de nivel intermedio como WebAPI
4. AD FS, devuelve el token de acceso al servicio Web de nivel intermedio. Para obtener funcionalidad adicional, servicio de nivel intermedio necesita tener acceso a la Backend WebAPI
5. Cliente utiliza el token de acceso para usar el servicio de nivel intermedio.
6. Servicio de nivel intermedio proporciona el token de acceso en el extremo de token de AD FS y las solicitudes de token de acceso para Backend WebAPI en nombre de usuario autenticado
7. AD FS devuelve el token de acceso para el back-end de WebAPI al servicio de nivel intermedio actiing como cliente
8. Servicio de nivel intermedio utiliza el token de acceso proporcionado por AD FS en el paso 7 para tener acceso a la API Web como cliente de back-end y realizar las funciones necesarias

## <a name="sample-structure"></a>Ejemplo de estructura

Ejemplo comprenderán de tres módulos


Módulo | Descripción
-------|------------
ToDoClient | Cliente nativo con el que interactúa el usuario
ToDoService | Segundo nivel API web que actúa como un cliente para el back-end de WebAPI
WebAPIOBO | Back-end web api que ToDoService sirve para realizar la operación necesaria cuando el usuario agrega una tarea pendiente




## <a name="setting-up-the-development-box"></a>Configurar el equipo del desarrollador

Este tutorial usa Visual Studio 2015. El proyecto utiliza mucho Active Directory Authentication Library (ADAL). Para obtener información sobre ADAL lea [Active Directory Authentication biblioteca de .NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

El ejemplo también utiliza LocalDB de SQL v11.0. Instalar LocalDB de SQL antes de trabajar en el ejemplo.

## <a name="setting-up-the-environment"></a>Configuración del entorno
Trabajaremos con una instalación básica de:

1. **DC**: Controlador de dominio para el dominio en el que se hospedará AD FS
2. **Servidor de AD FS**: El servidor de AD FS para el dominio
3. **Equipo de desarrollo**: Máquina donde tiene instalado Visual Studio y a desarrollar nuestro ejemplo

Puede, si lo desea, usar solo dos máquinas. Uno para ADFS/controlador de dominio y otro para el desarrollo de la muestra.

Cómo configurar el controlador de dominio y AD FS está fuera del ámbito de este artículo. Para información de implementación adicionales, vea:

- [AD DS Deployment](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Implementación de AD FS](../AD-FS-Deployment.md)

El ejemplo se basa en el ejemplo OBO existente en Azure creado por Vittorio Bertocci y disponible [aquí](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Siga las instrucciones para clonar el proyecto en el equipo de desarrollo y crear una copia del ejemplo para empezar a trabajar con.

## <a name="clone-or-download-this-repository"></a>Clone o descargue este repositorio

En el shell o la línea de comandos:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modificar el ejemplo

En cuanto se abra la solución OnBehalfOf-WebAPI-DotNet.sln, observará que tiene dos proyectos en la solución

* **ToDoListClient**: Esto servirá como el cliente de OpenID que interactúa con el usuario
* **ToDoListService**: Esto se la aplicación de servidor Web de nivel intermedio / servicio que interactúa con otra back-end de WebAPI OBO el usuario autenticado

Como puede ver, necesitamos agregar otro proyecto más adelante que actuará como el recurso que tendrán acceso al nivel intermedio ToDoListService.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configuración de AD FS para el cliente y la aplicación de servidor Web

En el formulario actual del ejemplo, la autenticación se configura para que se realicen en Azure AD. Queremos cambiar el mecanismo de autenticación y direct que hacia AD FS implementado en el entorno local. Para ello, necesitamos configurar AD FS para que reconozca al cliente y App WebServer tenemos en el ejemplo.

**Creación de un grupo de aplicaciones**

Abra el MMC de administración de AD FS y agregue un nuevo grupo de aplicaciones. Seleccione la plantilla de aplicación-WebAPI-nativo.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Haga clic en siguiente y aparecerá la página para proporcionar información acerca de la aplicación cliente. Asigne un nombre adecuado para el cliente de aplicación en AD FS. Copie el identificador de cliente y guárdela en algún lugar puede tener acceso a una versión posterior como esto será necesario en la configuración de la aplicación en visual studio.

>Nota: El URI de redirección puede ser cualquier URI arbitrario, ya que no se usa realmente en el caso de los clientes nativos

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Haga clic en siguiente y aparecerá la página para proporcionar información acerca de la API Web. Asigne un nombre adecuado para la entrada de AD FS para la API Web y escriba el URI de redireccionamiento que el URI que se ve en Visual Studio para el ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Haga clic en siguiente y verá la página de la directiva de Control de acceso a elegir. Asegúrese de que aparezca "Permitir todos los usuarios" en la sección de la directiva.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Haga clic en siguiente y aparecerá la página de permisos de la aplicación configuración. En esta página, seleccione los ámbitos permitidos como openid (opción predeterminada) y user_impersonation. El ámbito 'user_impersonation' es necesario para realizar correctamente solicitar un token de acceso en nombre de otra persona de AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

A continuación, haga clic en se mostrará la página de resumen. Recorra el resto del asistente y finalizar la configuración.

Con el fin de habilitar la autenticación en nombre de, es necesario asegurarse de que AD FS devuelve un token de acceso con ámbito user_impersonation al cliente. Modifique la emisión de notificaciones para ToDoListServiceWebApi incluir las siguientes tres reglas personalizadas:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Agregar ToDoListService como un cliente en el grupo de aplicaciones**

En esta fase, necesitamos realizar una entrada adicional en AD FS para el servidor Web App para que actúe como un cliente y no solo como un recurso. Abra el grupo de aplicaciones que acaba de crear y haga clic en Agregar aplicación.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

Aparecerá la página "Agregar una nueva aplicación a MySampleGroup". En la página, seleccione "Aplicación o sitio Web del servidor" como la aplicación independiente

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Haga clic en siguiente y aparecerá la página para proporcionar detalles de la aplicación. Proporcione un nombre adecuado para la entrada de configuración en la sección de nombre. Asegúrese de que el identificador de cliente es el mismo que el identificador para el ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Haga clic en siguiente y aparecerá la página para configurar las credenciales de la aplicación. Haga clic en "Generar un secreto compartido". Aparecerá un secreto que se genera automáticamente. Copie el secreto en alguna ubicación ya que será necesario mientras configuramos el ToDoListService en visual studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Haga clic en siguiente y complete el asistente.

### <a name="modifying-the-todolistclient-code"></a>Modificar el código ToDoListClient

#### <a name="modify-the-application-config"></a>Modificar la configuración de aplicación

Vaya a la está el proyecto ToDoListClient solución OnBehalfOf-WebAPI-DotNet. Abra el archivo App.config y realice las siguientes modificaciones

* Comentario de la entrada de la clave ida: inquilino
* RedirectURI ida: escriba el URI arbitrario que proporcionó al configurar el MySampleGroup_ClientApplication en AD FS.
* Para la clave ida: ClientID, proporcionar al cliente para el identificador de Id. que AD FS le proporcionó al configurar el MySampleGroup_ClientApplication.
* Para la ida: ToDoListResourceID proporcionan el identificador de recurso que asignó al configurar el ToDoListServiceWebApi en AD FS
* Comentario de la clave ida: AADInstance
* Para la ida: ToDoListBaseAddress escriba el identificador de recurso de la ToDoListServiceWebApi. Esto se utilizará al llamar a la ToDoList WebAPI.
* Agregar una clave ida: autoridad y proporcionar el valor como el URI para AD FS.

Su **appSettings** en el archivo App.Config debe ser similar al siguiente:

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

Comente la línea que leer la información del inquilino de la configuración de aplicación

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Cambie el valor de autoridad de cadena para

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Cambiar el código para leer los valores correctos de ToDoListResourceId y ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

En la función MainWindow() cambie la inicialización authcontext como:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Agregue el recurso de back-end

Para completar el flujo de on-behalf-of, deberá crear un recurso de back-end que accederá ToDoListService en nombre de usuario autenticado. La elección del recurso de back-end puede variar según los requisitos, pero con el propósito de este ejemplo puede crear una API Web básica.

* Haga clic con el botón derecho en la solución 'OnBehalfOf-WebAPI-DotNet' en el Explorador de soluciones y seleccione Agregar -> Nuevo proyecto
* Elija la plantilla aplicación Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* En el siguiente símbolo del sistema haga clic en "Cambiar autenticación"
* Seleccione "Cuentas profesionales o educativas" y en la lista desplegable derecha, seleccione 'Local'
* Escriba la ruta de acceso federationmetadata.xml para la implementación de AD FS y proporcione un URI de la aplicación (proporcione cualquier URI por ahora y se cambiará más adelante) y haga clic en Aceptar para agregar el proyecto a la solución.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Haga clic en los controladores en el Explorador de soluciones en el nuevo proyecto creado. Seleccione Agregar -> controlador
* En la selección de plantilla, seleccione "Web API 2 Controller - Empty" y haga clic en Aceptar.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Asigne el nombre de controlador adecuado

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Agregue el código siguiente en el controlador


~~~
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
~~~

Este código simplemente devolverá la cadena cuando alguien coloca una solicitud Get para la WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Agregar el nuevo back-end de WebAPI a AD FS

Abra el grupo de aplicaciones MySampleGroup. Haga clic en Agregar aplicación y seleccione la plantilla API Web y haga clic en siguiente.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

En la página Configurar Web API proporcionan un nombre adecuado para la entrada de API Web y el identificador. El identificador debe ser el valor de la dirección URL de SSL del proyecto WebAPIOBO en visual studio (similar a lo que hicimos para BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Continúe con el resto del Asistente para la misma como cuando se configura la ToDoListService WebAPI. Al final el grupo de aplicaciones debe ser similar al siguiente:

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modificar el código de ToDoListService

#### <a name="modifying-the-application-config"></a>Modificar la configuración de aplicación

* Abra el archivo Web.config
* Modifique las siguientes claves

| Key                      | Valor                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ida:Audience             | Id. de ToDoListService tal como se indica a AD FS durante la configuración de la ToDoListService WebAPI, por ejemplo, https://localhost:44321/                                                                                         |
| ida:ClientID             | Id. de ToDoListService tal como se indica a AD FS durante la configuración de la ToDoListService WebAPI, por ejemplo, <https://localhost:44321/> </br>**Es muy importante que la audiencia: ida y ida: ClientID coinciden entre sí** |
| ida:ClientSecret         | Este es el secreto que AD FS que se genera cuando se configura el cliente ToDoListService en AD FS                                                                                                                   |
| ida:AdfsMetadataEndpoint | Por ejemplo, esta es la URL a los metadatos de AD FS para https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| ida:OBOWebAPIBase        | Esta es la dirección base que se utilizará para llamar a la API de back-end para p. ej. https://localhost:44300                                                                                                                     |
| ida:Authority            | Se trata de la dirección URL para el servicio AD FS, ejemplo https://fs.anandmsft.com/adfs/                                                                                                                                          |

Las claves de todos los demás ida: XXXXXXX en el **appsettings** puede comentar o eliminar nodo

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Cambio de la autenticación de Azure AD a AD FS

* Abra el archivo Startup.Auth.cs
* Quite el código siguiente

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

#### <a name="modifying-the-todolistcontroller"></a>Modificar el ToDoListController

Agregue la referencia a System.Web.Extensions. Modificar a los miembros de clase, reemplazando el código siguiente

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

**Modificar la notificación utilizada para el nombre**

De AD FS hemos emitido la notificación Nmae pero no hemos emitido notificación NameIdentifier. El ejemplo usa NameIdentifier a clave de forma única en los elementos de lista de tareas. Por motivos de simplicidad, puede quitar el NameIdentifier con notificación de nombre en el código. Buscar y reemplazar todas las apariciones de NameIdentifier con nombre.

**Modificar la rutina Post y CallGraphAPIOnBehalfOfUser()**

Copie y pegue el código siguiente en ToDoListController.cs y reemplace el código para registrar e CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
      if (!ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
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

## <a name="running-the-solution"></a>Ejecución de la solución


De forma predeterminada, visual studio está configurado para ejecutar un proyecto cuando se alcanza la depuración para ejecutar.

* Haga clic con el botón secundario en la solución y seleccione Propiedades.
* En la página de propiedades seleccione proyectos de inicio múltiples y cambiar la acción de inicio para las tres entradas.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Presione F5 y ejecutar la solución

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Haga clic en el botón de inicio de sesión. Se le pedirá que inicie sesión mediante AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Después de iniciar sesión, agregue un elemento de lista de tareas en la lista. En segundo plano que vamos a hacer una operación Post para el ToDoListService lo más hará una llamada Post a la API de web WebAPIOBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

En la operación correcta verá que el elemento se ha agregado a la lista con el mensaje adicional desde el back-end de API Web que se accede mediante el flujo de autenticación delegada.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

También puede ver los seguimientos detallados en Fiddler. Inicie Fiddler y habilitar el descifrado de HTTPS. Puede ver que hacemos dos solicitudes al punto de conexión /adfs/oautincludes.
En la primera interacción, se presenta el código de acceso al extremo de token y obtener un acceso token para https://localhost:44321/ ![OBO de AD FS](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

En la segunda interacción con el extremo de token, puede ver que tenemos **requested_token_use** establecer como **on_behalf_of** y usamos el token de acceso obtenido para el servicio web de nivel intermedio, es decir, https://localhost:44321/ como la aserción para obtener el token de on-behalf-of.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
