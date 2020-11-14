---
title: Sintaxis y formato del comando Pktmon
description: Use esta página para comprender la sintaxis de pktmon, los comandos, el formato y la salida de las compilaciones de vibranium de Windows.
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 3e4c73af3b00f42301b34e59493f25b6d2ccb95c
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632661"
---
# <a name="pktmon-command-syntax-and-formatting"></a>Sintaxis y formato del comando Pktmon

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El monitor de paquetes (Pktmon) es una herramienta de diagnóstico de red integrada y entre componentes para Windows. Se puede usar para la captura de paquetes, la detección de paquetes, el filtrado de paquetes y el recuento. La herramienta es especialmente útil en escenarios de virtualización, como las redes de contenedor y SDN, ya que proporciona visibilidad dentro de la pila de red. El monitor de paquetes está disponible en la caja a través de pktmon.exe comando en vibranium OS (compilación 19041). Puede usar este tema para aprender a entender la sintaxis de pktmon, los comandos de formato y los resultados.

## <a name="quick-start"></a>Inicio rápido

Siga estos pasos para empezar a trabajar en escenarios genéricos:

1. Identifique el tipo de paquetes necesarios para la captura, es decir, direcciones IP, puertos o protocolos específicos asociados al paquete. A continuación, Compruebe la sintaxis para aplicar los filtros de captura y aplique los filtros para los paquetes identificados en el paso anterior.

   ```PowerShell
   C:\Test> pktmon filter add help
   C:\Test> pktmon filter add <filters>
   ```

2. Inicie la captura y habilite el registro de paquetes.

   ```PowerShell
   C:\Test> pktmon start --etw
   ```

3. Reproduzca el problema que se está diagnosticando. Consultar los contadores para confirmar la presencia del tráfico esperado y obtener una vista de alto nivel del flujo del tráfico en el equipo.

   ```PowerShell
   C:\Test> pktmon counters
   ```

4. Detenga la captura y recupere los registros en formato TXT para su análisis.

   ```PowerShell
   C:\Test> pktmon stop
   C:\Test> pktmon format <etl file>
   ```

