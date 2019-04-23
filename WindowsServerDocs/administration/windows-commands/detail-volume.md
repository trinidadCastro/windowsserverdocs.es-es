---
title: Volumen de detalle
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7cae6dc9b82992b58c4f94801f90c0b7072492b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856926"
---
# <a name="detail-volume"></a>Volumen de detalle



Muestra los discos en el que reside el volumen actual.

## <a name="syntax"></a>Sintaxis

```
detail volume
```

## <a name="remarks"></a>Comentarios

-   Debe seleccionarse un volumen para que esta operación se realice correctamente. Use la **seleccione volumen** comando para seleccionar un volumen y desplace el foco a ella.
-   Los detalles de volumen no son aplicables a los volúmenes de solo lectura, como una unidad de CD-ROM o DVD-ROM.

## <a name="BKMK_examples"></a>Ejemplos

Para ver todos los discos en el que reside el volumen actual, escriba:
```
detail volume
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

