---
title: pushd
description: Artículo de referencia para el comando Inserted, que almacena el directorio actual para que lo use el comando popd y, a continuación, cambia al directorio especificado.
ms.topic: reference
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 383f3f6da4fc171629350eaa9257ffe9759cca8e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033803"
---
# <a name="pushd"></a>pushd

Almacena el directorio actual para que lo use el comando **popd** y, a continuación, cambia al directorio especificado.

Cada vez que se usa el comando **Inserted** , se almacena un solo directorio para su uso. Sin embargo, puede almacenar varios directorios mediante el comando **Inserted** varias veces. Los directorios se almacenan secuencialmente en una pila virtual, por lo que si usa el comando **insertado** una vez, el directorio en el que se usa el comando se coloca en la parte inferior de la pila. Si vuelve a usar el comando, el segundo directorio se coloca en la parte superior de la primera. El proceso se repite cada vez que se usa el comando **Inserted** .

Si usa el comando **popd** , se quita el directorio de la parte superior de la pila y se cambia el directorio actual a ese directorio. Si vuelve a usar el comando **popd** , se quita el siguiente directorio de la pila. Si se habilitan las extensiones de comando, el comando **popd** quita todas las asignaciones de letra de unidad creadas por el comando **Inserted** .

## <a name="syntax"></a>Sintaxis

```
pushd [<path>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<path>` | Especifica el directorio en el que se va a crear el directorio actual. Este comando admite rutas de acceso relativas. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si se habilitan las extensiones de comando, el comando **Inserted** acepta una ruta de acceso de red o una letra de unidad local y la ruta de acceso.

- Si especifica una ruta de acceso de red, el comando **Inserted** asigna temporalmente la letra de unidad no usada más alta (a partir de Z:) al recurso de red especificado. Después, el comando cambia la unidad y el directorio actuales al directorio especificado en la unidad recién asignada. Si usa el comando **popd** con las extensiones de comando habilitadas, el comando **popd** quita la asignación de la letra de unidad creada por la **extracción**.

### <a name="examples"></a>Ejemplos

Para cambiar el directorio actual de aquél en el que se ejecutó el programa de batch y, a continuación, cámbielo de nuevo:

```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [popd (comando)](popd.md)
