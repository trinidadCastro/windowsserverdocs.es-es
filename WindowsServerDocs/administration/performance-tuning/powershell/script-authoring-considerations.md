---
title: Consideraciones sobre el rendimiento del scripting de PowerShell
description: Scripting para rendimiento en PowerShell
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 2898cf5ee965da77c9f6a3473e55c1cee6b53f2b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71354980"
---
# <a name="powershell-scripting-performance-considerations"></a>Consideraciones sobre el rendimiento del scripting de PowerShell

Los scripts de PowerShell que aprovechan .NET directamente y evitan que la canalización tenga más rapidez que idiomático PowerShell. Idiomático PowerShell usa normalmente cmdlets y funciones de PowerShell en gran medida, a menudo aprovechando la canalización, y se coloca en .NET solo cuando sea necesario.

>[!Note] 
> Muchas de las técnicas descritas aquí no son idiomático PowerShell y pueden reducir la legibilidad de un script de PowerShell. Se recomienda a los autores de scripts que usen idiomático PowerShell a menos que el rendimiento dicte lo contrario.

## <a name="suppressing-output"></a>Suprimir salida

Hay muchas maneras de evitar escribir objetos en la canalización:

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

La asignación a `$null` o a `[void]` son aproximadamente equivalentes y, por lo general, deben ser preferibles donde sea importante el rendimiento.

```PowerShell
$arrayList.Add($item) > $null
```

La redirección de archivos a `$null` es casi tan buena como las alternativas anteriores, la mayoría de los scripts nunca observarían la diferencia.
En función del escenario, el redireccionamiento de archivos introduce un poco de sobrecarga.

```PowerShell
$arrayList.Add($item) | Out-Null
```

La canalización a `Out-Null` tiene una sobrecarga importante en comparación con las alternativas.
Debe evitar el código sensible al rendimiento.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

Al introducir un bloque de script y llamarlo (mediante el uso de un punto de partida o de otro modo), asignar el resultado a `$null` es una técnica adecuada para suprimir la salida de un bloque grande de script.
Esta técnica realiza aproximadamente la canalización a `Out-Null` y debe evitarse en el script sensible al rendimiento.
La sobrecarga adicional en este ejemplo procede de la creación y la invocación de un bloque de scripts que anteriormente se encontraban en línea.


## <a name="array-addition"></a>Adición de matrices

La generación de una lista de elementos suele realizarse mediante una matriz con el operador de suma:

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Esto puede ser muy inefficent porque las matrices son inmutables.
Cada adición a la matriz crea realmente una nueva matriz lo suficientemente grande como para contener todos los elementos de los operandos izquierdo y derecho, y después copia los elementos de ambos operandos en la nueva matriz.
En el caso de las colecciones pequeñas, esta sobrecarga puede no ser importante.
En el caso de las colecciones de gran tamaño, esto puede ser un problema.

Hay un par de alternativas.
Si realmente no necesita una matriz, considere la posibilidad de usar una ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Si requiere una matriz, puede usar su propio `ArrayList` y simplemente llamar a `ArrayList.ToArray` cuando desee la matriz.
Como alternativa, puede permitir que PowerShell cree el `ArrayList` y `Array`:

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

En este ejemplo, PowerShell crea un `ArrayList` que contiene los resultados que se escriben en la canalización dentro de la expresión de matriz.
Justo antes de asignar a `$results`, PowerShell convierte el `ArrayList` a un `object[]`.

## <a name="processing-large-files"></a>Archivos grandes de procesamiento

La forma idiomático de procesar un archivo en PowerShell puede ser similar a la siguiente:

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Esto puede ser casi un orden de magnitud más lento que el uso de las API de .NET directamente:

```PowerShell
try
{
    $stream = [System.IO.StreamReader]::new($path)
    while ($line = $stream.ReadLine())
    {
        if ($line.Length -gt 10)
        {
            $line
        }
    }
}
finally
{
    $stream.Dispose()
}
```

## <a name="avoid-write-host"></a>Evitar write-host

Por lo general, se considera mala práctica escribir la salida directamente en la consola, pero cuando tiene sentido, muchos scripts usan `Write-Host`.

Si debe escribir muchos mensajes en la consola, `Write-Host` puede ser un orden de magnitud más lento que `[Console]::WriteLine()`. Sin embargo, tenga en cuenta que `[Console]::WriteLine()` solo es una alternativa adecuada para hosts específicos, como PowerShell. exe o powershell_ise. exe, no se garantiza que funcione en todos los hosts.

En lugar de usar `Write-Host`, considere la posibilidad de usar [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

