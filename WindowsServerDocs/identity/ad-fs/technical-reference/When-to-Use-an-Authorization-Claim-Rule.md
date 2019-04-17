---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: "Cuándo usar una regla de solicitud de autorización"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 144e382692e8f2a6732f8c7c5b8a1dd6049192cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-an-authorization-claim-rule"></a>Cuándo usar una regla de solicitud de autorización
Puedes usar esta regla en los servicios de federación de Active Directory \(AD FS\) cuando debas tomar un tipo de notificación entrante y, a continuación, aplicar una acción que determina si un usuario se permite o deniega el acceso en función del valor que especifiques en la regla. Cuando usas esta regla, puedes pasan o transformación notificaciones que coincidan con la siguiente lógica de regla, en función de cualquiera de las opciones que se configura en la regla:  
  
|Opción de regla|Lógica de la regla|  
|---------------|--------------|  
|Permitir que todos los usuarios|Si el tipo de notificación entrante es igual a *cualquier tipo de notificación* y el valor es igual a *cualquier valor*, a continuación, reclamar el problema con el valor es igual a *permitir*|  
|Permitir el acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *especifica el valor de notificación*, a continuación, reclamar el problema con el valor es igual a *permitir*|  
|Denegar el acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *especifica el valor de notificación*, a continuación, reclamar el problema con el valor es igual a *denegar*|  
  
Las siguientes secciones proporcionan una introducción básica para reclamar reglas y proporcionar más detalles sobre cuándo usar esta regla.  
  
## <a name="about-claim-rules"></a>Acerca de las reglas de notificación  
Una regla de Reclamación representa una instancia de la lógica de negocios que tomar una notificación entrante, aplicar una condición a ella \ (si está x después y\) y generar una notificación saliente basándose en los parámetros de la condición. La siguiente lista describe sugerencias que debes conocer reclamar reglas antes de leer más en este tema:  
  
-   En la administración de FS anuncios en snap\, reglas de notificación solo se pueden crear mediante plantillas de notificación de regla  
  
-   Proceso de reglas de notificación entrante dice directamente desde un proveedor de notificaciones \ (como Active Directory u otro Service\ federación) o desde la salida de la aceptación, transforman las reglas en una relación de confianza del proveedor de notificaciones.  
  
-   Reglas de notificación se procesan el motor de emisión de notificaciones en orden cronológico dentro de un conjunto de reglas determinado. Estableciendo la prioridad de las reglas de aún más puede restringir o filtrar notificaciones que se generan mediante reglas anteriores dentro de un conjunto de reglas determinado.  
  
-   Plantillas de notificación de regla siempre requieren que especifiques un tipo de notificación entrante. Sin embargo, puede procesar varios valores de las notificaciones con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre la reclamación y reclamación conjuntos de reglas, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información, cómo se procesan los conjuntos de reglas de notificación, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Permitir que todos los usuarios  
Cuando usas la plantilla de regla de permitir que todos los usuarios, todos los usuarios tendrán acceso a la parte del usuario de confianza. Sin embargo, puedes usar las reglas de autorización adicionales para restringir aún más el acceso. Si una regla permite al usuario acceder a la parte del usuario de confianza y otra regla deniega el acceso de usuario para el usuario de confianza, el resultado de Denegar reemplaza el resultado de autorización y el usuario deniega el acceso.  
  
Los usuarios que se permiten el acceso a la parte del usuario de confianza de los servicios de federación de todavía se pueden denegar servicio por el usuario de confianza.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Permitir el acceso a los usuarios con esta notificación entrante  
Cuando usas el permiso o denegación en función de los usuarios en una plantilla de regla de notificación entrante para crear una regla y establecer la condición para permitir, puede permitir el acceso de usuario específico para el usuario de confianza en función del tipo y el valor de una notificación entrante. Por ejemplo, puedes usar esta plantilla de regla para crear una regla que permita sólo aquellos usuarios que tienen un grupo reclamar con un valor de administradores de dominio. Si una regla permite al usuario acceder a la parte del usuario de confianza y otra regla deniega el acceso de usuario para el usuario de confianza, el resultado de Denegar reemplaza el resultado de autorización y el usuario deniega el acceso.  
  
