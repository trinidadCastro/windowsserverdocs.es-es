---
title: Agregar alias
description: Tema de los comandos de Windows para **agregar alias** -agrega alias para el entorno de alias.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 50de932ea0153546816face61f0852a08707ea85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862226"
---
# <a name="add-alias"></a>Agregar alias



Agrega los alias en el entorno de alias. Si se utiliza sin parámetros, **agregar alias** muestra la Ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<AliasName>|Especifica el nombre del alias.|
|\<AliasValue>|Especifica el valor del alias.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Los alias se guardan en el archivo de metadatos y se cargará con la **cargar metadatos** comando.

## <a name="BKMK_examples"></a>Ejemplos

Para obtener una lista de todas las sombras, incluidos sus alias, escriba:
```
list shadows all
```
El extracto siguiente muestra una instantánea a la que se asignó el alias predeterminado, VSS_SHADOW_x:
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
Para asignar un nuevo alias con el nombre System1 a esta instantánea, escriba:
```
add alias System1 %VSS_SHADOW_1%
```
Como alternativa, puede asignar el alias utilizando el identificador de instantánea:
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)