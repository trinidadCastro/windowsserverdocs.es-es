---
title: Supervisar el estado de distribución de la configuración del servidor de acceso remoto
description: Este tema forma parte de la guía de supervisión y contabilidad de acceso remoto en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 19c3d9c306215c92edb2bf322e722a93f9962738
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854038"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>Supervisar el estado de distribución de la configuración del servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de acceso remoto (RAS) en un solo rol de acceso remoto.  
  
La Consola de administración de acceso remoto compara las versiones de configuración de todos los servidores supervisados para verificar que coinciden y que utilizan la versión de configuración más reciente. De este modo se comprueba si la versión de configuración más reciente (que está especificada en los Objetos de directiva de grupo o GPO) se distribuyó en todos los servidores y si se aplicó correctamente en ellos.  
  
### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>Cómo usar el panel de supervisión para comprobar la distribución de la configuración  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haz clic en **PANEL** para ir a **Panel de acceso remoto** en la **Consola de administración de acceso remoto**.  
  
3.  En el panel de supervisión, fíjate en el icono **Estado de configuración** que aparece en el centro de la parte superior. Este icono muestra el estado actual de la distribución de la configuración.  
  
La siguiente tabla muestras los mensajes que genera el icono **Estado de configuración**, su significado y las acciones necesarias (si las hay).  
  
|||||  
|-|-|-|-|  
|Severity|Mensaje|Significado|¿Qué hacer?|  
|Correcto|La configuración se distribuyó correctamente.|La configuración en el GPO se aplicó correctamente al servidor.|No es necesaria ninguna acción.|  
|advertencia|No se recuperó la configuración para el servidor [*nombre del servidor*] desde el controlador de dominio. El GPO no está vinculado.|La configuración en el GPO todavía no ha llegado al servidor. Esto podría deberse a que el GPO no está vinculado al servidor.|Vincula el GPO a un ámbito de administración que esté aplicado al servidor, o en un escenario de GPO de almacenamiento provisional, exporta manualmente la configuración desde el GPO de almacenamiento provisional e impórtala al GPO de producción. Para obtener más información sobre los GPO de almacenamiento provisional, consulte **administrar GPO de acceso remoto con permisos limitados** en [Step-1-plan-The-DirectAccess-Infrastructure](../../directaccess/single-server-advanced/Step-1-Plan-the-DirectAccess-Infrastructure.md). Para conocer los pasos de almacenamiento provisional de los GPO, consulte **configuración de GPO de acceso remoto con permisos limitados** en [el paso 1: configurar la infraestructura de DirectAccess](../../directaccess/single-server-advanced/Step-1-Configuring-DirectAccess-Infrastructure.md).|  
|advertencia|No se recuperó aún la configuración para el servidor [*nombre del servidor*] del controlador de dominio.|La configuración en el GPO todavía no ha llegado al servidor.<p>La propagación de una configuración nueva podría tardar hasta 10 minutos.|Espera un poco más para que las directivas se actualicen en el servidor.|  
|Error|No se puede recuperar la configuración para el servidor [*nombre del servidor*] del controlador de dominio.|La configuración en el GPO no ha llegado al servidor, y han pasado más de 10 minutos desde que se cambió la configuración.|Esto podría pasar en uno de los siguientes casos:<p>-El servidor no tiene conectividad con el dominio para actualizar las directivas. Puede ejecutar "gpupdate/force" en el servidor para forzar una actualización de directiva.<br />-Podría ser necesaria la replicación de GPO para recuperar la configuración actualizada.<br />-No hay ningún controlador de dominio de escritura en el sitio Active Directory del servidor de acceso remoto.<p>Espera a que los GPO se repliquen en todos los controladores de dominio, y después utiliza el cmdlet **Set-DAEntryPointDC** de Windows PowerShell para asociar el punto de entrada con un controlador de dominio de escritura en Active Directory en el servidor de remoto acceso.|  
|advertencia|Se recuperó la configuración para el servidor [*nombre del servidor*] desde el controlador de dominio pero no se aplicó todavía.|La configuración en el GPO ha llegado al servidor pero todavía no se ha aplicado.<p>La configuración puede tardar hasta 15 minutos en aplicarse.|Espera un poco más para que la configuración se aplique por completo en el servidor.|  
|Error|No puede aplicarse la configuración para el servidor [*nombre del servidor*] recuperado desde el controlador de dominio.|La configuración en el GPO ha llegado al servidor pero no se ha aplicado correctamente, y han pasado más de 15 minutos desde que se cambió la configuración.|Esto podría pasar en uno de los siguientes casos:<p>1. la configuración está actualmente en proceso de aplicarse. Se muestra como error porque la recuperación de la configuración desde el GPO podría haber tardado mucho tiempo.<br />    Para comprobar si este es el motivo, utiliza el **Programador de tareas** para ir a Microsoft\Windows\RemoteAccess y comprueba si **RAConfigTask** se está ejecutando actualmente.<br />2. Si **RAConfigTask** no se está ejecutando actualmente, puede que no haya podido aplicar la configuración en el servidor.<br />    Comprueba los errores en el **Visor de eventos** en el canal de operaciones del servidor de acceso remoto, que se localiza en \Applications and Services Logs\Microsoft\Windows\RemoteAccess-RemoteAccessServer.<br />    Comprueba los errores en **ESTADO DE LAS OPERACIONES** en la Consola de administración de acceso remoto. Para obtener más información, consulta [Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md).|  
|Error|Se recuperó la configuración para todos los servidores de multisitio del controlador de dominio. La configuración no coincide en todos los servidores.|Hay una incoherencia entre las versiones de configuración de los GPO de servidor en la implementación multisitio.<p>Lo ideal es que todos los GPO de servidor de todos los puntos de entrada tengan la misma configuración global, pero por alguna razón no están sincronizados.|Esto puede ocurrir cuando un cambio en la configuración generó un error y no se revertió correctamente.<p>Deberías restaurar los GPO desde un estado de copia de seguridad en el que todos los GPO de servidor estén sincronizados. Para obtener información sobre un script que puede usar, consulte [copia de seguridad y restauración de la configuración de acceso remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).|  
  


