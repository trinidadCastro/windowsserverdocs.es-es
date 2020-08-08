---
title: Comprender y configurar Azure Monitor
description: Información de configuración detallada sobre qué es Azure Monitor y cómo configurar el correo electrónico y las alertas de SMS para el clúster de espacios de almacenamiento directo en Windows Server 2016 y 2019.
ms.author: adagashe
ms.topic: article
author: adagashe
ms.date: 01/10/2020
ms.openlocfilehash: 40ee23fa8c1fa88c54e5c8ee1e2c3ebd3453bfff
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961141"
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Uso de Azure Monitor para enviar correos electrónicos sobre errores del Servicio de mantenimiento

>Se aplica a: Windows Server 2019, Windows Server 2016

Azure Monitor maximiza la disponibilidad y el rendimiento de las aplicaciones con una completa solución que permite recopilar, analizar y administrar datos telemétricos tanto en la nube como en entornos locales. Esta solución le ayudará a entender cómo funcionan las aplicaciones y le permitirá identificar de manera proactiva los problemas que les afectan y los recursos de los que dependen.

Esto es especialmente útil para el clúster hiperconvergido local. Con Azure Monitor integrado, podrá configurar el correo electrónico, el texto (SMS) y otras alertas para hacer ping cuando haya algún problema con el clúster (o cuando desee marcar alguna otra actividad en función de los datos recopilados). A continuación, explicaremos brevemente cómo funciona Azure Monitor, cómo instalar Azure Monitor y cómo configurarlo para que le envíe notificaciones.

