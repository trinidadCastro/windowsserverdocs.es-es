---
title: detail disk
description: Temas de comandos de Windows para el disco de detalles, que muestra las propiedades del disco seleccionado y los volúmenes del disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d0768d45c0f56ba549ff54064c4e74ae3048e41
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846458"
---
# <a name="detail-disk"></a>detail disk

Muestra las propiedades del disco seleccionado y de sus volúmenes.

## <a name="syntax"></a>Sintaxis

```
detail disk
```

## <a name="remarks"></a>Comentarios

-   Se debe seleccionar un disco para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.
-   Si el disco seleccionado es un disco duro virtual (VHD), **detail Disk** informa del tipo de bus del disco como virtual.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver las propiedades del disco seleccionado e información acerca de los volúmenes del disco, escriba:
```
detail disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

