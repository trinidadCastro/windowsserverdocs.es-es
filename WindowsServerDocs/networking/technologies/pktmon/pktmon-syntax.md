---
title: Formato del comando Pktmon
description: Use esta página para comprender el formato y la salida del comando pktmon.
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 1/20/2021
ms.openlocfilehash: f0ad3d743e41a63a53d8bba2c484f7ed32e4a5d5
ms.sourcegitcommit: 58a13a29869b39b6a6ba0db4d7b9fe1ff8371ea7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98625808"
---
# <a name="pktmon-command-formatting"></a>Formato del comando Pktmon

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El monitor de paquetes (Pktmon) es una herramienta de diagnóstico de red integrada y entre componentes para Windows. Se puede usar para la captura de paquetes, la detección de paquetes, el filtrado de paquetes y el recuento. La herramienta es especialmente útil en escenarios de virtualización, como las redes de contenedor y SDN, ya que proporciona visibilidad dentro de la pila de red. El monitor de paquetes está disponible en la caja a través de pktmon.exe comando en vibranium OS (compilación 19041). Puede usar este tema para aprender a entender la sintaxis de pktmon, el formato de comandos y la salida. Para obtener una lista completa de comandos, consulte [Sintaxis de pktmon](/windows-server/administration/windows-commands/pktmon). 

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

Para obtener más información, consulte [Sintaxis de filtro pktmon](/windows-server/administration/windows-commands/pktmon-filter).

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

Para obtener más información, vea [pktmon Start Syntax](/windows-server/administration/windows-commands/pktmon-start).

## <a name="packet-analysis-and-formatting"></a>Análisis y formato de paquetes

El monitor de paquetes genera archivos de registro en formato ETL. Hay varias maneras de dar formato al archivo ETL para su análisis:

- Convierta el registro en formato de texto (opción predeterminada) y analicelo con la herramienta editor de texto como TextAnalysisTool.NET. Los datos del paquete se mostrarán en formato TCPDump. Siga la guía siguiente para obtener información sobre cómo analizar la salida en el archivo de texto.
- Convertir el registro en formato pcapng para analizarlo mediante [Wireshark](pktmon-pcapng-support.md)*
- Abra el archivo ETL con [monitor de red](pktmon-netmon-support.md)*

>[!NOTE]
>* Use los hipervínculos anteriores para obtener información sobre cómo analizar y analizar los registros de monitor de paquetes en **Wireshark** y **monitor de red**.

Para obtener más información, vea [Sintaxis de formato pktmon](/windows-server/administration/windows-commands/pktmon-format).

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

Para obtener más información, vea [pktmon counters Syntax](/windows-server/administration/windows-commands/pktmon-counters).

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

Para obtener más información, vea sintaxis de la [lista pktmon](/windows-server/administration/windows-commands/pktmon-list).
