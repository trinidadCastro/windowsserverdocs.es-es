---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: Cuándo usar una regla de notificaciones de transformación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5ed8ee500582e0e687a2b52e83d99fc3cb8f147f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188344"
---
# <a name="when-to-use-a-transform-claim-rule"></a>Cuándo usar una regla de notificaciones de transformación
Puede usar esta regla en Active Directory Federation Services \(AD FS\) cuando necesite asignar un tipo de notificación entrante a un tipo de notificación saliente y, a continuación, aplicar una acción que determine qué resultado se debe producir en función de los valores que se origina en la notificación entrante. Cuando usas esta regla, pasas a través o transformas notificaciones que coinciden con la lógica de la regla siguiente, según cualquiera de las opciones que configures en la regla, como se describe en la tabla siguiente.  
  
|Opción de regla|Lógica de regla|  
|---------------|--------------|  
|Pasar a través todas las notificaciones entrantes|Si el tipo de notificación entrante es igual a *tipo de notificación especificado* y el valor es igual a *cualquier valor*, pasa a través la notificación con el tipo de notificación saliente igual a *tipo de notificación especificado*|  
|Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente|Si el tipo de notificación entrante es igual a *tipo de notificación especificado* y el valor es igual a *valor de notificación especificado*, transforma la notificación con el nuevo valor de notificación saliente *valor de notificación especificado* y con el tipo de notificación saliente *tipo de notificación especificado*|  
|Reemplazando entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico|Si el tipo de notificación entrante es igual a *tipo de notificación especificado* y el valor es igual a *cualquier valor de sufijo*, transforma la notificación con el nuevo valor de notificación saliente *valor de sufijo especificado* y con el tipo de notificación saliente *tipo de notificación especificado*|  
  
Las secciones siguientes ofrecen una introducción básica a las reglas de notificación y proporcionan más detalles sobre cuándo utilizar esta regla.  
  
## <a name="about-claim-rules"></a>Sobre las reglas de notificación  
Una regla de notificación representa una instancia de la lógica de negocios que se tomará una notificación entrante, se aplicará una condición \(if x, entonces y\) y generará una notificación saliente basándose en los parámetros de condición. La lista siguiente destaca las sugerencias importantes que deberías conocer sobre las reglas de notificaciones antes de seguir leyendo este tema:  
  
-   En el complemento Administración de AD FS\-, notificación solo se pueden crear reglas mediante las plantillas de regla de notificación  
  
-   El proceso de reglas de notificación entrante de notificaciones directamente desde un proveedor de notificaciones \(como Active Directory u otro servicio de federación\) o desde la salida de la aceptación de las reglas en una confianza de proveedor de notificaciones de transformación.  
  
-   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una precedencia en las reglas, puedes perfeccionar o filtrar aún más las notificaciones que generan las reglas anteriores dentro de un conjunto determinado de reglas.  
  
-   Siempre tendrás que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre las reglas de notificación y conjuntos de reglas de notificación, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Para obtener más información cómo se procesan los conjuntos de reglas de notificación, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pasar a través todos los valores de notificaciones  
Cuando se usa esta acción, todos los valores de notificación entrantes que están organizados en un tipo de notificación entrante especificado se asignan a un tipo de notificación saliente especificado antes de enviarlos como notificaciones salientes a los tokens firmados por el Servicio de federación.  
  
Por ejemplo, cuando se establece una regla con la lógica de opción **Pasar a través todos los valores de notificación** y se especifica el tipo de notificación entrante de Grupo y el tipo de notificación saliente de Rol, se copian individualmente todos los valores de notificación entrante que entran desde el emisor en las nuevas notificaciones salientes con el tipo de notificación de Rol.  
  
## <a name="transforming-a-claim"></a>Transformar una notificación  
En AD FS, el término *transformación de notificaciones* significa reemplazar una entrante de notificación de valor con un valor de notificación saliente diferente. La regla de Transformar una notificación entrante es la que hace posible esta función. En las propiedades de esta regla, puedes establecer condiciones para transformar los valores entrantes con un valor de notificación saliente diferente según el tipo de notificación entrante especificado.  
  
Por ejemplo, como se muestra en la ilustración siguiente, cuando se configura una regla con la condición de reemplazar un valor entrante por un valor de notificación saliente diferente, todos los tipos de notificaciones entrantes de Grupo se asignan a nuevos tipos de notificaciones salientes de Rol. En este caso, el valor de notificación entrante de Comprador se reemplaza por el nuevo valor de notificación saliente de Administrador.  
  
![Cuándo usar una transformación](media/adfs2_transform.gif)  
  
