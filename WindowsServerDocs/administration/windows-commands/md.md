---
title: Md
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a605571fb74af99d0f365a100dd33fd4db0d3f22
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724005"
---
# <a name="md"></a>Md



Crea un directorio o subdirectorio.

> [!NOTE]
> Este comando es el mismo que el comando **mkdir** .



## <a name="syntax"></a>Sintaxis

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de unidad:|Especifica la unidad en la que desea crear el nuevo directorio.|
|\<Ruta de acceso>|Necesario. Especifica el nombre y la ubicación del nuevo directorio. El sistema de archivos determina la longitud máxima de cualquier ruta de acceso única.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Las extensiones de comandos, que están habilitadas de forma predeterminada, permiten usar un único comando **MD** para crear directorios intermedios en una ruta de acceso especificada.

## <a name="examples"></a>Ejemplos

Para crear un directorio denominado Directory1 en el directorio actual, escriba:
```
md Directory1
```
Para crear el árbol de directorios Taxes\Property\Current en el directorio raíz, con las extensiones de comando habilitadas, escriba:
```
md \Taxes\Property\Current
```
Para crear el árbol de directorios Taxes\Property\Current en el directorio raíz como en el ejemplo anterior, pero con extensiones de comando deshabilitadas, escriba la siguiente secuencia de comandos:
```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Cmd](cmd.md)