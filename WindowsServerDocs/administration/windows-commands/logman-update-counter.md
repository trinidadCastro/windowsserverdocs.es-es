---
title: Logman (actualizar contador)
description: Tema de referencia del comando Logman Update Counter, que actualiza las propiedades de un recopilador de datos de contador existente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f482e58417e5e3246989169bbb01917fcb6503b0
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222764"
---
# <a name="logman-update-counter"></a>Logman (actualizar contador)

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Actualiza las propiedades de un recopilador de datos de contador existente.

## <a name="syntax"></a>Sintaxis

```
logman update counter <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros


| Parámetro | Descripción |
| --------- | ----------- |
| -s`<computer name>` | Ejecute el comando en el equipo remoto especificado. |
| -config`<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n]`<name>` | Nombre del objeto de destino. |
| -f`<bin|bincirc>` | Especifica el formato del registro del recopilador de datos. |
| -[-] u`<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un `*` para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| -m`<[start] [stop] [[start] [stop] [...]]>` | Cambios en el inicio o detención manual en lugar de una hora de inicio o de finalización programada. |
| -RF`<[[hh:]mm:]ss>` | Ejecuta el recopilador de datos durante el período de tiempo especificado. |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | Comienza a recopilar datos en el momento especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Finaliza la recopilación de datos en el momento especificado. |
| -Si`<[[hh:]mm:]ss>` | Especifica el intervalo de ejemplo para los recopiladores de datos del contador de rendimiento. |
| -o`<path|dsn!log>` | Especifica el archivo de registro de salida o el DSN y el nombre del conjunto de registros en una base de datos SQL. |
| -[-] r | Repite el recopilador de datos diariamente a las horas de inicio y finalización especificadas. |
| -[-] a | Anexa un archivo de registro existente. |
| -[-] permitir | Sobrescribe un archivo de registro existente. |
| -[-] v`<nnnnnn|mmddhhmm>` | Adjunta información de versión del archivo al final del nombre del archivo de registro. |
| -[-] RC`<task>` | Ejecuta el comando especificado cada vez que se cierra el registro. |
| -[-] máx.`<value>` | Tamaño máximo del archivo de registro en MB o número máximo de registros para los registros de SQL. |
| -[-] CNF`<[[hh:]mm:]ss>` | Cuando se especifica Time, cree un nuevo archivo cuando haya transcurrido el tiempo especificado. Si no se especifica Time, cree un archivo nuevo cuando se supere el tamaño máximo. |
| -y | Responde afirmativamente a todas las preguntas sin preguntar. |
| -CF`<filename>` | Especifica el archivo que muestra los contadores de rendimiento que se van a recopilar. El archivo debe contener un nombre de contador de rendimiento por línea. |
| -c `<path [path [ ]]>` | Especifica los contadores de rendimiento que se van a recopilar. |
| -SC`<value>` | Especifica el número máximo de muestras que se van a recopilar con un recopilador de datos del contador de rendimiento. |
| /? | Muestra la ayuda contextual. |

#### <a name="remarks"></a>Comentarios

- Donde [-] aparece en la lista, al agregar un guion adicional (-) se anula la opción.

### <a name="examples"></a>Ejemplos

Para crear un contador llamado *perf_log* con el contador% de tiempo de procesador de la categoría de contador del procesador (_Total), escriba:

```
logman create counter perf_log -c \Processor(_Total)\% Processor time
```

Para actualizar un contador existente llamado *perf_log*, cambiar el intervalo de ejemplo a 10, el formato de registro a CSV y agregar el control de versiones al nombre del archivo de registro con el formato mmddHHMM, escriba:

```
logman update counter perf_log -si 10 -f csv -v mmddhhmm
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Logman Create Counter](logman-create-counter.md)

- [Logman (comando)](logman.md)
