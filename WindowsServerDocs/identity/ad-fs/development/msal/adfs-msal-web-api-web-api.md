---
title: AD FS API Web de MSAL llamada API Web (en nombre de escenario)
description: Obtenga información sobre cómo crear una API Web que llame a otra API Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: d5289caaa272931f2e96d6fafb410e9861896dfc
ms.sourcegitcommit: fc2a7c69a74edcd79372054c4a9a24237510babd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98672977"
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

      ![Captura de pantalla de la Página principal del Asistente para agregar grupos de aplicaciones que muestra la aplicación nativa que tiene acceso a una plantilla de API Web resaltada.](media/adfs-msal-web-api-web-api/webapi2.png)

  3. Copie el valor del **identificador de cliente** . Se usará más adelante como valor de **ClientID** en el archivo de **App.config** de la aplicación. Escriba lo siguiente para el **URI de redirección:**  -  https://ToDoListClient . Haga clic en **Agregar**. Haga clic en **Siguiente**.

      ![Captura de pantalla de la página aplicación nativa del Asistente para agregar grupos de aplicaciones que muestra el redireccionamiento U R I.](media/adfs-msal-web-api-web-api/webapi3.png)

  4. En la pantalla configurar API Web, escriba el **identificador:** https://localhost:44321/ . Haga clic en **Agregar**. Haga clic en **Siguiente**. Este valor se usará más adelante en los archivos de **App.config** y **Web.Config** de la aplicación.

      ![Captura de pantalla de la página configurar API Web del Asistente para agregar grupo de aplicaciones que muestra el identificador correcto.](media/adfs-msal-web-api-web-api/webapi4.png)

  5. En la pantalla aplicar Directiva de Access Control, seleccione **permitir todos** y haga clic en **siguiente**.

      ![Captura de pantalla de la página elegir Directiva de Access Control del Asistente para agregar grupo de aplicaciones que muestra la opción permitir todos resaltada.](media/adfs-msal-web-api-web-api/webapi5.png)

  6. En la pantalla configurar permisos de aplicación, seleccione **OpenID** y **user_impersonation**. Haga clic en **Siguiente**.

      ![Captura de pantalla de la página configurar permisos de aplicación del Asistente para agregar grupo de aplicaciones que muestra la opción abrir I D seleccionada.](media/adfs-msal-web-api-web-api/webapi6.png)

  7. En la pantalla resumen, haga clic en **siguiente**.

  8. En la pantalla completa, haga clic en **cerrar**.


  9. En administración de AD FS, haga clic en **grupos de aplicaciones** y seleccione grupo de aplicaciones de **WebApiToWebApi** . Haga clic con el botón secundario y seleccione **Propiedades**.

      ![Captura de pantalla del cuadro de diálogo de administración de D F S que muestra el grupo WebApiToWebApi resaltado y la opción propiedades en la lista desplegable.](media/adfs-msal-web-api-web-api/webapi7.png)

  10. En la pantalla de propiedades de WebApiToWebApi, haga clic en **Agregar aplicación..**..

      ![Captura de pantalla del cuadro de diálogo Propiedades de WebApiToWebApi que muestra la aplicación WebApiToWebApi-web A P I enumerada.](media/adfs-msal-web-api-web-api/webapi8.png)

  11. En aplicaciones independientes, seleccione **aplicación de servidor**.

      ![Captura de pantalla de la Página principal del Asistente para agregar una nueva aplicación a WebApiToWebApi que muestra la opción de aplicación de servidor resaltada.](media/adfs-msal-web-api-web-api/webapi9.png)

  12. En la pantalla de la aplicación de servidor, agregue https://localhost:44321/ como **identificador de cliente** y **URI de redirección**.

      ![Captura de pantalla de la página aplicación de servidor del Asistente para agregar una nueva aplicación a WebApiToWebApi que muestra el identificador de cliente correcto y redirigir U R I.](media/adfs-msal-web-api-web-api/webapi10.png)

  13. En la pantalla configurar credenciales de aplicación, seleccione **generar un secreto compartido**. Copie el secreto para su uso posterior.

      ![Captura de pantalla de la página configurar las credenciales de aplicación del Asistente para agregar una nueva aplicación a WebApiToWebApi que muestra la opción generar un secreto compartido seleccionado y el secreto compartido generado resaltado.](media/adfs-msal-web-api-web-api/webapi11.png)

  14. En la pantalla resumen, haga clic en **siguiente**.

  15. En la pantalla completa, haga clic en **cerrar**.

  16. En administración de AD FS, haga clic en **grupos de aplicaciones** y seleccione grupo de aplicaciones de **WebApiToWebApi** . Haga clic con el botón secundario y seleccione **Propiedades**.

      ![Segunda captura de pantalla del cuadro de diálogo de administración de D F S que muestra el grupo WebApiToWebApi resaltado y la opción propiedades en la lista desplegable.](media/adfs-msal-web-api-web-api/webapi12.png)

  17. En la pantalla de propiedades de WebApiToWebApi, haga clic en **Agregar aplicación..**..

      ![Segunda captura de pantalla del cuadro de diálogo Propiedades de WebApiToWebApi que muestra la aplicación WebApiToWebApi-web A P I enumerada.](media/adfs-msal-web-api-web-api/webapi13.png)

  18. En aplicaciones independientes, seleccione **Web API**.

      ![Captura de pantalla de la Página principal del Asistente para agregar una nueva aplicación a WebApiToWebApi que muestra la opción Web a P I resaltada.](media/adfs-msal-web-api-web-api/webapi14.png)

  19. En configurar API Web, agregue https://localhost:44300 como **identificador**.

      ![Captura de pantalla de la página de configuración de la web a P I del Asistente para agregar una nueva aplicación a WebApiToWebApi que muestra la solicitud de redireccionamiento correcta U R I.](media/adfs-msal-web-api-web-api/webapi15.png)

  20. En la pantalla aplicar Directiva de Access Control, seleccione **permitir todos** y haga clic en **siguiente**.

      ![Captura de pantalla de la página elegir Directiva de Access Control del Asistente para agregar una nueva aplicación a WebApiToWebApi que muestra la opción permitir todos resaltada.](media/adfs-msal-web-api-web-api/webapi16.png)

  21. En la pantalla configurar permisos de aplicación, haga clic en **siguiente**.

      ![Captura de pantalla de la página configurar permisos de aplicación del Asistente para agregar una nueva aplicación a WebApiToWebApi que muestra la siguiente opción denominada.](media/adfs-msal-web-api-web-api/webapi17.png)

  22. En la pantalla resumen, haga clic en **siguiente**.

  23. En la pantalla completa, haga clic en **cerrar**.

  24. Haga clic en aceptar en WebApiToWebApi: pantalla de propiedades de Web API 2

  25. En la pantalla de propiedades de WebApiToWebApi, seleccione **WebApiToWebApi – Web API** y haga clic en **Editar..**..

      ![Captura de pantalla del cuadro de diálogo Propiedades de WebApiToWebApi que muestra la aplicación WebApiToWebApi-web A P I resaltada.](media/adfs-msal-web-api-web-api/webapi18.png)

  26. En WebApiToWebApi: pantalla de propiedades de la API Web, seleccione la pestaña **reglas de transformación de emisión** y haga clic en **Agregar regla.**...

      ![Captura de pantalla del cuadro de diálogo Propiedades de WebApiToWebApi-Web en la que se muestra la pestaña reglas de transformación de emisión.](media/adfs-msal-web-api-web-api/webapi19.png)

  27. En el Asistente para agregar regla de notificación de transformación, seleccione **enviar notificaciones mediante una regla personalizada en la** lista desplegable y haga clic en **siguiente**.

      ![Captura de pantalla de la página Seleccionar plantilla de regla del Asistente para agregar regla de notificación de transformación que muestra la opción enviar notificaciones mediante una regla personalizada seleccionada.](media/adfs-msal-web-api-web-api/webapi20.png)

  28. Escriba **PassAllClaims** en **nombre de regla de notificaciones:** campo y **x: [] => problema (Claim = x);** regla de notificaciones en regla personalizada: campo y haga clic en finalizar.

      ![Captura de pantalla de la página configurar regla del Asistente para agregar regla de notificaciones de transformación que muestra la configuración que se explicó anteriormente.](media/adfs-msal-web-api-web-api/webapi21.png)

  29. Haga clic en aceptar en WebApiToWebApi: pantalla de propiedades de la API Web

  30. En la pantalla de propiedades de WebApiToWebApi, seleccione Select WebApiToWebApi – Web API 2 y haga clic en Editar...</br>
  ![Captura de pantalla del cuadro de diálogo Propiedades de WebApiToWebApi que muestra la aplicación WebApiToWebApi-web A P I 2 resaltada.](media/adfs-msal-web-api-web-api/webapi22.png)

  31. En la pantalla WebApiToWebApi – propiedades de Web API 2, seleccione la pestaña reglas de transformación de emisión y haga clic en Agregar regla...

  32. En el Asistente para agregar regla de notificación de transformación, seleccione enviar notificaciones mediante una regla personalizada en la lista desplegable y haga clic en ![ la siguiente captura de pantalla de la página Seleccionar plantilla de regla del Asistente para agregar regla de notificación de transformación que muestra la opción enviar notificaciones mediante una regla personalizada seleccionada.](media/adfs-msal-web-api-web-api/webapi23.png)

  33. Escriba PassAllClaims en nombre de regla de notificaciones: campo y **x: [] => problema (Claim = x);** regla de notificaciones en **regla personalizada:** campo y haga clic en **Finalizar**.

      ![Segunda captura de pantalla de la página configurar regla del Asistente para agregar regla de notificaciones de transformación que muestra la configuración que se explicó anteriormente.](media/adfs-msal-web-api-web-api/webapi24.png)

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

            ![Captura de pantalla del archivo de configuración de la aplicación que muestra los valores modificados.](media/adfs-msal-web-api-web-api/webapi25.png)

  4. Abra el archivo de Web.config en ToDoListService. Modifique lo siguiente:
       - ida: Audience: escriba el valor del identificador de cliente de #12 en el registro de la aplicación en AD FS sección anterior
       - ida: ClientId: escriba el valor del identificador de cliente de #12 en el registro de la aplicación en AD FS sección anterior.
       - Ida: ClientSecret: escriba el secreto compartido copiado de #13 en el registro de la aplicación en AD FS sección anterior.
       - ida: RedirectUri: escriba el valor de RedirectUri de #12 en el registro de la aplicación en AD FS sección anterior.
       - ida: AdfsMetadataEndpoint: escriba https://[su nombre de host de AD FS]/federationmetadata/2007-06/federationmetadata.xml
       - ida: OBOWebAPIBase: escriba el valor del identificador de #19 en el registro de la aplicación en AD FS sección anterior.
       - ida: autoridad: escriba https://[su nombre de host de AD FS]/ADFS

          ![Captura de pantalla del archivo de configuración Web en ToDoListService que muestra los valores modificados.](media/adfs-msal-web-api-web-api/webapi26.png)

 5. Abra el archivo de Web.config en WebAPIOBO. Modifique lo siguiente:
       - ida: AdfsMetadataEndpoint: escriba https://[su nombre de host de AD FS]/federationmetadata/2007-06/federationmetadata.xml
       - ida: Audience: escriba el valor del identificador de cliente de #12 en el registro de la aplicación en AD FS sección anterior

          ![Captura de pantalla del archivo de configuración Web en WebAPIOBO que muestra los valores modificados.](media/adfs-msal-web-api-web-api/webapi27.png)

