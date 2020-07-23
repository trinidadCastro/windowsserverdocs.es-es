---
title: net print
description: Artículo de referencia para el comando net Print. Este comando está en desuso y no se garantiza que se admita en versiones futuras de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ac91d2226e9a5394d6f7ea00ab6f268eb99015b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956807"
---
# <a name="net-print"></a>net print

> [!IMPORTANT]
> Este comando está en desuso. Sin embargo, puede realizar muchas de las mismas tareas con el [comando prnjobs](prnjobs.md), [instrumental de administración de Windows (WMI)](/windows/win32/wmisdk/wmi-start-page), [PrintManagement en PowerShell](/powershell/module/printmanagement)o [los recursos de script para los profesionales de ti](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing).

Muestra información acerca de una cola de impresión especificada o de un trabajo de impresión especificado, o controla un trabajo de impresión especificado.

## <a name="syntax"></a>Sintaxis

```
net print {\\<computername>\<sharename> | \\<computername> <jobnumber> [/hold | /release | /delete]} [help]
```

### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `\\<computername>\<sharename>` | Especifica (por nombre) el equipo y la cola de impresión sobre los que desea mostrar información. |
| `\\<computername>` | Especifica (por nombre) el equipo que hospeda el trabajo de impresión que desea controlar. Si no especifica un equipo, se supone que se trata del equipo local. Requiere el `<jobnumber>` parámetro. |
| `<jobnumber>` | Especifica el número del trabajo de impresión que desea controlar. Este número lo asigna el equipo que hospeda la cola de impresión donde se envía el trabajo de impresión. Una vez que un equipo asigna un número a un trabajo de impresión, ese número no se asigna a ningún otro trabajo de impresión en ninguna cola hospedada por ese equipo. Obligatorio cuando se usa el `\\<computername>` parámetro. |
| `[/hold | /release | /delete]` | Especifica la acción que se realizará con el trabajo de impresión. Si especifica un número de trabajo, pero no especifica ninguna acción, se muestra información sobre el trabajo de impresión.<ul><li>**/Hold** : retrasa el trabajo, permitiendo que otros trabajos de impresión lo omitan hasta que se libere.</li><li>**/Release** : libera un trabajo de impresión que se ha retrasado.</li><li>**/Delete** : quita un trabajo de impresión de una cola de impresión.</li></ul> |
| help | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- El `net print\\<computername>` comando muestra información acerca de los trabajos de impresión en una cola de impresora compartida. A continuación se presenta un ejemplo de un informe de todos los trabajos de impresión de una cola para una impresora compartida denominada *láser*:

    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
    USER1          84        93844      printing
    USER2          85        12555      Waiting
    USER3          86        10222      Waiting
    ```

- A continuación se presenta un ejemplo de un informe para un trabajo de impresión:

    ```
    Job #            35
    Status           Waiting
    Size             3096
    remark
    Submitting user  USER2
    Notify           USER2
    Job data type
    Job parameters
    additional info
    ```

### <a name="examples"></a>Ejemplos

Para mostrar el contenido de la cola de impresión *DotMatrix* en el equipo de * \\ producción* , escriba:

```
net print \\Production\Dotmatrix
```

Para mostrar información sobre el número de trabajo *35* en el equipo de * \\ producción* , escriba:

```
net print \\Production 35
```

Para retrasar el número de trabajo *263* en el equipo de * \\ producción* , escriba:

```
net print \\Production 263 /hold
```

Para liberar el número de trabajo *263* en el equipo de * \\ producción* , escriba:

```
net print \\Production 263 /release
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [referencia de comandos de impresión](print-command-reference.md)

- [prnjobs (comando)](prnjobs.md)

- [Instrumental de administración de Windows (WMI)](/windows/win32/wmisdk/wmi-start-page)

- [PrintManagement en PowerShell](/powershell/module/printmanagement)

- [Script de recursos para profesionales de ti](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing)
