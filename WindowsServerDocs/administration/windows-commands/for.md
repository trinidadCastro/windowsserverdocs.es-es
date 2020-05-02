---
title: for
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e275726c-035f-4a74-8062-013c37f5ded1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f13f6c4b98af141d19782d297200dfd18ab65ff0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725582"
---
# <a name="for"></a>for



Ejecuta un comando especificado para cada archivo en un conjunto de archivos.



## <a name="syntax"></a>Sintaxis

```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|{%%\|%} \<> de variable|Necesario. Representa un parámetro reemplazable. Use un solo signo de porcentaje**%**() para llevar a cabo el comando **for** en el símbolo del sistema. Use signos de porcentaje doble**%%**() para llevar a cabo el comando **for** en un archivo por lotes. Las variables distinguen mayúsculas de minúsculas y deben representarse con un valor alfabético como **% A**, **% B**o **% C**.|
|(\<Establecer>)|Necesario. Especifica uno o más archivos, directorios o cadenas de texto, o un intervalo de valores en el que se ejecuta el comando. Es obligatorio utilizar paréntesis.|
|\<> de comandos|Necesario. Especifica el comando que se desea llevar a cabo en cada archivo, directorio o cadena de texto, o en el intervalo de valores incluidos en el *conjunto*.|
|\<> CommandLineOptions|Especifica las opciones de línea de comandos que desea utilizar con el comando especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- Usar **para**

  Puede usar el comando **for** en un archivo por lotes o directamente desde el símbolo del sistema.
- Usar parámetros de Batch

  Los siguientes atributos se aplican al comando **for** :  
  - El comando **for** reemplaza **%** la <em>variable</em> o **%%** <em>variable</em> por cada cadena de texto en el conjunto especificado hasta que el comando especificado procesa todos los archivos.
  - Los nombres de variables distinguen mayúsculas de minúsculas, global y no más de 52 pueden estar activas a la vez.
  - Para evitar la confusión con los parámetros de lote **%0** a **%9**, puede usar cualquier carácter para la *variable* , excepto los números del 0 al 9. En el caso de los archivos por lotes sencillos, un solo carácter, como **%% f** , funcionará.
  - Puede usar varios valores para *variables* en archivos por lotes complejos para distinguir las diferentes variables reemplazables.
- Especificar un grupo de archivos

  El parámetro *set* puede representar un único grupo de archivos o varios grupos de archivos. Puede usar caracteres comodín (**&#42;** y **?**) para especificar un conjunto de archivos. Los siguientes son conjuntos de archivos válidos:  
  ```
  (*.doc) 
  (*.doc *.txt *.me)
  (jan*.doc jan*.rpt feb*.doc feb*.rpt)
  (ar??1991.* ap??1991.*)
  ```  
  Cuando se usa el comando **for** , el primer valor de *set* reemplaza **%** a <em>variable</em> o **%%** <em>variable</em>y, a continuación, el comando especificado procesa este valor. Esto continúa hasta que se procesan todos los archivos (o grupos de archivos) que corresponden al valor *establecido* .
- Usar las palabras clave **in** y **do**

  **En** y **no** son parámetros, pero debe usarlos con **para**. Si omite cualquiera de estas palabras clave, aparece un mensaje de error.
- Usar formas adicionales de **para**

  Si las extensiones de comandos están habilitadas (que es el valor predeterminado), se admiten las siguientes formas adicionales de **para para** :  
  - Solo directorios

    Si *set* contiene caracteres comodín (**&#42;** o **?**), el *comando* especificado se ejecuta para cada directorio (en lugar de un conjunto de archivos en un directorio especificado) que coincida con *set*.

    La sintaxis es:  
    ```
    for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
    ```  
  - Recursive

    Recorre el árbol de directorios cuya raíz se encuentra en *unidad*:*ruta de acceso* y ejecuta la instrucción **for** en cada directorio del árbol. Si no se especifica ningún directorio después de **/r**, se usa el directorio actual como directorio raíz. Si *set* es un solo punto (.), solo enumera el árbol de directorios.

    La sintaxis es:  
    ```
    for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    ```  
  - Recorrer en iteración un intervalo de valores

    Use una variable iterativa para establecer el valor inicial (#*Start*#) y, a continuación, recorra un intervalo de valores establecido hasta que el valor supere el valor de finalización de conjunto (*End*#). **/l** ejecutará la iteración mediante la comparación de *Start*# con *End*#. Si *Start*# es menor que *End*#, se ejecutará el comando. Cuando la variable iterativa supera *End*#, el shell de comandos sale del bucle. También puede usar un *paso*negativo para recorrer un intervalo en valores decrecientes. Por ejemplo, (1, 1, 5) genera la secuencia 1 2 3 4 5 y (5,-1, 1) genera la secuencia 5 4 3 2 1.

    La sintaxis es:  
    ```
    for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
    ```  
  - Iteración y análisis de archivos

    Use el análisis de archivos para procesar los resultados del comando, las cadenas y el contenido del archivo.  Use variables iterativas para definir el contenido o las cadenas que desea examinar y use las distintas opciones de *ParsingKeywords* para modificar aún más el análisis.  Use la opción de token *ParsingKeywords* para especificar los tokens que se deben pasar como variables iterativas. Tenga en cuenta que cuando se usa sin la opción de token, **/f** solo examinará el primer token.

    El análisis de archivos consiste en leer el contenido de la salida, la cadena o el archivo y, a continuación, dividirlo en líneas de texto individuales y analizar cada línea en cero o más tokens. A continuación, se llama al bucle **for** con el valor de variable iterativa establecido en el token. De forma predeterminada, **/f** pasa el primer token separado en blanco de cada línea de cada archivo. Se omiten las líneas en blanco.

    Las sintaxis son:  
    ```
    for /f [<ParsingKeywords>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    for /f [<ParsingKeywords>] {%%|%}<Variable> in (<LiteralString>) do <Command> [<CommandLineOptions>]
    for /f [<ParsingKeywords>] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
    ```  
    El argumento *set* especifica uno o más nombres de archivo. Cada archivo se abre, se lee y se procesa antes de pasar al siguiente archivo del *conjunto*. Para invalidar el comportamiento de análisis predeterminado, especifique *ParsingKeywords*. Se trata de una cadena entre comillas que contiene una o más palabras clave para especificar diferentes opciones de análisis.

    Si usa la opción **usebackq** , use una de las siguientes sintaxis:  
    ```
    for /f [usebackq <ParsingKeywords>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    for /f [usebackq <ParsingKeywords>] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
    for /f [usebackq <ParsingKeywords>] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
    ```  
    En la tabla siguiente se enumeran las palabras clave de análisis que puede usar para *ParsingKeywords*.  

    |      Palabra clave      |                                                                                                                                                                                                          Descripción                                                                                                                                                                                                          |
    |-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |     EOL =\<c>      |                                                                                                                                                                                   Especifica un carácter de final de línea (un solo carácter).                                                                                                                                                                                    |
    |     Skip =\<N>     |                                                                                                                                                                              Especifica el número de líneas que se van a omitir al principio del archivo.                                                                                                                                                                              |
    |   delims =\<XXX>   |                                                                                                                                                                     Especifica un conjunto de delimitadores. Esto reemplaza el conjunto de delimitadores predeterminado de espacio y tabulación.                                                                                                                                                                      |
    | tokens =\<X, Y, M – N> | Especifica los tokens de cada línea que se van a pasar al bucle **for** para cada iteración. Como resultado, se asignan los nombres de variable adicionales. *M*–*N* especifica un intervalo, desde el *m*a través de los tokens *N*. Si el último carácter de **tokens =** cadena es un asterisco (**&#42;**), se asigna una variable adicional y recibe el texto restante en la línea después del último token que se analiza. |
    |     usebackq      |                                                                                             Especifica que: ejecute una cadena entre comillas como un comando, use una cadena con comillas simples como una cadena literal, o bien, para los nombres de archivo largos que contengan * \<espacios\>*, permita que los nombres de archivo estén establecidos, entre comillas dobles.                                                                                              |


  - Sustitución de variables

    En la tabla siguiente se muestra la sintaxis opcional (para cualquier variable **I**).  

    |Variable con modificador|Descripción|
    |----------------------|-----------|
    |% ~ I|Expande **% I** que quita las comillas adyacentes ().|
    |% ~ fI|Expande **% I** a un nombre de ruta de acceso completo.|
    |% ~ dI|Expande **% I** solo a una letra de unidad.|
    |% ~ pI|Expande **% I** a solo una ruta de acceso.|
    |% ~ nI|Expande **% I** a un nombre de archivo únicamente.|
    |% ~ xI|Expande **% I** a una extensión de nombre de archivo únicamente.|
    |% ~ sI|Expande la ruta de acceso para que solo contenga nombres cortos.|
    |% ~ aI|Expande **% I** a los atributos de archivo del archivo.|
    |% ~ tI|Expande **% I** hasta la fecha y hora del archivo.|
    |% ~ zI|Expande **% I** hasta el tamaño del archivo.|
    |% ~ $PATH: I|Busca en los directorios que aparecen en la variable de entorno PATH y expande **% I** hasta el nombre completo del primer directorio encontrado. Si no se define el nombre de la variable de entorno o la búsqueda no encuentra el archivo, este modificador se expande a la cadena vacía.|

    En la tabla siguiente se muestran las combinaciones de modificadores que puede usar para obtener resultados compuestos.  

    |Variable con modificadores combinados|Descripción|
    |--------------------------------|-----------|
    |% ~ dpI|Expande **% I** solo a una letra de unidad y ruta de acceso.|
    |% ~ nxI|Expande **% I** a un nombre de archivo y a una extensión únicamente.|
    |% ~ fsI|Expande **% I** a un nombre de ruta de acceso completa solo con nombres cortos.|
    |% ~ DP $ ruta de acceso: I|Busca en los directorios que aparecen en la variable de entorno PATH de **% I** y expande hasta la letra de unidad y la ruta de acceso del primero encontrado.|
    |% ~ ftzaI|Expande **% I** a una línea de salida que es como **dir**.|

    En los ejemplos anteriores, puede reemplazar **% I** y path por otros valores válidos. Un válido **para** el nombre de variable finaliza **%~** la sintaxis.

    Mediante el uso de nombres de variables en mayúsculas como **% I**, puede hacer que el código sea más legible y evitar la confusión con los modificadores, que no distinguen mayúsculas de minúsculas.
- Analizar una cadena

  Puede usar la lógica **de análisis de for/f** en una cadena inmediata ajustando * \<\> LiteralString* en: comillas dobles (*sin* usebackq) o entre comillas simples (*con* usebackq) (por ejemplo, (String) o (' String '). LiteralString se trata como una sola línea de entrada de un archivo. * \<\> * Al analizar * \<LiteralString\> * entre comillas dobles, los símbolos de comando ( ** \\ \& \| \> \< \^ **como) se tratan como caracteres ordinarios.
- Resultados de análisis

  Puede usar el comando **for/f** para analizar la salida de un comando colocando un * \<comando\> * entre comillas entre paréntesis. Se trata como una línea de comandos, que se pasa a un archivo CMD. exe secundario. La salida se captura en la memoria y se analiza como si se tratase de un archivo.

## <a name="examples"></a>Ejemplos

Para utilizar **para** en un archivo por lotes, use la siguiente sintaxis:
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Para mostrar el contenido de todos los archivos del directorio actual que tengan la extensión. doc o. txt mediante la variable reemplazable **% f**, escriba:
```
for %f in (*.doc *.txt) do type %f 
```
En el ejemplo anterior, cada archivo que tiene la extensión. doc o. txt en el directorio actual se sustituye por la variable **% f** hasta que se muestra el contenido de cada archivo. Para usar este comando en un archivo por lotes, reemplace todas las apariciones de **% f** por **%% f**. De lo contrario, se omite la variable y se muestra un mensaje de error.

Para analizar un archivo, omitiendo las líneas comentadas, escriba:
```
for /f eol=; tokens=2,3* delims=, %i in (myfile.txt) do @echo %i %j %k
```
Este comando analiza cada línea en el archivo. txt. Omite las líneas que comienzan por un punto y coma y pasa el segundo y el tercer token de cada línea al cuerpo **for** (los tokens se delimitan mediante comas o espacios). El cuerpo de la instrucción **for** hace referencia a **% i** para obtener el segundo token, **% j** para obtener el tercer token y **% k** para obtener todos los tokens restantes. Si los nombres de archivo que proporciona contienen espacios, utilice comillas alrededor del texto (por ejemplo, nombre de archivo). Para usar comillas, debe utilizar **usebackq**. De lo contrario, las comillas se interpretan como la definición de una cadena literal que se va a analizar.

**% i** se declaró explícitamente en la instrucción **for** . **% j** y **% k** se declaran implícitamente mediante **tokens =**. Puede usar **tokens =** para especificar hasta 26 tokens, siempre que no se intente declarar una variable superior a la letra Z o z.

Para enumerar los nombres de las variables de entorno en el entorno actual. Para analizar la salida de un comando colocando el *conjunto* entre paréntesis, escriba:
```
for /f usebackq delims== %i in ('set') do @echo %i 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
