---
description: 'Más información acerca de: Cuándo usar una regla de petición de paso a través o filtrar'
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: Cuándo usar una regla de Pasar a través o filtrar una notificación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f7b33c485e4bf640dbf2f158ee25f54ddc62dbb9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050453"
---
# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Cuándo usar una regla de Pasar a través o filtrar una notificación
Puede usar esta regla en Servicios de federación de Active Directory (AD FS) \( AD FS \) cuando necesite tomar un tipo de notificaciones entrante específico y, a continuación, aplicar una acción que determinará qué salida se debe producir en función de los valores de la solicitud entrante. Cuando usas esta regla, pasas a través o filtras cualquier notificación que coincide con la lógica de la regla de la tabla siguiente, según cualquiera de las opciones que configures en la regla.

|Opción de regla|Lógica de regla|
|---------------|--------------|
|Pasar a través todos los valores de notificaciones|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor es igual a *cualquier valor*, pasa a través la notificación|
|Pasar a través solo un valor de notificación determinado|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor es igual a *valor de notificación determinado*, pasa a través la notificación|
|Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico \-|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor es igual a *valor de sufijo determinado*, pasa a través la notificación|
|Pasar a través solo valores de notificación que empiecen con un valor determinado|Si el tipo de notificación entrante es igual a *tipo de notificación determinado* y el valor empieza por *valor de notificación determinado*, pasa a través la notificación|

Las secciones siguientes ofrecen una introducción básica a las reglas de notificación y proporcionan más detalles sobre cuándo utilizar esta regla.

## <a name="about-claim-rules"></a>Acerca de las reglas de notificaciones
Una regla de notificaciones representa una instancia de la lógica de negocios que tomará una solicitud entrante, le aplicará una condición \( si x después \) y y generará una notificaciones salientes según los parámetros de la condición. La lista siguiente destaca las sugerencias importantes que deberías conocer sobre las reglas de notificaciones antes de seguir leyendo este tema:

-   En el complemento de administración de AD FS \- , las reglas de notificaciones solo se pueden crear mediante plantillas de regla de notificaciones.

-   Las reglas de notificación procesan las notificaciones entrantes directamente desde un proveedor de notificaciones \( como Active Directory u otro servicio de Federación \) o desde la salida de las reglas de transformación de aceptación en una relación de confianza para proveedor de notificaciones.

-   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una precedencia en las reglas, puedes perfeccionar o filtrar aún más las notificaciones que generan las reglas anteriores dentro de un conjunto determinado de reglas.

-   Siempre tendrás que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación con una sola regla.

