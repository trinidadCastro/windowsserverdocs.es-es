---
title: destino de bdehdcfg
description: Tema de comandos de Windows para el **destino de bdehdcfg**, que prepara una partición para su uso como unidad del sistema mediante BitLocker y la recuperación de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0f3e90fbb8725360cf8db335e79721e2328ab3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851018"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: destino

Prepara una partición para su uso como unidad del sistema mediante BitLocker y la recuperación de Windows. De forma predeterminada, esta partición se crea sin una letra de unidad.

Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| default | Indica que la herramienta de línea de comandos seguirá el mismo proceso que el asistente para la instalación de BitLocker. |
| unallocated | Crea la partición del sistema a partir del espacio sin asignar disponible en el disco. |
| `<DriveLetter>` reducir | Reduce la unidad especificada en la cantidad necesaria para crear una partición activa del sistema. Para utilizar este comando, la unidad especificada debe tener al menos el 5 por ciento de espacio disponible. |
| `<DriveLetter>` combinar | Utiliza la unidad de disco especificada como partición activa del sistema. La unidad del sistema operativo no puede ser un destino para la combinación. |

## <a name="examples"></a><a name=BKMK_Examples></a>Example

En el siguiente ejemplo se muestra el uso del comando de **destino** para designar una unidad existente (P) como unidad del sistema.

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)