---
title: Inicialización de discos nuevos
description: Cómo inicializar discos nuevos con Administración de discos y prepararlos para su uso. También incluye vínculos para la solución de problemas.
ms.date: 12/20/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c2cb88d5b30be28a8ab7709e3a3908ce82ae8408
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "75352359"
---
# <a name="initialize-new-disks"></a>Inicialización de discos nuevos

> **Se aplica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Si agregas un disco nuevo a un equipo y no se ve en el Explorador de archivos, es posible que necesites [agregar una letra de unidad](change-a-drive-letter.md) o inicializarlo antes de usarlo. Solo se pueden inicializar aquellas unidades de disco que aún no tengan formato. Al inicializar un disco duro se borra todo lo que tenga y se prepara para que lo use Windows, tras lo que se puede formatear y, después, almacenar archivos en él.

> [!WARNING]
> Si el disco tiene archivos que le interesen, no lo inicialice, ya que los perderá. En su lugar, se recomienda solucionar los problemas del disco de la solución de problemas para ver si se pueden leer los archivos (consulta [El estado de un disco aparece como No inicializado o falta el disco](troubleshooting-disk-management.md#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps)).

## <a name="to-initialize-new-disks"></a>Para inicializar discos nuevos

Aquí se muestra cómo inicializar un disco nuevo mediante Administración de discos. Si prefiere usar PowerShell, utilice el cmdlet [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) en su lugar.

1. Abre Administración de discos con permisos de administrador.
 
    Para ello, en el cuadro de búsqueda de la barra de tareas, escribe **Administración de discos**, selecciona y mantén pulsado (o haz clic con el botón derecho) **Administración de discos** y, después, selecciona **Ejecutar como administrador** > **Sí**. Si no puedes abrirlo como administrador, escribe **en Administración de equipos** en su lugar y, a continuación, vete a **Almacenamiento** > **Administración de discos**.
1. En Administración de discos, haz clic con el botón derecho en el disco que quieres inicializar y luego haz clic en **Inicializar disco** (se muestra aquí). Si el disco aparece como *Sin conexión*, primero haz clic en él con el botón derecho y selecciona **En línea**.

     Ten en cuenta que algunas unidades flash USB no tienen la opción de inicializarse, simplemente se les aplica formato y un [letra de unidad](change-a-drive-letter.md).

    ![Administración de discos mostrando un disco sin formatear en el que se muestra el menú contextual Inicializar disco](media/uninitialized-disk.PNG)
2. En el cuadro de diálogo **Inicializar disco** (se muestra aquí), comprueba que está seleccionado el disco correcto está seleccionado y haz clic en **Aceptar** para aceptar el estilo de partición predeterminado. Si necesita cambiar el estilo de la partición (GPT o MBR) consulte [Acerca de los estilos de partición (GPT y MBR)](#about-partition-styles---gpt-and-mbr).

     El estado del disco cambia durante poco tiempo a **Inicializando** y, después, al estado **En línea**. Si se produce algún error durante la inicialización, consulta [El estado de un disco aparece como No inicializado o falta el disco](troubleshooting-disk-management.md#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

    ![El cuadro de diálogo Inicializar disco con el estilo de partición GPT seleccionado](media/initialize-disk.PNG)

3. Selecciona y mantén pulsado (o haz clic con el botón derecho) el espacio sin asignar de la unidad y, a continuación, selecciona **Nuevo volumen simple**.
4. Selecciona **Siguiente**, especifica el tamaño del volumen (es posible que quieras usar el valor predeterminado, que utiliza toda la unidad) y, a continuación, selecciona **Siguiente**.
5. Especifica la letra de la unidad que deseas asignar al volumen y, a continuación, selecciona **Siguiente**.
6. Especifica el sistema de archivos que deseas usar (normalmente NTFS), selecciona **Siguiente** y, a continuación, **Finalizar**.

## <a name="about-partition-styles---gpt-and-mbr"></a>Acerca de los estilos de partición (GPT y MBR)

Los discos pueden dividirse en varios fragmentos, denominados particiones. Cada partición (incluso solo se tenga una) debe tener un estilo de partición (GPT o MBR). Windows usa el estilo de partición para saber cómo acceder a los datos del disco.

Actualmente lo habitual es que no haya que preocuparse del estilo de partición, ya que Windows utiliza automáticamente el tipo de disco adecuado.

La mayoría de los equipos utilizan el tipo de disco GPT (Tabla de particiones GUID) tanto para los discos duros como para las unidades de estado sólido. GPT es más sólido y permite volúmenes que superan los 2 TB. El tipo de disco MBR (Registro de arranque maestro) es más antiguo y lo usan los equipos de 32 bits, los equipos más antiguos y las unidades extraíbles, como las tarjetas de memoria.

Para convertir un disco MBR a GPT, o viceversa, primero hay que eliminar todos los volúmenes del disco y borrar todo su contenido. Para más información, consulte [Convertir un disco MBR en un disco GPT](change-an-mbr-disk-into-a-gpt-disk.md), o [Convertir un disco MBR en un disco GPT](change-a-gpt-disk-into-an-mbr-disk.md).