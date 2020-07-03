---
title: offline volume
description: Artículo de referencia para el comando volumen sin conexión, que toma el volumen en línea con el foco en el estado sin conexión.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f64a6924b0033b0e7889ccbcab4fb142a4f7c05
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936730"
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
