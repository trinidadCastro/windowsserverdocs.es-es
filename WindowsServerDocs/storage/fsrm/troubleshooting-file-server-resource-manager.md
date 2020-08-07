---
title: Solución de problemas del Administrador de recursos del servidor de archivos
description: En este artículo se describe cómo solucionar problemas comunes al usar el administrador de recursos del servidor de archivos.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: fbd44938f57cc27576ba3d3bfa39b3e87e4989ed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950458"
---
# <a name="troubleshooting-file-server-resource-manager"></a>Solución de problemas del Administrador de recursos del servidor de archivos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En esta sección se tratan problemas comunes que pueden surgir al usar el Administrador de recursos del servidor de archivos.

> [!Note]
> Una buena opción para solucionar problemas sería buscar los registros de eventos que genera el Administrador de recursos del servidor de archivos. Todas las entradas del registro de eventos para el servidor de archivos Administrador de recursos pueden encontrarse en el registro de eventos de**aplicación** en el origen **SRMSVC**

## <a name="i-am-not-receiving-e-mail-notifications"></a>No está recibiendo notificaciones de correo electrónico.

-   **Causa**: las opciones de correo electrónico no se han configurado o no se han configurado correctamente.

-   **Solución**: en la pestaña **notificaciones de correo electrónico** , en el cuadro de diálogo **Opciones de administrador de recursos de servidor de archivos** , compruebe que el servidor SMTP y los destinatarios de correo electrónico predeterminados se hayan especificado y sean válidos. Envíe un mensaje de correo electrónico de prueba para confirmar que la información es correcta y que el servidor SMTP que se utiliza para enviar las notificaciones está funcionando correctamente. Para obtener más información, consulte [configurar notificaciones por correo electrónico](configure-email-notifications.md).


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>Solo recibo una notificación por correo electrónico, aunque el evento que desencadenó esa notificación se produjera varias veces en una fila.

-   **Causa**: cuando un usuario intenta guardar varias veces un archivo que está bloqueado o guarda un archivo que supera un umbral de cuota, y hay una notificación de correo electrónico configurada para ese filtrado de archivos o evento de cuota, de forma predeterminada solo se envía un mensaje de correo electrónico al Administrador durante un período de 60 minutos. Con ello se evita la saturación de mensajes repetidos en la cuenta de correo electrónico del administrador.

-   **Solución**: en la pestaña **límites de notificación** , en el cuadro de diálogo Opciones de administrador de recursos de servidor de **archivos** , puede establecer un límite de tiempo para cada uno de los tipos de notificación: correo electrónico, registro de eventos, comando e informe. Cada límite especifica un período de tiempo que debe transcurrir antes de que se genere otra notificación del mismo tipo para el mismo caso. Para obtener más información, consulte [Configure Notification limits](configure-notification-limits.md).


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>Mis informes de almacenamiento siguen produciendo errores y poca o ninguna información está disponible en el registro de eventos con respecto al origen del error.

-   **Causa**: es posible que el volumen donde se guardan los informes esté dañado.
-   **Solución**: ejecute **CHKDSK** en el volumen e intente generar de nuevo los informes.

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>Los informes de auditoría de filtrado de archivos no contienen ninguna información.

-   **Causa**: uno o varios de los siguientes motivos pueden ser la causa:
    -   La base de datos de auditoría no está configurada para registrar la actividad de filtrado de archivos.
    -   La base de datos de auditoría está vacía (es decir, no se ha registrado ninguna actividad de filtrado de archivos).
    -   Los parámetros del informe de auditoría de filtrado de archivos no están seleccionando datos de la base de datos de auditoría.

-   **Solución**: en la pestaña **Auditoría de filtro de archivos** , en el cuadro de diálogo Opciones de administrador de recursos de servidor de **archivos** , compruebe que la casilla registrar la **actividad de filtrado de archivos en la base de datos de auditoría** está activada.
    -   Para obtener más información acerca de cómo registrar la actividad de filtrado de archivos, consulte [configurar la auditoría de filtro de archivos](configure-file-screen-audit.md).
    -   Para configurar los parámetros predeterminados para el informe de Auditoría de filtrado de archivos, consulte [configuración de informes de almacenamiento](configure-storage-reports.md).
    -   Para editar los parámetros de informe para tareas programadas de informes o informes a petición, consulte [Administración de informes de almacenamiento](storage-reports-management.md).

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>Los valores "usados" y "disponibles" de algunas de las cuotas que he creado no se corresponden con la configuración de "límite" real.

-   **Causa**: puede tener una cuota anidada, donde la cuota de una subcarpeta deriva un límite más restrictivo de la cuota de su carpeta principal. Por ejemplo, supongamos que tiene un límite de cuota de 100 megabytes (MB) aplicado a una carpeta principal y una cuota de 200 MB aplicada por separado a cada una de sus subcarpetas. Si la carpeta principal tiene un total de 50 MB de datos almacenados en ella (la suma de los datos almacenados en sus subcarpetas), cada una de las subcarpetas indicará únicamente 50 MB de espacio disponible.

-   **Solución**: en **Administración de cuotas**, haga clic en **cuotas**. En el panel de **resultados** , seleccione la entrada de cuota en la que está solucionando problemas. En el panel **acciones** , haga clic en **ver las cuotas que afectan**a la carpeta y, a continuación, busque las cuotas que se aplican a las carpetas principales. Esto le permite identificar qué cuotas de carpeta principal tienen una configuración de límite de almacenamiento inferior a la cuota seleccionada.

