---
title: writer
description: Temas de comandos de Windows para el escritor, que comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb13de162b8e5eb8150d145a4afacccf47bb25f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828988"
---
# <a name="writer"></a>writer



Comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración. Si se usa sin parámetros, el **escritor** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

### <a name="parameters"></a>Parámetros

| Parámetro  |                                                                                      Descripción                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   comprobar   | Comprueba que el escritor o el componente especificado está incluido en el procedimiento de copia de seguridad o restauración. Se producirá un error en el procedimiento de copia de seguridad o restauración si no se incluye el escritor o el componente. |
|  exclude   |                                                   Excluye el escritor o componente especificado del procedimiento de copia de seguridad o restauración.                                                    |
| [\<redactor > |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para comprobar un escritor especificando su GUID (en este ejemplo, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), escriba:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Para excluir un escritor con el nombre escritor del sistema, escriba:
```
writer exclude System Writer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)