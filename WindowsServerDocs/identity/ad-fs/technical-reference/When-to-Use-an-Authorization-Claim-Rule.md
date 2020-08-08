---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: Cuándo usar una regla de notificación de autorización
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 690ae558f625ca3a4c5878be229d950902f7f5e2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958703"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>Cuándo usar una regla de notificación de autorización
Puede usar esta regla en Servicios de federación de Active Directory (AD FS) \( AD FS \) cuando necesite tomar un tipo de demanda entrante y después aplicar una acción que determinará si se permitirá o denegará el acceso a un usuario en función del valor que especifique en la regla. Cuando usas esta regla, transformas o pasas por las notificaciones que coinciden con la lógica de la regla siguiente, según cualquiera de las opciones de que configuración de la regla:

|Opción de regla|Lógica de regla|
|---------------|--------------|
|Permitir a todos los usuarios|Si el tipo de notificación entrante es igual a *cualquier tipo de notificación* y el valor es igual a *cualquier valor*, emitir una notificación con un valor igual a *Permitir*|
|Permitir acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *tipo de notificación especificado* y el valor es igual a *valor de notificación especificado*, emitir una notificación con un valor igual a *Permitir*|
|Denegar acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *tipo de notificación especificado* y el valor es igual a *valor de notificación especificado*, emitir una notificación con un valor igual a *Denegar*|

Las secciones siguientes ofrecen una introducción básica a las reglas de notificación y proporcionan más detalles sobre cuándo utilizar esta regla.

## <a name="about-claim-rules"></a>Acerca de las reglas de notificaciones
Una regla de notificaciones representa una instancia de la lógica de negocios que tomará una solicitud entrante, le aplicará una condición \( si x después \) y y generará una notificaciones salientes según los parámetros de la condición. La lista siguiente destaca las sugerencias importantes que deberías conocer sobre las reglas de notificaciones antes de seguir leyendo este tema:

-   En el complemento de administración de AD FS \- , las reglas de notificaciones solo se pueden crear mediante plantillas de regla de notificaciones.

-   Las reglas de notificación procesan las notificaciones entrantes directamente desde un proveedor de notificaciones \( como Active Directory u otro servicio de Federación \) o desde la salida de las reglas de transformación de aceptación en una relación de confianza para proveedor de notificaciones.

-   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una precedencia en las reglas, puedes perfeccionar o filtrar aún más las notificaciones que generan las reglas anteriores dentro de un conjunto determinado de reglas.

-   Siempre tendrás que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación con una sola regla.

