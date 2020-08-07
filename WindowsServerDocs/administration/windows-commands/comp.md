---
title: comp
description: Artículo de referencia para el comando COMP, que compara el contenido de dos archivos o conjuntos de archivos byte a byte.
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18bd39483957959c746913a4ee18014be40c9eaa
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880031"
---
# <a name="comp"></a>comp

Compara el contenido de dos archivos o conjuntos de archivos byte a byte. Estos archivos se pueden almacenar en la misma unidad o en unidades diferentes, y en el mismo directorio o en directorios diferentes. Cuando este comando compara archivos, muestra su ubicación y sus nombres de archivo. Si se usa sin parámetros, **COMP** le pide que escriba los archivos que se van a comparar.

## <a name="syntax"></a>Sintaxis

```
comp [<data1>] [<data2>] [/d] [/a] [/l] [/n=<number>] [/c]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<data1>` | Especifica la ubicación y el nombre del primer archivo o conjunto de archivos que se van a comparar. Puede usar caracteres comodín (**&#42;** y **?**) para especificar varios archivos. |
| `<data2>` | Especifica la ubicación y el nombre del segundo archivo o conjunto de archivos que desea comparar. Puede usar caracteres comodín (**&#42;** y **?**) para especificar varios archivos. |
| /d | Muestra las diferencias en el formato decimal. (El formato predeterminado es hexadecimal). |
| /a | Muestra las diferencias como caracteres. |
| /l | Muestra el número de la línea donde se produce una diferencia, en lugar de mostrar el desplazamiento de bytes. |
| /n =`<number>` | Compara solo el número de líneas que se especifican para cada archivo, incluso si los archivos tienen tamaños diferentes. |
| /C | Realiza una comparación que no distingue entre mayúsculas y minúsculas. |
| /OFF [línea] | Procesa archivos con el conjunto de atributos sin conexión. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Observaciones

- Durante la comparación, **COMP** muestra mensajes que identifican las ubicaciones de información diferente entre los archivos. Cada mensaje indica la dirección de la memoria de desplazamiento de los bytes desiguales y el contenido de los bytes (en notación hexadecimal, a menos que se especifique el parámetro de línea de comandos **/a** o **/d** ). Los mensajes aparecen en el formato siguiente:

    ```
    Compare error at OFFSET xxxxxxxx
    file1 = xx
    file2 = xx
    ```

    Después de diez comparaciones desiguales, **COMP** deja de comparar los archivos y muestra el siguiente mensaje:

    `10 Mismatches - ending compare`

- Si omite los componentes necesarios de *data1* o *data2*, o si omite *data2* por completo, este comando le pedirá la información que falta.

- Si *data1* solo contiene una letra de unidad o un nombre de directorio sin nombre de archivo, este comando compara todos los archivos del directorio especificado con el archivo especificado en *data1*.

- Si *data2* solo contiene una letra de unidad o un nombre de directorio, el nombre de archivo predeterminado para *data2* se convierte en el mismo nombre que para *data1*.

- Si el comando **COMP** no encuentra los archivos especificados, aparecerá un mensaje que le indicará si desea comparar archivos adicionales.

- Los archivos que se comparan pueden tener el mismo nombre de archivo, siempre que estén en directorios diferentes o en unidades distintas. Puede usar caracteres comodín (**&#42;** y **?**) para especificar nombres de archivo.

- Debe especificar **/n** para comparar archivos de distintos tamaños. Si los tamaños de archivo son diferentes y no se especifica **/n** , se muestra el siguiente mensaje:

    ```
    Files are different sizes
    Compare more files (Y/N)?
    ```

    Para comparar estos archivos de todos modos, presione **N** para detener el comando. Después, vuelva a ejecutar el comando **COMP** con la opción **/n** para comparar solo la primera parte de cada archivo.

- Si usa caracteres comodín (**&#42;** y **?**) para especificar varios archivos, **COMP** busca el primer archivo que coincida con *data1* y lo compara con el archivo correspondiente en *data2*, si existe. El comando **COMP** informa de los resultados de la comparación de cada archivo que coincida con *data1*. Cuando termine, **COMP** mostrará el siguiente mensaje:

    `Compare more files (Y/N)?`

    Para comparar más archivos, presione **Y**. El comando **COMP** le pide las ubicaciones y los nombres de los nuevos archivos. Para detener las comparaciones, presione **N**. Al presionar **Y**, se le pedirán las opciones de línea de comandos que debe usar. Si no especifica ninguna opción de línea de comandos, **COMP** usa las que especificó antes.

## <a name="examples"></a>Ejemplos

Para comparar el contenido del directorio *c:\Informes* con el directorio de copia de seguridad `\\sales\backup\april` , escriba:

```
comp c:\reports \\sales\backup\april
```

Para comparar las diez primeras líneas de los archivos de texto en el directorio *\invoice* y mostrar el resultado en formato decimal, escriba:

```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)