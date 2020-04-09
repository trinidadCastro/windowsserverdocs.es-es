---
title: typeperf
description: Windows Commands topic for Typeperf, que escribe datos de rendimiento en la ventana comandos o en un archivo de registro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac5f7def37939a472eb8f47cf65edf184a2fe2fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832368"
---
# <a name="typeperf"></a>typeperf

El comando **Typeperf** escribe los datos de rendimiento en la ventana comandos o en un archivo de registro. Para detener **Typeperf**, presione Ctrl + C.

Para obtener ejemplos de cómo usar **Typeperf**, vea [ejemplos](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintaxis

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<contador [...]] >|Especifica los contadores de rendimiento que se van a supervisar.|

> [!NOTE]
> **\<contador >** es el nombre completo de un contador de rendimiento en *\\\\formato \Counter Computer\Object (instancia)* , como **\\\\Server1\Processor (0)\% tiempo de usuario**.

## <a name="options"></a>Opciones

|                   Opción                   |                                                         Descripción                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               Muestra la ayuda contextual.                                               |
| -f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL > |                                    Especifica el formato del archivo de salida. El valor predeterminado es CSV.                                     |
|              -CF \<nombre de archivo >               |              Especifica un archivo que contiene una lista de contadores de rendimiento que se van a supervisar, con un contador por línea.               |
|             -Si < [[HH:] mm:] SS >             |                                  Especifica el intervalo de ejemplo. El valor predeterminado es un segundo.                                   |
|               -o \<nombre de archivo >               |     Especifica la ruta de acceso del archivo de salida o la base de datos SQL. El valor predeterminado es STDOUT (se escribe en la ventana de comandos).      |
|                -q [objeto]                 | Mostrar una lista de contadores instalados (sin instancias). Para enumerar los contadores de un objeto, incluya el nombre del objeto. EJEMPLO de \*de \*de \* |
|                -QX [objeto]                |        Mostrar una lista de contadores instalados con instancias. Para enumerar los contadores de un objeto, incluya el nombre del objeto.        |
|               -SC \<ejemplos >               |             Especifica el número de muestras que se van a recopilar. El valor predeterminado es recopilar datos hasta que se presiona CTRL + C.              |
|            -config \<nombre de archivo >             |                                    Especifica un archivo de configuración que contiene opciones de comando.                                     |
|            -s \<computer_name >             |                   Especifica un equipo remoto que se va a supervisar si no se especifica ningún equipo en la ruta de acceso del contador.                    |
|                     -y                     |                                        Responda sí a todas las preguntas sin preguntar.                                        |

## <a name="examples"></a><a name=BKMK_EXAMPLES></a>Example

- En el ejemplo siguiente se escriben los valores del contador de rendimiento del equipo local **\\procesador de \\(_Total)\% tiempo de procesador** a la ventana de comandos en un intervalo de ejemplo predeterminado de 1 segundo hasta que se presiona Ctrl + C.  
  ```
  typeperf \Processor(_Total)\% Processor Time
  ```  
- En el ejemplo siguiente se escriben los valores de la lista de contadores de file **Counters. txt** en el archivo delimitado por tabulaciones **dominio2. TSV** en un intervalo de ejemplo de 5 segundos hasta que se han recopilado 50 ejemplos.  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- En el ejemplo siguiente se consultan los contadores instalados con instancias para el objeto de contador **DiscoFísico** y se escribe la lista resultante en el archivo **Counters. txt**.  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
