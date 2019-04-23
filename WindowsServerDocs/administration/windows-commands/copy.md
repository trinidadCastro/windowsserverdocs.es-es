---
title: copy
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d593fbdbffd2a5ee4e4dfb4a817ad4708162160a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853776"
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
|/d|Permite que los archivos cifrados que se va a copiar se guarden como archivos descifrados en el destino.|
|/v|Comprueba que los nuevos archivos se escriben correctamente.|
|/n|Usa un nombre de archivo corto, si está disponible, al copiar un archivo con un nombre más de ocho caracteres, o con una extensión de nombre de archivo más de tres caracteres.|
|/y|Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente.|
|/-y|Le pide que confirme que desea sobrescribir un archivo de destino existente.|
|/z|Copia los archivos en red en modo reiniciable.|
|/a|Indica un archivo de texto ASCII.|
|/b|Indica un archivo binario.|
|\<origen >|Obligatorio. Especifica la ubicación desde la que van a copiar un archivo o un conjunto de archivos. *Origen* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.|
|\<Destino >|Obligatorio. Especifica la ubicación a la que van a copiar un archivo o un conjunto de archivos. *Destino* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Puede copiar un archivo de texto ASCII con un carácter de final de archivo (CTRL+Z) para indicar el final del archivo.
-   Uso de **/a**

    Cuando **/a** precede o sigue a una lista de archivos en la línea de comandos, se aplica a todos los archivos enumerados hasta **copia** encuentra **/b**. En este caso, **/b** se aplica al archivo anterior **/b**.

    El efecto de **/a** depende de su posición en la cadena de línea de comandos. Cuando **/a** sigue *origen*, **copia** trata el archivo como un archivo ASCII y copia los datos que preceden al primer carácter de final de archivo (CTRL + Z).

    Cuando **/a** sigue *destino*, **copia** agrega un carácter de final de archivo (CTRL + Z) como el último carácter del archivo.
-   Uso de **/b**

    **/b** dirige el intérprete de comandos para leer el número de bytes especificado por el tamaño del archivo en el directorio. **/b** es el valor predeterminado para **copia**, a menos que **copia** combina archivos.

    Cuando **/b** precede o sigue a una lista de archivos en la línea de comandos, se aplica a todos los archivos enumerados hasta **copia** encuentra **/a**. En este caso, **/a** se aplica al archivo anterior **/a**.

    El efecto de **/b** depende de su posición en la cadena de línea de comandos. Cuando **/b** sigue *origen*, **copia** copia el archivo completo, incluido cualquier carácter de final de archivo (CTRL + Z).

    Cuando **/b** sigue *destino*, **copia** no agrega un carácter de final de archivo (CTRL + Z).
-   Uso de **/v**

    Si una operación de escritura no se puede comprobar que aparece un mensaje de error. Aunque se producen con poca frecuencia errores de grabación con **copia**, puede usar **/v** para comprobar que los datos críticos se ha registrado correctamente. El **/v** opción de línea de comandos también ralentiza la **copia** comando, porque es necesario comprobar cada sector que se escribe en el disco.
-   Uso de **/y** y   **/y**

    Si **/y** se preestablece en apunta el vínculo, puede invalidar esta configuración mediante el uso de **/y** en la línea de comandos. De forma predeterminada, se pide al reemplazar esta configuración, a menos que el **copia** comando se ejecuta en un script por lotes.
-   Anexar a archivos

    Para anexar archivos, especifique un único archivo para *destino*, pero varios archivos para *origen* (usar caracteres comodín o *File1* +  *File2*+*File3* formato).
-   Uso de   **/z**

    Si se pierde la conexión durante la fase de copia (por ejemplo, si el servidor está quedándose sin conexión interrumpe la conexión), **copiar/z** reanuda una vez restablecida la conexión. **/ z** también muestra el porcentaje de la operación de copia se completa para cada archivo.
-   Copiar a y desde los dispositivos

    Puede sustituir un nombre de dispositivo para una o más apariciones de *origen* o *destino*.
