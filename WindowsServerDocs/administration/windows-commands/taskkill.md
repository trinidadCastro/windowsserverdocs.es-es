---
title: taskkill
description: Artículo de referencia del comando taskkill, que finaliza una o más tareas o procesos.
ms.topic: reference
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e022bc980e2acd7fb70bf13af52f8096fd6aaa1f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718082"
---
# <a name="taskkill"></a>taskkill

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Finaliza uno o más procesos o tareas. Los procesos se pueden finalizar por el identificador del proceso o el nombre de la imagen. Puede utilizar el comando de [comando TaskList](tasklist.md) para determinar el identificador de proceso (PID) del proceso que se va a finalizar.

> [!NOTE]
> Este comando reemplaza a la herramienta **Kill** .

## <a name="syntax"></a>Sintaxis

```
taskkill [/s <computer> [/u [<domain>\]<username> [/p [<password>]]]] {[/fi <filter>] [...] [/pid <processID> | /im <imagename>]} [/f] [/t]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
|  modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `<domain>\<username>` | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<username>` o por `<domain>\<username>` . El parámetro **/u** solo se puede especificar si también se especifica **/s** . El valor predeterminado son los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /fi `<filter>` | Aplica un filtro para seleccionar un conjunto de tareas. Puede usar más de un filtro o usar el carácter comodín ( `*` ) para especificar todas las tareas o los nombres de las imágenes. Los filtros válidos se muestran en la sección **nombres de filtros, operadores y valores** de este artículo. |
| /PID `<processID>` | Especifica el identificador de proceso del proceso que se va a terminar. |
| /im `<imagename>` | Especifica el nombre de la imagen del proceso que se va a terminar. Use el carácter comodín ( `*` ) para especificar todos los nombres de imagen. |
| /f | Especifica que los procesos se terminan forzosamente. Este parámetro se omite para los procesos remotos; todos los procesos remotos han finalizado de manera forzada. |
| /t | Finaliza el proceso especificado y los procesos secundarios iniciados por él. |

#### <a name="filter-names-operators-and-values"></a>Filtrar nombres, operadores y valores

| Nombre del filtro | Operadores válidos | Valores válidos |
|--|--|--|
| STATUS | eq, ne | `RUNNING | NOT RESPONDING | UNKNOWN` |
| /IM | eq, ne | Nombre de la imagen |
| PID | eq, ne, gt, lt, ge, le | Valor de PID |
| SESSION | eq, ne, gt, lt, ge, le | Número de la sesión |
| CPUtime | eq, ne, gt, lt, ge, le | Tiempo de CPU con el formato *HH: mm: SS*, donde *mm* y *SS* están entre 0 y 59 y *HH* es cualquier número sin signo. |
| MEMUSAGE | eq, ne, gt, lt, ge, le | Uso de memoria en KB |
| USERNAME | eq, ne | Cualquier nombre de usuario válido ( `<user>` o `<domain\user>` ) |
| Server | eq, ne | Nombre del servicio |
| WINDOWTITLE | eq, ne | Título de la ventana |
| ADICIONALES | eq, ne | Nombre de DLL |

## <a name="remarks"></a>Observaciones

- Los filtros **WINDOWTITLE** y **status** no se admiten cuando se especifica un sistema remoto.

- El carácter comodín ( `*` ) se acepta para la `*/im` opción, solo cuando se aplica un filtro.

- La finalización de un proceso remoto siempre se lleva a cabo forzosamente, independientemente de si se especifica la opción **/f** .

- Proporcionar un nombre de equipo al filtro de nombre de host provoca un cierre y detiene todos los procesos.

## <a name="examples"></a>Ejemplos

Para finalizar los procesos con los identificadores de proceso *1230*, *1241*y *1253*, escriba:

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Para forzar la finalización del proceso *Notepad.exe* si el sistema lo inició, escriba:

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Para finalizar todos los procesos del equipo remoto *Srvmain* con un nombre de imagen que empiece por *Note*, mientras usa las credenciales de la cuenta de usuario *Hiropln*, escriba:

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Para finalizar el proceso con el identificador de proceso *2134* y los procesos secundarios que inició, pero solo si los procesos se iniciaron con la cuenta de administrador, escribe:

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Para finalizar todos los procesos que tienen un identificador de proceso *mayor o igual que 1000*, independientemente de sus nombres de imagen, escriba:

```
taskkill /f /fi "PID ge 1000" /im *
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Comando TaskList](tasklist.md)
