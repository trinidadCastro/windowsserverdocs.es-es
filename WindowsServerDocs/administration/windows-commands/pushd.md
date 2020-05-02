---
title: pushd
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e64c4f5090183b7d7b29dc7e040ffd94dc9d57ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722771"
---
# <a name="pushd"></a>pushd



Almacena el directorio actual para que lo use el comando **popd** y, a continuación, cambia al directorio especificado.



## <a name="syntax"></a>Sintaxis

```
pushd [<Path>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Ruta de acceso>|Especifica el directorio en el que se va a crear el directorio actual. Este comando admite rutas de acceso relativas.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Cada vez que se usa el comando **Inserted** , se almacena un solo directorio para su uso. Sin embargo, puede almacenar varios directorios mediante el comando **Inserted** varias veces.

    Los directorios se almacenan secuencialmente en una pila virtual. Si usa el comando **insertado** una vez, el directorio en el que usa el comando se coloca en la parte inferior de la pila. Si vuelve a usar el comando, el segundo directorio se coloca en la parte superior de la primera. El proceso se repite cada vez que se usa el comando **Inserted** .

    Puede usar el comando **popd** para cambiar el directorio actual al directorio almacenado más recientemente por el comando **Inserted** . Si usa el comando **popd** , el directorio en la parte superior de la pila se quita de la pila y el directorio actual se cambia a ese directorio. Si vuelve a usar el comando **popd** , se quita el siguiente directorio de la pila.
-   Si se habilitan las extensiones de comando, el comando **Inserted** acepta una ruta de acceso de red o una letra de unidad local y la ruta de acceso.
-   Si especifica una ruta de acceso de red, el comando **Inserted** asigna temporalmente la letra de unidad no usada más alta (a partir de Z:) al recurso de red especificado. Después, el comando cambia la unidad y el directorio actuales al directorio especificado en la unidad recién asignada. Si usa el comando **popd** con las extensiones de comando habilitadas, el comando **popd** quita la asignación de la letra de unidad creada por la **extracción**.

## <a name="examples"></a>Ejemplos

Para ver cómo puede usar el comando de **Inserted** y el comando **popd** en un programa de batch para cambiar el directorio actual de aquél en el que se ejecutó el programa de batch y, a continuación, cámbielo de nuevo:
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

[Popd](popd.md)