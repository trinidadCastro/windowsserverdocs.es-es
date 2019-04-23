---
title: Compactar vdisk
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ba04bdc8c469ef80853b6092b8745defa1f3f19e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877306"
---
# <a name="compact-vdisk"></a>Compactar vdisk

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reduce el tamaño físico de un archivo de disco duro virtual (VHD) de expansión dinámica. Este parámetro es útil porque el aumento del tamaño de los discos duros virtuales de expansión dinámica como agregar archivos, pero no se reduce automáticamente al eliminar archivos.
> [!NOTE]
> Este comando sólo es aplicable a Windows 7 y Windows Server 2008 R2.
## <a name="syntax"></a>Sintaxis
```
compact vdisk
```
## <a name="remarks"></a>Comentarios
-   Debe seleccionarse un VHD de expansión dinámica para que esta operación se realice correctamente. Use la **seleccione vdisk** comando para seleccionar un disco duro virtual y desplace el foco a ella.
-   Sólo puede compactar el VHD que se separa o adjunta como de solo lectura de expansión dinámica.
## <a name="BKMK_Examples"></a>Ejemplos
Para compactar un VHD de expansión dinámica, escriba:
```
compact vdisk
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)

-   [vdisk detallado](detail-vdisk.md)
-   [Desasociar vdisk](detach-vdisk.md)
-   [Expandir vdisk](expand-vdisk.md)
-   [Combinar vdisk](merge-vdisk.md)
-   [select vdisk](select-vdisk.md)
-   [list_1](list_1.md)
