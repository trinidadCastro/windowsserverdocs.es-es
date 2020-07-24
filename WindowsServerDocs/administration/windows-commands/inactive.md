---
title: inactivo
description: Artículo de referencia para el comando inactivo, que marca la partición del sistema o la partición de arranque con el foco como inactivo en discos básicos de registro de arranque maestro (MBR).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7e9679dee2bb8a75da14e1d7ee6c75b80bb1ef1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957137"
---
# <a name="inactive"></a>inactivo

Marca la partición del sistema o la partición de arranque con el foco como inactivo en discos básicos de registro de arranque maestro (MBR).

Se debe seleccionar una partición de arranque o del sistema activa para que esta operación se realice correctamente. Use el comando [Select Partition Command](select-partition.md) para seleccionar la partición activa y cambiar el foco a ella.

> [!CAUTION]
> Si no existe una partición activa, puede que el equipo no se inicie. No marque una partición de sistema o de arranque como inactiva, a menos que sea un usuario con experiencia con un conocimiento exhaustivo de la familia de sistemas operativos Windows.<p>Si no puede iniciar el equipo después de marcar la partición de sistema o de arranque como inactiva, inserte el instalación de Windows CD en la unidad de CD-ROM, reinicie el equipo y, a continuación, repare la partición mediante los comandos **fixmbr** y **fixboot** de la consola de recuperación.
>
> Después de marcar la partición del sistema o la partición de arranque como inactiva, el equipo se inicia con la siguiente opción especificada en el BIOS, como la unidad de CD-ROM o un entorno de ejecución previo al arranque (PXE).

## <a name="syntax"></a>Sintaxis

```
inactive
```

### <a name="examples"></a>Ejemplos

```
inactive
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [seleccionar partición (comando)](select-partition.md)

- [Solución avanzada de problemas de arranque de Windows](/windows/client-management/advanced-troubleshooting-boot-problems)
