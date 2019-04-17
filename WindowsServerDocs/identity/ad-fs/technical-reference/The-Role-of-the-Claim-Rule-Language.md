---
title: "La función del lenguaje de regla de notificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 206da33d33cbded0450db29615dc74eb1161cbce
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claim-rule-language"></a>La función del lenguaje de regla de notificación
Los servicios de federación de Active Directory (AD FS) reclamar el idioma de la regla actúa como el bloque de creación administrativo para el comportamiento de las notificaciones entrantes y salientes, mientras que el motor de reclamaciones actúa como el motor de procesamiento para la lógica en el idioma de la regla de notificación que define la regla personalizada. Para obtener más información acerca de cómo se procesan todas las reglas por el motor de notificaciones, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md).  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Creación de reglas de notificación personalizada con el lenguaje de regla de notificación  
AD FS proporciona a los administradores con la opción de definir reglas personalizadas que pueden usar para determinar el comportamiento de una reclamación de identidad con el idioma de la regla de notificación. Puedes usar los ejemplos de sintaxis de lenguaje de regla de reclamación en este tema para crear una regla personalizada que enumera, agrega, elimina y modifica las reclamaciones para satisfacer las necesidades de la organización. Puedes crear reglas personalizadas al escribir en la sintaxis de lenguaje de regla de notificación en el **enviar notificaciones usando una notificación personalizada** plantilla de regla.  
  
Las reglas se separan entre sí con punto y coma.  
  
Para obtener más información sobre cuándo usar reglas personalizadas, consulta [cuándo usar una regla personalizada de Reclamación](When-to-Use-a-Custom-Claim-Rule.md).  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Uso de plantillas de regla de notificación para obtener información sobre la sintaxis de lenguaje de regla de notificación  
AD FS también proporciona un conjunto de emisión de Reclamación predefinidos y plantillas de regla de aceptación que puedes usar para implementar comunes reclamar reglas de notificación. En la **editar reglas de notificación** cuadro de diálogo para una determinado confianza, puedes crear una regla predefinida: y ver la sintaxis de lenguaje de regla de notificación que conforma esta regla, haciendo clic en el **Ver regla idioma** ficha para esa regla. Usar la información de esta sección y la **Ver regla idioma** técnica puede proporcionar información sobre cómo crear sus propias reglas personalizadas.  
  
Para obtener más información sobre las reglas de notificación y plantillas de notificación de regla, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md).  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>Descripción de los componentes del lenguaje de regla de notificación  
El idioma de la regla de solicitud consta de los siguientes componentes, separados por la "= >" operador:  
  
-   Una condición  
  
-   Una instrucción de emisión  
  
### <a name="conditions"></a>Condiciones  
Puedes usar las condiciones en una regla para comprobar las solicitudes y determinar si se debe ejecutar la declaración de emisión de la regla. Una condición representa una expresión lógica que debe evaluarse en true para que se ejecute la parte del cuerpo de la regla. Si falta este elemento se da por hecho una verdadera lógica; es decir, siempre se ejecuta el cuerpo de la regla. La parte de las condiciones contiene una lista de condiciones que se combinan junto con el operador lógico junto ("& &"). Todas las condiciones en la lista deben evaluarse como true para el elemento totalmente condicional se evalúe en true. La condición puede ser un operador de selección de reclamaciones o una llamada de función de agregado. Estas dos son mutuamente excluyentes, lo que significa que los selectores de notificación y las funciones de agregado no se pueden combinar en una parte de las condiciones de regla única.  
  
Condiciones son opcionales en las reglas. Por ejemplo, la siguiente regla no tiene una condición:  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
Existen tres tipos de condiciones:  
  
-   Solo condición: esta es la forma más sencilla de una condición. Se realizan comprobaciones para una única expresión; Por ejemplo, el nombre de la cuenta de windows = usuario de dominio.  
  
-   Varios condición, esta condición requiere realizar comprobaciones adicionales para procesar varias expresiones en el cuerpo de la regla; Por ejemplo, el nombre de la cuenta de windows = usuario del dominio y grupo = contosopurchasers.  
  
> [!NOTE]  
> Existe otra condición, pero es un subconjunto de la condición simple o la condición de varios. Se conoce como una condición de la expresión regular (Regex). Sirve para hacer una expresión de entrada y coincide con la expresión con un modelo determinado. Puede usarse un ejemplo de cómo se muestra a continuación.  
  
Los siguientes ejemplos se muestran a algunas de las construcciones de sintaxis que se basan en los tipos de condiciones, que puedes usar para crear reglas personalizadas.  
  
#### <a name="single--condition-examples"></a>Único: condición ejemplos  
Único: condiciones de expresión se describen en la siguiente tabla. Se construyen para comprobar simplemente de una reclamación con un tipo de notificación especificada o de una reclamación con un tipo de notificación especificada y el valor de notificación.  
  
