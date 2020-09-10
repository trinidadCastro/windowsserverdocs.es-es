---
title: erase
description: Artículo de referencia del comando Erase, que elimina uno o varios archivos.
ms.topic: reference
ms.assetid: 024a4d0f-8679-4e06-b46f-61fdaf5464bc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5545e63efc87527506704ecd6ff956c8000b95a5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636090"
---
# <a name="erase"></a>erase

Elimina uno o varios archivos. Si usa **Borrar** para eliminar un archivo del disco, no podrá recuperarlo.

> [!NOTE]
> Este comando es igual que el [comando del](del.md).


## <a name="syntax"></a>Sintaxis

```
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<names>` | Especifica una lista de uno o varios archivos o directorios. Se pueden usar caracteres comodín para eliminar varios archivos. Si se especifica un directorio, se eliminarán todos los archivos del directorio. |
| /p | Solicita confirmación antes de eliminar el archivo especificado. |
| /f | Fuerza la eliminación de archivos de solo lectura. |
| /s | Elimina los archivos especificados del directorio actual y todos los subdirectorios. Muestra los nombres de los archivos que se van a eliminar. |
| /q | Especifica el modo silencioso. No se le pedirá confirmación de eliminación. |
| /a [:]`<attributes>` | Elimina archivos en función de los siguientes atributos de archivo:<ul><li>archivos de solo lectura de **r**</li><li>**h** archivos ocultos</li><li>**no hay** archivos indizados de contenido</li><li>archivos **del sistema**</li><li>**archivos listos** para archivar</li><li>**l** puntos de reanálisis</li><li>**-** Se usa como significado de prefijo ' not '</li></ul>. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si usa el `erase /p` comando, verá el mensaje siguiente:

    `FileName, Delete (Y/N)?`

    Para confirmar la eliminación, presione **Y**. Para cancelar la eliminación y mostrar el siguiente nombre de archivo (si ha especificado un grupo de archivos), presione **N**. Para detener el comando de **borrado** , presione Ctrl + C.

- Si deshabilita la extensión de comando, el parámetro **/s** mostrará los nombres de los archivos que no se hayan encontrado, en lugar de mostrar los nombres de los archivos que se van a eliminar.

- Si especifica carpetas específicas en el `<names>` parámetro, también se eliminarán todos los archivos incluidos. Por ejemplo, si desea eliminar todos los archivos de la carpeta *\work* , escriba:

  ```
  erase \work
  ```

- Puede usar caracteres comodín (**&#42;** y **?**) para eliminar más de un archivo a la vez. Sin embargo, para evitar la eliminación accidental de archivos, debe usar caracteres comodín con precaución. Por ejemplo, si escribe el comando siguiente:

  ```
  erase *.*
  ```

  El comando **Erase** muestra el siguiente mensaje:

  `Are you sure (Y/N)?`

  Para eliminar todos los archivos del directorio actual, presione **y** y, a continuación, presione Entrar. Para cancelar la eliminación, presione **N** y, a continuación, presione Entrar.

  > [!NOTE]
  > Antes de usar caracteres comodín con el comando **Erase** , use los mismos caracteres comodín con el comando **dir** para enumerar todos los archivos que se van a eliminar.

### <a name="examples"></a>Ejemplos

Para eliminar todos los archivos de una carpeta denominada test en la unidad C, escriba cualquiera de los siguientes:

```
erase c:\test
erase c:\test\*.*
```

Para eliminar todos los archivos con la extensión de nombre de archivo. bat del directorio actual, escriba:

```
erase *.bat
```

Para eliminar todos los archivos de solo lectura en el directorio actual, escriba:

```
erase /a:r *.*
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [del comando](del.md)