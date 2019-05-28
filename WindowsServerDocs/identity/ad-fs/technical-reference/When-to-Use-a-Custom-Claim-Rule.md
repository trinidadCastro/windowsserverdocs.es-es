---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: Cuándo usar una regla de notificación personalizada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e566de4df2895dfa2ed1104f85c1429881ff5bbf
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188290"
---
# <a name="when-to-use-a-custom-claim-rule"></a>Cuándo usar una regla de notificación personalizada
Escribir una regla de notificación personalizada en Active Directory Federation Services \(AD FS\) mediante el lenguaje de reglas de notificación, que es el marco de trabajo que utiliza el motor de emisión de notificaciones para generar mediante programación, transformar, pasar a través y filtrar las notificaciones. Mediante el uso de una regla personalizada, puedes crear reglas con una lógica más compleja que una plantilla de regla estándar. Considera el uso de una regla personalizada cuando quieras:  
  
-   Enviar notificaciones basadas en valores extraídos de un lenguaje de consulta estructurado \(SQL\) almacén de atributos.  
  
-   Enviar notificaciones basadas en valores extraídos de un Lightweight Directory Access Protocol \(LDAP\) almacén de atributos con un filtro LDAP personalizado.  
  
-   Enviar notificaciones basadas en valores extraídos de un almacén de atributos personalizado.  
  
-   Enviar notificaciones solo cuando están presentes dos o más notificaciones entrantes.  
  
-   Enviar notificaciones solo cuando un valor de notificación entrante coincida con un patrón complejo.  
  
-   Enviar notificaciones con cambios complejos a un valor de notificación entrante.  
  
-   Crear notificaciones para usarlas solo en reglas más adelante, sin enviar las notificaciones realmente.  
  
-   Construir una notificación saliente desde el contenido de más de una notificación entrante.  
  
También puedes usar una regla personalizada cuando el valor de notificación de la notificación saliente tenga que estar basado en el valor de la solicitud entrante, pero también tenga que incluir contenido adicional.  
  
El lenguaje de reglas de notificación se basa en reglas. Tiene una parte de condición y una parte de ejecución. Puedes usar la sintaxis del lenguaje de reglas de notificación para enumerar, agregar, eliminar o modificar notificaciones para satisfacer las necesidades de tu organización. Para obtener más información acerca de cómo cada uno de estos elementos funcionan, consulte [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
Las secciones siguientes ofrecen una introducción básica a las reglas de notificación. También proporcionan detalles sobre cuándo usar una regla de notificación personalizada.  
  
## <a name="about-claim-rules"></a>Sobre las reglas de notificación  
Una regla de notificación representa una instancia de la lógica de negocios que toma una notificación entrante, se aplicará una condición \(if x, entonces y\) y generará una notificación saliente basándose en los parámetros de condición.  
  
> [!IMPORTANT]  
> -   En el complemento Administración de AD FS\-en, notificaciones de las reglas se pueden crear sólo con las plantillas de regla de notificación  
> -   El proceso de reglas de notificación entrante de notificaciones directamente desde un proveedor de notificaciones \(como Active Directory u otro servicio de federación\) o desde la salida de la aceptación de las reglas en una confianza de proveedor de notificaciones de transformación.  
> -   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una prioridad en las reglas puedes afinar o filtrar aún más las notificaciones generadas por las reglas anteriores dentro de un conjunto determinado de reglas.  
> -   Siempre tienes que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación usando una sola regla.  
  
Para obtener más información sobre las reglas de notificación y conjuntos de reglas de notificación, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Para obtener más información cómo se procesan los conjuntos de reglas de notificación, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Cree esta regla creando primero la sintaxis que necesita para la operación utilizando el lenguaje de reglas de notificación y, a continuación, pegar el resultado en el texto del cuadro que es proporciona en el envío de notificaciones mediante una plantilla de regla personalizada en las propiedades de un proveedor de notificaciones tr ust o un usuario de confianza confiar en el complemento Administración de AD FS\-en.  
  
Esta plantilla de reglas proporciona las siguientes opciones:  
  
-   Especificar un nombre de regla de notificación.  
  
-   Escriba uno o más condiciones opcionales y una declaración de emisión mediante AD FS de lenguaje de reglas de notificación  
  
Para obtener más instrucciones para crear una regla personalizada mediante esta plantilla, consulte [crear una regla para enviar notificaciones mediante una regla personalizada](https://technet.microsoft.com/library/dd807049.aspx) en la Guía de implementación de AD FS.  
  
Para entender mejor cómo funciona el lenguaje de reglas de notificación, ver la sintaxis de lenguaje de reglas de notificación de otras reglas que ya existen en el complemento\-en haciendo clic en el **ver lenguaje de reglas** ficha en las propiedades de esa regla. La información de esta sección y la información de la sintaxis de esta ficha pueden proporcionar información sobre cómo construir tus propias reglas personalizadas.  
  
Para obtener más información sobre cómo usar el lenguaje de reglas de notificación, consulte [The Role of el lenguaje de reglas de notificación](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Por ejemplo: Cómo combinar los nombres y apellidos en función de los valores de atributo de nombre de un usuario  
La sintaxis de regla siguiente combina los nombres y apellidos de los valores de atributo en un almacén de atributos especificado. El motor de directiva forma un producto cartesiano de las coincidencias de todas las condiciones. Por ejemplo, el resultado para el nombre {"Frank", "Alan"} y los apellidos {"Miller", "Shen"} es {"Frank Miller", "Frank Shen", "Alan Miller", "Alan Shen"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Por ejemplo: Cómo emitir una notificación de administrador en función de si los usuarios disponen de informes directos  
La regla siguiente emite una notificación de administrador solo si el usuario tiene informes directos:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Por ejemplo: Cómo emitir una notificación PPID basada en un atributo LDAP  
La siguiente regla emite un identificador de Personal privada \(PPID\) notificación según la **windowsaccountname** y **originalissuer** atributos de usuarios en un atributo LDAP tienda:  
  
```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Los atributos comunes que se pueden usar para identificar de forma única al usuario para esta consulta incluyen lo siguiente:  
  
-   **SID de usuario**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

