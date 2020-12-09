---
title: Configure Notification Limits
description: En este artículo se describe cómo agregar límites de tiempo a varios tipos de notificación.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 625fb9c6a36717e57f3c6fd6da696b8df07fe8c3
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866064"
---
# <a name="configure-notification-limits"></a>Configure Notification Limits

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para reducir la cantidad de notificaciones que se acumulan cuando se supera un umbral de cuota o se intenta guardar un archivo no autorizado con frecuencia, el Administrador de recursos del servidor de archivos aplica límites temporales a los siguientes tipos de notificaciones:

-   Correo electrónico
-   Registro de eventos
-   Get-Help
-   Informe

Cada límite especifica un período de tiempo antes de que se genere otra notificación configurada del mismo tipo para un caso idéntico.

Para cada tipo de notificación se establece un límite predeterminado de 60 minutos, pero estos límites se pueden cambiar. El límite se aplica a todas las notificaciones de un tipo determinado, tanto si se generan mediante umbrales de cuota o eventos de filtrado.

## <a name="to-specify-a-standard-notification-limit-for-each-notification-type"></a>Para especificar un límite de notificación estándar para cada tipo de notificación

1.  En el árbol de consola, haga clic con el botón secundario en **servidor de archivos administrador de recursos** y, a continuación, haga clic en **configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2.  En la pestaña **límites de notificación** , escriba un valor en minutos para cada tipo de notificación que se muestra.

3.  Haga clic en **OK**.

> [!Note]
> Para personalizar los límites de tiempo asociados a las notificaciones de una cuota o un filtro de archivos específicos, puede usar las herramientas de línea de comandos del servidor de archivos Administrador de recursos **Dirquota.exe** y **Filescrn.exe**, o bien usar los cmdlets [Administrador de recursos del servidor de archivos](/powershell/module/fileserverresourcemanager/) .

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)
-   [Herramientas de línea de comandos](command-line-tools.md)
