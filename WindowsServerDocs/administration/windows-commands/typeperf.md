---
title: typeperf
description: Artículo de referencia para el comando Typeperf, que escribe datos de rendimiento en la ventana comandos o en un archivo de registro.
ms.topic: reference
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6a06d422db5f681c3e1775facc4f8d0eee422244
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156357"
---
# <a name="typeperf"></a>typeperf

El comando **Typeperf** escribe los datos de rendimiento en la ventana comandos o en un archivo de registro. Para detener **Typeperf**, presione Ctrl + C.

## <a name="syntax"></a>Sintaxis

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<counter [counter […]]>` | Especifica los contadores de rendimiento que se van a supervisar. El `<counter>` parámetro es el nombre completo de un contador de rendimiento en \\ formato \Counter de Computer\Object (instancia), como `\\Server1\Processor(0)\% User Time` .  |

#### <a name="options"></a>Opciones

| Opción | Descripción |
|--|--|
| -f `<CSV | TSV | BIN | SQL>` | Especifica el formato del archivo de salida. El valor predeterminado es CSV. |
| -CF `<filename>` | Especifica un archivo que contiene una lista de contadores de rendimiento que se van a supervisar, con un contador por línea. |
| -Si `<[[hh:]mm:]ss>` | Especifica el intervalo de ejemplo. El valor predeterminado es un segundo. |
| -o `<filename>` | Especifica la ruta de acceso del archivo de salida o la base de datos SQL. El valor predeterminado es STDOUT (se escribe en la ventana de comandos). |
| -q `[object]` | Mostrar una lista de contadores instalados (sin instancias). Para enumerar los contadores de un objeto, incluya el nombre del objeto. EJEMPLO |
| -QX `[object]` | Mostrar una lista de contadores instalados con instancias. Para enumerar los contadores de un objeto, incluya el nombre del objeto. |
| -SC `<samples>` | Especifica el número de muestras que se van a recopilar. El valor predeterminado es recopilar datos hasta que se presiona CTRL + C. |
| -config `<filename>` | Especifica un archivo de configuración que contiene opciones de comando. |
| -s `<computer_name>` | Especifica un equipo remoto que se va a supervisar si no se especifica ningún equipo en la ruta de acceso del contador. |
| -y | Responda *sí* a todas las preguntas sin preguntar. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para escribir los valores del contador de rendimiento del equipo local `\Processor(_Total)\% Processor Time` en la ventana de comandos en un intervalo de ejemplo predeterminado de 1 segundo hasta que se presione Ctrl + C, escriba:

```
typeperf \Processor(_Total)\% Processor Time
```

Para escribir los valores de la lista de contadores en el archivo *counters.txt* en el archivo delimitado por tabulaciones *dominio2. TSV* en un intervalo de ejemplo de 5 segundos hasta que se hayan recopilado 50 ejemplos, escriba:

```
typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
```

Para consultar los contadores instalados con instancias para el objeto de contador *DiscoFísico* y escribe la lista resultante en el archivo *counters.txt*, escriba:

```
typeperf -qx PhysicalDisk -o counters.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
