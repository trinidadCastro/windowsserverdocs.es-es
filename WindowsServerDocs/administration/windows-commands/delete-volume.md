---
title: delete volume
description: Windows Commands tema para Delete Volume, que elimina el volumen seleccionado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2e958785c278306563999b09c1fecc0fdfa7ecb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846558"
---
# <a name="delete-volume"></a>delete volume

Elimina el volumen seleccionado.

## <a name="syntax"></a>Sintaxis

```
delete volume [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Comentarios

-   No se puede eliminar el volumen del sistema, el volumen de arranque ni cualquier otro volumen que contenga el archivo de paginación o el archivo de volcado (volcado de memoria) activos.
-   Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para eliminar el volumen que tiene el foco, escriba:
```
delete volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

