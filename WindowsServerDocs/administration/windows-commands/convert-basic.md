---
title: convert basic
description: Artículo de referencia para el comando convertir básico, que convierte un disco dinámico vacío en un disco básico.
ms.topic: reference
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a094da440bb898f67178c18a3408567cee7eab3d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025939"
---
# <a name="convert-basic"></a>convert basic

Convierte un disco dinámico vacío en un disco básico. Se debe seleccionar un disco dinámico para que esta operación se realice correctamente. Use el [comando Seleccionar disco](select-disk.md) para seleccionar un disco dinámico y cambiar el foco a él.

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco básico. Haga una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.

> [!NOTE]
> Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco dinámico a un disco básico](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755238(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
convert basic [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para convertir el disco dinámico seleccionado en básico, escriba:

```
convert basic
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
