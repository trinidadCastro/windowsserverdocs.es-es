---
title: Create an Auto Apply Quota
description: En este artículo se describe cómo crear cuotas automáticas basadas en una plantilla de cuota.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 38354a6c6e39f58574a64c752bb86800f3fc3039
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474072"
---
# <a name="create-an-auto-apply-quota"></a>Create an Auto Apply Quota

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Las cuotas automáticas permiten asignar una plantilla de cuota a un volumen o una carpeta principal. El Administrador de recursos del servidor de archivos después generará automáticamente las cuotas basadas en esa plantilla. Se generan cuotas para cada una de las subcarpetas existentes y para las subcarpetas que se creen en el futuro.

Por ejemplo, puede definir una cuota automática para las subcarpetas que se creen a petición para usuarios de perfil móvil o para usuarios nuevos. Cada vez que se cree una subcarpeta, se genera automáticamente una nueva cuota con la plantilla de la carpeta principal. Estas entradas de cuota generadas automáticamente pueden verse como cuotas individuales en el nodo **cuotas** . Cada entrada de cuota puede mantenerse por separado.

## <a name="to-create-an-auto-apply-quota"></a>Para crear una cuota automática

1.  En **Administración de cuotas**, haga clic en el nodo **cuotas** .

2.  Haga clic con el botón secundario en **cuotas**y, a continuación, haga clic en **crear cuota** (o seleccione **crear cuota** en el panel **acciones** ). Se abrirá el cuadro de diálogo **crear cuota** .

3.  En **ruta de acceso de cuota**, escriba el nombre de o busque la carpeta principal a la que se aplicará el perfil de cuota. La cuota automática se aplicará a cada una de las subcarpetas (actuales y futuras) de esta carpeta.

4.  Haga clic en **aplicar plantilla automáticamente y cree cuotas en las subcarpetas nuevas y existentes**.

5.  En **derivar propiedades de esta plantilla de cuota**, seleccione la plantilla de cuota que desea aplicar en la lista desplegable. Tenga en cuenta que las propiedades de cada plantilla se muestran en **Resumen de las propiedades de la cuota**.

6.  Haga clic en **Crear**.

> [!Note]
> Para comprobar todas las cuotas generadas automáticamente, seleccione el nodo **cuotas** y, a continuación, seleccione **Actualizar**. Se mostrará una cuota individual para cada subcarpeta y el perfil de cuota automática del volumen o la carpeta principal.

## <a name="additional-references"></a>Referencias adicionales

-   [Administración de cuotas](quota-management.md)
-   [Editar propiedades de cuota automática](edit-auto-apply-quota-properties.md)