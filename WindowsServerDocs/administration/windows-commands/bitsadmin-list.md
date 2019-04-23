---
title: bitsadmin list
description: Tema de los comandos de Windows para **bitsadmin lista** -enumera los trabajos de transferencia de propiedad del usuario actual.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0b88001b9c4ae01b57006ffeef66dec0348ca77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873866"
---
# <a name="bitsadmin-list"></a>bitsadmin list



Enumera los trabajos de transferencia de propiedad del usuario actual.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ Allusers|Opcional: enumera los trabajos para todos los usuarios|
|/Verbose|Opcional: proporciona información detallada para cada trabajo.|

## <a name="remarks"></a>Comentarios

Debe tener privilegios de administrador para usar el parámetro /allusers

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera información sobre los trabajos que pertenecen al usuario actual.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)