Si usa System Center, consulte el [módulo de administración de espacios de almacenamiento directo](https://www.microsoft.com/download/details.aspx?id=100782) que supervisa los clústeres de windows Server 2019 y windows server 2016 espacios de almacenamiento directo.

Este módulo de administración incluye:

* Supervisión del rendimiento y el estado del disco físico
* Supervisión de estado y rendimiento del nodo de almacenamiento
* Supervisión de estado y rendimiento del bloque de almacenamiento
* Estado de tipo de resistencia de volumen y desduplicación

## <a name="understanding-azure-monitor"></a>Descripción de Azure Monitor

Todos los datos recopilados por Azure Monitor pueden clasificarse en uno de los dos tipos fundamentales: métricas y registros.

1. Las [métricas](/azure/azure-monitor/platform/data-collection#metrics) son valores numéricos que describen algún aspecto de un sistema en un momento dado. Las métricas son ligeras y capaces de admitir escenarios de tiempo casi real. Verá los datos recopilados por Azure Monitor derecha en su página de información general en el Azure Portal.

![Imagen de la ingesta de métricas en el explorador de métricas](media/configure-azure-monitor/metrics.png)

2. Los [registros](/azure/azure-monitor/platform/data-collection#logs) contienen distintos tipos de datos organizados en grupos de registros, donde cada tipo tiene diferentes conjuntos de propiedades. Los datos de telemetría, como los eventos y los seguimientos, se almacenan como registros junto con los datos de rendimiento para poder analizarlos de forma combinada. Los datos de registro recopilados por Azure Monitor se pueden analizar con [consultas](/azure/azure-monitor/log-query/log-query-overview) que recuperan, consolidan y analizan rápidamente los datos recopilados. Puede crear y probar consultas mediante [Log Analytics](/azure/azure-monitor/log-query/portals) en Azure Portal y después analizar los datos directamente mediante estas herramientas o guardar las consultas para usarlas con las [visualizaciones](/azure/azure-monitor/visualizations) o las [reglas de alertas](/azure/azure-monitor/platform/alerts-overview).

![Imagen de la ingesta de registros en Log Analytics](media/configure-azure-monitor/logs.png)

Veremos más detalles a continuación sobre cómo configurar estas alertas.

## <a name="onboarding-your-cluster-using-windows-admin-center"></a>Incorporación del clúster mediante el centro de administración de Windows

Con el centro de administración de Windows, puede incorporar el clúster a Azure Monitor.

![GIF del clúster de incorporación a Azure Monitor "](media/configure-azure-monitor/onboarding.gif)

Durante este flujo de incorporación, los pasos siguientes se están produciendo en el capó. Se explica cómo configurarlos en detalle en caso de que quiera configurar manualmente el clúster.

### <a name="configuring-health-service"></a>Configuración de Servicio de mantenimiento

Lo primero que debe hacer es configurar el clúster. Como sabe, el [Servicio de mantenimiento](../../failover-clustering/health-service-overview.md) mejora la supervisión diaria y la experiencia operativa de los clústeres que ejecutan Espacios de almacenamiento directo.

Como hemos visto anteriormente, Azure Monitor recopila registros de cada nodo que se está ejecutando en el clúster. Por lo tanto, tenemos que configurar el Servicio de mantenimiento para que escriba en un canal de eventos, que es:

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

Para obtener información general, [Azure log Analytics](/azure/azure-monitor/platform/agent-windows) puede recopilar datos directamente de los equipos de Windows físicos o virtuales de su centro de datos o de otro entorno en la nube en un solo repositorio para el análisis y la correlación detallados.

Para comprender la configuración compatible, revise los [sistemas operativos Windows admitidos](/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) y la [configuración del firewall de red](/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

#### <a name="login-in-to-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en Azure Portal en [https://portal.azure.com](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

#### <a name="create-a-workspace"></a>Crear un área de trabajo

Para obtener más detalles sobre los pasos que se indican a continuación, consulte la [documentación de Azure Monitor](/azure/azure-monitor/learn/quick-collect-windows-computer).

1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.<br><br>

   ![Azure portal](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Haga clic en **Crear** y, a continuación, seleccione opciones para los elementos siguientes:

   * Escriba el nombre del nuevo **área de trabajo de Log Analytics**, como por ejemplo *DefaultLAWorkspace*.
   * Seleccione una **suscripción** a la que vincularlo en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
   * Para **Grupo de recursos**, seleccione un grupo de recursos existente que contenga una o más máquinas virtuales de Azure. <br><br>

      ![Creación de una hoja de recursos de Log Analytics](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>

3. Después de proporcionar la información necesaria en el panel **Área de trabajo de Log Analytics**, haga clic en **Aceptar**.

Mientras se comprueba la información y se crea el espacio de trabajo, puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

#### <a name="obtain-workspace-id-and-key"></a>Obtención de la clave y el identificador de área de trabajo
Antes de instalar Microsoft Monitoring Agent para Windows, necesita la clave y el identificador de área de trabajo para el área de trabajo de Log Analytics.  El asistente de configuración necesita esta información para configurar el agente de la forma adecuada y asegurarse de que puede comunicarse correctamente con Log Analytics.

1. En Azure Portal, haga clic en **Todos los servicios**, en la esquina superior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. En la lista de áreas de trabajo de Log Analytics, seleccione *DefaultLAWorkspace* (creada antes).
3. Seleccione **Configuración avanzada**.<br><br> ![Configuración avanzada de Log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>
4. Seleccione **Orígenes conectados** y **Servidores Windows**.
5. Encontrará los valores a la derecha de **Id. del área de trabajo** y **Clave principal**. Guarde ambos temporalmente y cópielos y péguelos en el editor que prefiera por ahora.

### <a name="installing-the-agent-on-windows"></a>Instalación del agente en Windows
En los siguientes pasos se instala y configura Microsoft Monitoring Agent. **Asegúrese de instalar este agente en cada servidor del clúster e indique que desea que el agente se ejecute en el inicio de Windows.**

1. En la página **Servidores Windows**, seleccione la versión de **Descargar el agente de Windows** que descargar según la arquitectura del procesador del sistema operativo Windows.
2. Ejecute el programa de instalación para instalar al agente en el equipo.
2. En la página **principal**, haga clic en **Siguiente**.
3. En la página **Términos de licencia**, lea la licencia y haga clic en **Acepto**.
4. En la página e **Carpeta de destino**, cambie o mantenga la carpeta de instalación predeterminada y haga clic en **Siguiente**.
5. En la página **Opciones de instalación del agente**, elija la opción para conectar el agente a Azure Log Analytics y luego haga clic en **Siguiente**.
6. En la página **Azure Log Analytics**, realice lo siguiente:
   1. Pegue el **Id. del área de trabajo** y la **clave del área de trabajo (clave principal)** que copió anteriormente.
    a. Si el equipo necesita comunicarse a través de un servidor proxy con el servicio de Log Analytics, haga clic en **Avanzado** y proporcione la dirección URL y el número de puerto del servidor proxy.  Si el servidor proxy requiere autenticación, escriba el nombre de usuario y la contraseña para autenticar con el servidor proxy y, luego, haga clic en **Siguiente**.
7. Haga clic en **Siguiente** cuando haya terminado de proporcionar las opciones de configuración necesarias.<br><br> ![pegar identificador del área de trabajo y clave principal](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. En la página **Preparado para instalar**, revise las opciones seleccionadas y haga clic en **Instalar**.
9. En la página **La configuración finalizó correctamente**, haga clic en **Finalizar**.

Una vez completado el proceso, el **Agente de administración de Microsoft** aparece en el **Panel de control**. Puede revisar la configuración y comprobar que el agente esté conectado a Log Analytics. Al conectarse, en la pestaña **Azure Log Analytics**, el agente muestra un mensaje que indica: **Microsoft Monitoring Agent se ha conectado correctamente al servicio Microsoft Log Analytics.**

![Estado de la conexión de MMA en Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Para comprender la configuración compatible, revise los [sistemas operativos Windows admitidos](/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) y la [configuración del firewall de red](/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="setting-up-alerts-using-windows-admin-center"></a>Configuración de alertas mediante Windows Admin Center

En el centro de administración de Windows, puede configurar las alertas predeterminadas que se aplicarán a todos los servidores del área de trabajo Log Analytics.

![GIF de configuración de alertas "](media/configure-azure-monitor/setup1.gif)

Estas son las alertas y sus condiciones predeterminadas que puede modificar:

| Nombre de la alerta                | Condición predeterminada                                  |
|---------------------------|----------------------------------------------------|
| Uso de CPU           | Más de 85 % durante 10 minutos                            |
| Uso de la capacidad del disco | Más de 85 % durante 10 minutos                            |
| Uso de memoria        | Memoria disponible inferior a 100 MB durante 10 minutos   |
| Latido                 | Menos de 2 latidos durante 5 minutos                   |
| Error crítico del sistema     | Cualquier alerta crítica en el registro de eventos del sistema del clúster |
| Alerta del Servicio de mantenimiento      | Cualquier error del servicio de mantenimiento del clúster            |

Una vez configuradas las alertas en el centro de administración de Windows, puede ver las alertas en el área de trabajo de log Analytics en Azure.

![GIF de configuración de alertas "](media/configure-azure-monitor/setup2.gif)

Durante este flujo de incorporación, los pasos siguientes se están produciendo en el capó. Se explica cómo configurarlos en detalle en caso de que quiera configurar manualmente el clúster.

### <a name="collecting-event-and-performance-data"></a>Recopilación de datos de eventos y rendimiento

Log Analytics puede recopilar eventos de los registros de eventos de Windows, así como de los contadores de rendimiento que especifique para los informes y análisis a largo plazo, y actuar cuando se detecte una condición determinada.  Siga estos pasos para configurar la colección de eventos desde el Registro de eventos de Windows, así como desde varios contadores de rendimiento comunes, para empezar.

1. En Azure Portal, haga clic en **Más servicios**, en la esquina inferior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. Seleccione **Configuración avanzada**.<br><br> ![Configuración avanzada de Log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>
3. Seleccione **Datos** y, a continuación, **Registros de eventos de Windows**.
4. Aquí, para agregar el canal de eventos del Servicio de mantenimiento, escriba el nombre que aparece a continuación y haga clic en el signo más **+** .
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. En la tabla, compruebe los niveles de gravedad **Error** y **Advertencia**.
6. Haga clic en **Guardar** en la parte superior de la página para guardar la configuración.
7. Seleccione **Windows Performance Counters** (Contadores de rendimiento de Windows) para habilitar la recopilación de contadores de rendimiento en un equipo Windows.
8. La primera vez que se configuran los contadores de rendimiento Windows para un área de trabajo de Log Analytics nueva, se ofrece la opción de crear rápidamente varios contadores comunes. Se muestran todos con una casilla junto a cada uno.<br> ![Contadores de rendimiento predeterminados de Windows seleccionados](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Haga clic en **Agregar los contadores de rendimiento seleccionados**.  Se agregan con el valor preestablecido de un intervalo de ejemplo de recopilación de diez segundos.
9. Haga clic en **Guardar** en la parte superior de la página para guardar la configuración.

## <a name="creating-alerts-based-on-log-data"></a>Crear alertas basadas en datos de registro

Si ha llegado hasta este paso, el clúster debería enviar los registros y contadores de rendimiento a Log Analytics. El siguiente paso es crear reglas de alertas que ejecutan automáticamente búsquedas de registros a intervalos regulares. Si los resultados de la búsqueda de registros coinciden con determinados criterios, se desencadena una alerta que le envía una notificación de correo electrónico o de texto. Vamos a explorar esto a continuación.

### <a name="create-a-query"></a>Creación de una consulta

En primer lugar, abra el portal de búsqueda de registros.

1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Monitor**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Monitor**.
2. En el menú de navegación de Monitor, seleccione **Log Analytics** y, a continuación, seleccione un área de trabajo.

La forma más rápida de recuperar algunos datos con los que trabajar es con una consulta simple que devuelve todos los registros en una tabla. Escriba las consultas siguientes en el cuadro de búsqueda y haga clic en el botón de búsqueda.

```
Event
```

Se devuelven los datos en la vista de lista predeterminada y puede ver el número total de registros que se han devuelto.

![Consulta sencilla](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

En el lado izquierdo de la pantalla está el panel de filtros que le permite agregar filtros a la consulta sin necesidad de modificarla directamente.  Se muestran varias propiedades de registro para ese tipo de registro y puede seleccionar uno o más valores de propiedad para restringir los resultados de búsqueda.

Active la casilla situada junto a **Error** en **EVENTLEVELNAME** o escriba lo siguiente para limitar los resultados a los eventos de error.

```
Event | where (EventLevelName == "Error")
```

![Filter](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Una vez realizadas las consultas de apropiados para los eventos que le interesan, guárdelas en el paso siguiente.

### <a name="create-alerts"></a>Creación de alertas
Ahora, vamos a examinar un ejemplo de creación de una alerta.

1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. En el panel izquierdo, seleccione **Alertas** y, a continuación, haga clic en **Nueva regla de alertas** en la parte superior de la página para crear una nueva alerta.<br><br> ![Crear nueva regla de alertas](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Como primer paso, en la sección **Crear alerta**, seleccione el área de trabajo de Log Analytics como el recurso, ya que se trata de una señal de alerta basada en el registro.  Filtre los resultados seleccionando la **Suscripción** específica en la lista desplegable si tiene más de una, la cual contiene el área de trabajo de Log Analytics creada anteriormente.  Filtre el **Tipo de recurso** seleccionando **Log Analytics** en la lista desplegable.  Por último, seleccione el **recurso** **DefaultLAWorkspace** y haga clic en **Listo**.<br><br> ![Tarea del paso 1 de creación de alertas](media/configure-azure-monitor/alert-rule-03.png)<br>
4. En la sección **Criterios de alerta**, haga clic en **Agregar criterios** para seleccionar la consulta guardada y, a continuación, especifique la lógica que sigue la regla de alertas.
5. Configure la alerta con la siguiente información: a. En la lista desplegable **Basado en**, seleccione **Unidades métricas**.  Una medida de métricas creará una alerta por cada objeto de la consulta con un valor que supere el umbral especificado.
   b. En **condición**, seleccione **mayor que** y especifique un thershold.
   c. A continuación, defina cuándo desencadenar la alerta. Por ejemplo, puede seleccionar **Infracciones consecutivas** y, en la lista desplegable, seleccionar **Mayor que** y escribir un valor de 3.
   d. En la sección Evaluation based on (Evaluación basada en), modifique el valor de **Período** a **30** minutos y el de **Frecuencia** a 5. La regla se ejecutará cada cinco minutos y devolverá los registros que se crearon dentro de los últimos 30 minutos desde la hora actual.  El hecho de establecer el período de tiempo en una ventana más amplia justifica la posibilidad de latencia en los datos y garantiza que la consulta devuelve datos para evitar un falso negativo en aquellos casos en los que la alerta nunca se activa.
6. Haga clic en **Listo** para finalizar la regla de alertas.<br><br> ![Configurar la señal de alerta](media/configure-azure-monitor/alert-signal-logic-02.png)<br>
7. Ahora, en el segundo paso, proporcione un nombre para la alerta en el campo **Nombre de la regla de alertas**, como **Alerta para todos los eventos de error**.  Especifique una **Descripción** con los detalles específicos de la alerta y seleccione **Crítico (Gravedad 0)** para el valor **Gravedad** de las opciones proporcionadas.
8. Para activar inmediatamente la regla de alertas en la creación, acepte el valor predeterminado de **Habilitar regla tras la creación**.
9. En el paso tercero y último, especifique un **Grupo de acciones**, lo que garantiza que se realizan las mismas acciones cada vez que se desencadena una alerta y se puede utilizar para cada regla que se define. Configure un nuevo grupo de acciones con la información siguiente: a. Seleccione **Nuevo grupo de acciones** y aparecerá el panel **Agregar grupo de acciones**.
   b. En **Nombre del grupo de acciones**, especifique un nombre, como **Notificar las operaciones de TI -** y un **Nombre corto**, como **itops-n**.
   c. Compruebe que los valores predeterminados de **Suscripción** y **Grupo de recursos** son correctos. Si no es así, seleccione los valores correctos en la lista desplegable.
   d. En la sección Acciones, especifique un nombre para la acción, como **Enviar correo electrónico** y en **Tipo de acción**, seleccione **Correo electrónico/SMS/Push/Voz** en la lista desplegable. El panel de propiedades **Correo electrónico/SMS/Push/Voz** se abrirá a la derecha para proporcionar información adicional.
   e. En el panel **Correo electrónico/SMS/Push/Voz**, seleccione y configure sus preferencias. Por ejemplo, habilite **Correo electrónico** y proporcione una dirección SMTP válida de correo electrónico a la que entregar el mensaje.
   f. Haga clic en **Aceptar** para guardar los cambios.<br><br>

    ![Creación de un grupo de acciones](media/configure-azure-monitor/action-group-properties-01.png)

10. Seleccione **Aceptar** para crear el grupo de acciones.
11. Haga clic en **Crear regla de alertas** para finalizar la regla de alertas. Se iniciará la ejecución de inmediato.<br><br> ![Completar la creación de nueva regla de alertas](media/configure-azure-monitor/alert-rule-01.png)<br>

### <a name="example-alert"></a>Alerta de ejemplo

Como referencia, este es el aspecto de una alerta de ejemplo en Azure.

![GIF de la alerta en Azure](media/configure-azure-monitor/alert.gif)

A continuación se muestra un ejemplo del correo electrónico que se enviará mediante Azure Monitor:

![Ejemplo de correo electrónico de alerta](media/configure-azure-monitor/warning.png)

## <a name="additional-references"></a>Referencias adicionales

- [Introducción a Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- Para obtener información más detallada, lea la [documentación de Azure monitor](/azure/azure-monitor/learn/tutorial-viewdata).
- Lea este tema para obtener información general sobre cómo [conectarse a otros servicios híbridos de Azure](../../manage/windows-admin-center/azure/index.md).
