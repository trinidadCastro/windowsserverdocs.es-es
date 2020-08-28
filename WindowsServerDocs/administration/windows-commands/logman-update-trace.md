---
title: logman update trace
description: Artículo de referencia del comando Logman Update Trace, que actualiza las propiedades de un recopilador de datos de seguimiento de eventos existente.
ms.topic: reference
ms.assetid: b7111f7f-4162-4d1a-8e53-d766db0ede1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17a63116408458edaf11c2ff44ccf2c1a978cea0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036353"
---
# <a name="logman-update-trace"></a>logman update trace

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Actualiza las propiedades de un recopilador de datos de seguimiento de eventos existente.

## <a name="syntax"></a>Sintaxis

```
logman update trace <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s `<computer name>` | Ejecuta el comando en el equipo remoto especificado. |
| -config `<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| -ETS | Envía comandos a sesiones de seguimiento de eventos directamente sin guardar o programar. |
| [-n] `<name>` | Nombre del objeto de destino. |
| -f `<bin|bincirc>` | Especifica el formato del registro del recopilador de datos. |
| -[-] u `<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un `*` para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| -m `<[start] [stop] [[start] [stop] [...]]>` | Cambios en el inicio o detención manual en lugar de una hora de inicio o de finalización programada. |
| -RF `<[[hh:]mm:]ss>` | Ejecuta el recopilador de datos durante el período de tiempo especificado. |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | Comienza a recopilar datos en el momento especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Finaliza la recopilación de datos en el momento especificado. |
| -o `<path|dsn!log>` | Especifica el archivo de registro de salida o el DSN y el nombre del conjunto de registros en una base de datos SQL. |
| -[-] r | Repite el recopilador de datos diariamente a las horas de inicio y finalización especificadas. |
| -[-] a | Anexa un archivo de registro existente. |
| -[-] permitir | Sobrescribe un archivo de registro existente. |
| -[-] v `<nnnnnn|mmddhhmm>` | Adjunta información de versión del archivo al final del nombre del archivo de registro. |
| -[-] RC `<task>` | Ejecuta el comando especificado cada vez que se cierra el registro. |
| -[-] máx. `<value>` | Tamaño máximo del archivo de registro en MB o número máximo de registros para los registros de SQL. |
| -[-] CNF `<[[hh:]mm:]ss>` | Cuando se especifica Time, crea un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica Time, crea un archivo nuevo cuando se supera el tamaño máximo. |
| -y | Responde afirmativamente a todas las preguntas sin preguntar. |
| -CT `<perf|system|cycle>` | Especifica el tipo de reloj de la sesión de seguimiento de eventos. |
| -LN `<logger_name>` | Especifica el nombre del registrador para las sesiones de seguimiento de eventos. |
| -ft `<[[hh:]mm:]ss>` | Especifica el temporizador de vaciado de sesión de seguimiento de eventos. |
| -[-] p `<provider [flags [level]]>` | Especifica un proveedor de seguimiento de eventos único que se va a habilitar. |
| -PF `<filename>` | Especifica un archivo que enumera varios proveedores de seguimiento de eventos que se van a habilitar. El archivo debe ser un archivo de texto que contenga un proveedor por línea. |
| -[-] RT | Ejecuta la sesión de seguimiento de eventos en modo en tiempo real. |
| -[-] UL | Ejecuta la sesión de seguimiento de eventos en el usuario. |
| -BS `<value>` | Especifica el tamaño del búfer de la sesión de seguimiento de eventos en KB. |
| -NB `<min max>` | Especifica el número de búferes de la sesión de seguimiento de eventos. |
| -modo `<globalsequence|localsequence|pagedmemory>` | Especifica el modo de registrador de sesión de seguimiento de eventos, incluidos:<ul><li>**Globalsequence** : especifica que el seguimiento de eventos agrega un número de secuencia a cada evento que recibe con independencia de qué sesión de seguimiento ha recibido el evento.</li><li>**Localsequence** : especifica que el seguimiento de eventos agrega los números de secuencia de los eventos recibidos en una sesión de seguimiento específica. Cuando se utiliza esta opción, los números de secuencia duplicados pueden existir en todas las sesiones, pero serán únicos en cada sesión de seguimiento.</li><li>**Pagedmemory** : especifica que el seguimiento de eventos utiliza la memoria paginada en lugar del bloque de memoria no paginado predeterminado para sus asignaciones de búfer internas.</li></ul> |
| /? | Muestra la ayuda contextual. |

#### <a name="remarks"></a>Observaciones

- Donde [-] aparece en la lista, al agregar un guion adicional (-) se anula la opción.

### <a name="examples"></a>Ejemplos

Para actualizar un recopilador de datos de seguimiento de eventos existente llamado *trace_log*, cambiar el tamaño máximo del registro a 10 MB, actualizar el formato del archivo de registro a CSV y anexar el control de versiones de archivo en el formato mmddHHMM, escriba:

```
logman update trace trace_log -max 10 -f csv -v mmddhhmm
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Logman-crear seguimiento (comando)](logman-create-trace.md)

- [Logman (comando)](logman.md)
