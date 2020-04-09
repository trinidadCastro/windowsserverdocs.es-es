---
title: add
description: Comando comandos de Windows para **Agregar**, que agrega volúmenes al conjunto de volúmenes del que se van a realizar instantáneas, o agrega alias al entorno de alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9895082cc10223fd08cff6916c20c3af5613e947
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851348"
---
# <a name="add"></a>add

agrega volúmenes al conjunto de volúmenes de los que se van a realizar instantáneas o agrega alias al entorno de alias. Si se usa sin subcomandos, **Agregar** enumera los volúmenes y alias actuales.

> [!NOTE]
> Los alias no se agregan al entorno de alias hasta que se crea la instantánea. Los alias que necesite inmediatamente deben agregarse mediante **Agregar alias**.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>Agregar subcomandos

| Subcomando | Descripción |
| ---------- | ----------- |
| volumen | Agrega un volumen al conjunto de instantáneas, que es el conjunto de volúmenes de los que se va a realizar la instantánea. Consulte [agregar volumen](add-volume.md) para ver la sintaxis y los parámetros. |
| alias | Agrega el nombre y el valor especificados al entorno de alias. Vea [Agregar alias](add-alias.md) para la sintaxis y los parámetros. |
| `/?` | Muestra la ayuda en la línea de comandos. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

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