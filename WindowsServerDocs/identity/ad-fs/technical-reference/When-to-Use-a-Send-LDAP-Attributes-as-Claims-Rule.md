---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: Cuándo usar una regla de envío de atributos LDAP como notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bc7b98a5d65bf03eb11ed57a8a5f9a0ea696df19
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853778"
---
# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Cuándo usar una regla de envío de atributos LDAP como notificaciones
Puede usar esta regla en Servicios de federación de Active Directory (AD FS) \(AD FS\) cuando desee emitir notificaciones salientes que contengan los valores de atributo del Protocolo ligero de acceso a directorios \(LDAP\) que existan en un almacén de atributos y, a continuación, asociar un tipo de notificación a cada uno de los atributos LDAP. Para obtener más información sobre los almacenes de atributos, vea [el rol de almacenes de atributos](The-Role-of-Attribute-Stores.md).  
  
Cuando se usa esta regla, se emite una notificación para cada atributo LDAP especificado que coincide con la lógica de la regla, como se describe en la tabla siguiente.  
  
|Opción de regla|Lógica de regla|  
|---------------|--------------|  
|Asignación de atributos LDAP a tipos de notificación saliente|Si el almacén de atributos es igual al *almacén de atributos especificado* y el atributo LDAP es igual al *valor especificado*, asigne el valor del atributo LDAP al tipo de *notificación saliente especificado* y emita la notificación.|  
  
Las secciones siguientes ofrecen una introducción básica a las reglas de notificación. También se proporcionan detalles sobre cuándo se debe usar le regla de envío de atributos LDAP como notificaciones.  
  
## <a name="about-claim-rules"></a>Sobre las reglas de notificación  
Una regla de notificaciones representa una instancia de la lógica de negocios que tomará una solicitud entrante, le aplicará una condición \(si x, entonces y\) y genera una notificaciones salientes en función de los parámetros de la condición. La lista siguiente destaca las sugerencias importantes que deberías conocer sobre las reglas de notificaciones antes de seguir leyendo este tema:  
  
-   En el\-del complemento de administración de AD FS en, las reglas de notificaciones solo se pueden crear mediante plantillas de regla de notificaciones.  
  
-   Las reglas de notificación procesan las notificaciones entrantes directamente desde un proveedor de notificaciones \(como Active Directory u otro Servicio de federación\) o desde la salida de las reglas de transformación de aceptación en una confianza de proveedor de notificaciones.  
  
-   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una precedencia en las reglas, puedes perfeccionar o filtrar aún más las notificaciones que generan las reglas anteriores dentro de un conjunto determinado de reglas.  
  
-   Siempre tendrás que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación con una sola regla.  
  
