---
title: tasklist
description: Obtenga información sobre cómo mostrar una lista de los procesos que se ejecutan en el equipo local o remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abb36c8c794836387af5547f3706e910dc06fa42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843006"
---
# <a name="tasklist"></a>tasklist

Muestra una lista de procesos en ejecución actualmente en el equipo local o en un equipo remoto. **TaskList** reemplaza el **tlist** herramienta.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<equipo >|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u [\<Domain>\\\]\<UserName>|Ejecuta el comando con los permisos de cuenta del usuario que se especifica mediante *UserName* o *dominio*\** nombre de usuario. **/u** se puede especificar sólo si **/s** se especifica. El valor predeterminado es los permisos del usuario que ha iniciado sesión actualmente en el equipo que está emitiendo el comando.|
|/p \<contraseña >|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/m \<módulo >|Muestra todas las tareas con los módulos DLL cargados que coinciden con el nombre de un patrón determinado. Si no se especifica el nombre del módulo, esta opción muestra todos los módulos cargados por cada tarea.|
|/svc|Muestra toda la información de servicio para cada proceso sin truncamiento. Es válido cuando el **/fo** parámetro está establecido en **tabla**.|
|/v|Muestra información detallada de tareas en la salida. Para obtener resultados detallados completa sin truncamiento, utilice **/v** y **/svc** juntos.|
|/FO {tabla \| lista \| csv}|Especifica el formato que se usará para la salida. Los valores válidos son **tabla**, **lista**, y **csv**. El formato predeterminado para la salida es **tabla**.|
|/nh|Suprime los encabezados de columna en la salida. Es válido cuando el **/fo** parámetro está establecido en **tabla** o **csv**.|
|/Fi \<filtro >|Especifica los tipos de procesos para incluir o excluir de la consulta. Consulte la siguiente tabla para los nombres de filtro válido, operadores y valores.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="filter-names-operators-and-values"></a>Los nombres de filtro, operadores y valores

|Nombre de filtro|Operadores válidos|Valores válidos|
|-----------|---------------|------------|
|ESTADO|eq, ne|EJECUTANDO | NO RESPONDE | DESCONOCIDO|
|IMAGENAME|eq, ne|Nombre de la imagen|
|PID|eq, ne, gt, lt, ge, le|Valor de PID|
|SESIÓN|eq, ne, gt, lt, ge, le|Número de sesión|
|NOMBRE DE SESIÓN|eq, ne|Nombre de la sesión|
|CPUTIME|eq, ne, gt, lt, ge, le|Tiempo de CPU en el formato *HH ***:*** MM ***:*** SS*, donde *MM* y *SS* están comprendidos entre 0 y 59 y *HH* es cualquiera sin signo de número|
|MEMUSAGE|eq, ne, gt, lt, ge, le|Uso de memoria en KB|
|NOMBRE DE USUARIO|eq, ne|Cualquier nombre de usuario válido|
|SERVICIOS|eq, ne|Nombre del servicio|
|WINDOWTITLE|eq, ne|Título de ventana|
|MÓDULOS|eq, ne|Nombre de DLL|

## <a name="remarks"></a>Comentarios

No se admiten los filtros WINDOWTITLE y el estado cuando se especifica un sistema remoto.

## <a name="BKMK_examples"></a>Ejemplos

Para enumerar todas las tareas con un identificador de proceso mayor que 1000 y mostrarlos en formato CSV, escriba:
```
tasklist /v /fi "PID gt 1000" /fo csv
```
Para enumerar los procesos del sistema que se están ejecutando, escriba:
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
Para mostrar información detallada de todos los procesos que se están ejecutando, escriba:
```
tasklist /v /fi "STATUS eq running"
```
Para mostrar toda la información de servicio para los procesos en el equipo remoto "Srvmain" que tengan un nombre de archivo DLL comience por "ntdll", escriba:
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
Para obtener una lista de los procesos en el equipo remoto "Srvmain", con las credenciales de la cuenta de usuario que inició la sesión, escriba:
```
tasklist /s srvmain 
```
Para obtener una lista de los procesos en el equipo remoto "Srvmain", con las credenciales de la cuenta de usuario Hiropln, escriba:
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)