|Descripción de la condición|Ejemplo de sintaxis de la condición|  
|-------------------------|----------------------------|  
|Esta regla tiene una condición para comprobar si una solicitud de entrada con un tipo de notificación especificada ("http://test/name"). Si una reclamación coincidente está en las notificaciones de entrada, la regla copia la reclamación coincidente o reclamaciones al conjunto de notificaciones de salida.|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|Esta regla tiene una condición para comprobar una solicitud de entrada con un tipo de notificación especificada ("http://test/name") y el valor ("Daniel") de notificación. Si una reclamación coincidente está en las notificaciones de entrada, la regla copia la reclamación coincidente o reclamaciones al conjunto de notificaciones de salida.|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
Más - condiciones complejas se muestran en la siguiente sección, incluidas las condiciones para comprobar si varias notificaciones, las condiciones para comprobar al emisor de la solicitud y condiciones comprobar si los valores que coinciden con un patrón de expresión regular.  
  
#### <a name="multiple--condition-examples"></a>Ejemplos de condición múltiple:  
La siguiente tabla proporciona un ejemplo de - expresión condiciones.  
  
|Descripción de la condición|Ejemplo de sintaxis de la condición|  
|-------------------------|----------------------------|  
|Esta regla tiene una condición para que busque dos reclamaciones de entrada, cada uno con un tipo de notificación especificada ("http://test/name" y "http://test/email"). Si las dos notificaciones coincidentes están en las notificaciones de entrada, la regla de copia de la notificación de nombre para el conjunto de notificaciones de salida.|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>Normal - condición ejemplos  
La siguiente tabla proporciona un ejemplo de una expresión regular,-según la condición.  
  
|Descripción de la condición|Ejemplo de sintaxis de la condición|  
|-------------------------|----------------------------|  
|Esta regla tiene una condición que usa una expresión regular para buscar una e-Reclamación de correo que terminan en "@fabrikam.com". Si se encuentra una coincidencia reclamación en las notificaciones de entrada, la regla copia la notificación correspondiente al conjunto de notificaciones de salida.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>Instrucciones de emisión  
Reglas personalizadas se procesan en función de las declaraciones de emisión (*problema* o *agregar* ) que programa en la regla de notificación. Según el resultado deseado, la declaración de problema o agregar declaración puede escribirse en la regla para rellenar el conjunto de solicitud de entrada o la salida de conjunto de notificaciones. Una regla personalizada que usa la declaración de agregar explícitamente rellena reclamación valores solo a la entrada del conjunto de notificaciones mientras una regla de notificación personalizada que usa la declaración del problema rellena reclamación valores de entrada conjunto de notificaciones y conjunto de notificaciones en el resultado. Esto puede ser útil cuando un valor de la notificación está pensado para usarse solo en las futuras reglas en el conjunto de reglas de notificación.  
  
Por ejemplo, en la siguiente ilustración, la notificación entrante se agrega a la solicitud de entrada establecida por el motor de emisión de reclamaciones. Cuando se ejecuta la primera regla de notificación personalizada y se cumple los criterios de usuario de dominio, el motor de emisión de reclamaciones procesa la lógica de la regla mediante la declaración de agregar y el valor de **Editor** se agrega al conjunto de solicitud de entrada. Como el valor del Editor está presente en el conjunto de solicitud de entrada, regla 2 correctamente puede procesar la declaración del problema en su lógica y genera un nuevo valor de **Hello**, que se agrega a la salida de ambos conjunto de notificaciones y a la solicitud de entrada configurada para usarse en la siguiente regla de la regla establece. Regla 3 ahora puedes usar todos los valores que están presentes en la solicitud de entrada establecer como entrada para el procesamiento de su lógica.  
  