Los usuarios que tienen permiso para acceder a la parte del usuario de confianza de los servicios de federación de todavía se pueden denegar servicio por el usuario de confianza. Si quieres permitir que todos los usuarios para acceder a la parte del usuario de confianza, usa la plantilla de regla de permitir que todos los usuarios.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Denegar el acceso a los usuarios con esta notificación entrante  
Cuando usas el permitir o denegar a los usuarios en función de una plantilla de regla de notificación entrante para crear una regla y establecer la condición de denegación, puedes denegar acceso de usuario para el usuario de confianza en función del tipo y el valor de una notificación entrante. Por ejemplo, puedes usar esta plantilla de regla para crear una regla que denegará todos los usuarios que tienen un grupo de notificación con un valor de los usuarios del dominio.  
  
Si quieres utilizar la condición de denegación, pero también habilitar el acceso a la parte del usuario de confianza para usuarios específicos, debes agregar más adelante explícitamente las reglas de autorización con la condición de permiso para habilitar el acceso de los usuarios para el usuario de confianza.  
  
Si se deniega el acceso a un usuario cuando el motor de emisión de reclamaciones procesa el conjunto de reglas, más procesamiento de reglas se apaga y AD FS devuelve un error "Acceso denegado" a la solicitud del usuario.  
  
## <a name="authorizing-users"></a>Autorizar a los usuarios  
En AD FS, las reglas de autorización se usan para emitir una permitir o denegar reclamación que determina si un usuario o un grupo de usuarios \ (en función de used\ de tipo de notificación) se podrá acceder a recursos basados en Web\ en un determinado usuario de confianza o no. Las reglas de autorización solo pueden establecerse en confianzas de terceros de confianza.  
  
### <a name="authorization-rule-sets"></a>Conjuntos de reglas de autorización  
Conjuntos de reglas de autorización diferente existen según el tipo de permitir o denegación operaciones que tengas que configurar. Estos conjuntos de reglas se incluyen:  
  
-   **Las reglas de autorización de emisión**: estas reglas de determinan si un usuario puede recibir notificaciones para un usuario de confianza y, por lo tanto, el acceso a la parte del usuario de confianza.  
  
-   **Las reglas de autorización de delegación**: estas reglas de determinan si un usuario puede actuar como otro usuario para el usuario de confianza. Cuando un usuario actúa como otro usuario, notificaciones sobre el usuario solicite aún se colocan en el token.  
  
-   **Las reglas de autorización de suplantación**: estas reglas de determinan si un usuario puede suplantar completamente otro usuario para que el usuario de confianza. Suplantan la identidad de otro usuario es una funcionalidad muy eficaz, porque el usuario de confianza no sabrá que el usuario se suplanta.  
  
Para obtener más información acerca de cómo el proceso de regla de autorización se ajusta a la canalización de emisión de notificaciones, consulta el rol del motor de emisión de reclamaciones.  
  
### <a name="supported-claim-types"></a>Tipos de notificación compatibles  
AD FSdefines dos tipos de notificación que se usan para determinar si un usuario se permite o deniega. Estos identificadores uniformes de recursos \(URIs\) son del tipo de notificación:  
  
1.  **Permitir**: http:///\/schemas.microsoft.com\/authorization\/claims\/permit  
  
2.  **Denegar**: http:///\/schemas.microsoft.com\/authorization\/claims\/deny  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Puedes crear ambas reglas de autorización mediante el lenguaje de regla de notificación o la **permitir que todos los usuarios** plantilla de regla o el **permitir o denegar a los usuarios en función de una notificación entrante** plantilla de la regla en la administración de AD FS snap\ en. La plantilla de regla de permitir que todos los usuarios no proporciona ninguna opción de configuración. Sin embargo, la permitir o denegar a los usuarios en función de una plantilla de notificación entrante regla proporciona las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación  
  
-   Especificar un tipo de notificación entrante  
  
-   Escribe un valor de notificación entrante  
  
-   Permitir el acceso a los usuarios con esta notificación entrante  
  