-   Utilizar u omitir **/b** al copiar a un dispositivo

    Cuando *destino* es un dispositivo (por ejemplo, Com1 o Lpt1) **/b** copia los datos en el dispositivo en modo binario. En modo binario, **copiar /b** copia todos los caracteres (incluidos los caracteres especiales, como CTRL + C, CTRL+S, CTRL+Z y ENTRAR) en el dispositivo como datos. Sin embargo, si se omite **/b**, se copian datos en el dispositivo en modo ASCII. En modo ASCII, caracteres especiales pueden provocar archivos combinar durante el proceso de copia.
-   Usar el archivo de destino predeterminado

    Si no especifica un archivo de destino, una copia se crea con el mismo nombre, fecha de modificación y hora que el archivo original de la modificación. La nueva copia se almacena en el directorio actual en la unidad actual. Si el archivo de origen está en la unidad actual y en el directorio actual y no se especifica una unidad diferente o el directorio del archivo de destino, el **copia** comando detiene y se muestra el mensaje de error siguiente:

    `File cannot be copied onto itself`

    `0 File(s) copied`
-   Combinar archivos

    Si especifica más de un archivo en *origen*, **copia** combina todo en un solo archivo con el nombre de archivo especificado en *destino*. **Copia** asume los archivos combinados son archivos ASCII a menos que use el **/b** opción.
-   Copiar archivos de longitud cero

    **Copia** no copia los archivos que tienen una longitud de 0 bytes. Use **xcopy** para copiar estos archivos.
-   Cambiar la fecha y hora de un archivo

    Si desea asignar a la hora actual y la fecha a un archivo sin modificar el archivo, use la sintaxis siguiente:  
    ```
    copy /b <Source> +,,
    ```  
    Las comas indican la omisión de la *destino* parámetro.
-   Copiar archivos en subdirectorios

    Para copiar todos los archivos y subdirectorios de un directorio, use el **xcopy** comando.
-   El **copia** comando, con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Ejemplos

Para copiar un archivo llamado Memo.doc a carta.doc en la unidad actual y asegúrese de que es un carácter de final de archivo (CTRL+Z) al final del archivo copiado, escriba:
```
copy memo.doc letter.doc /a
```
Para copiar un archivo denominado Canario.TIP desde la unidad actual y el directorio a un directorio existente denominado aves que se encuentra en la unidad C, escriba:
```
copy robin.typ c:\birds
```
Si el directorio no existe, se copia el archivo Canario.TIP en un archivo denominado aves que se encuentra en el directorio raíz en el disco en la unidad C.

Para combinar Mar89.rpt Apr89.rpt y May89.rpt, que se encuentran en el directorio actual y se colocan en un archivo denominado informe (también en el directorio actual), escriba:
```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```
Al combinar los archivos, **copia** marca el archivo de destino con la fecha y hora actuales. Si se omite *destino*, los archivos se combinan y se almacenan con el nombre del primer archivo en la lista. Por ejemplo, para combinar todos los archivos de informe cuando un archivo denominado informe ya existe, escriba:
```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```
Para combinar todos los archivos en el directorio actual que tienen la extensión de nombre de archivo the.txt en un único archivo denominado Combined.doc, tipo:
```
copy *.txt Combined.doc 
```
Si desea combinar varios archivos binarios en un archivo mediante el uso de caracteres comodín, incluya **/b**. Esto impide que Windows considere CTRL+Z como carácter de final de archivo. Por ejemplo, escriba:
```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Si combina archivos binarios, el archivo resultante puede ser inservible debido a un formato interno.

En el ejemplo siguiente, **copia** combina cada archivo que tiene una extensión. txt con su correspondiente archivo .ref. El resultado es un archivo con el mismo nombre pero con una extensión .doc. **Copia** combina File1.txt con Archivo1.ref para formar Archivo1 y, a continuación, **copia** combina File2.txt con Archivo2.ref para formar Archivo2.doc y así sucesivamente. Por ejemplo, escriba:
```
copy *.txt + *.ref *.doc
```
Para combinar todos los archivos con la extensión .txt y, a continuación, combinar todos los archivos con la extensión .ref en un archivo denominado Combined.doc, escriba:
```
copy *.txt + *.ref Combined.doc
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)