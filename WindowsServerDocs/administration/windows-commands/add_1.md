---
title: agregar
description: 'Temas de comandos de Windows para **add_1** : agrega volúmenes al conjunto de volúmenes del que se van a realizar instantáneas, o agrega alias al entorno de alias.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1aaa211938d14a0019d29e64867f4df2475a877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382782"
---
# <a name="add"></a>agregar


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

|Subcomando|Descripción|
|----------|-----------|
|volumen|Agrega un volumen al conjunto de instantáneas, que es el conjunto de volúmenes de los que se va a realizar la instantánea. Consulte [agregar volumen](add-volume.md) para ver la sintaxis y los parámetros.|
|alias|Agrega el nombre y el valor especificados al entorno de alias. Vea [Agregar alias](add-alias.md) para la sintaxis y los parámetros.|
|/?|Muestra la ayuda en la línea de comandos.|

## <a name="BKMK_examples"></a>Example

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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)