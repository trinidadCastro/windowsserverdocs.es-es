---
title: Configurar límites de notificación
description: En este artículo se describe cómo agregar límites de tiempo a diversos tipos de notificación
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: dba5b3b3c8b651935ec3c69695583d04087b7f2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826316"
---
# <a name="configure-notification-limits"></a>Configurar límites de notificación

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para reducir el número de notificaciones que se acumulan al superar repetidamente un umbral de cuota o al intentar guardar un archivo no autorizado, el Administrador de recursos del servidor de archivos aplica límites de tiempo a los siguientes tipos de notificación:

-   Correo electrónico
-   Registro de eventos
-   Comando
-   Informe

Cada límite especifica un período de tiempo antes de que otra notificación configurada del mismo tipo se genere para un problema idéntico.

Se establece un límite predeterminado de 60 minutos para cada tipo de notificación, pero puedes cambiar estos límites. El límite se aplica a todas las notificaciones de un tipo determinado, ya sea si se generan mediante umbrales de cuota o mediante eventos de filtrado de archivos.

## <a name="to-specify-a-standard-notification-limit-for-each-notification-type"></a>Para especificar un límite de notificación estándar de cada tipo de notificación

1.  En el árbol de consola, haz clic con el botón derecho en **Administrador de recursos del servidor de archivos** y luego haz clic en **Configurar opciones**. Se abre el cuadro de diálogo **Opciones del Administrador de recursos del servidor de archivos**.

2.  En la pestaña **Límites de notificación**, escribe un valor en minutos para cada tipo de notificación que se muestra.

3.  Haga clic en **Aceptar**.

> [!Note]
> Para personalizar los límites de tiempo que están asociados a las notificaciones de una cuota o filtro de archivos específicos, puedes usar las herramientas de línea de comandos del Administrador de recursos del servidor de archivos **Dirquota.exe** y **Filescrn.exe**, o usar los cmdlets [Administrador de recursos del servidor de archivos](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager).

## <a name="see-also"></a>Vea también

-   [Opciones del Administrador de recursos del servidor de archivos de configuración](setting-file-server-resource-manager-options.md)
-   [Herramientas de línea de comandos](command-line-tools.md)