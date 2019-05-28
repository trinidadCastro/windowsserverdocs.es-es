---
title: El papel del lenguaje de reglas de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: bf36f12803b8ba621f2249b53ad868fcd8f6c4a7
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188461"
---
# <a name="the-role-of-the-claim-rule-language"></a>El papel del lenguaje de reglas de notificaciones
Lenguaje de reglas actúa como bloque de creación administrativo del comportamiento de las notificaciones entrantes y salientes, mientras que el motor de notificaciones actúa como el motor de procesamiento de la lógica en el lenguaje de reglas de notificación de notificación de los servicios de federación de Active Directory (AD FS) que define la regla personalizada. Para obtener más información acerca de cómo se procesan todas las reglas el motor de notificaciones, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Creación de reglas de notificación personalizadas mediante el lenguaje de las reglas de notificaciones  
AD FS ofrece a los administradores la opción de definir reglas personalizadas que pueden usar para determinar el comportamiento de las notificaciones de identidad con el lenguaje de reglas de notificación. Puede usar los ejemplos de sintaxis de lenguaje de reglas de notificación de este tema para crear una regla personalizada que enumere, agregue, elimine y modifique notificaciones para satisfacer las necesidades de su organización. Puede crear reglas personalizadas escribiendo en la sintaxis del lenguaje de reglas de notificaciones en la plantilla de la regla **Enviar notificaciones mediante una notificación personalizada**.  
  
Las reglas se separan entre sí mediante punto y coma.  
  
