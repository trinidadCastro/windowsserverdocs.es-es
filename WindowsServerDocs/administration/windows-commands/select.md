---
title: estable
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dc3bc8775f971968f096ba4344348e77c112cfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384109"
---
# <a name="select"></a>estable



Desplaza el foco a un disco, partición, volumen o disco duro virtual (VHD).

## <a name="syntax"></a>Sintaxis

```
select disk
select partition
select volume
select vdisk
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Seleccionar disco](select-disk.md)|Desplaza el foco a un disco.|
|[Seleccionar partición](select-partition.md)|Desplaza el foco a una partición.|
|[Seleccionar volumen](select-volume.md)|Desplaza el foco a un volumen.|
|[Seleccionar vDisk](select-vdisk.md)|Desplaza el foco a un disco duro virtual.|

## <a name="remarks"></a>Comentarios

-   Si se selecciona un volumen con una partición correspondiente, la partición se seleccionará automáticamente.
-   Si se selecciona una partición con un volumen correspondiente, el volumen se seleccionará automáticamente.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

