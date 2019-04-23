---
title: fc
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98fd96c73b962a13c0e715420ebbe6f3cd19a42b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888686"
---
# <a name="fc"></a>fc



Compara dos archivos o conjuntos de archivos y muestra las diferencias entre ellos.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/a|Abrevia la salida de una comparación ASCII. En lugar de mostrar todas las líneas que son diferentes, **fc** muestra sólo la primera y última línea para cada conjunto de diferencias.|
|/b|Compara los dos archivos en modo binario, byte a byte y no intenta volver a sincronizar los archivos después de encontrar un error de coincidencia. Éste es el modo predeterminado para comparar los archivos que tienen las siguientes extensiones: .exe, .com, .sys, .obj, .lib o .bin.|
|/c|Omite las mayúsculas y minúsculas.|
|/l|Compara los archivos en modo ASCII, línea por línea y los intentos de volver a sincronizar los archivos después de encontrar un error de coincidencia. Este es el modo predeterminado para comparar archivos, excepto los archivos con las siguientes extensiones de archivo: .exe, .com, .sys, .obj, .lib o .bin.|
|/lb\<N>|Establece el número de líneas para el búfer interno línea *N*. La longitud predeterminada de búfer es de 100 líneas. Si los archivos que van a comparar tienen más de 100 líneas diferentes consecutivas, **fc** cancela la comparación.|
|/n|Muestra los números de línea durante una comparación ASCII.|
|/off[line]|No omite los archivos que tienen establecido el atributo sin conexión.|
|/t|Evita que **fc** de convertir tabulaciones en espacios. El comportamiento predeterminado consiste en tratar las tabulaciones como espacios, con paradas en cada octava posición del carácter.|
|/u|Compara archivos como archivos de texto Unicode.|
|/w|Comprime los espacios en blanco (es decir, tabulaciones y espacios) durante la comparación. Si una línea contiene muchos espacios o tabulaciones consecutivos, **/w** trata estos caracteres como un único espacio. Cuando se usa con **/w**, **fc** omite los espacios en blanco al principio y al final de una línea.|
|/\<NNNN>|Especifica el número de líneas consecutivas que debe coincidir con un error de coincidencia, las siguientes antes **fc** considera que los archivos se vuelven a sincronizar. Si el número de líneas correspondientes en los archivos es menor que *NNNN*, **fc** muestra las líneas correspondientes como diferencias. El valor predeterminado es 2.|
|[\<Drive1>:][<Path1>]<FileName1>|Especifica la ubicación y el nombre del primer archivo o conjunto de archivos que se va a comparar. *NombreDeArchivo1* es necesario.|
|[\<Drive2>:][<Path2>]<FileName2>|Especifica la ubicación y el nombre del segundo archivo o conjunto de archivos que se va a comparar. *NombreDeArchivo2* es necesario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Este comando es implemeted por c:\WINDOWS\fc.exe. Puede usar este comando en PowerShell, pero asegúrese de deletrear el archivo ejecutable completo (fc.exe) puesto que "fc" es un alias de Format-Custom.

-   Informe de diferencias entre archivos en una comparación ASCII

    Cuando usas **fc** para obtener una comparación ASCII, **fc** muestra las diferencias entre dos archivos en el orden siguiente:  
    -   Nombre del primer archivo
    -   Líneas de *nombreDeArchivo1* que difieren entre los archivos
    -   Primera línea que coincida en ambos archivos
    -   Nombre del segundo archivo
    -   Líneas de *nombreDeArchivo2* que difieren
    -   Primera línea para que coincida con
-   Uso de **/b** para comparaciones binarias

    **/b** muestra las diferencias que se producen durante una comparación binaria en la sintaxis siguiente:

    `\<XXXXXXXX: YY ZZ>`

    El valor de *XXXXXXXX* especifica la dirección relativa hexadecimal para el par de bytes, medidos desde el principio del archivo. Las direcciones comienzan en 00000000. Hexadecimal de valores para *YY* y *ZZ* representan los bytes no coincidentes de *nombreDeArchivo1* y *nombreDeArchivo2*, respectivamente.
-   Uso de caracteres comodín

    Puede usar caracteres comodín (**&#42;** y **?**) en *nombreDeArchivo1* y *FileName2*. Si usa un carácter comodín en *nombreDeArchivo1*, **fc** compara todos los archivos especificados en el archivo o conjunto de archivos especificados por *nombreDeArchivo2*. Si usa un carácter comodín en *FileName2*, **fc** usa el valor correspondiente de *nombreDeArchivo1*.
-   Trabajar con la memoria

    Al comparar archivos ASCII, **fc** utiliza un búfer interno (lo suficientemente grande como para almacenar hasta 100 líneas) como almacenamiento. Si los archivos son más grandes que el búfer, **fc** compara lo que puede cargar en el búfer. Si **fc** no encuentra una coincidencia en las partes de la cargadas de los archivos, se detiene y se muestra el mensaje siguiente:

    `Resynch failed. Files are too different.`

    Al comparar archivos binarios que son mayores que la memoria disponible, **fc** compara ambos archivos en su totalidad, superponiendo las partes en la memoria con las siguientes partes desde el disco. El resultado es el mismo que para los archivos que se ajusten completamente en memoria.

## <a name="BKMK_examples"></a>Ejemplos

Para realizar una comparación ASCII de dos archivos de texto, mensual.rpt y ventas.rpt y ver los resultados en formato abreviado, escriba:
```
fc /a monthly.rpt sales.rpt 
```
Para realizar una comparación binaria de dos archivos por lotes, beneficios.bat e ingresos.bat, escriba:
```
fc /b profits.bat earnings.bat
```
Resultados similares a aparezca la siguiente:
```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
...
...
...
000005E8: 00 6E
FC: Earnings.bat longer than Profits.bat
```
Si es idénticos, el ingresos.bat y beneficios.bat **fc** muestra el mensaje siguiente:
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
Para comparar cada archivo .bat en el directorio actual con el archivo nuevo.bat, escriba:
```
fc *.bat new.bat
```
Para comparar el archivo nuevo.bat en la unidad C con el archivo nuevo.bat en la unidad D, escriba:
```
fc c:new.bat d:*.bat
```
Para comparar cada archivo por lotes en el directorio raíz en la unidad C en el archivo con el mismo nombre en el directorio raíz en la unidad D, escriba:
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
