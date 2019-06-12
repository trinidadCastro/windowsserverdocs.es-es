---
title: tracerpt
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c105fe714e30866297e4f6c3c83a670ff7966a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440973"
---
# <a name="tracerpt"></a>tracerpt



El **tracerpt** comando puede utilizarse para analizar los registros de seguimiento de eventos, los archivos de registro generados por el Monitor de rendimiento y los proveedores de seguimiento de eventos en tiempo real. Genera archivos de volcado de memoria, los archivos de informe y esquemas de informe.

Para obtener ejemplos de cómo usar **tracerpt**, consulte [ejemplos](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintaxis

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Opciones

|              Marca de la opción               |                                                                    Descripción                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         Sensible al contexto muestra la Ayuda.                                                          |
|          -config \<nombreDeArchivo >           |                                                 Cargar un archivo de configuración que contiene las opciones de comando.                                                  |
|                   -y                   |                                                  Responda Sí a todas las preguntas sin preguntar.                                                   |
|                -f \<XML                |                                                                       HTML>                                                                       |
|               -de \<CSV                |                                                                       EVTX                                                                        |
|            -df \<filename>             |                                            Crear un específico de Microsoft de recuento o informes de archivo de esquema.                                            |
|            -int \<nombreDeArchivo >            |                                            Volcar la estructura de evento interpretados en el archivo especificado.                                            |
|                  -rts                  |                        Marca de tiempo sin formato en el encabezado de seguimiento de eventos de informe. Sólo puede utilizarse con -o, no - informe o - resumen.                         |
|            -tmf \<nombreDeArchivo >            |                                                  Especifique un archivo de definición de formato de mensaje de seguimiento.                                                  |
|              -tp \<valor >              |                            Especifique la ruta de búsqueda del archivo TMF. Pueden utilizar varias rutas de acceso, separadas por punto y coma (;).                            |
|              -i \<value>               | Especifique la ruta de acceso de imagen del proveedor. El archivo PDB coincidente se ubicará en el servidor de símbolos. Pueden usar varias rutas de acceso, separadas por punto y coma (;). |
|             -pdb \<value>              |                             Especifique la ruta de acceso del servidor de símbolos. Pueden usar varias rutas de acceso, separadas por punto y coma (;).                             |
|                  -gmt                  |                                              Convertir las marcas de tiempo de carga WPP a la hora del meridiano de Greenwich.                                               |
|              -rl \<value>              |                                               Definir el nivel de informe del sistema de 1 a 5. El valor predeterminado es 1.                                               |
|          -Resumen [nombre_de_archivo]           |                                  Generar un archivo de texto del informe de resumen. Nombre de archivo si no se especifica es summary.txt.                                   |
|             -o [nombre_de_archivo]              |                                      Generar un archivo de salida de texto. Nombre de archivo si no se especifica es dumpfile.xml.                                      |
|           -informes [nombre_de_archivo]           |                                  Generar un archivo de informe de salida de texto. Nombre de archivo si no se especifica es workload.xml.                                   |
|                  -lr                   |                        Especifique "menos restrictivo". Esto usa esfuerzos para los eventos que no coinciden con el esquema de eventos.                         |
|           -Exportar [nombre_de_archivo]           |                                  Generar un archivo de exportación de esquema de eventos. Nombre de archivo si no se especifica es schema.man.                                   |
|       [-l]. \<valor [valor [...]] >        |                                                   Especifique el archivo de registro de seguimiento de eventos para procesar.                                                    |
| -rt \<session_name [session_name […]]> |                                                Especificar los orígenes de datos de sesión de seguimiento de eventos en tiempo real.                                                |

## <a name="BKMK_EXAMPLES"></a>Ejemplos

- Este ejemplo crea un informe basado en los dos registros de eventos **ArchivoReg1.ETL** y **ArchivoReg2.ETL** y crea el archivo de volcado **logdump.xml** en formato XML.  
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```  
- Este ejemplo crea un informe basado en el registro de eventos **ArchivoReg.ETL**, crea el archivo de volcado **logdmp.xml** en formato XML, usa mejores esfuerzos para identificar los eventos no está en el esquema, genera un archivo de informe de resumen **logdump.txt**y genera el archivo de informe **logrpt.xml**.  
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```  
- En este ejemplo usa los dos registros de eventos **ArchivoReg1.ETL** y **ArchivoReg2.ETL** para generar un archivo de volcado de memoria y el archivo de informe con los nombres de archivo predeterminado.  
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```  
- Este ejemplo utiliza el registro de eventos **ArchivoReg.ETL** y el registro de rendimiento **counterfile.blg** para generar el archivo de informe **logrpt.xml** y el esquema XML específico de Microsoft archivo **schema.xml**.  
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```  
- En este ejemplo lee la sesión de seguimiento de eventos en tiempo real "Registrador del Kernel de NT" y genera el archivo de volcado de memoria **logfile.csv** en formato CSV.  
  ```
  tracerpt -rt "NT Kernel Logger" -o logfile.csv -of CSV
  ```