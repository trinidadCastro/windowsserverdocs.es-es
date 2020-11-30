---
title: Supervisión de paquetes (Pktmon)
description: En este tema se proporciona información general sobre la herramienta de diagnóstico de red del monitor de paquetes (Pktmon).
ms.topic: overview
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: cab9de79c1d53b505acb61020c71472365ded348
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330497"
---
# <a name="packet-monitor-pktmon"></a>Monitor de paquetes \( Pktmon\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El monitor de paquetes (Pktmon) es una herramienta de diagnóstico de red integrada y entre componentes para Windows. Se puede usar para la captura de paquetes, la detección de paquetes, el filtrado de paquetes y el recuento. La herramienta es especialmente útil en escenarios de virtualización, como las redes de contenedor y SDN, ya que proporciona visibilidad dentro de la pila de red. Está disponible en la caja mediante el comando pktmon.exe y a través de las extensiones del centro de administración de Windows. 

## <a name="overview"></a>Información general

Cualquier equipo que se comunique a través de la red tiene al menos un adaptador de red. Todos los componentes entre este adaptador y una aplicación forman una pila de red: un conjunto de componentes de red que procesan y mueven el tráfico de red. En los escenarios tradicionales, la pila de red es pequeña y todos los enrutamiento y conmutación de paquetes se producen en dispositivos externos.

<center>

:::image type="content" source="media/networking-stack.png" alt-text="Pila de red en escenarios tradicionales" border="false":::

</center>

Sin embargo, con la llegada de la virtualización de red, el tamaño de la pila de red se ha multiplicado. Esta pila de redes extendidas ahora incluye componentes como el conmutador virtual que controla el procesamiento y el cambio de paquetes. Este entorno flexible permite el uso de recursos y el aislamiento de seguridad mucho mejores, pero también deja más espacio para los errores de configuración que pueden ser difíciles de diagnosticar. El monitor de paquetes proporciona una visibilidad mejorada dentro de la pila de red que se suele necesitar para identificar estos errores.

<center>

:::image type="content" source="media/packet-capture.png" alt-text="Captura de paquetes entre componentes de PacketMon" border="false":::

</center>

El monitor de paquetes intercepta los paquetes en varias ubicaciones de la pila de red, lo que expone la ruta de paquetes. Si un componente compatible ha quitado un paquete de la pila de red, el monitor de paquetes informará de la eliminación del paquete. Esto permite a los usuarios diferenciar entre un componente que es el destino previsto para un paquete y un componente que interfiere con un paquete. Además, el monitor de paquetes informará de los motivos de eliminación; por ejemplo, MTU Mistmatch, o VLAN filtrada, etc. Estas razones de eliminación proporcionan la causa principal del problema sin necesidad de agotar todas las posibilidades. El monitor de paquetes también proporciona contadores de paquetes para cada punto de interceptación, lo que permite un examen de flujo de paquetes de alto nivel sin necesidad de análisis de registros que consumen mucho tiempo.

<center>

:::image type="content" source="media/drop-detection.png" alt-text="Detección de eliminación de PacketMon" border="false":::

</center>

## <a name="best-practices"></a>Prácticas recomendadas

Siga estas prácticas recomendadas para optimizar el análisis de la red.

- Consulte la ayuda de la línea de comandos para obtener argumentos y capacidades (por ejemplo, pktmon Start Help).
- Configurar filtros de paquetes que coincidan con el escenario (pktmon filtro Add).
- Compruebe los contadores de paquetes durante el experimento para ver la vista de alto nivel (contadores de pktmon).
- Busque en el registro el análisis detallado (pktmon Format pktmon. ETL).

## <a name="functionality"></a>Funcionalidad

La funcionalidad del monitor de paquetes ha evolucionado a través de las versiones de Windows. En la tabla siguiente se muestran sus principales capacidades y su versión de Windows correspondiente.

| Capacidad                                                                  | V 1809 (B:17763) | V 1903 (B:18362) | V 2004 (B:19041) |
|:---------------------------------------------------------------------------:|:----------------:|:----------------:|:----------------:|
| Supervisión y recuento de paquetes en varias ubicaciones a través de la pila de red | &#x2611;         | &#x2611;         | &#x2611;         |
| Detección de paquetes descartados en varias ubicaciones de la pila                          | &#x2611;         | &#x2611;         | &#x2611;         |
| Filtrado flexible de paquetes en tiempo de ejecución                                           | &#x2611;         | &#x2611;         | &#x2611;         |
| Compatibilidad con la encapsulación                                                       | &#x2610;         | &#x2611;         | &#x2611;         |
| Análisis de red basado en el análisis de paquetes TcpDump                            | &#x2610;         | &#x2611;         | &#x2611;         |
| Análisis de metadatos de paquetes (OOB)                                              | &#x2610;         | &#x2610;         | &#x2611;         |
| Supervisión de paquetes en pantalla en tiempo real                                       | &#x2610;         | &#x2610;         | &#x2611;         |
| Registro de gran volumen en memoria                                               | &#x2610;         | &#x2610;         | &#x2611;         |
| Compatibilidad con los formatos Wireshark y Monitor de red                                | &#x2610;         | &#x2610;         | &#x2611;         |

