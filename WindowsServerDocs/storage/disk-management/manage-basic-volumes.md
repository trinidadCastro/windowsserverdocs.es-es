---
title: Administración de volúmenes básicos
description: En este artículo se describe cómo administrar volúmenes básicos.
ms.date: 10/12/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2eee5820891c108475c58c9ad024cd7c00391e43
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385917"
---
# <a name="manage-basic-volumes"></a>Administración de volúmenes básicos

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un disco básico es un disco físico que incluye particiones principales, particiones extendidas o unidades lógicas. Las particiones y unidades lógicas en los discos básicos se conocen como volúmenes básicos. Solo puedes crear volúmenes básicos en los discos básicos.

Puedes agregar más espacio a las particiones existentes principales y unidades lógicas ampliándolas con espacio sin asignar, contiguo y adyacente en el mismo disco. Para extender un volumen básico, debe estar formateado con el sistema de archivos NTFS. Puedes ampliar una unidad lógica en el espacio disponible contiguo en la partición extendida que lo contiene. Si se extiende a una unidad lógica más allá del espacio disponible en la partición extendida, la partición extendida crece para incluir la unidad lógica siempre que la partición extendida vaya seguida de un espacio contiguo y sin asignar.

## <a name="see-also"></a>Consulte también

-   [Asignar una ruta de acceso de carpeta de punto de montaje a una unidad](assign-a-mount-point-folder-path-to-a-drive.md)
-   [Extender un volumen básico](extend-a-basic-volume.md)
-   [Reducir un volumen básico](shrink-a-basic-volume.md)