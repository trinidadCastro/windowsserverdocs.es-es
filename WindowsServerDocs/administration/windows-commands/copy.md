---
title: copy
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 102fd6b59516b04b8986ee47b52f521be73f04de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379044"
---
# <a name="copy"></a>copy



Copia uno o varios archivos de una ubicación a otra.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <Source> [/a | /b] [+<Source> [/a | /b] [+ ...]] [<Destination> [/a | /b]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/d|Permite que los archivos cifrados que se copian se guarden como archivos descifrados en el destino.|
|/v|Comprueba que los nuevos archivos están escritos correctamente.|
|/n|Utiliza un nombre de archivo corto, si está disponible, cuando se copia un archivo con un nombre de más de ocho caracteres o una extensión de nombre de archivo de más de tres caracteres.|
|/y|Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente.|
|/-y|Le pide que confirme si desea sobrescribir un archivo de destino existente.|
|/z|Copia los archivos en red en modo reiniciable.|
|/a|Indica un archivo de texto ASCII.|
|b|Indica un archivo binario.|
|> de \<de origen|Obligatorio. Especifica la ubicación desde la que desea copiar un archivo o un conjunto de archivos. El *origen* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.|
|> de destino de \<|Obligatorio. Especifica la ubicación en la que desea copiar un archivo o un conjunto de archivos. El *destino* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Puede copiar un archivo de texto ASCII que use un carácter de fin de archivo (CTRL + Z) para indicar el final del archivo.
-   Usar **/a**

    Cuando **/a** precede o sigue a una lista de archivos de la línea de comandos, se aplica a todos los archivos enumerados hasta que la **copia** se encuentra **/b**. En este caso, **/b** se aplica al archivo anterior **/b**.

    El efecto de **/a** depende de su posición en la cadena de línea de comandos. Cuando **/a** sigue el *código fuente*, la **copia** trata el archivo como un archivo ASCII y copia los datos que preceden al primer carácter de fin de archivo (Ctrl + Z).

    Cuando **/a** sigue el *destino*, **Copy** agrega un carácter de fin de archivo (Ctrl + Z) como el último carácter del archivo.
-   Usar **/b**

    **/b** indica al intérprete de comandos que lea el número de bytes especificado por el tamaño del archivo en el directorio. **/b** es el valor predeterminado para **copiar**, a menos que **copiar** combine archivos.

    Cuando **/b** precede o sigue a una lista de archivos de la línea de comandos, se aplica a todos los archivos enumerados hasta que la **copia** se encuentra en **/a**. En este caso, **/a** se aplica al archivo anterior a **/a**.

    El efecto de **/b** depende de su posición en la cadena de línea de comandos. Cuando **/b** sigue a *source*, **Copy** copia todo el archivo, incluido cualquier carácter de fin de archivo (Ctrl + Z).

    Cuando **/b** sigue el *destino*, la **copia** no agrega un carácter de fin de archivo (Ctrl + Z).
-   Usar **/v**

    Si no se puede comprobar una operación de escritura, aparece un mensaje de error. Aunque los errores de grabación raramente se producen con la **copia**, puede usar **/v** para comprobar que los datos críticos se han registrado correctamente. La opción de línea de comandos **/v** también ralentiza el comando de **copia** , ya que debe comprobarse cada sector registrado en el disco.
-   Usar **/y** y **/-y**

    Si **/y** está preestablecido en la variable de entorno COPYCMD, puede invalidar esta configuración mediante el uso de **/-y** en la línea de comandos. De forma predeterminada, se le preguntará cuando reemplace este valor, a menos que el comando de **copia** se ejecute en un script por lotes.
-   Anexar archivos

    Para anexar archivos, especifique un único archivo para el *destino*, pero varios archivos para el *origen* (usar caracteres comodín o *archivo1*+el formato de *archivo2*+*archivo3* ).
-   Usar **/z**

    Si la conexión se pierde durante la fase de copia (por ejemplo, si el servidor se desconecta y se interrumpe la conexión), la **copia/z** se reanudará después de que se haya restablecido la conexión. **/z** también muestra el porcentaje de la operación de copia que se ha completado para cada archivo.
-   Copiar a dispositivos y desde ellos

    Puede sustituir un nombre de dispositivo por una o varias apariciones del *origen* o del *destino*.
-   Usar u omitir **/b** al copiar en un dispositivo

    Cuando el *destino* es un dispositivo (por ejemplo, COM1 o LPT1), **/b** copia los datos en el dispositivo en modo binario. En el modo binario, **Copy/b** copia todos los caracteres (incluidos los caracteres especiales, como Ctrl + C, Ctrl + S, Ctrl + Z y entrar) en el dispositivo como datos. Sin embargo, si se omite **/b**, los datos se copian en el dispositivo en modo ASCII. En el modo ASCII, los caracteres especiales pueden hacer que los archivos se combinen durante el proceso de copia.
-   Usar el archivo de destino predeterminado

    Si no especifica un archivo de destino, se crea una copia con el mismo nombre, fecha de modificación y hora de modificación que el archivo original. La nueva copia se almacena en el directorio actual de la unidad actual. Si el archivo de origen está en la unidad actual y en el directorio actual y no especifica una unidad o un directorio diferente para el archivo de destino, el comando de **copia** se detiene y muestra el mensaje de error siguiente:

    `File cannot be copied onto itself`

    `0 File(s) copied`
-   Combinar archivos

    Si especifica más de un archivo en el *origen*, **copiar** los combina en un único archivo con el nombre de archivo especificado en *destino*. **Copiar** presupone que los archivos combinados son archivos ASCII a menos que use la opción **/b** .
-   Copiar archivos de longitud cero

    La **copia** no copia los archivos que tienen una longitud de 0 bytes. Use **xcopy** para copiar estos archivos.
-   Cambiar la hora y la fecha de un archivo

    Si desea asignar la fecha y hora actuales a un archivo sin modificar el archivo, use la sintaxis siguiente:  
    ```
    copy /b <Source> +,,
    ```  
    Las comas indican la omisión del parámetro de *destino* .
-   Copiar archivos en subdirectorios

    Para copiar todos los archivos y subdirectorios de un directorio, use el comando **xcopy** .
-   El comando **Copy** , con distintos parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Example

Para copiar un archivo denominado Memo. doc en Letter. doc en la unidad actual y asegurarse de que el carácter de fin de archivo (CTRL + Z) está al final del archivo copiado, escriba:
```
copy memo.doc letter.doc /a
```
Para copiar un archivo llamado Robin. tip desde la unidad actual y el directorio a un directorio existente denominado pájaros que se encuentra en la unidad C, escriba:
```
copy robin.typ c:\birds
```
Si el directorio pájaros no existe, el archivo Robin. Typ se copia en un archivo denominado pájaros que se encuentra en el directorio raíz del disco de la unidad C.

Para combinar Mar89. RPT, Apr89. RPT y May89. RPT, que se encuentran en el directorio actual, y colóquelos en un archivo denominado Informe (también en el directorio actual), escriba:
```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```
Al combinar archivos, **Copy** marca el archivo de destino con la fecha y hora actuales. Si omite el *destino*, los archivos se combinan y se almacenan con el nombre del primer archivo de la lista. Por ejemplo, para combinar todos los archivos de informe cuando ya existe un archivo con el nombre informe, escriba:
```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```
Para combinar todos los archivos del directorio actual que tengan la extensión de nombre de archivo. txt en un único archivo denominado Combined. doc, escriba:
```
copy *.txt Combined.doc 
```
Si desea combinar varios archivos binarios en un archivo mediante caracteres comodín, incluya **/b**. Esto evita que Windows considere CTRL + Z como un carácter de fin de archivo. Por ejemplo, escriba:
```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Si combina archivos binarios, el archivo resultante podría quedar inutilizable debido al formato interno.

En el ejemplo siguiente, **Copy** combina cada archivo que tiene una extensión. txt con su archivo. Ref correspondiente. El resultado es un archivo con el mismo nombre de archivo, pero con una extensión. doc. La **copia** combina Archivo1. txt con Archivo1. Ref para formar Archivo1. doc y, a continuación, **copiar** combina archivo2. txt con archivo2. Ref para formar archivo2. doc, etc. Por ejemplo, escriba:
```
copy *.txt + *.ref *.doc
```
Para combinar todos los archivos con la extensión. txt y, a continuación, combine todos los archivos con la extensión. Ref en un archivo denominado Combined. doc, escriba:
```
copy *.txt + *.ref Combined.doc
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)