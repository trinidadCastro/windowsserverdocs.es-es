---
title: detail disk
description: Tema de referencia del disco de detalles, que muestra las propiedades del disco seleccionado y los volúmenes del disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a746506d6c9609e3214dbd48e5fa91f52d16ab4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710518"
---
# <a name="detail-disk"></a>detail disk

Muestra las propiedades del disco seleccionado y los volúmenes del mismo.

## <a name="syntax"></a>Sintaxis

```
detail disk
```

## <a name="remarks"></a>Observaciones

-   Se debe seleccionar un disco para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.
-   Si el disco seleccionado es un disco duro virtual (VHD), **detail Disk** informa del tipo de bus del disco como virtual.

## <a name="examples"></a>Ejemplos

Para ver las propiedades del disco seleccionado e información acerca de los volúmenes del disco, escriba:
```
detail disk
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

