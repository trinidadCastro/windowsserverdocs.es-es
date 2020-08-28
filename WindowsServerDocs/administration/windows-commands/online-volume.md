---
title: online volume
description: Artículo de referencia para el comando volumen en línea, que desconecta el volumen sin conexión al estado en línea.
ms.topic: reference
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8847f340ebe3e295946550c46fd86bd59de300af
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032693"
---
# <a name="online-volume"></a>online volume

Desconecta el volumen sin conexión al estado en línea. Este comando funciona en los volúmenes con errores, con errores o con errores de redundancia.

> [!NOTE]
> Se debe seleccionar un volumen para que el comando **volumen en línea** se ejecute correctamente. Use el comando [seleccionar volumen](select-volume.md) para seleccionar un volumen y cambiar el foco a él.

> [!IMPORTANT]
> Este comando producirá un error si se usa en un disco de solo lectura.

## <a name="syntax"></a>Sintaxis

```
online volume [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="examples"></a>Ejemplos

Para poner en línea el volumen que tiene el foco, escriba:

```
online volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