Para obtener más información sobre cuándo usar reglas personalizadas, vea [cuándo se debe usar una regla de notificación personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Uso de plantillas de regla de notificaciones para obtener información acerca de la sintaxis del lenguaje de reglas de notificación  
AD FS también proporciona un conjunto de reglas de notificación de las plantillas de reglas de aceptación que se pueden utilizar para implementar comunes de notificación y emisión de notificaciones. En el cuadro de diálogo **Editar reglas de notificación** de una confianza determinada, puede crear una regla predefinida y ver la sintaxis del lenguaje de la regla de notificación que compone dicha regla haciendo clic en la pestaña **Ver lenguaje de reglas**. Con la información de esta sección y la técnica **Ver lenguaje de reglas** puede proporcionar información sobre cómo construir sus propias reglas personalizadas.  
  
Para obtener más información sobre las reglas de notificación y las plantillas de regla de notificación, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>Descripción de los componentes del lenguaje de reglas de notificaciones  
El lenguaje de reglas de notificación consta de los siguientes componentes, separados por el "= >" operador:  
  
-   Una condición  
  
-   Una declaración de emisión  
  
### <a name="conditions"></a>Condiciones  
Puede usar las condiciones de una regla para comprobar las notificaciones de entrada y determinar si se debe ejecutar la instrucción de emisión de la regla. Una condición representa una expresión lógica que debe evaluarse si es verdadera para ejecutar la parte del cuerpo de la regla. Si este elemento no se encuentra presente, se ejecutará siempre el cuerpo de la regla. La parte de las condiciones contiene una lista de condiciones que se combinan con el operador lógico de conjunción ("& &"). Debe evaluarse si todas las condiciones de la lista son verdaderas para evaluar toda la parte condicional como verdadera. La condición puede ser un operador de selección de notificaciones o una llamada de función agregada. Estas dos se excluyen mutuamente, lo que significa que los selectores de notificación y las funciones totales no se pueden combinar en una parte de condiciones de una sola regla.  
  
Las condiciones son opcionales en las reglas. Por ejemplo, la siguiente regla no tiene una condición:  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
Existen tres tipos de condiciones:  
  
-   Condición única: se trata de la forma más sencilla de una condición. Se realizan comprobaciones para una única expresión; Por ejemplo, el nombre de la cuenta de windows = usuario de dominio.  
  
-   Varias condiciones, esta condición requiere comprobaciones adicionales para procesar varias expresiones en el cuerpo de la regla; Por ejemplo, el nombre de la cuenta de windows = usuario de dominio y grupo = contosopurchasers.  
  
> [!NOTE]  
> Existe otra condición, pero es un subconjunto de la condición única o múltiple. Se conoce como una condición de expresiones regulares (Regex). Sirve para tomar una expresión de entrada y hallar la correspondencia de la expresión con un patrón determinado. A continuación se muestra un ejemplo de cómo puede usarse.  
  
En los ejemplos siguientes se muestran algunas de las construcciones de sintaxis, que se basan en los tipos de condiciones, que puede usar para crear reglas personalizadas.  
  
#### <a name="single--condition-examples"></a>Single - ejemplos de condición  
Single - se describen las condiciones de expresión en la tabla siguiente. Se construyen para comprobar simplemente si hay una notificación con un tipo de notificación especificado o una notificación con un tipo de notificación especificado y un valor de notificación.  
  
|Descripción de la condición|Ejemplo de sintaxis de condición|  
|-------------------------|----------------------------|  
|Esta regla tiene una condición para comprobar una notificación de entrada con un tipo de notificación especificado ("http://test/name"). Si una notificación coincidente se encuentra en las notificaciones de entrada, la regla copia la notificación o notificaciones correspondientes en el conjunto de notificaciones de salida.|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|Esta regla tiene una condición para comprobar una notificación de entrada con un tipo de notificación especificado ("http://test/name") y ("Terry") del valor de notificación. Si una notificación coincidente se encuentra en las notificaciones de entrada, la regla copia la notificación o notificaciones correspondientes en el conjunto de notificaciones de salida.|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
Más - condiciones complejas se muestran en la sección siguiente, incluidas las condiciones para buscar varias notificaciones, las condiciones para comprobar al emisor de una notificación y las condiciones comprobar si hay valores que coinciden con un patrón de expresión regular.  
  
#### <a name="multiple--condition-examples"></a>Ejemplos de condiciones múltiples:  
En la tabla siguiente proporciona un ejemplo de varios - condiciones de expresión.  
  
|Descripción de la condición|Ejemplo de sintaxis de condición|  
|-------------------------|----------------------------|  
|Esta regla tiene una condición para comprobar si dos de entrada notificaciones, cada uno con un tipo de notificación especificado ("http://test/name" y "http://test/email"). Si las dos notificaciones coincidentes se encuentran en las notificaciones de entrada, la regla copia la notificación de nombre en el conjunto de notificaciones de salida.|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>Normal: ejemplos de condición  
En la tabla siguiente proporciona un ejemplo de una expresión regular,-según la condición.  
  
|Descripción de la condición|Ejemplo de sintaxis de condición|  
|-------------------------|----------------------------|  
|Esta regla tiene una condición que utiliza una expresión regular para comprobar una e-notificación de correo electrónico que terminen en "@fabrikam.com". Si se encuentra una notificación coincidente en las notificaciones de entrada, la regla copia la notificación correspondiente en el conjunto de notificaciones de salida.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>Declaraciones de emisión  
Reglas personalizadas se procesan según las instrucciones de emisión (*problema* o *agregar* ) que programa en la regla de notificación. En función del resultado deseado, la instrucción de problema o de agregar se puede escribir en la regla para rellenar el conjunto de notificaciones de entrada o de salida. Una regla personalizada que usa la instrucción de agregar explícitamente rellena valores de notificación solamente en el conjunto de notificaciones de entrada mientras una regla de notificación personalizada que usa la instrucción de problema rellena los valores de la notificación en el conjunto de notificaciones de entrada y de salida. Esto puede ser útil cuando un valor de notificación está pensado para ser usado únicamente por parte de reglas futuras en el conjunto de reglas de notificación.  
  
Por ejemplo, en la siguiente ilustración, la notificación entrante se agrega a la notificación de entrada establecida por el motor de emisión de notificaciones. Cuando se ejecuta la primera regla de notificación personalizada y se cumple los criterios de usuario de dominio, el motor de emisión de notificaciones procesa la lógica de la regla mediante la instrucción de adición y el valor de **Editor** se agrega al conjunto de notificaciones de entrada. Dado que el valor del Editor está presente en el conjunto de notificaciones de entrada, la regla 2 puede procesar la instrucción del problema correctamente en su lógica y generar un nuevo valor de **Hello**, que se agrega al conjunto de notificaciones de salida y de entrada para ser usado por la regla siguiente del conjunto. Ahora la regla 3 puede usar todos los valores que se encuentran en el conjunto de notificaciones de entrada como entrada para procesar su lógica.  
  
![Roles de AD FS](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>Acciones de emisión de notificaciones  
El cuerpo de la regla representa una acción de emisión de notificaciones. Hay dos acciones de emisión de notificaciones que el lenguaje reconoce:  
  
-   **Instrucción de problema:** La instrucción de problema crea una notificación que se va a los conjuntos de notificaciones de entrada y salida. Por ejemplo, la siguiente instrucción envía una nueva notificación según su conjunto de notificaciones de entrada:  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Instrucción de adición:** La instrucción de adición crea una nueva notificación que solo se agrega a la colección de conjuntos de notificaciones de entrada. Por ejemplo, la siguiente instrucción agrega una nueva notificación al conjunto de notificaciones de entrada:  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
La instrucción de emisión de una regla define qué notificaciones emitirá la regla cuando se cumplan las condiciones. Hay dos formas de instrucciones de emisión relacionadas con los argumentos y el comportamiento de las instrucciones:  
  
-   **Normal**: las instrucciones de emisión normal pueden emitir notificaciones mediante valores literales en la regla o los valores de las notificaciones que cumplen las condiciones. Una instrucción de emisión normal puede constar de uno o ambos de los siguientes formatos:  
  
    -   *Copia de la notificación*: la copia de la notificación crea una copia de la notificación existente en el conjunto de notificaciones de salida. Este formulario de emisión solo tiene sentido cuando se combina con la instrucción de emisión "problema". Cuando se combina con la instrucción de emisión "add", no tiene ningún efecto.  
  
    -   *Nueva notificación*: Este formato crea una nueva notificación, dada los valores para las distintas propiedades de notificación. Se debe especificar Claim.Type; todas las demás propiedades de notificación son opcionales. El orden de los argumentos de este formulario se ignora.  
  
-   **Almacén de atributos**: este formulario crea notificaciones con valores que se recuperan de un almacén de atributos. Es posible crear varios tipos de notificación mediante una instrucción de emisión única, lo que es importante para los almacenes de atributos que realizan operaciones de (E/S) de entrada/salida de red o disco durante la recuperación de atributos. Por lo tanto, es conveniente limitar el número de viajes de ida y vuelta entre el motor de directivas y el almacén de atributos. También es legal crear varias notificaciones para un tipo de notificación determinado. Cuando el almacén de atributos devuelve varios valores para un tipo de reclamación determinado, la instrucción de emisión crea automáticamente una notificación para cada valor de notificación devuelto. Una implementación de almacén de atributos usa los argumentos de parámetro para sustituir los marcadores de posición del argumento de la consulta por los valores proporcionados en los argumentos de parámetro. Los marcadores de posición utilizan la misma sintaxis que la función de .NET String.Format () (por ejemplo, {1}, {2}, y así sucesivamente). El orden de los argumentos de esta forma de emisión es importante y debe ser el orden que se indica en la siguiente gramática.  
  
En la tabla siguiente se describen algunas construcciones sintácticas comunes para ambos tipos de instrucciones de emisión de reglas de notificación.  
  
|Tipo de instrucción de emisión|Descripción de la instrucción de emisión|Ejemplo de sintaxis de instrucción de emisión|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normal|La siguiente regla siempre emite la misma notificación cada vez que un usuario tiene el tipo de notificación y el valor especificado:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normal|La siguiente regla convierte un tipo de notificación en otro. Observe que el valor de la notificación que coincide con la condición "c" se usa en la instrucción de emisión.|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Almacén de atributos|La siguiente regla usa el valor de una notificación entrante para consultar el almacén de atributos de Active Directory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Almacén de atributos|La siguiente regla usa el valor de una notificación entrante para consultar un almacén de atributos del lenguaje de consulta estructurado (SQL) configurado previamente:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>Expresiones  
Las expresiones se usan en el lado derecho para las restricciones del selector de notificaciones y los parámetros de la instrucción de emisión. Hay varios tipos de expresiones que admite el lenguaje. Todas las expresiones del lenguaje están basadas en cadenas, lo cual significa que toman cadenas como entrada y generan cadenas. No se admiten números u otros tipos de datos, como la fecha y hora en las expresiones. A continuación se facilitan los tipos de expresiones que admite el lenguaje:  
  
-   Literal de cadena: Valor de cadena delimitado por el carácter de comillas dobles (") en ambos lados.  
  
-   Concatenación de cadenas de expresiones: el resultado es una cadena que se genera mediante la concatenación de los valores izquierdo y derecho.  
  
-   Llamada de función: La función se identifica mediante un identificador y los parámetros se pasan como una coma: lista delimitada de expresiones situadas entre corchetes ("()").  
  
-   Acceso a la propiedad de la notificación en forma de nombre de propiedad DOT de nombre de variable: El resultado del valor de la propiedad de la notificación identificada para una valoración de variable determinada. En primer lugar la variable debe estar enlazada a un selector de notificaciones para que se pueda usar de esta manera. Es ilegal usar la variable que está enlazada a un selector de notificaciones situado dentro de las restricciones de ese mismo selector de notificaciones.  
  
Las siguientes propiedades de notificación están disponibles para acceder a ellas:  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[propiedad\_nombre\] (esta propiedad devuelve una cadena vacía si no se encuentra el _nombre de propiedad de colección de propiedades de la notificación. ) simple  
  
Puede usar la función RegexReplace llamar a dentro de una expresión. Esta función toma una expresión de entrada y la hace coincidir con el modelo especificado. Si el patrón coincide, el resultado de la coincidencia se reemplaza por el valor de la sustitución.  
  
#### <a name="exists-functions"></a>Funciones Exists  
La función Exists puede usarse en una condición para evaluar si existe una notificación que coincida con la condición en el conjunto de notificaciones de entrada. Si existe cualquier notificación coincidente, se llama a la instrucción de emisión tan solo una vez. En el ejemplo siguiente, la notificación de "origen" se emite exactamente una vez (si hay al menos una notificación en la colección del conjunto de notificaciones de entrada con el emisor establecido en "MSFT", independientemente de cuántas de las notificaciones tengan el emisor establecido en "MSFT". El uso de esta función evita la emisión de notificaciones duplicadas.  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>Cuerpo de la regla  
El cuerpo de la regla puede contener una única instrucción de emisión. Si las condiciones se usan sin usar la función Exists, el cuerpo de la regla se ejecuta una vez por cada vez que se produce una coincidencia con la parte de las condiciones.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Crear una regla para enviar notificaciones mediante una regla personalizada](https://technet.microsoft.com/library/dd807049.aspx)  
  

