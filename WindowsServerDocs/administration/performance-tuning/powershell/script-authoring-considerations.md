---
title: Consideraciones de rendimiento de secuencias de comandos de PowerShell
description: Secuencias de comandos para el rendimiento en PowerShell
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: df406c7e382907ca32006ec4dae6537a140a91cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830376"
---
# <a name="powershell-scripting-performance-considerations"></a>Consideraciones de rendimiento de secuencias de comandos de PowerShell

Scripts de PowerShell que aprovechan .NET directamente y evitar la canalización tienden a ser más rápido que PowerShell idiomático. PowerShell idiomático normalmente usa cmdlets y funciones de PowerShell muy a menudo aprovechamiento de la canalización y desplegando en .NET solo cuando sea necesario.

>[!Note] 
> Muchas de las técnicas descritas aquí no son idiomático PowerShell y pueden reducir la legibilidad de un script de PowerShell. Los autores de scripts se aconseja usar PowerShell idiomático a menos que el rendimiento dicte lo contrario.

## <a name="suppressing-output"></a>Suprimir salida

Hay muchas maneras de evitar la escritura de objetos a la canalización:

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

Asignación a `$null` o casting a `[void]` son aproximadamente equivalentes y por lo general debe preferirse donde es importante el rendimiento.

```PowerShell
$arrayList.Add($item) > $null
```

Redirección a archivos `$null` es casi tan bueno como las alternativas anteriores, más scripts no observaría nunca la diferencia.
Según el escenario, redirección de archivos presente aunque un poco de sobrecarga.

```PowerShell
$arrayList.Add($item) | Out-Null
```

Canaliza a `Out-Null` tiene una sobrecarga significativa en comparación con las alternativas.
Lo se debe evitar en código sensible de rendimiento.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

Introducción a un bloque de script y llamarlo (mediante el punto de origen o en caso contrario), a continuación, asignar el resultado a `$null` es una técnica conveniente para suprimir la salida de un bloque grande de secuencia de comandos.
Esta técnica se lleva a cabo más o menos, así como el tipo de canalización para `Out-Null` y debe evitarse en script confidenciales de rendimiento.
La sobrecarga adicional en este ejemplo procede de la creación de y la invocación de un bloque de script que era anteriormente el script en línea.


## <a name="array-addition"></a>Adición de matriz

Generar una lista de elementos se suele hacer uso de una matriz con el operador de suma:

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Esto puede ser muy inefficent porque las matrices son inmutables.
Cada adición a la matriz realmente crea una nueva matriz lo suficientemente grande como para contener todos los elementos de los operandos izquierdos y derecho y luego copia los elementos de ambos operandos en la nueva matriz.
Para colecciones pequeñas, esta sobrecarga puede no ser importante.
Para las colecciones grandes, esto puede ser definitivamente un problema.

Hay un par de alternativas.
Si realmente no requiere una matriz, en su lugar considere la posibilidad de usar un objeto ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Si es necesario que una matriz, puede usar su propio `ArrayList` y basta con llamar a `ArrayList.ToArray` cuando desee que la matriz.
Como alternativa, puede permitir que PowerShell crear el `ArrayList` y `Array` para usted:

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

En este ejemplo, PowerShell crea un `ArrayList` para almacenar los resultados que se escriben en la canalización dentro de la expresión de matriz.
Justo antes de asignar a `$results`, PowerShell convierte el `ArrayList` a un `object[]`.

## <a name="processing-large-files"></a>Procesamiento de archivos de gran tamaño

La manera idiomática de procesar un archivo en PowerShell podría ser similar:

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Esto puede ser casi un orden de magnitud más lenta que la API de .NET directamente:

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

## <a name="avoid-write-host"></a>Evitar Write-Host

Por lo general se considera una práctica deficiente para escribir la salida directamente a la consola, pero cuando sea conveniente, usan muchos scripts `Write-Host`.

Si debe escribir el número de mensajes en la consola, `Write-Host` puede ser un orden de magnitud más lenta que `[Console]::WriteLine()`. Sin embargo, tenga en cuenta que `[Console]::WriteLine()` es sólo una alternativa adecuada para los hosts específicos como powershell.exe o powershell_ise.exe: no se garantiza que funcione en todos los hosts.

En lugar de usar `Write-Host`, considere el uso de [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

