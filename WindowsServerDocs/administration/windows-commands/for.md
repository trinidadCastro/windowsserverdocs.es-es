---
title: para
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e275726c-035f-4a74-8062-013c37f5ded1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13a44bc3497b44d60bd4d351e389d493a50f1269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869466"
---
# <a name="for"></a>para



Ejecuta un comando especificado para cada archivo en un conjunto de archivos.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|{%%\|%}\<Variable>|Obligatorio. Representa un parámetro reemplazable. Use un único signo de porcentaje (**%**) para llevar a cabo la **para** en el símbolo del sistema del comando. Utilice signos de porcentaje doble (**%%**) para llevar a cabo la **para** comando dentro de un archivo por lotes. Las variables distinguen mayúsculas de minúsculas y se deben representar con un valor alfabético como **%a)**, **%B**, o **%C**.|
|(\<Establecido >)|Obligatorio. Especifica uno o varios archivos, directorios, o cadenas de texto o un intervalo de valores en el que se ejecute el comando. Los paréntesis son obligatorios.|
|\<Command>|Obligatorio. Especifica el comando que desea llevar alejar en cada archivo, directorio o cadena de texto o en el intervalo de valores incluidos en *establecer*.|
|\<CommandLineOptions>|Especifica las opciones de línea de comandos que desea usar con el comando especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Uso de **para**

    Puede usar el **para** comando dentro de un archivo por lotes o directamente desde el símbolo del sistema.
-   Uso de parámetros de proceso por lotes

    Los siguientes atributos se aplican a la **para** comando:  
    -   El **para** comando reemplaza **% *** Variable* o **%% *** Variable* con cada cadena de texto en el conjunto especificado hasta que el comando especificado procesa todos los archivos.
    -   Nombres de variables distinguen entre mayúsculas y minúsculas, global y no más de 52 pueden estar activas simultáneamente.
    -   Para evitar la confusión con los parámetros del lote **%0** a través de **%9**, puede usar cualquier carácter de *Variable* excepto los números del 0 al 9. Simples archivos por lotes, un único carácter como **%%** funcionará.
    -   Puede usar varios valores para *Variable* en archivos por lotes complejos para distinguir las distintas variables reemplazables.
