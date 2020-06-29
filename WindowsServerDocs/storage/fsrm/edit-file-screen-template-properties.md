---
title: Edit File Screen Template Properties
description: En este artículo se describe cómo editar las propiedades de la plantilla de filtro de archivos.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: cb0ab9105aeded58b2ef3de9e5eb86fe823d6b5b
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473532"
---
# <a name="edit-file-screen-template-properties"></a>Edit File Screen Template Properties

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al realizar cambios en una plantilla de filtro de archivos, tiene la opción de extender dichos cambios a los filtros de archivos creados con la plantilla de filtro de archivos original. Puede elegir entre modificar sólo los filtros de archivos que coinciden con la plantilla original o modificar todos los que se derivan de la plantilla original, con independencia de las modificaciones que se hayan realizado en ellos desde que se crearon. Esta característica simplifica el proceso de actualización de las propiedades de los filtros de archivos proporcionando un punto central en el que realizar todos los cambios.

> [!Note]
> Si aplica los cambios a todos los filtros de archivos derivados de la plantilla original, sobrescribirá todas las propiedades personalizadas de filtro de archivos que haya creado.

## <a name="to-edit-file-screen-template-properties"></a>Para editar las propiedades de la plantilla de filtro de archivos

1.  En **plantillas de filtro de archivos**, seleccione la plantilla que desea modificar.

2.  Haga clic con el botón derecho en la plantilla de filtro de archivos y haga clic en **Editar propiedades** de la plantilla (o en el panel **acciones** , en **plantillas de filtro de archivos seleccionadas**, seleccione **Editar propiedades de plantilla**). Se abrirá el cuadro de diálogo Propiedades de la **plantilla de filtro de archivos** .

3.  Si desea copiar las propiedades de otra plantilla como base para la plantilla modificada, seleccione una plantilla en la lista desplegable **copiar propiedades de la plantilla** . A continuación, haga clic en **copiar**.

4.  Realice todos los cambios necesarios. La configuración y las opciones de notificación son idénticas a las que están disponibles cuando se crea una plantilla de filtro de archivos.

5.  Cuando haya terminado de editar las propiedades de la plantilla, haga clic en **Aceptar**. Se abrirá el cuadro **de diálogo Actualizar filtros de archivos derivados de la plantilla** .

6.  Seleccione el tipo de actualización que desea aplicar:

    -   Si tiene filtros de archivos que se han modificado desde que se crearon con la plantilla original y no desea cambiarlos, haga clic en **aplicar plantilla solo a los filtros de archivos derivados que coincidan con la plantilla original**. Con esta opción se actualizarán sólo los filtros de archivos que no se hayan editado desde que se crearon con las propiedades de la plantilla original.
    -   Si desea modificar todos los filtros de archivos existentes que se crearon con la plantilla original, haga clic en **aplicar plantilla a todos los filtros de archivos derivados**.
    -   Si desea mantener los filtros de archivos existentes sin cambios, haga clic en no **aplicar plantilla a los filtros de archivos derivados**.

7.  Haga clic en **OK**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Crear una plantilla de filtro de archivos](create-file-screen-template.md)


