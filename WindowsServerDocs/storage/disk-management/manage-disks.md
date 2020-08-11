---
title: Administración de discos
description: En este artículo se describe cómo administrar discos
ms.date: 06/07/2019
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 27ce56c66ce22d64001facce899072dc64a1da16
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961519"
---
# <a name="manage-disks"></a>Administración de discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema y sus temas secundarios se explica el uso de Administración de discos para administrar los discos de un equipo y se incluye información acerca de la inicialización de discos nuevos, de la conversión de discos entre los estilos distintos de partición y de la forma en que Windows controla el estado en línea de los discos nuevos.

## <a name="online-and-offline-status"></a>Estado de conexión y sin conexión

Administración de discos muestra si un disco está en línea (disponible) o sin conexión.

En Windows, de manera predeterminada, todos los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura. En Windows Server, de manera predeterminada, los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura, salvo que estén en un bus compartido (por ejemplo, SCSI, iSCSI, Serial Attached SCSI o Fibre Channel). Los discos de un bus compartido están sin conexión la primera vez que se detectan.

Si un disco está sin conexión, debes conectarlo para poder inicializarlo o crear volúmenes en él.

Para poner un disco en línea o dejarlo sin conexión, haz clic con el botón derecho en el nombre del mismo y eligiendo la acción que te parezca oportuna.

## <a name="see-also"></a>Consulte también

-   [Inicializar nuevos discos](initialize-new-disks.md)
-   [Mover discos a otro equipo](move-disks-to-another-computer.md)
-   [Cambiar un disco dinámico por un disco básico](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Cambiar un disco de registro de arranque maestro por un disco de tabla de particiones GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Cambiar un disco de tabla de particiones GUID por un disco de registro de arranque maestro](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Administrar discos duros virtuales](manage-virtual-hard-disks.md)