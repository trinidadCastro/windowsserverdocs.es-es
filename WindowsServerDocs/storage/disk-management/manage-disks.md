---
title: Administrar discos
description: "En este artículo se describe cómo administrar discos"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ae96071733b961fbe65551120894c21c633db83e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="manage-disks"></a>Administrar discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema y sus temas secundarios se habla sobre el uso de la Administración de discos para administrar los discos en un equipo y se incluye información acerca de cómo convertir discos entre los estilos distintos de partición y cómo Windows controla el estado de conexión de los discos nuevos.

## <a name="converting-disk-types"></a>Convertir tipos de disco

Aunque la Administración de discos permite cambiar discos entre distintos tipos y estilos de partición, algunas de las operaciones son irreversibles (a menos que se vuelva a formatear la unidad). Debes tener cuidado con el tipo de disco y el estilo de partición que es más apropiado para tu aplicación.

La siguiente tabla muestra las opciones de conversión de discos entre los distintos estilos de partición:

| Tipo de disco | Convertir a MBR  | Convertir a GPT| Convertir a dinámico |
| ---- | --- | --- |--- |
| <p>Registro de arranque maestro (MBR)</p> | <p>--</p> | <p>Permitido (si no hay volúmenes en el disco).</p> | <p>Permitido, pero es posible que el disco no pueda arrancarse. Consulta la Nota.</p> |
| <p>Tabla de particiones GUID (GPT)</p> | <p>Permitido (si no hay volúmenes en el disco).</p> | <p>--</p>  | <p>Permitido, pero es posible que el disco no pueda arrancarse. Consulta la Nota.</p> |


> [!NOTE]
> En un escenario de arranque múltiple, si has arrancado en un sistema operativo y conviertes un disco MBR básico que incluye un sistema operativo alternativo en un disco dinámico, ya no podrás arrancar en el sistema operativo alternativo.

## <a name="online-and-offline-status"></a>Estado de conexión y sin conexión

La Administración de discos muestra el estado de conexión y sin conexión de los discos. 

En Windows, de manera predeterminada, todos los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura. En Windows Server, de manera predeterminada, los discos que se hayan detectado recientemente se conectan con acceso de lectura y escritura a menos que estén en un bus compartido (por ejemplo, SCSI, iSCSI, Serial Attached SCSI o Fibre Channel). Los discos en un bus compartido estarán desconectados la primera vez que se detectan.

Si un disco está desconectado, debes conectarlo antes de poder inicializarlo o crear volúmenes en él. 

-  Conecta un disco o desconéctalo haciendo clic con el botón derecho en el nombre del disco y eligiendo la acción adecuada.

## <a name="see-also"></a>Consulta también

-   [Inicializar nuevos discos](initialize-new-disks.md)
-   [Mover discos a otro equipo](move-disks-to-another-computer.md)
-   [Cambiar un disco dinámico por un disco básico](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Cambiar un disco de registro de arranque maestro por un disco de tabla de particiones GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Cambiar un disco de tabla de particiones GUID por un disco de registro de arranque maestro](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Administrar discos duros virtuales](manage-virtual-hard-disks.md)