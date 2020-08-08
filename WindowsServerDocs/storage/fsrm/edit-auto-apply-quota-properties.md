---
title: Edit Auto Apply Quota Properties
description: En este artículo se describe cómo editar las propiedades de la cuota automática
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 558f6b094e97a6196177e728c238f5bb7a38e7a1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957452"
---
# <a name="edit-auto-apply-quota-properties"></a>Edit Auto Apply Quota Properties

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al realizar cambios en una cuota automática, tiene la opción de extender dichos cambios a las cuotas existentes en la ruta de acceso a la cuota automática. Puede elegir entre modificar sólo las cuotas que coinciden con la cuota automática original o modificar todas las de la ruta de acceso a la cuota automática, con independencia de las modificaciones que se hayan realizado en ellas desde que se crearon. Esta característica simplifica el proceso de actualización de las propiedades de las cuotas derivadas de una cuota automática proporcionando un punto central en el que pueden realizarse todos los cambios.

> [!Note]
> Si elige aplicar los cambios a todas las cuotas de la ruta de acceso de la cuota automática, sobrescribirá las propiedades de cuota personalizadas que haya creado.

## <a name="to-edit-an-auto-apply-quota"></a>Para editar una cuota automática

1.  En **cuotas**, seleccione la cuota automática que desea modificar. Puede filtrar las cuotas para que sólo muestren cuotas automáticas.

2.  Haga clic con el botón secundario en la entrada cuota y, a continuación, haga clic en **Editar propiedades de cuota** (o en el panel **acciones** , en **cuotas seleccionadas,** seleccione **Editar propiedades de cuota**). Se abrirá el cuadro de diálogo **Editar cuota automática** .

3.  En **derivar propiedades de esta plantilla de cuota**, seleccione la plantilla de cuota que desea aplicar. Puede revisar las propiedades de cada plantilla de cuota en el cuadro de lista de resumen.

4.  Haga clic en **Aceptar**. Se abrirá el cuadro **de diálogo Actualizar cuotas derivadas de la cuota automática** .

5.  Seleccione el tipo de actualización que desea aplicar:

    -   Si tiene cuotas que se han modificado desde que se generaron automáticamente y no desea cambiarlas, seleccione **aplicar la cuota automática solo a las cuotas derivadas que coinciden con la cuota automática original**. Esta opción actualizará únicamente las cuotas de la ruta de acceso a la cuota automática que no se hayan editado desde que se generaron automáticamente.
    -   Si desea modificar todas las cuotas existentes en la ruta de acceso de la cuota automática, seleccione **aplicar la cuota automática a todas las cuotas derivadas**.
    -   Si desea mantener las cuotas existentes sin cambios, pero hacer que la cuota automática modificada sea efectiva para las nuevas subcarpetas de la ruta de acceso de la cuota automática, seleccione no **aplicar la cuota automática a las cuotas derivadas**.

6.  Haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de cuotas](quota-management.md)
-   [Crear una cuota automática](create-auto-apply-quota.md)


