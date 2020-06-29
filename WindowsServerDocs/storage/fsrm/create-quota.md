---
title: Create a Quota
description: En este artículo se describe cómo crear una cuota basada en una plantilla.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b3513510ef00eec7ea78a3193cf44c25ddb17c7e
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475222"
---
# <a name="create-a-quota"></a>Create a Quota

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Las cuotas pueden crearse a partir de una plantilla o con propiedades personalizadas. El procedimiento siguiente describe cómo crear una cuota basada en una plantilla (recomendado). Si necesita crear una cuota con propiedades personalizadas, puede guardar estas propiedades como una plantilla para reutilizarlas más adelante.

Cuando cree una cuota, elija una ruta de acceso, que es un volumen o una carpeta a los que se aplica el límite de almacenamiento. En una ruta de acceso de cuota determinada, puede usar una plantilla para crear uno de los siguientes tipos de cuota.

-   Una sola cuota que limite el espacio de un volumen o una carpeta en su totalidad.
-   Una cuota automática, que asigna la plantilla de cuota a una carpeta o un volumen. Las cuotas basadas en esta plantilla se generan automáticamente y se aplican a todas las subcarpetas. Para obtener más información sobre cómo crear cuotas automáticas, vea [crear una cuota automática](create-auto-apply-quota.md).


> [!Note]
> Si las cuotas se crean exclusivamente a partir de plantillas, es posible administrarlas centralmente actualizando las plantillas en lugar de las cuotas individuales. A continuación, puede aplicar los cambios a todas las cuotas basadas en la plantilla modificada. Esta característica simplifica la implementación de los cambios de las directivas de almacenamiento ya que proporciona un punto central donde se pueden llevar a cabo todas las actualizaciones.

## <a name="to-create-a-quota-that-is-based-on-a-template"></a>Para crear una cuota basada en una plantilla

1.  En **Administración de cuotas** haga clic en el nodo **Plantillas de cuota**.

2.  En el panel de resultados, seleccione la plantilla sobre la que basará la nueva cuota.

3.  Haga clic con el botón derecho en la plantilla y haga clic en **crear cuota desde plantilla** (o seleccione **crear cuota desde plantilla** en el panel **acciones** ). Se abrirá el cuadro de diálogo **crear cuota** con las propiedades de Resumen de la plantilla de cuota mostrada.

4.  En **ruta de acceso de cuota**, escriba o busque la carpeta a la que se aplicará la cuota.

5.  Haga clic en la opción **crear cuota en ruta de acceso** . Observe que las propiedades de la cuota se aplicarán a toda la carpeta.

     > [!Note]
     > Para crear una cuota automática, haga clic en la opción **aplicar la plantilla automáticamente y crear cuotas en las subcarpetas nuevas y existentes** . Para obtener más información acerca de las cuotas automáticas, consulte [crear una cuota automática](create-auto-apply-quota.md) .

6.  En **derivar propiedades de esta plantilla de cuota**, la plantilla que usó en el paso 2 para crear la nueva cuota está preseleccionada (o puede seleccionar otra plantilla de la lista). Tenga en cuenta que las propiedades de la plantilla se muestran en **Resumen de las propiedades de la cuota**.

7.  Haga clic en **Crear**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de cuotas](quota-management.md)
-   [Crear una cuota automática](create-auto-apply-quota.md)
-   [Crear una plantilla de cuota](create-quota-template.md)


