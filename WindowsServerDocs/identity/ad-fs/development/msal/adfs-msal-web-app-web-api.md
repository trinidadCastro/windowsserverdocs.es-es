---
title: AD FS aplicación Web de MSAL (aplicación de servidor) llamar a API Web
description: Obtenga información sobre cómo crear un usuario que inicie sesión en una aplicación web autenticado por AD FS 2019.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: e4f02400155dcb5899417b12e4c376fa3ee69fd0
ms.sourcegitcommit: fc2a7c69a74edcd79372054c4a9a24237510babd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98672957"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>Escenario: aplicación web (aplicación de servidor) llamada a Web API
>Se aplica a: AD FS 2019 y versiones posteriores

Obtenga información sobre cómo crear un usuario que inicie sesión en una aplicación web autenticado por AD FS 2019 y adquirir tokens mediante la [biblioteca MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) para llamar a las API Web.

Antes de leer este artículo, debe estar familiarizado con los [conceptos de AD FS](../ad-fs-openid-connect-oauth-concepts.md) y el flujo de concesión de código de [autorización](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)

## <a name="overview"></a>Información general

![Información general de la aplicación web que llama a la API Web](media/adfs-msal-web-app-web-api/webapp1.png)

En este flujo, agregará autenticación a la aplicación web (aplicación de servidor), que puede iniciar sesión a los usuarios y llamar a una API Web. Desde la aplicación Web, para llamar a la API Web, use el método de adquisición de tokens de [AcquireTokenByAuthorizationCode](/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder) de MSAL. Usará el flujo del código de autorización y almacenará el token adquirido en la caché de tokens. A continuación, el controlador adquirirá tokens de forma silenciosa de la caché cuando sea necesario. MSAL actualizará el token si es necesario.

Web Apps que llama a las API Web:


- Son aplicaciones cliente confidenciales.
- Esta es la razón por la que han registrado un secreto (secreto compartido de aplicación, certificado o cuenta de AD) con AD FS. Este secreto se pasa durante la llamada a AD FS para obtener un token.

Para comprender mejor cómo registrar una aplicación web en ADFS y configurarla para que adquiera tokens para llamar a una API Web, vamos a usar un ejemplo disponible [aquí](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi) y se le guiará por el registro de la aplicación y los pasos de configuración del código.


## <a name="pre-requisites"></a>Requisitos previos

- Herramientas de cliente de GitHub
- AD FS 2019 o posterior configurado y en ejecución
- Visual Studio 2013 o posterior.

## <a name="app-registration-in-ad-fs"></a>Registro de aplicaciones en AD FS
En esta sección se muestra cómo registrar la aplicación web como cliente confidencial y API Web como un usuario de confianza (RP) en AD FS.

  1. En administración de AD FS, haga clic con el botón derecho en **grupos de aplicaciones** y seleccione **Agregar grupo de aplicaciones**.
  2. En el Asistente para grupos de aplicaciones, en **nombre** , escriba **WebAppToWebApi** y en **aplicaciones cliente-servidor** seleccione la plantilla **aplicación de servidor que tiene acceso a una API Web** . Haga clic en **Siguiente**.

      ![Captura de pantalla de la Página principal del Asistente para agregar grupos de aplicaciones que muestra la aplicación de servidor que tiene acceso a una plantilla P I resaltada.](media/adfs-msal-web-app-web-api/webapp2.png)

  3. Copie el valor del **identificador de cliente** . Se usará más adelante como valor de **ida: ClientID** en el archivo de **Web.config** de aplicaciones. Escriba lo siguiente para el **URI de redirección:**  -  https://localhost:44326 . Haga clic en Agregar. Haga clic en **Siguiente**.

      ![Captura de pantalla de la página aplicación de servidor del Asistente para agregar grupos de aplicaciones que muestra el identificador de cliente correcto y redirigir U R I.](media/adfs-msal-web-app-web-api/webapp3.png)

  4. En la pantalla configurar credenciales de aplicación, active la casilla **generar un secreto compartido** y copie el secreto. Se usará más adelante como el valor de **ida: ClientSecret** en el archivo de **Web.config** de aplicaciones. Haga clic en **Siguiente**.

      ![Captura de pantalla de la página configurar las credenciales de aplicación del Asistente para agregar grupo de aplicaciones que muestra la opción generar un secreto compartido seleccionado y el secreto compartido generado rellenado.](media/adfs-msal-web-app-web-api/webapp4.png)

  5. En la pantalla configurar API Web, escriba el **identificador:** https://webapi . Haga clic en **Agregar**. Haga clic en **Siguiente**. Este valor se usará más adelante para **ida: GraphResourceId** en el archivo de **Web.config** de aplicaciones.

      ![Captura de pantalla de la página configurar API Web del Asistente para agregar grupo de aplicaciones que muestra el identificador correcto.](media/adfs-msal-web-app-web-api/webapp5.png)

  6. En la pantalla aplicar Directiva de Access Control, seleccione **permitir** todos y haga clic en **siguiente**.

      ![Captura de pantalla de la página elegir Directiva de Access Control del Asistente para agregar grupo de aplicaciones que muestra la opción permitir todos resaltada.](media/adfs-msal-web-app-web-api/webapp6.png)

  7. En la pantalla configurar permisos de aplicación, asegúrese de que **OpenID** y **user_impersonation** estén seleccionados y haga clic en **siguiente**.

      ![Captura de pantalla de la página configurar permisos de aplicación del Asistente para agregar grupo de aplicaciones que muestra las opciones abrir I D y suplantación de usuario seleccionadas.](media/adfs-msal-web-app-web-api/webapp7.png)

  8. En la pantalla resumen, haga clic en **siguiente**.

  9. En la pantalla completa, haga clic en **cerrar**.



