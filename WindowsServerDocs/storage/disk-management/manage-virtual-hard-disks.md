---
title: Administración de discos duros virtuales (VHD)
description: En este artículo se describe cómo administrar discos duros virtuales
ms.date: 10/12/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6ffa7e9dc769b8d8c892d0af1ceae5246df62d3e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385810"
---
# <a name="manage-virtual-hard-disks-vhd"></a>Administración de discos duros virtuales (VHD)

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe cómo crear, exponer y ocultar discos duros virtuales con Administración de discos. Los discos duros virtuales (VHD) son archivos de disco duro virtualizados que, una vez montados, aparecen y funcionan de forma prácticamente idéntica a una unidad de disco duro física. Se usan con mayor frecuencia con máquinas virtuales de Hyper-V. 

## <a name="viewing-vhds-in-disk-management"></a>Visualización de discos duros virtuales (VHD) en Administración de discos

Los VHD aparecen igual que los discos físicos en Administración de discos. Cuando se expone un VHD (es decir, cuando está disponible para que el sistema lo use), este aparece en azul. Si el disco está oculto (es decir, no está disponible), su icono se convierte en gris.

## <a name="creating-a-vhd"></a>Creación de un VHD

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

**Para crear un disco duro virtual**

1.  En el menú **Acción**, selecciona **Crear VHD**.

2.  En el cuadro diálogo **Crear y exponer disco duro virtual**, especifica la ubicación en el equipo físico en el que quieres almacenar el archivo VHD y el tamaño del VHD.

3.  En **Formato del disco duro virtual**, selecciona **Expansión dinámica** o **Tamaño fijo** y, a continuación, haz clic en **Aceptar**.

## <a name="attaching-and-detaching-a-vhd"></a>Exponer y ocultar un VHD

Para tener un disco duro virtual disponible para su uso (uno que acabas de crear u otro VHD existente): 

1. En el menú **Acción**, selecciona **Exponer VHD**.

2. Especifica la ubicación del VHD, mediante una ruta de acceso completa.

Para ocultar el disco duro virtual y hacer que no esté disponible: Haz clic con el botón derecho en el disco, selecciona **Ocultar VHD** y, después, selecciona **Aceptar**. Ocultar un VHD no elimina el disco duro virtual ni todos los datos almacenados en él.

## <a name="additional-considerations"></a>Consideraciones adicionales

-   La ruta de acceso que especifica la ubicación del VHD debe estar completa y no puede estar en el directorio \\Windows.
-   El tamaño mínimo para un VHD es de 3 megabytes (MB).
-   Un VHD solo puede ser un disco básico.
-   Dado que un VHD se inicializa cuando se crea, la creación de un disco duro virtual de tamaño fijo grande puede tardar algún tiempo.
