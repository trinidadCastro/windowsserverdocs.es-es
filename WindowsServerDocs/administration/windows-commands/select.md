---
title: estable
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4c3723dd414adca68c22011ef3f6be02eb6531d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889906"
---
# <a name="select"></a>estable



Cambia el foco a un disco, partición, volumen o disco duro virtual (VHD).

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
|[Seleccione el disco](select-disk.md)|Cambia el foco a un disco.|
|[Seleccione la partición](select-partition.md)|Cambia el foco a una partición.|
|[Seleccione el volumen](select-volume.md)|Cambia el foco a un volumen.|
|[Seleccione vdisk](select-vdisk.md)|Cambia el foco a un disco duro virtual.|

## <a name="remarks"></a>Comentarios

-   Si se selecciona un volumen con una partición correspondiente, se seleccionará automáticamente la partición.
-   Si se ha seleccionado una partición con un volumen correspondiente, se seleccionará automáticamente el volumen.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

