---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurar la supervisión de rendimiento
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5426ea929037e59d2105fb2b3b06d4ebfdb7a577
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192288"
---
# <a name="configure-performance-monitoring"></a>Configurar la supervisión de rendimiento
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS incluye sus propios contadores de rendimiento dedicado para ayudarle a supervisar el rendimiento de los servidores de federación y equipos de servidores proxy de federación. Para usar el Monitor de rendimiento para supervisar el rendimiento de los servidores de AD FS, es útil crear un nuevo conjunto de recopiladores de datos y agregar los contadores de AD FS a esa vista. El siguiente procedimiento describe cómo configurar la supervisión del rendimiento de AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Para configurar la supervisión del rendimiento de AD FS mediante el Monitor de rendimiento  
  
1.  En el **iniciar** , escriba **Monitor de rendimiento**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, expanda **conjuntos de recopiladores de datos**, ¿no\-haga clic en **definido por el usuario**, apunte a **New**y, a continuación, haga clic en **conjunto de recopiladores de datos** .  
  
    Aparece el Asistente Crear nuevo datos recopilador establecido.  
  
3.  En **crear nuevo conjunto de recopiladores datos**, para **nombre** escriba un nombre para el nuevo conjunto de recopiladores de datos \(como "Rendimiento de AD FS"\), haga clic en **crear manualmente \( Advanced\)** y, a continuación, haga clic en **siguiente**.  
  
4.  Para el tipo de datos que se incluirán, compruebe que **crear registros de datos** está seleccionada y, a continuación, haga clic en las casillas de verificación para los siguientes tipos de datos: **Contador de rendimiento**, **datos de seguimiento de eventos**, **información de configuración del sistema**.  
  
5.  Para los contadores de rendimiento, expanda **AD FS** en el **contadores disponibles** lista y, a continuación, haga clic en **agregar**.  
  
    Los contadores de rendimiento de AD FS deben aparecer en el **agregado contadores** lista.  
  
6.  Cuando se le pida para agregar proveedores de seguimiento de eventos, haga clic en **agregar**, seleccione **AD FS Eventing** y **traza de AD FS** desde la lista de proveedores.  
  
7.  Cuando se le solicite para agregar las claves del registro para supervisar, haga clic en **siguiente**.  
  
8.  Cuando se le pida que especifique la ubicación para guardar los datos de rendimiento, puede aceptar la ubicación predeterminada \( * *% systemdrive %\\PerfLogs\\Admin\\*** < datos\_recopilador\_establecer >* y, a continuación, haga clic en **siguiente**.  
  
9. Cuando se le pida para crear el conjunto de recopiladores de datos, seleccione **guardar y cerrar**y, a continuación, haga clic en **finalizar**.  
  
    El nuevo conjunto de recopiladores de datos aparece en el árbol de consola bajo el **definido por el usuario** nodo.  
  
10. Para trabajar con los contadores de rendimiento de AD FS, siga estos pasos:  
  
    -   Para empezar la supervisión de rendimiento mediante AD FS\-relacionados con los contadores, derecha\-haga clic en el conjunto de recopiladores de datos que agregó \(como "Rendimiento de AD FS"\)y, a continuación, haga clic en **iniciar**.  
  
    -   Para crear un informe para ver lo resultados de la supervisión de rendimiento, a la derecha\-haga clic en el conjunto de recopiladores de datos que agregó \(como "Rendimiento de AD FS"\)y, a continuación, haga clic en **último informe**.  
  
    -   Para finalizar una captura de datos de rendimiento para que pueda ver el informe más reciente, a la derecha\-haga clic en el conjunto de recopiladores de datos que agregó \(como "Rendimiento de AD FS"\)y, a continuación, haga clic en **detener**.  
  
    El informe más reciente se agrega y se numeran automáticamente \(comenzando en 000001\) en el **informe\\definido por usuario ***\\< datos\_recopilador\_establecer >* nodo en el árbol de consola.  
  
## <a name="ad-fs-performance-counters"></a>Contadores de rendimiento de AD FS  
En la tabla siguiente se enumera los contadores de rendimiento de AD FS y se describe cómo son útiles para supervisar la actividad que se relaciona con un servidor de federación o el servidor proxy de federación.  
  
|Contador|Descripción|Se puede usar en: 
|-----------|---------------|------------------- 
|Solicitudes de tokens|Supervisa el número de solicitudes de token enviado al servidor de federación, incluidas las solicitudes de token SSOAuth.|Servidores de federación 
|Solicitudes de token\/seg.|Supervisa el número de solicitudes de token enviado al servidor de federación, incluidas las solicitudes de token de SSOAuth por segundo.|Servidores de federación  
|Solicitudes de metadatos de federación|Supervisa el número de solicitudes entrantes de los metadatos de federación enviado al servidor de federación.|Servidores de federación  
|Las solicitudes de metadatos de federación\/seg.|Supervisa el número de federación metadatos solicitudes entrantes por segundo que se envían al servidor de federación.|Servidores de federación  
|Solicitudes de resolución de artefactos|Supervisa el número de federación metadatos solicitudes entrantes por segundo que se envían al servidor de federación.|Servidores de federación  
|Solicitudes de resolución de artefacto\/seg.|Supervisa el número de solicitudes al punto de conexión de resolución de artefacto por segundo que se envían al servidor de federación.|Servidores de federación  
|Solicitudes de proxy|Supervisa el número de solicitudes entrantes que se envían al servidor proxy de federación.|Servidores proxy de federación  
|Las solicitudes de proxy\/seg.|Supervisa el número de solicitudes entrantes por segundo que se envían al servidor proxy de federación.|Servidores proxy de federación  
|Solicitudes de intercambio de metadatos de proxy|Supervisa el número de WS entrante\-intercambio de metadatos \(MEX\) las solicitudes que se envían al servidor proxy de federación.|Servidores proxy de federación 
|Las solicitudes de proxy MEX\/seg.|Supervisa el número de solicitudes MEX entrantes por segundo que se envían al servidor proxy de federación.|Servidores proxy de federación  
  

