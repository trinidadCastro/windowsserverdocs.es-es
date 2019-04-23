---
title: relog
description: Obtenga información sobre cómo extraer información del contador de rendimiento de los archivos de registro de rendimiento coutner.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6804c25af04907edc8180b6a37be7efcc470f259
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869366"
---
# <a name="relog"></a>relog

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Extrae los contadores de rendimiento de los registros de contador de rendimiento en otros formatos, como texto TSV (por texto delimitado por tabulaciones), texto CSV (por texto delimitado por comas), BIN binario o SQL.   

## <a name="syntax"></a>Sintaxis  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|
|--|--|
|*Nombre de archivo* [*nombre de archivo...* ]|Especifica la ruta de acceso de un registro de contador de rendimiento existente. Puede especificar varios archivos de entrada.|
|-a |Anexa el archivo de salida en lugar de sobrescribir. Esta opción no se aplica al formato SQL donde el valor predeterminado es siempre que se anexará.  |
|-c *ruta* [*ruta de acceso...* ]|Especifica la ruta de acceso de contador de rendimiento para iniciar sesión. Para especificar varias rutas de acceso de contador, sepárelas con un espacio y encierre las rutas de acceso de contador entre comillas (por ejemplo, **"*** RutadeAccesoDeContador1* * RutadeAccesoDeContador2 ***"**)|  
|-cf *FileName*|Especifica la ruta de acceso del archivo de texto que se enumera los contadores de rendimiento que se incluirán en un archivo de volver a registrar. Use esta opción para las rutas de acceso del contador de lista en un archivo de entrada, uno por línea. Valor predeterminado es que volver a registrar todos los contadores en el archivo de registro original.|  
|-f {bin\| csv\|tsv\|SQL}|Especifica la ruta de acceso del formato de archivo de salida. El formato predeterminado es **bin**. Para una base de datos SQL, el archivo de salida especifica el *DSN! CounterLog*. Puede especificar la ubicación de la base de datos mediante el Administrador de ODBC para configurar el DSN (nombre de base de datos del sistema).  |
|-t *valor*|Especifica intervalos de muestra en "*N*" registros. Incluye cada n puntos de datos en el archivo de volver a registrar. Valor predeterminado es cada punto de datos.|  
|-o {*OutputFile* \| *"SQL:DSN! RegistroContadorDSN*} donde DSN es un DSN ODMC definidos en el sistema.|Especifica la ruta de acceso del archivo de salida o la base de datos SQL donde se escribirán los contadores. <br>Nota: Para las versiones de 64 bits y 32 bits de Relog.exe, deberá definir un DSN en el origen de datos ODBC (64 bits y 32 bits, respectivamente)|
|-b \< *M*/*d.*/*aaaa*> [[*HH*:]*MM*:]*SS*|Especifica la hora para copiar el primer registro desde el archivo de entrada de inicio. fecha y hora deben estar en el formato exacto *M***/*** d.***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|-e \< *M*/*d.*/*aaaa*> [[*HH*:]*MM*:]*SS* |Especifica la hora de finalización para copiar el último registro del archivo de entrada. fecha y hora deben estar en el formato exacto *M***/*** d.***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|-config {*FileName* \| *i*}|Especifica la ruta de acceso del archivo de configuración que contiene los parámetros de línea de comandos. Use *-i* en el archivo de configuración como un marcador de posición para obtener una lista de archivos de entrada que se pueden colocar en la línea de comandos. En la línea de comandos, sin embargo, no deberá usar *i*. También puede usar caracteres comodín, como *.blg para especificar varios nombres de archivo de entrada.|  
|-q|Muestra los contadores de rendimiento e intervalos de tiempo de archivos de registro especificados en el archivo de entrada.|  
|-y|Mensajes de confirmación contestando "Sí" a todas las preguntas de omisiones.|  
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios  
Formato de ruta de acceso de contador:  
-   El formato general de las rutas de acceso de contador es como sigue: [\\\<equipo >] \\ \<objeto > [\<primario >\\< #Index >] \\ \< Contador >] donde el primario, instancia, índice y componentes del formato del contador pueden contener un nombre válido o un carácter comodín. El equipo, principal, instancia y componentes del índice no son necesarios para todos los contadores.  
-   Determinar las rutas de acceso de contador utilizar basándose en el propio contador. Por ejemplo, el objeto disco lógico tiene una instancia <Index>, por lo que debe proporcionar < #index > o un carácter comodín. Por lo tanto, podría utilizar el siguiente formato: **\LogicalDisk (\*/\*#\*)\\\***  
-   En comparación, el objeto de proceso no requiere una instancia \<índice >. Por lo tanto, podría utilizar el siguiente formato: **\process(waiishost)\Private (\*) \ID proceso**  
-   Si se especifica un carácter comodín en el nombre del elemento primario, se devolverán todas las instancias del objeto especificado que coinciden con los campos de contador y la instancia especificada.  
-   Si se especifica un carácter comodín en el nombre de instancia, se devolverán todas las instancias del objeto especificado y del objeto primario si todos los nombres de instancia correspondiente al índice especificado coincide con el carácter comodín.  
-   Si se especifica un carácter comodín en el nombre del contador, se devuelven todos los contadores del objeto especificado.  
-   No se admiten las coincidencias de la cadena de ruta de acceso de contador parcial (por ejemplo, pro *).  

Archivos de contador:  
-   Contador son archivos de texto que enumeran uno o varios de los contadores de rendimiento en el registro existente. Copie el nombre de contador completo del registro o el **/q** de salida en \<equipo >\\\<objeto >\\\<instancia >\\ \< Contador > formato. enumerar una ruta de acceso de contador en cada línea.  

Copiar contadores:  
-   Cuando se ejecuta, **volver a registrar** copia los contadores especificados de cada registro en el archivo de entrada, convertir el formato si es necesario. Se permiten rutas de acceso de comodín en el archivo de contador.  
Guardando subconjuntos del archivo de entrada:  
-   Use la **/t** a intervalos de los archivos de salida de parámetro para especificar que los archivos de entrada se insertan en cada <n>registro th. De forma predeterminada, es volver a registrar los datos de cada registro.  
Uso de **/b** y **/e** parámetros con archivos de registro  
-   Puede especificar que los registros de salida incluyen registros desde antes de la hora de inicio (es decir, **/b**) para proporcionar datos para contadores que requieren valores calculados del valor con formato. El archivo de salida tendrá los últimos registros de archivos de entrada con marcas de tiempo menor que el **/e** (es decir, hora de finalización) parámetro.  
Mediante el **/config** opción:  
-   El contenido del archivo de configuración usado con el **/config** opción debe tener el formato siguiente:  
    -   \<CommandOption >\\\<valor >, donde \<CommandOption > es una opción de línea de comandos y \<valor > especifica su valor.

Para obtener más información acerca de cómo incorporar **volver a registrar** en sus scripts de Windows Management Instrumentation (WMI), vea "Scripting WMI" en el [sitio Web del Kit de recursos de Microsoft Windows](https://go.microsoft.com/fwlink/?LinkId=4665).  

## <a name="BKMK_Examples"></a>Ejemplos  
Para volver a muestrear los registros de seguimiento existentes a intervalos fijos de 30, enumerar las rutas de acceso de contador, archivos y formatos de salida:  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
Para volver a muestrear los registros de seguimiento existentes a intervalos fijos de 30, enumerar las rutas de acceso de contador y el archivo de salida:  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
Volver a muestrear los registros de seguimiento existente en una base de datos, use:
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
<!---
-   The following is a list of the possible formats:  
    -   \<computer>\\\<Object>(\<Parent>/\<Instance#Index>)\<Counter>  
    -   \<computer>\<Object>(<Parent>/<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance#Index>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>\\<Counter>  
    -   \\<Object>(<Parent>/<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Parent>/<Instance>)<Counter>  
    -   \\<Object>(<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Instance>)\\<Counter>  
    -   \\<Object>\\<Counter>  
--->