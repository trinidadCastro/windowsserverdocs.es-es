---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: "Configurar la supervisión del rendimiento"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6a7602cddcaee274d42213cd9365f6d1722dab79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-performance-monitoring"></a>Configurar la supervisión del rendimiento

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS incluye sus propio contadores de rendimiento dedicado para supervisar el rendimiento de los servidores de federación y equipos de proxy del servidor de federación. Para usar el Monitor de rendimiento para supervisar el rendimiento de los servidores de AD FS, resulta útil crear un nuevo conjunto de recopiladores de datos y agregar los contadores de AD FS a la vista. El siguiente procedimiento describe cómo configurar la supervisión del rendimiento de AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Para configurar la supervisión del rendimiento de AD FS mediante el Monitor de rendimiento  
  
1.  En la **inicio** , escriba **Performance Monitor**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, expande **conjuntos de recopilador de datos**, right\ clic **definido por el usuario**, elija **nuevo**y, a continuación, haz clic en **conjunto de recopiladores de datos**.  
  
    Aparece el nuevo selector establecer Asistente para crear datos.  
  
3.  En **crear nuevo conjunto de recopiladores de datos**, para **nombre** escribe un nombre para el conjunto de recopiladores de datos nueva \ (como "El rendimiento de AD FS" \), haz clic en **crear manualmente \(Advanced\)**y, a continuación, haz clic en **siguiente**.  
  
4.  Para el tipo de datos que quieres incluir, comprueba que **crear datos registros** está seleccionado y, a continuación, haz clic en las casillas de los siguientes tipos de datos: **contador de rendimiento**, **los datos de seguimiento de eventos**, **información de configuración del sistema**.  
  
5.  Contadores de rendimiento, expanda **AD FS** en la **contadores disponibles** lista y, a continuación, haz clic en **agregar**.  
  
    Los contadores de rendimiento de AD FS deben aparecer en la **agregado contadores** lista.  
  
6.  Cuando se te pida para agregar proveedores de seguimiento de eventos, haz clic en **agregar**, selecciona **AD FS eventos** y **AD FS seguimiento** en la lista de proveedores.  
  
7.  Cuando se le pida para agregar claves de registro para supervisar, haz clic en **siguiente**.  
  
8.  Cuando se te pida para especificar la ubicación para guardar los datos de rendimiento, puedes aceptar la ubicación predeterminada \ (**%systemdrive%\\PerfLogs\\Admin\\***< data\_collector\_set >*y, a continuación, haz clic en **siguiente**.  
  
9. Cuando se te pida para crear el conjunto de recopiladores de datos, selecciona **guarda y cierra**y, a continuación, haz clic en **finalizar**.  
  
    El conjunto de recopiladores de datos nuevo aparece en el árbol de consola en la **definido por el usuario** nodo.  
  
10. Usar los siguientes pasos para trabajar con los contadores de rendimiento de AD FS:  
  
    -   Para comenzar la supervisión del rendimiento con los contadores de AD FS\, right\ el recopilador de datos y haga clic en establecer que agregaste \ (como "El rendimiento de AD FS" \) y, a continuación, haz clic en **inicio**.  
  
    -   Para crear un informe para ver lo resultados de la supervisión de rendimiento, right\ el recopilador de datos y haga clic en establecer que agregaste \ (como "El rendimiento de AD FS" \) y, a continuación, haz clic en **último informe**.  
  
    -   Para finalizar una captura de datos de rendimiento para que pueda ver el informe más reciente, right\ el recopilador de datos y haga clic en establecer que agregaste \ (como "El rendimiento de AD FS" \) y, a continuación, haz clic en **detener**.  
  
    El informe más reciente se agregan y se numeran automáticamente \(starting at 000001\) bajo la **Report\\User definido***\\ < data\_collector\_set >* nodo del árbol de consola.  
  
## <a name="ad-fs-performance-counters"></a>Contadores de rendimiento de AD FS  
La siguiente tabla enumera los contadores de rendimiento de AD FS y describe cómo son útiles para supervisar la actividad relacionada con un servidor de federación o proxy del servidor de federación.  
  
|Contador|Descripción|Pueden usarse en: 
|-----------|---------------|------------------- 
|Solicitudes de token|Supervisa el número de solicitudes de token enviados al servidor de federación incluidos SSOAuth solicitudes de token.|Servidores de federación 
|Token Requests\ por segundo|Supervisa el número de solicitudes de token enviados al servidor de federación como solicitudes de token SSOAuth por segundo.|Servidores de federación  
|Solicitudes de metadatos de federación|Supervisa el número de solicitudes entrantes de metadatos de federación enviada al servidor de federación.|Servidores de federación  
|Federación Requests\ metadatos por segundo|Supervisa el número de federación metadatos solicitudes entrantes por segundo que se envían al servidor de federación.|Servidores de federación  
|Solicitudes de resolución de artefacto|Supervisa el número de federación metadatos solicitudes entrantes por segundo que se envían al servidor de federación.|Servidores de federación  
|Resolución de artefacto Requests\ por segundo|Supervisa el número de solicitudes hacia el extremo de resolución artefacto por segundo que se envían al servidor de federación.|Servidores de federación  
|Solicitudes de proxy|Supervisa el número de solicitudes entrantes enviada al federación servidor proxy.|Federación servidor proxy  
|Proxy Requests\ por segundo|Supervisa el número de solicitudes entrantes por segundo que se envían al servidor proxy de servidor de federación.|Federación servidor proxy  
|Solicitudes de intercambio de metadatos de proxy|Supervisa el número de solicitudes entrantes de intercambio de metadatos de WS\ \(MEX\) que se envían al servidor proxy de servidor de federación.|Federación servidor proxy 
|Intercambio de metadatos de proxy Requests\ por segundo|Supervisa el número de solicitudes de intercambio de metadatos entrantes por segundo que se envían al servidor proxy de servidor de federación.|Federación servidor proxy  
  

