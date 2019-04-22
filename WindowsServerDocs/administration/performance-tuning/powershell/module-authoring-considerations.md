---
title: Consideraciones de creación de módulo de PowerShell
description: Consideraciones de creación de módulo de PowerShell
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 37dd860019b91daf70947dba93d20274048487a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818726"
---
# <a name="powershell-module-authoring-considerations"></a>Consideraciones de creación de módulo de PowerShell

Este documento incluye algunas directrices relacionadas con cómo se crea un módulo para un mejor rendimiento.

## <a name="module-manifest-authoring"></a>Creación del manifiesto de módulo

Un manifiesto de módulo que no utilice las siguientes directrices puede tener un impacto perceptible en el rendimiento general de PowerShell, incluso si el módulo no se usa en una sesión.

Detección automática de comandos analiza cada módulo para determinar los comandos que exporta el módulo y este análisis pueden resultar costoso.
Los resultados del análisis de módulo se almacenan en caché por usuario, pero la memoria caché no está disponible en la primera ejecución, que es un escenario típico con contenedores.
Durante el análisis de módulo, si se pueden determinar completamente los comandos exportados desde el manifiesto, puede evitarse más costoso análisis del módulo.

### <a name="guidelines"></a>Instrucciones

* En el manifiesto del módulo, no use caracteres comodín en el `AliasesToExport`, `CmdletsToExport`, y `FunctionsToExport` entradas.

* Si el módulo no exporta los comandos de un tipo determinado, especificar esto explícitamente en el manifiesto especificando `@()`.
Falta un o `$null` entrada es equivalente a especificar el carácter comodín `*`.

El siguiente debe evitarse siempre que sea posible:

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

Al decidir cómo implementar el módulo, hay tres opciones principales:

* Binario (normalmente C#)
* Secuencia de comandos (PowerShell)
* CDXML (un archivo XML que se ajuste CIM)

Si es importante la velocidad de la carga el módulo, CDXML es aproximadamente un orden de magnitud más lenta que un módulo binario.

Un módulo binario se carga la mayor brevedad posible porque se compila antes de tiempo y puede usar NGen para la compilación JIT una vez por equipo.

Normalmente, un módulo de script carga un poco más lentamente que un módulo binario porque PowerShell debe analizar el script antes de compilar y ejecutarla.

Un módulo CDXML es normalmente más lento que un módulo de script porque primero debe analizar un archivo XML que, a continuación, genera gran cantidad de script de PowerShell que, a continuación, se analiza y compila.

