---
title: bdehdcfg driveinfo
description: 'Tema de los comandos de Windows para ** bdehdcfg: driveinfo **: muestra la letra de unidad, el tamaño total, el espacio máximo disponible y las características de la partición.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4aa041c27b1797e7d00476212887a7dc6dbc1880
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889066"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: driveinfo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra la letra de unidad, el tamaño total, el espacio máximo disponible y las características de la partición. Solo se enumeran las particiones válidas. El espacio sin asignar no se enumera si ya existen cuatro particiones principales o extendidas. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxis
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<DriveLetter>|Especifica una letra de unidad seguida de dos puntos.|
## <a name="remarks"></a>Comentarios
El comando es solo informativo y no realiza ninguna modificación en la unidad.
## <a name="BKMK_Examples"></a>Ejemplo
El siguiente ejemplo muestra la información de unidad para la unidad C.
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
