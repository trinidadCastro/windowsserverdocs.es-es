---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: "Cuándo usar una regla de Reclamación de transformación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c7b7ea2c8d9a08a4cbf6c89c2de2482043efe25b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-transform-claim-rule"></a>Cuándo usar una regla de Reclamación de transformación
Puedes usar esta regla en los servicios de federación de Active Directory \(AD FS\) cuando lo necesites para un tipo de notificación entrante se asignan a un tipo de notificación saliente y, a continuación, aplicar una acción que se determinará qué resultado debería ocurrir en función de los valores que se haya originado en la notificación entrante. Cuando usas esta regla, puedes pasan o transformación notificaciones que coincidan con la siguiente lógica de regla basada en cualquiera de las opciones que configurar en la regla, como se describe en la siguiente tabla.  
  
|Opción de regla|Lógica de la regla|  
|---------------|--------------|  
|Pasar a través de todas las notificaciones entrantes|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *cualquier valor*, a continuación, pasa la notificación a través de saliente igual de tipo de Reclamación *especifica el tipo de notificación*|  
|Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *especifica el valor de notificación*, transforma la solicitud con el nuevo valor de Reclamación saliente *especifica el valor de notificación* y tipo de notificación con saliente *especifica el tipo de notificación*|  
|El reemplazo reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *cualquier valor de sufijo*, transforma la solicitud con el nuevo valor de Reclamación saliente *especifica el valor de sufijo* y tipo de notificación con saliente *especifica el tipo de notificación*|  
  
Las siguientes secciones proporcionan una introducción básica para reclamar reglas y proporcionar más detalles sobre cuándo usar esta regla.  
  
## <a name="about-claim-rules"></a>Acerca de las reglas de notificación  
Una regla de Reclamación representa una instancia de la lógica de negocios que tomar una notificación entrante, aplicar una condición a ella \ (si está x después y\) y generar una notificación saliente basándose en los parámetros de la condición. La siguiente lista describe sugerencias que debes conocer reclamar reglas antes de leer más en este tema:  
  
-   En la administración de FS anuncios en snap\, reglas de notificación solo se pueden crear mediante plantillas de notificación de regla  
  
-   Proceso de reglas de notificación entrante dice directamente desde un proveedor de notificaciones \ (como Active Directory u otro Service\ federación) o desde la salida de la aceptación, transforman las reglas en una relación de confianza del proveedor de notificaciones.  
  
-   Reglas de notificación se procesan el motor de emisión de notificaciones en orden cronológico dentro de un conjunto de reglas determinado. Estableciendo la prioridad de las reglas de aún más puede restringir o filtrar notificaciones que se generan mediante reglas anteriores dentro de un conjunto de reglas determinado.  
  
-   Plantillas de notificación de regla siempre requieren que especifiques un tipo de notificación entrante. Sin embargo, puede procesar varios valores de las notificaciones con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre la reclamación y reclamación conjuntos de reglas, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información, cómo se procesan los conjuntos de reglas de notificación, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pasar por todos los valores de las notificaciones  
Al usar esta acción, todos los valores de notificación entrante adaptadas a un tipo de notificación entrante especificado se asignan a un tipo de notificación saliente especificado antes de que se envían como notificaciones salientes los tokens que estén firmadas por el servicio de federación de.  
  
Por ejemplo, cuando se establece una regla con la **paso a través de todos los valores de notificaciones** la entrada y la lógica de la opción de tipo del grupo de notificación y se especifica el tipo de notificación saliente de función, se copian todos los valores de notificación entrante flujo en desde el emisor individualmente en notificaciones salientes nuevo con el tipo de notificación de función.  
  
## <a name="transforming-a-claim"></a>Transformar una reclamación  
En AD FS, el término *dice transformación* valor con un valor distinto de Reclamación saliente de notificación de medios para reemplazar una entrada. Es la transformación una regla de notificación entrante que hace posible que esta función. Dentro de las propiedades de esta regla, puedes establecer condiciones para transformar los valores de entrada con un valor de Reclamación saliente diferente en función del tipo de notificación entrante especificado.  
  
Por ejemplo, como se muestra en la ilustración siguiente, cuando se establece una regla con la condición para reemplazar un valor de entrada con un valor distinto de Reclamación saliente, todos los tipos de notificación entrante de grupo se asignan a nuevos tipos de solicitud saliente de rol. En este caso, el valor de notificación entrante de comprador se reemplaza con el nuevo valor de la solicitud saliente de administrador.  
  
![Cuándo usar una transformación](media/adfs2_transform.gif)  
  