>[!NOTE]
>Azure Stack los clientes de HCl y Azure Stack Hub deben esperar la funcionalidad de vibranium.

## <a name="limitations"></a>Limitaciones

A continuación se muestran algunas de las limitaciones más significativas del monitor de paquetes.

- Ahora solo se admite el tráfico Ethernet. La compatibilidad con el tráfico inalámbrico se agregará en versiones posteriores.
- Las caídas de paquetes del firewall de Windows no son visibles en el monitor de paquetes todavía. 

## <a name="get-started-with-packet-monitor"></a>Introducción al monitor de paquetes

Los siguientes recursos están disponibles para ayudarle a empezar a usar el monitor de paquetes.

### <a name="pktmon-command-syntax-and-formatting"></a>[Sintaxis y formato del comando Pktmon](pktmon-syntax.md)

El monitor de paquetes está disponible en la caja a través de pktmon.exe comando en vibranium OS (compilación 19041). Puede [usar este tema](pktmon-syntax.md) para aprender a entender la sintaxis, los comandos, el formato y la salida de pktmon.

### <a name="packet-monitoring-extension-in-windows-admin-center"></a>[Extensión de la supervisión de paquetes en Windows Admin Center](pktmon-wac-extension.md)

La extensión de supervisión de paquetes le permite trabajar y consumir el monitor de paquetes a través del centro de administración de Windows. La extensión le ayuda a diagnosticar la red mediante la captura y visualización del tráfico de red a través de la pila de red en un registro que es fácil de seguir y manipular. Puede [usar este tema](pktmon-wac-extension.md) para obtener información sobre cómo usar la herramienta y entender su resultado.

### <a name="sdn-data-path-diagnostics-extension-in-windows-admin-center"></a>[Extensión de diagnósticos de ruta de acceso de datos SDN en el centro de administración de Windows](pktmon-sdn-data-path-wac-extension.md)

El diagnóstico de rutas de acceso de datos de SDN es una herramienta dentro de la extensión de supervisión de SDN del centro de administración de Windows. La herramienta automatiza las capturas de paquetes basadas en el monitor de paquetes según los distintos escenarios de SDN y presenta el resultado en una sola vista que es fácil de seguir y manipular. Puede [usar este tema](pktmon-sdn-data-path-wac-extension.md) para obtener información sobre cómo usar la herramienta y entender su resultado.

### <a name="microsoft-network-monitor-netmon-support"></a>[Soporte técnico del Monitor de red de Microsoft (Netmon)](pktmon-netmon-support.md)

El monitor de paquetes genera registros en formato ETL. Estos registros se pueden analizar mediante el Monitor de red de Microsoft (Netmon) mediante analizadores especiales. En [este tema](pktmon-netmon-support.md) se explica cómo analizar archivos ETL generados por el monitor de paquetes en Netmon.

### <a name="wireshark-pcapng-format-support"></a>[Soporte técnico de Wireshark (formato pcapng)](pktmon-pcapng-support.md)

El monitor de paquetes puede convertir registros al formato pcapng. Estos registros se pueden analizar mediante Wireshark (o cualquier analizador de pcapng). En [este tema](pktmon-pcapng-support.md) se explican los resultados esperados y cómo aprovecharlos.

## <a name="provide-feedback-to-engineering-team"></a>Proporcionar comentarios al equipo de ingeniería

Informe de los errores o envíe sus comentarios a través del centro de comentarios mediante los pasos siguientes:

1. Inicie el  **centro de comentarios**   a través del menú **Inicio** .

1. Seleccione el botón **notificar un problema** o el botón **sugerir una característica** .

1. Proporcione un título de comentario significativo en **resumir la caja del problema**   .

1. Proporcione los detalles y los pasos para reproducir el problema en el cuadro **proporcione más detalles**   .

1. Seleccione  **red e Internet**   como la categoría superior y, después, **Monitor de paquetes (pktmon.exe)** como subcategoría.

1. Para ayudarnos a identificar y corregir el error más rápido, Capture capturas de pantallas, adjunte el registro de salida de pktmon o vuelva a crear el problema.

1. Haga clic en **Enviar**.

Después de enviar los comentarios y los errores, el equipo de ingeniería podrá echar un vistazo a los comentarios y solucionarlos.
