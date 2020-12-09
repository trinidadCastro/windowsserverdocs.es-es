---
title: AD FS API Web de llamada de aplicaciones nativas MSAL
description: Obtenga cómo crear un usuario de sesión de aplicación nativa autenticado por AD FS 2019 y adquirir tokens mediante la biblioteca MSAL para llamar a las API Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.openlocfilehash: cf293b6732a29a173050bcf90ed8cbec4e1d9ea4
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865784"
---
# <a name="scenario-native-app-calling-web-api"></a>Escenario: aplicación nativa que llama a la API Web
>Se aplica a: AD FS 2019 y versiones posteriores

Obtenga información sobre cómo crear un usuario de sesión de aplicación nativa autenticado por AD FS 2019 y adquirir tokens mediante la [biblioteca MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)  para llamar a las API Web.

Antes de leer este artículo, debe estar familiarizado con los [conceptos de AD FS](../ad-fs-openid-connect-oauth-concepts.md) y el flujo de concesión de código de [autorización](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)

## <a name="overview"></a>Información general

 ![Información general](media/adfs-msal-native-app-web-api/native1.png)

En este flujo, agregará autenticación a la aplicación nativa (cliente público), que puede iniciar sesión a los usuarios y llamar a una API Web. Para llamar a una API Web desde una aplicación nativa que inicia la sesión de los usuarios, puede usar el método de adquisición de tokens de [AcquireTokenInteractive](/dotnet/api/microsoft.identity.client.ipublicclientapplication.acquiretokeninteractive#Microsoft_Identity_Client_IPublicClientApplication_AcquireTokenInteractive_System_Collections_Generic_IEnumerable_System_String__) de MSAL. Para habilitar esta interacción, MSAL aprovecha un explorador web.

Para comprender mejor cómo configurar una aplicación nativa en ADFS para adquirir el token de acceso de forma interactiva, vamos a usar un ejemplo disponible [aquí](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) y se guiará por el registro de la aplicación y los pasos de configuración del código.


## <a name="pre-requisites"></a>Requisitos previos

- Herramientas de cliente de GitHub
- AD FS 2019 o posterior configurado y en ejecución
- Visual Studio 2013 o posterior.

## <a name="app-registration-in-ad-fs"></a>Registro de aplicaciones en AD FS
En esta sección se muestra cómo registrar la aplicación nativa como cliente público y API Web como un usuario de confianza (RP) en AD FS

  1. En **Administración de AD FS**, haga clic con el botón derecho en **grupos de aplicaciones** y seleccione **Agregar grupo de aplicaciones**.

  2. En el Asistente para grupos de aplicaciones, en **nombre** , escriba **NativeAppToWebApi** y en **aplicaciones cliente-servidor** seleccione la plantilla **aplicación nativa que tiene acceso a una API Web** . Haga clic en **Siguiente**.

      ![REG. de aplicación](media/adfs-msal-native-app-web-api/native2.png)

  3. Copie el valor del **identificador de cliente** . Se usará más adelante como valor de **ClientID** en el archivo de **App.config** de la aplicación. Escriba lo siguiente para el **URI de redirección:** https://ToDoListClient . Haga clic en **Agregar**. Haga clic en **Siguiente**.

     ![REG. de aplicación](media/adfs-msal-native-app-web-api/native3.png)

  4. En la pantalla configurar API Web, escriba el **identificador:** https://localhost:44321/ . Haga clic en **Agregar**. Haga clic en **Siguiente**. Este valor se usará más adelante en los archivos de **App.config** y **Web.config** de la aplicación.

     ![REG. de aplicación](media/adfs-msal-native-app-web-api/native4.png)

  5. En la pantalla aplicar Directiva de Access Control, seleccione **permitir todos** y haga clic en **siguiente**.

     ![REG. de aplicación](media/adfs-msal-native-app-web-api/native5.png)

  6. En la pantalla configurar permisos de aplicación, asegúrese de que **OpenID** está seleccionado y haga clic en **siguiente**.

     ![REG. de aplicación](media/adfs-msal-native-app-web-api/native6.png)

  7. En la pantalla resumen, haga clic en **siguiente**.

  8. En la pantalla completa, haga clic en **cerrar**.

  9. En administración de AD FS, haga clic en **grupos de aplicaciones** y seleccione grupo de aplicaciones de **NativeAppToWebApi**         . Haga clic con el botón secundario y seleccione **Propiedades**.

      ![REG. de aplicación](media/adfs-msal-native-app-web-api/native7.png)

  10. En la pantalla de propiedades de NativeAppToWebApi, seleccione **NativeAppToWebApi – Web API** en **Web API** y haga clic en **Editar...**

      ![REG. de aplicación](media/adfs-msal-native-app-web-api/native8.png)

  11. En NativeAppToWebApi: pantalla de propiedades de la API Web, seleccione la pestaña **reglas de transformación de emisión** y haga clic en **Agregar regla..** .

      ![REG. de aplicación](media/adfs-msal-native-app-web-api/native9.png)

  12. En el Asistente para agregar regla de notificaciones de transformación, seleccione **transformar una demanda entrante** en la **plantilla de regla de notificaciones:** lista desplegable y haga clic en **siguiente**.

      ![REG. de aplicación](media/adfs-msal-native-app-web-api/native10.png)

  13. Escriba **NameID** en **el campo Nombre de regla de notificaciones:** . Seleccione **el nombre** para el **tipo de notificaciones entrantes:**, **ID. de nombre** para el **tipo de notificaciones salientes:** y **nombre común** para **el formato de identificador de nombre saliente:**. Haga clic en **Finalizar**.

      ![REG. de aplicación](media/adfs-msal-native-app-web-api/native11.png)

  14. Haga clic en aceptar en NativeAppToWebApi: pantalla de propiedades de la API Web y, a continuación, en pantalla de propiedades de NativeAppToWebApi.

## <a name="code-configuration"></a>Configuración del código
En esta sección se muestra cómo configurar una aplicación nativa para iniciar sesión en el usuario y recuperar el token para llamar a la API Web.

1. Descargue el ejemplo [aquí](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi)

2. Abrir el ejemplo con Visual Studio

3. Abra el archivo App.config. Modifique lo siguiente:
   - ida: autoridad-escriba h`ttps://[your AD FS hostname]/adfs`
   - ida: ClientId: escriba el valor del **identificador de cliente** de #3 en el registro de la aplicación en AD FS sección anterior.
   - ida: RedirectUri: escriba el valor de **URI de redirección** de #3 en el registro de la aplicación en AD FS sección anterior.
   - todo: TodoListResourceId: escriba el valor del **identificador** de #4 en el registro de la aplicación en AD FS sección anterior
   - ida: todo: TodoListBaseAddress: escriba el valor del **identificador** de #4 en el registro de la aplicación en AD FS sección anterior.

     ![configuración de código](media/adfs-msal-native-app-web-api/native12.png)

 4. Abra el archivo de Web.config. Modifique lo siguiente:
    - ida: Audience: escriba el valor del **identificador** de #4 en el registro de la aplicación en AD FS sección anterior
    - ida: AdfsMetadataEndpoint-Enter `https://[your AD FS hostname]/federationmetadata/2007-06/federationmetadata.xml`

      ![configuración de código](media/adfs-msal-native-app-web-api/native13.png)

## <a name="test-the-sample"></a>Prueba del ejemplo
En esta sección se muestra cómo probar el ejemplo configurado anteriormente.

  1. Una vez realizados los cambios en el código, vuelva a generar la solución

  2. En Visual Studio, haga clic con el botón derecho en la solución y seleccione **establecer proyectos de inicio..** .

     ![Prueba de la aplicación](media/adfs-msal-native-app-web-api/native14.png)

  3. En las páginas de propiedades, asegúrese de que la **acción** está establecida en **iniciar** para cada uno de los proyectos

     ![Prueba de la aplicación](media/adfs-msal-native-app-web-api/native15.png)

  4. En la parte superior de Visual Studio, haga clic en la flecha verde.

     ![Prueba de la aplicación](media/adfs-msal-native-app-web-api/native16.png)

  5. En la pantalla principal de la aplicación nativa, haga clic en **iniciar sesión**.

     ![Prueba de la aplicación](media/adfs-msal-native-app-web-api/native17.png)

   Si no ve la pantalla de la aplicación nativa, busque y quite `*msalcache.bin` archivos de la carpeta donde se guarda el repositorio del proyecto en el sistema.

  1. Se le redirigirá a la página de inicio de sesión de AD FS. Continúe e inicie sesión.

      ![Prueba de la aplicación](media/adfs-msal-native-app-web-api/native18.png)

  2. Una vez iniciada la sesión, escriba texto **compilación de aplicación nativa a API Web** en el **elemento crear un para**. Haga clic en **Agregar elemento**.  Se llamará al **servicio de lista de tareas pendientes (API Web)** y agregará el elemento en la memoria caché.

       ![Prueba de la aplicación](media/adfs-msal-native-app-web-api/native19.png)

## <a name="next-steps"></a>Pasos siguientes
[Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
