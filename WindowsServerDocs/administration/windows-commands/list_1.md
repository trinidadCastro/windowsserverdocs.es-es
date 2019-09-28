---
title: list
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ac8b19ecae30c339138f61a13c21147d4bcf1b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374649"
---
# <a name="list"></a>list



Muestra una lista de discos, de particiones en un disco, de volúmenes de un disco o de discos duros virtuales (VHD).

## <a name="syntax"></a>Sintaxis

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|disco|Muestra una lista de discos e información sobre ellos, como su tamaño, la cantidad de espacio libre disponible, si el disco es un disco básico o dinámico, y si el disco usa el estilo de partición de registro de arranque maestro (MBR) o de tabla de particiones GUID (GPT).|
|Y|Muestra las particiones enumeradas en la tabla de particiones del disco actual.|
|volumen|Muestra una lista de volúmenes básicos y dinámicos en todos los discos.|
|vDisk|Muestra una lista de los discos duros virtuales que están conectados o seleccionados. Este comando muestra los VHD desasociados si están seleccionados actualmente; sin embargo, el tipo de disco se establece en desconocido hasta que se adjunta el disco duro virtual. El VHD marcado con un asterisco (*) tiene el foco.</br>Nota: Este comando solo está disponible para Windows 7 y Windows Server 2008 R2.|

## <a name="remarks"></a>Comentarios

-   Al enumerar las particiones en un disco dinámico, es posible que las particiones no se correspondan con los volúmenes dinámicos del disco. Esta discrepancia se produce porque los discos dinámicos contienen entradas en la tabla de particiones para el volumen del sistema o el volumen de arranque (si está presente en el disco). También contienen una partición que ocupa el resto del disco con el fin de reservar espacio para que lo usen los volúmenes dinámicos.
-   El objeto marcado con un asterisco (*) tiene el foco.
-   Al enumerar los discos, si falta un disco, su número de disco tiene el prefijo M. Por ejemplo, el primer disco que falta se numera en M0.

## <a name="BKMK_examples"></a>Example

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

