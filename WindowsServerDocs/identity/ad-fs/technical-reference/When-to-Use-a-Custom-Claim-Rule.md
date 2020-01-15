---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: Cuándo usar una regla de notificación personalizada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c784c4b6dbfee7034dd9302dc87fc74b896763f5
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950142"
---
# <a name="when-to-use-a-custom-claim-rule"></a>Cuándo usar una regla de notificación personalizada
Escribe una regla de notificación personalizada en Servicios de federación de Active Directory (AD FS) \(AD FS\) mediante el lenguaje de reglas de notificación, que es el marco que usa el motor de emisión de notificaciones para generar, transformar, pasar y filtrar notificaciones mediante programación. Mediante el uso de una regla personalizada, puedes crear reglas con una lógica más compleja que una plantilla de regla estándar. Considera el uso de una regla personalizada cuando quieras:  
  
-   Enviar notificaciones basadas en valores que se extraen de un Lenguaje de consulta estructurado \(almacén de atributos de SQL\).  
  
-   Enviar notificaciones basadas en valores que se extraen de un protocolo ligero de acceso a directorios \(el almacén de atributos\) LDAP mediante un filtro LDAP personalizado.  
  
-   Enviar notificaciones basadas en valores extraídos de un almacén de atributos personalizado.  
  
-   Enviar notificaciones solo cuando están presentes dos o más notificaciones entrantes.  
  
-   Enviar notificaciones solo cuando un valor de notificación entrante coincida con un patrón complejo.  
  
-   Enviar notificaciones con cambios complejos a un valor de notificación entrante.  
  
-   Crear notificaciones para usarlas solo en reglas más adelante, sin enviar las notificaciones realmente.  
  
-   Construir una notificación saliente desde el contenido de más de una notificación entrante.  
  
También puedes usar una regla personalizada cuando el valor de notificación de la notificación saliente tenga que estar basado en el valor de la solicitud entrante, pero también tenga que incluir contenido adicional.  
  
El lenguaje de reglas de notificación se basa en reglas. Tiene una parte de condición y una parte de ejecución. Puedes usar la sintaxis del lenguaje de reglas de notificación para enumerar, agregar, eliminar o modificar notificaciones para satisfacer las necesidades de tu organización. Para obtener más información sobre cómo funciona cada uno de estos elementos, vea [el rol del lenguaje de reglas de notificaciones](The-Role-of-the-Claim-Rule-Language.md).  
  
En las secciones siguientes se ofrece una introducción básica a las reglas de notificaciones. También proporcionan detalles sobre cuándo usar una regla de notificación personalizada.  
  
## <a name="about-claim-rules"></a>Acerca de las reglas de notificaciones  
Una regla de notificaciones representa una instancia de la lógica de negocios que toma una solicitud entrante, le aplica una condición \(si x, entonces y\) y produce una solicitud saliente basada en los parámetros de la condición.  
  
> [!IMPORTANT]  
> -   En el\-del complemento de administración de AD FS en, las reglas de notificaciones solo se pueden crear mediante plantillas de reglas de notificaciones.  
> -   Las reglas de notificación procesan las notificaciones entrantes directamente desde un proveedor de notificaciones \(como Active Directory u otro Servicio de federación\) o desde la salida de las reglas de transformación de aceptación en una confianza de proveedor de notificaciones.  
> -   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico, dentro de un conjunto determinado de reglas. Al establecer una prioridad en las reglas puedes afinar o filtrar aún más las notificaciones generadas por las reglas anteriores dentro de un conjunto determinado de reglas.  
> -   Siempre tienes que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación usando una sola regla.  
  
Para obtener información más detallada sobre las reglas de notificaciones y los conjuntos de reglas de notificaciones, consulte [el rol de las reglas de notificaciones](The-Role-of-Claim-Rules.md). Para obtener más información sobre cómo se procesan las reglas, vea [el rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información sobre cómo se procesan los conjuntos de reglas de notificaciones, vea [el rol de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Puedes crear esta regla creando primero la sintaxis que necesitas para tu operación mediante el lenguaje de reglas de notificación y, después, pegando el resultado en el cuadro de texto que se proporciona en la plantilla enviar notificaciones mediante una regla personalizada en las propiedades de una confianza de proveedor de notificaciones o una relación de confianza para usuario autenticado en el\-de administración de AD FS en.  
  
Esta plantilla de reglas proporciona las siguientes opciones:  
  
-   Especificar un nombre de regla de notificaciones  
  
-   Escriba una o más condiciones opcionales y una declaración de emisión mediante el lenguaje de reglas de notificaciones de AD FS  
  
Para obtener más instrucciones sobre cómo crear una regla personalizada mediante esta plantilla, consulte [creación de una regla para enviar notificaciones mediante una regla personalizada](https://technet.microsoft.com/library/dd807049.aspx) en la guía de implementación de AD FS.  
  
Para comprender mejor cómo funciona el lenguaje de reglas de notificaciones, consulte la sintaxis del lenguaje de reglas de notificaciones de otras reglas que ya existen en el\-de ajuste de; para ello, haga clic en la pestaña **Ver lenguaje de reglas** en las propiedades de esa regla. La información de esta sección y la información de la sintaxis de esta ficha pueden proporcionar información sobre cómo construir tus propias reglas personalizadas.  
  
Para obtener más información sobre cómo usar el lenguaje de reglas de notificaciones, vea [el rol del lenguaje de reglas de notificaciones](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de reglas de notificaciones  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Ejemplo: Cómo combinar los nombres y apellidos en función de los valores de atributo de nombre de un usuario  
La sintaxis de regla siguiente combina los nombres y apellidos de los valores de atributo en un almacén de atributos especificado. El motor de directiva forma un producto cartesiano de las coincidencias de todas las condiciones. Por ejemplo, el resultado para el nombre {"Frank", "Alan"} y los apellidos {"Miller", "Shen"} es {"Frank Miller", "Frank Shen", "Alan Miller", "Alan Shen"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Ejemplo: Cómo emitir una notificación de administrador en función de si los usuarios disponen de informes directos  
La regla siguiente emite una notificación de administrador solo si el usuario tiene informes directos:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Ejemplo: Cómo emitir una notificación PPID basada en un atributo LDAP  
La siguiente regla emite un identificador personal privado \(PPID\) Claim en función de los atributos **atributos windowsaccountname** y **originalissuer** de los usuarios de un almacén de atributos LDAP:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Los atributos comunes que se pueden usar para identificar de forma única al usuario para esta consulta incluyen lo siguiente:  
  
-   **SID de usuario**  
  
-   **atributos windowsaccountname**  
  
-   **atributo**  
  

