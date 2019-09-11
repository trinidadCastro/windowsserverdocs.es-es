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
ms.openlocfilehash: 82added5018d83aeb9fe7d8033204a0d19bd047a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868102"
---
# <a name="configure-performance-monitoring"></a>Configurar la supervisión de rendimiento
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS incluye sus propios contadores de rendimiento específicos para ayudarle a supervisar el rendimiento de los servidores de Federación y los equipos de servidor proxy de Federación. Para usar el monitor de rendimiento para supervisar el rendimiento de los servidores de AD FS, es útil crear un nuevo conjunto de recopiladores de datos y agregar los contadores de AD FS a esa vista. En el procedimiento siguiente se describe cómo configurar la supervisión de rendimiento para AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Para configurar la supervisión de rendimiento para AD FS mediante el monitor de rendimiento  
  
1. En la pantalla **Inicio** , escriba **monitor de rendimiento**y, a continuación, presione Entrar.  
  
2. En el árbol de consola, expanda **conjuntos de recopiladores de datos**, haga clic con el\-botón secundario en **definido por el usuario**, seleccione **nuevo**y haga clic en **conjunto de recopiladores de datos**.  
  
   Aparecerá el Asistente para crear nuevo conjunto de recopiladores de datos.  
  
3. En **crear nuevo conjunto de recopiladores de datos**, en **nombre** , escriba un nombre para el \(nuevo conjunto de recopiladores de\)datos, como "AD FS rendimiento", haga clic en **crear \(manualmente opciones avanzadas\)** y, a continuación, haga clic en  **Siguiente**.  
  
4. Para el tipo de datos que se va a incluir, compruebe que la opción **crear registros de datos** está seleccionada y, a continuación, haga clic en las casillas de los siguientes tipos de datos: **Contador de rendimiento**, **datos de seguimiento de eventos**, información de **configuración del sistema**.  
  
5. Para los contadores de rendimiento, expanda **AD FS** en la lista **contadores disponibles** y, a continuación, haga clic en **Agregar**.  
  
   Los contadores de rendimiento de AD FS deben aparecer en la lista de **contadores agregados** .  
  
6. Cuando se le pida que agregue proveedores de seguimiento de eventos, haga clic en **Agregar**, seleccione **AD FS eventos** y **seguimiento de AD FS** en la lista de proveedores.  
  
7. Cuando se le pida que agregue las claves del registro que desea supervisar, haga clic en **siguiente**.  
  
8. Cuando se le pida que especifique la ubicación para guardar los datos de rendimiento, puede aceptar \(la ubicación predeterminada **% SystemDrive%\\\\\\** perflogss admin _< recopilador de\_datos. establezca\_>_ y, a continuación, haga clic en **siguiente**.  
  
9. Cuando se le pregunte si desea crear el conjunto de recopiladores de datos, seleccione **Guardar y cerrar**y, a continuación, haga clic en **Finalizar**.  
  
    El nuevo conjunto de recopiladores de datos aparece en el árbol de consola, en el nodo **definido por el usuario** .  
  
10. Siga los pasos que se indican a continuación para trabajar con los contadores de rendimiento de AD FS:  
  
    -   Para comenzar la supervisión del rendimiento\-mediante AD FS contadores relacionados\-, haga clic con el botón secundario en \(el conjunto de recopiladores de\)datos que agregó, como "rendimiento AD FS" y, a continuación, haga clic en **iniciar**.  
  
    -   Para crear un informe para ver los resultados de la supervisión de\-rendimiento, haga clic con el botón secundario \(en el conjunto de recopiladores\)de datos que agregó, como "AD FS rendimiento" y, a continuación, haga clic en **Informe más reciente**.  
  
    -   Para finalizar una captura de datos de rendimiento para que pueda ver el informe más reciente,\-haga clic con el botón secundario en el \(conjunto de recopiladores de datos\)que agregó, por ejemplo, "rendimiento AD FS" y, a continuación, haga clic en **detener**.  
  
    El informe más reciente se agrega y numera automáticamente \(a partir de\) 000001 en **el\\Informe definido por**<em>\\el usuario\_< conjunto de recopiladores de datos\_></em> nodo en. árbol de consola.  
  
## <a name="ad-fs-performance-counters"></a>Contadores de rendimiento de AD FS  
En la tabla siguiente se enumeran los contadores de rendimiento de AD FS y se describe cómo son útiles para supervisar la actividad relacionada con un servidor de Federación o un servidor proxy de Federación.  
  
|Contador|Descripción|Se puede usar en: 
|-----------|---------------|------------------- 
|Solicitudes de tokens|Supervisa el número de solicitudes de tokens enviadas al servidor de Federación, incluidas las solicitudes de token SSOAuth.|Servidores de federación 
|Solicitudes\/de token s|Supervisa el número de solicitudes de token enviadas al servidor de Federación, incluidas las solicitudes de tokens de SSOAuth por segundo.|Servidores de federación  
|Solicitudes de metadatos de federación|Supervisa el número de solicitudes de metadatos de Federación entrantes enviadas al servidor de Federación.|Servidores de federación  
|Solicitudes\/de metadatos de Federación s|Supervisa el número de solicitudes de metadatos de Federación entrantes por segundo que se envían al servidor de Federación.|Servidores de federación  
|Solicitudes de resolución de artefactos|Supervisa el número de solicitudes de metadatos de Federación entrantes por segundo que se envían al servidor de Federación.|Servidores de federación  
|Solicitudes\/de resolución de artefacto s|Supervisa el número de solicitudes al punto de conexión de resolución de artefactos por segundo que se envían al servidor de Federación.|Servidores de federación  
|Solicitudes de proxy|Supervisa el número de solicitudes entrantes enviadas al servidor proxy de Federación.|Servidores proxy de Federación  
|Solicitudes\/de proxy s|Supervisa el número de solicitudes entrantes por segundo que se envían al servidor proxy de Federación.|Servidores proxy de Federación  
|Solicitudes del proxy MEX|Supervisa el número de\-\) solicitudes entrantes de \(intercambio de metadatos de WS que se envían al servidor proxy de Federación.|Servidores proxy de Federación 
|Solicitudes\/Mex de proxy s|Supervisa el número de solicitudes MEX entrantes por segundo que se envían al servidor proxy de Federación.|Servidores proxy de Federación  
  

