---
title: sistema de escritura
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 87b10952c6a851b5536a1589b994b265e8699f59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826406"
---
# <a name="writer"></a>sistema de escritura



Comprueba que un sistema de escritura o un componente se incluye o excluye un componente o sistema de escritura de los procedimientos de copia de seguridad y restauración. Si se utiliza sin parámetros, **escritor** muestra la Ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|verify|Comprueba que el sistema de escritura especificado o el componente se incluye en el procedimiento de copia de seguridad o restauración. El procedimiento de copia de seguridad o restauración se producirá un error si el sistema de escritura o el componente no se incluye.|
|excluye|Excluye el escritor especificado o el componente de los procedimientos de copia de seguridad y restauración.|
|[\<Escritor > | <Component>]|Especifica el sistema de escritura o el componente para comprobar o excluir. Se especifican los escritores de GUID de sistema de escritura o por el nombre de sistema de escritura, por ejemplo "escritor del sistema".|

## <a name="BKMK_examples"></a>Ejemplos

Para comprobar que un sistema de escritura mediante la especificación de su GUID (para este ejemplo, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), escriba:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Para excluir un sistema de escritura con el nombre "escritor del sistema?, type:
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)