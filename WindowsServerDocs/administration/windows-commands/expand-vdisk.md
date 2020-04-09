---
title: expandir vDisk
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 272714372a35f7f205b5a2e70cb2f2669b3a0634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844898"
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
> ### <a name="parameters"></a>Parámetros
> 
> |  Parámetro  |                      Descripción                      |
> |-------------|-------------------------------------------------------|
> | máximo =<n> | Especifica el nuevo tamaño del disco duro virtual en megabytes (MB). |
> 
> ## <a name="remarks"></a>Comentarios
> - Para que esta operación se realice correctamente, se debe seleccionar y desasociar un disco duro virtual. Use el comando **Select vDisk** para seleccionar un volumen y desplazar el foco a él.
>   ## <a name="examples"></a><a name=BKMK_Examples></a>Example
>   Para expandir el disco duro virtual seleccionado a 20 GB, escriba:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
> - - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> - [Attach vDisk](attach-vdisk.md)
> - [Compact vDisk](compact-vdisk.md)

-   [Detach vDisk](detach-vdisk.md)
-   [detalles del vDisk](detail-vdisk.md)
-   [Merge vDisk](merge-vdisk.md)
-   [seleccionar vDisk](select-vdisk.md)
-   [list_1](list_1.md)
