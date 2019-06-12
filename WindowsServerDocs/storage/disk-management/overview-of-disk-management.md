---
title: Información general de la Administración de discos
description: Administración de discos es una utilidad del sistema de Windows que le permite realizar tareas avanzadas de almacenamiento, como inicializar una nueva unidad de ampliación de volúmenes, reducir las particiones y cambiar las letras de unidad.
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a3885ae6b09ad431fd1ea5e4c593e02c7bb274d9
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812549"
---
# <a name="overview-of-disk-management"></a>Información general de la Administración de discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administración de discos es una utilidad del sistema de Windows que le permite realizar tareas avanzadas de almacenamiento. Estas son algunas de las cosas que es buena para la administración de discos:

- Para configurar una nueva unidad, vea [inicializar una nueva unidad](initialize-new-disks.md).
- Para extender un volumen en el espacio que no forme parte de un volumen en la misma unidad, vea [extender un volumen básico](extend-a-basic-volume.md).
- Para reducir una partición, normalmente, por lo que puede extender una partición vecina, vea [reducir un volumen básico](shrink-a-basic-volume.md).
- Para cambiar una letra de unidad o asignar una letra de unidad nueva, consulte [cambiar una letra de unidad](change-a-drive-letter.md).

![Administración de discos que se muestra una unidad con tres particiones: una partición de sistema de 499 MB, una unidad más grande de C para Windows y otra partición de 499 MB para la recuperación típica](media/disk-management.png)

> [!TIP]
>  Si se produce un error o algo no funciona al seguir estos procedimientos, eche un vistazo a la [solución de problemas de administración de discos](troubleshooting-disk-management.md) tema. Si esto no ayuda - no se alarme! Hay una gran cantidad de información en el [comunidad Microsoft](https://answers.microsoft.com/en-us/windows) sitio: intente buscar el [archivos, carpetas y almacenamiento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) sección y, si aún necesita ayuda, publique una pregunta en ellos y Microsoft u otros miembros de la Comunidad intentará ayuda. Si tiene comentarios sobre cómo mejorar estos temas, nos encantaría conocer su opinión. Simplemente responda el *es útil esta página?* símbolo del sistema y dejar comentarios allí o en el subproceso de comentarios públicos en la parte inferior de este tema.

Estas son algunas tareas comunes que puedan ser necesarios pero que utilizar otras herramientas en Windows:

- Para liberar espacio en disco, consulte [liberar espacio en disco en Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space).
- Para desfragmentar las unidades de disco, consulte [desfragmentar los equipos Windows 10](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc).
- Para aprovechar varias unidades de disco duro y grupo de ellos juntos, similar a RAID, consulte [espacios de almacenamiento](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

## <a name="about-those-extra-recovery-partitions"></a>Acerca de esas particiones de recuperación adicionales

En caso de que tiene curiosidad (que hemos leído sus comentarios!), Windows normalmente incluyen tres particiones en la unidad principal (normalmente el C:\ unidad):

![Disco 0 que muestra tres particiones: una partición de sistema EFI, la partición de Windows y una partición de recuperación](media/windows-partitions.png)

- **Partición de sistema EFI** -se usa por los equipos modernos para iniciar (arrancar) el equipo y el sistema operativo.
- **Unidad de Windows del sistema operativo (C:)** -esto es donde está instalado Windows, y generalmente se donde se coloca el resto de las aplicaciones y archivos.
- **Partición de recuperación** -aquí es donde se almacenan las herramientas especiales para ayudarle a recuperar Windows en caso de que tiene problemas para iniciar o se ejecuta con otros problemas graves.

Aunque la administración de discos puede mostrar la partición del sistema EFI y la partición de recuperación como libre al 100%, se encuentra. Estas particiones son generalmente bastante completas con archivos muy importantes de que su equipo necesita para funcionar correctamente. Es mejor dejar por sí solo para realizar su trabajo a partir de su equipo y ayudarle a recuperarse de problemas.

## <a name="see-also"></a>Vea también

- [Administrar discos](manage-disks.md)
- [Administrar volúmenes básicos](manage-basic-volumes.md)
- [Solución de problemas de Administración de discos](troubleshooting-disk-management.md)
- [Opciones de recuperación de Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [Encontrar archivos perdidos tras la actualización a Windows 10](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [Copia de seguridad y restaurar los archivos](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [Crear una unidad de recuperación](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [Crear un punto de restauración del sistema](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [Encuentra la clave de recuperación de BitLocker](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
