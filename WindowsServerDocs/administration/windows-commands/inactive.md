---
title: Inactiva
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce91b6a024c165e3aa63148b9ad6dfcc4db7a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375373"
---
# <a name="inactive"></a>Inactiva



En los discos básicos de registro de arranque maestro (MBR), marca la partición del sistema o la partición de arranque con el foco como inactivo.

## <a name="syntax"></a>Sintaxis

```
inactive
```

## <a name="remarks"></a>Comentarios

> [!CAUTION]
> Es posible que el equipo no se inicie sin una partición activa. No marque una partición de sistema o de arranque como inactiva a menos que sea un usuario con experiencia que comprenda bien la familia de sistemas operativos Windows.</br>> Si no puede iniciar el equipo después de marcar la partición de sistema o de arranque como inactiva, inserte el CD de instalación de Windows en la unidad de CD-ROM, reinicie el equipo y, a continuación, repare la partición mediante los comandos **fixmbr** y **fixboot** del Consola de recuperación.
> -   Después de marcar la partición del sistema o la partición de arranque como inactiva, el equipo se inicia con la siguiente opción especificada en el BIOS, como la unidad de CD-ROM o un entorno de ejecución previo al arranque (PXE).
> -   Se debe seleccionar una partición de arranque o del sistema activa para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar la partición activa y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Example

```
inactive
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

