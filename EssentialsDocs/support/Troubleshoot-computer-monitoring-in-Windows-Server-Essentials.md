---
title: "Solucionar problemas de supervisión del equipo en Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 72fe309e0e7ce6d7227cce8b7f2c5dbf018eb4a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Solucionar problemas de supervisión del equipo en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tema proporciona la solución de problemas que surgen mientras supervisa el estado de los equipos en el Visor de alertas y a través de notificaciones de correo electrónico en Windows Server Essentials.  
  
> [!NOTE]
>  Para obtener información sobre solución de problemas más reciente de la Comunidad de Windows Server Essentials, te sugerimos que visite el [foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). El foro de Windows Server Essentials es un lugar excelente para buscar ayuda, o hacer una pregunta.  
  
##  <a name="BKMK_TS"></a>Solución de problemas de notificaciones de correo electrónico de alertas  
 Esta sección muestran varios problemas que podrían surgir al usar notificaciones de correo electrónico de alertas.  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>No se puede enviar el mensaje de prueba para la alerta  
 **Problema** se produce un error mensaje que dice, no se puede enviar el mensaje de prueba de alerta.  
  
 **Causa** este error puede producirse debido a cualquiera de los siguientes problemas en la configuración de notificaciones de alerta:  
  
-   Un nombre de servidor SMTP incorrecto o número de puerto.  
  
-   Se especificó incorrectamente que el servidor requiere una conexión de capa de Sockets único (SSL).  
  
-   El servidor SMTP requiere autenticación, y se especificaron credenciales incorrectas.  
  
 **Soluciones** corregir los errores en la configuración de notificación de correo electrónico.  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>Para identificar problemas en la configuración de notificación de correo electrónico  
  
-   Pida a tu proveedor de servicios de Internet (ISP) para el nombre del servidor SMTP correcto, el número de puerto y el uso SSL.  
  
-   Revisar los archivos de registro para notificaciones de correo electrónico de la alerta en el servidor, en esta ubicación:  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  Para ver la carpeta ProgramData, debe oculta elementos mostrados. Si don t ves la carpeta ProgramData, en la cinta s **vista** pestaña la **mostrar u ocultar** grupo, selecciona el **elementos ocultos** cuadro de texto.  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>Para actualizar la configuración de notificación de correo electrónico de alertas  
  
1.  En el panel, haz clic en cualquier icono de alerta en la esquina superior derecha para abrir el Visor de alertas.  
  
2.  En la parte inferior del Visor de alertas, haz clic en **configurar correo electrónico notificación de alertas**.  
  
3.  En la **configurar correo electrónico notificación de alertas** cuadro de diálogo, haz clic en **habilitar**.  
  
4.  En la **SMTP configuración** cuadro de diálogo, actualizar la configuración de SMTP y, a continuación, haz clic en **Aceptar**.  
  
5.  Para probar la configuración actualizada, haz clic en **aplicar y enviar correo electrónico**.  
  
6.  Después de comprobar que la prueba de correo electrónico fue correcta, haz clic en **Aceptar** para guardar la configuración actualizada.  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>Notificación de correo electrónico de prueba no enumera las alertas  
 **Problema** las notificaciones de correo electrónico de prueba de alertas no muestra las alertas incluso si hay alertas enumerados en el Visor de alertas.  
  
 **Solución** no todas las alertas que se notifican en el Visor de alertas generan una notificación por correo electrónico. Solo las alertas que están configuradas para enviará una notificación de correo electrónico en sus archivos de definición de estado se envían como mensajes de correo electrónico a los destinatarios del correo electrónico especificada.  
  
 Al hacer clic en **aplicar y enviar correo electrónico**, normalmente recibirá una notificación de correo electrónico de ejemplo con ningún alertas de estado que aparece. Sin embargo, si se identifica una alerta de salud que está configurada para enviar notificaciones por correo electrónico durante este proceso de prueba, alerta de que se incluye en el correo electrónico de prueba.  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Se muestran alertas activas para una aplicación desinstalada  
 **Problema** alertas activas para una aplicación se muestran a pesar de que se han desinstalado la aplicación y su archivo de definición de estado.  
  
 **Solución** debe eliminar manualmente las alertas de la aplicación desinstalada activas. Para eliminar una alerta, haz lo siguiente.  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Para eliminar una alerta desde el servidor mediante el panel  
  
1.  Desde el servidor, abra el panel.  
  
2.  En el panel de navegación, haz clic en cualquier icono de alerta mostrado (crítico, advertencia o informativos). Esto inicia el Visor de alertas.  
  
3.  En el Visor de alertas, haz clic en la alerta que quieras eliminar y, a continuación, haz clic en **eliminar esta alerta**.
