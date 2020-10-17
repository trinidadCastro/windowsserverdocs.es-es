---
title: tracerpt
description: Artículo de referencia para el comando tracerpt, que analiza los registros de seguimiento de eventos, los archivos de registro generados por el monitor de rendimiento y los proveedores de seguimiento de eventos en tiempo real.
ms.topic: reference
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1e468dab3c99219560047668f9f1bd001e8e451e
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156757"
---
# <a name="tracerpt"></a>tracerpt

El comando **tracerpt** analiza los registros de seguimiento de eventos, los archivos de registro generados por el monitor de rendimiento y los proveedores de seguimiento de eventos en tiempo real. También genera archivos de volcado de memoria, archivos de informe y esquemas de informe.

## <a name="syntax"></a>Sintaxis

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
|--|--|
| -config `<filename>` | Especifica el archivo de configuración que se va a cargar, que incluye las opciones del comando. |
| -y | Especifica la respuesta de **sí** a todas las preguntas sin preguntar. |
| -f `<XML | HTML>` | Especifica el formato del archivo de informe. |
| -de `<CSV | EVTX | XML>` | Especifica el formato del archivo de volcado. El valor predeterminado es **XML*. |
| -DF `<filename>` | Especifica que se cree un archivo de esquema de recuento o informes específico de Microsoft. |
| -int `<filename>` | Especifica el volcado de la estructura de eventos interpretada en el archivo especificado. |
| -RTS | Especifica que se va a agregar la marca de tiempo sin formato del informe en el encabezado de seguimiento de eventos. Solo se puede usar con **-o**. No es compatible con **-Report** o **-Summary**. |
| -tmf `<filename>` | Especifica el archivo de definición de formato del mensaje de seguimiento que se va a utilizar. |
| -TP `<value>` | Especifica la ruta de búsqueda del archivo TMF. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;). |
| -i `<value>` | Especifica la ruta de acceso de la imagen del proveedor. El archivo PDB correspondiente se ubicará en el servidor de símbolos. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;). |
| -PDB `<value>` | Especifica la ruta de acceso del servidor de símbolos. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;). |
| -GMT | Especifica que se conviertan las marcas de tiempo de la carga WPP en la hora del meridiano de Greenwich. |
| -rl `<value>` | Especifica el nivel de informe del sistema de 1 a 5. El valor predeterminado es *1*. |
| -Summary [nombre de archivo] | Especifica que se va a crear un archivo de texto de informe de resumen. Si no se especifica, el nombre de archivo se *summary.txt*. |
| -o [nombre de archivo] | Especifica que se va a crear un archivo de salida de texto. Si no se especifica, el nombre de archivo se *dumpfile.xml*. |
| -Report [nombre de archivo] | Especifica la creación de un archivo de informe de salida de texto. Si no se especifica, el nombre de archivo se *workload.xml*. |
| -LR | Especifica que sea menos restrictivo. Esto utiliza los mejores esfuerzos para los eventos que no coinciden con el esquema de eventos. |
| -Export [nombre de archivo] | Especifica que se va a crear un archivo de exportación del esquema de eventos. Si no se especifica, el nombre de archivo es *Schema. Man*. |
| [-l] `<value [value […]]>` | Especifica el archivo de registro de seguimiento de eventos que se va a procesar. |
| -RT `<session_name [session_name […]]>` | Especifica los orígenes de datos de la sesión de seguimiento de eventos en tiempo real. |
| -? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para crear un informe basado en los dos registros de eventos *logfile1. ETL* y *logfile2. ETL*, y para crear el archivo de volcado de memoria *logdump.xml* en formato *XML* , escriba:

```
tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
```

Para crear un informe basado en el registro de eventos *logfile. ETL*, para crear el archivo de volcado de memoria *logdmp.xml* en formato XML, para usar los mejores esfuerzos para identificar eventos que no están en el esquema, y para generar un archivo de informe de Resumen *logdump.txt* y un archivo de informe, *logrpt.xml*, escriba:

```
tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
```

Para usar los dos registros de eventos *logfile1. ETL* y *logfile2. ETL* para generar un archivo de volcado de memoria y para notificar el archivo con los nombres de archivo predeterminados, escriba:

```
tracerpt logfile1.etl logfile2.etl -o -report
```

Para usar el registro de eventos *logfile. ETL* y el archivo de registro de rendimiento *. BLG* para generar el archivo de informe *logrpt.xml* y el archivo de esquema XML específico de Microsoft *schema.xml*, escriba:

```
tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
```

Para leer el registrador de kernel de NT de la sesión de seguimiento de eventos en tiempo real y generar el archivo de volcado de memoria *logfile.csv* en formato *CSV* , escriba:

```
tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
