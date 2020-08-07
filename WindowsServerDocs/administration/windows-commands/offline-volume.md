---
title: offline volume
description: Artículo de referencia para el comando volumen sin conexión, que toma el volumen en línea con el foco en el estado sin conexión.
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e8a6bbebb88f97c4e4b04afccedeb05c4a1392a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885264"
---
# <a name="offline-volume"></a>offline volume

Toma el volumen en línea que tiene el foco en el estado sin conexión.

> [!NOTE]
> Se debe seleccionar un volumen para que el comando **volumen sin conexión** se realice correctamente. Use el comando [seleccionar volumen](select-volume.md) para seleccionar un disco y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
offline volume [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="examples"></a>Ejemplos

Para desconectar el disco con el foco, escriba:

```
offline volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
