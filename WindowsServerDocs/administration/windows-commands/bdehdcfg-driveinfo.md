---
title: bdehdcfg DriveInfo
description: 'Temas de comandos de Windows para * * bdehdcfg: DriveInfo * *: muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de la partición.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f4541bfd71fb7639d18e6e548559ed02918815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382272"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: DriveInfo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de la partición. Solo se enumeran las particiones válidas. El espacio sin asignar no se enumera si ya existen cuatro particiones principales o extendidas. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxis
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Parámetros

|   Parámetro   |                  Descripción                  |
|---------------|-----------------------------------------------|
| <DriveLetter> | Especifica una letra de unidad seguida de dos puntos. |

## <a name="remarks"></a>Comentarios
El comando es meramente informativo y no realiza ninguna modificación en la unidad.
## <a name="BKMK_Examples"></a>Ejemplo
En el ejemplo siguiente se mostrará la información de la unidad C.
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
