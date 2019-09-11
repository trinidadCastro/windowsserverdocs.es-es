---
title: Adaptadores de red de optimización de rendimiento
description: Este tema forma parte de la guía de optimización del rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a0d87f2e8c9b3ca7581d20009cd9c0e030241270
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871895"
---
# <a name="performance-tuning-network-adapters"></a>Adaptadores de red de optimización de rendimiento

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para optimizar el rendimiento de los adaptadores de red instalados en equipos que ejecutan Windows Server 2016.

Determinar la configuración de ajuste correcta para un adaptador de red depende de las siguientes variables:

- El adaptador de red y su conjunto de características  

- El tipo de carga de trabajo que realiza el servidor  

- Los recursos de hardware y software del servidor  

- Los objetivos de rendimiento para el servidor  

Si el adaptador de red ofrece opciones de ajuste, puedes optimizar la capacidad de proceso de la red y el uso de los recursos para lograr una capacidad de proceso óptima en función de los parámetros indicados.  

En las siguientes secciones se describen algunas de las opciones de ajuste del rendimiento.  

##  <a name="bkmk_offload"></a>Habilitar las características de descarga

Activar las características de descarga del adaptador de red suele ser beneficioso. Sin embargo, algunas veces el adaptador de red no tiene suficiente capacidad para administrar las funcionalidades de descarga con una capacidad de proceso alta.

>[!IMPORTANT]
>No use las características de descarga descarga de **tareas de IPSec** o descarga de **TCP Chimney**. Estas tecnologías están desusadas en Windows Server 2016 y pueden afectar negativamente al rendimiento del servidor y de la red. Además, es posible que Microsoft no admita estas tecnologías en el futuro.

Por ejemplo, habilitar la descarga de segmentación puede reducir la capacidad de proceso máxima sostenible en algunos adaptadores de red debido a la limitación de los recursos de hardware. Sin embargo, si no se espera que la reducción de la capacidad de proceso sea una limitación, debes habilitar las características de descarga, incluso para este tipo de adaptador de red.

>[!NOTE]
> Algunos adaptadores de red necesitan que las características de descarga se habiliten de forma independiente para las rutas de envío y recepción.

##  <a name="bkmk_rss_web"></a>Habilitar el ajuste de escala en lado de recepción (RSS) para servidores Web

RSS puede mejorar la escalabilidad y el rendimiento web cuando hay menos adaptadores de red que procesadores lógicos en el servidor. Cuando todo el tráfico de la red pasa por adaptadores de red compatibles con RSS, las solicitudes web que llegan de diferentes conexiones se pueden procesar simultáneamente en diferentes CPU.

Es importante tener en cuenta que, debido a la lógica en RSS y el protocolo \(de\) transferencia de hipertexto http para la distribución de la carga, el rendimiento podría disminuir gravemente si un adaptador de red que no es compatible con RSS acepta tráfico web en un servidor que tiene uno o más adaptadores de red compatibles con RSS. En este caso, debes usar adaptadores de red compatibles con RSS o deshabilitar RSS en la pestaña **Propiedades avanzadas** de las propiedades del adaptador de red. Para determinar si un adaptador de red es compatible con RSS, puedes ver la información de RSS en la pestaña **Propiedades avanzadas** de las propiedades del adaptador de red.

### <a name="rss-profiles-and-rss-queues"></a>Perfiles RSS y colas RSS

El perfil predefinido de RSS predeterminado es NUMA estático, que cambia el comportamiento predeterminado de las versiones anteriores del sistema operativo. Para comenzar con los perfiles de RSS, puedes revisar los perfiles disponibles para saber cuándo son beneficiosos y cómo se aplican a tu entorno y hardware de red.

Por ejemplo, si abres el Administrador de tareas, revisas los procesadores lógicos de tu servidor y parecen estar infrautilizados para la recepción de tráfico, puedes intentar aumentar el número de colas RSS de las 2 predeterminadas al máximo que admita tu adaptador de red. Quizás el adaptador de red tenga opciones para cambiar el número de colas RSS como parte del controlador.

