---
title: tracerpt
description: Artículo de referencia de tracerpt, que analiza los registros de seguimiento de eventos, los archivos de registro generados por el monitor de rendimiento y los proveedores de seguimiento de eventos en tiempo real.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7060932f0b7eb996d0f0934e6945665c0c91e916
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935582"
---
# <a name="tracerpt"></a>tracerpt

El comando **tracerpt** se puede usar para analizar los registros de seguimiento de eventos, los archivos de registro generados por el monitor de rendimiento y los proveedores de seguimiento de eventos en tiempo real. Genera archivos de volcado de memoria, archivos de informe y esquemas de informe.

## <a name="syntax"></a>Sintaxis

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Opciones

|              Marca de opción               |                                                                    Descripción                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         Muestra la ayuda contextual.                                                          |
|          -config\<filename>           |                                                 Carga un archivo de configuración que contiene opciones de comando.                                                  |
|                   -y                   |                                                  Responda sí a todas las preguntas sin preguntar.                                                   |
|            -f\<XML\|HTML>             |                                                                  Formato del informe.                                                                   |
|         -de\<CSV\|EVTX\|XML>          |                                                         El formato de volcado, el valor predeterminado es XML.                                                          |
|            -DF\<filename>             |                                            Cree un archivo de esquema de recuento o informes específico de Microsoft.                                            |
|            -int\<filename>            |                                            Vuelca la estructura de eventos interpretada en el archivo especificado.                                            |
|                  -RTS                  |                        Notificar marca de tiempo sin formato en el encabezado de seguimiento de eventos. Solo se puede usar con-o, no con informe o-Resumen.                         |
|            -tmf\<filename>            |                                                  Especifique un archivo de definición de formato de mensaje de seguimiento.                                                  |
|              -TP\<value>              |                            Especifique la ruta de búsqueda de archivos TMF. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;).                            |
|              -i\<value>               | Especifique la ruta de acceso de la imagen del proveedor. El archivo PDB correspondiente se ubicará en el servidor de símbolos. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;). |
|             -PDB\<value>              |                             Especifique la ruta de acceso del servidor de símbolos. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;).                             |
|                  -GMT                  |                                              Convierte las marcas de tiempo de carga de WPP en la hora del meridiano de Greenwich.                                               |
|              -rl \<value>              |                                               Defina el nivel de informe del sistema de 1 a 5. El valor predeterminado es 1.                                               |
|          -Summary [nombre de archivo]           |                                  Generar un archivo de texto de informe de resumen. FILENAME si no se especifica, se summary.txt.                                   |
|             -o [nombre de archivo]              |                                      Generar un archivo de salida de texto. FILENAME si no se especifica, se dumpfile.xml.                                      |
|           -Report [nombre de archivo]           |                                  Generar un archivo de informe de salida de texto. FILENAME si no se especifica, se workload.xml.                                   |
|                  -LR                   |                        Especifique menos restrictivo. Esto utiliza los mejores esfuerzos para los eventos que no coinciden con el esquema de eventos.                         |
|           -Export [nombre de archivo]           |                                  Generar un archivo de exportación del esquema de eventos. FILENAME si no se especifica es Schema. Man.                                   |
|       [-l]\<value [value […]]>        |                                                   Especifique el archivo de registro de seguimiento de eventos que se va a procesar.                                                    |
| -RT\<session_name [session_name […]]> |                                                Especifique orígenes de datos de sesión de seguimiento de eventos en tiempo real.                                                |

## <a name="examples"></a>Ejemplos

- En este ejemplo se crea un informe basado en los dos registros de eventos **logfile1. ETL** y **logfile2. ETL** y se crea el archivo de volcado de memoria **logdump.xml** en formato XML.
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```
- En este ejemplo se crea un informe basado en el registro de eventos **logfile. ETL**, se crea el archivo de volcado de memoria **logdmp.xml** en formato XML, se usan los mejores esfuerzos para identificar eventos que no están en el esquema, se genera un archivo de informe de Resumen **logdump.txt**y se genera el archivo de informe **logrpt.xml**.
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```
- En este ejemplo se usan los dos registros de eventos **logfile1. ETL** y **logfile2. ETL** para generar un archivo de volcado de memoria y un archivo de informe con los nombres de archivo predeterminados.
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```
- En este ejemplo se usa el registro de eventos **logfile. ETL** y el archivo de registro de rendimiento **. BLG** para generar el archivo de informe **logrpt.xml** y el archivo de esquema XML específico de Microsoft **schema.xml**.
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```
- En este ejemplo se lee el registrador de kernel de NT de la sesión de seguimiento de eventos en tiempo real y se genera el archivo de volcado de memoria **logfile.csv** en formato CSV.
  ```
  tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
  ```