Para obtener información más detallada sobre las reglas de notificaciones y los conjuntos de reglas de notificaciones, consulte [el rol de las reglas de notificaciones](The-Role-of-Claim-Rules.md). Para obtener más información sobre cómo se procesan las reglas, vea [el rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información sobre cómo se procesan los conjuntos de reglas de notificaciones, vea [el rol de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md).

## <a name="pass-through-all-claim-values"></a>Pasar a través todos los valores de notificaciones
Cuando se usa esta acción, todos los valores de notificación entrante del tipo de notificación determinado se pasan a través como notificaciones salientes. Por ejemplo, cuando se especifica el tipo de notificación entrante como el tipo de notificación de rol, todos los valores de notificación entrante se copian individualmente en nuevas notificaciones salientes con el tipo de notificación saliente del rol.

## <a name="filtering-a-claim"></a>Filtrado de una notificación
En AD FS, el término *filtrado de notificaciones* significa filtrar o restringir los valores de notificación entrantes para que solo ciertos valores se pasen o envíen como notificaciones salientes. Lo que hace posible esta función es la plantilla de reglas **Pasar a través o filtrar una notificación entrante**. Dentro de las propiedades de esta regla, puedes establecer condiciones para filtrar los valores de entrada para que solo se pasen a través los valores que cumplen los criterios especificados.

Por ejemplo, puedes usar esta regla para solo pasar a través de notificaciones que coinciden con el valor de notificación del comprador cuando el tipo de notificación entrante coincide con tipo de notificación de rol o también puede que quieras emitir solo las notificaciones sobre el nombre del usuario, pero no las notificaciones que contienen el número de seguridad social del usuario.

Cuando usas una condición de filtro con esta regla, se examinan todas las notificaciones entrantes para determinar qué notificaciones coinciden con los criterios establecidos por la regla. Se omiten todas las demás notificaciones para que solo se pasen a través los valores de notificación determinados que coinciden con un tipo de notificación en concreto.

Por ejemplo, como se muestra en la siguiente ilustración, cuando se establece una regla con la condición de filtrar solo las notificaciones entrantes que están codificadas con el tipo de notificación de UPN y que también terminan con @fabrikam.com , todas las demás notificaciones entrantes se omiten a menos que cumplan este criterio. Esto incluye la notificaciones entrantes con el tipo de notificaciones de dirección de correo electrónico, \- aunque su valor de notificaciones termine en @fabrikam.com . En este caso, solo se envía la demanda que contiene el valor de Nick@fabrikam.com al usuario de confianza.

![Cuándo usar pass through](media/adfs2_filter.gif)

## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurar esta regla en una confianza de proveedor de notificaciones
Cuando usas una confianza de proveedores de notificaciones, esta regla se puede configurar para pasar a través solo notificaciones entrantes del proveedor de notificaciones que coincidan con determinadas restricciones. Por ejemplo, puede que quiera aceptar solo \- notificaciones de correo electrónico del proveedor de notificaciones; por lo tanto, usaría esta plantilla de reglas para aceptar \- tipos de notificación de correo electrónico que terminen en el \( nombre DNS del sistema de nombres de dominio del proveedor de notificaciones \) .

## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurar esta regla en una relación de confianza para usuario autenticado
Cuando usas una relación de confianza para usuario autenticado, esta regla se puede configurar para pasar a través o filtrar las notificaciones salientes que se enviarán al usuario de confianza. Es posible que algunos usuarios de confianza no entiendan ciertos tipos de notificación o que ciertas notificaciones contengan información confidencial que no se deba enviar a ciertos usuarios de confianza. Esta plantilla de reglas puede ayudar a aplicar esas directivas a una determinada entidad de confianza.

## <a name="how-to-create-this-rule"></a>Cómo crear esta regla
Esta regla se crea mediante el lenguaje de reglas de notificaciones o mediante la plantilla de reglas pasar a través o filtrar una notificaciones entrantes en el complemento Administración \- de AD FS en. Esta plantilla de regla permite las siguientes opciones de configuración:

-   Especificar un nombre de regla de notificaciones

-   Especificar un tipo de notificación entrante

-   Pasar a través todos los valores de notificaciones

-   Pasar a través solo un valor de notificación determinado

-   Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico \-

-   Pasar a través solo valores de notificación que empiecen con un valor determinado

Para obtener más instrucciones sobre cómo crear esta plantilla, consulte [creación de una regla para pasar a través o filtrar una demanda entrante](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807060(v=ws.11)) en la guía de implementación de AD FS.

## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación
Si solo se debe enviar una notificación cuando el valor de notificación coincida con un patrón personalizado, tienes que usar una regla personalizada. Para obtener más información, consulta «Cuándo usar una regla personalizada».

### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Ejemplos de cómo crear una sintaxis de regla para Pasar a través o filtrar
Una regla de filtrado sencilla podría filtrar notificaciones basadas en una de las propiedades descritas anteriormente. Por ejemplo, la siguiente regla pasará a través todas las \- notificaciones de correo electrónico:

```
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"]  => issue(claim  = c);
```

Los filtros pueden ser lógicamente y \- Ed juntos. Por ejemplo, la siguiente regla aceptará todas las \- notificaciones de correo electrónico con valor johndoe@fabrikam.com:

```
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", value == "johndoe@fabrikam.com "]  => issue(claim  = c);
```

En los ejemplos anteriores, los filtros siempre usan un operador de igualdad. El lenguaje de reglas de notificación admite los operadores siguientes:

-   \=\=\-igual a \( distingue mayúsculas de minúsculas \-\)

-   \!\=\-no es igual a \( distinguir mayúsculas de minúsculas \-\)

-   \=~\- coincidencia de expresión regular

-   \!~ \- expresión regular no \- coincidente

Por ejemplo, la siguiente regla aceptará todas las \- notificaciones de correo electrónico no emitidas por el servidor de Federación local que tienen un sufijo de Boeing.com:

```
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", value =~ "^.*@boeing\.com$" , issuer != "LOCAL AUTHORITY"]  => issue(claim  = c);
```

### <a name="best-practices-for-creating-custom-rules"></a>Procedimientos recomendados para crear reglas personalizadas
Se puede aplicar un filtro a una o varias de las propiedades de cada notificación, tal y como se describe en la tabla siguiente.


| Propiedad de notificación |                                                                                                                                                                                                                                                                                                                                                  Descripción                                                                                                                                                                                                                                                                                                                                                  |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      Tipo      |                                                                                                                                                                                        El tipo \( de demanda que se representa normalmente como un URI \) refleja un acuerdo implícito entre los asociados de una Federación sobre qué tipo de información se transmite en la demanda. Por ejemplo, las notificaciones de tipo http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ Identity \/ Claims \/ EmailAddress contendrán la \- dirección de correo electrónico del usuario.                                                                                                                                                                                         |
|     Valor      |                                                                                                                                                                                                                                                                   Valor de la reclamación. Por ejemplo, una notificación de tipo http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ Identity \/ Claims \/ EmailAddress puede tener un valor dejohndoe@fabrikam.com                                                                                                                                                                                                                                                                    |
|   ValueType    |                                                                                                                                                                                                  ValueType representa cómo se interpretará la información contenida en el valor de la demanda. Normalmente, el valor de ValueType se establecerá en http: \/ \/ www.w3.org \/ 2001 \/ XmlSchema \# String, pero el valor de la Claim podría contener datos codificados de Base64Binary, \( por ejemplo, una imagen \) o una fecha, booleano, etc.                                                                                                                                                                                                  |
|     Emisor     | El emisor representa la entidad que emitió por última vez las notificaciones sobre el usuario. Si las notificaciones se obtienen en un servidor de Federación del proveedor de notificaciones, el emisor de todas las notificaciones se establecerá en "autoridad LOCAL". Si un servidor de federación Proveedor de notificaciones recibió las notificaciones, se establecerá el emisor de las notificaciones para el identificador de proveedor de notificaciones del proveedor de notificaciones que firmó el token. Por lo tanto, al procesar las reglas de las notificaciones que recibieron por parte de un proveedor de notificaciones, el emisor de todas las notificaciones se establecerá en el mismo valor. Al crear reglas para un usuario de confianza, se puede usar la propiedad emisora para distinguir entre notificaciones procedentes de proveedores de notificaciones diferentes. |
| Emisor original |                                                                                                   Esta propiedad de notificación está destinada a transmitir qué servidor de federación emitió originalmente la notificación. Dado que la propiedad issuer de Claims se establece en el último servidor de Federación que firmó el token, el emisor original es útil en escenarios en los que una notificación se ha distribuido a través de más de un servidor \( de Federación por ejemplo, un usuario de confianza que recibe un token de un servidor de Federación de proveedor de Federación podría estar interesado en qué servidor de Federación del proveedor de notificaciones\)                                                                                                   |
|   Propiedades   |                                                                                                                             Además de las cinco propiedades descritas anteriormente, todas las notificaciones tienen también una bolsa de propiedades donde se pueden almacenar propiedades con nombre. Estas propiedades no se encuentran en serie en el token y solo tienen sentido para pasar información entre los componentes de la canalización de emisión de notificaciones dentro del ámbito de un servidor de federación único. Por ejemplo, establecer una propiedad durante el procesamiento de las reglas del proveedor de notificaciones y después hacer referencia a ella en las reglas del usuario de confianza.                                                                                                                              |
