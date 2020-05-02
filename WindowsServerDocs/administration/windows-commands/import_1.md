---
title: importación
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 569d986c57ae8b3d7253c050146ac0583c7c92df
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724845"
---
# <a name="import"></a>importación



Importa un grupo de discos externos en el grupo de discos del equipo local.

## <a name="syntax"></a>Sintaxis

```
import [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Observaciones

-   El comando IMPORT importa todos los discos que se encuentra en el mismo grupo que el disco que tiene el foco.
-   Para que esta operación se realice correctamente, debe seleccionarse un disco dinámico en un grupo de discos externos. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a>Ejemplos

Para importar todos los discos que se encuentra en el mismo grupo de discos que el disco con el foco en el grupo de discos del equipo local, escriba:
```
import
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

