---
title: Inicializar nuevos discos
description: Cómo inicializar nuevos discos con administración de discos, volverlos a listo para su uso. También incluye vínculos a la solución de problemas.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 61c8eb2321ee2a282345aba01c6d04a128f0448c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819396"
---
# <a name="initialize-new-disks"></a>Inicializar nuevos discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si agrega un disco nuevo a su equipo y no se mostrará en el Explorador de archivos, es posible que deba [agregar una letra de unidad](change-a-drive-letter.md), o lo inicializa antes de usarlo. Solo se puede inicializar una unidad que aún no tiene el formato. Inicializar un disco borra todo su contenido y lo prepara para su uso con Windows, tras el cual puede darle formato y, a continuación, almacenar archivos en él.

> [!WARNING]
> Si el disco ya tiene archivos en ella que le interesen, no inicializarlo - perderá todos los archivos. En su lugar, se recomienda que el disco de la solución de problemas para ver si puede leer los archivos - consulte [estado de un disco no inicializado o el disco falta completamente](troubleshooting-disk-management.md#disk-not-initialized).

## <a name="to-initialize-new-disks"></a>Para inicializar nuevos discos

Aquí le mostramos cómo inicializar un disco nuevo mediante administración de discos. Si prefiere usar PowerShell, use el [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) cmdlet en su lugar.

1. Abra Administración de discos con permisos de administrador. <br>Para ello, en el cuadro de búsqueda en la barra de tareas, escriba **administración de discos**, seleccione y mantenga (o secundario) **administración de discos**, a continuación, seleccione **ejecutar como administrador**  >  **Sí**. Si no puede abrirlo como administrador, escriba **administración de equipos** en su lugar y, a continuación, vaya a **almacenamiento** > **administración de discos**.
1. En administración de discos, haga clic en el disco que desea inicializar y, a continuación, haga clic en **inicializar disco** (que se muestra a continuación). Si el disco aparece como *Offline*, primero haga clic en él y seleccione **Online**.<br>Tenga en cuenta que algunas unidades USB no tienen la opción de inicializarse, simplemente se obtener aplica formato y un [letra de unidad](change-a-drive-letter.md).

    ![Administración de discos que se muestra un disco sin formatear con el menú contextual de inicializar disco mostrado](media\uninitialized-disk.PNG)
2. En el **inicializar disco** cuadro de diálogo (se muestra aquí), compruebe para asegurarse de que el disco correcto está seleccionado y, a continuación, haga clic en **Aceptar** para aceptar el estilo de partición predeterminado. Si necesita cambiar el estilo de partición (GPT o MBR) consulte [sobre el estilo de partición - GPT y MBR](#about-partition-styles-GPT-and-MBR).<br>El estado del disco cambiará entonces brevemente a **inicializando** y, a continuación, en el **Online** estado. Si la inicialización se produce un error por algún motivo, consulte [estado de un disco no inicializado o el disco falta completamente](troubleshooting-disk-management.md#disk-not-initialized).

    ![El cuadro de diálogo inicializar disco con el estilo de partición GPT seleccionado](media\initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>Acerca de los estilos de partición - GPT y MBR

Los discos pueden dividirse en varias secciones, denominadas particiones. Cada partición: incluso si sólo tiene uno: debe tener un estilo de partición - GPT o MBR. Windows usa el estilo de partición para entender cómo obtener acceso a los datos en el disco.

Fascinante como es probable que no, la conclusión es que hoy en día, que normalmente no tiene que preocuparse sobre el estilo de partición, Windows utiliza automáticamente el tipo de disco adecuado.

La mayoría de los equipos utilizan el tipo de disco de tabla de particiones GUID (GPT) para los discos duros y unidades SSD. GPT es más estable y permite que los volúmenes mayor de 2 TB. Se usa el tipo de disco de registro de arranque maestro (MBR) anterior, los equipos de 32 bits, los equipos más antiguos y unidades extraíbles, como las tarjetas de memoria.

Para convertir un disco MBR a GPT o viceversa, primero debe eliminar todos los volúmenes del disco, borrando todo el contenido en el disco. Para obtener más información, consulte [convertir un disco MBR en un disco GPT](change-an-mbr-disk-into-a-gpt-disk.md), o [convertir un disco GPT en un disco MBR](change-a-gpt-disk-into-an-mbr-disk.md).