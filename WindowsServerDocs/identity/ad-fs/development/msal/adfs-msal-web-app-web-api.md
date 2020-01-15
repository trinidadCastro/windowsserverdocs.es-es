---
title: AD FS aplicación Web de MSAL (aplicación de servidor) llamar a API Web
description: Obtenga información sobre cómo crear un usuario que inicie sesión en una aplicación web autenticado por AD FS 2019.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f28e5feccb7544046104658585ab3f739f659957
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949503"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>Escenario: aplicación web (aplicación de servidor) llamada a Web API 
>Se aplica a: AD FS 2019 y versiones posteriores 
 
Obtenga información sobre cómo crear un usuario que inicie sesión en una aplicación web autenticado por AD FS 2019 y adquirir tokens mediante la [biblioteca MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) para llamar a las API Web.  
 
Antes de leer este artículo, debe estar familiarizado con los [conceptos de AD FS](../ad-fs-openid-connect-oauth-concepts.md) y el flujo de concesión de código de [autorización](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>Introducción 
 
![Información general de la aplicación web que llama a la API Web](media/adfs-msal-web-app-web-api/webapp1.png)

En este flujo, agregará autenticación a la aplicación web (aplicación de servidor), que puede iniciar sesión a los usuarios y llamar a una API Web. Desde la aplicación Web, para llamar a la API Web, use el método de adquisición de tokens de [AcquireTokenByAuthorizationCode](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder?view=azure-dotnet) de MSAL. Usará el flujo del código de autorización y almacenará el token adquirido en la caché de tokens. A continuación, el controlador adquirirá tokens de forma silenciosa de la caché cuando sea necesario. MSAL actualizará el token si es necesario. 

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
  2. En el Asistente para grupos de aplicaciones, en **nombre** , escriba **WebAppToWebApi** y en **aplicaciones cliente-servidor** seleccione la plantilla **aplicación de servidor que tiene acceso a una API Web** . Haz clic en **Siguiente**.  
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp2.png)
  
  3. Copie el valor del **identificador de cliente** . Se usará más adelante como valor de **ida: ClientID** en el archivo **Web. config** de aplicaciones. Escriba lo siguiente en **URI de redirección:**  - https://localhost:44326. Haz clic en Agregar. Haz clic en **Siguiente**. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp3.png)
  
  4. En la pantalla configurar credenciales de aplicación, active la casilla **generar un secreto compartido** y copie el secreto. Se usará más adelante como valor de **ida: ClientSecret** en el archivo **Web. config** de aplicaciones. Haz clic en **Siguiente**.  
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp4.png)
  
  5. En la pantalla configurar API Web, escriba el **identificador:** https://webapi. Haz clic en **Agregar**. Haz clic en **Siguiente**. Este valor se usará más adelante para **ida: GraphResourceId** en el archivo **Web. config** de las aplicaciones. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp5.png)
  
  6. En la pantalla aplicar Directiva de Access Control, seleccione **permitir** todos y haga clic en **siguiente**. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp6.png)
  
  7. En la pantalla configurar permisos de aplicación, asegúrese de que **OpenID** y **user_impersonation** estén seleccionados y haga clic en **siguiente**. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp7.png)
  
  8. En la pantalla resumen, haga clic en **siguiente**. 
  
  9. En la pantalla completa, haga clic en **cerrar**.



## <a name="code-configuration"></a>Configuración del código 

En esta sección se muestra cómo configurar una aplicación Web de ASP.NET para iniciar sesión en el usuario y recuperar el token para llamar a la API Web. 

  1. Descargue el ejemplo [aquí](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)   
  
  2. Abrir el ejemplo con Visual Studio 
  
  3. Abra el archivo Web. config. Modifique lo siguiente: 
       - ida: ClientId: escriba el valor del **identificador de cliente** de #3 en el registro de la aplicación en AD FS sección anterior. 
       - ida: ClientSecret: escriba el valor del **secreto** en #4 en el registro de la aplicación en AD FS sección anterior. 
       - ida: RedirectUri: escriba el valor de **URI de redirección** de #3 en el registro de la aplicación en AD FS sección anterior. 
       - ida: autoridad: escriba https://[su nombre de host de AD FS]/ADFS. Por ejemplo, https://adfs.contoso.com/adfs 
       - ida: Resource: escriba el valor del **identificador** de #5 en el registro de la aplicación en AD FS sección anterior. 
      
          ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp8.png)
 
 
### <a name="test-the-sample"></a>Prueba del ejemplo 
En esta sección se muestra cómo probar el ejemplo configurado anteriormente. 

  1. Una vez realizados los cambios en el código, vuelva a generar la solución 
  
  2. En la parte superior de Visual Studio, asegúrese de que Internet Explorer está seleccionado y haga clic en la flecha verde. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp9.png)

  3. En la Página principal, haga clic en iniciar sesión. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp10.png)

  4. Se le redirigirá a la página de inicio de sesión de AD FS. Continúe e inicie sesión. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp11.png)

  5. Una vez que haya iniciado sesión, haga clic en token de acceso.  
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp12.png)

  6. Al hacer clic en token de acceso, se obtiene la información del token de acceso mediante una llamada a la API Web. 
  
      ![Agregar grupo de aplicaciones](media/adfs-msal-web-app-web-api/webapp13.png)
 
 ## <a name="next-steps"></a>Pasos a seguir
[Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 