-   Especificar un grupo de archivos

    El *establecer* parámetro puede representar un único grupo de archivos o varios grupos de archivos. Puede usar caracteres comodín (**&#42;** y **?**) para especificar un archivo de conjunto. Estos son grupos de archivos válidos:  
    ```
    (*.doc) 
    (*.doc *.txt *.me)
    (jan*.doc jan*.rpt feb*.doc feb*.rpt)
    (ar??1991.* ap??1991.*)
    ```  
    Cuando se usa el **para** comando, el primer valor de *establecer* reemplaza **% *** Variable* o **%% *** Variable*y, a continuación, el especificado comando procesa este valor. Este proceso continúa hasta que todos los archivos (o grupos de archivos) que corresponden a la *establecer* valor se procesan.
-   Mediante el **en** y **hacer** palabras clave

    **En** y **hacer** no son parámetros, pero se debe utilizar con **para**. Si se omite cualquiera de estas palabras clave, aparece un mensaje de error.
-   Uso de otras formas de **para**

    Si se habilitan las extensiones de comando (es decir, el valor predeterminado), las siguientes formas adicionales de **para** se admiten:  
    -   Solo los directorios

        Si *establecer* contiene caracteres comodín (**&#42;** o **?**), especificado *comando* ejecuta para cada directorio (en lugar de un conjunto archivos de un directorio especificado) que coincida con *establecer*.

        La sintaxis es la siguiente:  
        ```
        for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
        ```  
    -   Recursive

        Recorre el árbol de directorio que se basa en *unidad*:*ruta* y ejecuta el **para** instrucción en cada directorio del árbol. Si no se especifica ningún directorio después de **/r**, se usa el directorio actual como directorio raíz. Si *establecer* es simplemente un punto (.), sólo se enumerará el árbol de directorios.

        La sintaxis es la siguiente:  
        ```
        for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
        ```  
    -   Recorrer en iteración un intervalo de valores

        Utilice una variable iterativa para establecer el valor inicial (*iniciar*#) y, a continuación, paso a través de un intervalo de conjunto de valores hasta que el valor supera el valor final de conjunto (*final*#). **/l** ejecutará la iteración mediante la comparación *iniciar*# con *final*#. Si *iniciar*# es menor que *final*# se ejecutará el comando. Cuando se supera la variable iterativa *final*#, el shell de comandos sale del bucle. También puede usar un negativo *paso*# paso a paso a través de un intervalo de valores que disminuyen. Por ejemplo, (1,1,5) genera la secuencia 1 2 3 4 5 y (5,-1,1) genera la secuencia 5 4 3 2 1.

        La sintaxis es la siguiente:  
        ```
        for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
        ```  
    -   Recorrer en iteración y análisis de archivos

        Usar análisis de archivos para la salida del comando de proceso, cadenas y contenido del archivo.  Usar variables de iteración para definir el contenido o las cadenas que desea examinar y usar las distintas *palabrasClaveDeAnálisis* opciones para modificar aún más el análisis.  Use la *palabrasClaveDeAnálisis* opción para especificar qué muestras deben pasarse como variables de iteración de token. Tenga en cuenta que cuando se usa sin la opción de token, **/f** examinará solo el primer token.

        Análisis del archivo consta de leer la salida, la cadena o el contenido del archivo y, a continuación, dividirlo en líneas individuales de texto y analizar cada línea en cero o más tokens. El **para** , a continuación, se llama al bucle con el valor de la variable iterativo establecido en el token. De forma predeterminada, **/f** pasa el primer espacio en blanco separado símbolo (token) de cada línea de cada archivo. Se omiten las líneas en blanco.

        La sintaxis son:  
        ```
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ("<LiteralString>") do <Command> [<CommandLineOptions>]
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
        ```  
        El *establecer* argumento especifica uno o más nombres de archivo. Cada archivo se puede abrir, leer y procesar antes de continuar con el siguiente archivo de *establecer*. Para invalidar el comportamiento predeterminado del análisis, especifique *palabrasClaveDeAnálisis*. Se trata de una cadena entre comillas que contiene una o más palabras clave para especificar diferentes opciones de análisis.

        Si usas el **usebackq** opción, use una de las siguientes sintaxis:  
        ```
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ("<Set>") do <Command> [<CommandLineOptions>]
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
        ```  
        En la tabla siguiente se enumera las palabras clave de análisis que puede usar para *palabrasClaveDeAnálisis*.  
        |Palabra clave|Descripción|
        |-------|-----------|
        |eol=\<c>|Especifica un carácter de final de línea (sólo un carácter).|
        |omitir =\<N >|Especifica el número de líneas que se omitirán al principio del archivo.|
        |delims=\<xxx>|Especifica un conjunto de delimitadores. Esto reemplaza el conjunto de delimitadores predeterminado de espacio y tabulación.|
        |los tokens =\<X, Y M: N >|Especifica los testigos de cada línea que se pasarán a la **para** bucle para cada iteración. Como resultado, se asignan los nombres de variables adicionales. *M*–*N* especifica un intervalo, desde el *M*th a través de la *N*tokens th. Si el último carácter de la **tokens =** cadena es un asterisco (**&#42;**), se asigna una variable adicional y recibe el texto restante en la línea después del último token que se analiza.|
        |usebackq|Especifica hasta: ejecutar una cadena entre comillas back como un comando, utilice una cadena entre comillas simples como una cadena literal o, para los nombres de archivo largos que contienen espacios, permitir que los nombres de archivo en  *\<establecer\>*, a cada uno se entre en entre comillas dobles.|
    -   Sustitución de variables

        En la tabla siguiente muestra la sintaxis opcional (para cualquier variable **me**).  
        |Variable con modificador|Descripción|
        |----------------------|-----------|
        |%~I|Se expande **%I** que quita todas las comillas ("").|
        |%~fI|Se expande **%I** a un nombre de ruta de acceso completa.|
        |%~dI|Se expande **%I** a solo una letra de unidad.|
        |%~pI|Se expande **%I** a solo una ruta de acceso.|
        |%~nI|Se expande **%I** a solo un nombre de archivo.|
        |%~xI|Se expande **%I** a sólo una extensión de nombre de archivo.|
        |%~sI|Expande la ruta de acceso para que contenga solo los nombres cortos.|
        |%~aI|Se expande **%I** a los atributos de archivo del archivo.|
        |%~tI|Se expande **%I** a la fecha y hora del archivo.|
        |%~zI|Se expande **%I** al tamaño del archivo.|
        |%~$PATH:I|Busca los directorios mostrados en la variable de entorno PATH y expande **%I** en el nombre completo de la primera se encuentra el directorio. Si el nombre de variable de entorno no está definido o no se encuentra el archivo en la búsqueda, este modificador se expande a una cadena vacía.|

        En la tabla siguiente se enumera combinaciones de modificador que puede usar para obtener resultados compuestos.  
        |Variable con modificadores combinados|Descripción|
        |--------------------------------|-----------|
        |%~dpI|Se expande **%I** a una letra de unidad y ruta de acceso de solo.|
        |%~nxI|Se expande **%I** sólo a un nombre de archivo y la extensión.|
        |%~fsI|Se expande **%I** a un nombre de ruta de acceso completa con solo los nombres cortos.|
        |%~dp$PATH:I|Busca en los directorios que se muestran en la variable de entorno PATH para **%I** y se expande a la letra de unidad y ruta de acceso de la primera de ellas se encuentra.|
        |%~ftzaI|Se expande **%I** a una línea de salida es similar a **dir**.|

        En los ejemplos anteriores, se puede reemplazar **%I** y ruta de acceso con otros valores válidos. Válido **para** nombre de variable finaliza el **%~** sintaxis.

        Con los nombres de variables en mayúsculas como **%I**, puede que el código sea más legible y evitar la confusión con los modificadores, que no distinguen mayúsculas de minúsculas.
-   Analizar una cadena

    Puede usar el **para /f** lógica en una cadena inmediata de análisis ajustando *\<LiteralString\>* de cualquiera: comillas dobles (*sin* " usebackq") o comillas simples (*con* "usebackq"), por ejemplo, ("MiCadena") o ("MiCadena"). *\<LiteralString\>*  se trata como una sola línea de entrada desde un archivo. Al analizar *\<LiteralString\>* en comillas dobles, símbolos de comandos (como **\\ \& \| \> \< \^**) se tratan como caracteres normales.
-   Salida de análisis

    Puede usar el **para /f** comando para analizar la salida de un comando colocando comillas back *\<comando\>* entre paréntesis. Se trata como una línea de comandos que se pasa a un elemento secundario de Cmd.exe. El resultado se captura en la memoria y se analiza como si es un archivo.

## <a name="BKMK_examples"></a>Ejemplos

Para usar **para** en un archivo por lotes, use la sintaxis siguiente:
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Para mostrar el contenido de todos los archivos en el directorio actual que tengan la extensión .doc o. txt mediante la variable reemplazable **%f**, tipo:
```
for %f in (*.doc *.txt) do type %f 
```
En el ejemplo anterior, cada archivo que tenga la extensión .doc o. txt en el directorio actual se sustituye por el **%f** variable hasta que se muestra el contenido de cada archivo. Para usar este comando en un archivo por lotes, reemplace cada aparición de **%f** con **%%**. En caso contrario, se omite la variable y se muestra un mensaje de error.

Para analizar un archivo, omitiendo las líneas de comentarios, tipo:
```
for /f "eol=; tokens=2,3* delims=," %i in (myfile.txt) do @echo %i %j %k
```
Este comando analiza cada línea en Myfile.txt. Se omite las líneas que comienzan con un punto y coma y pasa el token de segundo y tercero de cada línea para la **para** cuerpo (tokens están delimitados por comas o espacios). El cuerpo de la **para** instrucción hace referencia **%i** para obtener el token, el segundo **%j** para obtener el token terceros, y **%k** para obtener todos los restantes tokens. Si los nombres de archivo que se proporciona contienen espacios, utilice comillas alrededor del texto (por ejemplo, "nombre de archivo"). Para usar las comillas, debe usar **usebackq**. En caso contrario, las comillas se interpretan como definir una cadena literal a analizar.

**%i** se declara explícitamente en el **para** instrucción. **%j** y **%k** se declaran implícitamente mediante **tokens =**. Puede usar **tokens =** para especificar hasta 26 tokens, siempre que no hace un intento declarar una variable superior a la letra "z" o "Z".

El ejemplo siguiente enumeran los nombres de variable de entorno en el entorno actual. Para analizar la salida de un comando mediante la colocación de *establecer* entre paréntesis, escriba:
```
for /f "usebackq delims==" %i in ('set') do @echo %i 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