## <a name="code-configuration"></a>Configuración del código

En esta sección se muestra cómo configurar una aplicación Web de ASP.NET para iniciar sesión en el usuario y recuperar el token para llamar a la API Web.

  1. Descargue el ejemplo [aquí](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)

  2. Abrir el ejemplo con Visual Studio

  3. Abra el archivo de web.config. Modifique lo siguiente:
       - ida: ClientId: escriba el valor del **identificador de cliente** de #3 en el registro de la aplicación en AD FS sección anterior.
       - ida: ClientSecret: escriba el valor del **secreto** en #4 en el registro de la aplicación en AD FS sección anterior.
       - ida: RedirectUri: escriba el valor de **URI de redirección** de #3 en el registro de la aplicación en AD FS sección anterior.
       - ida: autoridad: escriba https://[su nombre de host de AD FS]/ADFS. Por ejemplo, https://adfs.contoso.com/adfs
       - ida: Resource: escriba el valor del **identificador** de #5 en el registro de la aplicación en AD FS sección anterior.

          ![Captura de pantalla del archivo de configuración web que muestra los valores modificados.](media/adfs-msal-web-app-web-api/webapp8.png)


### <a name="test-the-sample"></a>Prueba del ejemplo
En esta sección se muestra cómo probar el ejemplo configurado anteriormente.

  1. Una vez realizados los cambios en el código, vuelva a generar la solución

  2. En la parte superior de Visual Studio, asegúrese de que Internet Explorer está seleccionado y haga clic en la flecha verde.

      ![Captura de pantalla de la interfaz de usuario de Visual Studio con la opción IIS Express (Internet Explorer) denominada.](media/adfs-msal-web-app-web-api/webapp9.png)

  3. En la Página principal, haga clic en iniciar sesión.

      ![Captura de pantalla de la Página principal con la opción de inicio de sesión denominada.](media/adfs-msal-web-app-web-api/webapp10.png)

  4. Se le redirigirá a la página de inicio de sesión de AD FS. Continúe e inicie sesión.

      ![Captura de pantalla de la página de inicio de sesión.](media/adfs-msal-web-app-web-api/webapp11.png)

  5. Una vez que haya iniciado sesión, haga clic en token de acceso.

      ![Captura de pantalla de la Página principal con la opción de token de acceso denominada.](media/adfs-msal-web-app-web-api/webapp12.png)

  6. Al hacer clic en token de acceso, se obtiene la información del token de acceso mediante una llamada a la API Web.

      ![Captura de pantalla de la página token de acceso que muestra la información del token de acceso.](media/adfs-msal-web-app-web-api/webapp13.png)

 ## <a name="next-steps"></a>Pasos siguientes
[Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)