Para obtener información más detallada sobre las reglas de notificaciones y los conjuntos de reglas de notificaciones, consulte [el rol de las reglas de notificaciones](The-Role-of-Claim-Rules.md). Para obtener más información sobre cómo se procesan las reglas, vea [el rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información sobre cómo se procesan los conjuntos de reglas de notificaciones, vea [el rol de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Asignación de atributos LDAP a tipos de notificación saliente  
Cuando se usa la plantilla de regla enviar atributos LDAP como notificaciones, puede seleccionar atributos de un almacén de atributos LDAP, como Active Directory o Active Directory Domain Services \(AD DS\) para enviar sus valores como notificaciones al usuario de confianza. Básicamente, se asignan atributos LDAP específicos de un almacén de atributos definido por el usuario a un conjunto de notificaciones salientes que pueden usarse para la autorización.  
  
Esta plantilla permite agregar varios atributos, que se enviarán como notificaciones múltiples, de una sola regla. Por ejemplo, puede usar esta plantilla de regla para crear una regla que buscará valores de atributo para los usuarios autenticados de los atributos **company** y **departament** de Active Directory y, después, enviará estos valores como dos notificaciones salientes diferentes.  
  
También puede usar esta regla para enviar todas las pertenencias a grupos del usuario. Si desea enviar solo pertenencias a grupos individuales, use la plantilla de regla de envío de pertenencia a grupos como una notificación. Para obtener más información, consulte [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Puede crear esta regla mediante el lenguaje de reglas de notificación o usando la plantilla de reglas enviar atributos LDAP como notificaciones en el\-de AD FS de administración en. Esta plantilla de regla permite las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación.  
  
-   Seleccionar un almacén de atributos del que extraer los atributos LDAP  
  
-   Asignación de atributos LDAP a tipos de notificación saliente  
  
Para obtener más información sobre cómo crear esta regla, vea [crear una regla para enviar atributos LDAP como notificaciones](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación  
Si la consulta que se va a Active Directory, AD DS o Active Directory Lightweight Directory Services \(AD LDS\) debe compararse con un atributo LDAP distinto de **samAccountname**, debe usar una regla personalizada en su lugar. Si no hay ninguna notificación de nombre de cuenta de Windows en el conjunto de entrada, también debe usar una regla personalizada para especificar la notificación que se usará para consultar AD DS o AD LDS.  
  
Los ejemplos siguientes se proporcionan para ayudarle a comprender algunas de las distintas formas de que crear una regla personalizada mediante el lenguaje de reglas de notificación para la consulta y extracción de datos en un almacén de atributos.  
  
### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>Ejemplo: Cómo consultar un almacén de atributos de AD LDS y devolver un valor especificado  
Los parámetros deben estar separados por un punto y coma. El primer parámetro es el filtro LDAP. Los parámetros siguientes son los atributos para devolver en cualquier objeto coincidente.  
  
En el ejemplo siguiente se muestra cómo buscar un usuario por el atributo **samAccountName** y emitir una\-notificaciones de dirección de correo electrónico con el valor del atributo de correo del usuario:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
En el ejemplo siguiente se muestra cómo buscar un usuario por el atributo **mail** y emitir notificaciones de título y nombre para mostrar, usando los valores de los atributos **title** y **displayName** del usuario:  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
En el ejemplo siguiente se muestra cómo buscar un usuario por correo y título y, a continuación, emitir una claim de nombre para mostrar mediante el atributo **displayName** del usuario:  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>Ejemplo: Cómo consultar un almacén de atributos de Active Directory y devolver un valor especificado  
La consulta Active Directory debe incluir el nombre del usuario \(con el nombre de dominio\) como el último parámetro para que el almacén de atributos de Active Directory pueda consultar el dominio correcto. De lo contrario, se admitirá la misma sintaxis.  
  
En el ejemplo siguiente se muestra cómo buscar un usuario por el atributo **sAMAccountName** en su dominio y, luego, devolver el atributo **mail**:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Ejemplo: Cómo consultar un almacén de atributos de Active Directory según el valor de una notificación entrante  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
La consulta anterior se compone de las tres partes siguientes:  
  
-   El filtro LDAP: el usuario especifica esta parte de la consulta para recuperar los objetos cuyos atributos desea consultar. Para obtener información general acerca de las consultas LDAP válidas, vea RFC 2254. Cuando se consulta un almacén de atributos Active Directory y no se especifica un filtro LDAP, se supone la consulta samAccountName\={0} y el almacén de atributos Active Directory espera un parámetro que pueda alimentar el valor de {0}. De lo contrario, la consulta producirá un error. Para un almacén de atributos LDAP distinto de Active Directory, no se puede omitir la parte del filtro LDAP de la consulta o la consulta producirá un error.  
  
-   Especificación de atributos: en esta segunda parte de la consulta, se especifican los atributos \(que se separan\-comas si se usan varios valores de atributo\) que se desea extraer de los objetos filtrados. El número de atributos que especifique debe coincidir con el número de tipos de notificación que defina en la consulta.  
  
-   Dominio de Active Directory: especifique la última parte de la consulta solo si el almacén de atributos es Active Directory. \(no es necesario al consultar otros almacenes de atributos.\) esta parte de la consulta se usa para especificar la cuenta de usuario con el formato dominio\\nombre. El almacén de atributos de Active Directory usa la parte del dominio para determinar el controlador de dominio adecuado para conectarse a la consulta y ejecutarla, así como para solicitar los atributos.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>Ejemplo: Cómo usar dos reglas personalizadas para extraer el administrador e\-correo de un atributo en Active Directory  
Las dos reglas personalizadas siguientes, cuando se usan juntas en el orden que se muestra a continuación, consulta Active Directory para el atributo **Manager** de la cuenta de usuario \(regla 1\) y, a continuación, usar ese atributo para consultar la cuenta de usuario del administrador para el atributo **mail** \(regla 2\). Por último, el atributo **mail** se emite como una demanda "ManagerEmail". En Resumen, la regla 1 consulta Active Directory y pasa el resultado de la consulta a la regla 2, que luego extrae los valores de administrador e\-correo.  
  
Por ejemplo, cuando estas reglas terminan de ejecutarse, se emite una demanda que contiene la dirección de correo electrónico del\-del administrador para un usuario del dominio corp.fabrikam.com.  
  
**Regla 1**  
  
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
> Estas reglas solo funcionan si el administrador del usuario está en el mismo dominio que el usuario \(corp.fabrikam.com en este ejemplo\).  
  
## <a name="additional-references"></a>Referencias adicionales  
[Crear una regla para enviar atributos LDAP como notificaciones](https://technet.microsoft.com/library/dd807115.aspx)  
  