## <a name="test-the-sample"></a>Prueba del ejemplo

En esta sección se muestra cómo probar el ejemplo configurado anteriormente.

Una vez realizados los cambios en el código, vuelva a generar la solución

  1. En Visual Studio, haga clic con el botón derecho en la solución y seleccione **establecer proyectos de inicio..** .

      ![Captura de pantalla de la lista que aparece al hacer clic con el botón secundario en la solución con la opción establecer proyectos de inicio resaltada.](media/adfs-msal-web-api-web-api/webapi28.png)

  2. En las páginas de propiedades, asegúrese de que la **acción** está establecida en **iniciar** para cada uno de los proyectos, excepto TodoListSPA.

      ![Captura de pantalla del cuadro de diálogo páginas de propiedades de la solución que muestra la opción proyecto de inicio múltiple seleccionada y todas las acciones de los proyectos establecidas en iniciar.](media/adfs-msal-web-api-web-api/webapi29.png)

  3. En la parte superior de Visual Studio, haga clic en la flecha verde.

      ![Captura de pantalla de la interfaz de usuario de Visual Studio con la opción de inicio denominada.](media/adfs-msal-web-api-web-api/webapi30.png)

  4. En la pantalla principal de la aplicación nativa, haga clic en **iniciar sesión**.

      ![Captura de pantalla del cuadro de diálogo cliente de la lista de tareas pendientes.](media/adfs-msal-web-api-web-api/webapi31.png)

     Si no ve la pantalla de la aplicación nativa, busque y quite los archivos * msalcache. bin de la carpeta donde se guarda el repositorio del proyecto en el sistema.

  5. Se le redirigirá a la página de inicio de sesión de AD FS. Continúe e inicie sesión.

      ![Captura de pantalla de la página de inicio de sesión.](media/adfs-msal-web-api-web-api/webapi32.png)

  6. Una vez que haya iniciado sesión, escriba Text Web API to Web API Call en el **elemento crear una tarea pendiente**. Haga clic en **Agregar elemento**.  Esto llamará a la API Web (servicio de lista de tareas pendientes), que luego llama a la API Web 2 (WebAPIOBO) y agrega el elemento en la memoria caché.

      ![Captura de pantalla del cuadro de diálogo cliente de la lista de tareas con el nuevo elemento que se va a rellenar la sección elementos pendientes.](media/adfs-msal-web-api-web-api/webapi33.png)

 ## <a name="next-steps"></a>Pasos siguientes
[Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)


