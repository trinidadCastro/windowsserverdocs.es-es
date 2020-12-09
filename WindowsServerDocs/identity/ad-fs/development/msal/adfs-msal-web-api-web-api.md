---
title: AD FS API Web de MSAL llamada API Web (en nombre de escenario)
description: Obtenga información sobre cómo crear una API Web que llame a otra API Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: a8d3bb8c7094316dc8b54430137cac0c4fa7c75e
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866424"
---
# <a name="scenario-web-api-calling-web-api-on-behalf-of-scenario"></a>Escenario: API Web que llama a la API Web (en nombre de escenario)
> Se aplica a: AD FS 2019 y versiones posteriores

Obtenga información sobre cómo crear una API Web que llama a otra API Web en nombre del usuario.

Antes de leer este artículo, debe estar familiarizado con los [conceptos de AD FS](../ad-fs-openid-connect-oauth-concepts.md) y el [flujo Behalf_Of](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#on-behalf-of-flow)

## <a name="overview"></a>Información general


- Un cliente (aplicación web), no representado en el diagrama siguiente, llama a una API Web protegida y proporciona un token de portador JWT en su encabezado HTTP "Authorization".
- La API Web protegida valida el token y usa el método [AcquireTokenOnBehalfOf](/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync#Microsoft_IdentityModel_Clients_ActiveDirectory_AuthenticationContext_AcquireTokenAsync_System_String_Microsoft_IdentityModel_Clients_ActiveDirectory_ClientCredential_Microsoft_IdentityModel_Clients_ActiveDirectory_UserAssertion_)de MSAL   para solicitar (desde AD FS) otro token, de modo que se pueda llamar a una segunda API Web (llamada API Web de bajada) en nombre del usuario.
- La API web protegida usa este token para llamar a una API de bajada. También puede llamar a AcquireTokenSilentlater para solicitar tokens para otras API de nivel inferior (pero todavía en nombre del mismo usuario).AcquireTokenSilent actualiza el token cuando sea necesario.

     ![Información general](media/adfs-msal-web-api-web-api/webapi1.png)

Para comprender mejor cómo configurar en nombre del escenario de autenticación en ADFS, vamos a usar un ejemplo disponible [aquí](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof) y guiaremos en el registro de la aplicación y los pasos de configuración del código.

## <a name="pre-requisites"></a>Requisitos previos

- Herramientas de cliente de GitHub
- AD FS 2019 o posterior configurado y en ejecución
- Visual Studio 2013 o posterior.

## <a name="app-registration-in-ad-fs"></a>Registro de aplicaciones en AD FS

En esta sección se muestra cómo registrar la aplicación nativa como un cliente público y las API Web como usuarios de confianza (RP) en AD FS

  1. En administración de AD FS, haga clic con el botón derecho en **grupos de aplicaciones** y seleccione **Agregar grupo de aplicaciones**.

  2. En el Asistente para grupos de aplicaciones, en **nombre** , escriba **WebApiToWebApi** y en **aplicaciones cliente-servidor** seleccione la plantilla **aplicación nativa que tiene acceso a una API Web** . Haga clic en **Siguiente**.

      ![Registro de aplicaciones](media/adfs-msal-web-api-web-api/webapi2.png)

  3. Copie el valor del **identificador de cliente** . Se usará más adelante como valor de **ClientID** en el archivo de **App.config** de la aplicación. Escriba lo siguiente para el **URI de redirección:**  -  https://ToDoListClient . Haga clic en **Agregar**. Haga clic en **Siguiente**.

      ![Registro de aplicaciones](media/adfs-msal-web-api-web-api/webapi3.png)

  4. En la pantalla configurar API Web, escriba el **identificador:** https://localhost:44321/ . Haga clic en **Agregar**. Haga clic en **Siguiente**. Este valor se usará más adelante en los archivos de **App.config** y **Web.Config** de la aplicación.

      ![Registro de aplicaciones](media/adfs-msal-web-api-web-api/webapi4.png)

  5. En la pantalla aplicar Directiva de Access Control, seleccione **permitir todos** y haga clic en **siguiente**.

      ![Registro de aplicaciones](media/adfs-msal-web-api-web-api/webapi5.png)

  6. En la pantalla configurar permisos de aplicación, seleccione **OpenID** y **user_impersonation**. Haga clic en **Siguiente**.

      ![Registro de aplicaciones](media/adfs-msal-web-api-web-api/webapi6.png)

  7. En la pantalla resumen, haga clic en **siguiente**.

  8. En la pantalla completa, haga clic en **cerrar**.


  9. En administración de AD FS, haga clic en **grupos de aplicaciones** y seleccione grupo de aplicaciones de **WebApiToWebApi** . Haga clic con el botón secundario y seleccione **Propiedades**.

      ![Registro de aplicaciones](media/adfs-msal-web-api-web-api/webapi7.png)

  10. En la pantalla de propiedades de WebApiToWebApi, haga clic en **Agregar aplicación..**..

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi8.png)

  11. En aplicaciones independientes, seleccione **aplicación de servidor**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi9.png)

  12. En la pantalla de la aplicación de servidor, agregue https://localhost:44321/ como **identificador de cliente** y **URI de redirección**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi10.png)

  13. En la pantalla configurar credenciales de aplicación, seleccione **generar un secreto compartido**. Copie el secreto para su uso posterior.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi11.png)

  14. En la pantalla resumen, haga clic en **siguiente**.

  15. En la pantalla completa, haga clic en **cerrar**.

  16. En administración de AD FS, haga clic en **grupos de aplicaciones** y seleccione grupo de aplicaciones de **WebApiToWebApi** . Haga clic con el botón secundario y seleccione **Propiedades**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi12.png)

  17. En la pantalla de propiedades de WebApiToWebApi, haga clic en **Agregar aplicación..**..

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi13.png)

  18. En aplicaciones independientes, seleccione **Web API**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi14.png)

  19. En configurar API Web, agregue https://localhost:44300 como **identificador**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi15.png)

  20. En la pantalla aplicar Directiva de Access Control, seleccione **permitir todos** y haga clic en **siguiente**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi16.png)

  21. En la pantalla configurar permisos de aplicación, haga clic en **siguiente**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi17.png)

  22. En la pantalla resumen, haga clic en **siguiente**.

  23. En la pantalla completa, haga clic en **cerrar**.

  24. Haga clic en aceptar en WebApiToWebApi: pantalla de propiedades de Web API 2

  25. En la pantalla de propiedades de WebApiToWebApi, seleccione **WebApiToWebApi – Web API** y haga clic en **Editar..**..

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi18.png)

  26. En WebApiToWebApi: pantalla de propiedades de la API Web, seleccione la pestaña **reglas de transformación de emisión** y haga clic en **Agregar regla.**...

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi19.png)

  27. En el Asistente para agregar regla de notificación de transformación, seleccione **enviar notificaciones mediante una regla personalizada en la** lista desplegable y haga clic en **siguiente**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi20.png)

  28. Escriba **PassAllClaims** en **nombre de regla de notificaciones:** campo y **x: [] => problema (Claim = x);** regla de notificaciones en regla personalizada: campo y haga clic en finalizar.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi21.png)

  29. Haga clic en aceptar en WebApiToWebApi: pantalla de propiedades de la API Web

  30. En la pantalla de propiedades de WebApiToWebApi, seleccione Select WebApiToWebApi – Web API 2 y haga clic en Editar...</br>
  ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi22.png)

  31. En la pantalla WebApiToWebApi – propiedades de Web API 2, seleccione la pestaña reglas de transformación de emisión y haga clic en Agregar regla...

  32. En el Asistente para agregar regla de notificación de transformación, seleccione enviar notificaciones mediante una regla personalizada en dopdown y haga clic en siguiente de la ![ aplicación reg.](media/adfs-msal-web-api-web-api/webapi23.png)

  33. Escriba PassAllClaims en nombre de regla de notificaciones: campo y **x: [] => problema (Claim = x);** regla de notificaciones en **regla personalizada:** campo y haga clic en **Finalizar**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi24.png)

  34.  Haga clic en aceptar en WebApiToWebApi: pantalla de propiedades de Web API 2 y, a continuación, en la pantalla de propiedades de WebApiToWebApi.


