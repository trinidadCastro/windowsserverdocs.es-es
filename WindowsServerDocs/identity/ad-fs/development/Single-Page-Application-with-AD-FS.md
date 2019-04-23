---
title: Cree una aplicación web de página única con OAuth y ADAL. JS con AD FS 2016
description: Un tutorial que proporciona instrucciones para realizar la autenticación con AD FS mediante ADAL para JavaScript proteger un AngularJS en función de aplicación de página única
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 78ab9f5d7c3e75650a4efb171d3b9281c56c63d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865306"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>Cree una aplicación web de página única con OAuth y ADAL. JS con AD FS 2016

>Se aplica a: Windows Server 2016

Este tutorial proporciona instrucciones para realizar la autenticación con AD FS mediante ADAL para proteger un AngularJS de JavaScript basado en aplicación una sola página, implementada con un back-end de ASP.NET Web API.

En este escenario, cuando el usuario inicia sesión, el código JavaScript de front-end usa [Active Directory Authentication Library para JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) y la concesión de autorización implícita para obtener un token de identificador (id_token) de Azure AD. El token se almacena en caché y el cliente lo adjunta a la solicitud como el token de portador al realizar llamadas a la API Web back-end, que está protegido mediante el middleware de OWIN.

>ADVERTENCIA: El ejemplo que se puede generar aquí es solo con fines educativos. Estas instrucciones son para la implementación más sencilla y más mínima posible exponer los elementos necesarios del modelo. El ejemplo no puede incluir todos los aspectos del control de errores y otros relacionados con la funcionalidad.

>[!NOTE]
>En este tutorial es aplicable **sólo** al servidor de AD FS 2016 y versiones posterior 

## <a name="overview"></a>Información general
En este ejemplo vamos a crear un flujo de autenticación que van a autenticar un cliente de aplicación de página única con AD FS para proteger el acceso a los recursos de API Web en el back-end. A continuación es el flujo general de autenticación


