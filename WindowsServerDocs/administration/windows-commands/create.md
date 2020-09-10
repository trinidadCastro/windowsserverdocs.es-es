---
title: create
description: Artículo de referencia para el comando CREATE, que crea una partición o una partición de instantáneas en un disco, un volumen en uno o varios discos o un disco duro virtual (VHD).
ms.topic: reference
ms.assetid: b45acde1-8f4f-4ec3-b905-d8188f884af8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8a836320f9fe699beac20990ad60ade5f06e37f6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629012"
---
# <a name="create"></a>create

Crea una partición o una sombra en un disco, un volumen en uno o varios discos o un disco duro virtual (VHD). Si utiliza este comando para crear un volumen en el disco de instantáneas, debe tener al menos un volumen en el conjunto de instantáneas.

## <a name="syntax"></a>Sintaxis

```
create partition
create volume
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [comando crear partición principal](create-partition-primary.md) | Crea una partición primaria en el disco básico con el foco. |
| [comando CREATE Partition EFI](create-partition-efi.md) | Crea una partición del sistema de Extensible Firmware Interface (EFI) en un disco de tabla de particiones GUID (GPT) en equipos basados en Itanium. |
| [comando de creación de partición extendida](create-partition-extended.md) | Crea una partición extendida en el disco que tiene el foco. |
| [crear partición (comando lógico)](create-partition-logical.md) | Crea una partición lógica en una partición extendida existente. |
| [comando CREATE Partition MSR](create-partition-msr.md) | Crea una partición reservada de Microsoft (MSR) en un disco de tabla de particiones GUID (GPT). |
| [comando CREATE Volume simple](create-volume-simple.md) | Crea un volumen simple en el disco dinámico especificado. |
| [comando crear reflejo de volumen](create-volume-mirror.md) | Crea un reflejo de volumen mediante el uso de los dos discos dinámicos especificados. |
| [comando CREATE Volume RAID](create-volume-raid.md) | Crea un volumen RAID-5 con tres o más discos dinámicos especificados. |
| [comando CREATE Volume stripe](create-volume-stripe.md) | Crea un volumen seccionado mediante dos o más discos dinámicos especificados. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
