---
title: Supervisar servidores y configurar alertas con Azure Monitor desde el centro de administración de Windows
description: El centro de administración de Windows (Project Honolulu) se integra con Azure Monitor
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.date: 03/24/2019
ms.openlocfilehash: 81501cb5f4255b7a65ebc5f9f6cb9413938864f0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940126"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Supervisar servidores y configurar alertas con Azure Monitor desde el centro de administración de Windows

[Más información acerca de la integración de Azure con el centro de administración de Windows.](../plan/azure-integration-options.md)

[Azure monitor](https://docs.microsoft.com/azure/azure-monitor/overview) es una solución que recopila, analiza y actúa en la telemetría de una variedad de recursos, incluidos servidores y máquinas virtuales de Windows, tanto en el entorno local como en la nube. Aunque Azure Monitor extrae datos de máquinas virtuales de Azure y otros recursos de Azure, este artículo se centra en cómo funciona Azure Monitor con servidores locales y máquinas virtuales, específicamente con el centro de administración de Windows. Si está interesado en obtener información sobre cómo puede usar Azure Monitor para recibir alertas de correo electrónico sobre el clúster hiperconvergido, Lea sobre el [uso de Azure monitor para enviar correos electrónicos para errores de servicio de mantenimiento](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>¿Cómo funciona Azure Monitor?
![](../media/azure-monitor-diagram.png)los datos de IMG generados desde servidores locales de Windows se recopilan en un área de trabajo de log Analytics en Azure monitor. En un área de trabajo puede habilitar varias soluciones de supervisión: conjuntos de lógica que proporcionan información para un escenario determinado. Por ejemplo, Azure Update Management, Azure Security Center y Azure Monitor para VM son soluciones de supervisión que se pueden habilitar dentro de un área de trabajo.

Cuando se habilita una solución de supervisión en un área de trabajo de Log Analytics, todos los servidores que envían notificaciones a esa área de trabajo empiezan a recopilar datos pertinentes para esa solución, de modo que la esta pueda generar información para todos los servidores del área de trabajo.

Para recopilar datos de telemetría en un servidor local e insertarlos en el área de trabajo de Log Analytics, Azure Monitor requiere la instalación del Microsoft Monitoring Agent o MMA. Ciertas soluciones de supervisión también requieren un agente secundario. Por ejemplo, Azure Monitor para VM también depende de un agente ServiceMap para la funcionalidad adicional que proporciona esta solución.

Algunas soluciones, como Azure Update Management, dependen también de Azure Automation, lo que le permite administrar recursos de forma centralizada en entornos que pueden ser de Azure o no. Por ejemplo, Azure Update Management usa Azure Automation para programar y coordinar la instalación de actualizaciones en las máquinas de su entorno, de forma centralizada, desde Azure Portal.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>¿Cómo le permite Windows Admin Center usar Azure Monitor?

Desde WAC, puede habilitar dos soluciones de supervisión:

- [Azure Update Management](azure-update-management.md) (en la herramienta Actualizaciones)
- Azure Monitor para VM (en la configuración del servidor), a. k. a Virtual Machines Insights

Puede empezar a usar Azure Monitor de cualquiera de estas herramientas. Si nunca ha usado Azure Monitor antes, WAC aprovisionará automáticamente un área de trabajo Log Analytics (y Azure Automation cuenta, si es necesario), e instalará y configurará el Microsoft Monitoring Agent (MMA) en el servidor de destino. A continuación, se instalará la solución correspondiente en el área de trabajo.

Por ejemplo, si primero va a la herramienta de actualizaciones para configurar Azure Update Management, WAC:

1. Instalación de MMA en el equipo
2. Cree el área de trabajo Log Analytics y la cuenta de Azure Automation (ya que en este caso se necesita una cuenta de Azure Automation)
3. Instalará la solución Update Management en el área de trabajo recién creada.

Si desea agregar otra solución de supervisión desde WAC en el mismo servidor, WAC simplemente instalará esa solución en el área de trabajo existente a la que está conectado el servidor. WAC instalará además cualquier otro agente necesario.

Si se conecta a un servidor diferente, pero ya ha configurado un Log Analytics área de trabajo (ya sea a través de WAC o manualmente en el portal de Azure), también puede instalar el agente MMA en el servidor y conectarlo a un área de trabajo existente. Al conectar un servidor a un área de trabajo, esta empieza automáticamente a recopilar datos y generar informes para las soluciones instaladas en esa área de trabajo.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Azure Monitor for VM (anteriormente Virtual Machine Insights)
>Se aplica a: vista previa del centro de administración de Windows

Al configurar Azure Monitor para VM en la configuración del servidor, el centro de administración de Windows habilita la solución de Azure Monitor para VM, también conocida como información de la máquina virtual. Esta solución le permite supervisar el estado y los eventos del servidor, crear alertas de correo electrónico, obtener una vista consolidada del rendimiento del servidor en todo el entorno y visualizar las aplicaciones, sistemas y servicios conectados a un servidor determinado.

> [!NOTE]
> A pesar de su nombre, VM Insights funciona para los servidores físicos y las máquinas virtuales.

Con la oferta de 5 GB gratis de datos al mes por cliente de Azure Monitor, puede probar fácilmente un servidor o dos sin preocuparse de pagar nada. Siga leyendo para conocer más ventajas de la incorporación de servidores en Azure Monitor como, por ejemplo, la obtención de una vista consolidada del rendimiento de los sistemas en los servidores de su entorno.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurar el servidor para su uso con Azure Monitor**

En la página información general de una conexión de servidor, haga clic en el botón nuevo "Administrar alertas" o vaya a configuración del servidor > supervisión y alertas. En esta página, incorpore el servidor a Azure Monitor haciendo clic en "configurar" y completando el panel de instalación. El centro de administración se encarga de aprovisionar el área de trabajo de Azure Log Analytics, instalar el agente necesario y asegurarse de que la solución de VM Insights está configurada. Una vez terminado, el servidor enviará los datos del contador de rendimiento a Azure Monitor, lo que le permitirá ver y crear alertas de correo electrónico basadas en este servidor, desde Azure Portal.

### <a name="create-email-alerts"></a>**Crear alertas de correo electrónico**

Una vez que haya conectado el servidor a Azure Monitor, puede usar los hipervínculos inteligentes en la página Configuración > supervisión y alertas para ir a Azure portal. El centro de administración permite automáticamente la recopilación de contadores de rendimiento, por lo que puede [crear fácilmente una nueva alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) personalizando una de muchas consultas predefinidas o escribiendo las suyas propias.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>* * Obtener una vista consolidada en varios servidores * *

Si incorpora varios servidores a una sola área de trabajo de Log Analytics dentro de Azure Monitor, puede obtener una vista consolidada de todos estos servidores de la [solución de virtual machines Insights](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) en Azure monitor.  (Tenga en cuenta que solo las pestañas rendimiento y mapas de Virtual Machines Insights para Azure Monitor funcionarán con servidores locales: la pestaña mantenimiento solo funciona con máquinas virtuales de Azure). Para verlo en el Azure Portal, vaya a Azure Monitor > Virtual Machines (en información detallada) y navegue hasta las pestañas "rendimiento" o "asignaciones".

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualización de las aplicaciones, los sistemas y los servicios conectados a un servidor determinado**

Cuando el centro de administración incorpora un servidor en la solución de VM Insights dentro de Azure Monitor, también se enciende una funcionalidad llamada [Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Esta funcionalidad detecta automáticamente los componentes de la aplicación y asigna la comunicación entre los servicios para que pueda visualizar fácilmente las conexiones entre los servidores con gran detalle desde Azure Portal. Para encontrarlo, vaya a la Azure Portal > Azure Monitor > Virtual Machines (en información detallada) y vaya a la pestaña "Maps" (asignaciones).

> [!NOTE]
> Actualmente, las visualizaciones de Virtual Machines Insights para Azure Monitor se ofrecen en 6 regiones públicas.  Para obtener la información más reciente, consulte la [documentación de Azure Monitor para VM](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Debe implementar el área de trabajo de Log Analytics en una de las regiones admitidas para obtener las ventajas adicionales que proporciona la solución Virtual Machines Insights que se ha descrito anteriormente.

## <a name="disabling-monitoring"></a>Deshabilitar la supervisión

Para desconectar completamente el servidor del área de trabajo Log Analytics, desinstale el agente MMA. Esto significa que este servidor ya no enviará datos al área de trabajo y que todas las soluciones instaladas en esa área de trabajo dejarán de recopilar y procesar los datos de ese servidor. Sin embargo, esto no afecta al propio área de trabajo: todos los recursos que informan a esa área de trabajo seguirán haciendo esto. Para desinstalar el agente de MMA en Windows Admin Center, conéctese al servidor y, a continuación, vaya a **aplicaciones instaladas**, busque Microsoft Monitoring Agent y, a continuación, seleccione **Quitar**.

Si desea desactivar una solución específica dentro de un área de trabajo, tendrá que [quitar la solución de supervisión de Azure Portal](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). La eliminación de una solución de supervisión significa que la información creada por esa solución ya no se generará para _todos_ los servidores que envían notificaciones a esa área de trabajo. Por ejemplo, si desinstalo la Azure Monitor para VM solución, ya no veremos información sobre el rendimiento de la máquina virtual o del servidor desde cualquiera de las máquinas conectadas a mi área de trabajo.
