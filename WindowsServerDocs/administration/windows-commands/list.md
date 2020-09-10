---
title: list
description: Artículo de referencia del comando lista, que muestra una lista de discos, de particiones en un disco, de volúmenes de un disco o de discos duros virtuales (VHD).
ms.topic: reference
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 75ff0e4d335cf9a5ec16fb529540c85d540a2ae8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639541"
---
# <a name="list"></a>list

Muestra una lista de discos, de particiones en un disco, de volúmenes de un disco o de discos duros virtuales (VHD).

## <a name="syntax"></a>Sintaxis

```
list { disk | partition | volume | vdisk }
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| disk | Muestra una lista de discos e información acerca de ellos, como el tamaño, la cantidad de espacio libre disponible, si se trata de un disco básico o dinámico y si el disco usa el estilo de partición de registro de arranque maestro (MBR) o de tabla de particiones GUID (GPT). |
| partición | Muestra las particiones enumeradas en la tabla de particiones del disco actual. |
| volumen | Muestra una lista de volúmenes básicos y dinámicos en todos los discos. |
| vDisk | Muestra una lista de los discos duros virtuales que están conectados o seleccionados. Este comando muestra los VHD desasociados si están seleccionados actualmente; sin embargo, el tipo de disco se establece en desconocido hasta que se adjunta el disco duro virtual. El VHD marcado con un asterisco (*) tiene el foco. |

#### <a name="remarks"></a>Observaciones

- Al enumerar las particiones en un disco dinámico, es posible que las particiones no correspondan a los volúmenes dinámicos del disco. Esta discrepancia se produce porque los discos dinámicos contienen entradas en la tabla de particiones para el volumen de sistema o el volumen de arranque (si existen en el disco). También contienen una partición que ocupa el resto del disco con el fin de reservar espacio para que lo usen los volúmenes dinámicos.

- El objeto marcado con un asterisco (*) tiene el foco.

- Al enumerar los discos, si falta un disco, su número de disco tiene el prefijo M. Por ejemplo, el primer disco que falta se numera en *M0*.

### <a name="examples"></a>Ejemplos

```
list disk
list partition
list volume
list vdisk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
