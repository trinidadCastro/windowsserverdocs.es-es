---
title: convert gpt
description: Artículo de referencia del comando Convert GPT, que convierte un disco básico vacío con el estilo de partición de registro de arranque maestro (MBR) en un disco básico con el estilo de partición de tabla de particiones GUID (GPT).
ms.topic: reference
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 040bf89e020076e923f01f5cd20450c7d7fb820a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629311"
---
# <a name="convert-gpt"></a>convert gpt

Convierte un disco básico vacío con el estilo de partición de registro de arranque maestro (MBR) en un disco básico con el estilo de partición de tabla de particiones GUID (GPT). Se debe seleccionar un disco MBR básico para que esta operación se realice correctamente. Use el [comando Seleccionar disco](select-disk.md) para seleccionar un disco básico y desplazar el foco a él.

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco básico. Haga una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco. El tamaño de disco mínimo necesario para la conversión a GPT es de 128 megabytes.

> [!NOTE]
> Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco de registro de arranque maestro a un disco de tabla de particiones GUID](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc725671(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
convert gpt [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para convertir un disco básico de estilo de partición MBR a estilo de partición GPT, escriba:

```
convert gpt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
