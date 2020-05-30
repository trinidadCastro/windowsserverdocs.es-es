---
title: Logman crear seguimiento
description: Tema de referencia para el comando Logman Create Trace, que crea un recopilador de datos de seguimiento de eventos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b4dfecd-6f56-4c51-b622-c2054b4aabd7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 015fb7842146e372b36c71fe95a3598bdfa48676
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222983"
---
# <a name="logman-create-trace"></a>Logman crear seguimiento

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cree un recopilador de datos de seguimiento de eventos.

## <a name="syntax"></a>Sintaxis

```
logman create trace <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s`<computer name>` | Ejecuta el comando en el equipo remoto especificado. |
| -config`<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| -ETS | Envía comandos a sesiones de seguimiento de eventos directamente sin guardar o programar. |
| [-n]`<name>` | Nombre del objeto de destino. |
| -f`<bin|bincirc>` | Especifica el formato del registro del recopilador de datos. |
| -[-] u`<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un `*` para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | Cambios en el inicio o detención manual en lugar de una hora de inicio o de finalización programada. |
| -RF`<[[hh:]mm:]ss>` | Ejecuta el recopilador de datos durante el período de tiempo especificado. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Comienza a recopilar datos en el momento especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Finaliza la recopilación de datos en el momento especificado. |
| -o`<path|dsn!log>` | Especifica el archivo de registro de salida o el DSN y el nombre del conjunto de registros en una base de datos SQL. |
| -[-] r | Repite el recopilador de datos diariamente a las horas de inicio y finalización especificadas. |
| -[-] a | Anexa un archivo de registro existente. |
| -[-] permitir | Sobrescribe un archivo de registro existente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Adjunta información de versión del archivo al final del nombre del archivo de registro. |
| -[-] RC`<task>` | Ejecuta el comando especificado cada vez que se cierra el registro. |
| -[-] máx.`<value>` | Tamaño máximo del archivo de registro en MB o número máximo de registros para los registros de SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Cuando se especifica Time, crea un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica Time, crea un archivo nuevo cuando se supera el tamaño máximo. |
| -y | Responde afirmativamente a todas las preguntas sin preguntar. |
| -CT`<perf|system|cycle>` | Especifica el tipo de reloj de la sesión de seguimiento de eventos. |
| -LN`<logger_name>` | Especifica el nombre del registrador para las sesiones de seguimiento de eventos. |
| -ft`<[[hh:]mm:]ss>` | Especifica el temporizador de vaciado de sesión de seguimiento de eventos. |
| -[-] p`<provider [flags [level]]>` | Especifica un proveedor de seguimiento de eventos único que se va a habilitar. |
| -PF`<filename>` | Especifica un archivo que enumera varios proveedores de seguimiento de eventos que se van a habilitar. El archivo debe ser un archivo de texto que contenga un proveedor por línea. |
| -[-] RT | Ejecuta la sesión de seguimiento de eventos en modo en tiempo real. |
| -[-] UL | Ejecuta la sesión de seguimiento de eventos en el usuario. |
| -BS`<value>` | Especifica el tamaño del búfer de la sesión de seguimiento de eventos en KB. |
| -NB`<min max>` | Especifica el número de búferes de la sesión de seguimiento de eventos. |
| -modo`<globalsequence|localsequence|pagedmemory>` | Especifica el modo de registrador de sesión de seguimiento de eventos, incluidos:<ul><li>**Globalsequence** : especifica que el seguimiento de eventos agrega un número de secuencia a cada evento que recibe con independencia de qué sesión de seguimiento ha recibido el evento.</li><li>**Localsequence** : especifica que el seguimiento de eventos agrega los números de secuencia de los eventos recibidos en una sesión de seguimiento específica. Cuando se utiliza esta opción, los números de secuencia duplicados pueden existir en todas las sesiones, pero serán únicos en cada sesión de seguimiento.</li><li>**Pagedmemory** : especifica que el seguimiento de eventos utiliza la memoria paginada en lugar del bloque de memoria no paginado predeterminado para sus asignaciones de búfer internas.</li></ul> |
| /? | Muestra la ayuda contextual. |

#### <a name="remarks"></a>Comentarios

- Donde [-] aparece en la lista, al agregar un guion adicional (-) se anula la opción.

### <a name="examples"></a>Ejemplos

Para crear un recopilador de datos de seguimiento de eventos denominado *trace_log*, con menos de 16 y no más de 256 búferes, cada uno de los cuales tiene un tamaño de 64 KB y los resultados se colocan en c:\logfile, escriba:

```
logman create trace trace_log -nb 16 256 -bs 64 -o c:\logfile
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Logman Update Trace](logman-update-trace.md)

- [Logman (comando)](logman.md)
