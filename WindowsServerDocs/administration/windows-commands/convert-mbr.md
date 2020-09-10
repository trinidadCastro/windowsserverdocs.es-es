---
title: convert mbr
description: Artículo de referencia del comando Convert MBR, que convierte un disco básico vacío con el estilo de partición de tabla de particiones GUID (GPT) en un disco básico con el estilo de partición de registro de arranque maestro (MBR).
ms.topic: reference
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4811bbd0aff1bb0087b5275a83695623e0cb34b6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629325"
---
# <a name="convert-mbr"></a>convert mbr

Convierte un disco básico vacío con el estilo de partición de tabla de particiones GUID (GPT) en un disco básico con el estilo de partición de registro de arranque maestro (MBR). Se debe seleccionar un disco básico para que esta operación se realice correctamente. Use el [comando Seleccionar disco](select-disk.md) para seleccionar un disco básico y desplazar el foco a él.

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco básico. Haga una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.

> [!NOTE]
> Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco de tabla de particiones GUID a un disco de registro de arranque maestro](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc725797(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
convert mbr [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para convertir un disco básico de estilo de partición GPT a estilo de partición MBR, escriba>:

```
convert mbr
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
