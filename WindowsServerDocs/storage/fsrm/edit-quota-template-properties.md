---
title: Edit Quota Template Properties
description: En este artículo se describe cómo editar las propiedades de la plantilla de cuota para extender los cambios a las cuotas creadas a partir de la plantilla de cuota original.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4e8a112f26f2b0ffdf8047063411dbb5539f4eb1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961419"
---
# <a name="edit-quota-template-properties"></a>Edit Quota Template Properties

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al realizar cambios en una plantilla de cuota, tiene la opción de extender dichos cambios a las cuotas creadas a partir de la plantilla de cuota original. Puede elegir entre modificar sólo las cuotas que coinciden con la plantilla original o modificar todas las que se derivan de la plantilla original, con independencia de las modificaciones que se hayan realizado en ellas desde que se crearon. Esta característica simplifica el proceso de actualización de las propiedades de las cuotas proporcionando un punto central en el que pueden realizarse todos los cambios.

> [!Note]
> Si elige aplicar los cambios a todas las cuotas derivadas de la plantilla original, sobrescribirá todas las propiedades personalizadas de cuotas que haya creado.

## <a name="to-edit-quota-template-properties"></a>Para editar las propiedades de la plantilla de cuota

1.  En **plantillas de cuota**, seleccione la plantilla que desea modificar.

2.  Haga clic con el botón secundario en la plantilla de cuota y, a continuación, haga clic en **Editar propiedades** de la plantilla (o en el panel **acciones** , en **plantillas de cuota seleccionadas,** seleccione **Editar propiedades de plantilla**). Se abrirá el cuadro de diálogo Propiedades de la **plantilla de cuota** .

3.  Realice todos los cambios necesarios. La configuración y las opciones de notificación son idénticas a las que se pueden establecer cuando se crea una plantilla de cuota. Otra opción es copiar las propiedades de otra plantilla y modificarlas en ésta.

4.  Cuando haya terminado de editar las propiedades de la plantilla, haga clic en **Aceptar**. Se abrirá el cuadro **de diálogo Actualizar cuotas derivadas de la plantilla** .

5.  Seleccione el tipo de actualización que desea aplicar:

    -   Si tiene cuotas que se han modificado desde que se crearon con la plantilla original y no desea cambiarlas, seleccione **aplicar plantilla solo a las cuotas derivadas que coincidan con la plantilla original**. Con esta opción se actualizarán sólo las cuotas que no se hayan editado desde que se crearon con la plantilla original.
    -   Si desea modificar todas las cuotas existentes que se crearon a partir de la plantilla original, seleccione **aplicar plantilla a todas las cuotas derivadas**.
    -   Si desea mantener las cuotas existentes sin cambios, seleccione no **aplicar plantilla a las cuotas derivadas**.

6.  Haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de cuotas](quota-management.md)
-   [Crear una plantilla de cuota](create-quota-template.md)


