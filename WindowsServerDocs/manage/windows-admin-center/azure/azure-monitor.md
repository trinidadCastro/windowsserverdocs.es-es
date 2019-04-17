---
title: Supervisar los servidores y configurar alertas con el Monitor de Azure desde Windows Admin Center
description: Windows Admin Center (proyecto Honolulu) se integra con el Monitor de Azure
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 6ada708bf7dd8cd08e1bc2620be5244a07beac7d
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297105"
---
# Supervisar los servidores y configurar alertas con el Monitor de Azure desde Windows Admin Center

[Obtén más información acerca de la integración de Azure con Windows Admin Center.](../plan/azure-integration-options.md)

[Monitor de Azure](https://docs.microsoft.com/azure/azure-monitor/overview) es una solución que se recopila, analiza y actúa en la telemetría de una variedad de recursos, incluidos los servidores de Windows y las máquinas virtuales, tanto en local y en la nube. Aunque Azure Monitor extrae los datos de las máquinas virtuales de Azure y otros recursos de Azure, en este artículo se centra en cómo funciona el Monitor de Azure con servidores locales y las máquinas virtuales, específicamente con Windows Admin Center. Si estás interesado obtener información sobre cómo puedes usar a Azure Monitor para obtener alertas de correo electrónico acerca de los clústeres, lee sobre el [uso de Monitor de Azure para enviar correos electrónicos de errores de estado del servicio](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## ¿Cómo funciona el Monitor de Azure?
![IMG](../media/azure-monitor-diagram.png) datos generados a partir de los servidores locales de Windows se recopilan en un área de trabajo de Log Analytics en el Monitor de Azure. Dentro de un área de trabajo, puedes habilitar diversas soluciones de supervisión, establece de lógica que proporcionan detalles para un escenario en particular. Por ejemplo, administración de actualizaciones de Azure, Azure Security Center y el Monitor de Azure para las máquinas virtuales son todas las soluciones de supervisión que pueden habilitarse dentro de un área de trabajo. 

Al habilitar una solución de supervisión en un área de trabajo de Log Analytics, todos los servidores de informes en esa área de trabajo inicia la recopilación de datos relevantes para que esa solución, para que la solución pueda generar información para todos los servidores en el área de trabajo. 

Para recopilar datos de telemetría en un servidor local y enviarlo al área de trabajo de Log Analytics, Azure Monitor requiere la instalación de Microsoft Monitoring Agent o la MMA. Ciertas soluciones de supervisión también requieren a un agente secundario. Por ejemplo, Monitor de Azure para las máquinas virtuales también depende de un agente de ServiceMap para la funcionalidad adicional que ofrece esta solución. 

Algunas soluciones, como la administración de actualización de Azure, también dependen de automatización de Azure, que te permite administrar recursos de forma centralizada en entornos de que no sean de Azure y Azure. Por ejemplo, administración de actualizaciones de Azure utiliza la automatización de Azure para programar y organizar la instalación de actualizaciones en equipos en su entorno, de forma centralizada, desde el portal de Azure.


## ¿Cómo permite usar Azure Monitor Windows Admin Center?

Desde dentro de WAC, puedes habilitar dos soluciones de supervisión:

- [Administración de actualizaciones de Azure](azure-update-management.md) (en la herramienta actualizaciones)
- Monitor de Azure para las máquinas virtuales (en la configuración del servidor), también se conoce como información de máquinas virtuales

Puede comenzar a usar el Monitor de Azure desde cualquiera de estas herramientas. Si nunca has usado Azure Monitor antes, automáticamente aprovisionará WAC un área de trabajo de Log Analytics (y cuenta de Azure automatización, si es necesario) e instalar y configurar Microsoft Monitoring Agent (MMA) en el servidor de destino. A continuación, se instalará la solución correspondiente en el área de trabajo. 

Por ejemplo, si en primer lugar, ir a la herramienta de actualizaciones para configurar la administración de actualizaciones de Azure, WAC lo siguiente:

1. Instalar la MMA en la máquina
2. Crear el área de trabajo de Log Analytics y la cuenta de Azure automatización (ya que una cuenta de Azure automatización en este caso es necesaria)
3. Instalar la solución de administración de actualizaciones en el área de trabajo recién creado.

Si quieres agregar otra solución de supervisión desde dentro de WAC en el mismo servidor, WAC instalará simplemente esa solución en el área de trabajo existente al que está conectado dicho servidor. Además, WAC instalará a todos los agentes necesarios.

Si se conecta a un servidor diferente, pero ya has configurado un área de trabajo de Log Analytics (ya sea a través de WAC o manualmente en el Portal de Azure), también puede instalar al agente MMA en el servidor y conectarla a un área de trabajo existente. Cuando se conecta a un servidor en un área de trabajo, se inicia automáticamente la recopilación de datos e informar a soluciones instaladas en esa área de trabajo.

## Azure supervise para máquinas virtuales (también conocido como Información de máquina virtual)
>Se aplica a: Windows Admin Center Preview

Cuando configuras Monitor de Azure para las máquinas virtuales en configuración del servidor, Windows Admin Center permite al Monitor de Azure para soluciones de máquinas virtuales, también conocida como información de máquina Virtual. Esta solución permite supervisar el estado del servidor y eventos, crear alertas de correo electrónico, obtener una vista consolidada de rendimiento del servidor a través de tu entorno y visualizar las aplicaciones, los sistemas y servicios conectados a un servidor determinado.

> [!NOTE]
> A pesar de su nombre, información de máquina virtual funciona para servidores físicos, así como las máquinas virtuales.

Con el Monitor de Azure libre 5 GB de concesión de datos/mes/cliente, fácilmente pruebes esto para un servidor o dos sin preocuparse por obteniendo el cargo. Leer en las ventajas adicionales de consulta de incorporación de servidores en el Monitor de Azure, como la obtención de una vista consolidada de rendimiento de los sistemas en todos los servidores en su entorno.

### **Configurar el servidor para su uso con monitores de Azure**

Desde la página de información general de una conexión de servidor, haz clic en el nuevo botón "Administrar alertas" o, ve a configuración del servidor > alertas y supervisión. En esta página, incorporar el servidor Azure Monitor haciendo clic en "Configurar" y la finalización del panel de configuración. Centro de administración se encarga de aprovisionamiento en el área de trabajo de Log Analytics de Azure, la instalación del agente necesario y garantizar que la solución de información de máquina virtual está configurado. Una vez completado, el servidor enviará datos del contador de rendimiento al Monitor de Azure, lo que te permite ver y crear alertas de correo electrónico en función de este servidor, desde el portal de Azure.

### **Crear alertas de correo electrónico**

Una vez que el servidor que ha adjuntado al Monitor de Azure, puedes usar los hipervínculos dentro de la página de alertas y supervisión de > configuración inteligentes para navegar en el Portal de Azure. Centro de administración habilita automáticamente los contadores de rendimiento que se recopilen, por tanto, puede [crear una nueva alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) por personalizar una de muchas consultas predefinidas o escribir tu propio.

### ** Obtener una vista consolidada en varios servidores **

Si puedes incorporar varios servidores para un área de trabajo de Log Analytics solo dentro de Monitor de Azure, puedes obtener una vista consolidada de todos estos servidores de la [solución de información de máquinas virtuales](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) dentro de Monitor de Azure.  (Ten en cuenta que solo las pestañas de rendimiento y mapas de perspectivas de máquinas virtuales de Azure Monitor funcionará con servidores locales: las funciones de pestaña estado solo con máquinas virtuales de Azure.) Para ver esto en el portal de Azure, ve a Azure Monitor > máquinas virtuales (bajo Insights) y navegar a las pestañas "Rendimiento" o "Maps".

### **Visualizar las aplicaciones, los sistemas y servicios conectados a un servidor determinado**

Cuando un servidor en la solución de información de máquina virtual en el Monitor de Azure de centro de administración onboards, se también ilumina una funcionalidad denominada [Mapa de servicio](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Esta funcionalidad automáticamente descubre los componentes de la aplicación y asigna la comunicación entre los servicios para que se pueden visualizar fácilmente las conexiones entre servidores con más detalle desde el portal de Azure. Puedes encontrarlo en la > portal Azure Azure Monitor > máquinas virtuales (bajo información), y navegar a la pestaña "Maps".

> [!NOTE]
> Las visualizaciones para investigar las máquinas virtuales de Monitor de Azure se ofrecen actualmente en regiones públicas 6.  Para obtener la información más reciente, comprueba el [Monitor de Azure para la documentación de las máquinas virtuales](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Debes implementar el área de trabajo de Log Analytics en una de las regiones admitidas para obtener las ventajas adicionales proporcionadas por la solución de información de máquinas virtuales que se ha descrito anteriormente.

## Deshabilitar supervisión

Para desconectar el servidor completamente el área de trabajo de Log Analytics, desinstalar al agente MMA. Esto significa que este servidor no volverá a enviar datos al área de trabajo, y todas las soluciones instaladas en esa área de trabajo dejarán de recopilan y procesan datos desde el servidor. Sin embargo, esto no afecta a la propia área de trabajo: todos los recursos de informar a esa área de trabajo seguirán hacerlo. Para desinstalar al agente MMA dentro WAC, ve a las aplicaciones & características, busca a Microsoft Monitoring Agent y haga clic en desinstalar.

Si quieres desactivar una solución específica dentro de un área de trabajo, debes [quitar la solución de supervisión desde el portal de Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Quitar una solución de supervisión significa que las perspectivas crean por que ya no se generarán solución para _cualquier_ de los servidores de informes en esa área de trabajo. Por ejemplo, si desinstala al Monitor de Azure para soluciones de máquinas virtuales, ya no verán información sobre el rendimiento de la máquina virtual o servidor desde cualquiera de los equipos conectados a mi área de trabajo.