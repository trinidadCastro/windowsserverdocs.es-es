---
title: 'bitsadmin getproxylist: recupera la lista de proxy para el trabajo especificado.'
description: En el tema comandos de Windows para **bitsadmin getproxylist** se recupera la lista de proxy para el trabajo especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f176d268c816725b183da0a948afcb25272b2fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381314"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera la lista de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

La lista de proxy es la lista de servidores proxy que se va a usar. La lista está delimitada por comas.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera la lista de proxy para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)