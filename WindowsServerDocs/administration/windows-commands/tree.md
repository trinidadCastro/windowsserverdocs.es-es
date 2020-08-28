---
title: tree
description: Artículo de referencia para el árbol, que muestra la estructura de directorios de una ruta de acceso, o del disco de una unidad, de forma gráfica.
ms.topic: reference
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c164fe8999313ffd40ec12b29c7ad8cf2bd7c7a5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026893"
---
# <a name="tree"></a>tree

Muestra gráficamente la estructura de directorios de una ruta de acceso o del disco de una unidad.



## <a name="syntax"></a>Sintaxis

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive>:|Especifica la unidad que contiene el disco para el que desea mostrar la estructura de directorios.|
|\<Path>|Especifica el directorio para el que desea mostrar la estructura de directorios.|
|/f|Muestra los nombres de los archivos de cada directorio.|
|/a|Especifica que el **árbol** va a utilizar caracteres de texto en lugar de caracteres gráficos para mostrar las líneas que vinculan a los subdirectorios.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

La estructura que muestra el **árbol** depende de los parámetros que se especifiquen en el símbolo del sistema. Si no especifica una unidad o una ruta de acceso, **árbol** muestra la estructura de árbol que empieza con el directorio actual de la unidad actual.

## <a name="examples"></a>Ejemplos

Para mostrar los nombres de todos los subdirectorios del disco en la unidad actual, escriba:
```
tree \
```
Para mostrar una pantalla a la vez, los archivos de todos los directorios de la unidad C, escriba:
```
tree c:\ /f | more
```
Para imprimir una lista de todos los directorios de la unidad C, escriba:
```
tree c:\ /f  prn
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)