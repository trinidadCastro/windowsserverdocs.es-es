---
title: copy
description: Tema de referencia del comando copy, que copia uno o varios archivos de una ubicación a otra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 909bcdf83c87440dafe2653711c4d7e215f8d66b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719296"
---
# <a name="copy"></a>copy

Copia uno o varios archivos de una ubicación a otra.

> [!NOTE]
> También puede usar el comando **copiar** , con parámetros diferentes, de la consola de recuperación. Para obtener más información acerca de la consola de recuperación, consulte [entorno de recuperación de Windows (Windows re)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxis

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <source> [/a | /b] [+<source> [/a | /b] [+ ...]] [<destination> [/a | /b]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /d | Permite que los archivos cifrados que se copian se guarden como archivos descifrados en el destino. |
| /v | Comprueba que los nuevos archivos están escritos correctamente. |
| /n | Utiliza un nombre de archivo corto, si está disponible, cuando se copia un archivo con un nombre de más de ocho caracteres o una extensión de nombre de archivo de más de tres caracteres. |
| /y | Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente. |
| /-y | Le pide que confirme si desea sobrescribir un archivo de destino existente. |
| /z | Copia los archivos en red en modo reiniciable. |
| /a | Indica un archivo de texto ASCII. |
| /b | Indica un archivo binario. |
| `<source>` | Necesario. Especifica la ubicación desde la que desea copiar un archivo o un conjunto de archivos. El *origen* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos. |
| `<destination>` | Necesario. Especifica la ubicación en la que desea copiar un archivo o un conjunto de archivos. El *destino* puede constar de una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Puede copiar un archivo de texto ASCII que use un carácter de fin de archivo (CTRL + Z) para indicar el final del archivo.

- Si **/a** precede o sigue a una lista de archivos de la línea de comandos, se aplica a todos los archivos enumerados hasta que la **copia** se encuentra **/b**. En este caso, **/b** se aplica al archivo anterior **/b**.

    El efecto de **/a** depende de su posición en la cadena de línea de comandos:
      - Si **/a** sigue el *código fuente*, el comando de **copia** trata el archivo como un archivo ASCII y copia los datos que preceden al primer carácter de fin de archivo (Ctrl + Z).
      - Si **/a** sigue el *destino*, el comando de **copia** agrega un carácter de fin de archivo (Ctrl + Z) como el último carácter del archivo.

- Si **/b** indica al intérprete de comandos que lea el número de bytes especificado por el tamaño del archivo en el directorio. **/b** es el valor predeterminado para **copiar**, a menos que **copiar** combine archivos.

- Si **/b** precede o sigue a una lista de archivos en la línea de comandos, se aplica a todos los archivos enumerados hasta que la **copia** se encuentra en **/a**. En este caso, **/a** se aplica al archivo anterior a **/a**.

    El efecto de **/b** depende de su posición en la cadena de línea de comandos:-Si **/b** sigue el *código fuente*, el comando **copiar** copia todo el archivo, incluido cualquier carácter de fin de archivo (Ctrl + Z).
        -Si **/b** sigue el *destino*, el comando de **copia** no agrega un carácter de fin de archivo (Ctrl + Z).

- Si no se puede comprobar una operación de escritura, aparece un mensaje de error. Aunque raramente se producen errores de registro con el comando **Copy** , puede usar **/v** para comprobar que los datos críticos se han registrado correctamente. La opción de línea de comandos **/v** también ralentiza el comando de **copia** , ya que debe comprobarse cada sector registrado en el disco.

- Si **/y** está preestablecido en la variable de entorno **COPYCMD** , puede invalidar esta configuración mediante el uso de **/-y** en la línea de comandos. De forma predeterminada, se le preguntará cuando reemplace este valor, a menos que el comando de **copia** se ejecute en un script por lotes.
  
- Para anexar archivos, especifique un único archivo para el *destino*, pero varios archivos para el *origen* (usar caracteres comodín o formato *archivo1*+*archivo2*+*archivo3* ).

- Si la conexión se pierde durante la fase de copia (por ejemplo, si el servidor se desconecta y se interrumpe la conexión), puede usar la **copia/z** para reanudar una vez que se haya restablecido la conexión. La opción **/z** también muestra el porcentaje de la operación de copia que se ha completado para cada archivo.

- Puede sustituir un nombre de dispositivo por una o varias apariciones del *origen* o del *destino*.

- Si el *destino* es un dispositivo (por ejemplo, COM1 o LPT1), la opción **/b** copia los datos en el dispositivo en modo binario. En el modo binario, **Copy/b** copia todos los caracteres (incluidos los caracteres especiales, como Ctrl + C, Ctrl + S, Ctrl + Z y entrar) en el dispositivo, como datos. Sin embargo, si se omite **/b**, los datos se copian en el dispositivo en modo ASCII. En el modo ASCII, los caracteres especiales pueden hacer que los archivos se combinen durante el proceso de copia.

- Si no especifica un archivo de destino, se crea una copia con el mismo nombre, fecha de modificación y hora de modificación que el archivo original. La nueva copia se almacena en el directorio actual de la unidad actual. Si el archivo de origen está en la unidad actual y en el directorio actual y no especifica una unidad o un directorio diferente para el archivo de destino, el comando de **copia** se detiene y muestra el mensaje de error siguiente:

    ```
    File cannot be copied onto itself
    0 File(s) copied
    ```

- Si especifica más de un archivo en el *origen*, el comando **copiar** los combina en un único archivo con el nombre de archivo especificado en *destino*. El comando **Copy** supone que los archivos combinados son archivos ASCII a menos que use la opción **/b** .

- Para copiar los archivos que tienen una longitud de 0 bytes, o para copiar todos los archivos y subdirectorios de un directorio, use el [comando xcopy](xcopy.md).

- Para asignar la fecha y hora actuales a un archivo sin modificar el archivo, use la sintaxis siguiente:  

    ```
    copy /b <source> +,,
    ```

    Donde las comas indican que el parámetro de *destino* se ha dejado involuntariamente.

## <a name="examples"></a>Ejemplos

Para copiar un archivo denominado *Memo. doc* en *Letter. doc* en la unidad actual y asegurarse de que el carácter de fin de archivo (Ctrl + Z) está al final del archivo copiado, escriba:

```
copy memo.doc letter.doc /a
```

Para copiar un archivo llamado *Robin. tip* desde la unidad actual y el directorio a un directorio existente denominado *pájaros* que se encuentra en la unidad C, escriba:

```
copy robin.typ c:\birds
```

> [!NOTE]
> Si el directorio *pájaros* no existe, el archivo *Robin. Typ* se copia en un archivo denominado *pájaros* que se encuentra en el directorio raíz del disco de la unidad C.

Para combinar *Mar89. RPT*, *Apr89. RPT*y *May89. RPT*, que se encuentran en el directorio actual, y colóquelos en un archivo denominado *Informe* (también en el directorio actual), escriba:

```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```

> [!NOTE]
> Si combina archivos, el comando **copiar** marca el archivo de destino con la fecha y hora actuales. Si omite el *destino*, los archivos se combinan y se almacenan con el nombre del primer archivo de la lista.

Para combinar todos los archivos del *Informe*, cuando ya exista un archivo con el nombre *Informe* , escriba:

```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```

Para combinar todos los archivos del directorio actual que tengan la extensión de nombre de archivo. txt en un único archivo denominado *Combined. doc*, escriba:

```
copy *.txt Combined.doc
```

Para combinar varios archivos binarios en un archivo mediante caracteres comodín, incluya **/b**. Esto evita que Windows considere CTRL + Z como un carácter de fin de archivo. Por ejemplo, escriba:

```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Si combina archivos binarios, el archivo resultante podría quedar inutilizable debido al formato interno.

- La combinación de cada archivo que tiene una extensión. txt con su archivo. Ref correspondiente crea un archivo con el mismo nombre de archivo, pero con una extensión. doc. El comando **Copy** combina *archivo1. txt* con *archivo1. Ref* para formar *archivo1. doc*y, a continuación, el comando combina *archivo2. txt* con *archivo2. Ref* para formar *archivo2. doc*, etc. Por ejemplo, escriba:

```
copy *.txt + *.ref *.doc
```

Para combinar todos los archivos con la extensión. txt y, a continuación, combinar todos los archivos con la extensión. Ref en un archivo denominado *Combined. doc*, escriba:

```
copy *.txt + *.ref Combined.doc
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)
