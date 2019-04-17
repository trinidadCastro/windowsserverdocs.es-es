---
title: Configurar informes de almacenamiento
description: "En este artículo se describe cómo configurar los parámetros predeterminados de los informes de almacenamiento"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f62109a8d3ea3e4e6386956789d276f9aa911e80
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
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

5.  Repite los pasos del 2al4 para cada tipo de informe que deseas editar.

6. Para ver una lista de los parámetros predeterminados de todos los informes, haz clic en **Revisar informes**. Luego haz clic en **Cerrar**.

7.  Haz clic en **Aceptar**.

## <a name="see-also"></a>Consulta también

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)