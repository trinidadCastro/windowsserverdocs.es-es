---
title: add
description: Tema de referencia del comando Add, que agrega volúmenes al conjunto de volúmenes de los que se van a realizar instantáneas, o agrega alias al entorno de alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b621a3061c4e3366085c5cc44f91f26dd33d4e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719008"
---
# <a name="add"></a>add

Agrega volúmenes al conjunto de volúmenes de los que se van a realizar instantáneas o agrega alias al entorno de alias. Si se usa sin subcomandos, **Agregar** enumera los volúmenes y alias actuales.

> [!NOTE]
> Los alias no se agregan al entorno de alias hasta que se crea la instantánea. Los alias que necesite inmediatamente deben agregarse mediante **Agregar alias**.

## <a name="syntax"></a>Sintaxis

```
add
add volume <volume> [provider <providerid>]
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---------- | ----------- |
| volumen | Agrega un volumen al conjunto de instantáneas, que es el conjunto de volúmenes de los que se va a realizar la instantánea. Consulte [agregar volumen](add-volume.md) para ver la sintaxis y los parámetros. |
| alias | Agrega el nombre y el valor especificados al entorno de alias. Vea [Agregar alias](add-alias.md) para la sintaxis y los parámetros. |
| /? | Muestra la ayuda en la línea de comandos. |

## <a name="examples"></a>Ejemplos

Para mostrar los volúmenes agregados y los alias que se encuentran actualmente en el entorno, escriba:

```
add
```

La siguiente salida muestra que la unidad C se ha agregado al conjunto de instantáneas:

```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)