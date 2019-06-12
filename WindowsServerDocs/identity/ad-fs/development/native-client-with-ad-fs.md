---
title: Crear a un cliente nativo de aplicación de uso de los clientes públicos de OAuth con AD FS 2016 o posterior
description: Un tutorial que proporciona instrucciones al crear a un cliente nativo de aplicación mediante clientes públicos de OAuth y AD FS 2016 o posterior
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 15bb6f1e39f64ff19ebb5515188ee944e277d3b7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445480"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>Crear a un cliente nativo de aplicación de uso de los clientes públicos de OAuth con AD FS 2016 o posterior

## <a name="overview"></a>Información general

En este artículo se muestra cómo crear una aplicación nativa que interactúa con una API Web protegido por AD FS 2016 o posterior.

1. .Net aplicación TodoListClient WPF usa Active Directory Authentication Library (ADAL) para obtener un token de acceso JWT de Azure Active Directory (Azure AD) a través del protocolo OAuth 2.0
2. El token de acceso se utiliza como un token de portador para autenticar al usuario al llamar al punto de conexión de Todolist de la API web TodoListService.
 Se utiliza el ejemplo de aplicación para Azure AD aquí y, a continuación, modificarlo para AD FS 2016 o posterior.

![Información geneal sobre la aplicación](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Requisitos previos
La siguiente es una lista de requisitos previos necesarios antes de completar este documento. Este documento se supone que se ha instalado AD FS y se ha creado una granja de AD FS.

* Herramientas de cliente de GitHub
* AD FS en Windows Server 2016 o posterior
* Visual Studio 2013 o posterior

## <a name="creating-the-sample-walkthrough"></a>Crear el tutorial de ejemplo

### <a name="create-the-application-group-in-ad-fs"></a>Cree el grupo de aplicaciones en AD FS

1. Administración de AD FS, haga doble clic en **grupos de aplicaciones** y seleccione **Agregar grupo de aplicaciones**.

2. En el Asistente para grupo de aplicaciones, como nombre, escriba cualquier nombre que prefiera, por ejemplo, NativeToDoListAppGroup. Seleccione el **aplicación nativa accediendo a una API web** plantilla. Haz clic en **Siguiente**.
 ![Agregar grupo de aplicaciones](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. En el **aplicación nativa** página, tenga en cuenta el identificador generado por AD FS. Este es el identificador con el que AD FS reconocerá la aplicación de cliente público. Copia el **identificador de cliente** valor. Se usará más adelante como el valor de **ida: ClientId** en el código de aplicación. Si lo desea, que puede dar cualquier identificador personalizado aquí. El URI de redireccionamiento es cualquier valor arbitrario, ejemplo, coloque https://ToDoListClient ![aplicación nativa](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. En el **configurar Web API** página, establezca el valor de identificador de la API Web. En este ejemplo, debe ser el valor de la **dirección URL de SSL** donde la aplicación Web se supone que se ejecuta. Puede obtener este valor haciendo clic en las propiedades del proyecto TooListServer en la solución. Esto se usará más adelante como el **todo:TodoListResourceId** valor en **App.config** archivo de la aplicación cliente nativa y también como el **todo:TodoListBaseAddress**.
![API Web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Vaya a través de la **aplicar directiva de Control de acceso** y **configurar permisos de la aplicación** con los valores predeterminados en su lugar. La página de resumen debería ser similar al siguiente.
![Resumen](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

Haga clic en siguiente y, a continuación, complete el asistente.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Agregue la notificación NameIdentifier a la lista de notificaciones emitidas
La aplicación de demostración usa el valor de la notificación NameIdentifier en varios lugares. A diferencia de Azure AD, AD FS no emite una notificación NameIdentifier de forma predeterminada. Por lo tanto, necesitamos agregar una regla de notificación para emitir la notificación NameIdentifier para que la aplicación puede utilizar el valor correcto. En este ejemplo, el nombre dado del usuario se emite como el valor NameIdentifier para el usuario en el token.
Para configurar la regla de notificación, abra el grupo de aplicaciones que acaba de crear y haga doble clic en la API Web. Seleccione la ficha reglas de transformación de emisión y, a continuación, haga clic en el botón Agregar regla. En el tipo de regla de notificación, elija la regla de notificación personalizada y, a continuación, agregue la regla de notificación, tal como se muestra a continuación.

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![Regla de notificación NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modificar el código de aplicación

En esta sección se explica cómo descargar el ejemplo de API Web y modificarla en Visual Studio.   Vamos a usar el ejemplo de Azure AD que se [aquí](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop).  

Para descargar el proyecto de ejemplo, use Git Bash y escriba lo siguiente:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>Modificar ToDoListClient

Este proyecto en la solución representa la aplicación cliente nativa. Es necesario asegurarse de que conoce la aplicación cliente:

1. ¿Dónde debe ir para obtener el usuario autenticado cuando sea necesario?
2. ¿Qué es el identificador que el cliente necesita proporcionar a la autoridad de autenticación (AD FS)?
3. ¿Qué es el identificador del recurso que se solicita el token de acceso?
4. ¿Qué es la dirección base de la API Web?

Se necesitan los siguientes cambios de código con el fin de obtener la información anterior a la aplicación cliente nativa.

**App.config**

* Agregue la clave **ida: autoridad** con el valor que representan el servicio AD FS. Por ejemplo, https://fs.contoso.com/adfs/
* Modificar **ida: ClientId** clave con el valor de **identificador de cliente** en el **aplicación nativa** página durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, 3f07368b-6efd-4f50-a330-d93853f4c855
* Modificar el **todo:todo:TodoListResourceId** con el valor de **identificador** en el **configurar Web API** página durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, https://localhost:44321/
* Modificar el **todo:TodoListBaseAddress** con el valor de **identificador** en el **configurar Web API** página durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, https://localhost:44321/
* Establezca el valor de **ida: RedirectUri** con el valor de **URI de redireccionamiento** en el **aplicación nativa** página durante la creación del grupo de aplicaciones en AD FS. Por ejemplo, https://ToDoListClient
* Para facilitar la lectura se puede quitar / comment la clave para **ida: inquilino** y **ida: AADInstance**.

  ![Configuración de la aplicación](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* Comente la línea para aadInstance como aparece a continuación

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* Agregue el valor de la entidad como la siguiente

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Elimine la línea para crear el **autoridad** valor desde aadInstance y de inquilinos

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* En la función **MainWindow**, cambiar la creación de instancias de authContext a

        authContext = new AuthenticationContext(authority,false);

    ADAL no admite la validación de AD FS como entidad y, por tanto, tenemos que pasar un indicador de valor false para el parámetro validateAuthority.

  ![Ventana principal](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>Modificar TodoListService
Dos archivos necesitan cambios en este proyecto: Web.config y Startup.Auth.cs. Para obtener los valores correctos de los parámetros, se requieren cambios de Web.Config. Se requieren cambios de Startup.Auth.cs para establecer la API Web para autenticarse en AD FS, en lugar de Azure AD.

**Web.config**

* Comentario de la clave **ida: inquilino** como no se necesita
* Agregue la clave para **ida: autoridad** con el valor que indica el FQDN de la federación de servicio, el ejemplo, https://fs.contoso.com/adfs/
* Modificar clave **ida: audiencia** con el valor del identificador de API Web que especificó en el **configurar Web API** página durante Agregar grupo de aplicaciones en AD FS.
* Agregar clave **ida: AdfsMetadataEndpoint** con el valor correspondiente a la dirección URL de metadatos de federación de AD FS servicio para p. ej.: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Configuración Web](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

Modifique la función configureauth esté como aparece a continuación

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

En esencia, vamos a configurar la autenticación para usar AD FS y proporcionar más información acerca de los metadatos de AD FS y para validar el token, la notificación de audiencia debe ser el valor esperado por la API Web.
Ejecutar la aplicación

1. En la solución NativeClient-DotNet, haga clic en y vaya a propiedades. Cambie el proyecto de inicio como se muestra a continuación para proyectos de inicio múltiples y establezca tanto TodoListClient como TodoListService de inicio.
![Propiedades de la solución](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  Presione el botón F5 o seleccione Depurar > continuar en la barra de menús. Esto iniciará la aplicación nativa y la API Web. Haga clic en el botón de inicio de sesión en la aplicación nativa y aparecerá un inicio de sesión interactivo desde AD AL y redirigir al servicio de AD FS. Escriba las credenciales de un usuario válido.
![Inicio de sesión](media/native-client-with-ad-fs-2016/sign-in.png)

En este paso, la aplicación nativa redirige a AD FS y tiene un token de identificador y un token de acceso para la API Web

3. Escriba un elemento en el cuadro de texto y haga clic en Agregar elemento. En este paso, la aplicación llega a la API Web para agregar el elemento de tareas pendientes y para ello, presentan el token de acceso a la API Web obtenida de AD FS. La API Web coincide con el valor de audiencia para asegurarse de que el token está destinado a él y comprueba la firma del token con la información de los metadatos de federación.

![Iniciar sesión](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
