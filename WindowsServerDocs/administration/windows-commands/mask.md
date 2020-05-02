---
title: mask
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 816bcd932091b33ed897add5a13603e3a1eea925
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724018"
---
# <a name="mask"></a>mask



Quita las instantáneas de hardware que se importaron mediante el comando de **importación** .



## <a name="syntax"></a>Sintaxis

```
mask <ShadowSetID>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ShadowSetID|Quita las instantáneas que pertenecen al identificador de conjunto de instantáneas especificado.|

## <a name="remarks"></a>Observaciones

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowSetID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="examples"></a>Ejemplos

Para quitar la instantánea importada% Import_1%, escriba:
```
mask %Import_1%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)