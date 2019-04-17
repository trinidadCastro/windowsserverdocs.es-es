---
title: "Personalizar reclamaciones emitirán id_token al usar conectar OpenID o OAuth con AD FS de 2016"
description: "Una introducción técnica de la conecpts token identificador personalizado en AD FS de 2016"
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: c17d50bb6d90726eafc3e585f16a7a8189a68392
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personalizar reclamaciones emitirán id_token al usar conectar OpenID o OAuth con AD FS de 2016

## <a name="overview"></a>Introducción
El artículo [aquí](enabling-openId-connect-with-ad-fs.md) muestra cómo crear una aplicación que usa AD FS para conectar OpenID inicio de sesión. Sin embargo, de manera predeterminada hay solo un conjunto fijo de una reclamación disponibles en la id_token. AD FS 2016 tiene la capacidad para personalizar la id_token en escenarios de OpenID conectar.

## <a name="when-are-custom-id-token-used"></a>¿Cuando están identificador personalizado token que se utiliza?
En determinados escenarios es posible que la aplicación cliente no tiene un recurso que está intentando acceder. Por lo tanto, realmente no es necesario un token de acceso. En estos casos, la aplicación cliente debe básicamente solo una pero token de identificador con algunas notificaciones para ayudar a la funcionalidad adicionales.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>¿Cuáles son las restricciones sobre cómo obtener notificaciones personalizadas en el ID de token?

### <a name="scenario-1"></a>Escenario 1

![Restringir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode se establece como form_post
2.  Solo los clientes pública obtener notificaciones personalizadas en el ID de token
3.  Identificador de terceros de confianza debe ser el mismo que el identificador de cliente

### <a name="scenario-2"></a>Escenario 2

![Restringir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) instalados en los servidores de AD FS
1.  response_mode se establece como form_post
2.  Asignar allatclaims ámbito al cliente: par de punto de reunión.
Puedes asignar el ámbito mediante el cmdlet de Grant-ADFSApplicationPermission como se indica en el siguiente ejemplo:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Crear una aplicación de OAuth para controlar las notificaciones personalizadas en el token de Id.
Usar el artículo [aquí](Enabling-OpenId-Connect-with-AD-FS-2016.md) para crear una aplicación de la aplicación que usa AD FS para conectar OpenID iniciar sesión. A continuación, sigue estos pasos para configurar la aplicación en AD FS para recibir el token de identificador con notificaciones personalizadas.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Crear el grupo de aplicaciones en AD FS de 2016

1.  Crear un grupo de aplicaciones en función de la nueva plantilla, que se muestra a continuación, denomina CustomTokenClient.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Esta plantilla crea a un cliente confidencial. Ten en cuenta el identificador y especifica el URI de devolución como la dirección URL de SSL del proyecto VS.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  En el paso siguiente, selecciona **generar un secreto compartido** crear credenciales de cliente y copiar las credenciales del cliente generadas.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Haz clic en **siguiente** y continuar para completar el asistente.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Crear el usuario de confianza
Para agregar notificaciones personalizadas de Id. de token, debes crear un punto de reunión cuyas notificaciones se agregan en el token de ID. Usar al Asistente para agregar confiar confianza de terceros para crear un nuevo usuario de confianza, como se muestra a continuación:
 
![Usuario de confianza](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Después de crea el usuario de confianza, haga clic con el botón secundario en la entrada de terceros de confianza y selecciona **edición reclama la directiva de emisión** para agregar las reglas de emisión de reclamaciones. Agrega las notificaciones personalizadas necesarias para el token del identificador, como se muestra a continuación:

![Usuario de confianza](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Asignar ámbito "allatclaims" a la par de cliente y de usuario de confianza
Usar PowerShell en el servidor de AD FS, asigna el nuevo ámbito allatclaims tal como se indica en el siguiente ejemplo (cambiar el clientID y el servidor:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Cambiar el ClientRoleIdentifier y ServerRoleIdentifier según la configuración de la aplicación

## <a name="test-the-custom-claims-in-id-token"></a>Probar las notificaciones personalizadas en el token de Id.

A continuación, usar el mismo fragmento de código que siempre has usado para acceder a notificaciones, puedes ver las notificaciones adicionales que forman parte de la id_token.
Por ejemplo, en una aplicación de muestra MVC. NET, abre uno de los archivos de controlador y escribe código como el a continuación:


``` code
    [Authorize]
    public ActionResult About()
    {

        ClaimsPrincipal cp = ClaimsPrincipal.Current;

        string userName = cp.FindFirst(ClaimTypes.GivenName).Value;
        ViewBag.Message = String.Format("Hello {0}!", userName);
        return View();

    }

```

>[!NOTE]
>Ten en cuenta que se requiere el parámetro de recursos en la solicitud Oauth2.
>
>Ejemplo incorrecto:
>
>**HTTPS & #58; / / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7**
>
>Ejemplo:
>
>**HTTPS & #58; / / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=https&#58;//customidrp1/**
>
>Si el parámetro de recurso no está en la solicitud que puede recibir un error o un token sin cualquier reclamación personalizado.

## <a name="next-steps"></a>Pasos siguientes
[AD FS desarrollo](../../ad-fs/AD-FS-Development.md)  