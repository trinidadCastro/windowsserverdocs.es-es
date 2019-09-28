---
title: bitsadmin cache y DELETE
description: 'Tema de comandos de Windows para **caché de bitsadmin y eliminar** : elimina una entrada de caché específica.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c3ffd7e0c9c43e8e2eb6e5d5a1d98610a4d9ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382073"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache y DELETE



Elimina una entrada de caché específica.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|RecordID|GUID asociado a la entrada de caché.|

## <a name="BKMK_examples"></a>Example

En el siguiente ejemplo se elimina la entrada de caché con el RecordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)