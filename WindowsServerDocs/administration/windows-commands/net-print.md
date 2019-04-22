---
title: Net print
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 441a61756869442fb91d26bfacc64bbeb8b902f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826016"
---
# <a name="net-print"></a>Net print

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de una cola de impresión especificada o un trabajo de impresión especificado o controla un trabajo de impresión especificado.
Para obtener ejemplos de cómo usar este comando, consulte el [ejemplos](#BKMK_examples) sección de este documento.
> [!NOTE]
> Este comando está desusado en Windows 7 y Windows Server 2008 R2. Sin embargo, puede realizar muchas de las mismas tareas con prnjobs, Windows Management Instrumentation (WMI) o los cmdlets de Windows PowerShell. Para obtener más información, consulte [prnjobs](prnjobs.md), [Instrumental de administración de Windows](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991), [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)y el [Galería de TechNet Script Center](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635).
## <a name="syntax"></a>Sintaxis
```
Net print {\\<computerName>\<Sharename> | 
\\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
```
## <a name="parameters"></a>Parámetros
|Parámetros|Descripción|
|-------|--------|
|\\\\<computerName>\\<Sharename>|Especifica la cola de impresión y de equipo que va a mostrar información (por nombre).|
|\\\\<computerName>|Especifica el equipo que hospeda el trabajo de impresión que desea controlar (por nombre). Si no especifica un equipo, se supone que el equipo local. Requiere el <JobNumber> parámetro.|
|<JobNumber>|Especifica el número del trabajo de impresión que desea controlar. Este número lo asigna el equipo que hospeda la cola de impresión que se envía el trabajo de impresión. Después de que un equipo asigna un número a un trabajo de impresión, que el número no está asignado a los otros trabajos de impresión en cualquier cola hospedada por ese equipo. Necesario cuando se usa el \\ \\ <computerName> parámetro.|
|[/hold &#124; /release &#124; /delete]|Especifica la acción que se realizará con el trabajo de impresión.<br /><br />-El **/mantenga** parámetro retrasa el trabajo para permitir que otros trabajos de impresión omitirlo hasta que se publique.<br />-El **/versión** parámetro libera un trabajo de impresión que se ha retrasado.<br />-El **/eliminar** parámetro quita un trabajo de impresión de una cola de impresión.<br /><br />Si especifica un número de trabajo, pero no especifica ninguna acción, se muestra información sobre el trabajo de impresión.|
|ayuda|Muestra ayuda para la **Net print** comando.|
## <a name="remarks"></a>Comentarios
-   **Impresión de red** \\ \\ <computerName> muestra información sobre los trabajos de impresión en una cola de impresora compartida. El siguiente es un ejemplo de un informe para todos los trabajos de impresión en una cola para una impresora compartida llamada láser:
    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
       USER1          84        93844      printing
       USER2          85        12555      Waiting
       USER3          86        10222      Waiting
    ```
-   El siguiente es un ejemplo de un informe para un trabajo de impresión:
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
## <a name="BKMK_examples"></a>Ejemplos
En este ejemplo se muestra cómo enumerar el contenido de la cola de impresión de la matriz en la \\\Production equipo:
```
Net print \\Production\Dotmatrix 
```
En este ejemplo se muestra cómo mostrar información acerca del trabajo número 35 en el \\\Production equipo:
```
Net print \\Production 35 
```
En este ejemplo se muestra cómo retrasar el trabajo número 263 en el \\\Production equipo:
```
Net print \\Production 263 /hold 
```
En este ejemplo se muestra cómo liberar el trabajo número 263 en el \\\Production equipo:
```
Net print \\Production 263 /release 
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comandos de impresión](print-command-reference.md)
