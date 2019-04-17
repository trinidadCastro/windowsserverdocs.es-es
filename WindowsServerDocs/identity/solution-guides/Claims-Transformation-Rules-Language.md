---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: "Lenguaje de las reglas de transformación de notificaciones"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="claims-transformation-rules-language"></a>Lenguaje de las reglas de transformación de notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El bosque a través de notificaciones permite de función de transformación que puente las reclamaciones por el Control de acceso dinámico a través de los límites del bosque mediante la configuración de directivas de transformación reclamaciones en confianzas entre bosques. El componente principal de todas las directivas es reglas que se escriben en lenguaje de las reglas de transformación de notificaciones. En este tema proporciona detalles sobre este idioma y proporciona instrucciones sobre la creación de reglas de transformación de notificaciones.  
  
Los cmdlets de PowerShell de Windows para directivas de la transformación en entre bosques confía tienen opciones a establecer directivas simple que son necesarios en común escenarios. Estos cmdlets traducir la entrada del usuario en las directivas y reglas en el lenguaje de las reglas de transformación de notificaciones y luego almacenarlas en Active Directory en el formato indicado. Para obtener más información sobre los cmdlets de transformación de notificaciones, consulta el [Cmdlets de AD DS para el Control de acceso dinámico](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
Según la configuración de notificaciones y los requisitos que se colocan en la confianza en el bosque de los bosques de Active Directory, las directivas de la transformación de notificaciones que tenga que ser más complejo que las directivas compatibles con los cmdlets de Windows PowerShell para Active Directory. Para crear de forma eficaz estas directivas, es esencial para comprender la sintaxis de lenguaje de reglas de transformación de notificaciones y la semántica. Esto dice el lenguaje de las reglas de transformación ("idioma") en Active Directory es un subconjunto del idioma que se usa en [los servicios de federación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=243982) para fines y similares, tiene una sintaxis y semántica muy similar. Sin embargo, hay menos operaciones permitidas y restricciones adicionales de sintaxis se colocan en la versión de Active Directory del idioma.  
  
En este tema se explica brevemente la sintaxis y la semántica del lenguaje de las reglas de transformación de reclamaciones en Active Directory y las consideraciones de debe realizar al crear directivas. Proporciona varios conjuntos de reglas de ejemplo para empezar a trabajar y los ejemplos de una sintaxis incorrecta y los mensajes que se generan, que te ayudará a descifrarlos mensajes de error al crear las reglas.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Herramientas de creación de directivas de la transformación de notificaciones  
**Cmdlets de Windows PowerShell para Active Directory**: esta es la manera preferida y recomendada para crear y establecer directivas de transformación de notificaciones. Estos cmdlets proporcionar modificadores de directivas simple y comprobar las reglas que se establecen las directivas más complejas.  
  
**LDAP**: directivas de transformación de notificaciones se pueden editar en Active Directory mediante el protocolo ligero de acceso a directorios (LDAP). Sin embargo, esto no se recomienda debido a las directivas tienen varios componentes complejos y las herramientas que no pueden validar la directiva antes de escribir en Active Directory. Posteriormente, esto puede requerir una cantidad considerable de tiempo para diagnosticar problemas.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Lenguaje de transformación de las reglas de reclamaciones de Active Directory  
  
### <a name="syntax-overview"></a>Información general de la sintaxis  
Aquí te mostramos una breve introducción de la sintaxis y la semántica del idioma:  
  
-   El conjunto de reglas de transformación de reclamaciones consta de reglas de cero o más. Cada regla tiene dos partes activos: **selecciona la lista de condición** y **la acción de regla**. Si la **selecciona la lista de condición** da como resultado "true", se ejecuta la acción de regla correspondiente.  
  
-   **Selecciona la lista condición** tiene cero o más **selecciona condiciones**. Todos los **selecciona condiciones** debes evaluar en TRUE para el **selecciona la lista de condición** evalúe en TRUE.  
  
-   Cada **selecciona condición** tiene un conjunto de cero o más **condiciones de coincidencia de**. Todos los **condiciones de coincidencia de** debe evaluará en TRUE para que la condición selecciona evaluará en TRUE. Todas estas condiciones se evalúan según una sola solicitud. Una notificación que coincide con un **selecciona condición** pueden etiquetar por un **identificador** y contempladas en el **la acción de regla**.  
  
-   Cada **coincidencia condición** especifica la condición para que coincida con el **tipo** o **valor** o **ValueType** por una reclamación mediante el uso de diferentes **condición operadores** y **literales de cadena**.  
  
    -   Cuando especifiques un **coincidencia condición** para un **valor**, también debes especificar un **coincidencia condición** para un determinado **ValueType** y viceversa. Estas condiciones deben estar juntos en la sintaxis.  
  
    -   **ValueType** coincidencia condiciones debe utilizar específico **ValueType** literales solo.  
  
-   Un **la acción de regla** copiar una notificación que se etiqueta con un **identificador** o de un problema de una notificación en función de una reclamación que se etiqueta con un identificador o dada literales de cadena.  
  
**Regla de ejemplo**  
  
Este ejemplo muestra una regla que puede usarse para traducir las notificaciones de tipo entre dos bosques, siempre que usar las mismas notificaciones tipos de valor y tener el mismo interpretaciones para reclamaciones valores para este tipo. La regla tiene una condición coincidente y una declaración del problema que usa una referencia de reclamaciones coincidente y literales de cadena.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operación en tiempo de ejecución  
Es importante comprender el funcionamiento de en tiempo de ejecución de las transformaciones de notificaciones para crear las reglas de forma eficaz. La operación en tiempo de ejecución usa tres conjuntos de notificaciones:  
  
1.  **Conjunto de notificaciones de entrada**: el conjunto de entrada de una reclamación que se asignan a la operación de transformación de notificaciones.  
  
2.  **Conjunto de trabajo reclamaciones**: intermedios notificaciones que se leen y escritas durante la transformación de solicitudes.  
  
3.  **Conjunto de notificaciones de salida**: salida de la operación de transformación de notificaciones.  
  
Esta es una breve descripción de la operación de transformación de notificaciones en tiempo de ejecución:  
  
1.  Solicitudes de entrada para la transformación de solicitudes se usan para inicializar el conjunto de notificaciones de trabajo.  
  
    1.  Cuando se procesa cada regla, el conjunto de notificaciones de trabajo se usa para las notificaciones de entrada.  
  
    2.  La lista de la condición de selección en una regla se compara con todos los conjuntos de posibles de una reclamación del conjunto de notificaciones de trabajo.  
  
    3.  Cada conjunto de notificaciones que coinciden se usa para ejecutar la acción en dicha regla.  
  
    4.  Ejecuta una regla de resultados de la acción en una notificación, que se anexa a la salida notificaciones conjunto y el conjunto de notificaciones de trabajo. Por lo tanto, el resultado de una regla se usa como entrada para las reglas en el conjunto de reglas.  
  
2.  Las reglas en el conjunto de reglas se procesan en orden secuencial a partir de la primera regla.  
  
3.  Cuando se procesa el conjunto de reglas todo, se procesa el conjunto de notificaciones de salida para quitar notificaciones duplicadas y otros problemas de seguridad. Las notificaciones resultantes son el resultado del proceso de transformación de notificaciones.  
  
Es posible escribir transformaciones reclamaciones complejas en función del comportamiento en tiempo de ejecución anterior.  
  
**Ejemplo: Operación de en tiempo de ejecución**  
  
Este ejemplo muestra la operación en tiempo de ejecución de una transformación de notificaciones que usa dos reglas.  
  
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
Estos son una sintaxis especial para las reglas:  
  
1.  Conjunto de reglas vacío no == solicitudes de salida  
  
2.  Vaciar la lista de la condición de seleccionar == cada notificación coincide con la lista de condición selecciona  
  
    **Ejemplo: Lista de la condición de selecciona vacío**  
  
    La siguiente regla coincide con cada solicitud del conjunto de trabajo.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Vaciar la lista de coincidencia seleccionar == cada coincidencias de notificación de la lista de condición selecciona  
  
    **Ejemplo: Condiciones de coincidencia vacío**  
  
    La siguiente regla coincide con cada solicitud del conjunto de trabajo. Esto es la regla "Permitir a todos" básico si se usa solamente.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Consideraciones de seguridad  
**Notificaciones que introduzca un bosque**  
  
Las solicitudes presentadas entidades de seguridad entrantes de un bosque necesitan que se inspeccionarán minuciosamente para garantizar que no se permiten o emitir solo las aserciones correctas. Reclamaciones incorrectas pueden comprometer la seguridad de bosque y debe ser superior debe tener en cuenta al crear directivas de transformación para notificaciones que introduzca un bosque.  
  
Active Directory tiene las siguientes características para evitar el error de configuración de notificaciones que introduzca un bosque:  
  
-   Si una confianza de bosque carece de una directiva de transformación de reclamaciones establecido para las notificaciones que introduzca un bosque, por motivos de seguridad, Active Directory quita todas las solicitudes de entidad de seguridad que entren en el bosque.  
  
-   Si la regla Activar notificaciones que entra en un resultado de bosque de notificaciones que no están definidas en el bosque en ejecución, se quitan las notificaciones no definidas de las notificaciones de salida.  
  
**Notificaciones que salir de un bosque**  
  
Notificaciones que salir de un bosque presentan un problema de seguridad menor para el bosque de las notificaciones que se escribe en el bosque. Reclamaciones pueden salir del bosque como-es incluso cuando no hay ningún reclamaciones correspondientes directiva de la transformación en su lugar. También es posible emitir notificaciones que no están definidas en el bosque como parte de transformar notificaciones que salir del bosque. Esto sirve para configurar fácilmente confianzas entre bosques con notificaciones. Un administrador puede determinar si necesitan reclamaciones que entren en el bosque se transforman y configurar la directiva apropiada. Por ejemplo, un administrador puede establecer una directiva si es necesario para ocultar una reclamación a evitar la divulgación de información.  
  
**Errores de sintaxis en las reglas de transformación de notificaciones**  
  
Si una directiva de transformación reclamaciones determinado tiene un conjunto de reglas que es sintácticamente incorrecto o si hay otros problemas de almacenamiento o la sintaxis, la directiva se considera no válida. Esto se trata de forma diferente a las condiciones de forma predeterminada mencionadas anteriormente.  
  
Active Directory no puede determinar la intención en este caso y entra en un modo a prueba de errores, donde no solicitudes de salida se generan en esa confianza y la dirección del recorrido. Es necesaria la intervención del administrador para corregir el problema. Esto puede ocurrir si LDAP se usa para modificar la directiva de transformación de notificaciones. Cmdlets de Windows PowerShell para Active Directory tiene validación para evitar que escribir una directiva con problemas de sintaxis.  
  
## <a name="other-language-considerations"></a>Otras consideraciones de idioma  
  
1.  Hay varias palabras clave o caracteres especiales en este lenguaje (denominado terminales). Estos se presentan en el [terminales idioma](Claims-Transformation-Rules-Language.md#BKMK_LT) tabla más adelante en este tema. Los mensajes de error usan las etiquetas para estos terminales para desambiguación.  
  
2.  Terminales a veces, pueden usarse como literales de cadena. Sin embargo, dicho uso puede entrar en conflicto con la definición de idioma o tener consecuencias imprevistas. Este tipo de uso no se recomienda.  
  
3.  La acción de regla no puede realizar las conversiones de tipos de valores de notificaciones, y un conjunto de reglas que contiene dicha acción de regla se considera válido. Esto podría provocar un error en tiempo de ejecución, y no solicitudes de salida se producen.  
  
4.  Si una acción de regla hace referencia a un identificador que no se usó en la parte de seleccionar la lista de condición de la regla, es un uso no válido. Esto podría provocar un error de sintaxis.  
  
    **Ejemplo: Referencia de identificador incorrecto**  
    La siguiente regla ilustra un identificador incorrecto usado en la acción de regla.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Reglas de transformación de muestra  
  
-   **Permitir todas las reclamaciones de un determinado tipo**  
  
    Tipo exacto  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Usar Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **No permitir a un determinado tipo de notificación**  
    Tipo exacto  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Usar Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Algunos ejemplos de errores del analizador de reglas  
Se analizan las reglas de transformación de reclamaciones por un analizador personalizado para comprobar si hay errores de sintaxis. Analizador de esta se ejecuta mediante los cmdlets de Windows PowerShell relacionados antes de almacenar las reglas en Active Directory. Los errores de análisis de las reglas, incluidos errores de sintaxis, se imprimen en la consola. Controladores de dominio ejecutan también el analizador antes de usar las reglas para transformar notificaciones y registrar los errores en el registro de eventos (agregar números de registro de eventos).  
  
Esta sección muestran algunos ejemplos de reglas que se escriben con una sintaxis incorrecta y la sintaxis correspondiente errores generados por el analizador.  
  
1.  Ejemplo:  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    Este ejemplo tiene un punto y coma incorrectamente usado en lugar de dos puntos.   
    **Mensaje de error:**  
    *POLICY0002: No se pudo analizar datos de la directiva.*  
    *Número de línea: 1, número de columnas: 2, token de Error:;. Línea: ' c1; [] = > Issue(claim=c1);'.*  
    *Error del analizador: ' POLICY0030: error de sintaxis, inesperado ';', espera uno de los siguientes: ':'.'*  
  
2.  Ejemplo:  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    En este ejemplo, la etiqueta de identificador en la declaración de emisión de la copia no está definida.   
    **Mensaje de error**:   
    *POLICY0011: Ninguna condición en la regla de Reclamación coincide con la etiqueta de la condición especificada en el CopyIssuanceStatement: 'c2'.*  
  
3.  Ejemplo:  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    "bool" no es un Terminal en el idioma, y no es un tipo de valor válido. Terminales válidos se enumeran en el siguiente mensaje de error.   
    **Mensaje de error:**  
    *POLICY0002: No se pudo analizar datos de la directiva.*  
    Número de línea: 1, número de columnas: 39, token de Error: "bool". Línea: ' c1: [tipo == "x1" valor == "1", valuetype == "bool"] = > Issue(claim=c1);'.   
    *Error del analizador: ' POLICY0030: error de sintaxis inesperado 'cadena', espera uno de los siguientes: 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE', 'BOOLEAN_TYPE', 'Identificador'*  
  
4.  Ejemplo:  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    El número **1** en este ejemplo no es un token de seguridad en el idioma y no se permite dicho uso en una condición coincidente. Tiene que estar entre comillas dobles para que sea una cadena.   
    **Mensaje de error:**  
    *POLICY0002: No se pudo analizar datos de la directiva.*  
    *Número de línea: 1, número de columnas: 23 token de Error: 1. línea: ' c1: [tipo == "x1" valor == 1, valuetype == "bool"] = > Issue(claim=c1);'. * *Error del analizador: ' POLICY0029: entrada inesperada.*  
  
5.  Ejemplo:  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    Este ejemplo usa un signo igual doble (==) en lugar de un signo igual (=).   
    **Mensaje de error:**  
    *POLICY0002: No se pudo analizar datos de la directiva.*  
    *Número de línea: 1, número de columnas: 91, token de Error: ==. Línea: ' c1: [tipo == "x1" valor == "1",*  
    *ValueType == "boolean"] = > problema (type=c1.type, valor = "0", valuetype == "boolean");'.*  
    *Error del analizador: ' POLICY0030: error de sintaxis, inesperado '==', espera uno de los siguientes: '='*  
  
6.  Ejemplo:  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    Este ejemplo es sintácticamente y semánticamente correcto. Sin embargo, con "boolean" como un valor de cadena se enlaza a causar confusión y debe evitarse. Como se mencionó anteriormente, usar terminales de lenguaje como valores de notificaciones que deben evitarse siempre que sea posible.  
  
## <a name="BKMK_LT"></a>Terminales de idioma  
La siguiente tabla enumera el conjunto completo de terminal cadenas y los que se usan en el lenguaje de las reglas de transformación de reclamaciones terminales de idioma asociado. Estas definiciones de usan cadenas de UTF-16 entre mayúsculas y minúsculas.  
  
|Cadena|Terminal|  
|----------|------------|  
|"=>"|IMPLICA|  
|";"|PUNTO Y COMA|  
|":"|DOS PUNTOS|  
|","|COMA|  
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
|"type"|TIPO|  
|"value"|VALOR|  
|"valuetype"|VALUE_TYPE|  
|"reclamación"|NOTIFICACIÓN|  
|"[_A-Za-z][_A-Za-z0-9]*"|IDENTIFICADOR|  
|"\\"[^\\"\n]*\\""|CADENA|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"string"|STRING_TYPE|  
|"booleano"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintaxis de lenguaje  
Los siguientes idiomas de reglas de transformación de notificaciones se especifican en forma ABNF. Esta definición usa los terminales que se especifican en la tabla anterior, además de la producción de ABNF definida aquí. Las reglas deben codificarse en UTF-16 y las comparaciones de cadenas deben tratarse como mayúsculas de minúsculas.  
  
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
  


