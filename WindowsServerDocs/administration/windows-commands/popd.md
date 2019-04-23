---
title: popd
description: Obtenga información sobre cómo cambiar el directorio al directorio almacenado más recientemente por el comando pushd.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6da6dc9d1fc2d8965f8a081831cb1150375209a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827466"
---
# <a name="popd"></a>popd

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el directorio actual al directorio que se almacenó más recientemente por el **pushd** comando.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis
```
popd
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Cada vez que usa el **pushd** comando, un único directorio se almacena para su uso. Sin embargo, puede almacenar varios directorios mediante la **pushd** comando varias veces.
    Los directorios se almacenan secuencialmente en una pila virtual. Si usas el **pushd** comando una vez que el directorio en el que se use el comando se coloca en la parte inferior de la pila. Si usa el comando de nuevo, el segundo directorio se coloca en la primera de ellas. El proceso se repite cada vez que usa el **pushd** comando.
    Puede usar el **popd** comando para cambiar el directorio actual en el directorio almacenado más recientemente por el **pushd** comando. Si usas el **popd** de comandos, se quita el directorio en la parte superior de la pila de la pila y se cambia el directorio actual a ese directorio. Si usas el **popd** comando nuevo, se quita el directorio siguiente en la pila.
-   Cuando se habilitan las extensiones de comando, el **popd** comando quita las asignaciones de letras de unidad creados por **pushd**.

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

## <a name="additional-references"></a>Referencias adicionales
-   [pushd](pushd.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

