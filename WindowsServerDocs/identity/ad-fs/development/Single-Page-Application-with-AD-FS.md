---
title: "Crear una aplicación web de página única con OAuth y ADAL.JS con AD FS de 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 7b3d48e1e38baffeb84b1f236efb43cfda5048c0
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>Crear una aplicación web de página única con OAuth y ADAL.JS con AD FS de 2016

>Se aplica a: Windows Server 2016

Este tutorial proporciona instrucciones para autenticar con AD FS mediante ADAL para JavaScript proteger una AngularJS basado en aplicación de la página individual, implementada con un back-end de ASP.NET Web API.

>Advertencia: El ejemplo que se puede generar aquí es solo con fines educativos. Estas instrucciones son para la implementación más simple, más mínima posible exponer los elementos necesarios del modelo. El ejemplo, no puedes incluir todos los aspectos del control de errores y otros relacionados con la funcionalidad.

>[!NOTE]
>Este tutorial es aplicable **solo** al servidor de AD FS 2016 y posterior 

## <a name="overview"></a>Introducción
En este ejemplo crearemos un flujo de autenticación que van a autenticar un cliente de aplicación única página contra AD FS para proteger el acceso a los recursos de WebAPI en el back-end. A continuación se incluye el flujo general de autenticación


![AD FS autorización](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Al usar una aplicación de la página, el usuario navega a una ubicación de inicio, desde donde a partir de página y una colección de archivos JavaScript y vistas HTML se cargan. Debes configurar la biblioteca de autenticación de Active Directory (ADAL) para saber la información crítica sobre la aplicación, es decir, la instancia de AD FS, el identificador de cliente, de modo que puede dirigir la autenticación a tu AD FS.

Si ADAL ve un desencadenador para la autenticación, usa la información proporcionada por la aplicación y dirige la autenticación en su STS de AD FS.  La aplicación de la página, que se registra como un cliente de AD FS público, se configura automáticamente para el flujo de concesión implícita. Los resultados de la solicitud de autorización en un token de identificador que se devuelve a la aplicación a través de un #fragment. Las llamadas adicionales al back-end WebAPI llevará este token de identificador que el token bearer en el encabezado para obtener acceso a la WebAPI.

## <a name="setting-up-the-development-box"></a>Configurar el cuadro de desarrollo
Este tutorial usa Visual Studio 2015. El proyecto usa la biblioteca de JS ADAL. Para obtener información sobre ADAL lee [Active Directory autenticación biblioteca de. NET.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configurar el entorno
Para este tutorial vamos a usar una instalación básica de:

1.  DC: Controlador de dominio del dominio en el que se hospedará AD FS
2.  Servidor de AD FS: El servidor de AD FS para el dominio
3.  El equipo de desarrollo: Equipo donde tienen instalado Visual Studio y se pueden desarrollar nuestra muestra

Puedes, si quieres, usar solo dos equipos. Uno para el controlador de dominio o AD FS y el otro para el desarrollo de la muestra.

Cómo configurar el controlador de dominio y AD FS queda fuera del ámbito de este artículo. Para información de implementación adicional, consulta:

- [Implementación de AD DS](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [Implementación de AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonar o descargar este repositorio
Vamos a usar la aplicación de muestra creada para la integración de Azure AD en una aplicación de la misma página AngularJS y modificarla para proteger en su lugar el recurso de back-end mediante AD FS.

Desde el shell o una línea de comandos:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Acerca del código
Los archivos de claves que contiene la lógica de autenticación son los siguientes:

**App.js** : inserta la dependencia de módulo ADAL, proporciona los valores de configuración de la aplicación usados por ADAL para impulsar las interacciones de protocolo con AAD e indica qué rutas de acceso no deben realizarse sin autenticación anterior.

**index.HTML** -contiene una referencia a adal.js

**HomeController.js**-muestra cómo sacar provecho de los métodos login() y logOut() en ADAL.

**UserDataController.js** -muestra cómo extraer la información de usuario de la id_token almacenado en caché.

**Startup.Auth.cs** -contiene configuración para el WebAPI usar servicios de federación de Active Directory para la autenticación de portador.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrar al cliente público en AD FS
En la muestra, el WebAPI está configurada para escuchar en https://localhost:44326 /. El grupo de aplicaciones **explorador Web acceso a una aplicación web** puede usarse para configurar la aplicación de flujo de concesión implícita.

1. Abre la consola de administración de AD FS y haz clic en **Agregar grupo de aplicaciones**. En la **Asistente para agregar un grupo de aplicación** escribe el nombre de la aplicación, descripción y selecciona el **explorador Web acceso a una aplicación web** plantilla desde el **aplicaciones de cliente / servidor** sección tal como se muestra a continuación
    <br>![Crear nuevo grupo de aplicación](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. En la siguiente página **aplicación nativa**, proporcionar el identificador de cliente de la aplicación y el URI de redirección, como se muestra a continuación
    <br>![Crear nuevo grupo de aplicación](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. En la siguiente página **aplicar la directiva de Control de acceso** dejar los permisos que *permitir todo el mundo*

4. La página de resumen debe ser similar a continuación
    <br>![Crear nuevo grupo de aplicación](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Haz clic en **siguiente** para completar la adición del grupo de aplicación y cerrar el asistente.

## <a name="modifying-the-sample"></a>Modificar la muestra
Configurar ADAL JS

Abre el **app.js** del archivo y cambiar la **adalProvider.init** definición para:

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
|instancia|La URL STS, por ejemplo, https://fs.contoso.com/
|inquilino|Guárdala como 'adfs'
|clientID|Este es el identificador de cliente que especificaste al configurar al cliente público de la aplicación de la página única

## <a name="configure-webapi-to-use-ad-fs"></a>Configurar WebAPI para usar AD FS
Abre el **Startup.Auth.cs** del archivo en la muestra y agrega lo siguiente al principio: 

    using System.IdentityModel.Tokens;

quitar:

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

y agrega:

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
|ValidAudience|Este modo configura el valor de 'público' que se va a comprobar en el token
|ValidIssuer|Este modo configura el valor de ' emisor que se va a comprobar en el token
|MetadataEndpoint|Apunta a la información de metadatos de su STS

## <a name="add-application-configuration-for-ad-fs"></a>Agregar la configuración de la aplicación de AD FS
Cambiar la appsettings lo siguiente:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Ejecuta la solución
Limpiar la solución, volver a compilar la solución y ejecutarla. Si quieres ver seguimientos detallados, iniciar Fiddler y habilitar descifrado HTTPS.

El explorador cargará la SPA y aparecerá la siguiente pantalla:

![Registro del cliente](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Haz clic en el inicio de sesión.  La lista de tareas pendientes se desencadenará el flujo de autenticación y JS ADAL dirigirá la autenticación en AD FS

![Inicio de sesión](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

En Fiddler puede ver el token que se devuelven como parte de la dirección URL en el fragmento #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Podrás llamar ahora el back-end API para agregar elementos de lista de tareas pendientes para que el usuario ha iniciado sesión:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Pasos siguientes
[AD FS desarrollo](../../ad-fs/AD-FS-Development.md)  
