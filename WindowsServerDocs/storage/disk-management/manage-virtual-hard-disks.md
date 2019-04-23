---
title: Administrar discos duros virtuales (VHD)
description: En este artículo se describe cómo administrar discos duros virtuales
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2e371710752d59ebc7a1f8aa2dad3d9189872c47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827116"
---
# <a name="manage-virtual-hard-disks-vhd"></a>Administrar discos duros virtuales (VHD)

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe cómo crear, exponer y ocultar discos duros virtuales con la Administración de discos. Los discos duros virtuales (VHD) son archivos de disco duro virtualizados que, una vez montados, aparecen y funcionan de forma prácticamente idéntica a una unidad de disco duro física. Se usan con mayor frecuencia con máquinas virtuales Hyper-V. 

## <a name="viewing-vhds-in-disk-management"></a>Ver VHD en la Administración de discos

Los VHD aparecen como discos físicos en la Administración de discos. Cuando se expone un VHD (es decir, cuando está disponible para que el sistema lo use), se muestra en azul. Si el disco está oculto (es decir, no está disponible), su icono se convierte en gris.

## <a name="creating-a-vhd"></a>Crear un VHD

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

**Para crear un disco duro virtual**

1.  En el menú **Acción**, selecciona **Crear VHD**.

2.  En el cuadro diálogo **Crear y exponer disco duro virtual**, especifica la ubicación en el equipo físico en el que quieres almacenar el archivo VHD y el tamaño del VHD.

3.  En **Formato del disco duro virtual**, selecciona **Expansión dinámica** o **Tamaño fijo** y luego haz clic en **Aceptar**.

## <a name="attaching-and-detaching-a-vhd"></a>Exponer y ocultar un VHD

Para tener un disco duro virtual disponible para su uso (uno que acabas de crear u otro VHD existente): 

1. En el menú **Acción**, selecciona **Exponer VHD**.

2. Especifica la ubicación del VHD, mediante una ruta de acceso completo.

Para desasociar el disco duro virtual, lo que no está disponible: Haga clic en el disco, seleccione **ocultar VHD**y, a continuación, haga clic en **Aceptar**. Ocultar un VHD no elimina el disco duro virtual ni todos los datos almacenados en él.

## <a name="additional-considerations"></a>Consideraciones adicionales

-   La ruta de acceso especifica la ubicación para el disco duro virtual debe ser completo y no puede estar en el \\directorio de Windows.
-   El tamaño mínimo para un VHD es 3 megabytes (MB).
-   Un VHD solo puede ser un disco básico.
-   Dado que un VHD se inicializa cuando se crea, crear un disco duro virtual de tamaño fijo grande puede tardar algún tiempo.
