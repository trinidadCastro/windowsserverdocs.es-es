---
title: Cambiar una letra de unidad
description: Cómo cambiar o asignar una letra de unidad en Windows mediante administración de discos.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 93da32a204c42b3f4fb503349ff732c9c050db31
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192630"
---
# <a name="change-a-drive-letter"></a>Cambiar una letra de unidad

> **Se aplica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si no le gusta la letra de unidad asignada a una unidad, o si tiene una unidad que todavía no tiene una letra de unidad, puede usar Administración de discos para cambiarlo.

> [!IMPORTANT]
> Si cambia la letra de unidad de una unidad donde están instaladas Windows o aplicaciones, aplicaciones podrían tener problemas para ejecutar o buscar esa unidad. Por este motivo, se recomienda que no cambie la letra de unidad de una unidad en que están instaladas Windows o aplicaciones.

Aquí le mostramos cómo cambiar la letra de unidad (que en su lugar para montar la unidad en un valor vacío carpeta para que aparezca como otra carpeta, consulte [asignar una ruta de acceso de carpeta de punto de montaje a una unidad](assign-a-mount-point-folder-path-to-a-drive.md)).

1. Abra Administración de discos con permisos de administrador. 
    Para ello, en el cuadro de búsqueda en la barra de tareas, escriba **administración de discos**, seleccione y mantenga (o secundario) **administración de discos**, a continuación, seleccione **ejecutar como administrador**  >  **Sí**. Si no puede abrirlo como administrador, escriba **administración de equipos** en su lugar y, a continuación, vaya a **almacenamiento** > **administración de discos**.
1. En administración de discos, haga clic en la unidad para el que desea cambiar o agregar una letra de unidad y, a continuación, seleccione **cambiar la letra de unidad y las rutas de acceso**.

    ![Administración de discos que se muestra una unidad](media/change-drive-letter.png)
    > [!TIP]
    > Si no ve el **cambiar la letra de unidad y las rutas de acceso** opción o bien está atenuada, es posible el volumen no está listo para recibir una letra de unidad, que puede ser el caso si la unidad está sin asignar y debe ser [inicializado](initialize-new-disks.md). O bien, quizá lo ha no diseñados para tener acceso, que es el caso de las particiones de sistema EFI y de recuperación. Si ha confirmado que tiene un volumen formateado con una letra de unidad que se puede acceder y que todavía no puede cambiarlo, lamentablemente en este tema probablemente no le puede ayudar, por lo que se recomienda [ponerse en contacto con Microsoft](https://support.microsoft.com/contactus/) o el fabricante de su PC para obtener más ayuda.

1. Para cambiar la letra de unidad, seleccione **cambiar**. Para agregar una letra de unidad si la unidad todavía no tiene uno, seleccione **agregar**.

    ![El cuadro de diálogo Cambiar la letra de unidad y rutas de acceso](media/change-drive-letter2.png)
1. Seleccione la nueva letra de unidad, seleccione **Aceptar**y, a continuación, seleccione **Sí** cuando se le solicite acerca de cómo los programas que dependen de la letra de unidad podrían no ejecutarse correctamente.

    ![El cuadro de diálogo Cambiar la letra de unidad o ruta de acceso que se muestra si cambia la letra de unidad](media/change-drive-letter3.png)