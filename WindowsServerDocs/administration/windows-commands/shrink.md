---
title: shrink
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 09cc9ce7beebc04a0a45cd93c2b0da25c5a1a49e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441266"
---
# <a name="shrink"></a>shrink

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando de reducción de Diskpart reduce el tamaño del volumen seleccionado según la cantidad que especifique. Este comando hace que el espacio libre en disco que estén disponibles desde el espacio no usado al final del volumen.

## <a name="syntax"></a>Sintaxis
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>Parámetros

|  Parámetro  |                                                                                             Descripción                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired=<n> |                                                     Especifica la cantidad de espacio en megabytes (MB) para reducir el tamaño del volumen deseada.                                                     |
| mínimo =<n> |                                                           Especifica la cantidad mínima de espacio en MB para reducir el tamaño del volumen.                                                           |
|  querymax   |                       Devuelve la cantidad máxima de espacio en MB por el que se puede reducir el volumen. Este valor puede cambiar si las aplicaciones tienen acceso actualmente el volumen.                        |
|   NOWAIT    |                                                       obliga al comando para devolver de inmediato, mientras el proceso de reducción aún está en curso.                                                        |
|    noerr    | sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error. |

## <a name="remarks"></a>Comentarios
- Puede reducir el tamaño de un volumen sólo si tiene el formato sistema de archivos NTFS o si no tiene un sistema de archivos.
- Este comando funciona en volúmenes básicos y en volúmenes dinámicos simples o distribuidos.
- Si no se especifica una cantidad deseada, el volumen se reducirá la cantidad mínima (si se especifica).
- Si no se especifica una cantidad mínima, el volumen se reducirá la cantidad deseada (si se especifica).
- Si se especifica una cantidad mínima ni una cantidad deseada, el volumen se verá reducido en tanto como sea posible.
- Si se especifica una cantidad mínima pero no hay suficiente espacio libre está disponible, se producirá un error en el comando.
- Debe seleccionarse un volumen para que esta operación se realice correctamente. Use la **seleccione volumen** comando para seleccionar un volumen y desplace el foco a ella.
- Este comando no funciona en las particiones del fabricante de equipos originales (OEM), las particiones del sistema Extensible Firmware Interface (EFI) o las particiones de recuperación.
  ## <a name="BKMK_examples"></a>Ejemplos
  Para reducir el tamaño del volumen seleccionado por la mayor cantidad posible entre 500 y 250 MB, escriba:
  ```
  shrink desired=500 minimum=250
  ```
  Para mostrar el número máximo de MB que se puede reducir el volumen, escriba:
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
