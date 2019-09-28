---
title: Compact vDisk
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7efd40fa4b822636eda9f4082b5f561b452d3846
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379303"
---
# <a name="compact-vdisk"></a>Compact vDisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica. Este parámetro es útil porque los VHD de expansión dinámica aumentan de tamaño a medida que se agregan archivos, pero no se reducen automáticamente al eliminar archivos.
> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxis
> ```
> compact vdisk
> ```
> ## <a name="remarks"></a>Comentarios
> - Se debe seleccionar un VHD de expansión dinámica para que esta operación se realice correctamente. Use el comando **Select vDisk** para seleccionar un disco duro virtual y desplazar el foco a él.
> - Solo puede compactar los VHD de expansión dinámica que están desasociados o adjuntados como de solo lectura.
>   ## <a name="BKMK_Examples"></a>Example
>   Para compactar un VHD de expansión dinámica, escriba:
>   ```
>   compact vdisk
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
> - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> - [Attach vDisk](attach-vdisk.md)

-   [detalles del vDisk](detail-vdisk.md)
-   [Detach vDisk](detach-vdisk.md)
-   [expandir vDisk](expand-vdisk.md)
-   [Merge vDisk](merge-vdisk.md)
-   [seleccionar vDisk](select-vdisk.md)
-   [list_1](list_1.md)
