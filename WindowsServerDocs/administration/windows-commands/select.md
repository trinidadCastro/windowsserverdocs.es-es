---
title: select
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fb6230c24f723e20b09449967b157dd86347202
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032227"
---
# <a name="select"></a>select



Desplaza el foco a un disco, partición, volumen o disco duro virtual (VHD).

## <a name="syntax"></a>Sintaxis

```
select disk
select partition
select volume
select vdisk
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Seleccionar disco](select-disk.md)|Desplaza el foco a un disco.|
|[Seleccionar partición](select-partition.md)|Desplaza el foco a una partición.|
|[Seleccionar volumen](select-volume.md)|Desplaza el foco a un volumen.|
|[Seleccionar vDisk](select-vdisk.md)|Desplaza el foco a un disco duro virtual.|

## <a name="remarks"></a>Observaciones

-   Si se selecciona un volumen con una partición correspondiente, la partición se seleccionará automáticamente.
-   Si se selecciona una partición con un volumen correspondiente, el volumen se seleccionará automáticamente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

