---
title: tamaño de bdehdcfg
description: El tema comandos de Windows para el **tamaño de bdehdcfg**, que especifica el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c72bdfd0b8bf4dfd0aa36885ceb7fd249a18c332
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851028"
---
# <a name="bdehdcfg-size"></a>bdehdcfg: tamaño

Especifica el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema.

Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<SizeinMB>` | Indica el número de megabytes (MB) que se va a utilizar para la nueva partición. |

## <a name="remarks"></a>Comentarios

Si no especifica un tamaño, la herramienta utilizará el valor predeterminado de 300 MB. El tamaño mínimo de la unidad del sistema es 100 MB. Si va a almacenar las herramientas de recuperación del sistema u otras herramientas del sistema en la partición del sistema, deberá aumentar el tamaño consiguientemente.

> [!NOTE]
> El comando de **tamaño** no se puede combinar con el comando de **combinación** de `<DriveLetter>` de **destino** .

## <a name="examples"></a><a name=BKMK_Examples></a>Example

En el siguiente ejemplo se muestra el uso del comando **size** para asignar 500 MB a la unidad del sistema predeterminada.

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)