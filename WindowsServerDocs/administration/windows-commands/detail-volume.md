---
title: detail volume
description: Tema de referencia sobre el volumen de detalles, que muestra los discos en los que reside el volumen actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2958c82b1dfc3b99d0e15690ef9857e7d83b244f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719620"
---
# <a name="detail-volume"></a>detail volume

Muestra los discos en los que reside el volumen actual.

## <a name="syntax"></a>Sintaxis

```
detail volume
```

## <a name="remarks"></a>Observaciones

-   Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.
-   Los detalles del volumen no se aplican a los volúmenes de solo lectura, como una unidad de CD-ROM o DVD-ROM.

## <a name="examples"></a>Ejemplos

Para ver todos los discos en los que reside el volumen actual, escriba:
```
detail volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

