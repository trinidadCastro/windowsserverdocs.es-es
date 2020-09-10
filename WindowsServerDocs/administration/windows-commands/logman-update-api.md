---
title: logman update api
description: Artículo de referencia del comando Logman Update API, que actualiza las propiedades de un recopilador de datos de seguimiento de API existente.
ms.topic: reference
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 016a6728d032393990e1ff1fd0cb5e4e9a20df94
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639950"
---
# <a name="logman-update-api"></a>logman update api

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Actualiza las propiedades de un recopilador de datos de seguimiento de API existente.

## <a name="syntax"></a>Sintaxis

```
logman update api <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s `<computer name>` | Ejecuta el comando en el equipo remoto especificado. |
| -config `<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n] `<name>` | Nombre del objeto de destino. |
| -f `<bin|bincirc>` | Especifica el formato del registro del recopilador de datos. |
| -[-] u `<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un `*` para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| -m `<[start] [stop] [[start] [stop] [...]]>` | Se ha cambiado a Inicio o detención manual en lugar de a una hora de inicio o de finalización programada. |
| -RF `<[[hh:]mm:]ss>` | Ejecute el recopilador de datos durante el período de tiempo especificado. |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | Comienza a recopilar datos en el momento especificado. |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | Finaliza la recopilación de datos en el momento especificado. |
| -Si `<[[hh:]mm:]ss>` | Especifica el intervalo de ejemplo para los recopiladores de datos del contador de rendimiento. |
| -o `<path|dsn!log>` | Especifica el archivo de registro de salida o el DSN y el nombre del conjunto de registros en una base de datos SQL. |
| -[-] r | Repetir el recopilador de datos diariamente en las horas de inicio y finalización especificadas. |
| -[-] a | Anexar un archivo de registro existente. |
| -[-] permitir | Sobrescribir un archivo de registro existente. |
| -[-] v `<nnnnnn|mmddhhmm>` | Adjunta información de versión del archivo al final del nombre del archivo de registro. |
| -[-] RC `<task>` | Ejecute el comando especificado cada vez que se cierre el registro. |
| -[-] máx. `<value>` | Tamaño máximo del archivo de registro en MB o número máximo de registros para los registros de SQL. |
| -[-] CNF `<[[hh:]mm:]ss>` | Cuando se especifica Time, crea un nuevo archivo cuando ha transcurrido el tiempo especificado. Cuando no se especifica Time, crea un archivo nuevo cuando se supera el tamaño máximo. |
| -y | Responda sí a todas las preguntas sin preguntar. |
| -mods `<path [path [...]]>` | Especifica la lista de módulos de los que se van a registrar las llamadas API. |
| -INAPI` <module!api [module!api [...]]>` | Especifica la lista de llamadas API que se van a incluir en el registro. |
| -exapi `<module!api [module!api [...]]>` | Especifica la lista de llamadas API que se van a excluir del registro. |
| -[-] ano | Log (-ano) nombres de API solamente o no registran los nombres de API (-ano). |
| -[-] recursivo | Log (-Recursive) o no registra (-recursivo) API de forma recursiva más allá de la primera capa. |
| -exe `<value>` | Especifica la ruta de acceso completa a un archivo ejecutable para el seguimiento de la API. |
| /? | Muestra la ayuda contextual. |

#### <a name="remarks"></a>Observaciones

- Donde [-] aparece en la lista, al agregar un guion adicional (-) se anula la opción.

### <a name="examples"></a>Ejemplos

Para actualizar un contador de seguimiento de API existente denominado *trace_notepad*, para el archivo ejecutable c:\windows\notepad.exe, excluyendo la llamada API TlsGetValue producida por el módulo kernel32.dll, escriba:

```
logman update api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Logman Create API](logman-create-api.md)

- [Logman (comando)](logman.md)
