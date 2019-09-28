---
title: memoria caché y información de bitsadmin
description: 'Tema de comandos de Windows para **caché de bitsadmin y info** : vuelca una entrada de caché específica.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11963ff5640ef30e597e5e802778aff121c0efb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381969"
---
# <a name="bitsadmin-cache-and-info"></a>memoria caché y información de bitsadmin



Vuelca una entrada específica de la memoria caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|RecordID|GUID asociado a la entrada de caché.|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se vuelca la entrada de caché con el RecordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)