## <a name="code-configuration"></a>Configuración del código

En esta sección se muestra cómo configurar una API Web para llamar a otra API Web.

  1. Descargue el ejemplo [aquí](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)

  2. Abrir el ejemplo con Visual Studio

  3. Abra el archivo App.config. Modifique lo siguiente:
       - ida: autoridad: escriba https://[su nombre de host de AD FS]/ADFS/
       - ida: ClientId: escriba el valor de #3 en el registro de la aplicación en AD FS sección anterior.
       - ida: RedirectUri: escriba el valor de #3 en el registro de la aplicación en AD FS sección anterior.
       - todo: TodoListResourceId: escriba el valor del identificador de #4 en el registro de la aplicación en AD FS sección anterior
       - ida: todo: TodoListBaseAddress: escriba el valor del identificador de #4 en el registro de la aplicación en AD FS sección anterior.

            ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi25.png)

  4. Abra el archivo de Web.config en ToDoListService. Modifique lo siguiente:
       - ida: Audience: escriba el valor del identificador de cliente de #12 en el registro de la aplicación en AD FS sección anterior
       - ida: ClientId: escriba el valor del identificador de cliente de #12 en el registro de la aplicación en AD FS sección anterior.
       - Ida: ClientSecret: escriba el secreto compartido copiado de #13 en el registro de la aplicación en AD FS sección anterior.
       - ida: RedirectUri: escriba el valor de RedirectUri de #12 en el registro de la aplicación en AD FS sección anterior.
       - ida: AdfsMetadataEndpoint: escriba https://[su nombre de host de AD FS]/federationmetadata/2007-06/federationmetadata.xml
       - ida: OBOWebAPIBase: escriba el valor del identificador de #19 en el registro de la aplicación en AD FS sección anterior.
       - ida: autoridad: escriba https://[su nombre de host de AD FS]/ADFS

          ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi26.png)

 5. Abra el archivo de Web.config en WebAPIOBO. Modifique lo siguiente:
       - ida: AdfsMetadataEndpoint: escriba https://[su nombre de host de AD FS]/federationmetadata/2007-06/federationmetadata.xml
       - ida: Audience: escriba el valor del identificador de cliente de #12 en el registro de la aplicación en AD FS sección anterior

          ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi27.png)

