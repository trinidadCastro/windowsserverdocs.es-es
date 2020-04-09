---
title: detail volume
description: Temas de comandos de Windows para el volumen de detalles, que muestra los discos en los que reside el volumen actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0441beba769066c593e77b55b9266918e5f778
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846329"
---
# <a name="detail-volume"></a>detail volume

Muestra los discos en los que reside el volumen actual.

## <a name="syntax"></a>Sintaxis

```
detail volume
```

## <a name="remarks"></a>Comentarios

-   Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.
-   Los detalles del volumen no se aplican a los volúmenes de solo lectura, como una unidad de CD-ROM o DVD-ROM.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver todos los discos en los que reside el volumen actual, escriba:
```
detail volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