También puedes usar esta regla para aplicar una condición que va a reemplazar todas las reclamaciones entrantes con un valor de sufijo de correo de e\ especificado con un nuevo valor. Por ejemplo, puedes establecer una condición de esta regla para cambiar todos los valores de las notificaciones con el sufijo de sales.corp.fabrikam.com a fabrikam.com.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuración de esta regla en una relación de confianza del proveedor de notificaciones  
Cuando usas una confianza de proveedor de reclamaciones, esta regla puede configurarse para transformar notificaciones entrantes desde el proveedor de notificaciones en equivalentes de confianza. Los tipos de notificación o valores pueden tener un significado diferente de la organización que en las organizaciones de proveedor de reclamaciones de notificaciones. Puedes usar esta regla para normalizar las los tipos de notificación y los valores que provienen de proveedor de notificaciones para que sus equivalentes de Reclamación saliente se pueden entender por el usuario de confianza.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configuración de esta regla en una confianza de terceros de confianza  
Cuando usas una confianza de terceros de confianza, esta regla puede configurarse para transformar reclamaciones por el usuario de confianza específicas. Los tipos de notificación o notificación valores podrían tener un significado diferente para un usuario de confianza específicas, y esta regla hace posible para cambiar los tipos de notificaciones salientes y los valores para un único usuario de confianza.  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Crear esta regla con el lenguaje de regla de reclamación o mediante la **transformar una notificación entrante** plantilla de la regla en la administración de AD FS en snap\. Esta plantilla de regla proporciona las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación  
  
-   Transformar un tipo de notificación entrante específico para un tipo de notificación especificada saliente  
  
-   Pasar por todos los valores de las notificaciones  
  
-   Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente  
  
-   Reemplazar reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo  
  
Para obtener más instrucciones sobre cómo crear esta plantilla, consulta [crear una regla para transformar una notificación entrante](https://technet.microsoft.com/library/dd807068.aspx) en la Guía de implementación de AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Con el lenguaje de regla de notificación  
Si la solicitud saliente debe crearse del contenido de más de una notificación entrante, debes usar una regla personalizada en su lugar. Si el valor de la notificación de la solicitud saliente debe basarse en el valor de la notificación entrante, pero con contenido adicional, también debes usar una regla personalizada en ese contexto. Para obtener más información, consulta [cuándo usar una regla de notificación personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Ejemplos de cómo construir una sintaxis de regla de transformación  
Al usar la sintaxis de lenguaje de regla de notificación para transformar reclamaciones, puedes establecer una propiedad de la reclamación transformada en un nuevo valor literal. Por ejemplo, la siguiente regla cambia el valor de una reclamación de rol de los "administradores" a "raíz" mientras se mantiene el mismo tipo de notificación:  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
Expresiones regulares también pueden usarse para las transformaciones de notificación. Por ejemplo, la siguiente regla establecerá el dominio en windows notificaciones de nombre de usuario en formato dominio\\usuario a FABRIKAM:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedimientos recomendados para crear reglas personalizadas  
Transformaciones de notificaciones pueden aplicarse selectivamente a las reclamaciones seleccionados mediante las funciones básicas de filtrado. Cada una de las propiedades de Reclamación usadas para el filtrado se puede asignar valores, con lo siguiente:  
  
|Reclama la propiedad|Descripción|  
|------------------|---------------|  
|Tipo de valor, ValueType|Estas propiedades se usará con más frecuencia para las asignaciones. Deben especificar en el tipo y el valor mínimo para la reclamación transformada resultante.|  
|Emisor|Aunque el idioma de la regla de Reclamación permite establecer al emisor de una reclamación, esto no suele ser recomendable. El emisor de la solicitud no se serializa en el token. Cuando se recibe un token se establece la propiedad emisor de todas las reclamaciones en el identificador del servidor de federación que firmar el token. Por lo tanto, establecer al emisor de una reclamación en las reglas no tendrán efecto en el contenido del token y la configuración se perderán cuando se empaqueta la reclamación de un token. El único escenario que establecer al emisor de la solicitud tiene sentido es si se establece en un valor específico en el conjunto de reglas de reclamaciones proveedor y la confianza regla de terceros conjunto se creó con las reglas que hacen referencia a este valor específico. Si la propiedad emisor no se configura explícitamente en un valor en una regla de notificación, que el motor de emisión reclamaciones establece en "Autoridad LOCAL".|  
|OriginalIssuer|De forma similar al emisor, OriginalIssuer por lo general, no debería explícitamente asignarse un valor. A diferencia del emisor, la propiedad OriginalIssuer se serializa en el token, pero la expectativa de los consumidores tokens es que, si establece, lo que contenga el identificador del servidor de federación que emitió una reclamación.|  
|Propiedades|Como se describe en la sección anterior, el contenedor de propiedades de la solicitud no se conserva en el token, por lo que las asignaciones de propiedades solo deben hacerse si posteriores directivas locales se van a hacer referencia a la información almacenada en la propiedad.|  
  
Para obtener más información sobre cómo usar el lenguaje de regla de notificación, consulta [el rol del lenguaje de regla reclamar](The-Role-of-the-Claim-Rule-Language.md).  
  

