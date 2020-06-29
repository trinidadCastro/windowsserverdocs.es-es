---
title: Create a File Screen
description: En este artículo se describe cómo crear un filtro de archivos.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e7827f1e80b1cfe2288bee968cc3c4e3cd428e15
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474382"
---
# <a name="create-a-file-screen"></a>Create a File Screen

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al crear un nuevo filtro de archivos, tiene la opción de guardar una plantilla de filtro de archivos basada en las propiedades personalizadas de filtro de archivos que defina. La ventaja de este sistema es que se mantiene un vínculo entre los filtros de archivos y la plantilla empleada para crearlos, de modo que en lo sucesivo, los cambios que se realicen en la plantilla se aplicarán también a todos los filtros de archivos que deriven de ella. Se trata de una característica que simplifica la implementación de los cambios de la Directiva de almacenamiento, ya que proporciona un punto central en el que puede realizar todas las actualizaciones.

## <a name="to-create-a-file-screen-with-custom-properties"></a>Para crear un filtro de archivos con propiedades personalizadas

1.  En **Administración del filtrado de archivos**, haga clic en el nodo **filtros de archivos** .

2.  Haga clic con el botón derecho en **filtros de archivos** y haga clic en **crear filtro de archivos** (o seleccione **crear filtro de archivos** en el panel **acciones** ). Se abrirá el cuadro de diálogo **crear filtro de archivos** .

3.  En **ruta de acceso al filtro de archivos**, escriba el nombre de o busque la carpeta a la que se aplicará el filtro de archivos. El filtro de archivos se aplicará a la carpeta seleccionada y a todas sus subcarpetas.

4.  En **¿Cómo desea configurar las propiedades del filtro de archivos?**, haga clic en **definir propiedades personalizadas del filtro de archivos**y, a continuación, haga clic en **propiedades personalizadas**. Se abrirá el cuadro de diálogo **propiedades del filtro de archivos** .

5.  Si desea copiar las propiedades de una plantilla existente para usarla como base para el filtro de archivos, seleccione una plantilla en la lista desplegable **copiar propiedades de la plantilla** . A continuación, haga clic en **copiar**.

    En el cuadro de diálogo **propiedades del filtro de archivos** , modifique o establezca los siguientes valores en la pestaña **configuración** :

6.  En **tipo de filtrado**, haga clic en la opción **filtrado activo** o **filtrado pasivo** . (El filtrado activo impide a los usuarios guardar los archivos que pertenecen a los grupos de archivos bloqueados y genera notificaciones cuando los usuarios intentan guardar archivos no autorizados. El filtrado pasivo envía notificaciones configuradas, pero no impide a los usuarios guardar archivos.)

7.  En **grupos de archivos**, seleccione cada grupo de archivos que desee incluir en el filtro de archivos. (Para activar la casilla del grupo de archivos, haga doble clic en su etiqueta).

    Si desea ver los tipos de archivo que un grupo de archivos incluye y excluye, haga clic en la etiqueta del grupo de archivos y, a continuación, haga clic en **Editar**. Para crear un nuevo grupo de archivos, haga clic en **crear**.

8.  Además, puede configurar el **Administrador de recursos del servidor de archivos** para generar una o varias notificaciones estableciendo las opciones de las pestañas **mensaje de correo electrónico**, **registro de eventos**, **comando**e **Informe** . Para obtener más información sobre las opciones de notificación de filtro de archivos, vea [crear una plantilla de filtro de archivos](create-file-screen-template.md).

9.  Después de seleccionar todas las propiedades del filtro de archivos que desee usar, haga clic en **Aceptar** para cerrar el cuadro de diálogo **propiedades del filtro de archivos** .

10. En el cuadro de diálogo **crear filtro de archivos** , haga clic en **crear** para guardar el filtro de archivos. Se abrirá el cuadro de diálogo **Guardar propiedades personalizadas como plantilla** .

11. Seleccione el tipo de filtro de archivos personalizado que desea crear:

    -   Para guardar una plantilla basada en estas propiedades personalizadas (recomendado), haga clic en **guardar las propiedades personalizadas como plantilla** y escriba un nombre para la plantilla. Esta opción aplicará la plantilla al nuevo filtro de archivo; además podrá usar la plantilla para crear otros filtros de archivos en el futuro. Esto le permitirá después actualizar los filtros de archivos automáticamente mediante la actualización la plantilla.
    -   Si no desea guardar una plantilla al guardar el filtro de archivos, haga clic en **guardar el filtro de archivos personalizado sin crear una plantilla**.

12. Haga clic en **OK**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Definir grupos de archivos para el filtrado](define-file-groups-for-screening.md)
-   [Crear una plantilla de filtro de archivos](create-file-screen-template.md)
-   [Editar las propiedades de la plantilla de filtro de archivos](edit-file-screen-template-properties.md)


