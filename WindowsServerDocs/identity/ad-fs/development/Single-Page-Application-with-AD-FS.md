---
title: Compilar una aplicación Web de una sola página mediante OAuth y ADAL.JS con AD FS 2016 o posterior
description: Un tutorial en el que se proporcionan instrucciones para la autenticación en AD FS el uso de ADAL para JavaScript para proteger una aplicación de una sola página basada en AngularJS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: 09b789937c9ff1dad90c3533616a4ed800204267
ms.sourcegitcommit: 046123d4f2d24dc00b35ea99adee6f8d322c76bf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85416298"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>Compilar una aplicación Web de una sola página mediante OAuth y ADAL.JS con AD FS 2016 o posterior

En este tutorial se proporcionan instrucciones para la autenticación en AD FS mediante ADAL para JavaScript para proteger una aplicación de una sola página basada en AngularJS, implementada con un back-end de ASP.NET Web API.

En este escenario, cuando el usuario inicia sesión, el front-end de JavaScript usa la [biblioteca de autenticación de Active Directory para JavaScript (ADAL.JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) y la concesión de autorización implícita para obtener un token de identificación (id_token) de Azure AD. El token se almacena en caché y el cliente lo adjunta a la solicitud como token de portador al realizar llamadas al back-end de la API web, que está protegido con el middleware de OWIN.

>[!IMPORTANT]
>El ejemplo que se puede compilar aquí es solo con fines educativos. Estas instrucciones son para la implementación más sencilla y mínima posible para exponer los elementos necesarios del modelo. El ejemplo no puede incluir todos los aspectos del control de errores y otras funcionalidades relacionadas.

>[!NOTE]
>Este tutorial **solo** es aplicable a AD FS Server 2016 y versiones posteriores. 

## <a name="overview"></a>Información general
En este ejemplo, vamos a crear un flujo de autenticación en el que un cliente de aplicación de una sola página se autenticará en AD FS para proteger el acceso a los recursos de WebAPI en el back-end. A continuación se muestra el flujo de autenticación general


![Autorización de AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Cuando se usa una aplicación de una sola página, el usuario navega a una ubicación de inicio, desde donde la página de inicio y una colección de archivos JavaScript y vistas HTML se cargan. Debe configurar el Biblioteca de autenticación de Active Directory (ADAL) para conocer la información crítica sobre la aplicación, es decir, la instancia de AD FS, el identificador de cliente, para que pueda dirigir la autenticación a su AD FS.

Si ADAL ve un desencadenador para la autenticación, usa la información proporcionada por la aplicación y dirige la autenticación a su AD FS STS.  La aplicación de una sola página, que se registra como un cliente público en AD FS, se configura automáticamente para el flujo de concesión implícita. La solicitud de autorización genera un token de identificador que se devuelve a la aplicación a través de un #fragment. Otras llamadas al back-end de WebAPI llevarán este token de identificador como el token de portador en el encabezado para obtener acceso a WebAPI.

## <a name="setting-up-the-development-box"></a>Configuración del cuadro de desarrollo
En este tutorial se usa Visual Studio 2015. El proyecto usa la biblioteca de ADAL JS. Para obtener información sobre ADAL, lea [biblioteca de autenticación de Active Directory .net.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configuración del entorno
En este tutorial, usaremos una configuración básica de:

1.    DC: controlador de dominio para el dominio en el que se hospedará AD FS
2.    AD FS Server: el servidor de AD FS para el dominio
3.    Máquina de desarrollo: equipo en el que tenemos instalado Visual Studio y que va a desarrollar nuestro ejemplo

Si lo desea, puede usar solo dos máquinas. Uno para DC/AD FS y otro para desarrollar el ejemplo.

Cómo configurar el controlador de dominio y AD FS está fuera del ámbito de este artículo. Para obtener información adicional sobre la implementación, consulte:

- [AD DS Deployment](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Implementación de AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonación o descarga de este repositorio
Vamos a usar la aplicación de ejemplo creada para integrar Azure AD en una aplicación de página única de AngularJS y modificarla para proteger en su lugar el recurso de back-end mediante AD FS.

Desde el shell o la línea de comandos, ejecute:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Acerca del código
Los archivos de clave que contienen la lógica de autenticación son los siguientes:

**App.js** : inserta la dependencia del módulo Adal, proporciona los valores de configuración de la aplicación usados por Adal para conducir las interacciones de protocolo con AAD e indica a qué rutas no se debe tener acceso sin la autenticación anterior.

**index.html** : contiene una referencia a adal.js

**HomeController.js**: muestra cómo sacar partido de los métodos login () y logOut () en Adal.

**UserDataController.js** : muestra cómo extraer información de usuario de la ID_token almacenada en caché.

**Startup.auth.CS** : contiene la configuración de WebAPI para usar Active Directory servicio de Federación para la autenticación del portador.

## <a name="registering-the-public-client-in-ad-fs"></a>Registro del cliente público en AD FS
En el ejemplo, WebAPI se configura para escuchar en https://localhost:44326/ . El explorador Web del grupo de aplicaciones que **tiene acceso a una aplicación web** se puede usar para configurar la aplicación de flujo de concesión implícita.

1. Abra la consola de administración de AD FS y haga clic en **Agregar grupo de aplicaciones**. En el **Asistente para agregar grupos de aplicaciones** , escriba el nombre de la aplicación, Description y seleccione el **explorador Web que tiene acceso a una plantilla de aplicación web** en la sección **aplicaciones cliente-servidor** , como se muestra a continuación

    ![Crear nuevo grupo de aplicaciones](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. En la **aplicación nativa**de la página siguiente, proporcione el identificador de cliente de la aplicación y el URI de redireccionamiento, como se muestra a continuación

    ![Crear nuevo grupo de aplicaciones](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. En la página siguiente, **aplique la directiva Access Control** dejar los permisos como *permitir todos*

4. La página de resumen debe ser similar a la siguiente

    ![Crear nuevo grupo de aplicaciones](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Haga clic en **siguiente** para completar la adición del grupo de aplicaciones y cierre el asistente.

## <a name="modifying-the-sample"></a>Modificar el ejemplo
Configuración de ADAL JS

Abra el archivo **app.js** y cambie la definición de **adalProvider.init** a:

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
    );

|Configuración|Descripción|
|--------|--------|
|instance|Su dirección URL de STS, por ejemplo,https://fs.contoso.com/|
|tenant|Guárdela como ' ADFS '|
|clientID|Este es el identificador de cliente que especificó al configurar el cliente público para la aplicación de una sola página.|

## <a name="configure-webapi-to-use-ad-fs"></a>Configurar WebAPI para usar AD FS
Abra el archivo **Startup.auth.CS** en el ejemplo y agregue lo siguiente al principio:

    using System.IdentityModel.Tokens;

retirar

    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        }
    );

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

|Parámetro|Descripción|
|--------|--------|
|ValidAudience|Esto configura el valor de ' Audience ' que se comprobará en el token|
|ValidIssuer|Esto configura el valor de ' issuer que se comprobará en el token|
|MetadataEndpoint|Esto apunta a la información de metadatos de su STS|

## <a name="add-application-configuration-for-ad-fs"></a>Agregar configuración de aplicación para AD FS
Cambie el appSettings como se indica a continuación:
```xml
<appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
</appSettings>
```

## <a name="running-the-solution"></a>Ejecutar la solución
Limpie la solución, vuelva a compilar la solución y ejecútela. Si desea ver seguimientos detallados, inicie Fiddler y habilite el descifrado HTTPS.

El explorador (use chrome browser) cargará el SPA y aparecerá la siguiente pantalla:

![Registrar el cliente](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Haga clic en Inicio de sesión.  La lista de tareas desencadenará el flujo de autenticación y ADAL JS dirigirá la autenticación a AD FS

![Iniciar sesión](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

En Fiddler, puede ver el token que se devuelve como parte de la dirección URL en el # Fragment.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Ahora podrá llamar a la API de back-end para agregar elementos de la lista de tareas pendientes para el usuario que ha iniciado sesión:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
