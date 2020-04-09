---
title: shrink
description: El tema comandos de Windows para DiskPart reduce, lo que reduce el tamaño del volumen seleccionado en la cantidad especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2afdaf4ac27ef0c4378d6ae34d959dc81e63bc18
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834208"
---
# <a name="shrink"></a>shrink

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando de reducción de Diskpart reduce el tamaño del volumen seleccionado en la cantidad especificada. Este comando hace que el espacio libre en disco esté disponible en el espacio no usado al final del volumen.

## <a name="syntax"></a>Sintaxis
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
### <a name="parameters"></a>Parámetros

|  Parámetro  |                                                                                             Descripción                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Desired =<n> |                                                     Especifica el espacio en megabytes (MB) que se desea reducir en el volumen.                                                     |
| mínimo =<n> |                                                           Especifica el espacio mínimo en megabytes (MB) que se desea reducir en el volumen.                                                           |
|  querymax   |                       Devuelve la cantidad máxima de espacio en MB por el que se puede reducir el volumen. Este valor puede cambiar si hay aplicaciones que están obteniendo acceso al volumen.                        |
|   nowait    |                                                       obliga a que el comando se devuelva inmediatamente mientras el proceso de reducción aún está en curso.                                                        |
|    noerr    | solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Comentarios
- El tamaño de un volumen sólo se puede reducir si se ha formateado mediante el sistema de archivos NTFS o si no contiene un sistema de archivos.
- Este comando funciona en volúmenes básicos y en volúmenes dinámicos simples o distribuidos.
- Si no se especifica una cantidad deseada, el volumen se reducirá en la cantidad mínima (si se especifica).
- Si no se especifica una cantidad mínima, el volumen se reducirá según la cantidad deseada (si se especifica).
- Si no se especifica una cantidad mínima ni una cantidad deseada, el volumen se reducirá tanto como sea posible.
- Si se especifica una cantidad mínima pero no hay suficiente espacio disponible, se producirá un error en el comando.
- Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.
- Este comando no funciona en particiones de fabricante de equipos originales (OEM), particiones del sistema de Extensible Firmware Interface (EFI) o particiones de recuperación.
  ## <a name="examples"></a><a name=BKMK_examples></a>Example
  Para reducir el tamaño del volumen seleccionado en la cantidad más grande posible entre 250 y 500 megabytes, escriba:
  ```
  shrink desired=500 minimum=250
  ```
  Para mostrar el número máximo de MB por el que se puede reducir el volumen, escriba:
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
