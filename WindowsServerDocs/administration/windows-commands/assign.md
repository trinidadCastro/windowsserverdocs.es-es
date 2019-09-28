---
title: asignar
description: 'El tema comandos de Windows para **asignar** : asigna una letra de unidad o un punto de montaje al volumen que tiene el foco.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e07edcd4ac4ddf5eca1e57da17df441043d15f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382677"
---
# <a name="assign"></a>asignar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

asigna una letra de unidad o un punto de montaje al volumen que tiene el foco.

## <a name="syntax"></a>Sintaxis
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parámetros

|  Parámetro   |                                                                                                                                 Descripción                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Letter = <d>  |                                                                                                             La letra de la unidad que desea asignar al volumen.                                                                                                              |
| Mount = <path> | La ruta de acceso del punto de montaje que desea asignar al volumen.<br /><br />para obtener instrucciones sobre cómo usar este comando, consulte [asignar una ruta de carpeta de punto de montaje a una unidad](https://go.microsoft.com/fwlink/?LinkId=207059) (<https://go.microsoft.com/fwlink/?LinkId=207059>). |
|    Noerr     |                                    Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                     |

## <a name="remarks"></a>Comentarios
- Si no se especifica ninguna letra de unidad o punto de montaje, se asigna la siguiente letra de unidad disponible. Si la letra de unidad o el punto de montaje ya está en uso, se genera un error.
- Mediante el comando Assign, puede cambiar la letra de unidad asociada a una unidad extraíble.
- No se pueden asignar Letras de unidad a volúmenes del sistema, volúmenes de arranque o volúmenes que contengan el archivo de paginación. Además, no se puede asignar una letra de unidad a una partición de fabricante de equipos originales (OEM) o a una partición de tabla de particiones GUID (GPT) que no sea una partición de datos básica.
- Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.
  ## <a name="BKMK_examples"></a>Example
  Para asignar la letra E al volumen en el foco, escriba:
  ```
  assign letter=e
  ```

