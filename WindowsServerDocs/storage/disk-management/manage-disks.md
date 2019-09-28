---
title: Administración de discos
description: En este artículo se describe cómo administrar discos
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0aac0b78e79949de94ebd20912b8c2b2db167339
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385874"
---
# <a name="manage-disks"></a>Administración de discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema y sus temas secundarios se explica el uso de Administración de discos para administrar los discos de un equipo y se incluye información acerca de la inicialización de discos nuevos, de la conversión de discos entre los estilos distintos de partición y de la forma en que Windows controla el estado en línea de los discos nuevos.

## <a name="online-and-offline-status"></a>Estado de conexión y sin conexión

Administración de discos muestra si un disco está en línea (disponible) o sin conexión.

En Windows, de manera predeterminada, todos los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura. En Windows Server, de manera predeterminada, los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura, salvo que estén en un bus compartido (por ejemplo, SCSI, iSCSI, Serial Attached SCSI o Fibre Channel). Los discos de un bus compartido están sin conexión la primera vez que se detectan.

Si un disco está sin conexión, debes conectarlo para poder inicializarlo o crear volúmenes en él.

Para poner un disco en línea o dejarlo sin conexión, haz clic con el botón derecho en el nombre del mismo y eligiendo la acción que te parezca oportuna.

## <a name="see-also"></a>Consulta también

-   [Inicializar nuevos discos](initialize-new-disks.md)
-   [Mover discos a otro equipo](move-disks-to-another-computer.md)
-   [Cambiar un disco dinámico por un disco básico](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Cambiar un disco de registro de arranque maestro por un disco de tabla de particiones GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Cambiar un disco de tabla de particiones GUID por un disco de registro de arranque maestro](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Administrar discos duros virtuales](manage-virtual-hard-disks.md)