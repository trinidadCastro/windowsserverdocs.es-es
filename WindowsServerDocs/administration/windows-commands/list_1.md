---
title: list
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ef9262a04f469f54e43cf3a83efe30fac7ad8580
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854676"
---
# <a name="list"></a>list



Muestra una lista de discos, de particiones en un disco, de volúmenes en un disco o de discos duros virtuales (VHD).

## <a name="syntax"></a>Sintaxis

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|disco|Muestra una lista de discos e información acerca de ellos, como su tamaño, la cantidad de espacio libre disponible, si el disco es un disco básico o dinámico y si el disco utiliza el registro de arranque maestro (MBR) o el estilo de partición de tabla (GPT) de particiones GUID.|
|Y|Muestra las particiones enumeradas en la tabla de particiones del disco actual.|
|volumen|Muestra una lista de volúmenes básicos y dinámicos en todos los discos.|
|vdisk|Muestra una lista de los discos duros virtuales que se adjunta o se seleccionan. Este comando muestra los discos duros virtuales desasociados si actualmente están seleccionadas; Sin embargo, el tipo de disco se establece en desconocido hasta que se adjunta el disco duro virtual. El disco duro virtual marcado con un asterisco (*) tiene el foco.</br>Nota: Este comando solo está disponible para Windows 7 y Windows Server 2008 R2.|

## <a name="remarks"></a>Comentarios

-   Cuando se enumeran las particiones en un disco dinámico, las particiones no es posible que corresponden a los volúmenes dinámicos del disco. Esta discrepancia se produce porque los discos dinámicos contienen entradas en la tabla de particiones para el volumen del sistema o volumen de arranque (si está presente en el disco). También contienen una partición que ocupa el resto del disco para reservar el espacio para su uso por volúmenes dinámicos.
-   El objeto marcado con un asterisco (*) tiene el foco.
-   Cuando se muestran los discos, si falta un disco, su número de disco se prefija con M. Por ejemplo, el primer disco que falta se asigna un número M0.

## <a name="BKMK_examples"></a>Ejemplos

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

