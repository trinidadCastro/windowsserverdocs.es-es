---
title: relog
description: Aprenda a extraer información de contadores de rendimiento de los archivos de registro de coutner de rendimiento.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: daedd85f1557c191a690e7eb750559cfd268d3a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371617"
---
# <a name="relog"></a>relog

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Extrae los contadores de rendimiento de los registros de contadores de rendimiento en otros formatos, como text-TSV (para texto delimitado por tabuladores), Text-CSV (para texto delimitado por comas), Binary-BIN o SQL.   

## <a name="syntax"></a>Sintaxis  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Parámetros  

|                                         Parámetro                                          |                                                                                                                                                                  Descripción                                                                                                                                                                   |
|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                *Nombre de archivo* [*nombrearchivo...* ]                                 |                                                                                                                      Especifica el directorio de un registro de contador de rendimiento existente. Puede especificar varios archivos de entrada.                                                                                                                      |
|                                             -a                                             |                                                                                                          Anexa el archivo de salida en lugar de sobrescribirlo. Esta opción no se aplica al formato SQL, donde el valor predeterminado siempre es anexar.                                                                                                           |
|                                   -c *ruta de acceso* [*ruta de acceso...* ]                                   |                                                       Especifica la ruta de acceso del contador de rendimiento que se va a registrar. Para especificar varias rutas de acceso de contador, sepárelas con un espacio y encierre las rutas de acceso de contador entre comillas (por ejemplo, **"** <em>Counterpath1</em> <em>Counterpath2</em> **"** )                                                       |
|                                       -CF *nombre de archivo*                                       |                                            Especifica la ruta de acceso del archivo de texto que enumera los contadores de rendimiento que se van a incluir en un archivo de relog. Use esta opción para mostrar las rutas de acceso de contador en un archivo de entrada, una por línea. La configuración predeterminada es que se reinicien todos los contadores del archivo de registro original.                                            |
|                                  -f {bin\| CSV\|TSV\|SQL}                                  |                                       Especifica la ruta de acceso del formato del archivo de salida. El formato predeterminado es **bin**. En el caso de una base de datos SQL, el archivo de salida especifica el *DSN. CounterLog*. Puede especificar la ubicación de la base de datos mediante el administrador de ODBC para configurar el DSN (nombre del sistema de la base de datos).                                        |
|                                         *valor* -t                                         |                                                                                                           Especifica los intervalos de muestra en los registros "*N*". Incluye cada n punto de datos en el archivo relog. El valor predeterminado es cada punto de datos.                                                                                                           |
| -o {*OutputFile* \| *"SQL: DSN! Counter_Log*}, donde DSN es un DSN de ODMC definido en el sistema. |                                                   Especifica la ruta de acceso del archivo de salida o la base de datos SQL donde se escribirán los contadores. <br>Nota: para las versiones de 64 bits y 32 bits de relog. exe, debe definir un DSN en el origen de datos ODBC (64 bits y 32 bits, respectivamente).                                                   |
|                          -b \<*M*/*D*/*yyyy*> [[*HH*:]*mm*:]*SS*                           |                                                                          Especifica la hora de inicio para copiar el primer registro del archivo de entrada. la fecha y la hora deben tener el formato exacto <em>M</em> **/** <em>D</em> **/** <em>YYYYHH</em> **:** <em>mm</em> **:** <em>SS</em>.                                                                          |
|                          -e \<*M*/*D*/*yyyy*> [[*HH*:]*mm*:]*SS*                           |                                                                           Especifica la hora de finalización para copiar el último registro del archivo de entrada. la fecha y la hora deben tener el formato exacto <em>M</em> **/** <em>D</em> **/** <em>YYYYHH</em> **:** <em>mm</em> **:** <em>SS</em>.                                                                            |
|                                -config {*FileName* \| *i*}                                 | Especifica la ruta de acceso del archivo de configuración que contiene los parámetros de la línea de comandos. Use *-i* en el archivo de configuración como un marcador de posición para una lista de archivos de entrada que se pueden colocar en la línea de comandos. En la línea de comandos, sin embargo, no deberá usar *i*. También puede usar caracteres comodín, como \*. BLG, para especificar muchos nombres de archivo de entrada. |
|                                             -q                                             |                                                                                                                          Muestra los contadores de rendimiento y los intervalos de tiempo de los archivos de registro especificados en el archivo de entrada.                                                                                                                           |
|                                             -y                                             |                                                                                                                                            Omite preguntar al responder "sí" a todas las preguntas.                                                                                                                                             |
|                                             /?                                             |                                                                                                                                                      Muestra la ayuda en el símbolo del sistema.                                                                                                                                                      |

## <a name="remarks"></a>Observaciones  
Formato de la ruta de acceso del contador:  
- El formato general de las rutas de acceso de contador es el siguiente: [\\\<equipo >] \\\<objeto > [\<elemento primario >\\< instancia # Índice >] \\\<contador >] donde los componentes primarios, de instancia, de índice y de contador del formato pueden contener un nombre válido o un carácter comodín. Los componentes equipo, primario, instancia e índice no son necesarios para todos los contadores.  
- Se determinan las rutas de acceso del contador que se van a usar en función del propio contador. Por ejemplo, el objeto LogicalDisk tiene una instancia <Index>, por lo que debe proporcionar el < #index > o un carácter comodín. Por lo tanto, puede usar el siguiente formato: **\LogicalDisk (\*/\*#\*)** \\\\*  
- En comparación, el objeto de proceso no requiere una instancia de \<Índice >. Por lo tanto, puede usar el siguiente formato: **\Process (\*) \ID proceso**  
- Si se especifica un carácter comodín en el nombre principal, se devolverán todas las instancias del objeto especificado que coincidan con la instancia y los campos de contador especificados.  
- Si se especifica un carácter comodín en el nombre de instancia, se devolverán todas las instancias del objeto y el objeto primario especificados si todos los nombres de instancia correspondientes al índice especificado coinciden con el carácter comodín.  
- Si se especifica un carácter comodín en el nombre del contador, se devuelven todos los contadores del objeto especificado.  
- No se admiten las coincidencias de cadena de ruta de contador parcial (por ejemplo, Pro *).  

Archivos de contador:  
-   Los archivos de contador son archivos de texto que enumeran uno o varios de los contadores de rendimiento del registro existente. Copie el nombre completo del contador del registro o el resultado **/q** en \<equipo >\\\<objeto >\\\<>\\\<>. muestra una ruta de acceso de contador en cada línea.  

Copiando contadores:  
-   Cuando se ejecuta, **relog** copia los contadores especificados de cada registro del archivo de entrada, lo que convierte el formato si es necesario. Se permiten las rutas de acceso comodín en el archivo de contador.  
Guardar subconjuntos de archivos de entrada:  
-   Use el parámetro **/t** para especificar que los archivos de entrada se inserten en los archivos de salida a intervalos de cada registro de <n>. De forma predeterminada, los datos se reinician desde cada registro.  
Usar los parámetros **/b** y **/e** con los archivos de registro  
-   Puede especificar que los registros de salida incluyan registros desde antes de que el tiempo de inicio (es decir, **/b**) proporcionen datos para los contadores que requieran valores de cálculo del valor con formato. El archivo de salida tendrá los últimos registros de los archivos de entrada con marcas de tiempo menores que el parámetro **/e** (es decir, hora de finalización).  
Usar la opción **/config** :  
-   El contenido del archivo de configuración que se usa con la opción **/config** debe tener el formato siguiente:  
    -   \<CommandOption >\\\<valor >, donde \<CommandOption > es una opción de línea de comandos y \<valor > especifica su valor.

Para obtener más información acerca de cómo incorporar **relog** en los scripts de instrumental de administración de Windows (WMI), vea el tema sobre el scripting de WMI en el [sitio web de los kits de recursos de Microsoft Windows](https://go.microsoft.com/fwlink/?LinkId=4665).  

## <a name="BKMK_Examples"></a>Example  
Para volver a muestrear los registros de seguimiento existentes a intervalos fijos de 30, enumere las rutas de acceso de contador, los archivos de salida y los formatos:  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
Para volver a muestrear los registros de seguimiento existentes a intervalos fijos de 30, enumere las rutas de acceso del contador y el archivo de salida:  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
Para volver a muestrear los registros de seguimiento existentes en una base de datos, use:
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
