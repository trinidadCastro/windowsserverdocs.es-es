---
title: Entender y configurar Azure Monitor
description: El programa de instalación información detallada sobre qué es Azure Monitor y cómo configurar alertas de correo electrónico y sms para el clúster de espacios de almacenamiento directo en Windows Server 2016 y de 2019.
keywords: Espacios de almacenamiento directo, azure monitor, notificaciones, correo electrónico, sms
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 908e4a7a75606905caebfa4b79168b3976982e6d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447596"
---
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Use Azure Monitor para enviar mensajes de correo electrónico para errores de servicio de mantenimiento

>Se aplica a: Windows Server 2019, Windows Server 2016

Azure Monitor maximiza la disponibilidad y rendimiento de las aplicaciones al ofrecer una solución completa para recopilar, analizar y actuar en la telemetría de la nube y entornos locales. Le ayudará a comprender cómo funcionan las aplicaciones e identifica proactivamente los problemas que afectan a ellos y los recursos que dependen.

Esto es especialmente útil para el clúster hiperconvergido de en el entorno local. Con Azure Monitor integrado, podrá configurar el correo electrónico, texto (SMS) y otras alertas hacer ping cuando algo va mal con el clúster (o cuando desee marcar alguna otra actividad según los datos recopilados). A continuación, se explicará brevemente cómo funciona Azure Monitor, cómo instalar el Monitor de Azure y cómo configurarlo para enviar notificaciones.


## <a name="understanding-azure-monitor"></a>Descripción del Monitor de Azure

Todos los datos recopilados por Azure Monitor se adapta a uno de dos tipos fundamentales: métricas y registros.

