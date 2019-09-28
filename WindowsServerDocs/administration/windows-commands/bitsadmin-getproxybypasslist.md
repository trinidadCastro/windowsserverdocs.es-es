---
title: bitsadmin getproxybypasslist
description: En el tema comandos de Windows para **bitsadmin getproxybypasslist** se recupera la lista de omisión de proxy para el trabajo especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87cc131402707eac40329750e98218ec52083b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381423"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera la lista de omisión de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

La lista de omisión contiene los nombres de host o direcciones IP, o ambos, que no se enrutarán a través de un proxy. La lista puede contener "\<local >" para hacer referencia a todos los servidores de la misma LAN. La lista puede ser de punto y coma o delimitado por espacios.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera la lista de omisión de proxy para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)