---
title: online volume
description: Artículo de referencia para el comando volumen en línea, que desconecta el volumen sin conexión al estado en línea.
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b09730d3cc0cfe758c90c3ca57fd039282ba3fce
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885226"
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
