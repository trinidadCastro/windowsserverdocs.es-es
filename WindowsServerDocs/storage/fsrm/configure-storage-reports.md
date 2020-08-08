---
title: Configure Storage Reports
description: En este artículo se describe cómo configurar los parámetros predeterminados para los informes de almacenamiento.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8b9d6a53b5f34c0c053de860895f5c4e48b07c83
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961499"
---
# <a name="configure-storage-reports"></a>Configure Storage Reports

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Puede configurar los parámetros predeterminados para los informes de almacenamiento. Estos parámetros predeterminados se usan para los informes de incidentes que se generan cuando se produce un evento de filtrado de archivos o cuotas. Se emplean también en los informes a petición y programados, y los parámetros predeterminados pueden modificarse cuando se definen las propiedades específicas de estos informes.

> [!Important]
> Al cambiar los parámetros predeterminados de un tipo de informe, los cambios afectan a todos los informes de incidentes y a cualquier tarea de informe programada que use los valores predeterminados.

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>Para configurar los parámetros predeterminados para los informes de almacenamiento

1. En el árbol de consola, haga clic con el botón secundario en **servidor de archivos administrador de recursos**y, a continuación, haga clic en **configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2. En la pestaña **Informes de almacenamiento**, en **Configurar los parámetros predeterminados**, seleccione el tipo de informe que desea modificar.

3. Haga clic en **Editar parámetros**.

4. Según el tipo de informe que seleccione, habrá distintos parámetros de informe disponibles para la edición. Realice todas las modificaciones necesarias y, a continuación, haga clic en **Aceptar** para guardarlas como parámetros predeterminados para ese tipo de informe.

5.  Repita los pasos 2 a 4 para cada tipo de informe que desee editar.

6. Para ver una lista de los parámetros predeterminados para todos los informes, haga clic en **revisar informes**. A continuación, haga clic en **Cerrar**.

7.  Haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)