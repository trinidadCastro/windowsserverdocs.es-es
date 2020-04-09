---
title: Merge vDisk
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1315b82510ae356f80d5b519c0676d0d156ab9fd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839628"
---
# <a name="merge-vdisk"></a>Merge vDisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
> ## <a name="remarks"></a>Comentarios
> - Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.
> - Este parámetro modifica el VHD primario. Como resultado, otros VHD de diferenciación que dependen del elemento primario ya no serán válidos.
>   ## <a name="examples"></a><a name=BKMK_Examples></a>Example
>   Para fusionar mediante combinación un disco duro virtual de diferenciación con su VHD primario, escriba:
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
> - - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> - [Attach vDisk](attach-vdisk.md)
> - [Compact vDisk](compact-vdisk.md)

-   [detalles del vDisk](detail-vdisk.md)
-   [Detach vDisk](detach-vdisk.md)
-   [expandir vDisk](expand-vdisk.md)
-   [seleccionar vDisk](select-vdisk.md)
-   [list_1](list_1.md)
