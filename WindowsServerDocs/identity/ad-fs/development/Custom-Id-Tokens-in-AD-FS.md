---
title: Personalizar las notificaciones que se emitan en id_token al usar OpenID Connect o OAuth con AD FS 2016
description: Introducción técnica a la conecpts de token de identificador personalizado en AD FS 2016
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 8c8e14f22a4bca5b6d32e841814a58a4b4ddca01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820646"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personalizar las notificaciones que se emitan en id_token al usar OpenID Connect o OAuth con AD FS 2016

## <a name="overview"></a>Información general
El artículo [aquí](enabling-openId-connect-with-ad-fs.md) muestra cómo crear una aplicación que utiliza AD FS para OpenID Connect inicio de sesión. Sin embargo, de forma predeterminada hay solo un conjunto fijo de notificaciones disponibles en id_token. AD FS 2016 tiene la capacidad para personalizar el valor de id_token en escenarios de OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>¿Cuando están identificador personalizado token que se usa?
En determinados escenarios es posible que la aplicación cliente no tiene un recurso que está intentando obtener acceso. Por lo tanto, realmente no necesita un token de acceso. En tales casos, la aplicación cliente necesita básicamente solo un identificador token, pero con algunas notificaciones adicionales para ayudar en la funcionalidad.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>¿Cuáles son las restricciones sobre la obtención de notificaciones personalizadas en el identificador de token?

### <a name="scenario-1"></a>Escenario 1

![restringir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode se establece como form_post
2.  Solo los clientes públicos pueden obtener notificaciones personalizadas en el identificador de token
3.  Identificador del usuario autenticado debe coincidir con el identificador de cliente

### <a name="scenario-2"></a>Escenario 2

![restringir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Con [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) instalados en los servidores de AD FS
1.  response_mode se establece como form_post
2.  Asignar allatclaims de ámbito para el cliente: un par RP.
Puede asignar el ámbito mediante el cmdlet Grant ADFSApplicationPermission tal como se indica en el ejemplo siguiente:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Crear una aplicación de OAuth para controlar las notificaciones personalizadas en el token de identificador
Use el artículo [aquí](Enabling-OpenId-Connect-with-AD-FS-2016.md) para crear una aplicación de la aplicación que utiliza AD FS para OpenID Connect de sesión. A continuación, siga los pasos siguientes para configurar la aplicación en AD FS para recibir el token de identificador con las notificaciones personalizadas.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Cree el grupo de aplicaciones en AD FS 2016

1.  Cree un grupo de aplicaciones basado en la nueva plantilla, que se muestra a continuación, llama a CustomTokenClient.

![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Esta plantilla crea a un cliente confidencial. Tenga en cuenta el identificador y especifique el URI devuelto como la dirección URL de SSL del proyecto de VS.

![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  En el paso siguiente, seleccione **generar un secreto compartido** para crear las credenciales de cliente y copiar las credenciales de cliente generadas.

![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Haga clic en **siguiente** y complete el asistente.

![Remoto](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Crear usuario de confianza
Para agregar notificaciones personalizadas en el identificador de token, deberá crear un RP se agregarán cuyas notificaciones del token de identificador. Use el Asistente para agregar confianza para crear un nuevo usuario de confianza, como se muestra a continuación:
 
![Usuario de confianza](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Una vez creado el usuario de confianza, haga clic con el botón derecho en la entrada de entidad de usuario de confianza y seleccione **Editar directiva de emisión de notificación** para agregar reglas de emisión de notificaciones. Agregue las notificaciones personalizadas necesarias para el token de identificador tal como se muestra a continuación:

![Usuario de confianza](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Asignar el ámbito de "allatclaims" para el par de cliente y de confianza
Uso de PowerShell en el servidor de AD FS, asigne el nuevo ámbito allatclaims tal como se indica en el ejemplo siguiente (cambie el clientID y el servidor:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Cambie el ClientRoleIdentifier y ServerRoleIdentifier según la configuración de la aplicación

## <a name="test-the-custom-claims-in-id-token"></a>Probar las notificaciones personalizadas en el token de identificador

A continuación, usa el mismo fragmento de código que siempre ha usado para tener acceso a las notificaciones, puede ver las notificaciones adicionales que se pasará a formar parte del id_token.
Por ejemplo, en una aplicación de ejemplo .NET MVC, abra uno de los archivos de controlador y escriba el código como el siguiente:


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
>Ten en cuenta que es necesario el parámetro de recurso en la solicitud de Oauth2.
>
>Ejemplo incorrecto:
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token á & mbito = openid & los redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 & client_id = 6db3ec2a 075a - 4c 72-9b22-ca7ab60cb4e7 & estado = 42c2c156aef47e8d0870 & recurso = 72-9b22-ca7ab60cb4e7 6db3ec2a 075a - 4c**
>
>Buen ejemplo:
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token á & mbito = openid & los redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 & client_id = 6db3ec2a 075a - 4c 72-9b22-ca7ab60cb4e7 & estado = 42c2c156aef47e8d0870 & recurso = https&#58;//customidrp1/ & response_mode = form_post**
>
>Si el parámetro de recurso no está en la solicitud puede recibir un error o un token sin las notificaciones personalizadas.

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
