---
title: Compilar una aplicación cliente nativa con clientes de OAuth público con AD FS 2016 o posterior
description: Un tutorial que proporciona instrucciones para crear una aplicación de cliente nativo mediante los clientes de OAuth público y AD FS 2016 o posterior.
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 2eb2d0a3cfa6763d308bbd73f1ccf50795b06e77
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866338"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>Compilar una aplicación cliente nativa con clientes de OAuth público con AD FS 2016 o posterior

## <a name="overview"></a>Información general

En este artículo se muestra cómo crear una aplicación nativa que interactúe con una API Web protegida por AD FS 2016 o posterior.

1. La aplicación WPF TodoListClient de .net usa el Biblioteca de autenticación de Active Directory (ADAL) para obtener un token de acceso de JWT de Azure Active Directory (Azure AD) a través del protocolo OAuth 2,0.
2. El token de acceso se usa como un token de portador para autenticar al usuario cuando se llama al punto de conexión/ToDoList de la API Web de TodoListService.
 Vamos a usar el ejemplo de aplicación para Azure AD aquí y, a continuación, modificarlo para AD FS 2016 o posterior.

![Overivew de aplicación](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Requisitos previos
A continuación se muestra una lista de los requisitos previos necesarios antes de completar este documento. En este documento se supone que se ha instalado AD FS y se ha creado una granja de AD FS.

* Herramientas de cliente de GitHub
* AD FS en Windows Server 2016 o posterior
* Visual Studio 2013 o posterior

## <a name="creating-the-sample-walkthrough"></a>Crear el tutorial de ejemplo

### <a name="create-the-application-group-in-ad-fs"></a>Cree el grupo de aplicaciones en AD FS

1. En administración de AD FS, haga clic con el botón derecho en **grupos de aplicaciones** y seleccione **Agregar grupo de aplicaciones**.

2. En el Asistente para grupos de aplicaciones, en nombre, escriba el nombre que prefiera, por ejemplo, NativeToDoListAppGroup. Seleccione la **aplicación nativa que tiene acceso a una plantilla de API Web** . Haga clic en **Next**.
 ![Agregar grupo de aplicaciones](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. En la página **aplicación nativa** , tenga en cuenta el identificador generado por AD FS. Este es el identificador con el que AD FS reconocerá la aplicación cliente pública. Copie el valor del **identificador de cliente** . Se usará más adelante como valor de **ida: ClientID** en el código de la aplicación. Si quiere, puede asignar aquí cualquier identificador personalizado. El URI de redirección es cualquier valor arbitrario, ejemplo, https://ToDoListClient Put ![ aplicación nativa](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. En la página **configurar API Web** , establezca el valor de identificador de la API Web. En este ejemplo, debe ser el valor de la **dirección URL de SSL** donde se supone que la aplicación web se está ejecutando. Para obtener este valor, haga clic en las propiedades del proyecto TooListServer en la solución. Se usará más adelante como el valor **todo: TodoListResourceId** en el archivo **app. config** de la aplicación cliente nativa y también como **todo: TodoListBaseAddress**.
![API Web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Recorra la **Directiva aplicar Access Control** y **Configure los permisos** de la aplicación con los valores predeterminados en su lugar. La página de resumen debe ser similar a la siguiente.
![Resumen](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

Haga clic en siguiente y complete el asistente.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Agregar la notificación NameIdentifier a la lista de notificaciones emitidas
La aplicación de demostración usa el valor de la demanda de NameIdentifier en varios lugares. A diferencia de Azure AD, AD FS no emite una declaración NameIdentifier de forma predeterminada. Por lo tanto, es necesario agregar una regla de notificaciones para emitir la solicitud NameIdentifier de modo que la aplicación pueda usar el valor correcto. En este ejemplo, el nombre de usuario especificado se emite como el valor de NameIdentifier para el usuario en el token.
Para configurar la regla de notificaciones, abra el grupo de aplicaciones que acaba de crear y haga doble clic en la API Web. Seleccione la pestaña reglas de transformación de emisión y haga clic en el botón Agregar regla. En el tipo de regla de notificaciones, elige regla de notificaciones personalizadas y después agrega la regla de notificaciones, como se muestra a continuación.

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![Regla de notificaciones de NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modificación del código de la aplicación

En esta sección se describe cómo descargar la API Web de ejemplo y modificarla en Visual Studio.   Vamos a usar el ejemplo Azure AD que se encuentra [aquí](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop).  

Para descargar el proyecto de ejemplo, use git bash y escriba lo siguiente:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>Modificar ToDoListClient

Este proyecto de la solución representa la aplicación cliente nativa. Necesitamos asegurarnos de que la aplicación cliente sepa:

1. ¿Adónde ir para que el usuario se autentique cuando sea necesario?
2. ¿Cuál es el identificador que el cliente debe proporcionar a la entidad de autenticación (AD FS)?
3. ¿Cuál es el identificador del recurso para el que se solicita el token de acceso?
4. ¿Cuál es la dirección base de la API Web?

Los siguientes cambios de código son necesarios para obtener la información anterior a la aplicación cliente nativa.

**App. config**

* Agregue la clave **ida: Authority** con el valor que describe el servicio AD FS. Por ejemplo, https://fs.contoso.com/adfs/
* Modifique la clave **ida: ClientID** con el valor de **identificador de cliente** en la página **aplicación nativa** durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, 3f07368b-6efd-4f50-A330-d93853f4c855
* Modifique **todo: todo: TodoListResourceId** con el valor de **identificador** en la página **configurar API Web** durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, https://localhost:44321/
* Modifique la página **todo: TodoListBaseAddress** con el valor de **identificador** en la página **configurar API Web** durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, https://localhost:44321/
* Establezca el valor de **ida: RedirectUri** con el valor de **URI de redirección** en la página **aplicación nativa** durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, https://ToDoListClient
* Para facilitar la lectura, puede quitar o comentar la clave de **ida: tenant** y **ida: AADInstance**.

  ![Configuración de la aplicación](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* Comente la línea para aadInstance como se indica a continuación

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* Agregue el valor de autoridad como se indica a continuación.

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Elimine la línea para crear el valor de la **entidad** de aadInstance y el inquilino.

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* En la función **MainWindow**, cambie la instancia de authContext a

        authContext = new AuthenticationContext(authority,false);

    ADAL no admite la validación de AD FS como autoridad y, por lo tanto, tenemos que pasar una marca de valor false para el parámetro validateAuthority.

  ![Ventana principal](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>Modificar TodoListService
Dos archivos necesitan cambios en este proyecto: Web. config y Startup.Auth.cs. Los cambios de Web. config son necesarios para obtener los valores correctos de los parámetros. Los cambios de Startup.Auth.cs son necesarios para establecer que WebAPI se autentique en AD FS en lugar de Azure AD.

**Web.config**

* Comentario de la clave **ida: tenant** , ya que no es necesaria
* Agregue la clave para **ida: Authority** con el valor que indica el FQDN del servicio de Federación, por ejemplo, https://fs.contoso.com/adfs/
* Modifique clave **ida: Audience** con el valor del identificador de la API Web que especificó en la página de configuración de la **API Web** durante el agregar grupo de aplicaciones en AD FS.
* Agregue la clave **ida: AdfsMetadataEndpoint** con el valor correspondiente a la dirección URL de metadatos de Federación del servicio AD FS, por ejemplo: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Configuración Web](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

Modifique la función ConfigureAuth como se indica a continuación

    public void ConfigureAuth(IAppBuilder app)
    {
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
    }

En esencia, configuramos la autenticación para usar AD FS y proporcionamos más información sobre los metadatos de AD FS y para validar el token, la demanda de audiencia debe ser el valor esperado por la API Web.
Ejecutar la aplicación

1. En la solución NativeClient-DotNet, haga clic con el botón derecho y vaya a propiedades. Cambie el proyecto de inicio como se muestra a continuación a varios proyectos de inicio y establezca TodoListClient y TodoListService en Start (iniciar).
![Propiedades de la solución](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  Presione el botón F5 o seleccione Depurar > continuar en la barra de menús. Esto iniciará la aplicación nativa y el WebAPI. Haga clic en el botón iniciar sesión de la aplicación nativa para que aparezca un inicio de sesión interactivo desde AD AL y redirija a su servicio de AD FS. Escriba las credenciales de un usuario válido.
![Inicio de sesión](media/native-client-with-ad-fs-2016/sign-in.png)

En este paso, la aplicación nativa se redirigió a AD FS y tenía un token de identificador y un token de acceso para la API Web.

3. Escriba un elemento pendiente en el cuadro de texto y haga clic en Agregar elemento. En este paso, la aplicación llega a la API Web para agregar el elemento de tareas pendientes y, para ello, presenta el token de acceso a la WebAPI obtenida de AD FS. La API Web coincide con el valor de la audiencia para asegurarse de que el token está destinado y comprueba la firma del token con la información de los metadatos de Federación.

![Iniciar sesión](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
