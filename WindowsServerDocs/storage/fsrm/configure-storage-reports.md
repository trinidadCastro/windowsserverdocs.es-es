---
title: Configurar informes de almacenamiento
description: En este artículo se describe cómo configurar los parámetros predeterminados de los informes de almacenamiento
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d3500f4ea4fc264f3cb663f17c3a50439b9cb454
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394264"
---
# <a name="configure-storage-reports"></a>Configurar informes de almacenamiento

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Puedes configurar los parámetros predeterminados de los informes de almacenamiento. Estos parámetros predeterminados se usan para los informes de incidentes que se generan cuando se produce un evento de filtrado de archivos o de cuota. También se usan para los informes programados y a petición y puedes anular los parámetros predeterminados al definir las propiedades específicas de estos informes.

> [!Important]
> Al cambiar los parámetros predeterminados de un tipo de informe, los cambios afectarán a todos los incidentes de informes y a las tareas de informe programadas existentes que usan los valores predeterminados.

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>Para configurar los parámetros predeterminados de los informes de almacenamiento

1. En el árbol de consola, haz clic con el botón derecho en **Administrador de recursos del servidor de archivos** y luego haz clic en **Configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2. En la pestaña **Informes de almacenamiento**, en **Configurar los parámetros predeterminados**, selecciona el tipo de informe que quieres modificar.

3. Haz clic en **Editar parámetros**.

4. En función del tipo de informe que selecciones, estarán disponibles distintos parámetros de informe para su edición. Realiza todas las modificaciones necesarias y luego haz clic en **Aceptar** para guardarlas como los parámetros predeterminados de ese tipo de informe.

5.  Repite los pasos del 2 al 4 para cada tipo de informe que deseas editar.

6. Para ver una lista de los parámetros predeterminados de todos los informes, haz clic en **Revisar informes**. A continuación, haga clic en **Cerrar**.

7.  Haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)