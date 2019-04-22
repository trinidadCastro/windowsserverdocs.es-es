---
title: Configurar Auditoría de filtrado de archivos
description: En este artículo se describe cómo configurar la opción Auditoría de filtrado de archivos para generar el informe de Auditoría de filtrado de archivos
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 89592a9e1f61374d2d909678a91dc4a06e0b1972
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824476"
---
# <a name="configure-file-screen-audit"></a>Configurar Auditoría de filtrado de archivos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al usar el Administrador de recursos del servidor de archivos, puedes registrar la actividad de filtrado de archivos en una base de datos de auditoría. La información guardada en esta base de datos se usa para generar el informe de Auditoría de filtrado de archivos.

> [!Important]
> Si la casilla de verificación **Registrar la actividad de filtrado de archivos en la base de datos de auditoría** está desactivada, los informes de Auditoría de filtrado de archivos no incluirán ninguna información.

## <a name="to-configure-file-screen-audit"></a>Para configurar la opción de Auditoría de filtrado de archivos

1.  En el árbol de consola, haz clic con el botón derecho en **Administrador de recursos del servidor de archivos** y luego haz clic en **Configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2.  En la pestaña **Auditoría de filtro de archivos**, selecciona la casilla de verificación **Registrar la actividad de filtrado de archivos en la base de datos de auditoría**.

3.  Haga clic en **Aceptar**. Toda la actividad de filtrado de archivos ahora se almacenará en la base de datos de auditoría y se puede ver mediante la ejecución de un informe de Auditoría de filtrado de archivos.

## <a name="see-also"></a>Vea también

-   [Opciones del Administrador de recursos del servidor de archivos de configuración](setting-file-server-resource-manager-options.md)
-   [Administración de informes de almacenamiento](storage-reports-management.md)