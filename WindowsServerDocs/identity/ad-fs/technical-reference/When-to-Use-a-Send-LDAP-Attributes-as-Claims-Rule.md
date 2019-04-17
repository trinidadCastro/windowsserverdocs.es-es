---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: "Cuándo usar atributos LDAP enviar como reclamaciones regla"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05a2dd88057b64675bbc3bd30724d1eda0880c44
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Cuándo usar atributos LDAP enviar como reclamaciones regla
Puedes usar esta regla en los servicios de federación de Active Directory \(AD FS\) cuando quieras emitir notificaciones salientes que contienen valores de atributo de protocolo ligero de acceso a directorios \(LDAP\) real que existen en un almacén de atributo y después asociar un tipo de notificación con cada uno de los atributos LDAP. Para obtener más información acerca de los almacenes de atributo, consulta [el rol de atributo almacena](The-Role-of-Attribute-Stores.md).  
  
Cuando usas esta regla, se emite una solicitud para cada atributo LDAP que especifiques y que coincida con la lógica de la regla, tal como se describe en la siguiente tabla.  
  
|Opción de regla|Lógica de la regla|  
|---------------|--------------|  
|Asignación de atributos LDAP a tipos de notificaciones salientes|Si el almacén de atributo es igual a *tienda atributo especificado* y LDAP atributo es igual a *valor especificado*, a continuación, asigna el valor del atributo LDAP la *notificación saliente especificado* escribe y emitir la reclamación.|  
  
Las siguientes secciones proporcionan una introducción básica reglas de notificación. También ofrecen detalles sobre cuándo usar los atributos de LDAP enviar como regla de reclamaciones.  
  
## <a name="about-claim-rules"></a>Acerca de las reglas de notificación  
Una regla de Reclamación representa una instancia de la lógica de negocios que tomar una notificación entrante, aplicar una condición a ella \ (si está x después y\) y generar una notificación saliente basándose en los parámetros de la condición. La siguiente lista describe sugerencias que debes conocer reclamar reglas antes de leer más en este tema:  
  
-   En la administración de FS anuncios en snap\, reglas de notificación solo se pueden crear mediante plantillas de notificación de regla  
  
-   Proceso de reglas de notificación entrante dice directamente desde un proveedor de notificaciones \ (como Active Directory u otro Service\ federación) o desde la salida de la aceptación, transforman las reglas en una relación de confianza del proveedor de notificaciones.  
  
-   Reglas de notificación se procesan el motor de emisión de notificaciones en orden cronológico dentro de un conjunto de reglas determinado. Estableciendo la prioridad de las reglas de aún más puede restringir o filtrar notificaciones que se generan mediante reglas anteriores dentro de un conjunto de reglas determinado.  
  
-   Plantillas de notificación de regla siempre requieren que especifiques un tipo de notificación entrante. Sin embargo, puede procesar varios valores de las notificaciones con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre la reclamación y reclamación conjuntos de reglas, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información, cómo se procesan los conjuntos de reglas de notificación, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Asignación de atributos LDAP a tipos de notificaciones salientes  
Cuando uses los atributos de LDAP enviar como plantilla de regla de notificaciones, puedes seleccionar atributos de un almacén de atributo LDAP, como Active Directory o los servicios de dominio de Active Directory \(AD DS\) a enviar sus valores como notificaciones para el usuario de confianza. Esto asigna básicamente atributos LDAP específicos de un almacén de atributo que se define en un conjunto de notificaciones salientes que puede usarse para la autorización.  
  
Con esta plantilla, puedes agregar varios atributos, que se enviarán como varias solicitudes de una sola regla. Por ejemplo, puedes usar esta plantilla de regla para crear una regla que buscará los valores de atributo para usuarios autenticados desde el **empresa** y **departamento** atributos de Active Directory y, a continuación, enviar estos valores como dos notificaciones salientes diferentes.  
  
