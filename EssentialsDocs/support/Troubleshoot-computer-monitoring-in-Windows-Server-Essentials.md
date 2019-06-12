---
title: Solucionar problemas de supervisión del equipo en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1adf8ae2dd8763d0bc5a514609bb2470de6acde4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436070"
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Solucionar problemas de supervisión del equipo en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tema proporciona una solución de problemas que surgen al supervisar el estado de los equipos en el Visor de alertas y mediante notificaciones por correo electrónico en Windows Server Essentials.  
  
> [!NOTE]
>  Para la información de solución de problemas más reciente de la Comunidad de Windows Server Essentials, le recomendamos que visite el [foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). El foro de Windows Server Essentials es un buen lugar para buscar ayuda o realizar una pregunta.  
  
##  <a name="BKMK_TS"></a> Solución de problemas de notificaciones de correo electrónico para alertas  
 Esta sección presenta diversos problemas que pueden aparecer al usar notificaciones de alerta por correo electrónico.  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>No se puede enviar el mensaje de prueba de la alerta  
 **Problema** produce un error mensaje que dice, no se puede enviar el correo electrónico de prueba para la alerta.  
  
 **Causa** Este error puede producirse por cualquiera de los siguientes problemas en la configuración de las notificaciones de alerta:  
  
- Un nombre de servidor SMTP o un número de puerto incorrecto.  
  
- Se especificó incorrectamente que el servidor SMTP requiere una conexión de Capa de sockets seguros (SSL).  
  
- El servidor SMTP requiere autenticación, y se especificaron credenciales incorrectas.  
  
  **Soluciones** Corregir los errores en la configuración de las notificaciones por correo electrónico.  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>Para identificar problemas en la configuración de notificación de correo electrónico  
  
-   Pida a su proveedor de servicios de Internet (ISP) el nombre del servidor SMTP correcto, el número de puerto y el uso de SSL.  
  
-   Revise los archivos de registro para las notificaciones de correo electrónico para alertas en el servidor en la siguiente ubicación:  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  Para ver la carpeta ProgramData, debe ocultar los elementos mostrados. Si no ve la carpeta ProgramData, en la cinta de opciones **vista** ficha la **mostrar u ocultar** grupo, seleccione el **elementos ocultos** cuadro de texto.  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>Para actualizar la configuración de notificaciones de correo electrónico para alertas  
  
1.  En el panel, haga clic en cualquier icono de alerta de la esquina superior derecha para abrir el Visor de alertas.  
  
2.  En la parte inferior del Visor de alertas, haga clic en **Configurar notificación de alertas por correo electrónico**.  
  
3.  En el cuadro de diálogo **Configurar notificación de alertas por correo electrónico**, haga clic en **Habilitar**.  
  
4.  En el cuadro de diálogo **Configuración SMTP**, actualice la configuración SMTP y, a continuación, haga clic en **Aceptar**.  
  
5.  Para probar la configuración actualizada, haga clic en **Aplicar y enviar correo electrónico**.  
  
6.  Después de comprobar que el correo electrónico de prueba fue correcto, haga clic en **Aceptar** para guardar la configuración actualizada.  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>La notificación de prueba por correo electrónico no muestra las alertas  
 **Problema** Las notificaciones de alertas de prueba no muestran ninguna alerta, incluso si hay alertas en el Visor de alertas.  
  
 **Solución** No todas las alertas que se muestran en el Visor de alertas generan una notificación de correo electrónico. Solo las alertas que estén configuradas para enviarse como una notificación por correo electrónico dentro de sus archivos de definición de mantenimiento se envían como mensajes de correo electrónico a los destinatarios especificados.  
  
 Al hacer clic en **Aplicar y enviar correo electrónico**, normalmente recibirá una notificación de prueba por correo electrónico sin ninguna alerta de estado. Sin embargo, si se identifica una alerta de estado configurada para enviar notificaciones por correo electrónico durante este proceso de prueba, esa alerta se incluye en el mensaje de prueba.  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Se muestran alertas activas de una aplicación desinstalada  
 **Problema** Se muestran las alertas activas de una aplicación aunque se hayan desinstalado la aplicación y su archivo de definición de mantenimiento.  
  
 **Solución** Debe eliminar manualmente las alertas activas de la aplicación desinstalada. Para eliminar una alerta, haga lo siguiente.  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Para eliminar una alerta desde el servidor utilizando el panel  
  
1.  En el servidor, abra el panel.  
  
2.  En el panel de navegación, haga clic en cualquier icono de alerta mostrado (crítico, de advertencia o informativo). Se abrirá el Visor de alertas.  
  
3.  En el Visor de alertas, haga clic en la alerta que desee eliminar y, después, haga clic en **Eliminar esta alerta**.
