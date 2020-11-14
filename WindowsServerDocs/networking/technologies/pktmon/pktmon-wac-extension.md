---
title: Extensión de supervisión de paquetes en el centro de administración de Windows
description: Use esta página para operar y usar el monitor de paquetes (Pktmon) a través del centro de administración de Windows.
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 85d204d1731ebcf21c55364f48ebe0e7b6dc157c
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632650"
---
# <a name="packet-monitoring-extension-in-windows-admin-center"></a>Extensión de supervisión de paquetes en el centro de administración de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

La extensión de supervisión de paquetes le permite trabajar y consumir el monitor de paquetes a través del centro de administración de Windows. La extensión le ayuda a diagnosticar la red mediante la captura y visualización del tráfico de red a través de la pila de red en un registro que es fácil de seguir y manipular.

## <a name="what-is-packet-monitor-pktmon"></a>¿Qué es el monitor de paquetes (Pktmon)?
El monitor de paquetes (Pktmon) es una herramienta de diagnóstico de red integrada y entre componentes para Windows. Se puede usar para la captura de paquetes, la detección de paquetes, el filtrado de paquetes y el recuento. La herramienta es especialmente útil en escenarios de virtualización, como las redes de contenedor y SDN, ya que proporciona visibilidad dentro de la pila de red.

## <a name="what-is-windows-admin-center"></a>¿Qué es Windows Admin Center?
El centro de administración de Windows es una herramienta de administración basada en explorador y implementada localmente que le permite administrar los servidores de Windows sin depender de Azure o de la nube. Windows Admin Center ofrece el control total de todos los aspectos de tu infraestructura de servidores y es especialmente útil para la administración de servidores en redes privadas que no están conectadas a Internet. Windows Admin Center es la evolución moderna de herramientas de administración como el Administrador de servidores y MMC.

## <a name="before-you-start"></a>Antes de empezar
- Para usar la herramienta, el servidor de destino debe ejecutar Windows Server 2019 versión 1903 (19H1) y versiones posteriores.
- [Instale el centro de administración de Windows](/windows-server/manage/windows-admin-center/deploy/install).
- Agregar un servidor al centro de administración de Windows:
  1. Haga clic en **+ Agregar** en **Todas las conexiones**.
  2. Elija esta opción para agregar una conexión de servidor.
  3. Escriba el nombre del servidor y, si se le solicita, las credenciales que se usarán.
  4. Haga clic en **Enviar** para finalizar.

El servidor se agregará a la lista de conexiones en la página **información general** .

## <a name="getting-started"></a>Introducción

Para acceder a la herramienta, navegue hasta el servidor que creó en el paso anterior y, a continuación, vaya a la extensión "supervisión de paquetes".

## <a name="applying-filters"></a>Aplicar filtros

Se recomienda encarecidamente aplicar filtros antes de iniciar cualquier captura de paquetes, ya que la solución de problemas de conectividad con un destino determinado es más fácil cuando se centra en una única secuencia de paquetes. Por otro lado, capturar todo el tráfico de red puede hacer que el resultado sea demasiado ruidoso para analizarlo. En consecuencia, la extensión le guía primero al panel filtros antes de iniciar la captura. Puede omitir este paso haciendo clic en **siguiente** para iniciar la captura sin filtros. El panel filtros le guía para agregar filtros en tres pasos.

1. Filtrado por componentes de pila de red

   Si desea capturar el tráfico que atraviesa solo componentes específicos, el primer paso del panel filtros muestra el diseño de la pila de red para que pueda seleccionar los componentes por los que desea filtrar. Este también es un buen lugar para analizar y comprender el diseño de la pila de red de la máquina.

   :::image type="content" source="media/filtering-by-networking-stack-components.png" alt-text="Ejemplo de filtrado por componentes de pila de red" border="true":::

