---
title: merge vdisk
description: Artículo de referencia para el comando Merge vDisk, que combina un disco duro virtual (VHD) de diferenciación con su VHD primario correspondiente.
ms.topic: reference
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d57f4919fbc253149343660f7239cd3405910711
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037863"
---
# <a name="merge-vdisk"></a>merge vdisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Combina un disco duro virtual (VHD) de diferenciación con su VHD primario correspondiente. El VHD primario se modificará para incluir las modificaciones del VHD de diferenciación. Este comando modifica el VHD primario. Como resultado, otros VHD de diferenciación que dependen del elemento primario ya no serán válidos.

> [!IMPORTANT]
> Debe elegir y desasociar un VHD para que esta operación se realice correctamente. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
merge vdisk depth=<n>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| profundidad =`<n>` | Indica el número de archivos VHD primarios que se van a combinar. Por ejemplo, `depth=1` indica que el disco duro virtual de diferenciación se combinará con un nivel de la cadena de diferenciación. |

### <a name="examples"></a>Ejemplos

Para fusionar mediante combinación un disco duro virtual de diferenciación con su VHD primario, escriba:

```
merge vdisk depth=1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Attach vDisk (comando)](attach-vdisk.md)

- [Compact vDisk (comando)](compact-vdisk.md)

- [Detail vDisk (comando)](detail-vdisk.md)

- [detach vDisk (comando)](detach-vdisk.md)

- [comando Expand vDisk](expand-vdisk.md)

- [comando SELECT vDisk](select-vdisk.md)

- [List (comando)](list.md)
