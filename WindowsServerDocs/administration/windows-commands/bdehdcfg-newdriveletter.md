---
title: bdehdcfg newdriveletter
description: El tema comandos de Windows para **bdehdcfg newdriveletter**, que asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a8757e7d0684912525817708fbe34953b049582
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851058"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter

Asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema. Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---------| ----------- |
|`<DriveLetter>`|Define la letra de unidad que se asignará a la unidad de destino especificada.|

## <a name="remarks"></a>Comentarios

Como práctica recomendada, se recomienda no asignar una letra de unidad a la unidad del sistema.

## <a name="examples"></a><a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra la unidad predeterminada a la que se asigna la letra de unidad P.

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)