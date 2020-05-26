---
title: impresión en red
description: Tema de referencia del comando net Print, que muestra información acerca de un trabajo de impresión o cola de impresión especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44b781cb0c3b9fb7def5ee72bcc1242ac83ba4b2
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820885"
---
# <a name="net-print"></a>impresión en red

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de una cola de impresión especificada o de un trabajo de impresión especificado, o controla un trabajo de impresión especificado.

> [!NOTE]
> Este comando está en desuso en Windows 7 y Windows Server 2008 R2. Sin embargo, puede realizar muchas de las mismas tareas mediante prnjobs, Instrumental de administración de Windows (WMI) o cmdlets de Windows PowerShell. Para obtener más información, consulte [prnjobs](prnjobs.md), [instrumental de administración de Windows](https://go.microsoft.com/fwlink/?LinkID=29991) ( https://go.microsoft.com/fwlink/?LinkID=29991) , [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) ( https://go.microsoft.com/fwlink/?LinkID=128426) y la [Galería de TechNet script Center](https://go.microsoft.com/fwlink/?LinkId=164635) ) ( https://go.microsoft.com/fwlink/?LinkId=164635) .

## <a name="syntax"></a>Sintaxis
> ```
> Net print {\\<computerName>\<Sharename> |
> \\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
> ```
> ### <a name="parameters"></a>Parámetros
>
> |               Parámetros               |                                                                                                                                                                                                                     Descripción                                                                                                                                                                                                                      |
> |----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |    \\\\<computerName>\\<Sharename>     |                                                                                                                                                                            Especifica (por nombre) el equipo y la cola de impresión sobre los que desea mostrar información.                                                                                                                                                                             |
> |           \\\\<computerName>           |                                                                                                                                 Especifica (por nombre) el equipo que hospeda el trabajo de impresión que desea controlar. Si no especifica un equipo, se supone que se trata del equipo local. Requiere el <JobNumber> parámetro.                                                                                                                                  |
> |              <JobNumber>               |                                             Especifica el número del trabajo de impresión que desea controlar. Este número lo asigna el equipo que hospeda la cola de impresión donde se envía el trabajo de impresión. Una vez que un equipo asigna un número a un trabajo de impresión, ese número no se asigna a ningún otro trabajo de impresión en ninguna cola hospedada por ese equipo. Obligatorio cuando se usa el \\ \\ <computerName> parámetro.                                             |
> | [/HOLD &#124;/Release &#124;/DELETE] | Especifica la acción que se realizará con el trabajo de impresión.<p>-El parámetro **/Hold** retrasa el trabajo, lo que permite que otros trabajos de impresión lo omitan hasta que se libere.<br />-El parámetro **/Release** libera un trabajo de impresión que se ha retrasado.<br />-El parámetro **/Delete** quita un trabajo de impresión de una cola de impresión.<p>Si especifica un número de trabajo, pero no especifica ninguna acción, se muestra información sobre el trabajo de impresión. |
> |                  help                  |                                                                                                                                                                                                     Muestra la ayuda para el comando **net Print** .                                                                                                                                                                                                     |
>
>#### <a name="remarks"></a>Observaciones
> - **Net print** \\ Impresión \\ en <computerName> red muestra información acerca de los trabajos de impresión en una cola de impresora compartida. A continuación se presenta un ejemplo de un informe de todos los trabajos de impresión de una cola para una impresora compartida denominada láser:
>   ```
>   printers at \\PRODUCTION
>   Name              Job #      Size      Status
>   -----------------------------
>   LASER Queue       3 jobs               *printer active*
>      USER1          84        93844      printing
>      USER2          85        12555      Waiting
>      USER3          86        10222      Waiting
>   ```
> - A continuación se presenta un ejemplo de un informe para un trabajo de impresión:
>   ```
>   Job #            35
>   Status           Waiting
>   Size             3096
>   remark
>   Submitting user  USER2
>   Notify           USER2
>   Job data type
>   Job parameters
>   additional info
>   ```
>   ## <a name="examples"></a>Ejemplos
>   En este ejemplo se muestra cómo enumerar el contenido de la cola de impresión DotMatrix en el \\ equipo \Production:
>   ```
>   Net print \\Production\Dotmatrix
>   ```
>   En este ejemplo se muestra cómo Mostrar información sobre el número de trabajo 35 en el \\ equipo \Production:
>   ```
>   Net print \\Production 35
>   ```
>   En este ejemplo se muestra cómo retrasar el número de trabajo 263 en el \\ equipo \Production:
>   ```
>   Net print \\Production 263 /hold
>   ```
>   En este ejemplo se muestra cómo liberar el número de trabajo 263 en el \\ equipo \Production:
>   ```
>   Net print \\Production 263 /release
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
>   - Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
>    [referencia de comandos de impresión](print-command-reference.md)
