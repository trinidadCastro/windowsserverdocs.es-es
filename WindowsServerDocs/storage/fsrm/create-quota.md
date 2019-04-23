---
title: Crear una cuota
description: En este artículo se describe cómo crear una cuota en base a una plantilla
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f3c677f5ebf7dda44f4b99a64d0fbf8d2c72b92e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883196"
---
# <a name="create-a-quota"></a>Crear una cuota

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Las cuotas pueden crearse desde una plantilla o con propiedades personalizadas. El siguiente procedimiento describe cómo crear una cuota que se basa en una plantilla (recomendado). Si tienes que crear una cuota con propiedades personalizadas, puedes guardar estas propiedades como una plantilla para volver a usarla en una fecha posterior.

Cuando creas una cuota, elige una ruta de acceso de cuota, que es un volumen o carpeta al que se aplica el límite de almacenamiento. En una determinada ruta de acceso de cuota, puedes usar una plantilla para crear uno de los siguientes tipos de cuota:

-   Una única cuota que limita el espacio de todo un volumen o carpeta.
-   Una cuota automática, que asigna la plantilla de cuota a una carpeta o volumen. Las cuotas basadas en esta plantilla se generan automáticamente y se aplican a todas las subcarpetas. Para obtener más información sobre cómo crear cuotas automáticas, consulta [Crear una cuota automática](create-auto-apply-quota.md).


> [!Note]
> Si las cuotas se crean exclusivamente a partir de plantillas, es posible administrarlas centralmente actualizando las plantillas en lugar de actualizar cuotas individuales. Luego puedes aplicar cambios a todas las cuotas basadas en la plantilla modificada. Esta característica simplifica la implementación de los cambios de las directivas de almacenamiento, ya que ofrece un punto central donde se pueden llevar a cabo todas las actualizaciones.

## <a name="to-create-a-quota-that-is-based-on-a-template"></a>Para crear una cuota que se basa en una plantilla

1.  En **Administración de cuotas** haga clic en el nodo **Plantillas de cuota**.

2.  En el panel de resultados, selecciona la plantilla en la que basarás tu nueva cuota.

3.  Haz clic con el botón derecho en la plantilla y haz clic en **Crear cuota a partir de una plantilla** (o selecciona **Crear cuota a partir de una plantilla** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear cuota** y también se mostrarán las propiedades de resumen de la plantilla de cuota.

4.  En **Ruta de acceso de cuota**, escribe o busca la carpeta a la que se aplicará la cuota.

5.  Haz clic en la opción **Crear cuota en la ruta de acceso**. Ten en cuenta que las propiedades de la cuota se aplicarán a toda la carpeta.

     > [!Note]
     > Para crear una cuota automática, haz clic en la opción **Aplicar plantilla automáticamente para crear cuotas en subcarpetas nuevas y existentes**. Para obtener más información sobre cuotas automáticas, consulta [Crear una cuota automática](create-auto-apply-quota.md)

6.  En **Derivar propiedades de esta plantilla de cuota**, la plantilla que usaste en el paso 2 para crear tu nueva cuota está preseleccionada (o puedes seleccionar otra plantilla de la lista). Ten en cuenta que las propiedades de la plantilla se muestran en **Resumen de las propiedades de la cuota**.

7.  Haga clic en **Crear**.

## <a name="see-also"></a>Vea también

-   [Administración de cuotas](quota-management.md)
-   [Crear un automáticamente una cuota de aplicación](create-auto-apply-quota.md)
-   [Crear una plantilla de cuota](create-quota-template.md)


