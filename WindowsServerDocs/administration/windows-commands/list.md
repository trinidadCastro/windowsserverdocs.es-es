---
title: list
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91b42925fc822b10157bb488167d06fe82cfe1e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374702"
---
# <a name="list"></a>list



enumera escritores, instantáneas o proveedores de instantáneas registrados actualmente en el sistema. Si se usa sin parámetros, **List** muestra la ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|editores|Enumera escritores. Vea [lista de escritores](list-writers.md) para ver la sintaxis y los parámetros.|
|Las|Muestra las instantáneas no persistentes persistentes y existentes. Vea [lista de sombras](list-shadows.md) para ver la sintaxis y los parámetros.|
|Presta|Enumera los proveedores de instantáneas registrados actualmente. Consulte la [lista de proveedores](list-providers.md) para ver la sintaxis y los parámetros.|

## <a name="BKMK_examples"></a>Example

Para enumerar todas las instantáneas, escriba:
```
list shadows all
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)