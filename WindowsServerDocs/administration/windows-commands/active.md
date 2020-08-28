---
title: active
description: En el artículo de referencia del comando activo, que en discos básicos, marca la partición que tiene el foco como activa.
ms.topic: reference
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6baad8bd60307eeddf7b777f059ab5ba3846e1d4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029483"
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
