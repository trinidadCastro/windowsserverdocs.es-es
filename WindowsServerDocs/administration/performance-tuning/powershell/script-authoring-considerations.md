---
title: Consideraciones sobre el rendimiento del scripting de PowerShell
description: Scripting para rendimiento en PowerShell
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: f5ab7fbb1c993192f4626935d2adb73fc9401250
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896254"
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

La asignación a `$null` o conversión a `[void]` son aproximadamente equivalentes y, por lo general, deben ser preferibles donde sea importante el rendimiento.

```PowerShell
$arrayList.Add($item) > $null
```

La redirección de archivos a `$null` es prácticamente tan buena como las alternativas anteriores, la mayoría de los scripts nunca observarían la diferencia.
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
Esta técnica realiza aproximadamente la canalización `Out-Null` y se debe evitar en el script sensible al rendimiento.
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

Si requiere una matriz, puede usar la suya propia `ArrayList` y simplemente llamar a `ArrayList.ToArray` cuando desee la matriz.
Como alternativa, puede permitir que PowerShell cree el `ArrayList` y `Array` para usted:

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

En este ejemplo, PowerShell crea un `ArrayList` para contener los resultados que se escriben en la canalización dentro de la expresión de matriz.
Justo antes de asignar a `$results` , PowerShell convierte `ArrayList` en `object[]` .

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

Generalmente, se considera una práctica inadecuada escribir la salida directamente en la consola, pero cuando tiene sentido, se usan muchos scripts `Write-Host` .

Si debe escribir muchos mensajes en la consola, `Write-Host` puede ser un orden de magnitud más lento que `[Console]::WriteLine()` . Sin embargo, tenga en cuenta que `[Console]::WriteLine()` solo es una alternativa adecuada para hosts específicos, como powershell.exe o powershell_ise.exe, no se garantiza que funcione en todos los hosts.

En lugar de usar `Write-Host` , considere la posibilidad de usar [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

