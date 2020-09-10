---
title: logman update alert
description: Artículo de referencia del comando Logman Update Alert, que actualiza las propiedades de un recopilador de datos de alerta existente.
ms.topic: reference
ms.assetid: ede94a76-931c-40ed-9fda-6766bed8ff72
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9c870ba5699ec0c804d387f1d1c723032a6ac335
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639957"
---
# <a name="logman-update-alert"></a>logman update alert

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Actualiza las propiedades de un recopilador de datos de alerta existente.

## <a name="syntax"></a>Sintaxis

```
logman update alert <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s `<computer name>` | Ejecute el comando en el equipo remoto especificado. |
| -config `<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n] `<name>` | Nombre del objeto de destino. |
| -[-] u `<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un `*` para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| -m `<[start] [stop] [[start] [stop] [...]]>` | Cambios en el inicio o detención manual en lugar de una hora de inicio o de finalización programada. |
| -RF `<[[hh:]mm:]ss>` | Ejecuta el recopilador de datos durante el período de tiempo especificado. |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | Comienza a recopilar datos en el momento especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Finaliza la recopilación de datos en el momento especificado. |
| -Si `<[[hh:]mm:]ss>` | Especifica el intervalo de ejemplo para los recopiladores de datos del contador de rendimiento. |
| -o `<path|dsn!log>` | Especifica el archivo de registro de salida o el DSN y el nombre del conjunto de registros en una base de datos SQL. |
| -[-] r | Repite el recopilador de datos diariamente a las horas de inicio y finalización especificadas. |
| -[-] a | Anexa un archivo de registro existente. |
| -[-] permitir | Sobrescribe un archivo de registro existente. |
| -[-] v `<nnnnnn|mmddhhmm>` | Adjunta información de versión del archivo al final del nombre del archivo de registro. |
| -[-] RC `<task>` | Ejecuta el comando especificado cada vez que se cierra el registro. |
| -[-] máx. `<value>` | Tamaño máximo del archivo de registro en MB o número máximo de registros para los registros de SQL. |
| -[-] CNF `<[[hh:]mm:]ss>` | Cuando se especifica Time, crea un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica Time, crea un archivo nuevo cuando se supera el tamaño máximo. |
| -y | Responde afirmativamente a todas las preguntas sin preguntar. |
| -CF `<filename>` | Especifica el archivo que muestra los contadores de rendimiento que se van a recopilar. El archivo debe contener un nombre de contador de rendimiento por línea. |
| -[-] el | Habilita o deshabilita los informes del registro de eventos. |
| -TH `<threshold [threshold [...]]>` | Especifique los contadores y sus valores de umbral para una alerta. |
| -[-]rdcs `<name>` | Especifica el conjunto de recopiladores de datos que se va a iniciar cuando se desencadene una alerta. |
| -[-] TN `<task>` | Especifica la tarea que se va a ejecutar cuando se desencadene una alerta. |
| -[-] ex `<argument>` | Especifica los argumentos de tarea que se van a usar con la tarea especificada mediante-TN. |
| /? | Muestra la ayuda contextual. |

#### <a name="remarks"></a>Observaciones

- Donde [-] aparece en la lista, al agregar un guion adicional (-) se anula la opción.

### <a name="examples"></a>Ejemplos

Para actualizar la alerta existente llamada *new_alert*, establezca el valor de umbral para el contador% de tiempo de procesador del procesador (_Total) en 40%, escriba:

```
logman update alert new_alert -th \Processor(_Total)\% Processor time>40
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Logman crear alerta (comando)](logman-create-alert.md)

- [Logman (comando)](logman.md)
