---
title: expandir vDisk
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fb5d7b58b7aa4bc9557086c73020820cfa04284
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377290"
---
# <a name="expand-vdisk"></a>expandir vDisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande un disco duro virtual (VHD) al tamaño que especifique.
> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxis
> ```
> expand vdisk maximum=<n>
> ```
> ## <a name="parameters"></a>Parámetros
> 
> |  Parámetro  |                      Descripción                      |
> |-------------|-------------------------------------------------------|
> | máximo = <n> | Especifica el nuevo tamaño del disco duro virtual en megabytes (MB). |
> 
> ## <a name="remarks"></a>Comentarios
> - Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un volumen y desplazar el foco a él.
>   ## <a name="BKMK_Examples"></a>Example
>   Para expandir el disco duro virtual seleccionado a 20 GB, escriba:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
> - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> - [Attach vDisk](attach-vdisk.md)
> - [Compact vDisk](compact-vdisk.md)

-   [Detach vDisk](detach-vdisk.md)
-   [detalles del vDisk](detail-vdisk.md)
-   [Merge vDisk](merge-vdisk.md)
-   [seleccionar vDisk](select-vdisk.md)
-   [list_1](list_1.md)
