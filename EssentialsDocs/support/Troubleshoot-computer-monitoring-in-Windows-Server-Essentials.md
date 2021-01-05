---
title: Solucionar problemas de supervisión del equipo en Windows Server Essentials
description: Obtenga información acerca de cómo solucionar problemas detectados al supervisar el estado de mantenimiento de los equipos en el visor de alertas y mediante notificaciones por correo electrónico en Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: d3483482d3540bf632e2d2bb8fbd8828fa764b28
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810162"
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Solucionar problemas de supervisión del equipo en Windows Server Essentials

> Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este tema se describe la solución de problemas detectados al supervisar el estado de mantenimiento de los equipos en el visor de alertas y mediante notificaciones por correo electrónico en Windows Server Essentials.

> [!NOTE]
> Para obtener la información más reciente sobre la solución de problemas de la comunidad de Windows Server Essentials, se recomienda visitar el [Foro de Windows Server Essentials](/answers/topics/windows-server-essentials.html). El foro de Windows Server Essentials es un buen lugar para buscar ayuda o realizar una pregunta.

## <a name="troubleshooting-email-notifications-for-alerts"></a>Solución de problemas de notificación de alertas

 Esta sección presenta diversos problemas que pueden aparecer al usar notificaciones de alerta por correo electrónico.

### <a name="cannot-send-the-test-email-for-the-alert"></a>No se puede enviar el mensaje de prueba de la alerta

 **Problema** de Aparece un mensaje de error que indica que no se puede enviar el correo electrónico de prueba para la alerta.

 **Causa** Este error puede producirse por cualquiera de los siguientes problemas en la configuración de las notificaciones de alerta:

- Un nombre de servidor SMTP o un número de puerto incorrecto.

- Se especificó incorrectamente que el servidor SMTP requiere una conexión de Capa de sockets seguros (SSL).

- El servidor SMTP requiere autenticación, y se especificaron credenciales incorrectas.

  **Soluciones** Corregir los errores en la configuración de las notificaciones por correo electrónico.

#### <a name="to-identify-issues-in-your-email-notification-settings"></a>Para identificar problemas en la configuración de notificación de correo electrónico

- Pida a su proveedor de servicios de Internet (ISP) el nombre del servidor SMTP correcto, el número de puerto y el uso de SSL.

- Revise los archivos de registro para las notificaciones de correo electrónico para alertas en el servidor en la siguiente ubicación:

    ` %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log`

    > [!TIP]
    > Para ver la carpeta ProgramData, debe ocultar los elementos mostrados. Si no ve la carpeta ProgramData, en la pestaña **vista** de la cinta de opciones, en el grupo **Mostrar u ocultar** , seleccione el cuadro de texto **elementos ocultos** .

#### <a name="to-update-your-email-notification-setup-for-alerts"></a>Para actualizar la configuración de notificaciones de correo electrónico para alertas

1. En el panel, haga clic en cualquier icono de alerta de la esquina superior derecha para abrir el Visor de alertas.

2. En la parte inferior del Visor de alertas, haga clic en **Configurar notificación de alertas por correo electrónico**.

3. En el cuadro de diálogo **Configurar notificación de alertas por correo electrónico**, haga clic en **Habilitar**.

4. En el cuadro de diálogo **Configuración SMTP**, actualice la configuración SMTP y, a continuación, haga clic en **Aceptar**.

5. Para probar la configuración actualizada, haga clic en **Aplicar y enviar correo electrónico**.

6. Después de comprobar que el correo electrónico de prueba fue correcto, haga clic en **Aceptar** para guardar la configuración actualizada.

### <a name="test-email-notification-does-not-list-any-alerts"></a>La notificación de prueba por correo electrónico no muestra las alertas

**Problema** Las notificaciones de alertas de prueba no muestran ninguna alerta, incluso si hay alertas en el Visor de alertas.

**Solución** No todas las alertas que se muestran en el Visor de alertas generan una notificación de correo electrónico. Solo las alertas que estén configuradas para enviarse como una notificación por correo electrónico dentro de sus archivos de definición de mantenimiento se envían como mensajes de correo electrónico a los destinatarios especificados.

Al hacer clic en **Aplicar y enviar correo electrónico**, normalmente recibirá una notificación de prueba por correo electrónico sin ninguna alerta de estado. Sin embargo, si se identifica una alerta de estado configurada para enviar notificaciones por correo electrónico durante este proceso de prueba, esa alerta se incluye en el mensaje de prueba.

### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Se muestran alertas activas de una aplicación desinstalada

**Problema** Se muestran las alertas activas de una aplicación aunque se hayan desinstalado la aplicación y su archivo de definición de mantenimiento.

**Solución** Debe eliminar manualmente las alertas activas de la aplicación desinstalada. Para eliminar una alerta, haga lo siguiente.

#### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Para eliminar una alerta desde el servidor utilizando el panel

1. En el servidor, abra el panel.

2. En el panel de navegación, haga clic en cualquier icono de alerta mostrado (crítico, de advertencia o informativo). Se abrirá el Visor de alertas.

3. En el Visor de alertas, haga clic en la alerta que desee eliminar y, después, haga clic en **Eliminar esta alerta**.