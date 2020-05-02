---
title: inactivo
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f1c0e0cd5ebbf92638a221852bc3133116f4911
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724835"
---
# <a name="inactive"></a>inactivo



En discos de registro de arranque maestro (MBR) básicos, marca como inactiva la partición de sistema o la partición de arranque que tiene el foco.

## <a name="syntax"></a>Sintaxis

```
inactive
```

## <a name="remarks"></a>Observaciones

> [!CAUTION]
> Si no existe una partición activa, puede que el equipo no se inicie. No marque una partición de sistema o de arranque como inactiva a menos que sea un usuario con experiencia que comprenda bien la familia de sistemas operativos Windows.</br>> si no puede iniciar el equipo después de marcar la partición de sistema o de arranque como inactiva, inserte el CD de instalación de Windows en la unidad de CD-ROM, reinicie el equipo y, a continuación, repare la partición mediante los comandos **fixmbr** y **fixboot** de la consola de recuperación.
> -   Después de marcar la partición del sistema o la partición de arranque como inactiva, el equipo se inicia con la siguiente opción especificada en el BIOS, como la unidad de CD-ROM o un entorno de ejecución previo al arranque (PXE).
> -   Se debe seleccionar una partición de arranque o del sistema activa para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar la partición activa y cambiar el foco a ella.

## <a name="examples"></a>Ejemplos

```
inactive
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

