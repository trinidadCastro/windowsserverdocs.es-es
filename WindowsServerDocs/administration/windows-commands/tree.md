---
title: tree
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de22de1c9d62ba79c1aa68248109cca88009703a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872636"
---
# <a name="tree"></a>tree



Muestra gráficamente la estructura de directorios de una ruta de acceso o del disco en una unidad.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive>:|Especifica la unidad que contiene el disco para el que desea mostrar la estructura de directorios.|
|\<Path>|Especifica el directorio para el que desea mostrar la estructura de directorios.|
|/f|Muestra los nombres de los archivos en cada directorio.|
|/a|Especifica que **árbol** consiste en usar caracteres de texto en lugar de caracteres gráficos para mostrar las líneas que se vinculan los subdirectorios.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

La estructura mostrada por **árbol** depende de los parámetros que especifique en el símbolo del sistema. Si no especifica una unidad o ruta de acceso, **árbol** muestra el principio de la estructura de árbol con el directorio actual de la unidad actual.

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar los nombres de todos los subdirectorios en el disco en la unidad actual, escriba:
```
tree \
```
Para mostrar una pantalla a la vez, los archivos de todos los directorios en la unidad C, escriba:
```
tree c:\ /f | more 
```
Para imprimir una lista de todos los directorios en la unidad C, escriba:
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)