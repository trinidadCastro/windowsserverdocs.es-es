---
title: Ajustar el rendimiento de los adaptadores de red
description: Este tema forma parte de la guía de optimización del rendimiento del subsistema de red para Windows Server 2016.
audience: Admin - CI ID 111485 - CSSTroubleshoot
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 12/23/2019
ms.openlocfilehash: eb402c9cd7bb4f9ae472859fcd45fcc050d1df85
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182141"
---
# <a name="performance-tuning-network-adapters"></a>Ajustar el rendimiento de los adaptadores de red

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Utilice la información de este tema para optimizar los adaptadores de red de rendimiento para los equipos que ejecutan Windows Server 2016 y versiones posteriores. Si los adaptadores de red proporcionan opciones de optimización, puede usar estas opciones para optimizar el rendimiento de la red y el uso de recursos.

La configuración de optimización correcta para los adaptadores de red depende de las siguientes variables:

- El adaptador de red y su conjunto de características
- El tipo de carga de trabajo que realiza el servidor
- Los recursos de hardware y software del servidor
- Los objetivos de rendimiento para el servidor

En las siguientes secciones se describen algunas de las opciones de ajuste del rendimiento.

##  <a name="enabling-offload-features"></a><a name="bkmk_offload"></a>Habilitar las características de descarga

Activar las características de descarga del adaptador de red suele ser beneficioso. Sin embargo, es posible que el adaptador de red no sea lo suficientemente eficaz como para controlar las capacidades de descarga con un alto rendimiento.

> [!IMPORTANT]
> No use las características de descarga descarga de **tareas de IPSec** o descarga de **TCP Chimney**. Estas tecnologías están desusadas en Windows Server 2016 y pueden afectar negativamente al rendimiento del servidor y de la red. Además, es posible que Microsoft no admita estas tecnologías en el futuro.

Por ejemplo, considere un adaptador de red con recursos de hardware limitados.
En ese caso, la habilitación de las características de descarga de segmentación podría reducir el rendimiento máximo sostenible del adaptador. Sin embargo, si el rendimiento reducido es aceptable, debe continuar con las características de descarga de segmentación.

> [!NOTE]
> Algunos adaptadores de red requieren que se habiliten las características de descarga de forma independiente para las rutas de acceso de envío y recepción.

##  <a name="enabling-receive-side-scaling-rss-for-web-servers"></a><a name="bkmk_rss_web"></a>Habilitar el ajuste de escala en lado de recepción (RSS) para servidores Web

RSS puede mejorar la escalabilidad y el rendimiento web cuando hay menos adaptadores de red que procesadores lógicos en el servidor. Cuando todo el tráfico web atraviesa los adaptadores de red compatibles con RSS, el servidor puede procesar las solicitudes Web entrantes de distintas conexiones simultáneamente entre distintas CPU.

> [!IMPORTANT]
> Evite el uso de adaptadores de red que no sean de RSS y adaptadores de red compatibles con RSS en el mismo servidor. Debido a la lógica de distribución de la carga en RSS y el protocolo de transferencia de hipertexto (HTTP), el rendimiento puede verse afectado gravemente si un adaptador de red que no es compatible con RSS acepta tráfico web en un servidor que tiene uno o varios adaptadores de red compatibles con RSS. En este caso, debes usar adaptadores de red compatibles con RSS o deshabilitar RSS en la pestaña **Propiedades avanzadas** de las propiedades del adaptador de red.
>
> Para determinar si un adaptador de red es compatible con RSS, puedes ver la información de RSS en la pestaña **Propiedades avanzadas** de las propiedades del adaptador de red.

### <a name="rss-profiles-and-rss-queues"></a>Perfiles RSS y colas RSS

El perfil predefinido de RSS predeterminado es **NUMAStatic**, que difiere del valor predeterminado que usaban las versiones anteriores de Windows. Antes de empezar a usar perfiles RSS, revise los perfiles disponibles para saber cuándo son útiles y cómo se aplican a su entorno de red y hardware.

Por ejemplo, si abre el administrador de tareas y revisa los procesadores lógicos en el servidor y parecen estar infrautilizados para recibir tráfico, puede intentar aumentar el número de colas RSS desde el valor predeterminado de dos hasta el máximo que admita el adaptador de red. Quizás el adaptador de red tenga opciones para cambiar el número de colas RSS como parte del controlador.

