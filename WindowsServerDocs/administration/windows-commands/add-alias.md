---
title: add alias
description: Artículo de referencia para el comando Agregar alias, que agrega alias al entorno de alias.
ms.topic: reference
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1cc9b7c5395c29ac917a0b25b97fb091dc66a699
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633555"
---
# <a name="add-alias"></a>add alias

Agrega alias al entorno de alias. Si se usa sin parámetros, **Agregar alias** muestra la ayuda en el símbolo del sistema. Los alias se guardan en el archivo de metadatos y se cargarán con el comando **cargar metadatos** .

## <a name="syntax"></a>Sintaxis

```
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<aliasname>` | Especifica el nombre del alias. |
| `<aliasvalue>` | Especifica el valor del alias. |
| `? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para enumerar todas las sombras, incluidos sus alias, escriba:

```
list shadows all
```

En el siguiente fragmento se muestra una instantánea a la que se ha asignado el alias predeterminado, *VSS_SHADOW_x*:

```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```

Para asignar un nuevo alias con el nombre *System1* a esta instantánea, escriba:

```
add alias System1 %VSS_SHADOW_1%
```

Como alternativa, puede asignar el alias mediante el identificador de instantánea:

```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando cargar metadatos](load-metadata.md)
