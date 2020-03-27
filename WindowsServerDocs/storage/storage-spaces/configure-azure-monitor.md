---
title: Comprender y configurar Azure Monitor
description: Información de configuración detallada sobre qué es Azure Monitor y cómo configurar el correo electrónico y las alertas de SMS para el clúster de espacios de almacenamiento directo en Windows Server 2016 y 2019.
keywords: Espacios de almacenamiento directo, Azure monitor, notificaciones, correo electrónico, SMS
ms.assetid: ''
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/10/2020
ms.openlocfilehash: 878bbce9543439cf78501e496c59e06e077c5ddc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308179"
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Usar Azure Monitor para enviar correos electrónicos de Servicio de mantenimiento errores

>Se aplica a: Windows Server 2019, Windows Server 2016

Azure Monitor maximiza la disponibilidad y el rendimiento de las aplicaciones, ya que ofrece una solución completa para recopilar, analizar y actuar en la telemetría de los entornos locales y en la nube. Le ayuda a comprender el rendimiento de las aplicaciones y a identificar proactivamente los problemas que afectan a ellos y los recursos de los que dependen.

Esto es especialmente útil para el clúster hiperconvergido local. Con Azure Monitor integrado, podrá configurar el correo electrónico, el texto (SMS) y otras alertas para hacer ping cuando haya algún problema con el clúster (o cuando desee marcar alguna otra actividad en función de los datos recopilados). A continuación, explicaremos brevemente cómo funciona Azure Monitor, cómo instalar Azure Monitor y cómo configurarlo para que le envíe notificaciones.

