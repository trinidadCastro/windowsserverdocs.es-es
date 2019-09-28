---
title: Md
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 965a5c506535a2c52d6cc7b3557c6104182c12a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373688"
---
# <a name="md"></a>Md



Crea un directorio o subdirectorio.

> [!NOTE]
> Este comando es el mismo que el comando **mkdir** .

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Drive >:|Especifica la unidad en la que desea crear el nuevo directorio.|
|@no__t 0Path >|Obligatorio. Especifica el nombre y la ubicación del nuevo directorio. El sistema de archivos determina la longitud máxima de cualquier ruta de acceso única.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Las extensiones de comandos, que están habilitadas de forma predeterminada, permiten usar un único comando **MD** para crear directorios intermedios en una ruta de acceso especificada.

## <a name="BKMK_examples"></a>Example

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
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Cmd](cmd.md)