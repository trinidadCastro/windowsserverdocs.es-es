---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: Cuándo usar una regla de Pasar a través o filtrar una notificación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa205c46bf67dc25a55232b799bdd39fee4ac3c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812216"
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Cuándo usar una regla de Pasar a través o filtrar una notificación
Puede usar esta regla en Active Directory Federation Services \(AD FS\) cuando necesite tomar un tipo de notificación entrante específica y, a continuación, aplicar una acción que determine qué resultado se debe producir en función de los valores de la notificación entrante. Cuando usas esta regla, pasas a través o filtras cualquier notificación que coincide con la lógica de la regla de la tabla siguiente, según cualquiera de las opciones que configures en la regla.  
  
|Opción de regla|Lógica de regla|  
|---------------|--------------|  
|Pasar a través todos los valores de notificaciones|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor es igual a *cualquier valor*, pasa a través la notificación|  
|Pasar a través solo un valor de notificación determinado|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor es igual a *valor de notificación determinado*, pasa a través la notificación|  
|Pasar a través de los valores de notificación que coincidan con un determinado e\-valor de sufijo de correo|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor es igual a *valor de sufijo determinado*, pasa a través la notificación|  
|Pasar a través solo valores de notificación que empiecen con un valor determinado|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor empieza por *valor de notificación determinado*, pasa a través la notificación|  
  
Las secciones siguientes ofrecen una introducción básica a las reglas de notificación y proporcionan más detalles sobre cuándo utilizar esta regla.  
  
## <a name="about-claim-rules"></a>Sobre las reglas de notificación  
Una regla de notificación representa una instancia de la lógica de negocios que se tomará una notificación entrante, se aplicará una condición \(if x, entonces y\) y generará una notificación saliente basándose en los parámetros de condición. La lista siguiente destaca las sugerencias importantes que deberías conocer sobre las reglas de notificaciones antes de seguir leyendo este tema:  
  
-   En el complemento Administración de AD FS\-, notificación solo se pueden crear reglas mediante las plantillas de regla de notificación  
  
-   El proceso de reglas de notificación entrante de notificaciones directamente desde un proveedor de notificaciones \(como Active Directory u otro servicio de federación\) o desde la salida de la aceptación de las reglas en una confianza de proveedor de notificaciones de transformación.  
  
-   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una precedencia en las reglas, puedes perfeccionar o filtrar aún más las notificaciones que generan las reglas anteriores dentro de un conjunto determinado de reglas.  
  
-   Siempre tendrás que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre las reglas de notificación y conjuntos de reglas de notificación, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Para obtener más información cómo se procesan los conjuntos de reglas de notificación, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pasar a través todos los valores de notificaciones  
Cuando se usa esta acción, todos los valores de notificación entrante del tipo de notificación determinado se pasan a través como notificaciones salientes. Por ejemplo, cuando se especifica el tipo de notificación entrante como el tipo de notificación de rol, todos los valores de notificación entrante se copian individualmente en nuevas notificaciones salientes con el tipo de notificación saliente del rol.  
  
## <a name="filtering-a-claim"></a>Filtrado de una notificación  
En AD FS, el término *filtrado de notificaciones* valores de notificación de medios para filtrar o restringir entrantes para que solo ciertos valores se pasan o se envían a través como notificaciones salientes. Lo que hace posible esta función es la plantilla de reglas **Pasar a través o filtrar una notificación entrante**. Dentro de las propiedades de esta regla, puedes establecer condiciones para filtrar los valores de entrada para que solo se pasen a través los valores que cumplen los criterios especificados.  
  
Por ejemplo, puedes usar esta regla para solo pasar a través de notificaciones que coinciden con el valor de notificación del comprador cuando el tipo de notificación entrante coincide con tipo de notificación de rol o también puede que quieras emitir solo las notificaciones sobre el nombre del usuario, pero no las notificaciones que contienen el número de seguridad social del usuario.  
  
Cuando usas una condición de filtro con esta regla, se examinan todas las notificaciones entrantes para determinar qué notificaciones coinciden con los criterios establecidos por la regla. Se omiten todas las demás notificaciones para que solo se pasen a través los valores de notificación determinados que coinciden con un tipo de notificación en concreto.  
  
Por ejemplo, como se muestra en la siguiente ilustración, cuando se establece una regla con la condición para filtrar las notificaciones solo entrantes que están organizadas en el UPN tipo de notificación y terminar también con @fabrikam.com, se omiten todas las demás solicitudes entrantes a menos que cumplan este criterio. Esto incluye la notificación entrante con el tipo de notificación de E\-dirección de correo electrónico incluso después de que su valor de notificación termina en @fabrikam.com. En este caso, solo la notificación que contiene el valor de Nick@fabrikam.com se envía al usuario autenticado.  
  