![Roles de AD FS](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>Acciones de emisión de notificación  
El cuerpo de la regla representa una acción de emisión de la reclamación. Hay dos acciones de emisión de reclamación que reconoce el idioma:  
  
-   **Emitir declaración:** la declaración del problema crea una reclamación que va a la entrada y salida reclama conjuntos. Por ejemplo, la siguiente instrucción envía una notificación nueva basada en su conjunto de solicitud de entrada:  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Agrega la declaración:** la instrucción de agregar crea una nueva notificación que sólo se agrega a la colección de conjunto de solicitud de entrada. Por ejemplo, la siguiente instrucción agrega una nueva notificación para el conjunto de solicitud de entrada:  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
La instrucción de emisión de una regla define cuál será reclamaciones emitido por la regla cuando se cumplen las condiciones. Existen dos formas de instrucciones de emisión en relación con los argumentos y el comportamiento de la instrucción:  
  
-   **Normal**: instrucciones de emisión Normal pueden emitir notificaciones mediante valores literales en la regla o los valores de notificaciones que coincidan con las condiciones. Una instrucción de emisión normal puede constar de uno o ambos de los siguientes formatos:  
  
    -   *Reclamar copia*: la copia de la reclamación crea una copia de la notificación existente en el conjunto de solicitud de salida. Este formulario de emisión solo tiene sentido cuando se combina con la declaración de emisión "problema". Cuando se combina con la declaración de emisión "Agregar", no tiene ningún efecto.  
  
    -   *Nueva notificación*: Esto formato crea una nueva notificación, dado los valores de diversas propiedades de notificación. Se debe especificar Claim.Type; todas las demás propiedades de notificación son opcionales. El orden de los argumentos de este formulario se omite.  
  
-   **Atributo tienda**, este formulario crea notificaciones con valores que se recuperan de un almacén de atributo. Es posible crear varios tipos de notificación mediante una instrucción de emisión único, que es importante para los almacenes de atributo para realizar operaciones de (E/S) de entrada y salida de red o el disco durante la recuperación de atributo. Por lo tanto, es conveniente para limitar el número de ida y vuelta entre el motor de directivas y la tienda de atributo. También puede hacerse para crear varias notificaciones para un tipo de notificación determinado. Cuando la tienda de atributo devuelve varios valores para un tipo de notificación determinado, la declaración de emisión crea automáticamente una reclamación para cada valor devuelto reclamación. Una implementación de la tienda de atributo utiliza los argumentos de param sustituir los marcadores de posición en el argumento de consulta con los valores que se proporcionan en los argumentos de parámetro. Los marcadores de posición usan la misma sintaxis como la función () de .NET String.Format (por ejemplo, {1}, {2} y así sucesivamente). Es importante el orden de los argumentos de esta forma de emisión, y debe ser el orden indicado en la siguiente gramática.  
  
La siguiente tabla describe algunos construcciones comunes de sintaxis para ambos tipos de instrucciones de emisión de reglas de notificación.  
  
|Tipo de instrucción de emisión|Descripción de la declaración de emisión|Ejemplo de sintaxis de declaración de emisión|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normal|La siguiente regla emite siempre la misma notificación siempre que un usuario tiene el tipo de notificación especificada y valor:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normal|La siguiente regla convierte un tipo de notificación en otro. Ten en cuenta que se usa el valor de la notificación que coincide con la condición "c" en la declaración de emisión.|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Tienda de atributo|La siguiente regla usa el valor de una notificación entrante para consultar el almacén de atributo de Active Directory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Tienda de atributo|La siguiente regla usa el valor de una notificación entrante para consultar en un almacén de atributo de lenguaje de consulta estructurado (SQL) haya configurado anteriormente:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>Expresiones  
Expresiones se usan en el lado derecho para restricciones de selector de notificaciones y parámetros de la instrucción de emisión. Hay varios tipos de expresiones que admite el idioma. Todas las expresiones en el lenguaje son en función de cadena, lo que significa que toman cadenas como entrada y generar cadenas. No se admiten los números u otros tipos de datos, como la fecha y hora en las expresiones. El siguiente es los tipos de expresiones que admite el idioma:  
  
-   Literal de cadena: valor, delimitado por el carácter de comillas (") en ambas caras de cadena.  
  
-   Cadena concatenación de expresiones: el resultado es una cadena que se genera con concatenación de los valores de la izquierda y derecho.  
  
-   Llamada de función: la función se identifica mediante un identificador y los parámetros se pasan como una coma-lista delimitada de expresiones entre corchetes ("()").  
  
-   Acceso a las propiedades de la notificación en forma de un nombre de propiedad de punto de nombre de variable: el resultado del valor de propiedad de la notificación identificado para una valoración de variable determinada. La variable primero debe estar enlazada a un selector de notificaciones antes de que se puede usar de esta forma. No es válido para usar la variable que está enlazada a un selector de reclamaciones dentro de las restricciones de ese mismo selector de reclamaciones.  
  
Las siguientes propiedades de notificación están disponibles para el acceso:  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[property\_name\] (esta propiedad devuelve una cadena vacía si no se encuentra el _name de propiedad de colección de propiedades de la notificación. )  
  
Puedes usar la función RegexReplace para llamar a dentro de una expresión. Esta función toma una expresión de entrada y se hace coincidir con el modelo especificado. Si coincide con el patrón, el resultado de la coincidencia se reemplaza con el valor de reemplazo.  
  
#### <a name="exists-functions"></a>Existe funciones  
La función existe puede usarse en una condición para evaluar si una notificación que coincide con que la condición existe en la entrada del conjunto de notificaciones. Si existe cualquier reclamación coincidente, la declaración de emisión se llama una sola vez. En el siguiente ejemplo, la notificación "origin" se emite una sola vez, si hay al menos una notificación en la colección de conjunto de solicitud de entrada que tiene el emisor establecido en "MSFT", independientemente de cuántas solicitudes tienen establece el emisor para "MSFT". Con esta función impide que las notificaciones duplicadas de que se emite.  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>Cuerpo de la regla  
El cuerpo de la regla puede contener solo una instrucción de emisión único. Si se usan las condiciones sin usar la función existe, el cuerpo de la regla se ejecuta una vez por cada vez que se hace coincidir la parte de las condiciones.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Crear una regla para enviar notificaciones usando una regla personalizada](https://technet.microsoft.com/library/dd807049.aspx)  
  

