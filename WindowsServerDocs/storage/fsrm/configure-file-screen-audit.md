---
title: Configure File Screen Audit
description: En este artículo se describe cómo configurar la auditoría de filtro de archivos para generar el Auditoría de filtrado de archivos informe.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: cf1824e514c34ee89870daa6d15190bffd822a8b
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475322"
---
# <a name="configure-file-screen-audit"></a>Configure File Screen Audit

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Con el Administrador de recursos del servidor de archivos, puede registrar la actividad de filtrado de archivos en una base de datos de auditoría. La información almacenada en esta base de datos se usa para generar el informe Auditoría de filtrado de archivos.

> [!Important]
> Si la casilla **registrar la actividad de filtrado de archivos en la base de datos de auditoría** está desactivada, la auditoría de filtrado de archivos informes no contendrá ninguna información.

## <a name="to-configure-file-screen-audit"></a>Para configurar la auditoría de filtro de archivos

1.  En el árbol de consola, haga clic con el botón secundario en **servidor de archivos administrador de recursos**y, a continuación, haga clic en **configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2.  En la pestaña **Auditoría de filtro de archivos** , active la casilla registrar la **actividad de filtrado de archivos en la base de datos de auditoría** .

3.  Haga clic en **OK**. En lo sucesivo, toda la actividad de filtrado de archivos se almacenará en la base de datos de auditoría y se podrá ver ejecutando un informe de auditoría de filtrado de archivos.

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)