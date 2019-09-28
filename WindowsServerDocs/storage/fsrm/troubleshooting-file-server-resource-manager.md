---
title: Solución de problemas del Administrador de recursos del servidor de archivos
description: En este artículo se describe cómo solucionar problemas comunes al usar el Administrador de recursos del servidor de archivos
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0f30bcfd07f28ecd3fa1618e4f5d89b7a0b4d14f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403089"
---
# <a name="troubleshooting-file-server-resource-manager"></a>Solución de problemas del Administrador de recursos del servidor de archivos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Esta sección presenta problemas comunes que pueden aparecer al usar el Administrador de recursos del servidor de archivos.

> [!Note]
> Una buena opción para solucionar problemas es buscar los registros de eventos que ha generado el Administrador de recursos del servidor de archivos. Todas las entradas de registro de eventos del Administrador de recursos del servidor de archivos pueden encontrarse en el registro de eventos de la **Aplicación** en el origen **SRMSVC**

## <a name="i-am-not-receiving-e-mail-notifications"></a>No estoy recibiendo notificaciones por correo electrónico.

-   **Causa**: Las opciones de correo electrónico no se han configurado o no se han configurado correctamente.

-   **Solución**: En la pestaña **notificaciones de correo electrónico** , en el cuadro de diálogo **Opciones de administrador de recursos de servidor de archivos** , compruebe que el servidor SMTP y los destinatarios de correo electrónico predeterminados se hayan especificado y sean válidos. Envía un correo electrónico de prueba para confirmar que la información sea correcta y que el servidor SMTP que se usa para enviar las notificaciones funciona correctamente. Para obtener más información, consulta [Configurar notificaciones por correo electrónico](configure-email-notifications.md).


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>Solo estoy recibiendo una notificación por correo electrónico, aunque el evento que desencadenó la notificación ha ocurrido varias veces seguidas.

-   **Causa**: Cuando un usuario intenta guardar varias veces un archivo que está bloqueado o guarda un archivo que supera un umbral de cuota, y hay una notificación de correo electrónico configurada para ese filtrado de archivos o evento de cuota, solo se envía un mensaje de correo electrónico al Administrador durante un período de 60 minutos.  predeterminada. Esto evita una gran cantidad de mensajes redundantes en la cuenta de correo electrónico del administrador.

-   **Solución**: En la pestaña **límites de notificación** , en el cuadro de diálogo opciones de administrador de recursos de servidor de **archivos** , puede establecer un límite de tiempo para cada uno de los tipos de notificación: correo electrónico, registro de eventos, comando e informe. Cada límite especifica un período de tiempo que debe pasar antes de que otra notificación del mismo tipo se genere para un mismo problema. Para obtener más información, consulta [Configurar límites de notificación](configure-notification-limits.md).


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>Los informes de almacenamiento siguen fallando y hay poca o ninguna información disponible en el Registro de eventos en relación con el origen del error.

-   **Causa**: Es posible que el volumen donde se guardan los informes esté dañado.
-   **Solución**: Ejecute **CHKDSK** en el volumen e intente generar de nuevo los informes.

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>Los informes de Auditoría de filtrado de archivos no incluyen ninguna información.

-   **Causa**: Una o varias de las siguientes pueden ser la causa:
    -   La base de datos de auditoría no está configurado para registrar la actividad de filtrado de archivos.
    -   La base de datos de auditoría está vacía (es decir, no se ha registrado ninguna actividad de filtrado de archivos).
    -   Los parámetros del informe de Auditoría de filtrado de archivos no seleccionan datos de la base de datos de auditoría.
    
-   **Solución**: En la pestaña **Auditoría de filtro de archivos** , en el cuadro de diálogo opciones de administrador de recursos de servidor de **archivos** , compruebe que la casilla **registrar la actividad de filtrado de archivos en la base de datos de auditoría** está activada.
    -   Para obtener más información sobre el registro de la actividad de filtrado de archivos, consulta [Configurar Auditoría de filtrado de archivos](configure-file-screen-audit.md).
    -   Para configurar los parámetros predeterminados del informe de Auditoría de filtrado de archivos, consulta [Configurar informes de almacenamiento](configure-storage-reports.md).
    -   Para editar los parámetros de informe de las tareas de informe programadas o de los informes a petición, consulta [Administración de informes de almacenamiento](storage-reports-management.md).

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>Los valores "Usada" y "Disponible" de algunas de las cuotas que he creado no corresponden con el ajuste de "Límite" real.

-   **Causa**: Puede tener una cuota anidada, donde la cuota de una subcarpeta deriva un límite más restrictivo de la cuota de su carpeta principal. Por ejemplo, podrías tener un límite de cuota de 100 megabytes (MB) aplicado a una carpeta principal y una cuota de 200 MB aplicada de forma independiente a cada una de sus subcarpetas. Si la carpeta principal tiene un total de 50 MB de datos almacenados en ella (la suma de los datos almacenados en sus subcarpetas), cada una de las subcarpetas mostrará solo 50 MB de espacio disponible.

-   **Solución**: En **Administración de cuotas**, haga clic en **cuotas**. En el panel **Resultados**, selecciona la entrada de cuota que estás tratando de solucionar. En el panel **Acciones**, haz clic en **Ver las cuotas que afectan a la carpeta** y luego busca las cuotas que se apliquen a las carpetas principales. Esto te permitirá identificar las cuotas de las carpetas principales con un ajuste de límite de almacenamiento inferior al de la cuota que has seleccionado.

