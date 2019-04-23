---
title: Elección de un adaptador de red
description: En este tema forma parte de la Guía de ajuste de rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2b50f4b286e90a450278243c0294ea0aa7f221bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875856"
---
# <a name="choosing-a-network-adapter"></a>Elección de un adaptador de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre algunas de las características de los adaptadores de red que podrían afectar a las opciones de compras.

Aplicaciones de red intensiva requieren adaptadores de red de alto rendimiento. Esta sección explora algunas consideraciones para elegir los adaptadores de red, así como cómo configurar el adaptador de red diferente para lograr el mejor rendimiento de red.

> [!TIP]
>  Puede configurar la configuración del adaptador de red mediante Windows PowerShell. Para obtener más información, consulte [Cmdlets de adaptador de red en Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a> Capacidades de descarga

Descargar las tareas de la unidad central de procesamiento \(CPU\) a la red adaptador puede reducir el uso de CPU en el servidor, lo que mejora el rendimiento general del sistema.

La pila de red en productos de Microsoft puede descargar uno o más tareas a un adaptador de red si selecciona un adaptador de red que tiene el adecuado de capacidades de descarga. En la tabla siguiente proporciona una breve descripción de las capacidades de descarga diferentes que están disponibles en Windows Server 2016.
  
|Tipo de la descarga|Descripción|
|------------------|-----------------|  
|Cálculo de suma de comprobación para TCP|La pila de red puede descargar el cálculo y la validación del protocolo de Control de transmisión \(TCP\) las sumas de comprobación en enviar y recibir las rutas de código. También puede descargar el cálculo y la validación de IPv4 y IPv6 las sumas de comprobación en enviar y recepción las rutas de código.|  
|Cálculo de suma de comprobación para UDP |La pila de red puede descargar el cálculo y la validación del protocolo de datagramas de usuario \(UDP\) las sumas de comprobación en enviar y recibir las rutas de código.|
|Cálculo de suma de comprobación para IPv4 |La pila de red puede descargar el cálculo y la validación de IPv4, las sumas de comprobación en enviar y recepción las rutas de código. |
|Cálculo de suma de comprobación para IPv6 |La pila de red puede descargar el cálculo y la validación de IPv6, las sumas de comprobación en enviar y recepción las rutas de código. | 
|Segmentación de los paquetes TCP grandes|La capa de transporte de TCP/IP es compatible con la descarga de envío grande v2 (LSOv2). Con LSOv2, la capa de transporte de TCP/IP puede descargar la segmentación de los paquetes TCP grandes al adaptador de red.|  
|Escala en lado de recepción \(RSS\)|RSS es una tecnología de controlador de red que permite la distribución eficaz de red de procesamiento de recepción a través de varias CPU en sistemas multiprocesador. Más adelante en este tema, se proporcionan más detalles acerca de RSS.|  
|Fusión de segmentos de recepción \(RSC\)|RSC es la capacidad de los paquetes de grupo para minimizar el encabezado de procesamiento que sea necesaria para el host para realizar. Un máximo de 64 KB de carga recibido se puedan combinar en un único paquete más grande para su procesamiento. Se proporcionan más detalles acerca de RSC más adelante en este tema.|  
  
###  <a name="bkmk_rss"></a> Escala en lado de recepción

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 y Windows Server 2008 admiten la escala de recepción \(RSS\). 

Algunos servidores están configurados con varios procesadores lógicos que comparten los recursos de hardware \(como un núcleo físico\) y que se tratan como simultáneo de subprocesamiento múltiple \(SMT\) elementos del mismo nivel. La tecnología Hyper-Threading de Intel es un ejemplo. RSS dirige el procesamiento de red a un procesador lógico por núcleo. Por ejemplo, en un servidor con Hyper-Threading de Intel, 4 núcleos y 8 procesadores lógicos, RSS usa no más de 4 procesadores lógicos para el procesamiento de la red.  

RSS distribuye los paquetes entrantes de la E/S de red entre los procesadores lógicos para que los paquetes que pertenecen a la misma conexión TCP se procesan en el mismo procesador lógico, lo que se conserva la ordenación. 

RSS también cargar tráfico multidifusión y unidifusión UDP de saldos y enruta los flujos relacionados \(que se determinan aplicando un algoritmo hash a las direcciones de origen y destino\) con el mismo procesador lógico, conservando el orden de llegadas relacionados. Esto ayuda a mejorar la escalabilidad y rendimiento en escenarios de recepción intensivo para los servidores que tienen menos adaptadores de red de como lo hacen aptos procesadores lógicos. 

#### <a name="configuring-rss"></a>Configuración de RSS

En Windows Server 2016, puede configurar RSS mediante los cmdlets de Windows PowerShell y perfiles de RSS. 

Puede definir perfiles de RSS mediante el **: perfil** parámetro de la **Set-NetAdapterRss** cmdlet de Windows PowerShell.

**Comandos de Windows PowerShell para la configuración de RSS**

Los siguientes cmdlets le permiten ver y modificar los parámetros RSS por el adaptador de red.
  
>[!NOTE]
>Para obtener una referencia detallada acerca del comando para cada cmdlet, incluida la sintaxis y parámetros, puede hacer clic en los vínculos siguientes. Además, puede pasar el nombre del cmdlet **Get-Help** en el símbolo del sistema de Windows PowerShell para obtener más información sobre cada comando.  

- [Disable-NetAdapterRss](https://technet.microsoft.com/library/jj130892). Este comando deshabilita RSS en el adaptador de red que especifique.

- [Enable-NetAdapterRss](https://technet.microsoft.com/library/jj130859). Este comando habilita RSS en el adaptador de red que especifique.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Este comando recupera las propiedades RSS del adaptador de red que especifique.
  
- [Set-NetAdapterRss](https://technet.microsoft.com/library/jj130863). Este comando establece las propiedades RSS en el adaptador de red que especifique.  

#### <a name="rss-profiles"></a>Perfiles de RSS

Puede usar el **: perfil** parámetro del cmdlet Set-NetAdapterRss para especificar qué procesadores lógicos se asignan a qué adaptador de red. Los valores disponibles para este parámetro son:

- **Más cercano**. Se prefieren los números de procesador lógico que están cerca de procesador RSS de la base del adaptador de red. Con este perfil, el sistema operativo podría volver a equilibrar dinámicamente según la carga de procesadores lógicos.
  
- **ClosestStatic**. Se prefieren los números de procesador lógico cerca de procesador RSS de la base del adaptador de red. Con este perfil, el sistema operativo no volver a equilibrar dinámicamente según la carga de procesadores lógicos.
  
- **NUMA**. Números de procesador lógico generalmente se seleccionan en nodos NUMA diferentes para distribuir la carga. Con este perfil, el sistema operativo podría volver a equilibrar dinámicamente según la carga de procesadores lógicos.
  
- **NUMAStatic**. Se trata de la **perfil predeterminado**. Números de procesador lógico generalmente se seleccionan en nodos NUMA diferentes para distribuir la carga. Con este perfil, el sistema operativo no reequilibrará dinámicamente según la carga de procesadores lógicos.

- **Conservador**. RSS usa la menor cantidad de procesadores posible para admitir la carga. Esta opción ayuda a reducir el número de interrupciones.

Según el escenario y las características de carga de trabajo, también puede usar otros parámetros de la **Set-NetAdapterRss** cmdlet de Windows PowerShell para especificar lo siguiente:

- En una base de adaptador de red, cuántos procesadores lógicos se pueden usar para RSS.
- Desplazamiento inicial del intervalo de procesadores lógicos.
- El nodo desde el que el adaptador de red asigna memoria.

Siguientes son adicionales **Set-NetAdapterRss** parámetros que puede usar para configurar RSS:

>[!NOTE]
>En la sintaxis de ejemplo para cada parámetro a continuación, el nombre del adaptador de red **Ethernet** se utiliza como un valor de ejemplo para el **– nombre** parámetro de la **Set-NetAdapterRss** comando. Al ejecutar el cmdlet, asegúrese de que el nombre del adaptador de red que usa es adecuado para su entorno.

- **\* MaxProcessors**: Establece el número máximo de procesadores RSS para usarse. Esto garantiza que el tráfico de la aplicación está enlazado a un número máximo de procesadores en una interfaz determinada. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\* BaseProcessorGroup**: Establece el grupo de procesadores de base de un nodo NUMA. Esto afecta a la matriz de procesador que está usando RSS. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**: Establece el grupo de procesadores máximo de un nodo NUMA. Esto afecta a la matriz de procesador que está usando RSS. Establecer este valor podría restringir un grupo de procesadores máximo para que el equilibrio de carga se alinea dentro de un grupo k. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**: Establece el número de procesadores de base de un nodo NUMA. Esto afecta a la matriz de procesador que está usando RSS. Esto permite crear particiones procesadores en todos los adaptadores de red. Este es el primer procesador lógico en los procesadores de intervalo de RSS que se asigna a cada adaptador. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**: Cada adaptador de red puede asignar memoria desde el nodo NUMA. Puede tratarse de un grupo k o desde diferentes grupos k. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\* NumberofReceiveQueues**: Si los procesadores lógicos parecen estar infrautilizados para recibir tráfico \(por ejemplo, como vio en el Administrador de tareas\), puede intentar aumentar el número de colas RSS desde las 2 predeterminadas al máximo que sea compatible con el adaptador de red . El adaptador de red puede tener opciones para cambiar el número de colas RSS como parte del controlador. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Para obtener más información, haga clic en el siguiente vínculo para descargar [Scalable Networking: Eliminando la recepción de procesamiento cuello de botella: Introducción a RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) en formato de Word.
  
#### <a name="understanding-rss-performance"></a>Análisis de rendimiento de RSS

Ajustar RSS, es necesario comprender la configuración y la lógica de equilibrio de carga. Para comprobar que la configuración de RSS ha surtido efecto, puede revisar la salida al ejecutar el **Get-NetAdapterRss** cmdlet de Windows PowerShell. Siguiente es el resultado de ejemplo de este cmdlet.
  
```

PS C:\Users\Administrator> get-netadapterrss  
Name                           : testnic 2  
InterfaceDescription           : Broadcom BCM5708C NetXtreme II GigE (NDIS VBD Client) #66
Enabled                        : True
NumberOfReceiveQueues          : 2
Profile                        : NUMAStatic
BaseProcessor: [Group:Number]  : 0:0
MaxProcessor: [Group:Number]   : 0:15
MaxProcessors                  : 8
  
IndirectionTable: [Group:Number]:
     0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
…   
(# indirection table entries are a power of 2 and based on # of processors)  
…   
                          0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
```  

Además de repetir los parámetros que se establecieron, el aspecto clave de la salida es el resultado de la tabla de direccionamiento indirecto. La tabla de direccionamiento indirecto muestran los depósitos de tabla hash que se utilizan para distribuir el tráfico entrante. En este ejemplo, la notación n:c designa la K Numa-par de índice de grupo: CPU que se usa para dirigir el tráfico entrante. Vemos exactamente 2 entradas únicas (0:0 y 0:4), que representan grupos k 0/cpu0 y grupo k 0/cpu 4, respectivamente.

Hay un único grupo k para este sistema (grupo k 0) y una n (donde n < = 128) entrada de la tabla de direccionamiento indirecto. Dado que el número de colas de recepción se establece en 2, solo 2 procesadores (0:0, 0:4) se eligen - aunque procesadores máximos está establecido en 8. De hecho, la tabla de direccionamiento indirecto es hash el tráfico entrante para usar sólo 2 CPU fuera el 8 que están disponibles.

Para utilizar totalmente la CPU, el número de colas de RSS de recepción debe ser igual o mayor que el máximo procesadores. En el ejemplo anterior, la cola de recepción debe establecerse en 8 o superior.

#### <a name="nic-teaming-and-rss"></a>Formación de equipos NIC y RSS

RSS puede habilitarse en un adaptador de red que se combinan con otra tarjeta de interfaz de red con formación de equipos NIC. En este escenario, sólo el adaptador de red física subyacente puede configurarse para usar RSS. Un usuario no puede establecer los cmdlets RSS en el adaptador de red agrupados.
  
###  <a name="bkmk_rsc"></a> Recepción (RSC) de fusión de segmentos

Fusión de segmentos de recepción \(RSC\) mejora el rendimiento al reducir el número de encabezados IP que se procesan durante un período determinado de los datos recibidos. Se debe usar para ayudar a escalar el rendimiento de los datos recibidos mediante la agrupación de \(o fusión\) los paquetes más pequeños en unidades más grandes.

Este enfoque puede afectar a la latencia ventajas mayoritariamente en las mejoras de rendimiento. Se recomienda aumentar el rendimiento para cargas de trabajo recibidos RSC. Considere la posibilidad de implementar los adaptadores de red que admiten RSC. 

En estos adaptadores de red, asegúrese de que RSC está en \(trata la configuración predeterminada\), a menos que tenga cargas de trabajo específicas \(por ejemplo, baja latencia, bajo redes rendimiento\) que aprovechan show RSC está desactivado .

#### <a name="understanding-rsc-diagnostics"></a>Descripción de los diagnósticos RSC

Puede diagnosticar RSC mediante los cmdlets de Windows PowerShell **Get NetAdapterRsc** y **Get NetAdapterStatistics**.

Siguiente es el resultado de ejemplo al ejecutar el cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

El **obtener** cmdlet muestra si RSC está habilitado en la interfaz y si TCP permite RSC en un estado operativo. El motivo del error proporciona detalles acerca del error para habilitar RSC en esa interfaz.

En el escenario anterior, IPv4 RSC está operativa y no admitidos en la interfaz. Para entender los errores de diagnóstico, puede ver los bytes combinados o las excepciones producidas. Esto proporciona una indicación de los problemas de fusión.

Siguiente es el resultado de ejemplo al ejecutar el cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC y virtualización

RSC solo se admite en el host físico cuando el adaptador de red de host no está enlazado al conmutador Virtual de Hyper-V. Cuando el host está enlazado al conmutador Virtual de Hyper-V, RSC está deshabilitada por el sistema operativo. Además, las máquinas virtuales no obtienen la ventaja de RSC porque los adaptadores de red virtual no compatible con RSC.

RSC puede habilitarse para una máquina virtual cuando la virtualización de entrada/salida de raíz única \(SR-IOV\) está habilitado. En este caso, las funciones virtuales admiten la funcionalidad RSC; por lo tanto, las máquinas virtuales también reciben un beneficio de RSC.

##  <a name="bkmk_resources"></a> Recursos de adaptador de red

Algunos adaptadores de red administración activamente los recursos para lograr un rendimiento óptimo. Varios adaptadores de red le permiten configurar manualmente los recursos mediante el uso de la **Advanced Networking** pestaña para el adaptador. Para estos adaptadores, puede establecer los valores de un número de parámetros, incluido el número de búferes de recepción y los búferes de envío.

Configuración de recursos de adaptador de red se simplifica mediante el uso de los siguientes cmdlets de Windows PowerShell.

- [Get-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [Set-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [Enable-NetAdapter](https://technet.microsoft.com/library/jj130876.aspx)

- [Enable-NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [Enable-NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [Enable-NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [Enable-NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [Enable-NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [Enable-NetAdapterQos](https://technet.microsoft.com/library/jj130866.aspx)

- [Enable-NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [Enable-NetAdapterSriov](https://technet.microsoft.com/library/jj130899.aspx)

Para obtener más información, consulte [Cmdlets de adaptador de red en Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste de rendimiento del subsistema de red](net-sub-performance-top.md).