2. Filtrar por parámetros de paquete

   En el segundo paso, el panel le permite filtrar los paquetes por sus parámetros. Para que se notifique un paquete, debe coincidir con todas las condiciones especificadas en al menos un filtro; se admite un máximo de 8 filtros a la vez. Para cada filtro, puede especificar parámetros de paquete como direcciones MAC, direcciones IP, puertos, Ethertype, protocolo de transporte, ID. de VLAN.

   - Cuando se especifican dos equipos Mac, direcciones IP o puertos, la herramienta no distinguirá entre el origen o el destino. capturará los paquetes que tienen ambos valores, ya sea como origen o destino. Sin embargo, los filtros de visualización pueden hacer esa distinción; Vea la sección [filtros para mostrar](#display-filters) a continuación.
   - Para filtrar aún más los paquetes TCP, se puede proporcionar una lista opcional de marcas TCP para la coincidencia. Las marcas admitidas son FIN, SYN, RST, PSH, ACK, URG, ECE y CWR.
   - Si se activa el cuadro **encapsulación** , la herramienta aplica el filtro a los paquetes internos encapsulados, además del paquete externo. Los métodos de encapsulación admitidos son VXLAN, GRE, NVGRE y IP-in-IP. El puerto VXLAN personalizado es opcional y su valor predeterminado es 4789.

   :::image type="content" source="media/filtering-by-packet-parameters.png" alt-text="Ejemplo de filtrado por parámetros de paquete" border="true":::

3. Filtrar por estado de flujo de paquetes

   De forma predeterminada, el monitor de paquetes capturará los paquetes que fluyen y los descarte. Para capturar solo los paquetes descartados, seleccione **paquetes descartados**.

   :::image type="content" source="media/filtering-by-packet-flow-status.png" alt-text="Ejemplo de filtrado por estado de flujo de paquetes" border="true":::

   Después, se muestra un resumen de todas las condiciones de filtro seleccionadas para su revisión. Podrá recuperar la vista después de iniciar la captura a través del botón **capturar condiciones** .

   :::image type="content" source="media/filters-review.png" alt-text="Cómo capturar solo paquetes quitados" border="true":::

## <a name="capture-log"></a>Registro de captura

Los resultados se muestran en una tabla que muestra los parámetros principales de los paquetes capturados: marca de tiempo, dirección IP de orígenes, Puerto de origen, dirección IP de destino, Puerto de destino, Ethertype, protocolo, marcas TCP, si el paquete se ha quitado y el motivo de la eliminación.

   - La marca de tiempo de cada uno de estos paquetes también es un hipervínculo que le redirigirá a una página diferente donde puede encontrar más información sobre el paquete seleccionado. Vea la sección página de detalles a continuación.
   - Todos los paquetes descartados tienen un valor "true" en la pestaña **quitada** , un motivo de eliminación y se muestran en texto rojo para que sean más fáciles de identificar.
   - Todas las pestañas se pueden ordenar de forma ascendente y descendente.
   - Puede buscar un valor en cualquier columna del registro mediante la barra de búsqueda.
   - Puede reiniciar la captura con los mismos filtros elegidos mediante el botón **reiniciar** .

   :::image type="content" source="media/capture-log-result.png" alt-text="Ejemplo de tabla de resultados del registro de captura" border="true":::

## <a name="details-page"></a>Details page

En esta página se presenta una instantánea del paquete mientras fluye por cada componente de la pila de red local. Esta vista muestra la ruta de acceso del flujo de paquetes y le permite investigar cómo cambian los paquetes a medida que los procesa cada componente que pasan.

   - Las instantáneas de paquetes se agrupan por cada adaptador/pila de conmutador; es decir, las instantáneas de paquetes capturadas por un adaptador o conmutador, sus controladores de filtro y sus controladores de protocolo se agruparán bajo el nombre del adaptador o conmutador. Esto hará que sea más fácil seguir el flujo del paquete de un adaptador al otro.
   - Cuando se selecciona una instantánea, se muestran más detalles sobre esta instantánea específica, incluidos los encabezados de paquete sin formato.
   - Todos los paquetes descartados tienen un valor "true" en la pestaña **quitada** , un motivo de eliminación y se muestran en texto rojo para que sean más fáciles de identificar.

   :::image type="content" source="media/details-page.png" alt-text="Ejemplo de página de detalles que muestra instantáneas de paquetes" border="true":::

## <a name="display-filters"></a>Mostrar filtros

Los filtros de presentación le permiten filtrar el registro después de capturar los paquetes. Para cada filtro, puede especificar parámetros de paquete como direcciones MAC, direcciones IP, puertos, Ethertype y Protocolo de transporte. A diferencia de los filtros de captura:

   - Los filtros de presentación pueden distinguir entre el origen y el destino de las direcciones IP, las direcciones MAC y los puertos.
   - Los filtros de presentación pueden eliminarse y editarse después de aplicarlos para cambiar la vista del registro.
   - Los filtros de presentación se invierten en los registros guardados.

   :::image type="content" source="media/display-filters.png" alt-text="Pantalla Mostrar filtros" border="true":::

## <a name="save-feature"></a>Guardar característica

El botón Guardar permite guardar el registro en el equipo local, en el equipo remoto o en ambos. Los filtros de presentación se invertirán en el registro guardado.

   - Si el registro se guarda en el equipo local, podrá guardarlo en varios formatos:
      - Formato ETL que se puede analizar mediante Monitor de red de Microsoft. Nota: para obtener más información, consulte esta página.
      - Formato de texto que se puede analizar mediante cualquier editor de texto como TextAnalysisTool.NET.
      - Pcapng fomat, que se puede analizar con herramientas como Wireshark.
         - La mayoría de los metadatos del monitor de paquetes se perderán durante esta conversión. [Consulte esta página](pktmon-pcapng-support.md) para obtener más información.

   :::image type="content" source="media/packet-monitoring-save-feature.png" alt-text="Guardar una copia local de la captura" border="true":::

## <a name="open-feature"></a>Abrir característica

La característica abrir le permitirá volver a abrir cualquiera de los cinco últimos registros guardados para analizar a través de la herramienta.

   :::image type="content" source="media/open.png" alt-text="Abrir un registro reciente" border="true":::