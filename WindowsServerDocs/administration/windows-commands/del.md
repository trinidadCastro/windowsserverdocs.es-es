---
title: del
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b10da1a6035155d525a516f35f83a25209e90075
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433888"
---
# <a name="del"></a>del



Elimina uno o varios archivos. Este comando es el mismo que el **borrar** comando.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Los nombres >|Especifica una lista de uno o varios archivos o directorios. Para eliminar varios archivos, se pueden usar caracteres comodín. Si se especifica un directorio, se eliminarán todos los archivos dentro del directorio.|
|/p|Solicita confirmación antes de eliminar el archivo especificado.|
|/f|Fuerza la eliminación de archivos de solo lectura.|
|/s|Elimina los archivos desde el directorio actual y todos los subdirectorios especificados. Muestra los nombres de los archivos como que se están eliminando.|
|/q|Especifica el modo silencioso. No se le pida confirmación de eliminación.|
|/ a [::]\<atributos >|Elimina archivos basados en los atributos de archivo siguientes:</br>**r** archivos de solo lectura</br>**h** archivos ocultos</br>**no** archivos indizados del contenido</br>**s** archivos del sistema</br>**un** archivos listos para archivar</br>**l** puntos de reanálisis</br>-Prefix lo que significa "no"|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

> [!CAUTION]
> Si usas **SUPR** para eliminar un archivo desde el disco, no podrá recuperarla.
> -   Si usas **/p**, **SUPR** muestra el nombre de un archivo y envía el mensaje siguiente:

    `FileName, Delete (Y/N)?`

    To confirm the deletion, press Y. To cancel the deletion and display the next file name (that is, if you specified a group of files), press N. To stop the **del** command, press CTRL+C.
- Si deshabilita las extensiones de comando, **/s** muestra los nombres de los archivos que no se encontraron en lugar de mostrar los nombres de archivos que se va a eliminar (es decir, se invierte el comportamiento).
- Si especifica una carpeta en *nombres*, se eliminan todos los archivos en la carpeta. Por ejemplo, el siguiente comando elimina todos los archivos de la carpeta \Work:  
  ```
  del \work
  ```  
- Puede usar caracteres comodín ( **&#42;** y **?** ) para eliminar más de un archivo a la vez. Sin embargo, para evitar eliminar archivos involuntariamente, debe usar caracteres comodín con precaución con el **SUPR** comando. Por ejemplo, si escribe el siguiente comando:  
  ```
  del *.*
  ```  
  El **SUPR** comando muestra el símbolo del sistema siguiente:

  `Are you sure (Y/N)?`

  Para eliminar todos los archivos en el directorio actual, presione s y, a continuación, presione ENTRAR. Para cancelar la eliminación, presione N y, a continuación, presione ENTRAR.

> [!NOTE]
> Antes de usar caracteres comodín con el **SUPR** comando, use los mismos caracteres comodín con el **dir** comando para enumerar todos los archivos que se eliminará.
> -   El **SUPR** comando, con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Ejemplos

Para eliminar todos los archivos en una carpeta llamada Test en la unidad C, escriba lo siguiente:
```
del c:\test
del c:\test\*.*
```
Para eliminar todos los archivos con la extensión de nombre de archivo .bat desde el directorio actual, escriba:
```
del *.bat
```
Para eliminar todos los archivos de solo lectura en el directorio actual, escriba:
```
del /a:r *.*
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
