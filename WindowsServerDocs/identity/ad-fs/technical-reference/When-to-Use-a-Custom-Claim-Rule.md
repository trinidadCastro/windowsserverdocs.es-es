---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: "Cuándo usar una regla de notificación personalizada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5398f0b10d9e62548145fdde0354a3d047eb0ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>Cuándo usar una regla de notificación personalizada
Puedes escribir una regla de notificación personalizada en \(AD FS\) los servicios de federación de Active Directory mediante el lenguaje de regla de notificación, que es el marco de trabajo que el motor de emisión de notificaciones que se usa para generar mediante programación, transformar, pasar y filtrar notificaciones. Al usar una regla personalizada, puedes crear reglas con una lógica compleja más que una plantilla de regla estándar. Considera la posibilidad de usar una regla personalizada cuando quieras:  
  
-   Enviar notificaciones basadas en valores que se extraen de un almacén de atributo de lenguaje de consulta estructurado \(SQL\).  
  
-   Enviar notificaciones basadas en valores que se extraen de un almacén de atributo de protocolo ligero de acceso a directorios \(LDAP\) con un filtro personalizado de LDAP.  
  
-   Enviar notificaciones basadas en valores que se extraen de un almacén de atributo personalizado.  
  
-   Enviar notificaciones solo cuando están presentes dos o más notificaciones entrantes.  
  
-   Enviar notificaciones solo cuando un entrante reclamar valor coincide con un patrón complejo.  
  
-   Enviar notificaciones con cambios complejas para un entrante el valor de notificación.  
  
-   Crear notificaciones para su uso solo en las reglas posteriores, sin necesidad de enviar realmente las notificaciones.  
  
-   Crear una notificación saliente del contenido de más de una notificación entrante.  
  
También puedes usar una regla personalizada cuando el valor de la notificación de la solicitud saliente debe basarse en el valor de la notificación entrante, pero también debe incluir contenido adicional.  
  
El idioma de la regla de notificaciones es regla basada. Tiene una parte de la condición y una parte de ejecución. Puedes usar la sintaxis de lenguaje de regla de notificación para enumerar, agregar, eliminar o modificar las reclamaciones para satisfacer las necesidades de la organización. Para obtener más información acerca de cómo cada uno de estos elementos funciona, consulta [el rol del lenguaje de regla reclamación](The-Role-of-the-Claim-Rule-Language.md).  
  
Las siguientes secciones proporcionan una introducción básica reglas de notificación. También ofrecen detalles sobre cuándo usar una regla de notificación personalizada.  
  
## <a name="about-claim-rules"></a>Acerca de las reglas de notificación  
Una regla de Reclamación representa una instancia de la lógica de negocios que toma una notificación entrante, aplicar una condición a ella \ (si está x, a continuación, y\) y generar una notificación saliente basándose en los parámetros de la condición.  
  
> [!IMPORTANT]  
> -   En la administración de FS anuncios en snap\, reclamar se pueden crear reglas solo con plantillas de regla de notificación  
> -   Proceso de reglas de notificación entrante dice directamente desde un proveedor de notificaciones \ (como Active Directory u otro Service\ federación) o desde la salida de la aceptación, transforman las reglas en una relación de confianza del proveedor de notificaciones.  
> -   Reglas de notificación se procesan el motor de emisión de notificaciones en orden cronológico dentro de un conjunto de reglas determinado. Al establecer la prioridad en las reglas, puedes aún más refinar o filtrar notificaciones que se generan mediante reglas anteriores dentro de un conjunto de reglas determinado.  
> -   Plantillas de regla de Reclamación siempre requieren que especifiques un tipo de notificación entrante. Sin embargo, puede procesar varios valores de las notificaciones con el mismo tipo de notificación mediante el uso de una sola regla.  
  
Para obtener más información sobre la reclamación y reclamación conjuntos de reglas, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información, cómo se procesan los conjuntos de reglas de notificación, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Se crea esta regla mediante la creación de la sintaxis que necesitas para la operación con el lenguaje de regla de la reclamación y, a continuación, pegando el resultado en el cuadro de texto que se proporciona en el envío un notificaciones usando una plantilla de regla personalizada de las propiedades de un proveedor de notificaciones de confianza o confiar en un usuario de confianza en la administración de AD FS snap\ en primera.  
  
Esta plantilla de regla proporciona las siguientes opciones:  
  
-   Especificar un nombre de regla de notificación  
  
-   Escribe una o más condiciones opcionales y una instrucción de emisión con la de AD FS reclamar el idioma de la regla  
  
Para obtener más instrucciones para crear una regla personalizada mediante esta plantilla, consulta [crear una regla para enviar notificaciones mediante una regla personalizada](https://technet.microsoft.com/library/dd807049.aspx) en la Guía de implementación de AD FS.  
  
Para comprender mejor cómo funciona el idioma de la regla de notificación, ver la sintaxis de lenguaje de regla de Reclamación de las demás reglas que ya existen en el snap\ haciendo clic en el **Ver regla idioma** ficha en las propiedades para dicha regla. Usar la información de esta sección y la información de sintaxis en esta pestaña, puede proporcionar información sobre cómo crear sus propias reglas personalizadas.  
  
Para obtener más información sobre cómo usar el lenguaje de regla de notificación, consulta [el rol del lenguaje de regla reclamar](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Con el lenguaje de regla de notificación  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Ejemplo: Cómo combinar los nombres y los apellidos basados en valores de atributo de nombre de un usuario  
La siguiente sintaxis de regla combina los nombres y los apellidos de valores de atributo en un almacén de atributo determinado. El motor de directiva constituye un producto cartesiano de las coincidencias para cada condición. Por ejemplo, el resultado para {"Frank", "Alan"} nombre y apellidos {"Miller", "García"} es {"Frank Miller", "Frank Shen", "Alan Miller", "Bernardo García"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Ejemplo: Procedimiento para emitir una reclamación de administrador en función de si los usuarios tienen informes directos  
La siguiente regla emite una reclamación de administrador solo si el usuario tiene informes directos:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Ejemplo: Procedimiento para emitir una reclamación de p pid basándose en un atributo LDAP  
La siguiente regla emite una reclamación de identificador de Personal privada \(PPID\) en función de la **windowsaccountname** y **originalissuer** atributos de los usuarios en un almacén de atributo LDAP:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Atributos comunes que pueden usarse para identificar al usuario para esta consulta incluyen lo siguiente:  
  
-   **SID del usuario**  
  
-   **windowsaccountname**  
  
-   **sAMAccountName**  
  