-   Denegar el acceso a los usuarios con esta notificación entrante  
  
Para obtener más instrucciones sobre cómo crear esta plantilla, consulta [crear una regla para permitir que todos los usuarios](https://technet.microsoft.com/library/ee913577.aspx) o [crear una regla para permitir o denegar a los usuarios en función de una notificación entrante](https://technet.microsoft.com/library/ee913594.aspx) en la Guía de implementación de AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Con el lenguaje de regla de notificación  
Si solo cuando el valor de notificación coincide con un patrón personalizado, se debe enviar una reclamación, debes usar una regla personalizada. Para obtener más información, consulta [cuándo usar una regla de notificación personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Ejemplo de cómo crear una regla de autorización en función de varias notificaciones  
Al usar la sintaxis de lenguaje de regla de notificación para autorizar reclamaciones, una reclamación también se puede emitir en función de la presencia de una reclamación varios en reclamaciones original del usuario. La siguiente regla emite una solicitud de autorización solo si el usuario es miembro de los editores de grupo y se ha autenticado mediante la autenticación de Windows:  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Ejemplo de cómo crear la autorización de reglas que se delegado que puedan crear o eliminar relaciones de confianza de federación servidor proxy  
Para que un servicio de federación pueda usar a un proxy de servidor de federación para redirigir las solicitudes de cliente, primero se debe establecer una relación de confianza entre el servicio de federación y el equipo de proxy del servidor de federación. De manera predeterminada, se establece una confianza de proxy cuando cualquiera de las siguientes credenciales se proporciona correctamente en el Asistente para configuración de Proxy de federación de servidor de AD FS:  
  
-   La cuenta de servicio usada por el servicio de federación, que se protege el proxy  
  
-   Una cuenta de dominio de Active Directory que sea miembro del grupo de administradores local en todos los servidores de federación en una granja de servidores de federación  
  
Cuando quieras especificar qué usuarios o los usuarios pueden crear una confianza de proxy de un servicio de federación determinado, puedes usar cualquiera de los siguientes métodos de la delegación. Esta lista de métodos está en orden de prioridad, según las recomendaciones del equipo de producto de AD FS de los métodos más seguras y menos problemáticas de la delegación. Es necesario usar solo uno de estos métodos, según las necesidades de la organización:  
  
1.  Crear un grupo de seguridad de dominio de Active Directory \ (por ejemplo, FSProxyTrustCreators\), agrega este grupo al grupo de administradores locales en cada uno de los servidores de federación de la batería y, a continuación, agregar solo las cuentas de usuario a la que desea delegar este derecho al nuevo grupo. Este es el método preferido.  
  
2.  Agregar cuenta de dominio del usuario al grupo de administradores en cada uno de los servidores de federación de la batería.  
  
3.  Si por algún motivo no puedes usar cualquiera de estos métodos, también puedes crear una regla de autorización para este propósito. Aunque no se recomienda, debido a posibles problemas que pueden surgir si no está escrita correctamente esta regla, puedes usar una regla de autorización personalizada para delegar qué Active Directory cuentas de usuario de dominio pueden crear o incluso quitar las confianzas entre todos los proxy de servidor de federación que están asociados a un servicio de federación determinado.  
  
    Si eliges método 3, puedes usar la siguiente sintaxis de la regla para emitir una solicitud de autorización que permitirá que el usuario especificado \ (en este caso, contoso\\frankm\) para crear proxies de servidor de servicios de federación de confianzas de federación de uno o más. Debes aplicar esta regla usando el comando de Windows PowerShell **Set ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    Más adelante, si quieres quitar del usuario para que el usuario ya no puede crear relaciones de confianza de proxy, puedes revertir a la regla de autorización de confianza de proxy predeterminado para quitar a la derecha para que el usuario crear confianzas de proxy para el servicio de federación. También debes aplicar esta regla usando el comando de Windows PowerShell **Set ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Para obtener más información sobre cómo usar el lenguaje de regla de notificación, consulta [el rol del lenguaje de regla reclamar](The-Role-of-the-Claim-Rule-Language.md).  
  

