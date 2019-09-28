---
title: Elección de un adaptador de red
description: Este tema forma parte de la guía de optimización del rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cf93e6f91f4a1c21050c7ad1cb4de43258be1a65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401855"
---
# <a name="choosing-a-network-adapter"></a>Elección de un adaptador de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre algunas de las características de los adaptadores de red que pueden afectar a las opciones de compra.

Las aplicaciones que consumen muchos recursos de red requieren adaptadores de red de alto rendimiento. En esta sección se analizan algunas consideraciones para elegir los adaptadores de red, así como cómo configurar diferentes opciones del adaptador de red para lograr el mejor rendimiento de la red.

> [!TIP]
>  Puede establecer la configuración del adaptador de red mediante Windows PowerShell. Para obtener más información, consulte [cmdlets de adaptador de red en Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a>Funcionalidades de descarga

La descarga de tareas de la CPU \(\) de la unidad de procesamiento central en el adaptador de red puede reducir el uso de CPU en el servidor, lo que mejora el rendimiento global del sistema.

La pila de red de los productos de Microsoft puede descargar una o más tareas en un adaptador de red si selecciona un adaptador de red que tenga las capacidades de descarga adecuadas. En la tabla siguiente se proporciona una breve descripción de las distintas funcionalidades de descarga disponibles en Windows Server 2016.
  
|Tipo de descarga|Descripción|
|------------------|-----------------|  
|Cálculo de la suma de comprobación para TCP|La pila de red puede descargar el cálculo y la validación de sumas\) de comprobación TCP del protocolo \(de control de transmisión en rutas de acceso del código de envío y recepción. También puede descargar el cálculo y la validación de las sumas de comprobación de IPv4 e IPv6 en las rutas de acceso del código de envío y recepción.|  
|Cálculo de la suma de comprobación para UDP |La pila de red puede descargar el cálculo y la validación de las \(sumas de comprobación UDP\) del Protocolo de datagramas de usuario en las rutas de acceso del código de envío y recepción.|
|Cálculo de suma de comprobación para IPv4 |La pila de red puede descargar el cálculo y la validación de las sumas de comprobación IPv4 en las rutas de acceso del código de envío y recepción. |
|Cálculo de la suma de comprobación para IPv6 |La pila de red puede descargar el cálculo y la validación de las sumas de comprobación IPv6 en las rutas de acceso del código de envío y recepción. | 
|Segmentación de paquetes TCP grandes|La capa de transporte TCP/IP admite la descarga de envío de gran tamaño V2 (LSOv2). Con LSOv2, la capa de transporte TCP/IP puede descargar la segmentación de paquetes TCP grandes en el adaptador de red.|  
|RSS de ajuste \(de escala en lado de recepción\)|RSS es una tecnología de controlador de red que permite la distribución eficaz del procesamiento de recepción de red entre varias CPU en sistemas multiprocesador. Más adelante en este tema se proporciona más información sobre RSS.|  
|\(RSC de fusión de segmentos de recepción\)|RSC es la capacidad de agrupar paquetes para minimizar el procesamiento de encabezados necesario para que el host realice la ejecución. Un máximo de 64 KB de carga útil recibida puede combinarse en un único paquete mayor para su procesamiento. Más adelante en este tema se proporciona más información sobre RSC.|  
  
###  <a name="bkmk_rss"></a>Ajuste de escala en lado de recepción

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 y Windows Server 2008 admiten el escalado \(en\)lado de recepción RSS. 

Algunos servidores se configuran con varios procesadores lógicos que \(comparten recursos de hardware,\) como un núcleo físico, y que se tratan como \(elementos\) del mismo nivel de SMT multiproceso simultáneos. La tecnología Hyper-Threading de Intel es un ejemplo. RSS dirige el procesamiento de la red a un procesador lógico por núcleo. Por ejemplo, en un servidor con la tecnología Hyper-Threading de Intel, 4 núcleos y 8 procesadores lógicos, RSS no usa más de 4 procesadores lógicos para el procesamiento de la red.  

RSS distribuye paquetes de e/s de red entrantes entre procesadores lógicos para que los paquetes que pertenecen a la misma conexión TCP se procesen en el mismo procesador lógico, lo que conserva el orden. 

RSS también equilibra la carga del tráfico de unidifusión y multidifusión UDP y enruta los \(flujos relacionados que se determinan mediante el hash de\) las direcciones de origen y de destino en el mismo procesador lógico, conservando el orden de las llegadas relacionadas. Esto ayuda a mejorar la escalabilidad y el rendimiento de los escenarios de recepción intensiva para los servidores que tienen menos adaptadores de red que los procesadores lógicos que cumplen los posibles. 

#### <a name="configuring-rss"></a>Configuración de RSS

En Windows Server 2016, puede configurar RSS con los cmdlets de Windows PowerShell y los perfiles de RSS. 

Puede definir perfiles RSS mediante el parámetro **– Profile** del cmdlet **set-NetAdapterRss** de Windows PowerShell.

**Comandos de Windows PowerShell para la configuración de RSS**

Los siguientes cmdlets le permiten ver y modificar los parámetros de RSS por adaptador de red.
  
>[!NOTE]
>Para obtener una referencia de comando detallada para cada cmdlet, incluida la sintaxis y los parámetros, puede hacer clic en los vínculos siguientes. Además, puede pasar el nombre del cmdlet a **Get-Help** en el símbolo del sistema de Windows PowerShell para obtener más información sobre cada comando.  

- [Disable-NetAdapterRss](https://technet.microsoft.com/library/jj130892). Este comando deshabilita RSS en el adaptador de red que especifique.

- [Enable-NetAdapterRss](https://technet.microsoft.com/library/jj130859). Este comando habilita RSS en el adaptador de red que especifique.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Este comando recupera las propiedades RSS del adaptador de red que especifique.
  
- [Set-NetAdapterRss](https://technet.microsoft.com/library/jj130863). Este comando establece las propiedades de RSS en el adaptador de red que especifique.  

#### <a name="rss-profiles"></a>Perfiles de RSS

Puede usar el parámetro **– Profile** del cmdlet Set-NetAdapterRss para especificar los procesadores lógicos que se asignan a cada adaptador de red. Los valores disponibles para este parámetro son:

- **Más cercano**. Se prefieren los números de procesador lógico que están cerca del procesador RSS base del adaptador de red. Con este perfil, el sistema operativo puede reequilibrar los procesadores lógicos de forma dinámica en función de la carga.
  
- **ClosestStatic**. Se prefieren los números de procesador lógico cerca del procesador RSS base del adaptador de red. Con este perfil, el sistema operativo no reequilibra los procesadores lógicos de forma dinámica en función de la carga.
  
- **NUMA**. Los números de procesador lógico se suelen seleccionar en nodos NUMA diferentes para distribuir la carga. Con este perfil, el sistema operativo puede reequilibrar los procesadores lógicos de forma dinámica en función de la carga.
  
- **NUMAStatic**. Este es el **Perfil predeterminado**. Los números de procesador lógico se suelen seleccionar en nodos NUMA diferentes para distribuir la carga. Con este perfil, el sistema operativo no volverá a equilibrar los procesadores lógicos de forma dinámica en función de la carga.

- **Conservador**. RSS usa la menor cantidad de procesadores posible para admitir la carga. Esta opción ayuda a reducir el número de interrupciones.

En función del escenario y de las características de la carga de trabajo, también puede usar otros parámetros del cmdlet **set-NetAdapterRss** de Windows PowerShell para especificar lo siguiente:

- En cada adaptador de red, el número de procesadores lógicos que se pueden usar para RSS.
- Desplazamiento inicial para el intervalo de procesadores lógicos.
- Nodo desde el que el adaptador de red asigna memoria.

A continuación se muestran los parámetros **set-NetAdapterRss** adicionales que puede usar para configurar RSS:

>[!NOTE]
>En la sintaxis de ejemplo para cada parámetro siguiente, el nombre de adaptador de red **Ethernet** se usa como un valor de ejemplo para el parámetro **– Name** del comando **set-NetAdapterRss** . Al ejecutar el cmdlet, asegúrese de que el nombre del adaptador de red que usa es adecuado para su entorno.

- **MaxProcessors\*** : Establece el número máximo de procesadores RSS que se van a utilizar. Esto garantiza que el tráfico de la aplicación está enlazado a un número máximo de procesadores en una interfaz determinada. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **BaseProcessorGroup\*** : Establece el grupo de procesadores base de un nodo NUMA. Esto afecta a la matriz de procesadores que usa RSS. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **MaxProcessorGroup\*** : Establece el grupo de procesadores máximo de un nodo NUMA. Esto afecta a la matriz de procesadores que usa RSS. Si se establece, se restringirá un grupo de procesadores máximo para que el equilibrio de carga se alinee dentro de un grupo k. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **BaseProcessorNumber\*** : Establece el número de procesador base de un nodo NUMA. Esto afecta a la matriz de procesadores que usa RSS. Esto permite la creación de particiones en los adaptadores de red. Este es el primer procesador lógico del intervalo de procesadores RSS que se asigna a cada adaptador. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **NumaNode\*** : Nodo NUMA desde el que cada adaptador de red puede asignar memoria. Puede estar dentro de un grupo k o de distintos grupos k. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **NumberofReceiveQueues\*** : Si parece que los procesadores lógicos están infrautilizados para recibir tráfico \(, por ejemplo, como se ve en el administrador\)de tareas, puede intentar aumentar el número de colas RSS desde el valor predeterminado de 2 hasta el máximo admitido por el adaptador de red. . Es posible que el adaptador de red tenga opciones para cambiar el número de colas de RSS como parte del controlador. Sintaxis de ejemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Para obtener más información, haga clic en el siguiente [vínculo para descargar redes escalables: Eliminar el cuello de botella de procesamiento de](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) recepción: Introducción a RSS en formato de Word.
  
#### <a name="understanding-rss-performance"></a>Descripción del rendimiento de RSS

La optimización de RSS requiere la comprensión de la configuración y la lógica de equilibrio de carga. Para comprobar que la configuración de RSS ha surtido efecto, puede revisar la salida al ejecutar el cmdlet de Windows PowerShell **Get-NetAdapterRss** . A continuación se muestra un resultado de ejemplo de este cmdlet.
  
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

Además de los parámetros de eco que se establecieron, el aspecto clave de la salida es la salida de la tabla de direccionamiento indirecto. La tabla de direccionamiento indirecto muestra los cubos de la tabla hash que se utilizan para distribuir el tráfico entrante. En este ejemplo, la notación n:c designa el par de índice de CPU de Numa K-Group, que se usa para dirigir el tráfico entrante. Vemos exactamente dos entradas únicas (0:0 y 0:4), que representan el grupo k 0/cpu0 y el grupo k 0/CPU 4, respectivamente.

Solo hay un grupo k para este sistema (k-Group 0) y un n (donde n < = 128) entrada de la tabla de direccionamiento indirecto. Dado que el número de colas de recepción está establecido en 2, solo se eligen 2 procesadores (0:0, 0:4), aunque el número máximo de procesadores está establecido en 8. En efecto, la tabla de direccionamiento indirecto está aplicando un algoritmo hash al tráfico entrante para usar solo 2 CPU de los 8 que están disponibles.

Para utilizar las CPU por completo, el número de colas de recepción RSS debe ser igual o mayor que el número máximo de procesadores. En el ejemplo anterior, la cola de recepción debe establecerse en 8 o más.

#### <a name="nic-teaming-and-rss"></a>Formación de equipos NIC y RSS

RSS se puede habilitar en un adaptador de red que esté agrupado con otra tarjeta de interfaz de red mediante la formación de equipos NIC. En este escenario, solo se puede configurar el adaptador de red físico subyacente para usar RSS. Un usuario no puede establecer cmdlets de RSS en el adaptador de red agrupado.
  
###  <a name="bkmk_rsc"></a>Fusión de segmentos de recepción (RSC)

El \(RSC\) de fusión de segmentos de recepción ayuda al rendimiento al reducir el número de encabezados IP que se procesan para una cantidad determinada de datos recibidos. Debe usarse para ayudar a escalar el rendimiento de los datos recibidos mediante \(\) la agrupación o combinación de los paquetes más pequeños en unidades más grandes.

Este enfoque puede afectar a la latencia con las ventajas que se han descubierto principalmente en las mejoras de rendimiento. Se recomienda usar RSC para aumentar el rendimiento de las cargas de trabajo pesadas recibidas. Considere la posibilidad de implementar adaptadores de red compatibles con RSC. 

En estos adaptadores de red, asegúrese de que RSC está \(en esta es la configuración\)predeterminada, a menos \(que tenga cargas de trabajo específicas; por ejemplo, baja latencia\) , redes de bajo rendimiento que muestran la ventaja de que RSC está desactivada. .

#### <a name="understanding-rsc-diagnostics"></a>Descripción de diagnósticos de RSC

Puede diagnosticar RSC mediante los cmdlets de Windows PowerShell **Get-NetAdapterRsc** y **Get-NetAdapterStatistics**.

A continuación se muestra un resultado de ejemplo al ejecutar el cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

El cmdlet **Get** muestra si RSC está habilitado en la interfaz y si TCP permite que RSC esté en un estado operativo. La razón del error proporciona detalles sobre el error para habilitar RSC en esa interfaz.

En el escenario anterior, se admite y funciona el RSC IPv4 en la interfaz. Para comprender los errores de diagnóstico, puede ver los bytes combinados o las excepciones que se producen. Esto proporciona una indicación de los problemas de fusión.

A continuación se muestra un resultado de ejemplo al ejecutar el cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC y virtualización

RSC solo se admite en el host físico cuando el adaptador de red del host no está enlazado al conmutador virtual de Hyper-V. RSC está deshabilitado por el sistema operativo cuando el host está enlazado al conmutador virtual de Hyper-V. Además, las máquinas virtuales no obtienen la ventaja de RSC porque los adaptadores de red virtuales no admiten RSC.

RSC se puede habilitar para una máquina virtual cuando se habilita la virtualización \(de entrada/salida de raíz única SR-IOV.\) En este caso, las funciones virtuales admiten la funcionalidad de RSC; por lo tanto, las máquinas virtuales también obtienen la ventaja de RSC.

##  <a name="bkmk_resources"></a>Recursos de adaptador de red

Algunos adaptadores de red administran activamente sus recursos para lograr un rendimiento óptimo. Varios adaptadores de red permiten configurar los recursos manualmente mediante la ficha **redes avanzadas** del adaptador. Para estos adaptadores, puede establecer los valores de una serie de parámetros, incluido el número de búferes de recepción y búferes de envío.

La configuración de los recursos del adaptador de red se simplifica mediante el uso de los siguientes cmdlets de Windows PowerShell.

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

Para obtener más información, consulte [cmdlets de adaptador de red en Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste del rendimiento del subsistema de red](net-sub-performance-top.md).