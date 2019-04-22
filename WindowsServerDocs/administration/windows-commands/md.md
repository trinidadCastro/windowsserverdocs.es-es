---
title: Md
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1396038410ecc5db5a124a1768038c4f8c8bea8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820846"
---
# <a name="md"></a>Md



Crea un directorio o subdirectorio.

> [!NOTE]
> Este comando es el mismo que el **mkdir** comando.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive>:|Especifica la unidad en la que desea crear el nuevo directorio.|
|\<Path>|Obligatorio. Especifica el nombre y la ubicación del nuevo directorio. La longitud máxima de una ruta de acceso viene determinada por el sistema de archivos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Las extensiones de comando, que están habilitadas de forma predeterminada, podrá usar una sola **md** comando para crear directorios intermedios en una ruta de acceso especificada.

## <a name="BKMK_examples"></a>Ejemplos

Para crear un directorio denominado Directory1 dentro del directorio actual, escriba:
```
md Directory1
```
Para crear el árbol de directorios Taxes\Property\Current dentro del directorio raíz, están habilitadas las extensiones de comando, escriba:
```
md \Taxes\Property\Current
```
Para crear el árbol de directorios Taxes\Property\Current dentro del directorio raíz del ejemplo anterior, pero con las extensiones de comando deshabilitadas, escriba la siguiente secuencia de comandos:
```
md \Taxes
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Cmd](cmd.md)