---
title: tasklist
description: Artículo de referencia para el comando TaskList, que muestra una lista de los procesos que se ejecutan en el equipo local o remoto.
ms.topic: reference
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: af89112d98cb7799a2fa910d562ac8c2a35bba19
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718062"
---
# <a name="tasklist"></a>tasklist

Muestra una lista de procesos en ejecución actualmente en el equipo local o en un equipo remoto. **TaskList** reemplaza a la herramienta **Tlist** .

> [!NOTE]
> Este comando reemplaza a la herramienta **Tlist** .

## <a name="syntax"></a>Sintaxis

```
tasklist [/s <computer> [/u [<domain>\]<username> [/p <password>]]] [{/m <module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <filter> [/fi <filter> [ ... ]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `<domain>\<username>` | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<username>` o por `<domain>\<username>` . El parámetro **/u** solo se puede especificar si también se especifica **/s** . El valor predeterminado son los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /m `<module>` | Muestra todas las tareas con módulos de DLL cargados que coinciden con el nombre de patrón especificado. Si no se especifica el nombre del módulo, esta opción muestra todos los módulos cargados por cada tarea. |
| SVC | Muestra toda la información del servicio para cada proceso sin truncamiento. Válido cuando el parámetro **/FO** está establecido en **TABLE**. |
| /v | Muestra información detallada de las tareas en la salida. Para obtener una salida detallada completa sin truncamiento, utilice **/v** y **/SVC** juntos. |
| /FO `{table | list | csv}` | Especifica el formato que se va a utilizar para la salida. Los valores válidos son **TABLE**, **List**y **CSV**. El formato predeterminado de la salida es **TABLE**. |
| /NH | Suprime los encabezados de columna en la salida. Válido cuando el parámetro **/FO** está establecido en **TABLE** o **CSV**. |
| /fi `<filter>` | Especifica los tipos de procesos que se van a incluir o excluir de la consulta. Puede usar más de un filtro o usar el carácter comodín ( `\` ) para especificar todas las tareas o los nombres de las imágenes. Los filtros válidos se muestran en la sección **nombres de filtros, operadores y valores** de este artículo.  |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="filter-names-operators-and-values"></a>Filtrar nombres, operadores y valores

| Nombre del filtro | Operadores válidos | Valores válidos |
|--|--|--|
| STATUS | eq, ne | `RUNNING | NOT RESPONDING | UNKNOWN`. Este filtro no se admite si se especifica un sistema remoto. |
| /IM | eq, ne | Nombre de la imagen |
| PID | eq, ne, gt, lt, ge, le | Valor de PID |
| SESSION | eq, ne, gt, lt, ge, le | Número de la sesión |
| NOMBREDESESIÓN | eq, ne | Nombre de sesión |
| CPUtime | eq, ne, gt, lt, ge, le | Tiempo de CPU con el formato *HH: mm: SS*, donde *mm* y *SS* están entre 0 y 59 y *HH* es cualquier número sin signo. |
| MEMUSAGE | eq, ne, gt, lt, ge, le | Uso de memoria en KB |
| USERNAME | eq, ne | Cualquier nombre de usuario válido ( `<user>` o `<domain\user>` ) |
| Server | eq, ne | Nombre del servicio |
| WINDOWTITLE | eq, ne | Título de la ventana. Este filtro no se admite si se especifica un sistema remoto. |
| ADICIONALES | eq, ne | Nombre de DLL |

## <a name="examples"></a>Ejemplos

Para enumerar todas las tareas con un *ID. de proceso superior a 1000*y *mostrarlas en formato CSV*, escriba:

```
tasklist /v /fi "PID gt 1000" /fo csv
```

Para enumerar los procesos del sistema que se están ejecutando actualmente, escriba:

```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```

Para mostrar información detallada sobre todos los procesos que se están ejecutando actualmente, escriba:

```
tasklist /v /fi "STATUS eq running"
```

Para obtener una lista de toda la información del servicio para los procesos en el equipo remoto *srvmain*, que tiene un nombre de dll que *empieza por ntdll*, escriba:

```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```

Para enumerar los procesos en el equipo remoto *srvmain*, con las credenciales de la cuenta de usuario que ha iniciado sesión actualmente, escriba:

```
tasklist /s srvmain
```

Para enumerar los procesos en el equipo remoto *srvmain*, con las credenciales de la *cuenta de usuario Hiropln*, escriba:

```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
