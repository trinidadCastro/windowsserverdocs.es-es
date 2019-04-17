---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: "Cuándo usar uno de paso o la regla de filtro de notificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa205c46bf67dc25a55232b799bdd39fee4ac3c6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Cuándo usar uno de paso o la regla de filtro de notificación
Puedes usar esta regla en servicios de federación de Active Directory \(AD FS\) cuando debas tomar un tipo de notificación entrante específico y, a continuación, aplicar una acción que puede determinar qué resultado debería ocurrir en función de los valores de la notificación entrante. Cuando usas esta regla, puedes pasan o filtrar cualquier reclamación que coincidan con la lógica de la regla en la tabla siguiente, en función de cualquiera de las opciones que se configura en la regla.  
  
|Opción de regla|Lógica de la regla|  
|---------------|--------------|  
|Pasar por todos los valores de las notificaciones|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *cualquier valor*, a continuación, pasa la solicitud a través de|  
|Pasar solo un determinado valor de notificación|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *especifica el valor de notificación*, a continuación, pasa la solicitud a través de|  
|Pasar solo valores de las notificaciones que coinciden con un valor de sufijo correo e\ específico|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *especifica el valor de sufijo*, a continuación, pasa la solicitud a través de|  
|Pasar a través de los valores de notificación que comienzan con un valor específico|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y valor comienza con *especifica el valor de notificación*, a continuación, pasa la solicitud a través de|  
  
Las siguientes secciones proporcionan una introducción básica para reclamar reglas y proporcionar más detalles sobre cuándo usar esta regla.  
  
## <a name="about-claim-rules"></a>Acerca de las reglas de notificación  
Una regla de Reclamación representa una instancia de la lógica de negocios que tomar una notificación entrante, aplicar una condición a ella \ (si está x después y\) y generar una notificación saliente basándose en los parámetros de la condición. La siguiente lista describe sugerencias que debes conocer reclamar reglas antes de leer más en este tema:  
  
-   En la administración de FS anuncios en snap\, reglas de notificación solo se pueden crear mediante plantillas de notificación de regla  
  
-   Proceso de reglas de notificación entrante dice directamente desde un proveedor de notificaciones \ (como Active Directory u otro Service\ federación) o desde la salida de la aceptación, transforman las reglas en una relación de confianza del proveedor de notificaciones.  
  
-   Reglas de notificación se procesan el motor de emisión de notificaciones en orden cronológico dentro de un conjunto de reglas determinado. Estableciendo la prioridad de las reglas de aún más puede restringir o filtrar notificaciones que se generan mediante reglas anteriores dentro de un conjunto de reglas determinado.  
  
-   Plantillas de notificación de regla siempre requieren que especifiques un tipo de notificación entrante. Sin embargo, puede procesar varios valores de las notificaciones con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre la reclamación y reclamación conjuntos de reglas, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información, cómo se procesan los conjuntos de reglas de notificación, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pasar por todos los valores de las notificaciones  
Al usar esta acción, todos los valores de notificación entrante para el tipo de notificación especificada se pasan como notificaciones salientes. Por ejemplo, cuando se especifica el tipo de notificación entrante como el tipo de notificación de rol, todos los valores de las notificaciones entrantes se copian individualmente en notificaciones salientes nuevo con el tipo de notificación saliente de función.  
  
## <a name="filtering-a-claim"></a>Filtrado de una reclamación  
En AD FS, el término *dice filtrado* medio para filtrar o restringir entrante valores de notificaciones para que solo determinados valores se pasan o se envían a través de como notificaciones salientes. Es el **paso a través o una notificación entrante de filtro** plantilla de regla que permite esta función. Dentro de las propiedades de esta regla, puedes establecer condiciones para filtrar los valores de entrada para que solo los valores que cumplen tus criterios especificados se pasan sin.  
  
Por ejemplo, puedes usar esta regla solo pase a través de notificaciones que coincidan con el valor de Reclamación de comprador cuando entrante reclamar coincidencias de tipo el tipo de notificación de función o quiera emitir solo notificaciones sobre el nombre del usuario, pero no las reclamaciones que contiene el número de la seguridad social del usuario.  
  
Cuando usas una condición de filtro con esta regla, se examinan todas las notificaciones entrantes para determinar qué notificaciones coinciden con los criterios establecidos por la regla. Para que solo los valores de notificación especificada que coinciden con un tipo de notificación seleccionado se pasarán, se omiten todas las demás reclamaciones.  
  
Por ejemplo, como se muestra en la ilustración siguiente, cuando se establece una regla con la condición para filtrar notificaciones entrantes solo adaptadas al UPN tipo de notificación y también de terminar con @fabrikam.com, se omiten todas las demás reclamaciones entrantes a menos que cumplan estos criterios. Esto incluye la notificación entrante con el tipo de notificación de dirección de correo de E\ incluso si su valor reclamación termina en @fabrikam.com. En este caso, solo la reclamación que contiene el valor de Nick@fabrikam.com se envía al usuario de confianza.  
  
