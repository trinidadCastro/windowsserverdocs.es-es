---
title: escritor
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c00f6067cd5f6cf741cddbd6d62c5bcbb1f37a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361860"
---
# <a name="writer"></a>escritor



Comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración. Si se usa sin parámetros, el **escritor** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parámetros

| Parámetro  |                                                                                      Descripción                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | Comprueba que el escritor o el componente especificado está incluido en el procedimiento de copia de seguridad o restauración. Se producirá un error en el procedimiento de copia de seguridad o restauración si no se incluye el escritor o el componente. |
|  excluye   |                                                   Excluye el escritor o componente especificado del procedimiento de copia de seguridad o restauración.                                                    |
| [\<Writer > |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>Example

Para comprobar un escritor especificando su GUID (en este ejemplo, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), escriba:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Para excluir un escritor con el nombre "escritor del sistema", escriba:
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)