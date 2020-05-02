---
title: active
description: Tema de referencia del comando activo, que en discos básicos, marca la partición que tiene el foco como activa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 997c57b93434738c87396812c9b5e5b12d7a8e89
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719017"
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
