---
title: memoria caché de bitsadmin y deleteurl
description: 'Tema de comandos de Windows para la **caché de bitsadmin y deleteurl** : elimina todas las entradas de caché de la dirección URL especificada.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 869d3bc0f011cc82aaea9b7468667964051e1c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382056"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>memoria caché de bitsadmin y deleteurl



Elimina todas las entradas de la memoria caché de la dirección URL especificada.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|url|Localizador uniforme de recursos que identifica un archivo remoto.|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se eliminan todas las entradas de la memoria caché para https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)