Si usa System Center, consulte el [módulo de administración de espacios de almacenamiento directo](https://www.microsoft.com/download/details.aspx?id=100782) que supervisa los clústeres de windows Server 2019 y windows server 2016 espacios de almacenamiento directo.

Este módulo de administración incluye:

* Supervisión del rendimiento y el estado del disco físico
* Supervisión de estado y rendimiento del nodo de almacenamiento
* Supervisión de estado y rendimiento del bloque de almacenamiento
* Estado de tipo de resistencia de volumen y desduplicación

## <a name="understanding-azure-monitor"></a>Descripción de Azure Monitor

Todos los datos recopilados por Azure Monitor caben en uno de los dos tipos fundamentales: métricas y registros.

1. Las [métricas](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) son valores numéricos que describen algún aspecto de un sistema en un momento determinado. Son ligeras y capaces de admitir escenarios casi en tiempo real. Verá los datos recopilados por Azure Monitor derecha en su página de información general en el Azure Portal.

![imagen de la ingesta de métricas en el explorador de métricas](media/configure-azure-monitor/metrics.png)

2. Los [registros](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs) contienen distintos tipos de datos organizados en registros con diferentes conjuntos de propiedades para cada tipo. La telemetría, como eventos y seguimientos, se almacenan como registros, además de los datos de rendimiento, para que se puedan combinar para su análisis. Los datos de registro recopilados por Azure Monitor se pueden analizar con [consultas](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) para recuperar, consolidar y analizar rápidamente los datos recopilados. Puede crear y probar consultas mediante [log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals) en el Azure portal y, a continuación, analizar directamente los datos mediante estas herramientas o guardar consultas para usarlas con [visualizaciones](https://docs.microsoft.com/azure/azure-monitor/visualizations) o [reglas de alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

![imagen de la ingesta de registros en log Analytics](media/configure-azure-monitor/logs.png)

Veremos más detalles a continuación sobre cómo configurar estas alertas.

## <a name="onboarding-your-cluster-using-windows-admin-center"></a>Incorporación del clúster mediante el centro de administración de Windows

Con el centro de administración de Windows, puede incorporar el clúster a Azure Monitor.

![GIF del clúster de incorporación a Azure Monitor "](media/configure-azure-monitor/onboarding.gif)

Durante este flujo de incorporación, los pasos siguientes se están produciendo en el capó. Se explica cómo configurarlos en detalle en caso de que quiera configurar manualmente el clúster. 

### <a name="configuring-health-service"></a>Configuración de Servicio de mantenimiento

Lo primero que debe hacer es configurar el clúster. Como puede saber, el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) mejora la supervisión cotidiana y la experiencia operativa de los clústeres que ejecutan espacios de almacenamiento directo. 

Como vimos anteriormente, Azure Monitor recopila registros de cada nodo en el que se está ejecutando en el clúster. Por lo tanto, tenemos que configurar la Servicio de mantenimiento para escribir en un canal de eventos, que es:

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Para configurar el Servicio de mantenimiento, ejecute:

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Al ejecutar el cmdlet anterior para establecer la configuración de mantenimiento, se producen los eventos que deseamos que comiencen a escribirse en el canal de eventos *Microsoft-Windows-Health/Operational* .

### <a name="configuring-log-analytics"></a>Configuración de Log Analytics

Ahora que ha configurado el registro correcto en el clúster, el siguiente paso es configurar correctamente log Analytics.

Para obtener información general, [Azure log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows) puede recopilar datos directamente de los equipos de Windows físicos o virtuales de su centro de datos o de otro entorno en la nube en un solo repositorio para el análisis y la correlación detallados.

Para comprender la configuración admitida, consulte [sistemas operativos Windows compatibles](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) y [configuración del firewall de red](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.

#### <a name="login-in-to-azure-portal"></a>Inicio de sesión en Azure portal

Inicie sesión en el Azure Portal en [https://portal.azure.com](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

#### <a name="create-a-workspace"></a>Crear un área de trabajo

Para obtener más información sobre los pasos que se indican a continuación, consulte la [documentación de Azure monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer).

1. En el Azure Portal, haga clic en **todos los servicios**. En la lista de recursos, escriba **log Analytics**. Cuando empiece a escribir, la lista se filtrará en función de la entrada. Seleccione **log Analytics**.<br><br> 

   ![Portal de Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Haga clic en **crear**y seleccione Opciones para los elementos siguientes:

   * Proporcione un nombre para el nuevo **área de trabajo log Analytics**, como *DefaultLAWorkspace*. 
   * Seleccione una **suscripción** a la que vincular seleccionando en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
   * En **grupo de recursos**, seleccione un grupo de recursos existente que contenga una o varias máquinas virtuales de Azure. <br><br>

      ![Crear Log Analytics hoja de recursos](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Después de proporcionar la información necesaria en el panel del **área de trabajo log Analytics** , haga clic en **Aceptar**.  

Mientras se comprueba la información y se crea el área de trabajo, puede realizar un seguimiento de su progreso en **notificaciones** en el menú. 

#### <a name="obtain-workspace-id-and-key"></a>Obtener ID. de área de trabajo y clave
Antes de instalar el Microsoft Monitoring Agent para Windows, necesita el identificador de área de trabajo y la clave para el área de trabajo de Log Analytics.  Esta información es necesaria para que el Asistente para la instalación configure correctamente el agente y asegúrese de que puede comunicarse correctamente con Log Analytics.  

1. En el Azure Portal, haga clic en **todos los servicios** que se encuentran en la esquina superior izquierda. En la lista de recursos, escriba **log Analytics**. Cuando empiece a escribir, la lista se filtrará en función de la entrada. Seleccione **log Analytics**.
2. En la lista de áreas de trabajo de Log Analytics, seleccione *DefaultLAWorkspace* creado anteriormente.
3. Seleccione **Configuración avanzada**.<br><br> ![Log Analytics configuración avanzada](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Seleccione **orígenes conectados**y, a continuación, seleccione **servidores de Windows**.   
5. Valor a la derecha de ID. de **área de trabajo** y **clave principal**. Guarde la copia temporalmente y péguela en su editor favorito para que el tiempo sea.   

### <a name="installing-the-agent-on-windows"></a>Instalación del agente en Windows
En los pasos siguientes se instalan y configuran los Microsoft Monitoring Agent. **Asegúrese de instalar este agente en cada servidor del clúster e indique que desea que el agente se ejecute en el inicio de Windows.**

1. En la página **servidores de Windows** , seleccione la versión de descarga del agente de **Windows** adecuada que se va a descargar según la arquitectura del procesador del sistema operativo Windows.
2. Ejecute el programa de instalación para instalar el agente en el equipo.
2. En la página de bienvenida **, haga clic en** Siguiente **.
3. En la página **términos de licencia** , lea la licencia y haga clic en **acepto**.
4. En la página **carpeta de destino** , cambie o mantenga la carpeta de instalación predeterminada y, a continuación, haga clic en **siguiente**.
5. En la página **Opciones de instalación del agente** , elija conectar el agente a Azure log Analytics y, a continuación, haga clic en **siguiente**.   
6. En la página **log Analytics de Azure** , realice lo siguiente:
   1. Pegue el **identificador del área de trabajo** y la **clave del área de trabajo (clave principal)** que copió anteriormente.    
    a. Si el equipo necesita comunicarse a través de un servidor proxy con el servicio Log Analytics, haga clic en **Opciones avanzadas** y proporcione la dirección URL y el número de puerto del servidor proxy.  Si el servidor proxy requiere autenticación, escriba el nombre de usuario y la contraseña para autenticarse en el servidor proxy y, a continuación, haga clic en **siguiente**.  
7. Haga clic en **siguiente** cuando haya terminado de proporcionar las opciones de configuración necesarias.<br><br> ![pegar el identificador del área de trabajo y la clave principal](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. En la página **listo para instalar** , revise las opciones seleccionadas y, a continuación, haga clic en **instalar**.
9. En la página la **configuración se completó correctamente** , haga clic en **Finalizar**.

Cuando haya finalizado, el **Microsoft Monitoring Agent** aparecerá en el **Panel de control**. Puede revisar la configuración y comprobar que el agente está conectado a Log Analytics. Cuando se conecta, en la pestaña **Azure log Analytics** , el agente muestra un mensaje que indica: **la Microsoft Monitoring Agent se ha conectado correctamente al servicio Microsoft log Analytics.** 

![Estado de conexión de MMA a Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Para comprender la configuración admitida, consulte [sistemas operativos Windows compatibles](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) y [configuración del firewall de red](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="setting-up-alerts-using-windows-admin-center"></a>Configuración de alertas mediante el centro de administración de Windows

En el centro de administración de Windows, puede configurar las alertas predeterminadas que se aplicarán a todos los servidores del área de trabajo Log Analytics. 

![GIF de configuración de alertas "](media/configure-azure-monitor/setup1.gif)

Estas son las alertas y sus condiciones predeterminadas en las que puede participar:

| Nombre de la alerta                | Condición predeterminada                                  |
|---------------------------|----------------------------------------------------|
| Uso de CPU           | Más de 85% durante 10 minutos                            |
| Uso de la capacidad de disco | Más de 85% durante 10 minutos                            |
| Uso de memoria        | Memoria disponible inferior a 100 MB durante 10 minutos   |
| Latido                 | Menos de 2 pulsaciones durante 5 minutos                   |
| Error crítico del sistema     | Cualquier alerta crítica en el registro de eventos del sistema del clúster |
| Alerta de servicio de mantenimiento      | Cualquier error del servicio de mantenimiento en el clúster            |

Una vez configuradas las alertas en el centro de administración de Windows, puede ver las alertas en el área de trabajo de log Analytics en Azure.

![GIF de configuración de alertas "](media/configure-azure-monitor/setup2.gif)

Durante este flujo de incorporación, los pasos siguientes se están produciendo en el capó. Se explica cómo configurarlos en detalle en caso de que quiera configurar manualmente el clúster. 

### <a name="collecting-event-and-performance-data"></a>Recopilación de datos de eventos y rendimiento

Log Analytics puede recopilar eventos del registro de eventos de Windows y los contadores de rendimiento que especifique para los informes y análisis a largo plazo, y actuar cuando se detecte una condición determinada.  Siga estos pasos para configurar la recopilación de eventos del registro de eventos de Windows y varios contadores de rendimiento comunes con los que empezar.  

1. En el Azure Portal, haga clic en **más servicios** en la esquina inferior izquierda. En la lista de recursos, escriba **log Analytics**. Cuando empiece a escribir, la lista se filtrará en función de la entrada. Seleccione **log Analytics**.
2. Seleccione **Configuración avanzada**.<br><br> ![Log Analytics configuración avanzada](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Seleccione **datos**y, a continuación, seleccione **registros de eventos de Windows**.  
4. Aquí, agregue el canal de eventos Servicio de mantenimiento escribiendo en el nombre que aparece a continuación y haga clic en el signo más **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. En la tabla, compruebe los **errores** de gravedad y **ADVERTENCIA**.   
6. Haga clic en **Guardar** en la parte superior de la página para guardar la configuración.
7. Seleccione **contadores de rendimiento de Windows** para habilitar la recopilación de contadores de rendimiento en un equipo Windows. 
8. La primera vez que se configuran los contadores de rendimiento de Windows para un nuevo área de trabajo Log Analytics, se ofrece la opción de crear rápidamente varios contadores comunes. Aparecen con una casilla junto a cada una de ellas.<br> ![contadores de rendimiento de Windows predeterminados seleccionados](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Haga clic en **Agregar los contadores de rendimiento seleccionados**.  Se agregan y preparan con un intervalo de ejemplo de colección de diez segundos.  
9. Haga clic en **Guardar** en la parte superior de la página para guardar la configuración.

## <a name="creating-alerts-based-on-log-data"></a>Crear alertas basadas en datos de registro

Si lo ha hecho hasta ahora, el clúster debe enviar los registros y los contadores de rendimiento a Log Analytics. El siguiente paso consiste en crear reglas de alerta que ejecutan automáticamente búsquedas de registros a intervalos regulares. Si los resultados de la búsqueda de registros coinciden con determinados criterios, se desencadena una alerta que le envía una notificación de texto o correo electrónico. Vamos a explorar esto a continuación.

### <a name="create-a-query"></a>Crear una consulta

Para empezar, abra el portal de búsqueda de registros.   

1. En el Azure Portal, haga clic en **todos los servicios**. En la lista de recursos, escriba **monitor**. Cuando empiece a escribir, la lista se filtrará en función de la entrada. Seleccione **monitor**.
2. En el menú de navegación monitor, seleccione **log Analytics** y, a continuación, seleccione un área de trabajo.

La forma más rápida de recuperar algunos datos con los que trabajar es una consulta simple que devuelve todos los registros de la tabla. Escriba las siguientes consultas en el cuadro de búsqueda y haga clic en el botón Buscar.  

```
Event
```

Los datos se devuelven en la vista de lista predeterminada y puede ver el número total de registros que se devolvieron.

![Consulta simple](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

En el lado izquierdo de la pantalla está el panel filtro, que le permite agregar filtrado a la consulta sin modificarla directamente.  Se muestran varias propiedades de registro para ese tipo de registro y puede seleccionar uno o varios valores de propiedad para restringir los resultados de la búsqueda.

Active la casilla junto a **error** en **EVENTLEVELNAME** o escriba lo siguiente para limitar los resultados a los eventos de error.

```
Event | where (EventLevelName == "Error")
```

![Filtro](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Una vez realizadas las consultas de apropiados para los eventos que le interesan, guárdelas en el paso siguiente.

### <a name="create-alerts"></a>Crear alertas
Ahora, vamos a examinar un ejemplo para crear una alerta.

1. En el Azure Portal, haga clic en **todos los servicios**. En la lista de recursos, escriba **log Analytics**. Cuando empiece a escribir, la lista se filtrará en función de la entrada. Seleccione **log Analytics**.
2. En el panel izquierdo, seleccione **alertas** y, después, haga clic en **nueva regla de alerta** en la parte superior de la página para crear una nueva alerta.<br><br> ![crear nueva regla de alerta](media/configure-azure-monitor/alert-rule-02.png)<br>
3. En el primer paso, en la sección **crear alerta** , va a seleccionar su log Analytics área de trabajo como el recurso, ya que se trata de una señal de alerta basada en el registro.  Filtre los resultados seleccionando la **suscripción** específica de la lista desplegable si tiene más de una, que contiene log Analytics área de trabajo creada anteriormente.  Filtre el **tipo de recurso** seleccionando **log Analytics** en la lista desplegable.  Por último, seleccione el **recurso** **DefaultLAWorkspace** y, a continuación, haga clic en **listo**.<br><br> ![tarea crear paso 1 de la alerta](media/configure-azure-monitor/alert-rule-03.png)<br>
4. En la sección **criterios de alerta**, haga clic en **Agregar criterios** para seleccionar la consulta guardada y, a continuación, especifique la lógica que sigue la regla de alerta.
5. Configure la alerta con la siguiente información:  
   a. En la lista desplegable **basado en** , seleccione **medición de métricas**.  Una medición de métrica creará una alerta para cada objeto de la consulta con un valor que supere el umbral especificado.  
   b. En **condición**, seleccione **mayor que** y especifique un thershold.  
   c. A continuación, defina Cuándo desencadenar la alerta. Por ejemplo, puede seleccionar **infracciones consecutivas** y, en la lista desplegable, seleccionar **mayor que** un valor de 3.  
   d. En la sección evaluación basada en, modifique el valor de **período** en **30** minutos y la **frecuencia** en 5. La regla se ejecutará cada cinco minutos y devolverá los registros que se crearon en los últimos treinta minutos a partir de la hora actual.  Establecer el período de tiempo en una ventana más amplia para el potencial de latencia de los datos y garantiza que la consulta devuelve datos para evitar un falso negativo en el que la alerta nunca se activa.  
6. Haga clic en **listo** para completar la regla de alerta.<br><br> ![configurar la señal de alerta](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. Ahora, en el segundo paso, proporcione un nombre de la alerta en el campo Nombre de la **regla de alerta** , como **alerta en todos los eventos de error**.  Especifique una **Descripción** que detalle los detalles de la alerta y seleccione **crítico (gravedad 0)** para el valor de **gravedad** de las opciones proporcionadas.
8. Para activar inmediatamente la regla de alerta al crearla, acepte el valor predeterminado para **Habilitar regla tras la creación**.
9. En el tercer y último paso, se especifica un **grupo de acciones**, lo que garantiza que las mismas acciones se realizan cada vez que se desencadena una alerta y se puede usar para cada regla que defina. Configure un nuevo grupo de acciones con la siguiente información:  
   a. Seleccione **nuevo grupo de acciones** y aparecerá el panel **Agregar grupo de acciones** .  
   b. En **nombre del grupo de acciones**, especifique un nombre como **operaciones de ti-notificar** y un **nombre corto** como **ITOPS-n**.  
   c. Compruebe que los valores predeterminados de **suscripción** y **grupo de recursos** son correctos. Si no es así, seleccione el correcto en la lista desplegable.   
   d. En la sección acciones, especifique un nombre para la acción, como **Enviar correo electrónico** y, en **tipo de acción** , seleccione **correo electrónico/SMS/inserciones/voz** en la lista desplegable. Se abrirá el panel de propiedades **correo electrónico/SMS/enviar/voz** a la derecha para proporcionar información adicional.  
   e. En el panel **correo electrónico/SMS/enviar/voz** , seleccione y configure sus preferencias. Por ejemplo, habilite el **correo electrónico** y proporcione una dirección SMTP de correo electrónico válida para enviar el mensaje a.  
   f. Haga clic en **Aceptar** para guardar los cambios.<br><br> 

    ![Crear nuevo grupo de acciones](media/configure-azure-monitor/action-group-properties-01.png)

10. Haga clic en **Aceptar** para completar el grupo de acciones. 
11. Haga clic en **crear regla de alerta** para completar la regla de alerta. Empieza a ejecutarse inmediatamente.<br><br> ![completar la creación de la nueva regla de alerta](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="example-alert"></a>Alerta de ejemplo

Como referencia, este es el aspecto de una alerta de ejemplo en Azure.

![GIF de la alerta en Azure](media/configure-azure-monitor/alert.gif)

A continuación se muestra un ejemplo del correo electrónico que se enviará mediante Azure Monitor:

![Ejemplo de correo electrónico de alerta](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- Para obtener información más detallada, lea la [documentación de Azure monitor](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata).
- Lea este tema para obtener información general sobre cómo [conectarse a otros servicios híbridos de Azure](../../manage/windows-admin-center/azure/index.md).
