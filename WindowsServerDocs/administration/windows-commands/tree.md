---
title: árbol
description: Tema de referencia sobre el árbol, que muestra la estructura de directorios de una ruta de acceso, o del disco de una unidad, de forma gráfica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94b0429dadc3965c7e41ad5aa881fc902988ec9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721284"
---
# <a name="tree"></a>árbol

Muestra gráficamente la estructura de directorios de una ruta de acceso o del disco de una unidad.



## <a name="syntax"></a>Sintaxis

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad:|Especifica la unidad que contiene el disco para el que desea mostrar la estructura de directorios.|
|\<Ruta de acceso>|Especifica el directorio para el que desea mostrar la estructura de directorios.|
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