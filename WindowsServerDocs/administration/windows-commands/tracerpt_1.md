---
title: tracerpt
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1c8e85acc59383d0a027c8d5ec51e68942a76b85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385676"
---
# <a name="tracerpt"></a>tracerpt



El comando **tracerpt** se puede usar para analizar los registros de seguimiento de eventos, los archivos de registro generados por el monitor de rendimiento y los proveedores de seguimiento de eventos en tiempo real. Genera archivos de volcado de memoria, archivos de informe y esquemas de informe.

Para obtener ejemplos de cómo usar **tracerpt**, vea [ejemplos](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintaxis

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Opciones

|              Marca de opción               |                                                                    Descripción                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         Muestra la ayuda contextual.                                                          |
|          -config \<nombre de archivo >           |                                                 Carga un archivo de configuración que contiene opciones de comando.                                                  |
|                   -y                   |                                                  Responda sí a todas las preguntas sin preguntar.                                                   |
|            -f \<XML\|HTML >             |                                                                  Formato del informe.                                                                   |
|         -de \<CSV\|archivo\|XML de EVTX >          |                                                         El formato de volcado, el valor predeterminado es XML.                                                          |
|            -DF \<nombreDeArchivo >             |                                            Cree un archivo de esquema de recuento o informes específico de Microsoft.                                            |
|            -int \<filename >            |                                            Vuelca la estructura de eventos interpretada en el archivo especificado.                                            |
|                  -RTS                  |                        Notificar marca de tiempo sin formato en el encabezado de seguimiento de eventos. Solo se puede usar con-o, no con informe o-Resumen.                         |
|            -TMF \<nombrearchivo >            |                                                  Especifique un archivo de definición de formato de mensaje de seguimiento.                                                  |
|              -valor \<de TP >              |                            Especifique la ruta de búsqueda de archivos TMF. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;).                            |
|              -i \<valor >               | Especifique la ruta de acceso de la imagen del proveedor. El archivo PDB correspondiente se ubicará en el servidor de símbolos. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;). |
|             -valor \<de PDB >              |                             Especifique la ruta de acceso del servidor de símbolos. Se pueden usar varias rutas de acceso, separadas por un punto y coma (;).                             |
|                  -GMT                  |                                              Convierte las marcas de tiempo de carga de WPP en la hora del meridiano de Greenwich.                                               |
|              -valor \<RL >              |                                               Defina el nivel de informe del sistema de 1 a 5. El valor predeterminado es 1.                                               |
|          -Summary [nombre de archivo]           |                                  Generar un archivo de texto de informe de resumen. FILENAME si no se especifica es Summary. txt.                                   |
|             -o [nombre de archivo]              |                                      Generar un archivo de salida de texto. FILENAME si no se especifica, el archivo Dumpfile. Xml.                                      |
|           -Report [nombre de archivo]           |                                  Generar un archivo de informe de salida de texto. FILENAME si no se especifica, es Workload. Xml.                                   |
|                  -LR                   |                        Especifique "menos restrictivo". Esto utiliza los mejores esfuerzos para los eventos que no coinciden con el esquema de eventos.                         |
|           -Export [nombre de archivo]           |                                  Generar un archivo de exportación del esquema de eventos. FILENAME si no se especifica es Schema. Man.                                   |
|       [-l] \<value [valor [...]] >        |                                                   Especifique el archivo de registro de seguimiento de eventos que se va a procesar.                                                    |
| -RT \<session_name [session_name [...]] > |                                                Especifique orígenes de datos de sesión de seguimiento de eventos en tiempo real.                                                |

## <a name="BKMK_EXAMPLES"></a>Example

- En este ejemplo se crea un informe basado en los dos registros de eventos **logfile1. ETL** y **logfile2. ETL** y se crea el archivo de volcado de memoria **LOGDUMP. XML** en formato XML.  
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```  
- En este ejemplo se crea un informe basado en el registro de eventos **logfile. ETL**, se crea el archivo de volcado de memoria **logdmp. XML** en formato XML, se usan los mejores esfuerzos para identificar eventos que no están en el esquema, se genera un archivo de informe de Resumen **logdump. txt**y se genera el archivo de informe **logrpt. XML**.  
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```  
- En este ejemplo se usan los dos registros de eventos **logfile1. ETL** y **logfile2. ETL** para generar un archivo de volcado de memoria y un archivo de informe con los nombres de archivo predeterminados.  
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```  
- En este ejemplo se usa el registro de eventos **logfile. ETL** y el archivo de registro de rendimiento **. BLG** para generar el archivo de informe **logrpt. XML** y el archivo de esquema XML específico de Microsoft **Schema. XML**.  
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```  
- En este ejemplo se lee la sesión de seguimiento de eventos en tiempo real "NT kernel Logger" y se genera el archivo **logfile. csv** en formato CSV.  
  ```
  tracerpt -rt "NT Kernel Logger" -o logfile.csv -of CSV
  ```
