---
title: Extensión de diagnósticos de ruta de acceso de datos SDN en el centro de administración de Windows
description: Use este tema para automatizar las capturas de paquetes basadas en el monitor de paquetes con la extensión de diagnóstico de rutas de acceso de datos de SDN en el centro de administración de Windows.
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 9b1a247e0d07a4e44ba7640aa2e95180956ccee8
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632649"
---
# <a name="sdn-data-path-diagnostics-extension-in-windows-admin-center"></a>Extensión de diagnósticos de ruta de acceso de datos SDN en el centro de administración de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El diagnóstico de rutas de acceso de datos de SDN es una herramienta dentro de la extensión de supervisión de SDN del centro de administración de Windows que automatiza las capturas de paquetes basadas en el monitor de paquetes según los distintos escenarios de SDN y presenta el resultado en una sola vista que es fácil de seguir y manipular.

## <a name="what-is-packet-monitor-pktmon"></a>¿Qué es el monitor de paquetes (Pktmon)?
El monitor de paquetes (Pktmon) es una herramienta de diagnóstico de red integrada y entre componentes para Windows. Se puede usar para la captura de paquetes, la detección de paquetes, el filtrado de paquetes y el recuento. La herramienta es especialmente útil en escenarios de virtualización, como las redes de contenedor y SDN, ya que proporciona visibilidad dentro de la pila de red.

## <a name="what-is-windows-admin-center"></a>¿Qué es Windows Admin Center?
El centro de administración de Windows es una herramienta de administración basada en explorador y implementada localmente que le permite administrar los servidores de Windows sin depender de Azure o de la nube. Windows Admin Center ofrece el control total de todos los aspectos de tu infraestructura de servidores y es especialmente útil para la administración de servidores en redes privadas que no están conectadas a Internet. Windows Admin Center es la evolución moderna de herramientas de administración como el Administrador de servidores y MMC.

## <a name="before-you-start"></a>Antes de empezar
- Para usar la herramienta, el servidor de destino debe ejecutar Windows Server 2019 versión 1903 (19H1) y versiones posteriores.
- [Instale el centro de administración de Windows](/windows-server/manage/windows-admin-center/deploy/install).
- Agregar un clúster al centro de administración de Windows:
  1. Haga clic en **+ Agregar** en **Todas las conexiones**.
  2. Elija Agregar una conexión de clúster de Hyper-Converged.
  3. Escriba el nombre del clúster y, si se le solicita, las credenciales que se usarán.
  4. Active **la casilla configurar la controladora de red** para continuar.
  5. Escriba el URI de la controladora de red y haga clic en **validar**.
  6. Haga clic en **Agregar** para finalizar.

El clúster se agregará a la lista de conexiones. Haga clic en él para iniciar el panel.

<center>

:::image type="content" source="media/add-sdn-enabled-hci-connection.png" alt-text="Agregar una conexión HCI habilitada para SDN con el centro de administración de Windows" border="true":::

</center>

## <a name="getting-started"></a>Introducción

Para ir a la herramienta, navegue hasta el clúster que creó en el paso anterior, a continuación, a la extensión "SDN Monitoring" y, a continuación, a la pestaña "Data path Diagnostics".

## <a name="selecting-scenarios"></a>Selección de escenarios

En la primera página se enumeran todos los escenarios de SDN clasificados como escenarios de carga de trabajo y escenarios de infraestructura, tal como se muestra en la imagen siguiente. Para empezar, seleccione el escenario de SDN que debe diagnosticarse.

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-main-page.png" alt-text="Supervisión de SDN: Página de escenarios de diagnóstico" border="true":::

</center>

## <a name="scenario-parameters"></a>Parámetros de escenario

Después de elegir el escenario, rellene una lista de parámetros obligatorios y opcionales para iniciar la captura. Estos parámetros básicos apuntarán a la herramienta a la conexión que se debe diagnosticar. A continuación, la herramienta usará estos parámetros para que las consultas ejecuten una captura correcta, sin que intervenga el usuario para averiguar el flujo de paquetes esperado, las máquinas implicadas en el escenario, su ubicación en el clúster o los filtros de captura que se van a aplicar en cada máquina. Los parámetros obligatorios permiten que se ejecute la captura y los parámetros opcionales ayudan a filtrar algún ruido.

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-scenario-parameters.png" alt-text="Supervisión de SDN: Página de condiciones de captura" border="true":::

</center>

## <a name="capture-log"></a>Registro de captura

