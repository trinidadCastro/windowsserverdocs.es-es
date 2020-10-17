---
title: ver
description: Artículo de referencia del comando ver, que muestra el número de versión del sistema operativo.
ms.topic: reference
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9e99c5af9ee45b33cb0c050307c83c89874d89cb
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156271"
---
# <a name="ver"></a>ver

Muestra el número de versión del sistema operativo. Este comando se admite en el símbolo del sistema de Windows (Cmd.exe), pero no en PowerShell.

## <a name="syntax"></a>Sintaxis

```
ver
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para obtener el número de versión del sistema operativo desde el shell de comandos (cmd.exe), escriba:

```
ver
```

El comando **Ver** no funciona en PowerShell. Si desea obtener el número de versión del sistema operativo a través de PowerShell, escriba:

```powershell
$PSVersionTable.BuildVersion
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
