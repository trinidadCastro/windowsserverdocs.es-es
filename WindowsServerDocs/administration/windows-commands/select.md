---
title: select
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: da601731f9abe1f84f082fd91528db03e6a8b05a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638994"
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

