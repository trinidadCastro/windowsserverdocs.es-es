---
title: list
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aacc93e1c7a16a7327ddbd17515f19cf41a5b458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825546"
---
# <a name="list"></a>list



Los escritores de listas, las instantáneas o proveedores de instantáneas actualmente registrados que se encuentran en el sistema. Si se utiliza sin parámetros, **lista** muestra la Ayuda en el símbolo del sistema.

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
|escritores|Enumera los escritores. Consulte [List writers](list-writers.md) para la sintaxis y los parámetros.|
|shadows|Se enumeran las instantáneas no persistente de persistente y existente. Consulte [lista sombras](list-shadows.md) para la sintaxis y los parámetros.|
|proveedores|Las listas de los proveedores de instantáneas registrados actualmente. Consulte [enumerar proveedores](list-providers.md) para la sintaxis y los parámetros.|

## <a name="BKMK_examples"></a>Ejemplos

Para obtener una lista de todas las instantáneas, escriba:
```
list shadows all
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)