1. [Las métricas](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/data-collection#metrics) son valores numéricos que se describen algunos aspectos de un sistema en un momento determinado en el tiempo. Son ligeros y capaz de admitir escenarios en tiempo real casi. Podrá ver los datos recopilados por la derecha de Azure Monitor en su página de información general en Azure portal.

![imagen de la ingesta en las métricas en el Explorador de métricas](media/configure-azure-monitor/metrics.png)

2. [Registros](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/data-collection#logs) contienen distintos tipos de datos que se organizan en registros con diferentes conjuntos de propiedades para cada tipo. Datos de telemetría, como eventos y seguimientos se almacenan como registros además a los datos de rendimiento para que lo puede todos combinarse para el análisis. Se pueden analizar los datos de registro recopilados por Azure Monitor con [consultas](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/log-query-overview) rápidamente recuperar, consolidar y analizar los datos recopilados. Puede crear y probar consultas usando [Log Analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/portals) en el portal de Azure y, a continuación, ya sea directamente analizar los datos mediante estas herramientas o guardar las consultas para su uso con [visualizaciones](https://docs.microsoft.com/en-us/azure/azure-monitor/visualizations) o [alerta las reglas de](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview).

![imagen de los registros de ingesta en log analytics](media/configure-azure-monitor/logs.png)

Tendremos más detalles a continuación sobre cómo configurar estas alertas.

## <a name="configuring-health-service"></a>Configuración de servicio de mantenimiento

Lo primero que necesita hacer es configurar el clúster. Como ya sabrá, el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) mejora la experiencia operativa para los clústeres que ejecutan espacios de almacenamiento directo y la supervisión diaria. 

Como hemos visto anteriormente, Azure Monitor recopila registros de cada nodo que se está ejecutando en el clúster. Por lo tanto, se debe configurar el servicio de mantenimiento para escribir en un canal de eventos, que es:

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Para configurar el servicio de mantenimiento, ejecute:

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Al ejecutar el cmdlet anterior para establecer la configuración de estado, provocar los eventos que desea empezar a la que se escriben en el *Microsoft-Windows-estado/Operational* canal de eventos.

## <a name="configuring-log-analytics"></a>Configuración de Log Analytics

Ahora que ha configurado el registro adecuado en el clúster, el siguiente paso es configurar correctamente de log analytics.

Para ofrecer una introducción, [Azure Log Analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/agent-windows) puede recopilar datos directamente desde los equipos de Windows físicos o virtuales en su centro de datos o en otro entorno de nube en un único repositorio para la correlación y análisis detallados.

Para comprender la configuración admitida, revise [admite los sistemas operativos de Windows](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) y [configuración de firewall de red](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Si no tiene una suscripción de Azure, cree un [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.

### <a name="login-in-to-azure-portal"></a>Inicio de sesión en el portal de Azure

Inicie sesión en el portal de Azure en [ https://portal.azure.com ](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

### <a name="create-a-workspace"></a>Crear un área de trabajo

Para obtener más detalles sobre los pasos indicados a continuación, consulte el [documentación de Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-windows-computer).

1. En el portal de Azure, haga clic en **todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando empiece a escribir, la lista se filtra según la entrada. Seleccione **de Log Analytics**.<br><br> 

   ![Portal de Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Haga clic en **crear**y, a continuación, seleccione opciones para los siguientes elementos:

   * Proporcione un nombre para el nuevo **el área de trabajo de Log Analytics**, tales como *DefaultLAWorkspace*. 
   * Seleccione un **suscripción** para vincular a seleccionándola en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
   * Para **grupo de recursos**, seleccione un grupo de recursos existente que contiene uno o más máquinas virtuales de Azure. <br><br>

      ![Crear hoja de recursos de Log Analytics](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Después de proporcionar la información necesaria en el **el área de trabajo de Log Analytics** panel, haga clic en **Aceptar**.  

Mientras se comprueba la información y se crea el área de trabajo, puede seguir su progreso en **notificaciones** en el menú. 

### <a name="obtain-workspace-id-and-key"></a>Obtener Id. de área de trabajo y clave
Antes de instalar el Microsoft Monitoring Agent para Windows, necesita el identificador de área de trabajo y la clave del área de trabajo de Log Analytics.  Esta información es necesaria por el Asistente para configuración para configurar al agente y garantizar que puede comunicarse correctamente con Log Analytics correctamente.  

1. En el portal de Azure, haga clic en **todos los servicios** se encuentra en la esquina superior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando empiece a escribir, la lista se filtra según la entrada. Seleccione **de Log Analytics**.
2. En la lista de áreas de trabajo de Log Analytics, seleccione *DefaultLAWorkspace* creado anteriormente.
3. Seleccione **configuración avanzada**.<br><br> ![Configuración avanzada de log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Seleccione **orígenes conectados**y, a continuación, seleccione **servidores Windows**.   
5. El valor a la derecha del **Id. de área de trabajo** y **Primary Key**. Guarde ambos temporalmente: copie y pegue ambos en su editor favorito por el momento.   

## <a name="installing-the-agent-on-windows"></a>Instalación del agente en Windows
Los pasos siguientes instalarán y configuración a Microsoft Monitoring Agent. **Asegúrese de instalar a este agente en cada servidor del clúster y para indicar que desea que el agente para ejecutarse en el inicio de Windows.**

1. En el **servidores Windows** , seleccione la adecuada **Download Windows Agent** versión para descargar según la arquitectura de procesador del sistema operativo Windows.
2. Ejecute el programa de instalación para instalar al agente en el equipo.
2. En el **bienvenida** página, haga clic en **siguiente**.
3. En el **términos de licencia** página, lea la licencia y, a continuación, haga clic en **acepto**.
4. En el **carpeta de destino** página, cambie o mantenga la carpeta de instalación predeterminada y, a continuación, haga clic en **siguiente**.
5. En el **opciones de configuración de agente** página, elija conectar el agente a Azure Log Analytics y, a continuación, haga clic en **siguiente**.   
6. En el **Azure Log Analytics** , realice lo siguiente:
   1. Pegar la **Id. de área de trabajo** y **clave del área de trabajo (clave principal)** que copió anteriormente.    
    a. Si el equipo necesita comunicarse a través de un servidor proxy para el servicio Log Analytics, haga clic en **avanzadas** y proporcione la dirección URL y el número de puerto del servidor proxy.  Si el servidor proxy requiere autenticación, escriba el nombre de usuario y la contraseña para autenticarse con el servidor proxy y, a continuación, haga clic en **siguiente**.  
7. Haga clic en **siguiente** una vez que haya terminado de proporcionar los valores de configuración necesarios.<br><br> ![Pegue el Id. de área de trabajo y la clave principal](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. En el **preparado para instalar** , revise las opciones seleccionadas y, a continuación, haga clic en **instalar**.
9. En el **completó correctamente la configuración** página, haga clic en **finalizar**.

Cuando haya finalizado, el **Microsoft Monitoring Agent** aparece en **Panel de Control**. Puede revisar la configuración y compruebe que el agente está conectado a Log Analytics. Cuando se conecta, en el **Azure Log Analytics** ficha, el agente muestra un mensaje que indica: **Microsoft Monitoring Agent se conectó correctamente al servicio Log Analytics de Microsoft.** 

![Estado de la conexión de MMA en Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Para comprender la configuración admitida, revise [admite los sistemas operativos de Windows](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) y [configuración de firewall de red](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="collecting-event-and-performance-data"></a>Recopilar datos de rendimiento y eventos

Log Analytics puede recopilar eventos desde el registro de eventos de Windows y los contadores de rendimiento que especifique para el análisis del término informes y a largo y actuar cuando se detecte una condición determinada.  Siga estos pasos para configurar la recopilación de eventos desde el registro de eventos de Windows y varios contadores de rendimiento comunes para empezar.  

1. En el portal de Azure, haga clic en **más servicios** se encuentra en la esquina inferior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando empiece a escribir, la lista se filtra según la entrada. Seleccione **de Log Analytics**.
2. Seleccione **configuración avanzada**.<br><br> ![Configuración avanzada de log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Seleccione **datos**y, a continuación, seleccione **registros de eventos de Windows**.  
4. En este caso, agregue el canal de eventos del servicio de mantenimiento para ello, escriba el nombre siguiente y haga clic en el signo más **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. En la tabla, compruebe las gravedades **Error** y **advertencia**.   
6. Haga clic en **guardar** en la parte superior de la página para guardar la configuración.
7. Seleccione **los contadores de rendimiento de Windows** para habilitar la recopilación de contadores de rendimiento en un equipo Windows. 
8. Al configurar los contadores de rendimiento de Windows para una nueva área de trabajo de Log Analytics en primer lugar, tiene la opción para crear rápidamente varios contadores comunes. Aparecen con una casilla junto a cada uno.<br> ![Contadores de rendimiento Windows predeterminados seleccionados](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Haga clic en **agregar los contadores de rendimiento seleccionados**.  Se agregan y el valor preestablecidos con un intervalo de muestra de recopilación de diez segundos.  
9. Haga clic en **guardar** en la parte superior de la página para guardar la configuración.

## <a name="creating-alerts-based-on-log-data"></a>Crear alertas basadas en datos de registro

Si se logró esto ahora, el clúster debe estar enviando los registros y contadores de rendimiento a Log Analytics. El siguiente paso es crear reglas de alerta que se ejecutan automáticamente búsquedas de registros a intervalos regulares. Si los resultados de la búsqueda de registros coinciden con determinados criterios, se activará una alerta que le envíe una notificación de correo electrónico o texto. Vamos a examinar esto a continuación.

### <a name="create-a-query"></a>Crear una consulta

Comience abriendo el portal de búsqueda de registros.   

1. En el portal de Azure, haga clic en **todos los servicios**. En la lista de recursos, escriba **Monitor**. Cuando empiece a escribir, la lista se filtra según la entrada. Seleccione **Monitor**.
2. En el menú de navegación del Monitor, seleccione **Log Analytics** y, a continuación, seleccione un área de trabajo.

Recuperar algunos datos para trabajar con la forma más rápida es una consulta simple que devuelve todos los registros en la tabla. Escriba las siguientes consultas en el cuadro de búsqueda y haga clic en el botón de búsqueda.  

```
Event
```

Se devuelven los datos en la vista de lista predeterminada y puede ver el número total de registros se han devuelto.

![Consulta simple](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

En el lado izquierdo de la pantalla es el panel de filtro que permite agregar filtros a la consulta sin modificarlo directamente.  Se muestran varias propiedades de registro para ese tipo de registro, y puede seleccionar uno o más valores de propiedad para restringir los resultados de búsqueda.

Active la casilla de verificación junto a **Error** en **EVENTLEVELNAME** o escriba lo siguiente para limitar los resultados a los eventos de error.

```
Event | where (EventLevelName == "Error")
```

![Filtro](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Una vez que las consultas apropiados para los eventos que le interesan, guárdelos para el paso siguiente.

### <a name="create-alerts"></a>Creación de alertas
Ahora, veamos un ejemplo para crear una alerta.

1. En el portal de Azure, haga clic en **todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando empiece a escribir, la lista se filtra según la entrada. Seleccione **de Log Analytics**.
2. En el panel izquierdo, seleccione **alertas** y, a continuación, haga clic en **nueva regla de alerta** desde la parte superior de la página para crear una nueva alerta.<br><br> ![Crear nueva regla de alerta](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Para el primer paso, en el **crear alerta** sección, va a seleccionar el área de trabajo de Log Analytics como el recurso, ya que se trata de una señal de alerta en función de registro.  Filtrar los resultados seleccionando la específica **suscripción** en la lista desplegable si tiene más de uno, que contiene el área de trabajo de Log Analytics creado anteriormente.  Filtro del **tipo de recurso** seleccionando **Log Analytics** en la lista desplegable.  Por último, seleccione el **recursos** **DefaultLAWorkspace** y, a continuación, haga clic en **realiza**.<br><br> ![Crear tarea de alerta paso 1](media/configure-azure-monitor/alert-rule-03.png)<br>
4. En la sección **criterios de alerta**, haga clic en **agregar criterios** para seleccionar la consulta guardada y, a continuación, especificar la lógica que sigue la regla de alerta.
5. Configure la alerta con la siguiente información:  
   a. Desde el **según** lista desplegable, seleccione **unidades métricas**.  Una métrica medida creará una alerta para cada objeto en la consulta con un valor que supera el umbral especificado.  
   b. Para el **condición**, seleccione **mayor** y especifique un thershold.  
   c. A continuación, defina cuándo se debe desencadenar la alerta. Por ejemplo, podría seleccionar **infracciones consecutivas** y en la lista desplegable, seleccione **mayor** un valor de 3.  
   d. Bajo basándose en la sección de evaluación, modifique la **período** valor **30** minutos y **frecuencia** a 5. La regla se ejecute cada cinco minutos y devolver los registros que se crearon en los últimos treinta minutos desde la hora actual.  Establecer el período de tiempo en una ventana más amplia de cuentas para el potencial de latencia de datos y garantiza que la consulta devuelve datos para evitar un falso negativo donde la alerta nunca se activa.  
6. Haga clic en **realiza** para completar la regla de alerta.<br><br> ![Configurar la señal de alerta](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. Mover ahora en el segundo paso, proporcione un nombre de la alerta en el **nombre de regla de alerta** campo, como **alerta en todos los eventos de Error**.  Especifique un **descripción** que detalla los detalles de la alerta y seleccione **crítico (gravedad 0)** para el **gravedad** valor entre las opciones proporcionadas.
8. Para activar inmediatamente la regla de alertas, acepte el valor predeterminado para **Habilitar regla tras la creación**.
9. Para el tercer y último paso, especificará un **grupo de acciones**, lo que garantiza que las mismas acciones se realizan cada vez una alerta se desencadena y se puede usar para cada regla que defina. Configurar un nuevo grupo de acciones con la siguiente información:  
   a. Seleccione **nuevo grupo de acciones** y **Agregar grupo de acciones** aparecerá el panel.  
   b. Para **nombre del grupo de acción**, especifique un nombre como **notificar las operaciones de TI -** y un **nombre corto** como **itops n**.  
   c. Compruebe los valores predeterminados de **suscripción** y **grupo de recursos** son correctos. Si no es así, seleccione la correcta en la lista desplegable.   
   d. En la sección Acciones, especifique un nombre para la acción, como **enviar correo electrónico** y en **tipo de acción** seleccione **correo electrónico o SMS/Push, voz** en la lista desplegable. El **correo electrónico o SMS/Push, voz** se abrirá el panel de propiedades a la derecha con el fin de proporcionar información adicional.  
   e. En el **correo electrónico o SMS/Push, voz** panel, seleccionar y configurar sus preferencias. Por ejemplo, habilitar **correo electrónico** y proporcione un dirección SMTP para entregar el mensaje de correo electrónico válida.  
   f. Haga clic en **Aceptar** para guardar los cambios.<br><br> 

    ![Crear nuevo grupo de acciones](media/configure-azure-monitor/action-group-properties-01.png)

10. Haga clic en **Aceptar** para completar el grupo de acciones. 
11. Haga clic en **crear regla de alertas** para completar la regla de alerta. Empieza a ejecutarse inmediatamente.<br><br> ![Completar la creación de nueva regla de alerta](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="test-alerts"></a>Alertas de prueba

Como referencia, este es el aspecto de una alerta de ejemplo:

![Ejemplo de correo electrónico de alerta](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Vea también

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
- Para obtener más información, lea el [documentación de Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/tutorial-viewdata).
