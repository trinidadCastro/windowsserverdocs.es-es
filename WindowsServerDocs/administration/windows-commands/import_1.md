---
title: importar DiskPart
description: Artículo de referencia para el comando de importación, que importa un grupo de discos externos en el grupo de discos del equipo local.
ms.topic: reference
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c20154e4f687aaaba5639baf4cbbbc6b536c72bd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634512"
---
# <a name="import-diskpart"></a>import (diskpart)

Importa un grupo de discos externos en el grupo de discos del equipo local. Este comando importa todos los discos que se encuentra en el mismo grupo que el disco que tiene el foco.

> AÚN Para poder usar este comando, debe usar el [comando Seleccionar disco](select-disk.md) para seleccionar un disco dinámico en un grupo de discos externos y cambiar el foco a él.

## <a name="syntax"></a>Sintaxis

```
import [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="examples"></a>Ejemplos

Para importar todos los discos que se encuentra en el mismo grupo de discos que el disco con el foco en el grupo de discos del equipo local, escriba:

```
import
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando DiskPart](diskpart.md)
