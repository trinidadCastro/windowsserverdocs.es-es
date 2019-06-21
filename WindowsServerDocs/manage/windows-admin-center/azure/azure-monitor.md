---
title: Supervisar los servidores y la configuración de alertas con Azure Monitor desde Windows Admin Center
description: Windows Admin Center (proyecto Honolulu) se integra con Azure Monitor
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 8f7baba465071cc95ab7492037ff25c5cd58219e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280435"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Supervisar los servidores y la configuración de alertas con Azure Monitor desde Windows Admin Center

[Más información sobre la integración de Azure con Windows Admin Center.](../plan/azure-integration-options.md)

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) es una solución que recopila, analiza y actúa en los datos de telemetría desde una variedad de recursos, incluidos los servidores de Windows y las máquinas virtuales, tanto locales como en la nube. Aunque Azure Monitor extrae datos de máquinas virtuales de Azure y otros recursos de Azure, en este artículo se centra en cómo funciona Azure Monitor con servidores locales y máquinas virtuales, específicamente con Windows Admin Center. Si está interesado en conocer cómo puede usar Azure Monitor para obtener alertas por correo electrónico sobre el clúster hiperconvergido, obtenga información sobre [mediante Azure Monitor para enviar mensajes de correo electrónico para errores de servicio de mantenimiento](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>¿Cómo funciona Azure Monitor?
![IMG](../media/azure-monitor-diagram.png) se recopilan los datos generados a partir de servidores de Windows de forma local en un área de trabajo de Log Analytics en Azure Monitor. Dentro de un área de trabajo, puede habilitar varias soluciones de supervisión, conjuntos de lógica que proporcionan información para un escenario determinado. Por ejemplo, la administración de actualizaciones de Azure, Azure Security Center y Azure Monitor para las máquinas virtuales son todas las soluciones de supervisión que se pueden habilitar dentro de un área de trabajo. 

Cuando se habilita una solución de supervisión en un área de trabajo de Log Analytics, todos los servidores de informes a esa área de trabajo se iniciará la recopilación de datos relevantes para esa solución, para que la solución puede generar información para todos los servidores en el área de trabajo. 

Para recopilar datos de telemetría en un servidor local y los insertará en el área de trabajo de Log Analytics, Azure Monitor requiere la instalación de Microsoft Monitoring Agent, o el agente MMA. Determinadas soluciones de supervisión también requieren a un agente secundaria. Por ejemplo, Azure Monitor para las máquinas virtuales también depende de un agente de Service Map para que esta solución proporciona una funcionalidad adicional. 

Algunas soluciones como Update Management de Azure, también dependen de Azure Automation, que le permite administrar de forma centralizada recursos en Azure y los entornos que no son de Azure. Por ejemplo, la administración de actualizaciones de Azure usa Azure Automation para programar y organizar la instalación de actualizaciones en equipos en su entorno, de forma centralizada, desde el portal de Azure.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>¿Cómo permite Windows Admin Center usar Azure Monitor?

Desde WAC, puede habilitar dos soluciones de supervisión:

- [Administración de actualizaciones de Azure](azure-update-management.md) (en la herramienta de actualizaciones)
- Azure Monitor para máquinas virtuales (en el servidor de configuración), conocidas también como máquinas virtuales insights

Puede empezar a trabajar con Azure Monitor desde cualquiera de estas herramientas. Si nunca ha usado Azure Monitor antes, WAC aprovisionará automáticamente un área de trabajo de Log Analytics (y cuenta de Azure Automation, si es necesario) e instalar y configurar Microsoft Monitoring Agent (MMA) en el servidor de destino. A continuación, instalará la solución correspondiente en el área de trabajo. 

Por ejemplo, si en primer lugar, vaya a la herramienta de actualizaciones para configurar la administración de actualizaciones de Azure, WAC hará lo siguiente:

1. Instalar el agente de MMA en la máquina
2. Crear el área de trabajo de Log Analytics y la cuenta de Azure Automation (ya que una cuenta de Azure Automation en este caso es necesaria)
3. Instale la solución de administración de actualizaciones en el área de trabajo recién creado.

Si desea agregar otra solución de supervisión desde dentro de WAC en el mismo servidor, WAC simplemente instalará esa solución en el área de trabajo existente al que está conectado ese servidor. Además, WAC instalará a los otros agentes necesarios.

Si conectarse a un servidor diferente, pero ya ha configurado un área de trabajo de Log Analytics (ya sea a través de WAC o manualmente en el Portal de Azure), también puede instalar al agente de MMA en el servidor y conectarlo hasta un área de trabajo. Cuando se conecta a un servidor en un área de trabajo, inicia automáticamente la recopilación de datos e informes a las soluciones instaladas en esa área de trabajo.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Supervisión de Azure para las máquinas virtuales (también conocida como Información de la máquina virtual)
>Se aplica a: Versión preliminar de Windows Admin Center

Al configurar Azure Monitor para las máquinas virtuales en el servidor de configuración, Windows Admin Center habilita al Monitor de Azure para soluciones de máquinas virtuales, también conocido como información de máquina Virtual. Esta solución permite supervisar los eventos y el estado del servidor, crear alertas de correo electrónico, obtener una vista consolidada del rendimiento del servidor en un entorno y visualizar las aplicaciones, sistemas y servicios conectados a un servidor determinado.

> [!NOTE]
> A pesar de su nombre, información de la máquina virtual funciona para servidores físicos, así como las máquinas virtuales.

Con Azure Monitor libre 5 GB de provisión de datos/mes/cliente, puede probar fácilmente esta para un servidor o dos sin preocuparse de los cargos. Siga leyendo para ver otras ventajas de incorporación de servidores en Azure Monitor, como obtener una vista consolidada de rendimiento del sistema a través de los servidores en su entorno.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurar el servidor para su uso con Azure Monitor**

En la página información general de una conexión al servidor, haga clic en el nuevo botón "Administrar alertas de" o vaya a configuración del servidor > supervisión y alertas. En esta página, incorporar el servidor a Azure Monitor, haga clic en "Configurar" y completar el panel de configuración. Centro de administración se encarga de aprovisionar el área de trabajo de Log Analytics de Azure, instalar al agente es necesario, y garantizar que la solución de información de la máquina virtual está configurada. Una vez que haya finalizado, el servidor enviará datos del contador de rendimiento a Azure Monitor, lo que le permite ver y crear alertas de correo electrónico en función de este servidor desde el portal de Azure.

### <a name="create-email-alerts"></a>**Crear alertas de correo electrónico**

Una vez que el servidor que ha adjuntado a Azure Monitor, puede utilizar los hipervínculos dentro de la configuración inteligentes > página de supervisión y alertas para navegar al Portal de Azure. Centro de administración habilita automáticamente los contadores de rendimiento recopilar, por lo que le resultará muy fácil [crea una nueva alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) personalizar una de muchas consultas predefinidas o creando el suyo propio.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>** Obtener una vista consolidada en varios servidores **

Si puede incorporar varios servidores para una sola área de trabajo de Log Analytics en Azure Monitor, puede obtener una vista consolidada de todos estos servidores desde el [solución Insights de las máquinas virtuales](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) dentro de Azure Monitor.  (Tenga en cuenta que solo las pestañas de rendimiento y mapas de información de las máquinas virtuales de Azure Monitor funcione con servidores locales: las funciones de la ficha de estado solo con máquinas virtuales de Azure.) Para ver esto en Azure portal, vaya a Azure Monitor > máquinas virtuales (con la información) y navegue hasta las pestañas "Rendimiento" o "Asignaciones".

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualización de las aplicaciones, sistemas y servicios conectados a un servidor determinado.**

Al centro de administración incorpora un servidor en la solución de información de máquina virtual dentro de Azure Monitor, también introduce una funcionalidad denominada " [Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Esta funcionalidad automáticamente descubre componentes de la aplicación y asigna la comunicación entre servicios para que pueda visualizar fácilmente las conexiones entre servidores con gran detalle desde el portal de Azure. Puede encontrarlo, vaya al portal de Azure > Azure Monitor > máquinas virtuales (con la información) y navegar a la pestaña "Asignaciones".

> [!NOTE]
> Las visualizaciones para obtener información de las máquinas virtuales de Azure Monitor se ofrecen en las regiones públicas de 6 actualmente.  Para obtener la información más reciente, compruebe el [Azure Monitor para documentación sobre máquinas virtuales](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Debe implementar el área de trabajo de Log Analytics en una de las regiones admitidas para obtener las ventajas adicionales proporcionadas por la solución de información de las máquinas virtuales que se ha descrito anteriormente.

## <a name="disabling-monitoring"></a>Deshabilitar la supervisión

Para desconectar por completo el servidor desde el área de trabajo de Log Analytics, desinstale al agente MMA. Esto significa que este servidor no volverá a enviar datos al área de trabajo, y todas las soluciones instaladas en esa área de trabajo dejarán de recopilan y procesan los datos de ese servidor. Sin embargo, esto no influye en el área de trabajo: todos los recursos que informan a esa área de trabajo continuará para hacerlo. Para desinstalar al agente de MMA en WAC, vaya a aplicaciones y características, busque a Microsoft Monitoring Agent y haga clic en desinstalar.

Si desea desactivar una solución específica dentro de un área de trabajo, deberá [quitar la solución de supervisión de Azure portal](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Eliminación de una solución de supervisión significa que ya no se generará la información creada por esa solución para _cualquier_ de los servidores de informes a esa área de trabajo. Por ejemplo, si se desinstala al Monitor de Azure para la solución de máquinas virtuales, dejará de ver información detallada sobre el rendimiento de máquina virtual o servidor desde cualquiera de las máquinas conectadas a mi área de trabajo.