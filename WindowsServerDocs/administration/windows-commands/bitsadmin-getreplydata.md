---
title: bitsadmin getreplydata
description: En el tema comandos de Windows para **bitsadmin getreplydata** se recuperan los datos de respuesta del servidor en formato hexadecimal.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ebd3ee77e5d442467f49bb209c560f089f2271b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381277"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera los datos de respuesta del servidor en formato hexadecimal.

**BITS 1,2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Válido solo para trabajos de carga y respuesta.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recuperan los datos de respuesta del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)