También puedes usar esta regla para enviar la pertenencia a grupos de todos los del usuario. Si quieres enviar solo pertenencias a grupos individuales, usa la pertenencia al grupo de enviar como una plantilla de regla de notificación. Para obtener más información, consulta [cuándo usar una pertenencia al grupo de enviar como una regla de Reclamación](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Puedes crear esta regla ya sea mediante el idioma de la regla de reclamación o mediante los atributos de LDAP enviar como notificaciones de plantilla en la administración de AD FS snap\ en la regla. Esta plantilla de regla proporciona las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación  
  
-   Selecciona un almacén de atributo del que extraer los atributos LDAP  
  
-   Asignación de atributos LDAP a tipos de notificaciones salientes  
  
Para obtener más información sobre cómo crear esta regla, consulta [crear una regla para enviar atributos LDAP como notificaciones](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Con el lenguaje de regla de notificación  
Si la consulta a Active Directory, AD DS o Active Directory Lightweight Directory Services \(AD LDS\) debe comparar con un atributo LDAP distinto de **samAccountname**, debes usar una regla personalizada en su lugar. Si no hay ninguna notificación de nombre de cuenta de Windows en el conjunto de entrada, también debes usar una regla personalizada para especificar la reclamación a usar para realizar consultas a AD DS o AD LDS.  
  
Los ejemplos siguientes se proporcionan para ayudarte a comprender algunas de las distintas formas de crear una regla personalizada mediante el lenguaje de regla de notificación de consulta y extrae datos en un almacén de atributo.  
  
### <a name="example-how-to-query-an-ad-lds-attribute-store-and-return-a-specified-value"></a>Ejemplo: Procedimiento para consultar un almacén de atributo de AD LDS y devolver un valor especificado  
Parámetros deben estar separados por punto y coma. El primer parámetro es el filtro LDAP. Los parámetros siguientes son los atributos que se devuelven en todos los objetos coincidentes.  
  
El siguiente ejemplo muestra cómo buscar un usuario por el **sAMAccountName** atributo y emitir una notificación de dirección de correo e\, usando el valor de atributo de correo del usuario:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
El siguiente ejemplo muestra cómo buscar un usuario por el **correo** atributo y emitir reclamaciones de título y el nombre para mostrar, con los valores del usuario **título** y **displayname** atributos:  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
El siguiente ejemplo muestra cómo buscar un usuario por correo electrónico y el título y, a continuación, emitir una reclamación de nombre para mostrar con el usuario **displayname** atributo:  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-and-return-a-specified-value"></a>Ejemplo: Procedimiento para consultar un almacén de atributo de Active Directory y devolver un valor especificado  
La consulta de Active Directory debe incluir el nombre de usuario \ (con el dominio equipo\) como el último parámetro para que el atributo de Active Directory de la tienda puede consultar el dominio correcto. De lo contrario, se admite la misma sintaxis.  
  
El siguiente ejemplo muestra cómo buscar un usuario por el **sAMAccountName** atributo en su dominio y, a continuación, devolver la **correo** atributo:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Ejemplo: Cómo consultar un almacén de atributo de Active Directory en función del valor de una notificación entrante  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
La consulta anterior se compone de las siguientes tres partes:  
  
-   El filtro LDAP, especifican esta parte de la consulta para recuperar los objetos que quieres consultar los atributos. Para obtener información general sobre válido consultas LDAP, consulte RFC 2254. Cuando vas a consultar un almacén de atributo de Active Directory y no se especifica un filtro LDAP, el samAccountName\ = {0} se supone que consulta y el almacén de atributo de Active Directory espera un parámetro que puede incorporarse el valor de {0}. De lo contrario, la consulta se producirá un error. Para un almacén de atributo LDAP distinto de Active Directory, no se pueden omitir el elemento de filtro LDAP de la consulta o la consulta se producirá un error.  
  
-   Especificación de atributo, en esta segunda parte de la consulta, especifica los atributos \ (que son comma\-separados si usas varios values\ atributo) que quieres partir de los objetos filtrados. El número de atributos que especifiques debe coincidir con el número de los tipos de notificación que se definen en la consulta.  
  
-   Dominio de Active Directory, especifique la última parte de la consulta solo cuando la tienda de atributo es Active Directory. \ (No es necesario cuando se consulta otras tiendas de atributo. \) esta parte de la consulta se usa para especificar la cuenta de usuario en el domain\\name del formulario. El almacén de atributo de Active Directory usa la parte del dominio para determinar el controlador de dominio adecuados para conectarse y ejecutar la consulta y solicitar los atributos.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-active-directory"></a>Ejemplo: Cómo usar dos reglas personalizadas para extraer el administrador e\ correo electrónico de un atributo en Active Directory  
Las siguientes dos reglas personalizadas, cuando se usan juntas en el orden en que se muestra a continuación, consultan Active Directory para la **manager** cuenta \(Rule 1\) de atributo del usuario y, a continuación, utiliza ese atributo para consultar la cuenta de usuario del administrador para la **correo** atributo \(Rule 2\). Por último, el **correo** atributo se emite como solicitar un "ManagerEmail". En resumen, la regla 1 consulta Active Directory y pasa el resultado de la consulta regla 2, que, a continuación, extrae el Administrador de valores e\ correo.  
  
Por ejemplo, cuando estas reglas fin su ejecución, se emite una reclamación que contiene la dirección del administrador e\ correo de un usuario en el dominio corp.fabrikam.com.  
  
**Regla de 1**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**Regla 2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> Funcionan estas reglas solo si el administrador del usuario está en el mismo dominio que el usuario \ (corp.fabrikam.com en este example\).  
  
## <a name="additional-references"></a>Referencias adicionales  
[Crear una regla para enviar atributos LDAP como notificaciones](https://technet.microsoft.com/library/dd807115.aspx)  
  