![Cuándo usar pasada a través de](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuración de esta regla en una relación de confianza del proveedor de notificaciones  
Cuando usas una confianza de proveedor de reclamaciones, esta regla puede configurarse para atravesar entrante solo notificaciones desde el proveedor de notificaciones que coinciden con ciertas limitaciones. Por ejemplo, es posible que quieres Aceptar sólo reclamaciones e\ correo desde el proveedor de notificaciones; por lo tanto, deberías usar esta plantilla de regla para aceptar los tipos de notificación e\ correo que terminan en sistema de nombres de dominio \(DNS\) nombre del proveedor de notificaciones.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configuración de esta regla en una confianza de terceros de confianza  
Cuando usas una confianza de terceros de confianza, esta regla puede configurarse para atravesar o filtrar notificaciones salientes que se enviarán a la parte del usuario de confianza. Algunas partes de usuario de confianza quizás no comprenden determinados tipos de notificación o ciertas notificaciones pueden contener información confidencial que no debe enviarse a determinadas partes de usuario de confianza. Esta plantilla de regla puede ayudar a aplicar las directivas para una confianza de terceros de confianza particular.  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Puedes crear esta regla ya sea con el idioma de la regla de reclamación o mediante la pasa a través de o filtrar una plantilla de regla de notificación entrante en la administración de AD FS en snap\. Esta plantilla de regla proporciona las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación  
  
-   Especificar un tipo de notificación entrante  
  
-   Pasar por todos los valores de las notificaciones  
  
-   Pasar solo un determinado valor de notificación  
  
-   Pasar solo valores de las notificaciones que coinciden con un valor de sufijo correo e\ específico  
  
-   Pasar a través de los valores de notificación que comienzan con un valor específico  
  
Para obtener más instrucciones sobre cómo crear esta plantilla, consulta [filtrar una notificación entrante o crear una regla para pasar a través de](https://technet.microsoft.com/library/dd807060.aspx) en la Guía de implementación de AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Con el lenguaje de regla de notificación  
Si solo cuando el valor de notificación coincide con un patrón personalizado, se debe enviar una reclamación, debes usar una regla personalizada. Para obtener más información, consulta cuándo usar una regla personalizada.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Ejemplos de cómo crear uno de paso o sintaxis de regla de filtro  
Una regla de filtrado simple podría filtrar reclamaciones basadas en una de las propiedades descritas anteriormente. Por ejemplo, la siguiente regla se pasa a través de todas las reclamaciones e\ correo:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
Filtros pueden ser lógicamente y\ ed juntos. Por ejemplo, la siguiente regla aceptará todas las reclamaciones e\ correo con valorjohndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
En los ejemplos anteriores, los filtros siempre usan un operador de igualdad. El idioma de la regla de notificación es compatible con los operadores siguientes:  
  
-   \ = \ = \-es igual a \(case\-sensitive\)  
  
-   \! \ = \-no es igual a \(case\-sensitive\)  
  
-   \ = ~ \-coincidencia de expresión regular  
  
-   \! ~ \-coincidencia non\ de expresión regular  
  
Por ejemplo, la siguiente regla aceptará todas las reclamaciones e\ correo no emitidas por el servidor de federación locales que tienen un sufijo de boeing.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedimientos recomendados para crear reglas personalizadas  
Un filtro puede aplicarse a una o varias de las propiedades de cada solicitud, como se describe en la siguiente tabla.  
  
|Reclama la propiedad|Descripción|  
|------------------|---------------|  
|Tipo|El tipo de notificación \ (normalmente se representa como una Uri\) refleja el acuerdo implícito entre partners en una federación acerca de qué tipo de información se transmite en la notificación. Por ejemplo, reclamaciones de tipo http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress contendrá la dirección de correo de e\ del usuario.|  
|Valor|El valor de dicha reclamación. Por ejemplo, una reclamación de tipo http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress puede tener un valor dejohndoe@fabrikam.com|  
|Tipo de valor|El tipo de valor representa cómo es interpretar la información contenida en un valor de la notificación. Normalmente se establece el tipo de valor en http:///\/ www.w3.org \/2001\/XMLSchema\#string, pero el valor de notificación puede contener datos Base64Binary codificado \ (por ejemplo, una image\) o una fecha, booleano y así sucesivamente.|  
|Emisor|El emisor representa la parte que emitió última las notificaciones sobre el usuario. Si se obtienen las notificaciones en un servidor de federación de proveedor de reclamaciones el emisor de todas las reclamaciones que se va a establecerse en "Autoridad LOCAL". Si se recibieron los créditos por un servidor de federación de proveedor de federación, el emisor de las notificaciones se va a establecerse en el identificador de proveedor de reclamaciones del proveedor de notificaciones que firmar el token. Por lo tanto, cuando se procesan las reglas en reclamaciones recibidos de un proveedor de notificaciones el emisor de todas las reclamaciones que se va a establecer en el mismo valor. Al crear reglas para un usuario de confianza, la propiedad emisor puede usarse para distinguir entre solicitudes procedentes de proveedores de notificaciones diferentes.|  
|OriginalIssuer|Esta propiedad reclamación está pensada para transmitir qué servidor de federación emitió la reclamación. Dado que se establece la propiedad emisor de una reclamación a la última servidor de federación que firmar el token, el emisor original es útil en escenarios donde se ajusta una reclamación a través de más de un servidor de federación \ (por ejemplo, un usuario de confianza que recibe un token de un servidor de federación de proveedor de federación podría interesarte qué particular reclamaciones de servidor de federación de proveedor de autentica la user\)|  
|Propiedades|Además de las propiedades de cinco descritas anteriormente, cada notificación también tiene un contenedor de propiedades que se pueden almacenar propiedades con nombre. Estas propiedades no se serializan en el token y solo tienen sentido para pasar información entre los componentes de la canalización de emisión de notificaciones en el ámbito de un servidor de federación único. Por ejemplo, para establecer una propiedad durante las notificaciones de las reglas del proveedor de procesamiento y, a continuación, hacer referencia a ella en las reglas de terceros de confianza.|  
  

