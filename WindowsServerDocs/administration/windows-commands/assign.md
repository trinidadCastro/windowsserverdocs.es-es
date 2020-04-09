---
title: assign
description: Comandos de Windows tema para **asignar**, que asigna una letra de unidad o un punto de montaje al volumen que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4745b0472e2c8ee7a4034d9a06d395d6089db6f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851308"
---
# <a name="assign"></a>assign

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Asigna una letra de unidad o un punto de montaje al volumen que tiene el foco.

## <a name="syntax"></a>Sintaxis

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `letter=<d>` | La letra de la unidad que desea asignar al volumen. |
| `mount=<path>` | La ruta de acceso del punto de montaje que desea asignar al volumen. Para obtener instrucciones sobre cómo usar este comando, consulte [asignar una ruta de carpeta de punto de montaje a una unidad](https://go.microsoft.com/fwlink/?LinkId=207059). |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Comentarios

- Si no se especifica una letra de unidad o un punto de montaje, se asigna la siguiente letra de unidad disponible. Si la letra de unidad o el punto de montaje ya están en uso, se genera un error.

- El comando assign permite cambiar la letra de unidad asociada a una unidad extraíble.

- No se pueden asignar letras de unidad a los volúmenes de sistema, volúmenes de arranque o volúmenes que contengan el archivo de paginación. Además, no se puede asignar una letra de unidad a una partición de fabricante de equipos originales (OEM) o a una partición de tabla de particiones GUID (GPT) que no sea una partición de datos básica.

- Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example
Para asignar la letra E al volumen en el foco, escriba:
```
assign letter=e
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos] (command-line-syntax-key.md

