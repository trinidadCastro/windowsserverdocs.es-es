---
title: mask
description: Tema de referencia del comando Mask, que quita las instantáneas de hardware que se importaron mediante el comando IMPORT.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee01bb74b1fef1bb31a266c01a9e9bde7743691d
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354645"
---
# <a name="mask"></a>mask

Quita las instantáneas de hardware que se importaron mediante el comando de **importación** .

## <a name="syntax"></a>Sintaxis

```
mask <shadowsetID>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| shadowsetID | Quita las instantáneas que pertenecen al identificador de conjunto de instantáneas especificado. |

#### <a name="remarks"></a>Observaciones

- Puede usar un alias existente o una variable de entorno en lugar de *ShadowSetID*. Use **Agregar** sin parámetros para ver los alias existentes.

### <a name="examples"></a>Ejemplos

Para quitar la instantánea importada *% Import_1%*, escriba:

```
mask %Import_1%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)