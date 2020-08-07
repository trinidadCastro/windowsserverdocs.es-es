---
title: Consideraciones sobre la creación de módulos de PowerShell
description: Consideraciones sobre la creación de módulos de PowerShell
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: bb22009262cc1ae713846779c6b24402e3ed7928
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896265"
---
# <a name="powershell-module-authoring-considerations"></a>Consideraciones sobre la creación de módulos de PowerShell

En este documento se incluyen algunas directrices relacionadas con el modo en que se crea un módulo para obtener el mejor rendimiento.

## <a name="module-manifest-authoring"></a>Creación de manifiestos de módulo

Un manifiesto de módulo que no usa las siguientes directrices puede tener un impacto perceptible en el rendimiento general de PowerShell, incluso si el módulo no se usa en una sesión.

La detección automática de comandos analiza cada módulo para determinar qué comandos exporta el módulo y este análisis puede ser caro.
Los resultados del análisis de módulos se almacenan en caché por usuario, pero la memoria caché no está disponible en la primera ejecución, que es un escenario típico con contenedores.
Durante el análisis de módulos, si los comandos exportados se pueden determinar completamente a partir del manifiesto, se puede evitar el análisis más caro del módulo.

### <a name="guidelines"></a>Instrucciones

* En el manifiesto del módulo, no use caracteres comodín en las `AliasesToExport` `CmdletsToExport` entradas, y `FunctionsToExport` .

* Si el módulo no exporta comandos de un tipo determinado, especifíquelo explícitamente en el manifiesto especificando `@()` .
Una entrada o que falta `$null` es equivalente a especificar el carácter comodín `*` .

Siempre que sea posible, debe evitarse lo siguiente:

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

En su lugar, use:

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>Evitar CDXML

A la hora de decidir cómo implementar el módulo, hay tres opciones principales:

* Binario (normalmente C#)
* Script (PowerShell)
* CDXML (un contenedor de archivos XML)

Si la velocidad de carga del módulo es importante, CDXML es aproximadamente un orden de magnitud más lento que un módulo binario.

Un módulo binario carga el más rápido porque se compila de antemano y puede usar NGen para la compilación JIT una vez por máquina.

Normalmente, un módulo de script carga un poco más lentamente que un módulo binario, ya que PowerShell debe analizar el script antes de compilarlo y ejecutarlo.

Normalmente, un módulo CDXML es mucho más lento que un módulo de script porque debe analizar primero un archivo XML que, a continuación, genera bastante un script de PowerShell que se analiza y se compila.

