---
title: convert dynamic
description: Artículo de referencia para el comando convertir dinámico, que convierte un disco básico en un disco dinámico.
ms.topic: reference
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7a67b7e900ce57e7551d8c565022aa80be5d714a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629354"
---
# <a name="convert-dynamic"></a>convert dynamic

Convierte un disco básico en un disco dinámico. Se debe seleccionar un disco básico para que esta operación se realice correctamente. Use el [comando Seleccionar disco](select-disk.md) para seleccionar un disco básico y desplazar el foco a él.

> [!NOTE]
> Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco dinámico a un disco básico](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755238(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

#### <a name="remarks"></a>Observaciones

- Todas las particiones existentes en el disco básico se convertirán en volúmenes simples.

## <a name="examples"></a>Ejemplos

Para convertir un disco básico en un disco dinámico, escriba:

```
convert dynamic
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
