---
title: del
description: Tema de referencia de del, que elimina uno o varios archivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 703597ae422518a5b401b656ace0b4cd73418be8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716745"
---
# <a name="del"></a>del

Elimina uno o varios archivos. Este comando es el mismo que el comando **Erase** .



## <a name="syntax"></a>Sintaxis

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Nombres>|Especifica una lista de uno o varios archivos o directorios. Se pueden usar caracteres comodín para eliminar varios archivos. Si se especifica un directorio, se eliminarán todos los archivos del directorio.|
|/p|Solicita confirmación antes de eliminar el archivo especificado.|
|/f|Fuerza la eliminación de archivos de solo lectura.|
|/s|Elimina los archivos especificados del directorio actual y todos los subdirectorios. Muestra los nombres de los archivos que se van a eliminar.|
|/q|Especifica el modo silencioso. No se le pedirá confirmación de eliminación.|
|/a [:]\<atributos>|Elimina archivos en función de los siguientes atributos de archivo:</br>archivos de solo lectura de **r**</br>**h** archivos ocultos</br>**no hay** archivos indizados de contenido</br>archivos **del sistema**</br>**archivos listos** para archivar</br>**l** puntos de reanálisis</br>-El significado del prefijo ' not '|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

> [!CAUTION]
> Si usa **Supr** para eliminar un archivo del disco, no puede recuperarlo.

-   Si usa **/p** **, el** muestra el nombre de un archivo y envía el mensaje siguiente:

    `FileName, Delete (Y/N)?`

    Para confirmar la eliminación, presione Y. Para cancelar la eliminación y mostrar el siguiente nombre de archivo (es decir, si especificó un grupo de archivos), presione N. Para detener el comando **del** , presione Ctrl + C.
- Si deshabilita las extensiones de comando, **/s** muestra los nombres de los archivos que no se han encontrado en lugar de mostrar los nombres de los archivos que se están eliminando (es decir, se invierte el comportamiento).
- Si especifica una carpeta en *nombres*, se eliminarán todos los archivos de la carpeta. Por ejemplo, el siguiente comando elimina todos los archivos de la carpeta \Work:  
  ```
  del \work
  ```  
- Puede usar caracteres comodín (**&#42;** y **?**) para eliminar más de un archivo a la vez. Sin embargo, para evitar la eliminación accidental de archivos, debe usar caracteres comodín con precaución con el comando **del** . Por ejemplo, si escribe el comando siguiente:  
  ```
  del *.*
  ```  
  El comando **Supr** muestra el siguiente mensaje:

  `Are you sure (Y/N)?`

  Para eliminar todos los archivos del directorio actual, presione Y y, a continuación, presione Entrar. Para cancelar la eliminación, presione N y, a continuación, presione Entrar.

> [!NOTE]
> Antes de usar caracteres comodín con el comando **del** , use los mismos caracteres comodín con el comando **dir** para enumerar todos los archivos que se van a eliminar.

-   El comando **del** , con diferentes parámetros, está disponible en la consola de recuperación.

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
