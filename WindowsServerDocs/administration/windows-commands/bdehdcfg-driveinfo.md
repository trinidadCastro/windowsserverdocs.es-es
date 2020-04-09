---
title: bdehdcfg DriveInfo
description: Windows Commands topic for **bdehdcfg DriveInfo**, que muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de la partición.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c598ea2d1d140090d623b3b48dbcc1be51ee66c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851068"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: DriveInfo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de la partición. Solo se enumeran las particiones válidas. El espacio sin asignar no se enumera si ya existen cuatro particiones principales o extendidas.

Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -driveinfo <DriveLetter>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| <DriveLetter> | Especifica una letra de unidad seguida de dos puntos. |

## <a name="remarks"></a>Comentarios

El comando es meramente informativo y no realiza ninguna modificación en la unidad.

## <a name="example"></a><a name=BKMK_Examples></a>Ejemplo

En el ejemplo siguiente se mostrará la información de la unidad C.

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