![Autorización de AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Cuando se usa una aplicación una sola página, el usuario navega a una ubicación de inicio, desde donde a partir de página y una colección de archivos de JavaScript y se cargan vistas HTML. Deberá configurar el Active Directory Authentication Library (ADAL) para conocer la información crítica acerca de la aplicación, es decir, la instancia de AD FS, Id. de cliente, para que pueda dirigir la autenticación en AD FS.

Si ADAL ve un desencadenador para la autenticación, usa la información proporcionada por la aplicación y dirige la autenticación en el STS de AD FS.  La aplicación de página única, que se registra como un cliente público en AD FS, se configura automáticamente para el flujo de concesión implícita. La solicitud de autorización da como resultado un token de identificador que se devuelve a la aplicación a través de un #fragment. Las llamadas subsiguientes a lo back-end de WebAPI llevar este token de identificador como el token de portador en el encabezado para obtener acceso a la API Web.

## <a name="setting-up-the-development-box"></a>Configurar el equipo del desarrollador
Este tutorial usa Visual Studio 2015. El proyecto usa la biblioteca de ADAL JS. Para obtener información sobre ADAL lea [Active Directory Authentication biblioteca de. NET.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configuración del entorno
En este tutorial vamos a usar una instalación básica de:

1.  DC: Controlador de dominio para el dominio en el que se hospedará AD FS
2.  Servidor de AD FS: El servidor de AD FS para el dominio
3.  Equipo de desarrollo: Máquina donde tiene instalado Visual Studio y a desarrollar nuestro ejemplo

Puede, si lo desea, usar solo dos máquinas. Uno para DC/AD FS y otro para el desarrollo de la muestra.

Cómo configurar el controlador de dominio y AD FS está fuera del ámbito de este artículo. Para información de implementación adicionales, vea:

- [Implementación de AD DS](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [Implementación de AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clone o descargue este repositorio
Vamos a usar la aplicación de ejemplo creada para la integración de Azure AD en una aplicación de página única de AngularJS y modificarlo en su lugar, proteger el recurso de back-end mediante AD FS.

En el shell o la línea de comandos:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Acerca del código
Los archivos de claves que contiene la lógica de autenticación son las siguientes:

**App.js** : inserta la dependencia del módulo ADAL, proporciona los valores de configuración de la aplicación usados ADAL para impulsar las interacciones de protocolo con AAD e indica qué rutas de acceso no deben realizarse sin autenticación anterior.

**index.HTML** -contiene una referencia a adal.js

**HomeController.js**-muestra cómo aprovechar las ventajas de los métodos login() y logOut() en ADAL.

**UserDataController.js** -muestra cómo extraer información de usuario de id_token en caché.

**Startup.Auth.cs** -contiene configuración para la API Web usar el servicio de federación de Active Directory para la autenticación de portador.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrar al cliente público en AD FS
En el ejemplo, la API Web está configurada para escuchar en https://localhost:44326/. El grupo de aplicaciones **explorador Web acceso a una aplicación web** se puede usar para configurar la aplicación de flujo de concesión implícita.

1. Abra la consola de administración de AD FS y haga clic en **Agregar grupo de aplicaciones**. En el **Asistente para agregar un grupo de aplicaciones** escriba el nombre de la aplicación, descripción y seleccione el **explorador Web acceso a una aplicación web** plantilla desde el **cliente / servidor aplicaciones** sección tal como se muestra a continuación  <br>![Crear nuevo grupo de aplicaciones](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. En la página siguiente **aplicación nativa**, proporcione el identificador de aplicación cliente y URI de redirección, como se muestra a continuación  <br>![Crear nuevo grupo de aplicaciones](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. En la página siguiente **aplicar directiva de Control de acceso** dejar los permisos como *permitir todos los usuarios*

4. La página de resumen un aspecto parecida al siguiente  <br>![Crear nuevo grupo de aplicaciones](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Haga clic en **siguiente** para completar la adición del grupo de aplicaciones y cerrar el asistente.

## <a name="modifying-the-sample"></a>Modificar el ejemplo
Configuración de ADAL JS

Abra el **app.js** y cambie el **adalProvider.init** definición para:

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|Configuración|Descripción
|--------|--------
|Instancia|La URL del STS, p. ej. https://fs.contoso.com/
|inquilino|Mantenerlo como está "adfs"
|clientID|Este es el identificador de cliente que especificó al configurar al cliente público para su aplicación de página única

## <a name="configure-webapi-to-use-ad-fs"></a>Configurar WebAPI para usar AD FS
Abra el **Startup.Auth.cs** en el ejemplo de archivo y agregue lo siguiente al principio: 

    using System.IdentityModel.Tokens;

eliminar:

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

y agregue:

    app.UseActiveDirectoryFederationServicesBearerAuthentication(
    new ActiveDirectoryFederationServicesBearerAuthenticationOptions
    {
    MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
    TokenValidationParameters = new TokenValidationParameters()
    {
    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"],
    ValidIssuer = ConfigurationManager.AppSettings["ida:Issuer"]
    }
    }
    );

|Parámetro|Descripción
|--------|--------
|ValidAudience|Esto configura el valor de 'audience' que va a proteger contra el token
|ValidIssuer|Esto configura el valor de ' emisor que se va a proteger contra el token
|MetadataEndpoint|Esto señala a la información de metadatos de STS

## <a name="add-application-configuration-for-ad-fs"></a>Agregar configuración de la aplicación de AD FS
Cambie la appsettings como sigue:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Ejecución de la solución
Limpiar la solución, vuelva a compilar la solución y ejecutarla. Si desea ver seguimientos detallados, inicie Fiddler y habilitar el descifrado de HTTPS.

El explorador cargará la SPA y aparecerá la siguiente pantalla:

![Registro del cliente](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Haga clic en Inicio de sesión.  La lista de tareas se desencadenará el flujo de autenticación y ADAL JS dirigirá la autenticación en AD FS

![inicio de sesión](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

En Fiddler puede ver el token que se devuelven como parte de la dirección URL en el fragmento de #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Podrá llamar a la API para agregar elementos de lista de tareas para el usuario ha iniciado la sesión de back-end:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
