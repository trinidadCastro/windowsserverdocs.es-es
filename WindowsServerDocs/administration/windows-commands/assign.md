---
title: asignar
description: Tema de los comandos de Windows para **asignar** -asigna una letra de unidad para el punto de montaje al volumen que tiene el foco.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e7f680fe93e846f5b916cf3210a7ca61f190674
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435302"
---
# <a name="assign"></a>asignar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

asigna una letra de unidad para el punto de montaje al volumen que tiene el foco.

## <a name="syntax"></a>Sintaxis
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parámetros

|  Parámetro   |                                                                                                                                 Descripción                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  letter=<d>  |                                                                                                             La letra de unidad que desea asignar al volumen.                                                                                                              |
| mount=<path> | La ruta de acceso de punto de montaje que desea asignar al volumen.<br /><br />Para obtener instrucciones sobre cómo usar este comando, consulte [asignar una ruta de acceso de carpeta de punto de montaje a una unidad](https://go.microsoft.com/fwlink/?LinkId=207059) (<https://go.microsoft.com/fwlink/?LinkId=207059>). |
|    noerr     |                                    sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.                                     |

## <a name="remarks"></a>Comentarios
- Si no se especifica ningún punto de montaje o letra de unidad, se asigna la siguiente letra de unidad disponible. Si el punto de montaje o letra de unidad ya está en uso, se genera un error.
- Mediante el comando de asignación, puede cambiar la letra de unidad asociada a una unidad extraíble.
- No se puede asignar letras de unidad a volúmenes del sistema, volúmenes de arranque o volúmenes que contienen el archivo de paginación. Además, no se puede asignar una letra de unidad a una partición de fabricante de equipos originales (OEM) o cualquier partición de tabla de particiones GUID (gpt) que no sea una partición de datos básica.
- Debe seleccionarse un volumen para que esta operación se realice correctamente. Use la **seleccione volumen** comando para seleccionar un volumen y desplace el foco a ella.
  ## <a name="BKMK_examples"></a>Ejemplos
  Para asignar la letra E al volumen en el foco, escriba:
  ```
  assign letter=e
  ```

