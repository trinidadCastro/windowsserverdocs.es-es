---
title: tasklist
description: Obtenga información acerca de cómo mostrar una lista de los procesos que se ejecutan en el equipo local o remoto.
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4f87c4cc2dc80c67e2004c929fa23aea8791fb9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881811"
---
# <a name="tasklist"></a>tasklist

Muestra una lista de procesos en ejecución actualmente en el equipo local o en un equipo remoto. **TaskList** reemplaza a la herramienta **Tlist** .



## <a name="syntax"></a>Sintaxis

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

### <a name="parameters"></a>Parámetros

|          Parámetro           |                                                                                                                                            Descripción                                                                                                                                             |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        modificado\<Computer>        |                                                                                         Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local.                                                                                         |
| 5.50\<Domain>\\\]\<UserName> | Ejecuta el comando con los permisos de cuenta del usuario que se especifica mediante *el nombre de usuario o el* *dominio* \* nombreDeUsuario<em>. \* \* /u</em> \* solo se puede especificar si se especifica **/s** . El valor predeterminado son los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando. |
|        /p\<Password>        |                                                                                                       Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                                                        |
|         /m\<Module>         |                                                               Muestra todas las tareas con módulos de DLL cargados que coinciden con el nombre de patrón especificado. Si no se especifica el nombre del módulo, esta opción muestra todos los módulos cargados por cada tarea.                                                                |
|             /SVC             |                                                                                    Muestra toda la información del servicio para cada proceso sin truncamiento. Válido cuando el parámetro **/FO** está establecido en **TABLE**.                                                                                    |
|              /v              |                                                                                 Muestra información detallada de las tareas en la salida. Para obtener una salida detallada completa sin truncamiento, utilice **/v** y **/SVC** juntos.                                                                                 |
|  /FO {TABLE \| List \| CSV}  |                                                                             Especifica el formato que se va a utilizar para la salida. Los valores válidos son **TABLE**, **List**y **CSV**. El formato predeterminado de la salida es **TABLE**.                                                                             |
|             /NH              |                                                                                             Suprime los encabezados de columna en la salida. Válido cuando el parámetro **/FO** está establecido en **TABLE** o **CSV**.                                                                                              |
|        /fi\<Filter>         |                                                                          Especifica los tipos de procesos que se van a incluir o excluir de la consulta. Vea la tabla siguiente para ver los nombres de filtro, los operadores y los valores válidos.                                                                          |
|              /?              |                                                                                                                                Muestra la ayuda en el símbolo del sistema.                                                                                                                                |

### <a name="filter-names-operators-and-values"></a>Filtrar nombres, operadores y valores

| Nombre del filtro |    Operadores válidos     |                                                                 Valores válidos                                                                 |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STATUS    |         eq, ne         |                                                                   RUNNING                                                                    |
|  /IM  |         eq, ne         |                                                                  Nombre de la imagen                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valor de PID                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                Número de la sesión                                                                |
| NOMBREDESESIÓN |         eq, ne         |                                                                 Nombre de sesión                                                                 |
|   CPUTIME   | eq, ne, gt, lt, ge, le | Tiempo de CPU con el formato <em>HH</em>**:**<em>mm</em>**:**<em>SS</em>, donde *mm* y *SS* están entre 0 y 59 y *HH* es cualquier número sin signo. |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Uso de memoria en KB                                                              |
|  USERNAME   |         eq, ne         |                                                             Cualquier nombre de usuario válido                                                              |
|  Server   |         eq, ne         |                                                                 Nombre de servicio                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Título de la ventana                                                                 |
|   ADICIONALES   |         eq, ne         |                                                                   Nombre de DLL                                                                   |

## <a name="remarks"></a>Observaciones

Los filtros WINDOWTITLE y STATUs no se admiten cuando se especifica un sistema remoto.

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos

Para enumerar todas las tareas con un ID. de proceso superior a 1000 y mostrarlas en formato CSV, escriba:
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
Para mostrar toda la información de servicio para los procesos en el equipo remoto "Srvmain" que tienen un nombre de DLL que empieza por "ntdll", escriba:
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
Para enumerar los procesos del equipo remoto "Srvmain", con las credenciales de la cuenta de usuario que ha iniciado sesión actualmente, escriba:
```
tasklist /s srvmain
```
Para enumerar los procesos del equipo remoto "Srvmain", con las credenciales de la cuenta de usuario Hiropln, escriba:
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)