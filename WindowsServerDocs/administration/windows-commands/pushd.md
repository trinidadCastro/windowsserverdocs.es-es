---
title: pushd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 548f39921c1f6aa3837e6443e396922396eb84f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887466"
---
# <a name="pushd"></a>pushd



Almacena el directorio actual para su uso por el **popd** comando y, a continuación, cambios en el directorio especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
pushd [<Path>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Path>|Especifica el directorio para que el directorio actual. Este comando admite rutas de acceso relativas.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Cada vez que usa el **pushd** comando, un único directorio se almacena para su uso. Sin embargo, puede almacenar varios directorios mediante la **pushd** comando varias veces.

    Los directorios se almacenan secuencialmente en una pila virtual. Si usas el **pushd** comando una vez que el directorio en el que se use el comando se coloca en la parte inferior de la pila. Si usa el comando de nuevo, el segundo directorio se coloca en la primera de ellas. El proceso se repite cada vez que usa el **pushd** comando.

    Puede usar el **popd** comando para cambiar el directorio actual en el directorio almacenado más recientemente por el **pushd** comando. Si usas el **popd** de comandos, se quita el directorio en la parte superior de la pila de la pila y se cambia el directorio actual a ese directorio. Si usas el **popd** comando nuevo, se quita el directorio siguiente en la pila.
-   Si se habilitan las extensiones de comando, el **pushd** comandos acepta una ruta de acceso de red o una ruta de acceso y una letra de unidad local.
-   Si especifica una ruta de acceso de red, el **pushd** comando temporalmente asigna la letra de unidad no utilizada más alta (comenzando por Z:) en el recurso de red especificado. A continuación, el comando cambia la unidad actual y el directorio en el directorio especificado en la unidad recién asignada. Si usas el **popd** comando con las extensiones de comando habilitadas, el **popd** comando quita la asignación de la letra de unidad creado por **pushd**.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente muestra cómo puede usar el **pushd** comando y el **popd** comando en un programa por lotes para cambiar el directorio actual de la que en el que se ejecutó el programa por lotes y, a continuación, cambiarla de nuevo:
```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Popd](popd.md)