##  <a name="bkmk_resources"></a>Aumentar los recursos del adaptador de red

Para los adaptadores de red que permiten configurar los recursos manualmente, por ejemplo, los búferes de envío y recepción, debes aumentar los recursos asignados. 

Algunos adaptadores de red establecen un valor bajo para los búferes de recepción para conservar la memoria asignada del host. El valor bajo produce la pérdida de paquetes y un menor rendimiento. Por lo tanto, en escenarios con un alto volumen de recepción, te recomendamos que aumentes el valor del búfer de recepción al máximo.

>[!NOTE]
>Si un adaptador de red no expone la configuración de recursos manual, configura los recursos dinámicamente o están establecidos en un valor fijo que no se puede cambiar.

### <a name="enabling-interrupt-moderation"></a>Habilitar la moderación de interrupciones

Para controlar la moderación de interrupciones, algunos adaptadores de red ofrecen diferentes niveles de moderación de interrupciones, parámetros de fusión de búferes (algunas veces por separado para los búferes de envío y recepción) o ambos.

Debes considerar el uso de moderación de interrupciones para cargas de trabajo ligadas a la CPU, y tener en cuenta el equilibrio entre latencia y ahorro de CPU host frente al mayor ahorro de CPU host debido al mayor número de interrupciones y la menor latencia. Si el adaptador de red no realiza moderación de interrupciones, pero expone fusión de búferes, aumentar el número de búferes fusionados permite más búferes por envío o recepción, lo que mejora el rendimiento.

##  <a name="bkmk_low"></a>Optimización del rendimiento para el procesamiento de paquetes de baja latencia

Muchos adaptadores de red ofrecen opciones para optimizar la latencia inducida por el sistema operativo. La latencia es el tiempo que transcurre desde que el controlador de red procesa un paquete de entrada hasta que lo envía de vuelta. Este tiempo suele medirse en microsegundos. Para la comparación, el tiempo de transmisión de las transmisiones de paquetes a través de distancias largas normalmente \(se mide en milisegundos\)en un orden de magnitud mayor. Este ajuste no reducirá el tiempo que un paquete está en tránsito.

Las siguientes son algunas sugerencias de ajuste para redes con una sensibilidad de microsegundos.

