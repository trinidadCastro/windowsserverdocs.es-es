---
title: Bitsadmin gettemporaryname
description: Tema de los comandos de Windows para **gettemporaryname bitsadmin** -notifica el nombre de archivo temporal del archivo determinado dentro del trabajo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 762a2a5943202b38e94a245b74745e6631e0792d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876716"
---
# <a name="bitsadmin-gettemporaryname"></a>Bitsadmin gettemporaryname



Notifica el nombre de archivo temporal del archivo determinado dentro del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Índice de archivo|Comienza por 0|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se notifica el nombre de archivo temporal de archivo 2 del trabajo denominado *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)