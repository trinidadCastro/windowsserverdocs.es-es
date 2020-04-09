---
title: Agregar alias
description: El tema comandos de Windows para **Agregar alias**, que agrega alias al entorno de alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebffc1504f502711dab30f6f9b120ad20e64ae9d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851368"
---
# <a name="add-alias"></a>Agregar alias

Agrega alias al entorno de alias. Si se usa sin parámetros, **Agregar alias** muestra la ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
add alias <AliasName> <AliasValue>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|`<AliasName>`|Especifica el nombre del alias.|
|`<AliasValue>`|Especifica el valor del alias.|
|`/?`|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Los alias se guardan en el archivo de metadatos y se cargarán con el comando **cargar metadatos** .

## <a name="examples"></a><a name=BKMK_examples></a>Example

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

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)