- Establece el BIOS del equipo en **Alto rendimiento**, con C-states deshabilitado. Sin embargo, ten en cuenta que esto depende del BIOS y del sistema, y que algunos sistemas proporcionarán un rendimiento mayor si el sistema operativo controla la administración de la energía. Puede comprobar y ajustar la configuración de administración de energía desde la **configuración** o mediante el comando **powercfg** . Para obtener más información, vea [Opciones de la línea de comandos de powercfg](https://technet.microsoft.com/library/cc748940.aspx) .

- Establece el perfil de administración de energía del sistema operativo en **Sistema de alto rendimiento**. Ten en cuenta que esto no funcionará correctamente si el BIOS del sistema se ha establecido para deshabilitar el control de la administración de energía en el sistema operativo.

- Habilita Static Offloads, por ejemplo, UDP Checksums, TCP Checksums y Send Large Offload (LSO).

- Habilita RSS si el tráfico se transmite en múltiples secuencias, por ejemplo, en la recepción de multidifusión de gran volumen.

-   Deshabilita la opción **Moderación de interrupciones** para los controladores de tarjetas de red que requieran la menor latencia posible. Recuerda que esto puede usar más tiempo de CPU y que supone una contrapartida.

- Administra las interrupciones y DPC de los adaptadores de red en un procesador de núcleo que comparta la memoria caché de la CPU con el núcleo usado por el programa (subproceso de usuario) que está administrando el paquete. Para ello, se puede usar el ajuste de la afinidad de la CPU para dirigir un proceso a determinados procesadores lógicos junto con la configuración de RSS. Cuando se usa el mismo núcleo para la interrupción, el DPC y el subproceso del modo de usuario, el rendimiento es menor porque la carga aumenta debido a que el ISR, DPC y el subproceso luchan por usar el núcleo.

##  <a name="bkmk_smi"></a>Interrupciones de administración del sistema

Muchos sistemas de hardware usan las interrupciones \(de\) la administración del sistema SMI para diversas funciones de mantenimiento, como la \(creación\) de informes de errores de memoria ECC de código de corrección de errores, compatibilidad con USB heredada, ventilador control y administración de energía controlada por BIOS. 

La SMI es la interrupción de mayor prioridad en el sistema y pone la CPU en un modo de administración, que impide cualquier otra actividad mientras ejecuta una rutina de servicio de interrupción, normalmente contenida en el BIOS.

Lamentablemente, esto puede producir picos de latencia de 100 microsegundos o más. 

Si necesitas lograr la menor latencia, debes solicitar a tu proveedor de hardware una versión del BIOS que reduzca los SMI al mínimo posible. Estas suelen denominarse "BIOS de baja latencia" o "BIOS sin SMI". En algunos casos, en una plataforma de hardware no se puede eliminar la actividad de SMI por completo porque se usa para controlar funciones esenciales (por ejemplo, los ventiladores de refrigeración).

>[!NOTE]
>El sistema operativo puede no ejercer ningún control sobre las SMI porque el procesador lógico se está ejecutando en un modo de mantenimiento especial que impide la intervención del sistema operativo.

##  <a name="bkmk_tcp"></a>Ajuste del rendimiento de TCP

 Puedes ajustar el rendimiento de TCP usando los siguientes elementos.

###  <a name="bkmk_tcp_params"></a>Ajuste automático de la ventana de recepción de TCP

Antes de Windows Server 2008, la pila de red usaba una ventana de recepción de tamaño fijo (65.535 bytes) que limitaba el rendimiento total potencial de las conexiones. Uno de los cambios más significativos de la pila TCP es el ajuste automático de la ventana de recepción de TCP. 

Puede calcular el rendimiento total de una sola conexión cuando use una ventana de recepción TCP de tamaño fijo como:

**Rendimiento total factible en bytes = tamaño de la ventana de recepción TCP en \* bytes (1/latencia de conexión en segundos)**

Por ejemplo, el rendimiento total que se alcanza es solo de 51 Mbps en una conexión con una \(latencia de 10 ms de un valor razonable para\)una infraestructura de red corporativa de gran tamaño. 

Sin embargo, con el ajuste automático, la ventana del lado de recepción es ajustable y puede crecer para atender las demandas del remitente. Es posible que una conexión alcance una velocidad de línea completa de una conexión de 1 Gbps. Los escenarios de uso de red que podrían haber estado limitados en el pasado por la capacidad de proceso total posible de las conexiones TCP, ahora pueden usar completamente la red.

#### <a name="deprecated-tcp-parameters"></a>Parámetros TCP desusados

La siguiente configuración del registro de Windows Server 2003 ya no se admite y se omiten en versiones posteriores.

Todos estos valores tenían la siguiente ubicación del registro:

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a>Plataforma de filtrado de Windows

La plataforma de filtrado de Windows (WFP) que se presentó en Windows Vista y Windows Server 2008 proporciona API a proveedores de software independientes (ISV) que no son de Microsoft para crear filtros de procesamiento de paquetes. Algunos ejemplos son firewall y software antivirus.

>[!NOTE]
>Un filtro WFP mal escrito puede reducir significativamente el rendimiento de red de un servidor. Para obtener más información, vea [portar controladores de procesamiento de paquetes y aplicaciones a WFP](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) en el centro de desarrollo de Windows.


Para obtener vínculos a todos los temas de esta guía, consulte [ajuste del rendimiento del subsistema de red](net-sub-performance-top.md).
