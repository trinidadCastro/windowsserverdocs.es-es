---
title: pushd
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7866a54e83bd57c8689512a1b75b151f74cb93c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837098"
---
# <a name="pushd"></a>pushd



Almacena el directorio actual para que lo use el comando **popd** y, a continuación, cambia al directorio especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
pushd [<Path>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ruta de acceso >|Especifica el directorio en el que se va a crear el directorio actual. Este comando admite rutas de acceso relativas.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Cada vez que se usa el comando **Inserted** , se almacena un solo directorio para su uso. Sin embargo, puede almacenar varios directorios mediante el comando **Inserted** varias veces.

    Los directorios se almacenan secuencialmente en una pila virtual. Si usa el comando **insertado** una vez, el directorio en el que usa el comando se coloca en la parte inferior de la pila. Si vuelve a usar el comando, el segundo directorio se coloca en la parte superior de la primera. El proceso se repite cada vez que se usa el comando **Inserted** .

    Puede usar el comando **popd** para cambiar el directorio actual al directorio almacenado más recientemente por el comando **Inserted** . Si usa el comando **popd** , el directorio en la parte superior de la pila se quita de la pila y el directorio actual se cambia a ese directorio. Si vuelve a usar el comando **popd** , se quita el siguiente directorio de la pila.
-   Si se habilitan las extensiones de comando, el comando **Inserted** acepta una ruta de acceso de red o una letra de unidad local y la ruta de acceso.
-   Si especifica una ruta de acceso de red, el comando **Inserted** asigna temporalmente la letra de unidad no usada más alta (a partir de Z:) al recurso de red especificado. Después, el comando cambia la unidad y el directorio actuales al directorio especificado en la unidad recién asignada. Si usa el comando **popd** con las extensiones de comando habilitadas, el comando **popd** quita la asignación de la letra de unidad creada por la **extracción**.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se muestra cómo se puede usar el comando **Inserted** y el comando **popd** en un programa por lotes para cambiar el directorio actual de aquél en el que se ejecutó el programa de batch y, a continuación, cambiarlo de nuevo:
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