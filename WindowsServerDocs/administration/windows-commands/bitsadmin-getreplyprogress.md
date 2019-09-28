---
title: bitsadmin getreplyprogress
description: 'Temas de comandos de Windows para **bitsadmin getreplyprogress** : recupera el tamaño y el progreso de la respuesta del servidor.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c791fe98271b497e5ecf48338ab3bbb0cc50de98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381240"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Recupera el tamaño y el progreso de la respuesta del servidor.

**BITS 1,2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Válido solo para trabajos de carga y respuesta.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el progreso de la respuesta para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)