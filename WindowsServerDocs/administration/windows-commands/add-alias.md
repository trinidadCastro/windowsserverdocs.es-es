---
title: Agregar alias
description: 'Tema de comandos de Windows para **Agregar alias** : agrega alias al entorno de alias.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2834376e497f54eadf1d9077e74f9c398202c5a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382816"
---
# <a name="add-alias"></a>Agregar alias



Agrega alias al entorno de alias. Si se usa sin parámetros, **Agregar alias** muestra la ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<AliasName >|Especifica el nombre del alias.|
|\<AliasValue >|Especifica el valor del alias.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Los alias se guardan en el archivo de metadatos y se cargarán con el comando **cargar metadatos** .

## <a name="BKMK_examples"></a>Example

Para enumerar todas las sombras, incluidos sus alias, escriba:
```
list shadows all
```
En el siguiente fragmento se muestra una instantánea a la que se ha asignado el alias predeterminado, VSS_SHADOW_x:
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
Para asignar un nuevo alias con el nombre System1 a esta instantánea, escriba:
```
add alias System1 %VSS_SHADOW_1%
```
Como alternativa, puede asignar el alias mediante el identificador de instantánea:
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)