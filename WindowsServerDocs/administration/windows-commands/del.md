---
title: del
description: Artículo de referencia para el comando del, que elimina uno o varios archivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe1ae558da0a4cb19159c68c67e1e72970e3b502
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958417"
---
# <a name="del"></a>del

Elimina uno o varios archivos. Este comando realiza las mismas acciones que el comando **Erase** .

El comando **del** también se puede ejecutar desde la consola de recuperación de Windows, con parámetros diferentes. Para obtener más información, vea [entorno de recuperación de Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

> [!WARNING]
> Si usa **Supr** para eliminar un archivo del disco, no podrá recuperarlo.

## <a name="syntax"></a>Sintaxis

```
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
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

- Si usa el `del /p` comando, verá el mensaje siguiente:

    `FileName, Delete (Y/N)?`

    Para confirmar la eliminación, presione **Y**. Para cancelar la eliminación y mostrar el siguiente nombre de archivo (si ha especificado un grupo de archivos), presione **N**. Para detener el comando **del** , presione Ctrl + C.

- Si deshabilita la extensión de comando, el parámetro **/s** mostrará los nombres de los archivos que no se hayan encontrado, en lugar de mostrar los nombres de los archivos que se van a eliminar.

- Si especifica carpetas específicas en el `<names>` parámetro, también se eliminarán todos los archivos incluidos. Por ejemplo, si desea eliminar todos los archivos de la carpeta *\work* , escriba:

  ```
  del \work
  ```

- Puede usar caracteres comodín (**&#42;** y **?**) para eliminar más de un archivo a la vez. Sin embargo, para evitar la eliminación accidental de archivos, debe usar caracteres comodín con precaución. Por ejemplo, si escribe el comando siguiente:

  ```
  del *.*
  ```

  El comando **Supr** muestra el siguiente mensaje:

  `Are you sure (Y/N)?`

  Para eliminar todos los archivos del directorio actual, presione **y** y, a continuación, presione Entrar. Para cancelar la eliminación, presione **N** y, a continuación, presione Entrar.

  > [!NOTE]
  > Antes de usar caracteres comodín con el comando **del** , use los mismos caracteres comodín con el comando **dir** para enumerar todos los archivos que se van a eliminar.

## <a name="examples"></a>Ejemplos

Para eliminar todos los archivos de una carpeta denominada test en la unidad C, escriba cualquiera de los siguientes:

```
del c:\test
del c:\test\*.*
```

Para eliminar todos los archivos con la extensión de nombre de archivo. bat del directorio actual, escriba:

```
del *.bat
```

Para eliminar todos los archivos de solo lectura en el directorio actual, escriba:

```
del /a:r *.*
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Entorno de recuperación de Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
