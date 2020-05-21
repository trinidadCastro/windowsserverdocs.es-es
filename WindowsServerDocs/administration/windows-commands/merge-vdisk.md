---
title: Merge vDisk
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cec8eeaa80436dbb34eb055950169b6895efa544
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437150"
---
# <a name="merge-vdisk"></a>Merge vDisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Combina un disco duro virtual (VHD) de diferenciación con su VHD primario correspondiente. El VHD primario se modificará para incluir las modificaciones del VHD de diferenciación.
> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxis
> ```
> merge vdisk depth=<n>
> ```
> #### <a name="parameters"></a>Parámetros
>
> | Parámetro |                                                                                    Descripción                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | profundidad =<n> | Indica el número de archivos VHD primarios que se van a combinar. Por ejemplo, **Depth = 1** indica que el disco duro virtual de diferenciación se combinará con un nivel de la cadena de diferenciación. |
>
>#### <a name="remarks"></a>Observaciones
> - Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.
> - Este parámetro modifica el VHD primario. Como resultado, otros VHD de diferenciación que dependen del elemento primario ya no serán válidos.
>   ## <a name="examples"></a>Ejemplos
>   Para fusionar mediante combinación un disco duro virtual de diferenciación con su VHD primario, escriba:
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
> - - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [Compact vDisk](compact-vdisk.md)

-   [detalles del vDisk](detail-vdisk.md)
-   [Detach vDisk](detach-vdisk.md)
-   [expandir vDisk](expand-vdisk.md)
-   [seleccionar vDisk](select-vdisk.md)
-   [list_1](list_1.md)