Para obtener información más detallada sobre las reglas de notificaciones y los conjuntos de reglas de notificaciones, consulte [el rol de las reglas de notificaciones](The-Role-of-Claim-Rules.md). Para obtener más información sobre cómo se procesan las reglas, vea [el rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información sobre cómo se procesan los conjuntos de reglas de notificaciones, vea [el rol de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md).

## <a name="permit-all-users"></a>Permitir a todos los usuarios
Cuando usas la plantilla de regla Permitir a todos los usuarios, todos los usuarios tendrán acceso al usuario de confianza. Sin embargo, puedes usar reglas de autorización adicionales para restringir aún más el acceso. Si una regla permite a un usuario tener acceso al usuario de confianza y otra regla deniega a un usuario el acceso al usuario de confianza, el resultado de denegación invalida al resultado de autorización y se le deniega el acceso al usuario.

Aún así, es posible que los usuarios que tienen permiso de acceso al usuario de confianza por parte del Servicio de federación tengan el acceso denegado por parte del usuario de confianza.

## <a name="permit-access-to-users-with-this-incoming-claim"></a>Permitir acceso a los usuarios con esta notificación entrante
Cuando se usa la plantilla de reglas permitir o denegar a los usuarios en función de una notificaciones entrantes para crear una regla y establecer la condición para permitir, puede permitir el acceso de un usuario específico al usuario de confianza según el tipo y el valor de una demanda entrante. Por ejemplo, puedes usar esta plantilla de reglas para crear una regla que solo dé permiso a los usuarios que tienen un grupo de notificaciones con el valor de Admins. del dominio. Si una regla permite a un usuario tener acceso al usuario de confianza y otra regla deniega a un usuario el acceso al usuario de confianza, el resultado de denegación invalida al resultado de autorización y se le deniega el acceso al usuario.

Aún así, es posible que los usuarios que tienen permiso para acceder al usuario de confianza por parte del Servicio de federación tengan el acceso denegado por parte del usuario de confianza. Si quieres permitir a todos los usuarios tener acceso al usuario de confianza, usa la plantilla de regla Permitir a todos los usuarios.

## <a name="deny-access-to-users-with-this-incoming-claim"></a>Denegar acceso a los usuarios con esta notificación entrante
Si usa la plantilla de reglas permitir o denegar a los usuarios en función de una notificaciones entrantes para crear una regla y establecer la condición en denegar, puede denegar el acceso del usuario al usuario de confianza según el tipo y el valor de una demanda entrante. Por ejemplo, puedes usar esta plantilla de reglas para crear una regla que deniegue el acceso a todos los usuarios que tienen un grupo de notificaciones con el valor de Usuarios del dominio.

Si quieres usar la condición de denegación pero también habilitar el acceso al usuario de confianza a unos usuarios específicos, más adelante tienes que agregar explícitamente las reglas de autorización con la condición de permisión para permitir el acceso de esos usuarios al usuario de confianza.

Si a un usuario se le deniega el acceso cuando el motor de emisión de notificaciones procesa el conjunto de reglas, el procesamiento de reglas se cierra y AD FS devuelve un error de "acceso denegado" a la solicitud del usuario.

## <a name="authorizing-users"></a>Permitir acceso a los usuarios
En AD FS, las reglas de autorización se usan para emitir un permiso o denegar notificaciones que determinarán si un usuario o un grupo de usuarios en \( función del tipo de notificaciones que se usa se \) permitirá tener acceso a \- los recursos basados en Web de un determinado usuario de confianza o no. Las reglas de autorización solo se pueden establecer en relaciones de confianza para usuario autenticado.

### <a name="authorization-rule-sets"></a>Conjuntos de reglas de autorización
Existen diferentes conjuntos de reglas de autorización según el tipo de operaciones de permiso o denegación que tengas que configurar. Estos conjuntos de reglas incluyen:

-   **Reglas de autorización de emisión**: estas reglas determinan si un usuario puede recibir notificaciones para un usuario de confianza y, por lo tanto, acceder al usuario de confianza.

-   **Reglas de autorización de delegación**: estas reglas determinan si un usuario puede actuar como otro usuario de cara al usuario de confianza. Cuando un usuario está actuando como otro usuario, las notificaciones sobre el usuario solicitante todavía están ubicadas en el token.

-   **Reglas de autorización de suplantación**: estas reglas determinan si un usuario puede suplantar totalmente a otro usuario de cara al usuario de confianza. El suplantar a otro usuario es una capacidad muy eficaz, ya que el usuario de confianza no sabrá que se está suplantando al usuario.

Para obtener más información sobre cómo encaja el proceso de las regla de autorización en la canalización de emisión de notificaciones, consulta «El papel del Motor de emisión de notificaciones».

### <a name="supported-claim-types"></a>Tipos de notificaciones admitidos
AD FS define dos tipos de notificaciones que se usan para determinar si se permite o se deniega un usuario. Los URI de los identificadores uniformes \( de recursos de tipos de notificaciones \) son los siguientes:

1.  **Permitir**: http: \/ \/ \/ permiso de \/ notificaciones de autorización de schemas.Microsoft.com \/

2.  **Deny**: http: \/ \/ schemas.Microsoft.com \/ de \/ notificaciones de autorización \/ denegar

## <a name="how-to-create-this-rule"></a>Cómo crear esta regla
Puede crear ambas reglas de autorización mediante el lenguaje de reglas de notificaciones o mediante la plantilla de reglas **permitir a todos los usuarios** o la plantilla de reglas **permitir o denegar a los usuarios según una reclamación entrante** en el complemento Administración \- de AD FS en. La plantilla de reglas «Permitir a todos los usuarios» no proporciona ninguna opción de configuración. Sin embargo, la plantilla de reglas «Permitir o denegar a los usuarios según una notificación entrante» proporciona las siguientes opciones de configuración:

-   Especificar un nombre de regla de notificaciones

-   Especificar un tipo de notificación entrante

-   Escribir un valor de notificación entrante

-   Permitir acceso a los usuarios con esta notificación entrante

-   Denegar acceso a los usuarios con esta notificación entrante

Para obtener más instrucciones sobre cómo crear esta plantilla, consulte [creación de una regla para permitir a todos los usuarios](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913577(v=ws.11)) o [creación de una regla para permitir o denegar a los usuarios en función de una demanda entrante](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913594(v=ws.11)) en la guía de implementación de AD FS.

## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación
Si solo se debe enviar una notificación cuando el valor de notificación coincida con un patrón personalizado, tienes que usar una regla personalizada. Para obtener más información, consulte [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).

### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Ejemplo de cómo crear una regla de autorización basada en notificaciones múltiples
Cuando se usa la sintaxis del lenguaje de reglas de notificación para autorizar las notificaciones, también se puede emitir una notificación basada en la presencia de varias notificaciones en las notificaciones originales del usuario. La siguiente regla emite solo una notificación de autorización si el usuario es miembro del grupo de Editores y se ha autenticado mediante la autenticación de Windows:

```
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",
value == "urn:federation:authentication:windows" ]
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == "editors"]
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");
```

### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Ejemplo de cómo crear reglas de autorización que deleguen quién puede crear o eliminar las relaciones de confianza del servidor proxy de federación
Antes de que un Servicio de federación pueda usar un servidor proxy de federación para redirigir las solicitudes de cliente, primero tiene que establecerse una relación de confianza entre el Servicio de federación y el equipo del servidor proxy de federación. De forma predeterminada, se establece una relación de confianza de proxy cuando alguna de las siguientes credenciales se proporciona correctamente en el Asistente para la configuración del proxy del servidor de federación AD FS:

-   La cuenta de servicio que usó el Servicio de federación y que protegerá el proxy

-   Una cuenta de dominio de Active Directory que sea miembro del grupo local de Administradores en todos los servidores de federación de una granja de servidores de federación

Si quieres especificar qué usuario o usuarios pueden crear una confianza de proxy para un Servicio de federación determinado, puedes usar cualquiera de los siguientes métodos de delegación. Esta lista de métodos está en orden de prioridad, en función de las recomendaciones del equipo de productos AD FS de los métodos de delegación más seguros y menos problemáticos. Es necesario usar solo uno de estos métodos, según las necesidades de tu empresa:

1.  Cree un grupo de seguridad de dominio en Active Directory \( por ejemplo, FSProxyTrustCreators \) , agregue este grupo al grupo local Administradores en cada uno de los servidores de Federación de la granja y, a continuación, agregue únicamente las cuentas de usuario a las que desea delegar este derecho al nuevo grupo. Este es el método preferido.

2.  Agregue la cuenta de dominio del usuario al grupo de administradores en cada uno de los servidores de Federación de la granja.

3.  Si por algún motivo no puedes usar cualquiera de estos métodos, también puedes crear una regla de autorización para este propósito. Aunque no es lo recomendado (debido a posibles complicaciones que pueden producirse si no se escribe correctamente esta regla), puedes usar una regla de autorización personalizada para delegar qué cuentas de usuario de dominio de Active Directory también pueden crear o incluso eliminar las confianzas entre todos los servidores proxy de federación que estén asociados a un Servicio de federación determinado.

    Si eliges el método 3, puedes usar la siguiente sintaxis de regla para emitir una demanda de autorización que permita a un usuario especificado \( en este caso, contoso \\ frankm \) crear confianzas para uno o más servidores proxy de federación en el servicio de Federación. Debe aplicar esta regla mediante el conjunto de comandos de Windows PowerShell ** \- ADFSProperties AddProxyAuthorizationRules**.

    ```
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")

    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");

    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );

    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );
    ```

    Más adelante, si quieres eliminar el usuario para que no pueda crear confianzas de proxy, puedes volver a la regla de autorización de confianza de proxy predeterminada para eliminar el derecho del usuario a crear confianzas de proxy para el Servicio de federación. También debe aplicar esta regla mediante el conjunto de comandos de Windows PowerShell ** \- ADFSProperties AddProxyAuthorizationRules**.

    ```
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");

    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );

    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );
    ```

Para obtener más información sobre cómo usar el lenguaje de reglas de notificaciones, vea [el rol del lenguaje de reglas de notificaciones](The-Role-of-the-Claim-Rule-Language.md).

