---
title: Expandir vdisk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cf8fd0b05ca5baeeee4fadd670adb3169130d04e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837406"
---
# <a name="expand-vdisk"></a>Expandir vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

se expande un disco duro virtual (VHD) para el tamaño que especifique.
> [!NOTE]
> Este comando sólo es aplicable a Windows 7 y Windows Server 2008 R2.
## <a name="syntax"></a>Sintaxis
```
expand vdisk maximum=<n>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|máximo =<n>|Especifica el nuevo tamaño del disco duro virtual en megabytes (MB).|
## <a name="remarks"></a>Comentarios
-   Un disco duro virtual debe estar seleccionado y desconectado para que esta operación se realice correctamente. Use la **seleccione vdisk** comando para seleccionar un volumen y desplace el foco a ella.
## <a name="BKMK_Examples"></a>Ejemplos
Para expandir el disco duro virtual seleccionado en 20 GB, escriba:
```
expand vdisk maximum=20000
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)
-   [compact vdisk](compact-vdisk.md)

-   [Desasociar vdisk](detach-vdisk.md)
-   [vdisk detallado](detail-vdisk.md)
-   [Combinar vdisk](merge-vdisk.md)
-   [select vdisk](select-vdisk.md)
-   [list_1](list_1.md)
