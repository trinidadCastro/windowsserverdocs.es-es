---
title: bdehdcfg newdriveletter
description: Tema de los comandos de Windows para bdehdcfg newdriveletter - asigna una letra de unidad nueva a la parte de una unidad utilizada como unidad del sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd40942dfb724d46c0fa9a43c4646e1db09d2a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887146"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter



Asigna una letra de unidad nueva a la parte de una unidad utilizada como unidad del sistema. Para obtener un ejemplo de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DriveLetter>|Define la letra de unidad que se asignará a la unidad de destino especificado.|

## <a name="remarks"></a>Comentarios

Como práctica recomendada, se recomienda no asignar una letra de unidad a la unidad del sistema.

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente muestra la unidad predeterminada que se asigna la letra de unidad, P.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)