##  <a name="increasing-network-adapter-resources"></a><a name="bkmk_resources"></a>Aumentar los recursos del adaptador de red

En el caso de los adaptadores de red que permiten configurar manualmente recursos como los búferes de envío y recepción, debe aumentar los recursos asignados.

Algunos adaptadores de red establecen un valor bajo para los búferes de recepción para conservar la memoria asignada del host. El valor bajo produce la pérdida de paquetes y un menor rendimiento. Por lo tanto, en escenarios con un alto volumen de recepción, te recomendamos que aumentes el valor del búfer de recepción al máximo.

> [!NOTE]
> Si un adaptador de red no expone la configuración de recursos manual, configura dinámicamente los recursos o los recursos se establecen en un valor fijo que no se puede cambiar.

### <a name="enabling-interrupt-moderation"></a>Habilitar moderación de interrupciones

Para controlar la moderación de interrupciones, algunos adaptadores de red exponen distintos niveles de moderación de interrupciones, diferentes parámetros de fusión de búferes (a veces por separado para los búferes de envío y recepción) o ambos.

Debe considerar la moderación de interrupciones para cargas de trabajo enlazadas a la CPU. Al usar la moderación de interrupciones, tenga en cuenta el equilibrio entre el ahorro y la latencia de la CPU del host frente al aumento del ahorro de CPU del host debido a más interrupciones y menos latencia. Si el adaptador de red no realiza la moderación de interrupciones, pero expone la fusión del búfer, puede mejorar el rendimiento si aumenta el número de búferes fusionados para permitir más búferes por envío o recepción.

##  <a name="performance-tuning-for-low-latency-packet-processing"></a><a name="bkmk_low"></a>Optimización del rendimiento para el procesamiento de paquetes de baja latencia

Muchos adaptadores de red ofrecen opciones para optimizar la latencia inducida por el sistema operativo. La latencia es el tiempo que transcurre desde que el controlador de red procesa un paquete de entrada hasta que lo envía de vuelta. Este tiempo suele medirse en microsegundos. En comparación, el tiempo de transmisión en transmisiones de paquetes en grandes distancias suele medirse en milisegundos (un orden de magnitud mayor). Este ajuste no reducirá el tiempo que un paquete está en tránsito.

Las siguientes son algunas sugerencias de ajuste para redes con una sensibilidad de microsegundos.

