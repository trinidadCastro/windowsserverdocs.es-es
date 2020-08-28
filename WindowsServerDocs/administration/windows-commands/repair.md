---
title: reparación
description: Artículo de referencia del comando reparar, que repara los volúmenes RAID-5 mediante la sustitución de la región de disco con error por un disco dinámico especificado.
ms.topic: reference
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0768a82f2de22a424b4979aa8844e9fbb4f67e5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027293"
---
# <a name="repair"></a>reparación

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Repara el volumen RAID-5 que tiene el foco reemplazando la región del disco con el disco dinámico especificado.

Para que esta operación se realice correctamente, debe seleccionarse un volumen en una matriz RAID-5. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

## <a name="syntax"></a>Sintaxis

```
repair disk=<n> [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| disco =`<n>` | Especifica el disco dinámico que reemplazará la región de disco con errores. Donde *n* debe tener un espacio libre mayor o igual que el tamaño total de la región de disco con errores en el volumen RAID-5. |
| align =`<n>` | Alinea todas las extensiones de volumen o partición con el límite de alineación más cercano. Donde *n* es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| noerr | solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="examples"></a>Ejemplos

Para reemplazar el volumen que tiene el foco reemplazándolo por el disco dinámico 4, escriba:

```
repair disk=4
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Seleccionar volumen](select-volume.md)