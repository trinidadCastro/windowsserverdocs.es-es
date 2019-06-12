---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Lenguaje de reglas de transformación de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a1f5c724d041a9f64c3b2697a8b5acd17a2a7bd9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445809"
---
# <a name="claims-transformation-rules-language"></a>Lenguaje de reglas de transformación de notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita la característica transformación le puente de notificaciones de Control de acceso dinámico a través de los límites del bosque mediante el establecimiento de directivas de transformación de notificaciones en las confianzas entre bosques de notificaciones entre los bosques. El componente principal de todas las directivas es reglas que están escritas en lenguaje de reglas de transformación de notificaciones. En este tema se proporciona detalles sobre este lenguaje y se proporciona instrucciones sobre la creación de reglas de transformación de notificaciones.  
  
Los cmdlets de Windows PowerShell para directivas de transformación en entre bosques confía tiene opciones para establecer directivas sencillas que son necesarios en común los escenarios. Estos cmdlets traducir la entrada del usuario en las directivas y reglas en el lenguaje de reglas de transformación de notificaciones y, a continuación, almacenarlas en Active Directory en el formato prescrito. Para obtener más información sobre los cmdlets para la transformación de notificaciones, consulte el [Cmdlets de AD DS para el Control de acceso dinámico](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
Según la configuración de notificaciones y los requisitos que se colocan en la confianza entre bosques en los bosques de Active Directory, pueden que sea más compleja que las directivas compatibles con los cmdlets de Windows PowerShell para Active las directivas de transformación de notificaciones Directorio. Para crear eficazmente dichas directivas, es esencial tener en cuenta la sintaxis del lenguaje de reglas de transformación de notificaciones y la semántica. Esto de notificaciones de lenguaje de reglas de transformación ("lenguaje") en Active Directory es un subconjunto del lenguaje que se usa por [Active Directory Federation Services](https://go.microsoft.com/fwlink/?LinkId=243982) para fines y similares tiene una semántica y una sintaxis muy similar. Sin embargo, hay menos operaciones permitidas y las restricciones de sintaxis adicionales se colocan en la versión de Active Directory del lenguaje.  
  
En este tema se explica brevemente la sintaxis y semántica de lenguaje de reglas de transformación de notificaciones en Active Directory y consideraciones para realizarse al crear directivas. Proporciona varios conjuntos de reglas de ejemplo para ayudarle a comenzar y ejemplos de sintaxis incorrecta y los mensajes que generan, para ayudar a descifrar los mensajes de error al crear las reglas.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Herramientas de creación de directivas de transformación de notificaciones  
**Cmdlets de Windows PowerShell para Active Directory**: Se trata de la manera preferida y recomendada para crear y establecer directivas de transformación de notificaciones. Estos cmdlets proporciona modificadores para directivas sencillas y comprobar las reglas que están establecidas para las directivas más complejas.  
  
**LDAP**: Directivas de transformación de notificaciones se pueden editar en Active Directory del protocolo de acceso a directorios (LDAP) a través de ligero. Sin embargo, esto no se recomienda porque las directivas tienen varios componentes complejos, y las herramientas que usa no pueden validar la directiva antes de escribir en Active Directory. Posteriormente, esto puede requerir una cantidad considerable de tiempo para diagnosticar problemas.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Lenguaje de reglas de transformación de notificaciones de Active Directory  
  
### <a name="syntax-overview"></a>Información general de sintaxis  
Esta es una breve descripción de la sintaxis y semántica del lenguaje:  
  
-   El conjunto de reglas de transformación de notificaciones consta de cero o más reglas. Cada regla tiene dos partes de activas: **Seleccione la lista de condiciones** y **Rule Action**. Si el **seleccione lista de condiciones** se evalúa como TRUE, se ejecuta la acción de regla correspondiente.  
  
-   **Seleccione la lista de condiciones** tiene cero o más **seleccionar condiciones**. Todos los **seleccionar condiciones** deben evaluarse como TRUE para el **seleccione lista de condiciones** se evalúe como TRUE.  
  
-   Cada **Seleccionar condición** tiene un conjunto de cero o más **condiciones de coincidencia de**. Todos los **condiciones de coincidencia de** deben evaluarse como TRUE para seleccionar condición se evalúe como TRUE. Todas estas condiciones se evalúan con una única notificación. Una notificación que coincide con un **Seleccionar condición** se pueden etiquetar por un **identificador** y hace referencia en el **la acción de regla**.  
  
-   Cada **condición de coincidencia** especifica la condición para que coincida con el **tipo** o **valor** o **ValueType** de una notificación mediante el uso de diferentes  **Operadores de condición** y **literales de cadena**.  
  
    -   Cuando se especifica un **condición de coincidencia** para un **valor**, también debe especificar un **condición de coincidencia** para un determinado **ValueType** y a la inversa. Estas condiciones deben estar junto a la otra en la sintaxis.  
  
    -   **ValueType** condiciones de coincidencia deben utilizar específico **ValueType** solo literales.  
  
-   Un **la acción de regla** puede copiar una notificación que se etiqueta con un **identificador** o emitir una notificación según una notificación que se etiqueta con un identificador o dado de literales de cadena.  
  
**Regla de ejemplo**  
  
En este ejemplo se muestra una regla que se puede usar para traducir las notificaciones de tipo entre dos bosques, siempre que use las mismas notificaciones, tipos de valor y tener el mismas interpretaciones para notificaciones de los valores para este tipo. La regla tiene una condición de coincidencia y una instrucción de problema que usa literales de cadena y una referencia de notificaciones coincidentes.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operación en tiempo de ejecución  
Es importante comprender el funcionamiento de en tiempo de ejecución de las transformaciones de notificaciones para crear las reglas de forma eficaz. La operación en tiempo de ejecución utiliza tres conjuntos de notificaciones:  
  
1.  **Conjunto de notificaciones de entrada**: El conjunto de notificaciones que se entregan a la operación de transformación de notificaciones de entrada.  
  
2.  **Conjunto de notificaciones de trabajo**: Notificaciones intermedias que se leen y escritas durante la transformación de notificaciones.  
  
3.  **Conjunto de notificaciones de salida**: Resultado de la operación de transformación de notificaciones.  
  
Aquí es una breve descripción de la operación de transformación de notificaciones en tiempo de ejecución:  
  
1.  Notificaciones de entrada de transformación de notificaciones se utilizan para inicializar el conjunto de notificaciones de trabajo.  
  
    1.  Cuando se procesa cada regla, el conjunto de notificaciones de trabajo se usa para las notificaciones de entrada.  
  
    2.  La lista de condiciones de selección en una regla se compara con todos los posibles conjuntos de notificaciones desde el conjunto de notificaciones de trabajo.  
  
    3.  Cada conjunto de notificaciones coincidentes se usa para ejecutar la acción en esa regla.  
  
    4.  Ejecuta una regla de los resultados de acción en una notificación, que se anexa a la salida de notificaciones de conjunto y el conjunto de notificaciones de trabajo. Por lo tanto, el resultado de una regla se usa como entrada para las reglas que siguen en el conjunto de reglas.  
  
2.  Las reglas del conjunto de reglas se procesan en orden secuencial comenzando por la primera regla.  
  
3.  Cuando se procesa el conjunto de reglas completo, se procesa el conjunto de notificaciones de salida para quitar notificaciones duplicadas y para otros problemas de seguridad. Las notificaciones resultantes son el resultado del proceso de transformación de notificaciones.  
  
Es posible escribir transformaciones de notificaciones complejas en función del comportamiento en tiempo de ejecución anterior.  
  
**Ejemplo: Operación en tiempo de ejecución**  
  
En este ejemplo se muestra la operación en tiempo de ejecución de una transformación de notificaciones que utiliza dos reglas.  
  
```  
  
     C1:[Type=="EmpType", Value=="FullTime",ValueType=="string"] =>  
                Issue(Type=="EmployeeType", Value=="FullTime",ValueType=="string");  
     [Type=="EmployeeType"] =>   
               Issue(Type=="AccessType", Value=="Privileged", ValueType=="string");  
Input claims and Initial Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
After Processing Rule 1:  
 Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"), (Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  
After Processing Rule 2:  
Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
Final Output:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
```  
  
### <a name="special-rules-semantics"></a>Semántica de reglas especiales  
Los siguientes son una sintaxis especial para las reglas:  
  
1.  Vaciar el conjunto de reglas no == ninguna notificación de salida  
  
2.  Seleccione lista de condiciones de vaciar == cada notificación coincide con la lista Select de condición  
  
    **Ejemplo: Seleccione la condición vacía lista**  
  
    La siguiente regla coincide con cada demanda en el espacio de trabajo.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Vaciar la lista Select de coincidencia == cada coincidencias de notificación de la lista Select de condición  
  
    **Ejemplo: Condiciones de coincidencia vacía**  
  
    La siguiente regla coincide con cada demanda en el espacio de trabajo. Se trata de la regla básica "Allow-all" si se usa por sí solo.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Consideraciones de seguridad  
**Notificaciones que entran en un bosque**  
  
Las notificaciones presentadas por entidades de seguridad que entran en un bosque deben ser inspeccionado detenidamente para asegurarse de que se permiten o emiten solo las notificaciones correctas. Notificaciones incorrectas pueden poner en peligro la seguridad de bosque, y debe ser una consideración fundamental cuando se crean directivas de transformación de notificaciones que entran en un bosque.  
  
Active Directory tiene las siguientes características para evitar que una configuración incorrecta de las solicitudes que entran en un bosque:  
  
-   Si una confianza de bosque no tiene ninguna directiva de transformación de notificaciones establecida para las notificaciones que entran en un bosque, por motivos de seguridad, Active Directory quita todas las notificaciones de entidad de seguridad que entran en el bosque.  
  
-   Si está ejecutando el conjunto de notificaciones de reglas que entra en los resultados de un bosque en las notificaciones que no estén definidas en el bosque, las notificaciones no definidas se quitan de las notificaciones de salida.  
  
**Notificaciones que dejan un bosque**  
  
Notificaciones que dejan un bosque que presente un problema de seguridad menor para el bosque que las notificaciones que entran en el bosque. Las notificaciones pueden dejar el bosque como-es incluso cuando no hay ninguna notificación correspondiente directiva de transformación en su lugar. También es posible emitir notificaciones que no estén definidas en el bosque como parte de la transformación de notificaciones que salen del bosque. Esto es para configurar fácilmente las confianzas entre bosques con notificaciones. Un administrador puede determinar si necesitan notificaciones que entran en el bosque se transforman y configurar la directiva adecuada. Por ejemplo, un administrador puede definir una directiva si es necesario para ocultar una notificación para evitar la divulgación de información.  
  
**Errores de sintaxis en las reglas de transformación de notificaciones**  
  
Si una directiva de transformación de notificaciones determinado tiene un conjunto de reglas que no es sintácticamente correcto o si hay otros problemas de sintaxis o almacenamiento, la directiva se considera no válida. Esto se trata de manera diferente a las condiciones de forma predeterminada que se ha mencionado anteriormente.  
  
Active Directory no puede determinar la intención en este caso y entra en un modo a prueba de errores, donde ninguna notificación de salida se genera en esa confianza + dirección de exploración transversal. Se requiere la intervención del administrador para corregir el problema. Esto podría suceder si se usa LDAP para editar la directiva de transformación de notificaciones. Cmdlets de Windows PowerShell para Active Directory tienen validación para evitar la escritura de una directiva con problemas de sintaxis.  
  
## <a name="other-language-considerations"></a>Otras consideraciones de idioma  
  
1.  Hay varias palabras clave o caracteres especiales en este idioma (denominada terminales). Estos se presentan en el [terminales de lenguaje](Claims-Transformation-Rules-Language.md#BKMK_LT) tabla más adelante en este tema. Los mensajes de error usar las etiquetas para estos terminales de anulación de ambigüedades.  
  
2.  Los terminales a veces pueden usarse como literales de cadena. Sin embargo, dicho uso puede entrar en conflicto con la definición del lenguaje o tener consecuencias no deseadas. No se recomienda este tipo de uso.  
  
3.  La acción de regla no puede realizar las conversiones de tipos en valores de notificación y un conjunto de reglas que contiene dicha acción de regla se considera no válido. Esto podría producir un error de tiempo de ejecución, y no se produce ninguna notificación de salida.  
  
4.  Si una acción de regla hace referencia a un identificador que no se ha usado en la parte Select List de la condición de la regla, es un uso no válido. Esto produciría un error de sintaxis.  
  
    **Ejemplo: Referencia de identificador incorrecta**  
    La regla siguiente muestra un identificador incorrecto usado en la acción de regla.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Reglas de transformación de ejemplo  
  
-   **Permitir todas las notificaciones de un tipo determinado**  
  
    Tipo exacto  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Uso de Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **No permitir un determinado tipo de notificación**  
    Tipo exacto  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Uso de Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Ejemplos de errores del analizador de reglas  
Se analizan las reglas de transformación de notificaciones con un analizador personalizado para comprobar si hay errores de sintaxis. Este analizador se ejecuta mediante cmdlets de Windows PowerShell relacionados antes de almacenar las reglas en Active Directory. Los errores de análisis de las reglas, incluidos los errores de sintaxis, se imprimen en la consola. Los controladores de dominio también ejecutan el analizador antes de usar las reglas para transformar notificaciones, y registrarán errores en el registro de eventos (agregar números de registro de eventos).  
  
En esta sección se muestra algunos ejemplos de reglas que se escriben con sintaxis incorrecta y la sintaxis correspondiente errores generados por el analizador.  
  
1. Por ejemplo:  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   En este ejemplo tiene un punto y coma utilizado incorrectamente en lugar de dos puntos.   
   **Mensaje de error:**  
   *POLICY0002: No se pudo analizar los datos de la directiva.*  
   *Número de línea: 1, número de columna: Token de 2, error:;. Line: 'c1;[]=>Issue(claim=c1);'.*  
   *Error del analizador: ' POLICY0030: Error de sintaxis inesperado ';', se esperaba uno de los siguientes: ':'.'*  
  
2. Por ejemplo:  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   En este ejemplo, la etiqueta de identificador en la instrucción de emisión de la copia es indefinida.   
   **Mensaje de error**:   
   *POLICY0011: Ninguna condición en la regla de notificación coincida con la etiqueta de la condición especificada en el CopyIssuanceStatement: "c2".*  
  
3. Por ejemplo:  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool" no es un Terminal en el lenguaje y no es un tipo de valor válido. Terminales válidos se muestran en el siguiente mensaje de error.   
   **Mensaje de error:**  
   *POLICY0002: No se pudo analizar los datos de la directiva.*  
   Número de línea: 1, número de columna: Token de 39, error: "bool". Line: 'c1:[type=="x1", value=="1",valuetype=="bool"]=>Issue(claim=c1);'.   
   *Error del analizador: ' POLICY0030: Error de sintaxis inesperado 'STRING', se esperaba uno de los siguientes: 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFIER'*  
  
4. Por ejemplo:  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   El número **1** en este ejemplo no es un token válido en el lenguaje, y dicho uso no se permite en una condición de coincidencia. Tiene que estar entre comillas dobles para que sea una cadena.   
   **Mensaje de error:**  
   *POLICY0002: No se pudo analizar los datos de la directiva.*  
   *Número de línea: 1, número de columna: 23 símbolo (token) de error: 1. Line: 'c1:[type=="x1", value==1, valuetype=="bool"]=>Issue(claim=c1);'.* <em>Parser error: ' POLICY0029: Entrada inesperada.</em>  
  
5. Por ejemplo:  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   En este ejemplo se utiliza un signo igual doble (==) en lugar de un solo signo igual (=).   
   **Mensaje de error:**  
   *POLICY0002: No se pudo analizar los datos de la directiva.*  
   *Número de línea: 1, número de columna: Error al 91, el token: ==. Line: 'c1:[type=="x1", value=="1",*  
   *valuetype=="boolean"]=>Issue(type=c1.type, value="0", valuetype=="boolean");'.*  
   *Error del analizador: ' POLICY0030: Error de sintaxis, inesperado '==', se esperaba uno de los siguientes: '='*  
  
6. Por ejemplo:  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   En este ejemplo es sintáctica y semánticamente correcto. Sin embargo, mediante "boolean" como valor de cadena está enlazado a provocar confusión, y debe evitarse. Como se mencionó anteriormente, mediante los terminales de idioma como valores de notificaciones deben evitarse siempre que sea posible.  
  
## <a name="BKMK_LT"></a>Terminales de lenguaje  
En la tabla siguiente enumera el conjunto completo de las cadenas de terminales y los terminales de idioma asociado a la que se usan en el lenguaje de reglas de transformación de notificaciones. Estas definiciones usen las cadenas de UTF-16 entre mayúsculas y minúsculas.  
  
|Cadena|Terminal|  
|----------|------------|  
|"=>"|INSINUAR|  
|";"|PUNTO Y COMA|  
|":"|COLON|  
|","|COMMA|  
|"."|PUNTO|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|ASIGNAR|  
|"&&"|Y|  
|"problema"|PROBLEMA|  
|"type"|TYPE|  
|"value"|VALOR|  
|"valuetype"|VALUE_TYPE|  
|"notificación"|NOTIFICACIÓN|  
|"[_A-Za-z][_A-Za-z0-9]*"|IDENTIFICADOR|  
|"\\"[^\\"\n]*\\""|CADENA|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"string"|STRING_TYPE|  
|"boolean"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintaxis del lenguaje  
Los siguientes idiomas de las reglas de transformación de notificaciones se especifican en el formulario ABNF. Esta definición usa los terminales de los que se especifican en la tabla anterior, además de las producciones ABNF definidos aquí. Las reglas deben estar codificadas en UTF-16, y las comparaciones de cadena deben ser tratadas como con diferenciación entre mayúsculas y minúsculas.  
  
```  
Rule_set        = ;/*Empty*/  
             / Rules  
Rules         = Rule  
             / Rule Rules  
Rule          = Rule_body  
Rule_body       = (Conditions IMPLY Rule_action SEMICOLON)  
Conditions       = ;/*Empty*/  
             / Sel_condition_list  
Sel_condition_list   = Sel_condition  
             / (Sel_condition_list AND Sel_condition)  
Sel_condition     = Sel_condition_body  
             / (IDENTIFIER COLON Sel_condition_body)  
Sel_condition_body   = O_SQ_BRACKET Opt_cond_list C_SQ_BRACKET  
Opt_cond_list     = /*Empty*/  
             / Cond_list  
Cond_list       = Cond  
             / (Cond_list COMMA Cond)  
Cond          = Value_cond  
             / Type_cond  
Type_cond       = TYPE Cond_oper Literal_expr  
Value_cond       = (Val_cond COMMA Val_type_cond)  
             /(Val_type_cond COMMA Val_cond)  
Val_cond        = VALUE Cond_oper Literal_expr  
Val_type_cond     = VALUE_TYPE Cond_oper Value_type_literal  
claim_prop       = TYPE  
             / VALUE  
Cond_oper       = EQ  
             / NEQ  
             / REGEXP_MATCH  
             / REGEXP_NOT_MATCH  
Literal_expr      = Literal  
             / Value_type_literal  
  
Expr          = Literal  
             / Value_type_expr  
             / (IDENTIFIER DOT claim_prop)  
Value_type_expr    = Value_type_literal  
             /(IDENTIFIER DOT VALUE_TYPE)  
Value_type_literal   = INT64_TYPE  
             / UINT64_TYPE  
             / STRING_TYPE  
             / BOOLEAN_TYPE  
Literal        = STRING  
Rule_action      = ISSUE O_BRACKET Issue_params C_BRACKET  
Issue_params      = claim_copy  
             / claim_new  
claim_copy       = CLAIM ASSIGN IDENTIFIER  
claim_new       = claim_prop_assign_list  
claim_prop_assign_list = (claim_value_assign COMMA claim_type_assign)  
             /(claim_type_assign COMMA claim_value_assign)  
claim_value_assign   = (claim_val_assign COMMA claim_val_type_assign)  
             /(claim_val_type_assign COMMA claim_val_assign)  
claim_val_assign    = VALUE ASSIGN Expr  
claim_val_type_assign = VALUE_TYPE ASSIGN Value_type_expr  
Claim_type_assign   = TYPE ASSIGN Expr  
  
```  
  


