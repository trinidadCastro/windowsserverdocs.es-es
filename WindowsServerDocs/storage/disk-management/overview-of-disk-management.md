---
title: Introducción a la Administración de discos
description: La Administración de discos es una utilidad del sistema de Windows que permite realizar tareas avanzadas de almacenamiento, como la inicialización de una nueva unidad, la ampliación de volúmenes, la reducción de particiones y el cambio de letras de unidad.
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e200901d8ddd59f0c0bc45b34681f3aa41b44923
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474422"
---
# <a name="overview-of-disk-management"></a>Introducción a la Administración de discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La Administración de discos es una utilidad del sistema de Windows que permite realizar tareas avanzadas de almacenamiento. A continuación, se enumeran algunas de las ventajas de la Administración de discos:

- Para configurar una nueva unidad, consulta [Inicializar una nueva unidad](initialize-new-disks.md).
- Para extender un volumen en un espacio que aún no forma parte de un volumen en la misma unidad, consulta [Extender un volumen básico](extend-a-basic-volume.md).
- Para reducir una partición, normalmente para poder extender una partición vecina, consulta [Reducir un volumen básico](shrink-a-basic-volume.md).
- Para cambiar una letra de unidad o asignar una letra de unidad nueva, consulta [Cambiar una letra de unidad](change-a-drive-letter.md).

![Administración de discos que muestra una unidad típica con tres particiones: una partición del sistema de 499 MB, una unidad C más grande para Windows y otra partición de 499 MB para la recuperación](media/disk-management.png)

> [!TIP]
>  Si se produce un error o algo no funciona al seguir estos procedimientos, echa un vistazo al tema [Solución de problemas de Administración de discos](troubleshooting-disk-management.md). Si esto no ayuda, no te alarmes. Hay una gran cantidad de información en el sitio de la [comunidad Microsoft](https://answers.microsoft.com/en-us/windows): intenta buscar la sección [Archivos, carpetas y almacenamiento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) y, si aún necesitas ayuda, publica una pregunta en el sitio y Microsoft u otros miembros de la comunidad intentarán ayudarte. Si tienes comentarios sobre cómo mejorar estos temas, nos encantaría conocer tu opinión. Solo tienes que contestar al aviso *¿Es útil esta página?* y dejar comentarios allí o en el subproceso de comentarios públicos en la parte inferior de este tema.

Estas son algunas tareas comunes que es posible que quieras hacer, pero que usan otras herramientas en Windows:

- Para liberar espacio en disco, consulta [Liberar espacio en unidades de disco en Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Para desfragmentar las unidades, consulta [Desfragmentar el PC con Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Para aprovechar varias unidades de disco duro y agruparlas, de forma similar a una RAID, consulta [Espacios de almacenamiento en Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>Acerca de esas particiones de recuperación adicionales

En caso de que tengas curiosidad (hemos leído sus comentarios), Windows normalmente incluye tres particiones en la unidad principal (normalmente la unidad C:\):

![El disco 0 muestra tres particiones: una partición de sistema EFI, la partición de Windows y una partición de recuperación.](media/windows-partitions.png)

- **Partición de sistema EFI**: la usan los equipos modernos para iniciar (arrancar) el equipo y el sistema operativo.
- **Unidad del sistema operativo de Windows (C:)** : es donde está instalado Windows y, normalmente, donde se ubican el resto de aplicaciones y archivos.
- **Partición de recuperación**: es donde se almacenan las herramientas especiales para ayudarte a recuperar Windows en caso de que tengas problemas para iniciar o de que se produzcan otros problemas graves.

Aunque la Administración de discos puede mostrar la partición de sistema EFI y la partición de recuperación como libre al 100%, no es verdad. Estas particiones normalmente están bastante llenas con archivos muy importantes que el equipo necesita para funcionar correctamente. Es mejor dejarlos trabajar para que inicien el equipo y te ayuden en la recuperación de los problemas.

## <a name="additional-references"></a>Referencias adicionales

- [Administrar discos](manage-disks.md)
- [Administrar volúmenes básicos](manage-basic-volumes.md)
- [Solución de problemas de Administración de discos](troubleshooting-disk-management.md)
- [Opciones de recuperación de Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Buscar archivos perdidos tras la actualización a Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Copias de seguridad y restauración de archivos](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Crear una unidad de recuperación](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Crear un punto de restauración del sistema](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Buscar mi clave de recuperación de BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