## <a name="test-the-sample"></a>Prueba del ejemplo

En esta sección se muestra cómo probar el ejemplo configurado anteriormente.

Una vez realizados los cambios en el código, vuelva a generar la solución

  1. En Visual Studio, haga clic con el botón derecho en la solución y seleccione **establecer proyectos de inicio..** .

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi28.png)

  2. En las páginas de propiedades, asegúrese de que la **acción** está establecida en **iniciar** para cada uno de los proyectos, excepto TodoListSPA.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi29.png)

  3. En la parte superior de Visual Studio, haga clic en la flecha verde.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi30.png)

  4. En la pantalla principal de la aplicación nativa, haga clic en **iniciar sesión**.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi31.png)

     Si no ve la pantalla de la aplicación nativa, busque y quite los archivos * msalcache. bin de la carpeta donde se guarda el repositorio del proyecto en el sistema.

  5. Se le redirigirá a la página de inicio de sesión de AD FS. Continúe e inicie sesión.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi32.png)

  6. Una vez que haya iniciado sesión, escriba Text Web API to Web API Call en el **elemento crear una tarea pendiente**. Haga clic en **Agregar elemento**.  Esto llamará a la API Web (servicio de lista de tareas pendientes), que luego llama a la API Web 2 (WebAPIOBO) y agrega el elemento en la memoria caché.

      ![REG. de aplicación](media/adfs-msal-web-api-web-api/webapi33.png)

 ## <a name="next-steps"></a>Pasos siguientes
[Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)


