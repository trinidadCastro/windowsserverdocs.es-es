---
title: shrink
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0e5caa0103018c94671d7441a6d2349ab734be6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370898"
---
# <a name="shrink"></a>shrink

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando de reducción de Diskpart reduce el tamaño del volumen seleccionado en la cantidad especificada. Este comando hace que el espacio libre en disco esté disponible en el espacio no usado al final del volumen.

## <a name="syntax"></a>Sintaxis
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>Parámetros

|  Parámetro  |                                                                                             Descripción                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Desired = <n> |                                                     Especifica la cantidad deseada de espacio en megabytes (MB) para reducir el tamaño del volumen.                                                     |
| mínimo = <n> |                                                           Especifica la cantidad mínima de espacio en MB para reducir el tamaño del volumen.                                                           |
|  querymax   |                       Devuelve la cantidad máxima de espacio en MB por el que se puede reducir el volumen. Este valor puede cambiar si las aplicaciones actualmente tienen acceso al volumen.                        |
|   nowait    |                                                       obliga a que el comando se devuelva inmediatamente mientras el proceso de reducción aún está en curso.                                                        |
|    Noerr    | Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Comentarios
- Puede reducir el tamaño de un volumen solo si está formateado con el sistema de archivos NTFS o si no tiene un sistema de archivos.
- Este comando funciona en volúmenes básicos y en volúmenes dinámicos simples o distribuidos.
- Si no se especifica una cantidad deseada, el volumen se reducirá en la cantidad mínima (si se especifica).
- Si no se especifica una cantidad mínima, el volumen se reducirá según la cantidad deseada (si se especifica).
- Si no se especifica una cantidad mínima ni una cantidad deseada, el volumen se reducirá tanto como sea posible.
- Si se especifica una cantidad mínima pero no hay suficiente espacio disponible, se producirá un error en el comando.
- Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.
- Este comando no funciona en particiones de fabricante de equipos originales (OEM), particiones del sistema de Extensible Firmware Interface (EFI) o particiones de recuperación.
  ## <a name="BKMK_examples"></a>Example
  Para reducir el tamaño del volumen seleccionado en la cantidad más grande posible entre 250 y 500 megabytes, escriba:
  ```
  shrink desired=500 minimum=250
  ```
  Para mostrar el número máximo de MB por el que se puede reducir el volumen, escriba:
  ```
  shrink querymax
  ```

[Cambiar tamaño-partición](https://technet.microsoft.com/library/hh848680.aspx)
