---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Lenguaje de reglas de transformación de notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f391c3f8ef2bb5b12f0dd15db55df4f861c05f9b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861278"
---
# <a name="claims-transformation-rules-language"></a>Lenguaje de reglas de transformación de notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La característica de transformación de notificaciones entre bosques le permite enlazar notificaciones de Access Control dinámicas a través de los límites del bosque mediante la configuración de directivas de transformación de notificaciones en confianzas entre bosques. El componente principal de todas las directivas son las reglas que se escriben en el lenguaje de reglas de transformación de notificaciones. En este tema se proporcionan detalles sobre este lenguaje y se proporcionan instrucciones sobre la creación de reglas de transformación de notificaciones.  
  
Los cmdlets de Windows PowerShell para las directivas de transformación en confianzas entre bosques tienen opciones para establecer directivas simples que se requieren en escenarios comunes. Estos cmdlets traducen la entrada del usuario en directivas y reglas en el lenguaje de reglas de transformación de notificaciones y, después, las almacenan en Active Directory en el formato indicado. Para obtener más información sobre los cmdlets para la transformación de notificaciones, consulte los [cmdlets de AD DS para la Access Control dinámica](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
En función de la configuración de notificaciones y de los requisitos de la confianza entre bosques en los bosques de Active Directory, las directivas de transformación de notificaciones pueden tener que ser más complejas que las directivas admitidas por los cmdlets de Windows PowerShell para Active Directory. Para crear de forma eficaz estas directivas, es esencial comprender la sintaxis y la semántica del lenguaje de reglas de transformación de notificaciones. Este lenguaje de reglas de transformación de notificaciones ("el lenguaje") en Active Directory es un subconjunto del lenguaje que usa [servicios de Federación de Active Directory (AD FS)](https://go.microsoft.com/fwlink/?LinkId=243982) para propósitos similares y tiene una sintaxis y una semántica muy similares. Sin embargo, se permiten menos operaciones y las restricciones de sintaxis adicionales se colocan en la versión Active Directory del lenguaje.  
  
En este tema se explica brevemente la sintaxis y la semántica del lenguaje de reglas de transformación de notificaciones en Active Directory y consideraciones que se deben realizar al crear directivas. Proporciona varios conjuntos de reglas de ejemplo para empezar a trabajar y ejemplos de sintaxis incorrecta y los mensajes que generan, para ayudarle a descifrar los mensajes de error al crear las reglas.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Herramientas para la creación de directivas de transformación de notificaciones  
**Cmdlets de Windows PowerShell para Active Directory**: esta es la manera preferida y recomendada para crear y establecer directivas de transformación de notificaciones. Estos cmdlets proporcionan modificadores para las directivas simples y comprueban las reglas que se establecen para las directivas más complejas.  
  
**LDAP**: las directivas de transformación de notificaciones se pueden editar en Active Directory a través del Protocolo ligero de acceso a directorios (LDAP). Sin embargo, esto no es recomendable porque las directivas tienen varios componentes complejos, y las herramientas que se usan no pueden validar la Directiva antes de escribirla en Active Directory. Posteriormente, esto puede requerir una cantidad considerable de tiempo para diagnosticar problemas.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Active Directory lenguaje de reglas de transformación de notificaciones  
  
### <a name="syntax-overview"></a>Información general sobre la sintaxis  
A continuación se muestra una breve descripción de la sintaxis y la semántica del lenguaje:  
  
-   El conjunto de reglas de transformación de notificaciones consta de cero o más reglas. Cada regla tiene dos partes activas: **Seleccione la lista de condiciones y la** **acción de regla**. Si la **lista Select Condition** se evalúa como true, se ejecuta la acción de la regla correspondiente.  
  
-   **Select Condition List** tiene cero o más **condiciones Select**. Todas las **condiciones Select** deben evaluarse como true para que la **lista Select Condition** se evalúe como true.  
  
-   Cada **condición Select** tiene un conjunto de cero o más **condiciones de coincidencia**. Todas las **condiciones de coincidencia** deben evaluarse como true para que la condición Select se evalúe como true. Todas estas condiciones se evalúan con una única demanda. Una demanda que coincide con una **condición Select** se puede etiquetar mediante un **identificador** y se hace referencia a ella en la **acción de regla**.  
  
-   Cada **condición coincidente** especifica la condición que debe coincidir con el tipo **o** valor o el **tipo** de **valor** de una demanda mediante el uso de diferentes **operadores de condición** y **literales de cadena**.  
  
    -   Cuando se especifica una **condición de coincidencia** para un **valor**, también se debe especificar una **condición de coincidencia** para un **ValueType** específico y viceversa. Estas condiciones deben estar una junto a la otra en la sintaxis.  
  
    -   Las condiciones de coincidencia de **ValueType** solo deben usar literales de **tipo ValueType** específicos.  
  
-   Una **acción de regla** puede copiar una de las notificaciones etiquetadas con un **identificador** o emitir una notificaciones en función de una demanda etiquetada con un identificador y/o literales de cadena dados.  
  
**Regla de ejemplo**  
  
En este ejemplo se muestra una regla que se puede usar para traducir el tipo de notificaciones entre dos bosques, siempre que usen los mismos tipos de valor de notificaciones y tengan las mismas interpretaciones para los valores de notificaciones de este tipo. La regla tiene una condición de coincidencia y una instrucción de problema que usa literales de cadena y una referencia de notificaciones coincidentes.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operación en tiempo de ejecución  
Es importante comprender la operación en tiempo de ejecución de las transformaciones de notificaciones para crear las reglas de forma eficaz. La operación en tiempo de ejecución utiliza tres conjuntos de notificaciones:  
  
1.  **Conjunto de notificaciones de entrada**: el conjunto de entrada de notificaciones que se asignan a la operación de transformación de notificaciones.  
  
2.  **Conjunto de notificaciones de trabajo**: notificaciones intermedias que se leen y se escriben durante la transformación de notificaciones.  
  
3.  **Conjunto de notificaciones de salida**: salida de la operación de transformación de notificaciones.  
  
Esta es una breve descripción de la operación de transformación de notificaciones en tiempo de ejecución:  
  
1.  Las notificaciones de entrada para la transformación de notificaciones se usan para inicializar el conjunto de notificaciones de trabajo.  
  
    1.  Al procesar cada regla, el conjunto de notificaciones de trabajo se usa para las notificaciones de entrada.  
  
    2.  La lista de condiciones de selección de una regla coincide con todos los posibles conjuntos de notificaciones del conjunto de notificaciones de trabajo.  
  
    3.  Cada conjunto de notificaciones coincidentes se usa para ejecutar la acción en esa regla.  
  
    4.  La ejecución de una acción de regla genera una notificación, que se anexa al conjunto de notificaciones de salida y al conjunto de notificaciones de trabajo. Por lo tanto, la salida de una regla se usa como entrada para las reglas subsiguientes del conjunto de reglas.  
  
2.  Las reglas del conjunto de reglas se procesan en orden secuencial a partir de la primera regla.  
  
3.  Cuando se procesa el conjunto de reglas completo, el conjunto de notificaciones de salida se procesa para quitar las notificaciones duplicadas y otros problemas de seguridad. Las notificaciones resultantes son la salida del proceso de transformación de notificaciones.  
  
Es posible escribir transformaciones complejas de notificaciones basadas en el comportamiento anterior en tiempo de ejecución.  
  
**Ejemplo: operación en tiempo de ejecución**  
  
En este ejemplo se muestra la operación de tiempo de ejecución de una transformación de notificaciones que utiliza dos reglas.  
  
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
A continuación se muestran una sintaxis especial para las reglas:  
  
1.  Conjunto de reglas vacío = = no hay notificaciones de salida  
  
2.  Lista de condiciones Select vacía = = cada demanda coincide con la lista de condiciones Select  
  
    **Ejemplo: lista vacía de selección de condiciones**  
  
    La siguiente regla coincide con todas las notificaciones del espacio de trabajo.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Selección de lista de coincidencias vacía = = cada demanda coincide con la lista de condiciones Select  
  
    **Ejemplo: condiciones de coincidencia vacías**  
  
    La siguiente regla coincide con todas las notificaciones del espacio de trabajo. Esta es la regla básica "Allow-All" si se usa por sí sola.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Consideraciones de seguridad  
**Notificaciones que entran en un bosque**  
  
Las notificaciones presentadas por entidades de seguridad que son de entrada a un bosque deben inspeccionarse minuciosamente para asegurarse de que se permiten o se emiten solo las notificaciones correctas. Las notificaciones inadecuadas pueden poner en peligro la seguridad del bosque y esto debe ser una consideración importante a la hora de crear directivas de transformación para las notificaciones que entran en un bosque.  
  
Active Directory tiene las siguientes características para evitar la configuración inestable de notificaciones que entran en un bosque:  
  
-   Si una confianza de bosque no tiene establecida ninguna directiva de transformación de notificaciones para las notificaciones que entran en un bosque, por motivos de seguridad, Active Directory quita todas las notificaciones principales que entran en el bosque.  
  
-   Si al ejecutar el conjunto de reglas en notificaciones que entran en un bosque, se producen notificaciones que no están definidas en el bosque, las notificaciones no definidas se quitan de las notificaciones de salida.  
  
**Notificaciones que salen de un bosque**  
  
Las notificaciones que dejan un bosque presentan un problema de seguridad menor para el bosque que las notificaciones que entran en el bosque. Las notificaciones pueden dejar el bosque tal cual, incluso cuando no hay ninguna directiva de transformación de notificaciones correspondiente establecida. También se pueden emitir notificaciones que no están definidas en el bosque como parte de la transformación de notificaciones que abandonan el bosque. Esto es para configurar fácilmente las confianzas entre bosques con notificaciones. Un administrador puede determinar si es necesario transformar las notificaciones que entran en el bosque y configurar la Directiva adecuada. Por ejemplo, un administrador puede establecer una Directiva si es necesario ocultar una demanda para evitar la divulgación de información.  
  
**Errores de sintaxis en las reglas de transformación de notificaciones**  
  
Si una directiva de transformación de notificaciones determinada tiene un conjunto de reglas que es sintácticamente incorrecto o si hay otros problemas de almacenamiento o sintaxis, la Directiva se considera no válida. Esto se trata de forma diferente a las condiciones predeterminadas mencionadas anteriormente.  
  
Active Directory no puede determinar la intención en este caso y entra en un modo de error, en el que no se generan notificaciones de salida en esa confianza + dirección de exploración transversal. Se requiere la intervención del administrador para corregir el problema. Esto puede ocurrir si LDAP se usa para editar la Directiva de transformación de notificaciones. Los cmdlets de Windows PowerShell para Active Directory tienen la validación en contexto para evitar escribir una directiva con problemas de sintaxis.  
  
## <a name="other-language-considerations"></a>Consideraciones sobre otros lenguajes  
  
1.  Hay varias palabras clave o caracteres especiales en este idioma (denominados terminales). Estos se presentan en la tabla de [terminales de idioma](Claims-Transformation-Rules-Language.md#BKMK_LT) más adelante en este tema. Los mensajes de error usan las etiquetas de estos terminales para la desambiguación.  
  
2.  A veces, los terminales se pueden usar como literales de cadena. Sin embargo, este uso puede entrar en conflicto con la definición del lenguaje o tener consecuencias imprevistas. No se recomienda este tipo de uso.  
  
3.  La acción de la regla no puede realizar conversiones de tipo en valores de notificaciones y un conjunto de reglas que contiene dicha acción de regla se considera no válido. Esto provocaría un error en tiempo de ejecución y no se generarán notificaciones de salida.  
  
4.  Si una acción de regla hace referencia a un identificador que no se usó en la parte de la lista de selección de condiciones de la regla, se trata de un uso no válido. Esto produciría un error de sintaxis.  
  
    **Ejemplo: referencia de identificador incorrecta**  
    La siguiente regla muestra un identificador incorrecto que se usa en la acción de regla.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Reglas de transformación de ejemplo  
  
-   **Permitir todas las notificaciones de un tipo determinado**  
  
    Tipo exacto  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Usar Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **No permitir un determinado tipo de notificaciones**  
    Tipo exacto  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Usar Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Ejemplos de errores del analizador de reglas  
Un analizador personalizado analiza las reglas de transformación de notificaciones para comprobar si hay errores de sintaxis. Este analizador se ejecuta mediante cmdlets de Windows PowerShell relacionados antes de almacenar reglas en Active Directory. Los errores en el análisis de las reglas, incluidos los errores de sintaxis, se imprimen en la consola. Los controladores de dominio también ejecutan el analizador antes de usar las reglas para transformar las notificaciones y registran los errores en el registro de eventos (agregar números de registro de eventos).  
  
En esta sección se muestran algunos ejemplos de reglas que se escriben con una sintaxis incorrecta y los errores de sintaxis correspondientes que genera el analizador.  
  
1. Ejemplo:  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   En este ejemplo se usa un punto y coma incorrectamente en lugar de un signo de dos puntos.   
   **Mensaje de error:**  
   *POLICY0002: no se pudieron analizar los datos de la Directiva.*  
   *Número de línea: 1, número de columna: 2, token de error:;. Línea: ' C1; [] = > problema (Claim = C1); ".*  
   *Error del analizador: ' POLICY0030: error de sintaxis, no esperado '; ', se esperaba uno de los siguientes elementos: ': '. '*  
  
2. Ejemplo:  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   En este ejemplo, la etiqueta de identificador de la instrucción Copy emisión no está definida.   
   **Mensaje de error**:   
   *POLICY0011: no hay condiciones en la regla de notificaciones que coincidan con la etiqueta de condición especificada en CopyIssuanceStatement: ' C2 '.*  
  
3. Ejemplo:  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool" no es un terminal en el idioma y no es un tipo ValueType válido. Los terminales válidos se enumeran en el mensaje de error siguiente.   
   **Mensaje de error:**  
   *POLICY0002: no se pudieron analizar los datos de la Directiva.*  
   Número de línea: 1, número de columna: 39, token de error: "bool". Línea: ' C1: [type = = "x1", Value = = "1", ValueType = = "bool"] = > problema (Claim = C1); ".   
   *Error del analizador: ' POLICY0030: error de sintaxis, ' cadena ' inesperado, se esperaba uno de los siguientes elementos: ' INT64_TYPE ' ' UINT64_TYPE ' ' STRING_TYPE ' ' BOOLEAN_TYPE ' ' identificador '*  
  
4. Ejemplo:  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   El número **1** en este ejemplo no es un token válido en el lenguaje y dicho uso no se permite en una condición de coincidencia. Debe escribirse entre comillas dobles para convertirlo en una cadena.   
   **Mensaje de error:**  
   *POLICY0002: no se pudieron analizar los datos de la Directiva.*  
   *Número de línea: 1, número de columna: 23, token de error: 1. línea: ' C1: [type = = "x1", Value = = 1, ValueType = = "bool"] = > problema (Claim = C1); '.* <em>Error del analizador: ' POLICY0029: entrada inesperada.</em>  
  
5. Ejemplo:  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   En este ejemplo se usa un signo igual doble (= =) en lugar de un único signo igual (=).   
   **Mensaje de error:**  
   *POLICY0002: no se pudieron analizar los datos de la Directiva.*  
   *Número de línea: 1, número de columna: 91, token de error: = =. Línea: ' C1: [type = = "x1", Value = = "1",*  
   *ValueType = = "Boolean"] = > problema (Type = C1. Type, Value = "0", ValueType = = "Boolean"); ".*  
   *Error del analizador: ' POLICY0030: error de sintaxis, inesperado ' = = ', se esperaba uno de los siguientes elementos: ' = '*  
  
6. Ejemplo:  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   Este ejemplo es correcto sintácticamente y semánticamente. Sin embargo, el uso de "Boolean" como valor de cadena se enlaza para causar confusión y debe evitarse. Como se mencionó anteriormente, el uso de los terminales de idioma como valores de notificaciones debe evitarse siempre que sea posible.  
  
## <a name="language-terminals"></a><a name="BKMK_LT"></a>Terminales de idioma  
En la tabla siguiente se muestra el conjunto completo de cadenas de terminal y los terminales de idioma asociados que se usan en el lenguaje de reglas de transformación de notificaciones. Estas definiciones usan cadenas UTF-16 que no distinguen mayúsculas de minúsculas.  
  
|String|Terminal|  
|----------|------------|  
|"= >"|IMPLICA|  
|";"|PUNTO Y coma|  
|":"|Y|  
|","|UNAS|  
|"."|SEMITONO|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|AJUSTES|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|QUITAR|  
|"& &"|AND|  
|publicación|PROBLEMA|  
|automáticamente|TYPE|  
|valor|VALOR|  
|ValueType|VALUE_TYPE|  
|declara|DECLARA|  
|"[_A-Za-z] [_A-Za-z0-9] *"|IDENTIFICADOR|  
|"\\" [^\\"\n] *\\" "|String@|  
|UInt64|UINT64_TYPE|  
|Int64|INT64_TYPE|  
|"string"|STRING_TYPE|  
|booleano|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintaxis del lenguaje  
El siguiente lenguaje de reglas de transformación de notificaciones se especifica en formato ABNF. Esta definición usa los terminales especificados en la tabla anterior, además de las producciones ABNF definidas aquí. Las reglas se deben codificar en UTF-16 y las comparaciones de cadenas se deben tratar como no distinguir mayúsculas de minúsculas.  
  
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
  


