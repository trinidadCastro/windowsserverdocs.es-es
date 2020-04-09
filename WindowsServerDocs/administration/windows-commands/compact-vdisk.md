---
title: Compact vDisk
description: El tema de comandos de Windows forcompact vDisk, que reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9691be21c188fbc2c3b2e782acde127270decf56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847428"
---
# <a name="compact-vdisk"></a>Compact vDisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica. Este parámetro es útil porque los VHD de expansión dinámica aumentan de tamaño a medida que se agregan archivos, pero no se reducen automáticamente al eliminar archivos.

> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis
```
compact vdisk
```

## <a name="remarks"></a>Comentarios

- Se debe seleccionar un VHD de expansión dinámica para que esta operación se realice correctamente. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.

- Solo puede compactar los VHD de expansión dinámica que están desasociados o adjuntados como de solo lectura.

## <a name="examples"></a><a name=BKMK_Examples></a>Example
Para compactar un VHD de expansión dinámica, escriba:
```
compact vdisk
```

## <a name="additional-references"></a>Referencias adicionales
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [Attach vDisk](attach-vdisk.md)
- [detalles del vDisk](detail-vdisk.md)
- [Detach vDisk](detach-vdisk.md)
- [expandir vDisk](expand-vdisk.md)
- [Merge vDisk](merge-vdisk.md)
- [seleccionar vDisk](select-vdisk.md)
- [list_1](list_1.md)
