---
title: Editar propiedades de plantillas de cuotas
description: En este artículo se describe cómo editar las propiedades de plantillas de cuotas para ampliar los cambios en las cuotas creadas a partir de la plantilla original de la cuota
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 37719656e107869b97045af98c1a63744e4f6b38
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403028"
---
# <a name="edit-quota-template-properties"></a>Editar propiedades de plantillas de cuotas

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al realizar cambios en una plantilla de cuota, tienes la opción de ampliar estos cambios a las cuotas que se crearon a partir de la plantilla original de la cuota. Puedes elegir entre modificar solo las cuotas que aún coincidan con la plantilla original o todas las cuotas que se derivaron de la plantilla original, independientemente de las modificaciones que se realizaron en las cuotas desde que se crearon. Esta característica simplifica el proceso de actualización de las propiedades de las cuotas, ya que ofrece un punto central donde se pueden llevar a cabo todos los cambios.

> [!Note]
> Si decides aplicar los cambios a todas las cuotas que se derivaron de la plantilla original, se sobrescribirán las propiedades personalizadas de cuotas que hayas creado.

## <a name="to-edit-quota-template-properties"></a>Para editar propiedades de plantillas de cuotas

1.  En **Plantillas de cuota**, selecciona la plantilla que quieres modificar.

2.  Haz clic con el botón derecho en la plantilla y luego haz clic en **Editar las propiedades de la plantilla** (o en el panel **Acciones**, en **Plantillas de cuota seleccionadas**, selecciona **Editar las propiedades de la plantilla**). Se abrirá el cuadro de diálogo **Propiedades de la plantilla de cuota**.

3.  Realiza todos los cambios necesarios. Las opciones de notificación y configuración son idénticas a las que puedes establecer cuando crear una plantilla de cuota. También puedes copiar las propiedades de una plantilla diferente y modificarlas para esta plantilla.

4.  Cuando hayas terminados de editar las propiedades de la plantilla, haz clic en **Aceptar**. Se abrirá el cuadro de diálogo **Actualizar cuotas derivadas de la plantilla**.

5.  Selecciona el tipo de actualización que quieres aplicar:

    -   Si tienes cuotas que se han modificado desde que se crearon con la plantilla original y no quieres cambiarlas, selecciona **Aplicar plantilla solo a las cuotas derivadas que coinciden con la plantilla original**. Esta opción actualizará solo las cuotas que no se han editado desde que se crearon con la plantilla original.
    -   Si quieres modificar todas las cuotas existentes que se crearon a partir de la plantilla original, selecciona **Aplicar plantilla a todas las cuotas derivadas**.
    -   Si quieres mantener las cuotas existentes sin modificar, selecciona **No aplicar la plantilla a las cuotas derivadas**.

6.  Haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también

-   [Administración de cuotas](quota-management.md)
-   [Crear una plantilla de cuota](create-quota-template.md)


