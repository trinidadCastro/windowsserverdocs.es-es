---
title: Elección de un adaptador de red
description: Este tema es parte de la Guía de optimización del rendimiento de red subsistema de Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b3b9d206273dfd0e9115ebc27cf28aa960bfb0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-network-adapter"></a>Elección de un adaptador de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre algunas de las características de adaptadores de red que podrían afectar a las opciones de compras.

Aplicaciones basadas en red requieren adaptadores de red de alto rendimiento. En esta sección se explora algunas consideraciones para elegir los adaptadores de red, así como configurar el adaptador de red diferentes para lograr el mejor rendimiento de red.

> [!TIP]
>  Configuración del adaptador de red puede configurar mediante Windows PowerShell. Para obtener más información, consulta [Cmdlets del adaptador de red en Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a>Capacidades de descarga

Descarga de tareas desde la unidad central de procesamiento \(CPU\) al adaptador de red puede reducir el uso de CPU en el servidor, lo que mejora el rendimiento general del sistema.

La pila de red en los productos de Microsoft puede descargar uno o más tareas a un adaptador de red si seleccionas un adaptador de red que tiene los ajustes de capacidades de descarga. La siguiente tabla proporciona una breve introducción de la descarga de diferentes capacidades que están disponibles en Windows Server 2016.
  
|La descarga de tipo|Descripción|
|------------------|-----------------|  
|Cálculo de suma de comprobación para TCP|La pila de red puede descargar el cálculo y validación de Control de transporte Protocolo \(TCP\) sumas de comprobación de enviar y recibir las rutas de código. También puede descargar el cálculo y la validación de IPv4 y IPv6 sumas de comprobación de enviar y reciban las rutas de código.|  
|Cálculo de suma de comprobación para UDP |La pila de red puede descargar el cálculo y validación de protocolo de datagramas de usuario \(UDP\) sumas de comprobación de enviar y recibir las rutas de código.|
|Cálculo de suma de comprobación para IPv4 |La pila de red puede descargar el cálculo y la validación de IPv4 sumas de comprobación de enviar y reciban las rutas de código. |
|Cálculo de suma de comprobación para IPv6 |La pila de red puede descargar el cálculo y la validación de IPv6 sumas de comprobación de enviar y reciban las rutas de código. | 
|Segmentación de paquetes TCP grandes|La capa de transporte TCP/IP admite la descarga de enviar grandes v2 (LSOv2). Con LSOv2, la capa de transporte TCP/IP puede descargar la segmentación de paquetes TCP grandes al adaptador de red.|  
|Recibir lado \(RSS\) de ajuste de escala|RSS es una tecnología de controladores de red que permite la distribución eficaz de la red recibe procesamiento entre varias CPU en sistemas con varios procesadores. Más adelante en este tema, se proporcionan más detalles acerca de RSS.|  
|Recibir segmento fusión \(RSC\)|RSC es la capacidad de paquetes de grupo para minimizar el encabezado de procesamiento que sea necesaria para el host para llevar a cabo. Puede fusionarse un máximo de 64 KB de carga recibido en un único paquete más grande para el procesamiento. Se proporcionan más detalles sobre RSC más adelante en este tema.|  
  
###  <a name="bkmk_rss"></a>Recibir escala

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 y Windows Server 2008 admiten \(RSS\) escala de recepción. 

Algunos servidores están configurados con varios procesadores lógicos que comparten recursos de hardware \ (por ejemplo, un core\ físico) y que se tratan como \(SMT\) múltiples subprocesos simultáneos sistemas del mismo nivel. Tecnología Hyper-Threading de Intel es un ejemplo. RSS indica al procesamiento de red para hasta un procesador lógico por núcleo. Por ejemplo, en un servidor con Hyper-Threading de Intel, 4 núcleos y 8 procesadores lógicos, RSS usa procesadores lógicos no más de 4 para el procesamiento de red.  

RSS distribuye paquetes de E/S de red entrantes entre procesadores lógicos para que se procesen paquetes que pertenecen a la misma conexión TCP en el mismo procesador lógico, que conserva el orden. 

RSS carga también los saldos UDP unidifusión y el tráfico de multidifusión y enruta flujos relacionados \ (que dependen de la aplicación del algoritmo hash addresses\ de origen y destino) con el mismo procesador lógico, conserva el orden de las llegadas relacionados. Esto ayuda a mejorar la escalabilidad y rendimiento de los escenarios de uso intensivo recibir los servidores que tengan menos de los adaptadores de red que tienen procesadores lógicos elegibles. 

#### <a name="configuring-rss"></a>Configuración de RSS

En Windows Server 2016, puedes configurar RSS mediante cmdlets de Windows PowerShell y perfiles RSS. 

Puedes definir los perfiles RSS mediante el uso de la **: perfil** parámetro de la **conjunto NetAdapterRss** cmdlet de Windows PowerShell.

**Comandos de Windows PowerShell para la configuración de RSS**

Los siguientes cmdlets te permiten ver y modificar los parámetros RSS por el adaptador de red.
  
>[!NOTE]
>Para obtener una referencia detallada acerca del comando para cada cmdlet, incluida la sintaxis y los parámetros, puedes hacer clic en los siguientes vínculos. Además, puedes pasar el nombre del cmdlet para **Get-Help** en el símbolo del sistema de Windows PowerShell para obtener detalles sobre cada comando.  

- [Deshabilitar NetAdapterRss](https://technet.microsoft.com/library/jj130892). Este comando deshabilita RSS en el adaptador de red que especifiques.

- [Habilitar NetAdapterRss](https://technet.microsoft.com/library/jj130859). Este comando permite RSS en el adaptador de red que especifiques.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Este comando recupera las propiedades de RSS del adaptador de red que especifiques.
  
- [Conjunto NetAdapterRss](https://technet.microsoft.com/library/jj130863). Este comando establece las propiedades RSS en el adaptador de red que especifiques.  

#### <a name="rss-profiles"></a>Perfiles RSS

Puedes usar el **: perfil** parámetro del cmdlet Set-NetAdapterRss para especificar qué procesadores lógicos se asignan a qué adaptador de red. Los valores disponibles para este parámetro son:

- **Más cercana**. Se prefieren los números de procesador lógico que se encuentran cerca de procesador RSS base del adaptador de red. Con este perfil, el sistema operativo puede equilibrar los procesadores lógicos dinámicamente en función de carga.
  
- **ClosestStatic**. Se prefieren los números de procesador lógico cerca de procesador RSS base del adaptador de red. Con este perfil, el sistema operativo no equilibrar los procesadores lógicos dinámicamente en función de carga.
  
- **NUMA**. Números de procesador lógico por lo general, se seleccionan en distintos nodos NUMA para distribuir la carga. Con este perfil, el sistema operativo puede equilibrar los procesadores lógicos dinámicamente en función de carga.
  
- **NUMAStatic**. Este es el **perfil predeterminado**. Números de procesador lógico por lo general, se seleccionan en distintos nodos NUMA para distribuir la carga. Con este perfil, el sistema operativo no se equilibrar los procesadores lógicos dinámicamente en función de carga.

- **Conservadores**. RSS usa menor número de procesadores como sea posible para mantener la carga. Esta opción ayuda a reducir el número de interrupciones.

Según el escenario y las características de carga de trabajo, también puedes usar otros parámetros de la **conjunto NetAdapterRss** cmdlet de Windows PowerShell para especificar lo siguiente:

- En una base de adaptador de red, la cantidad de procesadores lógica puede usarse para RSS.
- El desplazamiento inicial para el intervalo de procesadores lógicos.
- El nodo desde el que el adaptador de red asigna memoria.

Siguiente es los más **conjunto NetAdapterRss** parámetros que puedes usar para configurar RSS:

>[!NOTE]
>En la sintaxis de ejemplo para cada parámetro a continuación, el nombre del adaptador de red **Ethernet** se usa como un valor de ejemplo para el **: nombre** parámetro de la **conjunto NetAdapterRss** comando. Cuando se ejecuta el cmdlet, asegúrate de que el nombre del adaptador de red que usas es apropiado para el entorno.

- **\ * MaxProcessors**: establece el número máximo de procesadores RSS para usarse. Esto garantiza que el tráfico de la aplicación está enlazado a un número máximo de procesadores en una interfaz dada. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\ * BaseProcessorGroup**: establece el grupo de procesador de base de un nodo NUMA. Esto afecta a la matriz de procesador que se usa en RSS. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\ * MaxProcessorGroup**: establece el grupo de procesadores Max de un nodo NUMA. Esto afecta a la matriz de procesador que se usa en RSS. Esta configuración pueda restringir un grupo de máxima del procesador para que el equilibrio de carga se alinea dentro de un grupo de k. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\ * BaseProcessorNumber**: establece el número de procesador de base de un nodo NUMA. Esto afecta a la matriz de procesador que se usa en RSS. Esto permite la partición procesadores en adaptadores de red. Este es el primer procesador lógico en los procesadores de intervalo de RSS que se asigna a cada adaptador. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\ * NumaNode**: nodos NUMA el que cada adaptador de red puede asignar memoria a partir de. Esto puede ser un grupo de k o desde diferentes k-grupos. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\ * NumberofReceiveQueues**: si tus procesadores lógicos parecen estar infrautilizado para recibir tráfico \ (por ejemplo, tal como se ve en el Administrador de tareas archivos\), puede intentar aumentar el número de colas RSS del valor predeterminado de 2 en la medida en que es compatible con el adaptador de red. El adaptador de red puede tener opciones para cambiar el número de colas RSS como parte del controlador. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Para obtener más información, haz clic en el siguiente vínculo para descargar [escalable a redes: elimina el recibir procesamiento cuello de botella: Introducción RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) en formato de Word.
  
#### <a name="understanding-rss-performance"></a>Descripción del rendimiento de RSS

La optimización de RSS requiere la descripción de la configuración y la lógica de equilibrio de carga. Para comprobar que se hayan aplicado la configuración de RSS, puede revisar los resultados cuando ejecutas la **Get NetAdapterRss** cmdlet de Windows PowerShell. La siguiente es el resultado del ejemplo de este cmdlet.
  
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

Además de repetir los parámetros que se han establecido, el aspecto clave de la salida es el resultado de la tabla de direccionamiento indirecto. La tabla de direccionamiento indirecto muestra los depósitos de tabla hash que se usan para distribuir el tráfico entrante. En este ejemplo, la notación n:c designa la K Numa-par de índice de grupo: CPU que se usa para dirigir el tráfico entrante. Vemos exactamente 2 entradas únicas (0:0 y 0:4), que representan k grupo 0/cpu0 y k grupo 0/cpu 4, respectivamente.

Hay solo un grupo de k de este sistema (k-group 0) y un n (donde n < = 128) entrada de la tabla de direccionamiento indirecto. Dado que el número de colas de recepción se establece en 2, procesadores solo 2 (0:0, 0:4) se eligen - aunque procesadores máximos se establece en 8. En efecto, en la tabla de direccionamiento indirecto se hash el tráfico entrante para que use solo 2 CPU fuera de los 8 que están disponibles.

Para utilizar completamente las CPU, el número de RSS recibir colas debe ser igual o mayor que Max procesadores. En el ejemplo anterior, la cola de recibir debe establecerse en 8 o superior.

#### <a name="nic-teaming-and-rss"></a>El equipo NIC y RSS

RSS puede estar habilitado en un adaptador de red que está asociado con otra tarjeta de interfaz de red con el equipo NIC. En este escenario, solo el adaptador de red física subyacente puede configurarse para usar RSS. Un usuario no puede establecer los cmdlets RSS en el adaptador de red en equipo.
  
###  <a name="bkmk_rsc"></a>Recibir segmento fusión (RSC)

Recibir segmento fusión \(RSC\) ayuda a rendimiento al reducir el número de encabezados de IP que se procesan por una determinada cantidad de datos recibidos. Se debe usar para ayudar a escalar el rendimiento de los datos recibidos por \(or coalescing\) agrupar los paquetes más pequeños en unidades más grandes.

Este enfoque puede afectar a la latencia con ventajas mayoritariamente aumento en el rendimiento. Se recomienda RSC para aumentar el rendimiento para las cargas de trabajo recibidos. Considera implementar los adaptadores de red que admiten RSC. 

En estos adaptadores de red, asegúrese de que está RSC en \ (este es el establecer\ predeterminado), a menos que tengas cargas de trabajo específicas \ (por ejemplo, baja latencia, networking\ bajo rendimiento) que mostrar se benefician RSC está desactivado.

#### <a name="understanding-rsc-diagnostics"></a>Descripción de diagnósticos RSC

Puede diagnosticar RSC mediante los cmdlets de Windows PowerShell **Get NetAdapterRsc** y **Get NetAdapterStatistics**.

Siguiente es el resultado del ejemplo cuando se ejecuta el cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

La **obtener** cmdlet muestra si RSC está habilitado en la interfaz y si TCP permite RSC en un estado de funcionamiento. El motivo del error proporciona detalles sobre los errores para habilitar RSC en dicha interfaz.

En el escenario anterior, IPv4 RSC está operativa y compatibles en la interfaz. Para comprender los errores de diagnósticos, se pueden ver los bytes del fusionadas o excepciones producidas. Esto proporciona una indicación de los problemas de uso combinados.

Siguiente es el resultado del ejemplo cuando se ejecuta el cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC y la virtualización

RSC solo se admite en el host físico cuando el adaptador de red de host no está enlazado al modificador virtuales Hyper-V. Cuando el host está enlazado al modificador virtuales Hyper-V, RSC está deshabilitado por el sistema operativo. Además, máquinas virtuales no obtener la ventaja de RSC RSC no admiten adaptadores de red virtual.

Cuando se habilita la virtualización de entrada y salida de raíz solo \(SR-IOV\), RSC puede habilitarse para una máquina virtual. En este caso, las funciones virtuales admiten funcionalidad RSC; por lo tanto, máquinas virtuales también reciben la ventaja de RSC.

##  <a name="bkmk_resources"></a>Recursos de adaptador de red

Algunos adaptadores de red activamente administración sus recursos para lograr un rendimiento óptimo. Varios adaptadores de red te permiten configurar manualmente los recursos mediante la **avanzada redes** ficha para el adaptador. Para estos adaptadores, puedes establecer los valores de un número de parámetros, incluido el número de búferes de recepción y enviar búferes.

Se ha simplificado la configuración de recursos de adaptador de red mediante el uso de los siguientes cmdlets de Windows PowerShell.

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

Para obtener más información, consulta [Cmdlets del adaptador de red en Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Para obtener vínculos a todos los temas de esta guía, consulte [optimización del rendimiento de red subsistema](net-sub-performance-top.md).