- Establece el BIOS del equipo en **Alto rendimiento**, con C-states deshabilitado. Sin embargo, ten en cuenta que esto depende del BIOS y del sistema, y que algunos sistemas proporcionarán un rendimiento mayor si el sistema operativo controla la administración de la energía. Puede comprobar y ajustar la configuración de administración de energía desde la **configuración** o mediante el comando **powercfg** . Para obtener más información, vea [Opciones de la línea de comandos de powercfg](https://docs.microsoft.com/windows-hardware/design/device-experiences/powercfg-command-line-options).

- Establece el perfil de administración de energía del sistema operativo en **Sistema de alto rendimiento**.
   > [!NOTE]
   > Esta configuración no funciona correctamente si se ha configurado el BIOS del sistema para deshabilitar el control del sistema operativo de la administración de energía.

- Habilitar descargas estáticas. Por ejemplo, habilite las sumas de comprobación UDP, las sumas de comprobación TCP y la configuración de envío de gran carga (LSO).

- Si el tráfico tiene varias secuencias, por ejemplo, al recibir tráfico de multidifusión de gran volumen, habilite RSS.

- Deshabilita la opción **Moderación de interrupciones** para los controladores de tarjetas de red que requieran la menor latencia posible. Recuerde que esta configuración puede utilizar más tiempo de CPU y representa un equilibrio.

- Administra las interrupciones y DPC de los adaptadores de red en un procesador de núcleo que comparta la memoria caché de la CPU con el núcleo usado por el programa (subproceso de usuario) que está administrando el paquete. Para ello, se puede usar el ajuste de la afinidad de la CPU para dirigir un proceso a determinados procesadores lógicos junto con la configuración de RSS. Cuando se usa el mismo núcleo para la interrupción, el DPC y el subproceso del modo de usuario, el rendimiento es menor porque la carga aumenta debido a que el ISR, DPC y el subproceso luchan por usar el núcleo.

##  <a name="system-management-interrupts"></a><a name="bkmk_smi"></a>Interrupciones de administración del sistema

Muchos sistemas de hardware usan interrupciones de administración del sistema (SMI) para diversas funciones de mantenimiento, como la notificación de errores de memoria de código de corrección de errores (ECC), el mantenimiento de la compatibilidad con USB heredada, el control del ventilador y la administración de la configuración de energía controlada por BIOS.

La SMI es la interrupción de la prioridad más alta en el sistema y coloca la CPU en modo de administración. Este modo se adelanta a todas las demás actividades mientras SMI ejecuta una rutina de servicio de interrupción, normalmente contenida en BIOS.

Desafortunadamente, este comportamiento puede producir picos de latencia de 100 microsegundos o más.

Si necesitas lograr la menor latencia, debes solicitar a tu proveedor de hardware una versión del BIOS que reduzca los SMI al mínimo posible. Estas versiones del BIOS suelen denominarse "BIOS de baja latencia" o "BIOS gratis de SMI". En algunos casos, en una plataforma de hardware no se puede eliminar la actividad de SMI por completo porque se usa para controlar funciones esenciales (por ejemplo, los ventiladores de refrigeración).

> [!NOTE]
> El sistema operativo no puede controlar SMIs porque el procesador lógico se está ejecutando en un modo de mantenimiento especial, lo que impide la intervención del sistema operativo.

##  <a name="performance-tuning-tcp"></a><a name="bkmk_tcp"></a>Ajuste del rendimiento de TCP

 Puede usar los siguientes elementos para optimizar el rendimiento de TCP.

###  <a name="tcp-receive-window-autotuning"></a><a name="bkmk_tcp_params"></a>Ajuste automático de la ventana de recepción de TCP

En Windows Vista, Windows Server 2008 y versiones posteriores de Windows, la pila de red de Windows usa una característica denominada nivel de optimización automática de la *ventana de recepción de TCP* para negociar el tamaño de la ventana de recepción de TCP. Esta característica puede negociar un tamaño de ventana de recepción definido para cada comunicación TCP durante el protocolo de enlace TCP.

En versiones anteriores de Windows, la pila de red de Windows usaba una ventana de recepción de tamaño fijo (65.535 bytes) que limitaba el rendimiento total potencial de las conexiones. El rendimiento total factible de las conexiones TCP podría limitar los escenarios de uso de red. El ajuste automático de la ventana de recepción de TCP habilita estos escenarios para usar completamente la red.

En el caso de una ventana de recepción TCP que tenga un tamaño determinado, puede utilizar la siguiente ecuación para calcular el rendimiento total de una única conexión.

> *Rendimiento total factible en bytes*  =  *Tamaño de la ventana de recepción TCP en bytes* \* (1/ *latencia de conexión en segundos*)

Por ejemplo, para una conexión que tiene una latencia de 10 ms, el rendimiento total que se alcanza es solo de 51 Mbps. Este valor es razonable para una infraestructura de red corporativa de gran tamaño. Sin embargo, mediante el ajuste automático para ajustar la ventana de recepción, la conexión puede lograr la velocidad de línea completa de una conexión de 1 Gbps.

Algunas aplicaciones definen el tamaño de la ventana de recepción de TCP. Si la aplicación no define el tamaño de la ventana de recepción, la velocidad del vínculo determina el tamaño de la manera siguiente:

- Menos de 1 megabit por segundo (Mbps): 8 kilobytes (KB)
- de 1 Mbps a 100 Mbps: 17 KB
- 100 Mbps a 10 Gigabits por segundo (Gbps): 64 KB
- 10 Gbps o más rápido: 128 KB

Por ejemplo, en un equipo que tiene instalado un adaptador de red de 1 Gbps, el tamaño de la ventana debe ser de 64 KB.

Esta característica también hace un uso completo de otras características para mejorar el rendimiento de la red. Estas características incluyen el resto de las opciones de TCP definidas en [RFC 1323](https://tools.ietf.org/html/rfc1323). Mediante el uso de estas características, los equipos basados en Windows pueden negociar tamaños de ventana de recepción de TCP más pequeños, pero que se escalan en un valor definido, dependiendo de la configuración. Este comportamiento facilita el control de los dispositivos de red.

> [!NOTE]
> Puede experimentar un problema en el que el dispositivo de red no sea compatible con la opción de escala de la **ventana TCP**, tal y como se define en [RFC 1323](https://tools.ietf.org/html/rfc1323) y, por lo tanto, no admite el factor de escala. En tales casos, consulte este [KB 934430, se produce un error de conectividad de red al intentar usar Windows Vista detrás de un dispositivo de Firewall](https://support.microsoft.com/help/934430/network-connectivity-fails-when-you-try-to-use-windows-vista-behind-a) o ponerse en contacto con el equipo de soporte técnico del proveedor de dispositivos de red.

#### <a name="review-and-configure-tcp-receive-window-autotuning-level"></a>Revisar y configurar el nivel de ajuste automático de la ventana de recepción de TCP

Puede usar comandos Netsh o cmdlets de Windows PowerShell para revisar o modificar el nivel de ajuste automático de la ventana de recepción de TCP.

> [!NOTE]
> A diferencia de las versiones de Windows anteriores a Windows 10 o Windows Server 2019, ya no se puede usar el registro para configurar el tamaño de la ventana de recepción de TCP. Para obtener más información sobre la configuración en desuso, consulte [parámetros TCP desusados](#deprecated-tcp-parameters).

> [!NOTE]
> Para obtener información detallada acerca de los niveles de optimización automático disponibles, consulte [autotuning Levels](#autotuning-levels).

**Para usar Netsh para revisar o modificar el nivel de ajuste automático**

Para revisar la configuración actual, abra una ventana del símbolo del sistema y ejecute el siguiente comando:

```cmd
netsh interface tcp show global
```

La salida de este comando debe ser similar a la siguiente:

```
Querying active state...

TCP Global Parameters
-----
Receive-Side Scaling State : enabled
Chimney Offload State : disabled
Receive Window Auto-Tuning Level : normal
Add-On Congestion Control Provider : default
ECN Capability : disabled
RFC 1323 Timestamps : disabled
Initial RTO : 3000
Receive Segment Coalescing State : enabled
Non Sack Rtt Resiliency : disabled
Max SYN Retransmissions : 2
Fast Open : enabled
Fast Open Fallback : enabled
Pacing Profile : off
```

Para modificar la configuración, ejecute el siguiente comando en el símbolo del sistema:

```cmd
netsh interface tcp set global autotuninglevel=<Value>
```

> [!NOTE]
> En el comando anterior, \<*Value*> representa el nuevo valor para el nivel de ajuste automático.

Para obtener más información acerca de este comando, vea [comandos Netsh para el protocolo de control de transmisión de interfaz](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731258(v=ws.10)).

**Para usar PowerShell para revisar o modificar el nivel de ajuste automático**

Para revisar la configuración actual, abra una ventana de PowerShell y ejecute el siguiente cmdlet.

```PowerShell
Get-NetTCPSetting | Select SettingName,AutoTuningLevelLocal
```

La salida de este cmdlet debe ser similar a la siguiente.

```
SettingName           AutoTuningLevelLocal
-----------          --------------------
Automatic
InternetCustom       Normal
DatacenterCustom     Normal
Compat               Normal
Datacenter           Normal
Internet             Normal
```

Para modificar la configuración, ejecute el siguiente cmdlet en el símbolo del sistema de PowerShell.

```PowerShell
Set-NetTCPSetting -AutoTuningLevelLocal <Value>
```

> [!NOTE]
> En el comando anterior, \<*Value*> representa el nuevo valor para el nivel de ajuste automático.

Para obtener más información sobre estos cmdlets, consulte los siguientes artículos:

- [Get-NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/get-nettcpsetting?view=win10-ps)
- [Set-NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

#### <a name="autotuning-levels"></a>Ajustar niveles

Puede establecer el ajuste automático de la ventana de recepción en cinco niveles. El nivel predeterminado es **normal**. En la tabla siguiente se describen los niveles.

|Nivel |Valor hexadecimal |Comentarios |
| --- | --- | --- |
|Normal (opción predeterminada) |0x8 (factor de escala de 8) |Establezca el tamaño de la ventana de recepción de TCP para que se adapte a casi todos los escenarios. |
|Deshabilitada |No hay ningún factor de escala disponible |Establezca la ventana de recepción TCP en su valor predeterminado. |
|Restringido |0x4 (factor de escala de 4) |Establezca la ventana de recepción TCP para que supere su valor predeterminado, pero limite dicho crecimiento en algunos escenarios. |
|Muy restringido |0X2 (factor de escala de 2) |Establezca la ventana de recepción TCP para que crezca más allá de su valor predeterminado, pero hágalo con mucha cautela. |
|Habilitación de características |0xE (factor de escala de 14) |Establezca la ventana de recepción de TCP en crecimiento para adaptarse a escenarios extremos. |

Si utiliza una aplicación para capturar paquetes de red, la aplicación debe notificar datos similares a los siguientes para diferentes valores de nivel de ajuste automático de ventana.

- Nivel de optimización automático: **normal (estado predeterminado)**

   ```
   Frame: Number = 492, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2667, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60975, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=4075590425, Ack=0, Win=64240 ( Negotiating scale factor 0x8 ) = 64240
   SrcPort: 60975
   DstPort: Microsoft-DS(445)
   SequenceNumber: 4075590425 (0xF2EC9319)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x8 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x8 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 8 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nivel de optimización automático: **deshabilitado**

   ```
   Frame: Number = 353, Captured Frame Length = 62, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2576, Total IP Length = 48
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60956, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2315885330, Ack=0, Win=64240 ( ) = 64240
   SrcPort: 60956
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2315885330 (0x8A099B12)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 112 (0x70)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( ) = 64240 ----------------------------------------> TCP Receive Window set as 64K as per NIC Link bitrate. Note there is no Scale Factor defined. In this case, Scale factor is not being sent as a TCP Option, so it will not be used by Windows.
   Checksum: 0x817E, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nivel de optimización automático: **restringido**

   ```
   Frame: Number = 3, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2319, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60890, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1966088568, Ack=0, Win=64240 ( Negotiating scale factor 0x4 ) = 64240
   SrcPort: 60890
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1966088568 (0x75302178)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x4 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x4 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 4 -------------------------------> Scale factor, defined by AutoTuningLevel.
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nivel de ajuste automático: **muy restringido**

   ```
   Frame: Number = 115, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2388, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60903, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1463725706, Ack=0, Win=64240 ( Negotiating scale factor 0x2 ) = 64240
   SrcPort: 60903
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1463725706 (0x573EAE8A)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x2 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x2 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 2 ------------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Nivel de optimización automático: **experimental**

   ```
   Frame: Number = 238, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2490, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60933, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2095111365, Ack=0, Win=64240 ( Negotiating scale factor 0xe ) = 64240
   SrcPort: 60933
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2095111365 (0x7CE0DCC5)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0xe ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0xe Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 14 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

#### <a name="deprecated-tcp-parameters"></a>Parámetros TCP desusados

La siguiente configuración del registro de Windows Server 2003 ya no se admite y se omiten en versiones posteriores.

- **TcpWindowSize**
- **NumTcbTablePartitions**
- **MaxHashTableSize**

Todas estas opciones se encontraban en la subclave del Registro siguiente:

> **HKEY_LOCAL_MACHINE \System\CurrentControlSet\Services\Tcpip\Parameters**

###  <a name="windows-filtering-platform"></a><a name="bkmk_wfp"></a>Plataforma de filtrado de Windows

Windows Vista y Windows Server 2008 presentaron la plataforma de filtrado de Windows (WFP). WFP proporciona API a proveedores de software independientes (ISV) que no son de Microsoft para crear filtros de procesamiento de paquetes. Algunos ejemplos son firewall y software antivirus.

> [!NOTE]
> Un filtro WFP mal escrito puede reducir significativamente el rendimiento de red de un servidor. Para obtener más información, vea [portar controladores de procesamiento de paquetes y aplicaciones a WFP](https://docs.microsoft.com/windows-hardware/drivers/network/porting-packet-processing-drivers-and-apps-to-wfp) en el centro de desarrollo de Windows.

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste del rendimiento del subsistema de red](net-sub-performance-top.md).