Después de iniciar la captura, la extensión mostrará una lista de los equipos en los que se inicia la captura. Es posible que se le pida que inicie sesión en estas máquinas si sus credenciales no se guardaron. Puede empezar a reproducir el ping o el problema que está intentando diagnosticar capturando los paquetes relativos. Una vez capturados los paquetes, la extensión mostrará marcas junto a las máquinas en las que se capturaron los paquetes.

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-loading-wheel2.png" alt-text="Inicio de la captura de paquetes" border="true":::

</center>

Después de detener la captura, los registros de todas las máquinas se mostrarán en una sola página, dividida por el título de la máquina. Cada título incluirá el nombre del equipo, su rol en el escenario y su host en el caso de las máquinas virtuales (VM).

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-log.png" alt-text="Registro de diagnóstico de ruta de datos después de detener la captura" border="true":::

</center>

Los resultados se muestran en una tabla que muestra los parámetros principales de los paquetes capturados: marca de tiempo, dirección IP de orígenes, Puerto de origen, dirección IP de destino, Puerto de destino, Ethertype, protocolo, marcas TCP, si el paquete se ha quitado y el motivo de la eliminación.

   - La marca de tiempo de cada uno de estos paquetes también es un hipervínculo que le redirigirá a una página diferente donde puede encontrar más información sobre el paquete seleccionado. Vea la sección página de detalles a continuación.
   - Todos los paquetes descartados tienen un valor "true" en la pestaña **quitada** , un motivo de eliminación y se muestra en texto rojo para que sea más fácil de identificar.
   - Todas las pestañas se pueden ordenar de forma ascendente y descendente.
   - Puede buscar un valor en cualquier columna del registro mediante la barra de búsqueda.
   - Puede reiniciar la captura con los mismos filtros elegidos mediante el botón **reiniciar** .

## <a name="details-page"></a>Details page

La información de esta página es especialmente valiosa si tiene problemas incorrectos de propagación de paquetes o problemas de configuración incorrecta, ya que puede investigar el flujo del paquete a través de cada componente de la pila de red. Para cada salto de paquete, hay detalles que incluyen los parámetros de paquete, así como los detalles del paquete sin formato.

- Los saltos se agrupan en función de los componentes implicados. Cada adaptador y los controladores que se encuentran en la parte superior se agrupan por el nombre del adaptador. Esto facilita el seguimiento del paquete en un nivel alto a través de estos títulos de grupo.
- Todos los paquetes descartados también se mostrarán en texto rojo para que sean más fáciles de identificar.

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-details-page.png" alt-text="Página de detalles de diagnóstico de ruta de datos" border="true":::

</center>

Seleccione un salto para ver más detalles. En escenarios de encapsulación y NAT (traducción de direcciones de red), esta característica le permite ver el cambio de paquetes a medida que pasa por la pila de red y comprobar si hay algún problema de configuración de errores.

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-details-page-with-pane1.png" alt-text="Ver detalles sobre un salto específico" border="true":::

</center>

Desplácese hacia abajo para ver los detalles del paquete sin formato:

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-details-page-with-pane-raw-packet1.png" alt-text="Ver detalles de paquetes sin formato sobre un salto específico" border="true":::

</center>

## <a name="display-filters"></a>Mostrar filtros

Los filtros de presentación le permiten filtrar el registro después de capturar los paquetes. Para cada filtro, puede especificar parámetros de paquete como direcciones MAC, direcciones IP, puertos, Ethertype y Protocolo de transporte.

   - Los filtros de presentación pueden distinguir entre el origen y el destino de las direcciones IP, las direcciones MAC y los puertos.
   - Los filtros de presentación pueden eliminarse y editarse después de aplicarlos para cambiar la vista del registro.
   - Los filtros de presentación se invierten en los registros guardados.

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-display-filters.png" alt-text="filtrar registros con filtros de presentación" border="true":::

</center>

## <a name="save"></a>Guardar

El botón Guardar permite guardar el registro en el equipo local para su posterior análisis a través de otras herramientas. Los filtros de presentación se invertirán en el registro guardado. Los registros se pueden guardar en varios formatos:

   - Formato ETL que se puede analizar mediante Monitor de red de Microsoft. Nota: para obtener más información, [consulte esta página](pktmon-netmon-support.md) .
   - Formato de texto que se puede analizar mediante cualquier editor de texto como TextAnalysisTool.NET.
   - Pcapng fomat, que se puede analizar con herramientas como Wireshark.
      - La mayoría de los metadatos del monitor de paquetes se perderán durante esta conversión. Nota: para obtener más información, [consulte esta página](pktmon-pcapng-support.md) .

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-save.png" alt-text="guardar registros localmente" border="true":::

</center>
