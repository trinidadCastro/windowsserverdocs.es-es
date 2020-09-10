---
title: active
description: En el artículo de referencia del comando activo, que en discos básicos, marca la partición que tiene el foco como activa.
ms.topic: reference
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8c8a90b2341bd74cf37c1431987d0d47403e1de6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633591"
---
# <a name="active"></a>active

En discos básicos, marca como activa la partición que tiene el foco. Solo se pueden marcar como activas las particiones. Se debe seleccionar una partición para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar una partición y desplazar el foco a ella.

> [!CAUTION]
> DiskPart solo informa al sistema básico de entrada y salida (BIOS) o Extensible Firmware Interface (EFI) de que la partición o el volumen es una partición del sistema o un volumen del sistema válidos, y es capaz de contener los archivos de inicio del sistema operativo. DiskPart no comprueba el contenido de la partición. Si marca una partición como activa erróneamente y no contiene los archivos de inicio del sistema operativo, es posible que el equipo no se inicie.

## <a name="syntax"></a>Sintaxis

```
active
```

## <a name="examples"></a>Ejemplos

Para marcar la partición que tiene el foco como la partición activa, escriba:

```
active
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [seleccionar partición (comando)](select-partition.md)