También puede usar esta regla para aplicar una condición que reemplazará todas las notificaciones entrantes con un especificado e\-valor con un nuevo valor del sufijo de correo. Por ejemplo, podrías establecer una condición en esta regla para cambiar todos los valores de notificación por el sufijo de sales.corp.fabrikam.com to fabrikam.com.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurar esta regla en una confianza de proveedor de notificaciones  
Cuando usas una confianza de proveedores de notificaciones, se puede configurar esta regla para transformar las notificaciones entrantes del proveedor de notificaciones en equivalentes de confianza. Los tipos de notificación o los valores de notificación pueden tener un significado diferente en tu organización y en las organizaciones del proveedor de notificaciones. Puedes usar esta regla para normalizar los tipos y valores de notificación que proceden del proveedor de notificaciones para que el usuario de confianza pueda entender sus notificaciones salientes equivalentes.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurar esta regla en una relación de confianza para usuario autenticado  
Cuando usas una relación de confianza para usuario autenticado, se puede configurar esta regla para transformar las notificaciones para el usuario de confianza específico. Los tipos de notificación o los valores de notificación podrían tener un significado diferente para un usuario de confianza específico y esta regla te permite cambiar los tipos y valores de notificación saliente para un único usuario de confianza.  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Crear esta regla mediante el lenguaje de reglas de notificación o usando el **transformar una notificación entrante** plantilla de regla en el complemento Administración de AD FS\-en. Esta plantilla de regla permite las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación.  
  
-   Transformar un tipo de notificación entrante especificado para un tipo de notificación saliente especificado  
  
-   Pasar a través todos los valores de notificaciones  
  
-   Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente  
  
-   Reemplace entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico  
  
Para obtener más instrucciones sobre cómo crear esta plantilla, consulte [crear una regla para transformar una notificación entrante](https://technet.microsoft.com/library/dd807068.aspx) en la Guía de implementación de AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación  
Si la notificación saliente se tiene que crear a partir del contenido de más de una notificación entrante, tienes que usar una regla personalizada en su lugar. Si el valor de notificación de la notificación saliente tiene que estar basado en el valor de la notificación entrante, pero con contenido adicional, también tienes que usar una regla personalizada en ese contexto. Para obtener más información, consulte [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Ejemplos de cómo construir una sintaxis de regla de transformación  
Cuando se utiliza la sintaxis del lenguaje de reglas de notificación para transformar las notificaciones, puedes establecer una propiedad de la notificación transformada en un nuevo valor literal. Por ejemplo, la regla siguiente cambia el valor de las notificaciones de rol de «Administradores» a «raíz» manteniendo el mismo tipo de notificación:  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
También se pueden usar expresiones comunes para las transformaciones de notificaciones. Por ejemplo, la regla siguiente establecerá el dominio en las notificaciones de nombre de usuario de windows en el dominio\\formato del usuario a FABRIKAM:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedimientos recomendados para crear reglas personalizadas  
Las transformaciones de notificaciones se pueden aplicar selectivamente a solicitudes seleccionadas con las capacidades de filtrado básicas. Todas las propiedades de notificación usadas para filtrar pueden ser valores asignados, con las siguientes advertencias:  
  
|Propiedad de notificación|Descripción|  
|------------------|---------------|  
|Tipo, valor, tipo de valor|Estas propiedades se usarán con más frecuencia para las asignaciones. Se deben especificar en el tipo y valor mínimos para la notificación transformada resultante.|  
|Emisor|Aunque el lenguaje de reglas de notificación permite establecer el emisor de una notificación, generalmente no se recomienda hacerlo. No se serializa el emisor de una notificación en el token. Cuando se recibe un token,se establece la propiedad emisora de todas las notificaciones en el identificador del servidor de federación que firmó el token. Por lo tanto, el establecer el emisor de una notificación en las reglas no tendrá efecto en el contenido del token y se perderá la configuración una vez que la notificación se empaquete en un token. El único escenario en el que tiene sentido establecer el emisor de la notificación es si se establece en un valor específico en el conjunto de reglas del proveedor de notificaciones y el conjunto de reglas del usuario de confianza creó las reglas que hacen referencia a este valor específico. Si la propiedad emisora no se establece explícitamente en un valor de una regla de notificación, el motor de emisión de notificaciones la establece en "AUTORIDAD LOCAL".|  
|Emisor original|De forma similar al emisor, al emisor original generalmente no se le debería asignar explícitamente un valor. A diferencia del emisor, la propiedad del emisor original se serializa en el token, pero la expectativa de consumidores de tokens es que, si se establece, contendrá el identificador del servidor de federación que emitió una notificación originalmente.|  
|Propiedades|Tal y como se describe en la sección anterior, el contenedor de propiedades de una solicitud no se conserva en el token, por lo que solo se deben realizar asignaciones de propiedades si las posteriores directivas locales van a hacer referencia a la información almacenada en la propiedad.|  
  
Para obtener más información sobre cómo usar el lenguaje de reglas de notificación, consulte [The Role of el lenguaje de reglas de notificación](The-Role-of-the-Claim-Rule-Language.md).  
  
