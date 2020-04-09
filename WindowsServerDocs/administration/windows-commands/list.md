---
title: lista
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d60c42b868a1e9a26e3168e4b489573f9f87e179
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841108"
---
# <a name="list"></a>lista



Enumera escritores, instantáneas o proveedores de instantáneas registrados actualmente en el sistema. Si se usa sin parámetros, **List** muestra la ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|escritores|Enumera escritores. Vea [lista de escritores](list-writers.md) para ver la sintaxis y los parámetros.|
|las|Muestra las instantáneas no persistentes persistentes y existentes. Vea [lista de sombras](list-shadows.md) para ver la sintaxis y los parámetros.|
|proveedores|Enumera los proveedores de instantáneas registrados actualmente. Consulte la [lista de proveedores](list-providers.md) para ver la sintaxis y los parámetros.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para enumerar todas las instantáneas, escriba:
```
list shadows all
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)