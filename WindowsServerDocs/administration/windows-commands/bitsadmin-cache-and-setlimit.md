---
title: memoria caché de bitsadmin y setlimit
description: 'Temas de comandos de Windows para la **caché de bitsadmin y setlimit** : establece el límite de tamaño de caché.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a10ce8599202e237daa6822cf62806d3c21429
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381939"
---
# <a name="bitsadmin-cache-and-setlimit"></a>memoria caché de bitsadmin y setlimit



Establece el límite de tamaño de la memoria caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Cierto|Límite de caché definido como un porcentaje del espacio total del disco duro.|

## <a name="BKMK_examples"></a>Example

En el siguiente ejemplo se limita el tamaño de la caché al 50%.
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)