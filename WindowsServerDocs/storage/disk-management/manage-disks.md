---
title: Administrar discos
description: En este artículo se describe cómo administrar discos
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 344dd363e970b195abe20fcb69e741c450fc7a21
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812410"
---
# <a name="manage-disks"></a>Administrar discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema y sus temas secundarios analiza el uso de administración de discos para administrar los discos en un equipo e incluye información acerca de cómo inicializar nuevos discos, conversión de discos entre los estilos de partición diferentes, y cómo Windows administra el estado de conexión de discos nuevos.

## <a name="online-and-offline-status"></a>Estado de conexión y sin conexión

Administración de discos muestra si un disco está en línea (disponible), o sin conexión.

En Windows, de manera predeterminada, todos los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura. En Windows Server, de manera predeterminada, los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura a menos que estén en un bus compartido (por ejemplo, SCSI, iSCSI, Serial Attached SCSI o Fibre Channel). Los discos en un bus compartido están desconectados de la primera vez que se detectan.

Si un disco está desconectado, debes conectarlo antes de poder inicializarlo o crear volúmenes en él.

Para conectar un disco o dejarlo sin conexión, haga clic en el nombre del disco y, a continuación, elija la acción apropiada.

## <a name="see-also"></a>Vea también

-   [Inicializar nuevos discos](initialize-new-disks.md)
-   [Mover discos a otro equipo](move-disks-to-another-computer.md)
-   [Cambiar un disco dinámico por un disco básico](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Cambiar un disco de registro de arranque maestro a un disco con tabla de particiones GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Cambiar un disco de tabla de particiones GUID a un disco de registro de arranque maestro](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Administrar discos duros virtuales](manage-virtual-hard-disks.md)