![Cuándo usar pase a través de](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurar esta regla en una confianza de proveedor de notificaciones  
Cuando usas una confianza de proveedores de notificaciones, esta regla se puede configurar para pasar a través solo notificaciones entrantes del proveedor de notificaciones que coincidan con determinadas restricciones. Por ejemplo, desea aceptar sólo e\-notificaciones de correo electrónico del proveedor de notificaciones; por lo tanto, podría usar esta plantilla de regla para aceptar e\-tipos de notificación que terminan en el sistema de nombres de dominio del proveedor de notificaciones de correo \(DNS\) nombre.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurar esta regla en una relación de confianza para usuario autenticado  
Cuando usas una relación de confianza para usuario autenticado, esta regla se puede configurar para pasar a través o filtrar las notificaciones salientes que se enviarán al usuario de confianza. Es posible que algunos usuarios de confianza no entiendan ciertos tipos de notificación o que ciertas notificaciones contengan información confidencial que no se deba enviar a ciertos usuarios de confianza. Esta plantilla de reglas puede ayudar a aplicar esas directivas a una determinada entidad de confianza.  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Crear esta regla mediante el lenguaje de reglas de notificación o con el paso a través o filtrar una plantilla de regla de notificación entrante en el complemento Administración de AD FS\-en. Esta plantilla de regla permite las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación.  
  
-   Especificar un tipo de notificación entrante  
  
-   Pasar a través todos los valores de notificaciones  
  
-   Pasar a través solo un valor de notificación determinado  
  
-   Pasar a través de los valores de notificación que coincidan con un determinado e\-valor de sufijo de correo  
  
-   Pasar a través solo valores de notificación que empiecen con un valor determinado  
  
Para obtener más instrucciones sobre cómo crear esta plantilla, consulte [cree una regla de paso a través o filtrar una notificación entrante](https://technet.microsoft.com/library/dd807060.aspx) en la Guía de implementación de AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación  
Si solo se debe enviar una notificación cuando el valor de notificación coincida con un patrón personalizado, tienes que usar una regla personalizada. Para obtener más información, consulta «Cuándo usar una regla personalizada».  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Ejemplos de cómo crear una sintaxis de regla para Pasar a través o filtrar  
Una regla de filtrado sencilla podría filtrar notificaciones basadas en una de las propiedades descritas anteriormente. Por ejemplo, la regla siguiente pasará a través de todos los e\-notificaciones de correo:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
Los filtros pueden ser lógicamente AND\-ed juntos. Por ejemplo, la siguiente regla aceptará todas\-notificaciones con el valor de correo johndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
En los ejemplos anteriores, los filtros siempre usan un operador de igualdad. El lenguaje de reglas de notificación admite los operadores siguientes:  
  
-   \=\= \- es igual a \(caso\-confidenciales\)  
  
-   \!\= \- no es igual a \(caso\-confidenciales\)  
  
-   \=~\- coincidencia de expresión regular  
  
-   \!~ \- expresión regular no\-coincide con  
  
Por ejemplo, la siguiente regla aceptará todas\-correo electrónico no emitidas por el servidor de federación local de notificaciones que tienen un sufijo de boeing.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedimientos recomendados para crear reglas personalizadas  
Se puede aplicar un filtro a una o varias de las propiedades de cada notificación, tal y como se describe en la tabla siguiente.  
  
|Propiedad de notificación|Descripción|  
|------------------|---------------|  
|Tipo|El tipo de notificación \(normalmente se representa como un Uri\) refleja un acuerdo implícito entre los asociados de una federación sobre qué tipo de información se muestra en la notificación. Por ejemplo, notificaciones de tipo http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identidad\/notificaciones\/emailaddress contendrá la e\-dirección del usuario de correo.|  
|Valor|El valor de la notificación. Por ejemplo, una notificación de tipo http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identidad\/notificaciones\/emailaddress puede tener un valor de johndoe@fabrikam.com|  
|ValueType|El tipo de valor representa cómo hay que interpretar la información contenida en el valor de la notificación. Normalmente, el tipo de valor se establecerá en http:\/\/www.w3.org\/2001\/XMLSchema\#cadena, pero el valor de notificación podría contener datos codificados de Base64Binary \(por ejemplo, una imagen \) o fecha, Boolean y así sucesivamente.|  
|Emisor|El emisor representa la entidad que emitió por última vez las notificaciones sobre el usuario. Si las notificaciones se obtienen del servidor de federación del emisor de notificaciones, el proveedor de todas las notificaciones se establecerá como "Autoridad local". Si un servidor de federación Proveedor de notificaciones recibió las notificaciones, se establecerá el emisor de las notificaciones para el identificador de proveedor de notificaciones del proveedor de notificaciones que firmó el token. Por lo tanto, al procesar las reglas de las notificaciones que recibieron por parte de un proveedor de notificaciones, el emisor de todas las notificaciones se establecerá en el mismo valor. Al crear reglas para un usuario de confianza, se puede usar la propiedad emisora para distinguir entre notificaciones procedentes de proveedores de notificaciones diferentes.|  
|Emisor original|Esta propiedad de notificación está destinada a transmitir qué servidor de federación emitió originalmente la notificación. Puesto que la propiedad del emisor de notificaciones se establece en el último servidor de federación que firmó el token, emisor original es útil en escenarios donde ha fluido una notificación a través de más de un servidor de federación \(por ejemplo, un usuario de confianza que recibe un token un proveedor de federación de servidor de federación podría estar interesado qué servidor de federación del proveedor de notificaciones autentican al usuario\)|  
|Propiedades|Además de las cinco propiedades descritas anteriormente, todas las notificaciones tienen también una bolsa de propiedades donde se pueden almacenar propiedades con nombre. Estas propiedades no se encuentran en serie en el token y solo tienen sentido para pasar información entre los componentes de la canalización de emisión de notificaciones dentro del ámbito de un servidor de federación único. Por ejemplo, establecer una propiedad durante el procesamiento de las reglas del proveedor de notificaciones y después hacer referencia a ella en las reglas del usuario de confianza.|  
  