Consulte [analizar la salida del monitor de paquetes](#analyze-packet-monitor-output) para obtener instrucciones sobre cómo analizar la salida txt.

## <a name="capture-filters"></a>Filtros de captura

Se recomienda aplicar filtros antes de iniciar cualquier captura de paquetes, ya que la solución de problemas de conectividad con un destino determinado es más fácil cuando se centra en una única secuencia de paquetes. La *captura del tráfico de red* puede hacer que el resultado sea demasiado ruidoso para analizarlo. Para que se notifique un paquete, debe coincidir con todas las condiciones especificadas en al menos un filtro. Se admiten hasta 32 filtros a la vez.

Por ejemplo, el siguiente conjunto de filtros capturará cualquier tráfico ICMP desde o hacia la dirección IP 10.0.0.10 así como cualquier tráfico en el puerto 53.

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t icmp
C:\Test> pktmon filter add -p 53
```

### <a name="filtering-capability"></a>Funcionalidad de filtrado

- El monitor de paquetes admite el filtrado por direcciones MAC, direcciones IP, puertos, EtherType, protocolo de transporte e identificador de VLAN. 

- El monitor de paquetes no distinguirá entre el origen o el destino cuando llegue a la dirección MAC, la dirección IP o los filtros de puerto. 

- Para filtrar aún más los paquetes TCP, se puede proporcionar una lista opcional de marcas TCP para la coincidencia. Las marcas admitidas son FIN, SYN, RST, PSH, ACK, URG, ECE y CWR.

  - Por ejemplo, el siguiente filtro capturará todos los paquetes SYN enviados o recibidos por la dirección IP 10.0.0.10:

```PowerShell
C:\Test> pktmon filter add -i 10.0.0.10 -t tcp syn
```

- El monitor de paquetes puede aplicar un filtro a los paquetes internos encapsulados, además del paquete externo si la marca [-e] se agregó a cualquier filtro. Los métodos de encapsulación admitidos son VXLAN, GRE, NVGRE y IP-in-IP. El puerto VXLAN personalizado es opcional y su valor predeterminado es 4789.

### <a name="pktmon-filters-syntax"></a>Sintaxis de los filtros de Pktmon

```PowerShell
C:\Test> pktmon filter help
pktmon filter { list | add | remove } [OPTIONS | help]

Commands
    list      Display active packet filters.
    add       Add a filter to control which packets are reported.
    remove    Removes all filters.

help
    Show help text for a command.

C:\Test> pktmon filter add help
pktmon filter add <name> [-m mac [mac2]] [-v vlan] [-d { IPv4 | IPv6 | number }]
                         [-t { TCP [flags...] | UDP | ICMP | ICMPv6 | number }]
                         [-i ip [ip2]] [-p port [port2]] [-e [port]]
    Add a filter to control which packets are reported. For a packet to be
    reported, it must match all conditions specified in at least one filter.
    Up to 8 filters can be active at once.

    NOTE1: When two MACs (-m), IPs (-i), or ports (-p) are specified, the filter
           matches packets that contain both. It will not distinguish between source
           or destination for this purpose.

name
    Optional name or description of the filter.

Ethernet frame
    -m, --mac[-address]
        Match source or destination MAC address. See NOTE1 above.

    -v, --vlan
        Match by VLAN Id (VID) in the 802.1Q header.

    -d, --data-link[-protocol], --ethertype
        Match by data link (layer 2) protocol. Can be IPv4, IPv6, ARP, or
        a protocol number.

IP header
    -t, --transport[-protocol], --ip-protocol
        Match by transport (layer 4) protocol. Can be TCP, UDP, ICMP, ICMPv6, or
        a protocol number.
        To further filter TCP packets, an optional list of TCP flags to match can
        be provided. Supported flags are FIN, SYN, RST, PSH, ACK, URG, ECE, and CWR.

    -i, --ip[-address]
        Match source or destination IP address. See NOTE1 above.
        To match by subnet, use CIDR notation with the prefix length.

TCP/UDP header
    -p, --port
        Match source or destination port number. See NOTE1 above.

Encapsulation
    -e, --encap
        This filter also applies to encapsulated inner packets, in addition to the outer
        packet. Supported encapsulation methods are VXLAN, GRE, NVGRE, and IP-in-IP.
        Custom VXLAN port is optional, and defaults to 4789.

Example 1: Ping filter
        pktmon filter add MyPing -i 10.10.10.10 -t ICMP

Example 2: TCP SYN filter for SMB traffic
    pktmon filter add MySmbSyn -i 10.10.10.10 -t TCP SYN -p 445

Example 3: Subnet filter
    pktmon filter add MySubnet -i 10.10.10.0/24
```

## <a name="packet-capture-and-logging"></a>Captura de paquetes y registro

El monitor de paquetes puede capturar el tráfico de red con o sin registro de paquetes. Para obtener más información acerca de la captura del tráfico sin el registro de paquetes, consulte la sección contadores de paquetes a continuación. Para capturar y registrar paquetes, agregue el parámetro **[--ETW]** al comando START.

Seleccione los componentes que se van a supervisar mediante el parámetro **[-c]** . Puede ser todos los componentes, solo NIC o una lista de identificadores de componente. El valor predeterminado es capturar el tráfico en todos los componentes. Supervise los paquetes descartados solo con el parámetro [-d]. El valor predeterminado es capturar paquetes de flujo y descartados.

Por ejemplo, el siguiente comando capturará los contadores de paquetes de todos los adaptadores de red sin registrar los paquetes:

```PowerShell
C:\Test> pktmon start -c nics
```

Sin embargo, el siguiente comando capturará solo los paquetes descartados que pasan a través de los componentes 4 y 5 y los registra:

```PowerShell
C:\Test> pktmon start --etw -c 4,5 -d
```

### <a name="packet-logging-capability"></a>Capacidad de registro de paquetes

- El monitor de paquetes admite varios modos de registro:

  - Circular: los nuevos paquetes sobrescriben los más antiguos cuando se alcanza el tamaño máximo de archivo. Este es el modo de registro predeterminado.
  - Varios archivos: se crea un nuevo archivo de registro cuando se alcanza el tamaño máximo de archivo. Los archivos de registro se numeran secuencialmente: PktMon1. ETL, PktMon2. ETL, etc. Aplique este modo de registro para mantener todo el registro, pero tenga cuidado con el uso del almacenamiento. Nota: Use la marca de tiempo de creación de archivos de cada archivo de registro como una indicación de un intervalo de tiempo específico en la captura.
  - En tiempo real: los paquetes se muestran en la pantalla en tiempo real. No se crea ningún archivo de registro. Use Ctrl + C para detener la supervisión.
  - Memoria: los eventos se escriben en un búfer de memoria circular. El tamaño del búfer se especifica mediante el parámetro **[-s]** . El contenido del búfer se escribe en un archivo de registro después de detener la captura. Use este modo de registro para escenarios muy ruidosos para capturar una cantidad enorme de tráfico en un período de tiempo muy corto en el búfer de memoria. Con cualquier otro modo de registro, es posible que se pierda parte del tráfico.

- Especifique la cantidad del paquete que se va a registrar a través del parámetro **[-p]** . Registre el paquete completo de todos los paquetes con independencia de su tamaño; para ello, establezca ese parámetro en 0. El valor predeterminado es 128 bytes, que deben incluir los encabezados de la mayoría de los paquetes.

- Especifique el tamaño del archivo de registro mediante el parámetro **[-s]** . Este será el tamaño máximo del archivo en el modo de registro circular antes de que el monitor de paquetes empiece a sobrescribir los paquetes más antiguos con los más recientes. También será el tamaño máximo de cada archivo en el modo de registro de varios archivos antes de que el monitor de paquetes cree un nuevo archivo para registrar los siguientes paquetes. También puede usar este parámetro para establecer el tamaño de búfer para el modo de registro de memoria.

### <a name="pktmon-start-syntax"></a>Sintaxis de inicio de Pktmon

```PowerShell
C:\Test> pktmon start help
pktmon start [-c { all | nics | [ids...] }] [-d] [--etw [-p size] [-k keywords]]
             [-f] [-s] [--log-mode {circular | multi-file | real-time | memory}]
    Start packet monitoring.

-c, --components
    Select components to monitor. Can be all components, NICs only, or a
    list of component ids. Defaults to all.

-d, --drop-only
    Only report dropped packets. By default, successful packet propagation
    is reported as well.

ETW Logging
    --etw
        Start a logging session for packet capture.

    -p, --packet-size
        Number of bytes to log from each packet. To always log the entire
        packet, set this to 0. Default is 128 bytes.

    -k, --keywords
        Hexadecimal bitmask (i.e. sum of the below flags) that controls
        which events are logged. Default is 0x012.

        Flags:
        0x001 - Internal Packet Monitor errors.
        0x002 - Information about components, counters and filters.
                This information is added to the end of the log file.
        0x004 - Source and destination information for the first
                packet in NET_BUFFER_LIST group.
        0x008 - Select packet metadata from NDIS_NET_BUFFER_LIST_INFO
                enumeration.
        0x010 - Raw packet, truncated to the size specified in
                [--packet-size] parameter.

    -f, --file-name
        .etl log file. Default is PktMon.etl.

    -s, --file-size
        Maximum log file size in megabytes. Default is 512 MB.

    -l, --log-mode
        Select logging mode. Default is circular.

        circular
            New events overwrite the oldest ones when
            when the maximum file size is reached.

        multi-file
            A new log file is created when the maximum file size is reached.
            Log files are sequentially numbered. PktMon1.etl, PktMon2.etl, etc.

        real-time
            Display events and packets on screen at real time. No log file is created.
            Press Ctrl+C to stop monitoring.

        memory
            Events are written to a circular memory buffer.
            Buffer size is specified in [--file-size] parameter.
            Buffer contents is written to a log file during stop operation.

Example: pktmon start --etw -l real-time
```

## <a name="packet-analysis-and-formatting"></a>Análisis y formato de paquetes

El monitor de paquetes genera archivos de registro en formato ETL. Hay varias maneras de dar formato al archivo ETL para su análisis:

- Convierta el registro en formato de texto (opción predeterminada) y analicelo con la herramienta editor de texto como TextAnalysisTool.NET. Los datos del paquete se mostrarán en formato TCPDump. Siga la guía siguiente para obtener información sobre cómo analizar la salida en el archivo de texto.
- Convertir el registro en formato pcapng para analizarlo mediante [Wireshark](pktmon-pcapng-support.md)*
- Abra el archivo ETL con [monitor de red](pktmon-netmon-support.md)*

>[!NOTE]
>* Use los hipervínculos anteriores para obtener información sobre cómo analizar y analizar los registros de monitor de paquetes en **Wireshark** y **monitor de red**.

### <a name="pktmon-format-syntax"></a>Sintaxis de formato Pktmon

```PowerShell
C:\Test> pktmon format help
pktmon format log.etl [-o log.txt] [-b] [-v [level]] [-x] [-e] [-l [port]
    Convert log file to text format.

-o, --out
    Name of the formatted text file.

-s, --stats-only
    Display log file statistical information.

Network packet formatting options

    -b, --brief
        Abbreviated packet format.

    -v, --verbose
        Verbosity level [1..3].

    -x, --hex
        Hexadecimal format.

    -e, --no-ethernet
        Don't print ethernet header.

    -l, --vxlan
        Custom VXLAN port.


Example: pktmon format C:\tmp\PktMon.etl

C:\Test> pktmon pcapng help
pktmon pcapng log.etl [-o log.pcapng]
    Convert log file to pcapng format.
    Dropped packets are not included by default.

-o, --out
    Name of the formatted pcapng file.

-d, --drop-only
    Convert dropped packets only.

-c, --component-id
    Filter packets by a specific component ID.

Example: pktmon pcapng C:\tmp\PktMon.etl -d -c nics
```

### <a name="analyze-packet-monitor-output"></a>Analizar la salida del monitor de paquetes

El monitor de paquetes captura una instantánea del paquete por cada componente de la pila de red. En consecuencia, habrá varias instantáneas de cada paquete (representado en la imagen siguiente por las líneas del cuadro azul).
Cada una de estas instantáneas de paquetes se representa mediante un par de líneas (cuadros rojo y verde). Hay al menos una línea que incluye algunos datos sobre la instancia de paquete a partir de la marca de tiempo. Justo después de, hay al menos una línea (en negrita en la imagen siguiente) para mostrar el paquete sin formato analizado en formato de texto (sin una marca de tiempo); podría tratarse de varias líneas si el paquete está encapsulado, como el paquete en el cuadro verde.

<center>

:::image type="content" source="media/pktmon-log-example.png" alt-text="Ejemplo de salida del registro txt del monitor de paquetes" border="false":::

</center>

Para correlacionar todas las instantáneas de los mismos paquetes, supervise los valores de PktGroupId y PktNumber (resaltados en amarillo); todas las instantáneas del mismo paquete deben tener estos 2 valores en común. El valor de apariencia (resaltado en azul) actúa como un contador para cada instantánea subsiguiente del mismo paquete. Por ejemplo, la primera instantánea del paquete (donde el paquete apareció por primera vez en la pila de red) tiene el valor 1 para la apariencia, la siguiente instantánea tiene el valor 2, y así sucesivamente.

Cada instantánea de paquete tiene un identificador de componente (subrayado en la imagen anterior) que denota el componente asociado a la instantánea. Para resolver el nombre del componente y los parámetros, busque este identificador de componente en la lista de componentes en la parte inferior del archivo de registro. En la imagen siguiente se muestra una parte de la tabla de componentes en el resaltado "componente 1" en amarillo (este era el componente en el que se capturó la última instantánea anterior).
Los componentes con 2 bordes notificarán dos instantáneas en cada borde (como las instantáneas con la apariencia 3 y la apariencia 4, por ejemplo, en la imagen anterior).

En la parte inferior de cada archivo de registro, la lista filtros se presenta como se muestra en la imagen siguiente (resaltada en azul). Cada filtro muestra los parámetros especificados (protocolo ICMP en el ejemplo siguiente) y ceros para el resto de los parámetros.

<center>

:::image type="content" source="media/pktmon-log-example-components.png" alt-text="Ejemplo de componentes de salida de registro txt del monitor de paquetes":::

</center>

En el caso de los paquetes descartados, la palabra "quitar" aparece antes que cualquiera de las líneas que representan la instantánea en la que se quitó el paquete. Cada paquete quitado también proporciona un valor de dropReason. Este parámetro dropReason proporciona una breve descripción del motivo de la eliminación de paquetes; por ejemplo, la diferencia de MTU, la VLAN filtrada, etc.

<center>

:::image type="content" source="media/dropped-packet-log-example.png" alt-text="Ejemplo de un registro de paquetes quitado":::

</center>

## <a name="packet-counters"></a>Contadores de paquetes

Los contadores del monitor de paquetes proporcionan una vista de alto nivel del tráfico de red a través de la pila de red sin necesidad de analizar un registro, lo que puede ser un proceso costoso. Examine los patrones de tráfico consultando los contadores de paquetes con los **contadores de pktmon** después de iniciar la captura del monitor de paquetes. Restablecer los contadores a cero mediante **pktmon restablecer** o detener la supervisión de forma conjunta mediante **pktmon STOP**.

- Los contadores se organizan por pilas de enlace con adaptadores de red en la parte superior y los protocolos en la parte inferior.
- TX/RX: los contadores se dividen en dos columnas para las direcciones Send (TX) y Receive (RX).  
- Bordes: los componentes informan sobre la propagación de paquetes cuando un paquete cruza el límite de componente (perimetral). Cada componente puede tener uno o más bordes. Los controladores de minipuerto suelen tener un solo borde superior, los protocolos tienen un solo borde inferior y los controladores de filtro tienen bordes superiores e inferiores.  
- Caídas: los contadores de eliminación de paquetes se están inscritos en la misma tabla.

En el ejemplo siguiente, se ha iniciado una nueva captura y, a continuación, se ha usado el comando **pktmon counters** para consultar los contadores antes de que se detuviera la captura. Los contadores muestran un único paquete que lo convierte fuera de la pila de red, comenzando por la capa del protocolo hasta el adaptador de red físico y su respuesta regresa en la otra dirección. Si falta el comando ping o la respuesta, es fácil detectar esto a través de los contadores.

<center>

:::image type="content" source="media/pktmon-counters-with-perfect-flow.png" alt-text="Ejemplo de un contador de paquetes con un flujo perfecto" border="false":::

</center>

En el ejemplo siguiente, se muestran las caídas en la columna "Counter". Recupere el último motivo de la eliminación de cada componente solicitando los datos de los contadores en formato JSON mediante los **contadores de pktmon--JSON** o analice el registro de salida para obtener información más detallada.

<center>

:::image type="content" source="media/pktmon-counters-drop-example.png" alt-text="Ejemplo de un contador de paquetes con un paquete descartado" border="false":::

</center>

Tal y como se muestra en estos ejemplos, los contadores pueden proporcionar una gran cantidad de información a través de un diagrama que se puede analizar con solo una vista rápida.

### <a name="pktmon-counters-syntax"></a>Sintaxis de los contadores Pktmon

```PowerShell
C:\Test> pktmon counters help
pktmon [comp] counters [-t { all | drop | flow }] [-z] [--json]
    Display current per-component counters.

-t, --counter-type
    Select which types of counters to show.
    Supported values are all counters (default), drops only, or flows only.

-z, --show-zeros
    Show counters that are zero in both directions.

-i, --show-hidden
    Show components that are hidden by default.

--json
    Output the counters in JSON format.
```

## <a name="networking-stack-layout"></a>Diseño de la pila de red

Examine el diseño de la pila de red a través de la lista de comandos **pktmon**.

:::image type="content" source="media/pktmon-networking-stack-example.png" alt-text="Ejemplo del diseño de la pila de red" border="false":::

El comando muestra los componentes de red (controladores) organizados por enlaces de adaptadores.

Un enlace típico consta de:

- Una sola tarjeta de interfaz de red (NIC)
- Algunos controladores de filtro (posiblemente cero)
- Uno o varios controladores de protocolo (TCP/IP u otros)

Cada componente se identifica de forma única mediante un identificador de componente del monitor de paquetes, que se usa para dirigirse a componentes individuales para la supervisión.

>[!NOTE]
>Los identificadores no son persistentes y pueden cambiar entre los reinicios y el reinicio del controlador del monitor de paquetes.

### <a name="pktmon-list-syntax"></a>Sintaxis de la lista Pktmon

```powershell
C:\Test> pktmon list help
pktmon [comp] list
    List all active components.

-i, --show-hidden
    Show components that are hidden by default.

--json
    Output the list in JSON format.
```