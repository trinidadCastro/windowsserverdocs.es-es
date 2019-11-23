---
title: Supervisar servidores y configurar alertas con Azure Monitor desde el centro de administración de Windows
description: El centro de administración de Windows (Project Honolulu) se integra con Azure Monitor
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 03/24/2019
ms.openlocfilehash: 28108a79bbdc654f6437a698c158a3f74d4423ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407013"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Supervisar servidores y configurar alertas con Azure Monitor desde el centro de administración de Windows

[Más información acerca de la integración de Azure con el centro de administración de Windows.](../plan/azure-integration-options.md)

[Azure monitor](https://docs.microsoft.com/azure/azure-monitor/overview) es una solución que recopila, analiza y actúa en la telemetría de una variedad de recursos, incluidos servidores y máquinas virtuales de Windows, tanto en el entorno local como en la nube. Aunque Azure Monitor extrae datos de máquinas virtuales de Azure y otros recursos de Azure, este artículo se centra en cómo funciona Azure Monitor con servidores locales y máquinas virtuales, específicamente con el centro de administración de Windows. Si está interesado en obtener información sobre cómo puede usar Azure Monitor para recibir alertas de correo electrónico sobre el clúster hiperconvergido, Lea sobre el [uso de Azure monitor para enviar correos electrónicos para errores de servicio de mantenimiento](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>¿Cómo funciona Azure Monitor?
![IMG](../media/azure-monitor-diagram.png) datos generados desde servidores locales de Windows se recopilan en un área de trabajo de Log Analytics en Azure Monitor. Dentro de un área de trabajo, puede habilitar varias soluciones de supervisión: conjuntos de lógica que proporcionan información para un escenario determinado. Por ejemplo, Azure Update Management, Azure Security Center y Azure Monitor para VM son soluciones de supervisión que se pueden habilitar dentro de un área de trabajo. 

Cuando se habilita una solución de supervisión en un área de trabajo Log Analytics, todos los servidores que informan a esa área de trabajo comenzarán a recopilar datos relevantes para esa solución, de modo que la solución pueda generar información para todos los servidores del área de trabajo. 

Para recopilar datos de telemetría en un servidor local e insertarlos en el área de trabajo de Log Analytics, Azure Monitor requiere la instalación del Microsoft Monitoring Agent o MMA. Ciertas soluciones de supervisión también requieren un agente secundario. Por ejemplo, Azure Monitor para VM también depende de un agente ServiceMap para la funcionalidad adicional que proporciona esta solución. 

Algunas soluciones, como Azure Update Management, dependen también de Azure Automation, lo que permite administrar recursos de forma centralizada en entornos de Azure y no de Azure. Por ejemplo, Azure Update Management usa Azure Automation para programar y coordinar la instalación de actualizaciones en las máquinas de su entorno, de forma centralizada, desde el Azure Portal.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>¿Cómo le permite usar Azure Monitor el centro de administración de Windows?

Desde WAC, puede habilitar dos soluciones de supervisión:

- [Update Management de Azure](azure-update-management.md) (en la herramienta de actualización)
- Azure Monitor para VM (en la configuración del servidor), a. k. a Virtual Machines Insights

Puede empezar a usar Azure Monitor de cualquiera de estas herramientas. Si nunca ha usado Azure Monitor antes, WAC aprovisionará automáticamente un área de trabajo Log Analytics (y Azure Automation cuenta, si es necesario), e instalará y configurará el Microsoft Monitoring Agent (MMA) en el servidor de destino. A continuación, se instalará la solución correspondiente en el área de trabajo. 

Por ejemplo, si primero va a la herramienta de actualizaciones para configurar Azure Update Management, WAC:

1. Instalación de MMA en el equipo
2. Cree el área de trabajo Log Analytics y la cuenta de Azure Automation (ya que en este caso se necesita una cuenta de Azure Automation)
3. Instale la solución Update Management en el área de trabajo recién creada.

Si desea agregar otra solución de supervisión desde WAC en el mismo servidor, WAC simplemente instalará esa solución en el área de trabajo existente a la que está conectado el servidor. WAC instalará además cualquier otro agente necesario.

Si se conecta a un servidor diferente, pero ya ha configurado un Log Analytics área de trabajo (ya sea a través de WAC o manualmente en el portal de Azure), también puede instalar el agente MMA en el servidor y conectarlo a un área de trabajo existente. Al conectar un servidor a un área de trabajo, se inicia automáticamente la recopilación de datos y la generación de informes en las soluciones instaladas en dicha área de trabajo.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Azure Monitor para máquinas virtuales (también conocidas como Información sobre máquinas virtuales)
>Se aplica a: vista previa del centro de administración de Windows

Al configurar Azure Monitor para VM en la configuración del servidor, el centro de administración de Windows habilita la solución de Azure Monitor para VM, también conocida como información de la máquina virtual. Esta solución le permite supervisar el estado y los eventos del servidor, crear alertas de correo electrónico, obtener una vista consolidada del rendimiento del servidor en todo el entorno y visualizar las aplicaciones, los sistemas y los servicios conectados a un servidor determinado.

> [!NOTE]
> A pesar de su nombre, VM Insights funciona para los servidores físicos y las máquinas virtuales.

Con la deducción de 5 GB de datos/mes/cliente gratis de Azure Monitor, puede probar fácilmente un servidor o dos sin preocuparse por la obtención de cargos. Siga leyendo para ver más ventajas de la incorporación de servidores en Azure Monitor, como obtener una vista consolidada del rendimiento de los sistemas en los servidores de su entorno.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurar el servidor para su uso con Azure Monitor**

En la página información general de una conexión de servidor, haga clic en el botón nuevo "Administrar alertas" o vaya a configuración del servidor > supervisión y alertas. En esta página, incorpore el servidor a Azure Monitor haciendo clic en "configurar" y completando el panel de instalación. El centro de administración se encarga de aprovisionar el área de trabajo de Azure Log Analytics, instalar el agente necesario y asegurarse de que la solución de VM Insights está configurada. Una vez completado, el servidor enviará los datos del contador de rendimiento a Azure Monitor, lo que le permitirá ver y crear alertas de correo electrónico basadas en este servidor, desde el Azure Portal.

### <a name="create-email-alerts"></a>**Crear alertas de correo electrónico**

Una vez que haya conectado el servidor a Azure Monitor, puede usar los hipervínculos inteligentes en la página Configuración > supervisión y alertas para ir a Azure portal. El centro de administración permite automáticamente la recopilación de contadores de rendimiento, por lo que puede [crear fácilmente una nueva alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) personalizando una de muchas consultas predefinidas o escribiendo las suyas propias.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * Obtener una vista consolidada en varios servidores * *

Si incorpora varios servidores a una sola área de trabajo de Log Analytics dentro de Azure Monitor, puede obtener una vista consolidada de todos estos servidores de la [solución de virtual machines Insights](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) en Azure monitor.  (Tenga en cuenta que solo las pestañas rendimiento y mapas de Virtual Machines Insights para Azure Monitor funcionarán con servidores locales: la pestaña mantenimiento solo funciona con máquinas virtuales de Azure). Para verlo en el Azure Portal, vaya a Azure Monitor > Virtual Machines (en información detallada) y navegue hasta las pestañas "rendimiento" o "asignaciones".

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualización de las aplicaciones, los sistemas y los servicios conectados a un servidor determinado**

Cuando el centro de administración incorpora un servidor en la solución de VM Insights dentro de Azure Monitor, también se enciende una funcionalidad llamada [Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Esta funcionalidad detecta automáticamente los componentes de la aplicación y asigna la comunicación entre los servicios para que pueda visualizar fácilmente las conexiones entre los servidores con gran detalle desde el Azure Portal. Para encontrarlo, vaya a la Azure Portal > Azure Monitor > Virtual Machines (en información detallada) y vaya a la pestaña "Maps" (asignaciones).

> [!NOTE]
> Actualmente, las visualizaciones de Virtual Machines Insights para Azure Monitor se ofrecen en 6 regiones públicas.  Para obtener la información más reciente, consulte la [documentación de Azure monitor para VM](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Debe implementar el área de trabajo de Log Analytics en una de las regiones admitidas para obtener las ventajas adicionales que proporciona la solución Virtual Machines Insights que se ha descrito anteriormente.

## <a name="disabling-monitoring"></a>Deshabilitar la supervisión

Para desconectar completamente el servidor del área de trabajo Log Analytics, desinstale el agente MMA. Esto significa que este servidor ya no enviará datos al área de trabajo y todas las soluciones instaladas en esa área de trabajo dejarán de recopilar y procesar los datos de ese servidor. Sin embargo, esto no afecta al propio área de trabajo: todos los recursos que informan a esa área de trabajo seguirán haciendo esto. Para desinstalar el agente de MMA en WAC, vaya a aplicaciones & características, busque el Microsoft Monitoring Agent y haga clic en desinstalar.

Si desea desactivar una solución específica dentro de un área de trabajo, tendrá que [quitar la solución de supervisión del Azure portal](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). La eliminación de una solución de supervisión significa que la información que se crea en esa solución ya no se generará para _ninguno_ de los servidores que informan a esa área de trabajo. Por ejemplo, si desinstalo la Azure Monitor para VM solución, ya no veremos información sobre el rendimiento de la máquina virtual o del servidor desde cualquiera de las máquinas conectadas a mi área de trabajo.