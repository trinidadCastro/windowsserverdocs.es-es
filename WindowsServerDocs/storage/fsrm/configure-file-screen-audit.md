---
title: Configure File Screen Audit
description: En este artículo se describe cómo configurar la auditoría de filtro de archivos para generar el Auditoría de filtrado de archivos informe.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 822c483fc7f5f4518ca976b1f7d719b95730008f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950694"
---
# <a name="configure-file-screen-audit"></a>Configure File Screen Audit

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Con el Administrador de recursos del servidor de archivos, puede registrar la actividad de filtrado de archivos en una base de datos de auditoría. La información almacenada en esta base de datos se usa para generar el informe Auditoría de filtrado de archivos.

> [!Important]
> Si la casilla **registrar la actividad de filtrado de archivos en la base de datos de auditoría** está desactivada, la auditoría de filtrado de archivos informes no contendrá ninguna información.

## <a name="to-configure-file-screen-audit"></a>Para configurar la auditoría de filtro de archivos

1.  En el árbol de consola, haga clic con el botón secundario en **servidor de archivos administrador de recursos**y, a continuación, haga clic en **configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2.  En la pestaña **Auditoría de filtro de archivos** , active la casilla registrar la **actividad de filtrado de archivos en la base de datos de auditoría** .

3.  Haga clic en **Aceptar**. En lo sucesivo, toda la actividad de filtrado de archivos se almacenará en la base de datos de auditoría y se podrá ver ejecutando un informe de auditoría de filtrado de archivos.

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)