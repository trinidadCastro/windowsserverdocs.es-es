---
title: FC
description: Artículo de referencia para el comando FC, que compara dos archivos o conjuntos de archivos y muestra las diferencias entre ellos.
ms.topic: reference
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb4bd745ec9c1a9dfe066fd5eeefdc2d5517d7cb
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036633"
---
# <a name="fc"></a>FC

Compara dos archivos o conjuntos de archivos y muestra las diferencias entre ellos.

## <a name="syntax"></a>Sintaxis

```
fc /a [/c] [/l] [/lb<n>] [/n] [/off[line]] [/t] [/u] [/w] [/<nnnn>] [<drive1>:][<path1>]<filename1> [<drive2>:][<path2>]<filename2>
fc /b [<drive1:>][<path1>]<filename1> [<drive2:>][<path2>]<filename2>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /a | Abrevia el resultado de una comparación de ASCII. En lugar de mostrar todas las líneas que son diferentes, **FC** solo muestra la primera y la última línea para cada conjunto de diferencias. |
| /b | Compara los dos archivos en modo binario, byte por byte y no intenta volver a sincronizar los archivos después de encontrar una incoherencia. Este es el modo predeterminado para comparar archivos con las siguientes extensiones de archivo:. exe,. com,. sys,. obj,. lib o. bin. |
| /C | Omite el caso de la letra. |
| /l | Compara los archivos en modo ASCII, línea a línea e intenta volver a sincronizar los archivos después de encontrar una incoherencia. Este es el modo predeterminado para comparar archivos, excepto los archivos con las siguientes extensiones de archivo:. exe,. com,. sys,. obj,. lib o. bin. |
| /lb`<n>` | Establece el número de líneas del búfer de línea interno en *N*. La longitud predeterminada del búfer de línea es 100 líneas. Si los archivos que está comparando tienen más de 100 líneas distintas consecutivas, **FC** cancela la comparación. |
| /n | Muestra los números de línea durante una comparación de ASCII. |
| /OFF [línea] | No omite los archivos que tienen el conjunto de atributos sin conexión. |
| /t | Impide que **FC** convierta tabulaciones en espacios. El comportamiento predeterminado consiste en tratar las tabulaciones como espacios, con paradas en cada octava posición de carácter. |
| /U | Compara los archivos como archivos de texto Unicode. |
| /w | Comprime el espacio en blanco (es decir, tabulaciones y espacios) durante la comparación. Si una línea contiene muchos espacios consecutivos o tabulaciones, **/w** trata estos caracteres como un solo espacio. Cuando se usa con **/w**, **FC** omite los espacios en blanco al principio y al final de una línea. |
| /`<nnnn>` | Especifica el número de líneas consecutivas que deben coincidir después de un error de coincidencia, antes de que **FC** considere que los archivos se van a volver a sincronizar. Si el número de líneas coincidentes en los archivos es menor que *nnnn*, **FC** muestra las líneas coincidentes como diferencias. El valor predeterminado es 2. |
| `[<drive1>:][<path1>]<filename1>` | Especifica la ubicación y el nombre del primer archivo o conjunto de archivos que se van a comparar. se requiere *nombreDeArchivo1* . |
| `[<drive2>:][<path2>]<filename2>` | Especifica la ubicación y el nombre del segundo archivo o conjunto de archivos que se van a comparar. se requiere *Nombredearchivo2* . |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Este comando se implemeted por c:\WINDOWS\fc.exe. Puede usar este comando en PowerShell, pero asegúrese de deletrear el ejecutable completo (fc.exe), ya que ' FC ' también es un alias para Format-Custom.

- Cuando se usa **FC** para una comparación de ASCII, **FC** muestra las diferencias entre dos archivos en el siguiente orden:

  - Nombre del primer archivo

  - Líneas de *archivo1* que difieren entre los archivos

  - Primera línea que debe coincidir en ambos archivos

  - Nombre del segundo archivo

  - Líneas de *Nombredearchivo2* que difieren

  - Primera línea que debe coincidir

- **/b** muestra las discrepancias que se encuentran durante una comparación binaria en la sintaxis siguiente:

    `\<XXXXXXXX: YY ZZ>`

    El valor de *xxxxxxxx* especifica la dirección hexadecimal relativa para el par de bytes, medido desde el principio del archivo. Las direcciones se inician en 00000000. Los valores hexadecimales para *YY* y *ZZ* representan los bytes no coincidentes de *nombreDeArchivo1* y *Nombredearchivo2*, respectivamente.

- Puede usar caracteres comodín (**&#42;** y **?**) en *nombreDeArchivo1* y *Nombredearchivo2*. Si usa un carácter comodín en *nombreDeArchivo1*, **FC** compara todos los archivos especificados con el archivo o conjunto de archivos especificado por *Nombredearchivo2*. Si usa un carácter comodín en *Nombredearchivo2*, **FC** usa el valor correspondiente de *nombreDeArchivo1*.

- Al comparar archivos ASCII, **FC** usa un búfer interno (lo suficientemente grande como para contener 100 líneas) como almacenamiento. Si los archivos son más grandes que el búfer, **FC** compara lo que puede cargar en el búfer. Si **FC** no encuentra ninguna coincidencia en las partes cargadas de los archivos, se detiene y muestra el mensaje siguiente:

    `Resynch failed. Files are too different.`

    Al comparar archivos binarios mayores que la memoria disponible, **FC** compara ambos archivos por completo, superponiendo las partes de la memoria con las siguientes partes del disco. El resultado es el mismo que el de los archivos que caben completamente en la memoria.

### <a name="examples"></a>Ejemplos

Para realizar una comparación ASCII de dos archivos de texto, *Monthly. RPT* y *sales. RPT*, y mostrar los resultados en formato abreviado, escriba:

```
fc /a monthly.rpt sales.rpt
```

Para realizar una comparación binaria de dos archivos por lotes, *profits.bat* y *earnings.bat*, escriba:

```
fc /b profits.bat earnings.bat
```

Aparecen resultados similares a los siguientes:

```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
000005E8: 00 6E
FC: earnings.bat longer than profits.bat
```

Si los archivos profits.bat y earnings.bat son idénticos, **FC** muestra el siguiente mensaje:

```
Comparing files profits.bat and earnings.bat
FC: no differences encountered
```

Para comparar cada archivo. bat en el directorio actual con el archivo *new.bat*, escriba:

```
fc *.bat new.bat
```

Para comparar el archivo *new.bat* de la unidad C con el archivo *new.bat* en la unidad D, escriba:

```
fc c:new.bat d:*.bat
```

Para comparar cada archivo por lotes del directorio raíz de la unidad C con el archivo con el mismo nombre en el directorio raíz de la unidad D, escriba:

```
fc c:*.bat d:*.bat
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
