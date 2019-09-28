---
title: Editar propiedades de cuota automática
description: En este artículo se describe cómo editar propiedades de cuota automática
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4b4fda5cdfeed8df02fee922c8dc5fddc75c56ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403112"
---
# <a name="edit-auto-apply-quota-properties"></a>Editar propiedades de cuota automática

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al realizar cambios en una cuota automática, tienes la opción de ampliar estos cambios a las cuotas existentes en la ruta de acceso de cuota automática. Puedes elegir entre modificar solo las cuotas que aún coinciden con la cuota automática original o todas las cuotas de la ruta de acceso de cuota automática, independientemente de las modificaciones que se realizaron en las cuotas desde que se crearon. Esta característica simplifica el proceso de actualización de las propiedades de cuotas que se derivaron de una cuota automática, ya que ofrece un punto central donde puede realizar todos los cambios.

> [!Note]
> Si decides aplicar los cambios a todas las cuotas de la ruta de acceso de cuota automática, se sobrescribirán todas las propiedades de cuotas personalizadas que hayas creado.

## <a name="to-edit-an-auto-apply-quota"></a>Para editar una cuota automática

1.  En **Cuotas**, selecciona la cuota automática que quieras modificar. Puedes filtrar las cuotas para mostrar únicamente cuotas automáticas.

2.  Haz clic con el botón derecho en la entrada de cuota y luego haz clic en **Editar las propiedades de la cuota** (o en el panel **Acciones**, en **Cuotas seleccionadas**, selecciona **Editar las propiedades de la cuota**). Se abrirá el cuadro de diálogo **Editar cuota automática**.

3.  En **Derivar propiedades de esta plantilla de cuota**, selecciona la plantilla de cuota que quieres aplicar. Puede revisar las propiedades de cada plantilla de cuota en el cuadro de lista de resumen.

4.  Haga clic en **Aceptar**. Se abrirá el cuadro de diálogo **Actualizar cuotas derivadas de la cuota automática**.

5.  Selecciona el tipo de actualización que quieres aplicar:

    -   Si tienes cuotas que se han modificado desde que se generaron automáticamente y no quieres cambiarlas, selecciona **Emplear la cuota automática solo en las cuotas derivadas que coinciden con la cuota automática original**. Esta opción actualizará solo las cuotas de la ruta de acceso de cuota automático que no se han editado desde que se generaron automáticamente.
    -   Si quieres modificar todas las cuotas existentes de la ruta de acceso de cuota automática, selecciona **Emplear la cuota automática en todas las cuotas derivadas**.
    -   Si quieres mantener las cuotas existentes sin modificar, pero que la cuota automática modificada tenga validez en las nuevas subcarpetas de la ruta de acceso de cuota automática, selecciona **No emplear la cuota automática en las cuotas derivadas**.

6.  Haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también

-   [Administración de cuotas](quota-management.md)
-   [Crear una cuota automática](create-auto-apply-quota.md)


