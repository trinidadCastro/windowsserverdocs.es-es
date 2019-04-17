---
title: Adaptadores de red de optimización de rendimiento
description: Este tema es parte de la Guía de optimización del rendimiento de red subsistema de Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1c6d36966ce6e2d407b6568e16946745256ee69b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tuning-network-adapters"></a>Adaptadores de red de optimización de rendimiento

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para adaptadores de red de ajuste de rendimiento que están instalados en equipos que ejecutan Windows Server 2016.

Determinar la configuración de ajuste correcta para el adaptador de red dependen de las siguientes variables:

- El adaptador de red y su conjunto de características  

- El tipo de carga de trabajo realizada por el servidor  

- Los recursos de hardware y software de servidor  

- Los objetivos de rendimiento para el servidor  

Si el adaptador de red proporciona opciones de optimización, puede optimizar el uso de rendimiento y los recursos de red para lograr un rendimiento óptimo en función de los parámetros que se describió anteriormente.  

Las siguientes secciones describen algunas de las opciones de optimización del rendimiento.  

##  <a name="bkmk_offload"></a>Habilitación de características de descarga

Activar características de descarga de adaptador de red es muy conveniente. A veces, sin embargo, el adaptador de red no es lo suficientemente eficaz para controlar las funcionalidades de la descarga con alto rendimiento.

>[!IMPORTANT]
>No uses las características de descarga **descarga de la tarea de IPsec** o **descarga Chimenea TCP **. Estas tecnologías están en desuso en Windows Server 2016 y podrían afectar negativamente a servidor y el rendimiento de la red. Además, estas tecnologías podrían no ser compatible con Microsoft en el futuro.

Por ejemplo, lo que permite la descarga de segmentación puede reducir el rendimiento máximo sostenible en algunos adaptadores de red debido a los recursos de hardware limitados. Sin embargo, si el rendimiento reducido no se espera que sea una limitación, debes habilitar funcionalidades de la descarga, incluso para este tipo de adaptador de red.

>[!NOTE]
> Algunos adaptadores de red requieren características de descarga para habilitar por separado para enviar y recibir las rutas de acceso.

##  <a name="bkmk_rss_web"></a>Habilitar recibir lado ajuste de escala (RSS) para los servidores Web

RSS puede mejorar el rendimiento y la escalabilidad de web cuando haya menos adaptadores de red que los procesadores lógicos en el servidor. Cuando todo el tráfico de web va a través de los adaptadores de red compatibles con RSS, las solicitudes web entrantes de conexiones diferentes pueden procesarse simultáneamente a través de CPU diferentes.

Es importante tener en cuenta que debido a la lógica de RSS y transferencia de hipertexto protocolo \(HTTP\) para la distribución de carga, su rendimiento puede ser gravemente degradarse si un adaptador de red compatibles con RSS acepta el tráfico de la web en un servidor que tiene uno o varios adaptadores de red compatibles con RSS. En este caso, debes usar adaptadores de red compatibles con RSS o deshabilitar RSS en las propiedades del adaptador de red **propiedades avanzadas** pestaña. Para determinar si un adaptador de red es capaz de RSS, puede ver la información de RSS en las propiedades del adaptador de red **propiedades avanzadas** pestaña.

### <a name="rss-profiles-and-rss-queues"></a>Perfiles RSS y colas RSS

El perfil RSS predefinidos predeterminado es NUMA estático, lo que modifica el comportamiento predeterminado de las versiones anteriores del sistema operativo. Para comenzar con los perfiles de RSS, puedes revisar los perfiles disponibles para comprender cuando están útil y cómo se aplican a tu entorno de red y el hardware.

Por ejemplo, si puedes abrir el Administrador de tareas y revisión los procesadores lógicos en el servidor y parecen estar infrautilizado para recibir tráfico, puede intentar aumentar el número de colas RSS del valor predeterminado de 2 en la medida en que es compatible con el adaptador de red. El adaptador de red podría tener opciones para cambiar el número de colas RSS como parte del controlador.

##  <a name="bkmk_resources"></a>Aumentar los recursos de adaptador de red

Para los adaptadores de red que permiten la configuración manual de recursos, por ejemplo, recibirán y enviar búferes, deberá aumentar los recursos asignados. 

Algunos adaptadores de red establece los búferes de recepción bajo en conservar asignada memoria del host. El valor inferior resultado paquetes descartados y una disminución del rendimiento. Por lo tanto, para escenarios de uso intensivo recibir, te recomendamos que aumente el valor del búfer de recepción al máximo.

>[!NOTE]
>Si un adaptador de red no expone la configuración manual de recursos, ya sea dinámicamente configura los recursos o los recursos se establecen en un valor fijo que no se puede cambiar.

### <a name="enabling-interrupt-moderation"></a>Habilitar moderación de interrupción

Para controlar moderación de interrupción, algunos adaptadores de red exponer niveles de moderación de interrupción diferentes, parámetros de uso combinados de búfer (a veces por separado para enviar y recibir búferes), o ambos.

Considera moderación de interrupción para cargas de trabajo de CPU enlazado y tener en cuenta el equilibrio entre el ahorro de CPU de host y la latencia en comparación con el host mayor ahorro de CPU debido a interrupciones de más y menor latencia. Si el adaptador de red no lleva a cabo moderación de interrupción, pero expone búfer fusión, aumentar el número de búferes fusionados permite búferes más por enviar o recibe, lo que mejora el rendimiento.

##  <a name="bkmk_low"></a>Ajustes de procesamiento de paquetes de latencia baja de rendimiento

Muchos adaptadores de red proporcionan opciones para optimizar la latencia provocado por el sistema operativo. Latencia es el tiempo transcurrido entre el controlador de red procesar un paquete entrante y enviar el paquete de nuevo el controlador de red. Normalmente, este tiempo se mide en microsegundos. Para fines de comparación, el tiempo de transmisión de transmisiones de paquetes largas distancias normalmente se mide en milisegundos \ (un orden de magnitud larger\). Este ajuste no reducirá el tiempo que pasa un paquete en tránsito.

Los siguientes son algunas sugerencias para redes microsegundos confidenciales de optimización del rendimiento.

- Establece el BIOS del equipo en **alto rendimiento**, con los Estados C deshabilitados. Sin embargo, ten en cuenta que se trata de sistema y el BIOS dependientes, y algunos sistemas proporcionará un rendimiento superior si la administración de energía de los controles del sistema operativo. Puedes comprobar y ajustar la configuración de administración de energía de **configuración** o mediante la **powercfg** comando. Para obtener más información, consulta [opciones de línea de comandos de Powercfg](https://technet.microsoft.com/library/cc748940.aspx)

- Establece el perfil de administración de energía de sistema operativo en **sistema de alto rendimiento**. Ten en cuenta que esto no funcionará correctamente si se ha establecido el BIOS del sistema para deshabilitar el control del sistema operativo de administración de energía.

- Habilitar la descarga estática, por ejemplo, sumas de comprobación de UDP, TCP sumas de comprobación y enviar grande que la descarga (también).

- Habilitar RSS si el tráfico multi en secuencias, como la recepción de multidifusión de gran volumen.

-   Deshabilitar la **interrumpir moderación** configuración para que los controladores de tarjeta de red que requieren la menor latencia posible. Recuerda que esto puede usar más de tiempo de CPU y representa un equilibrio.

- Controlar DPC e interrupciones del adaptador de red en un procesador de núcleo que comparte memoria caché de CPU con el principal que se está usando el programa (subproceso del usuario) que controla el paquete. Optimización de afinidad de CPU puede usarse para dirigir a un proceso para determinados procesadores lógicos junto con la configuración de RSS para lograr esto. Usar el mismo núcleo para el subproceso de modo de interrupción, DPC y usuario exhibe peor rendimiento a medida que aumenta la carga porque la ISR y DPC subprocesos compiten por el uso de las principales.

##  <a name="bkmk_smi"></a>Interrupciones de administración del sistema

Muchos sistemas de hardware usan \(SMI\) interrupciones de administración de sistema para una variedad de funciones de mantenimiento, incluidos los informes de errores de memoria \(ECC\) de código de corrección de errores, compatibilidad USB, control de ventilador y BIOS heredado controla la administración de energía. 

La SMI es la interrupción de prioridad más alta en el sistema y coloca la CPU en un modo de administración, que se adelanta todas las otras actividades mientras se ejecuta una rutina de servicio de interrupción, suele estar contenida en el BIOS.

Por desgracia, esto puede provocar picos de latencia de 100 microsegundos o más. 

Si necesitas lograr la latencia más baja, debe solicitar una versión de BIOS del proveedor de hardware que reduce SMIs hasta el grado más baja posible. Con frecuencia se conocen como "baja latencia BIOS" o "SMI gratuito." En algunos casos, no es posible para una plataforma de hardware eliminar la actividad SMI por completo porque se usa para controlar las funciones esenciales (por ejemplo, ventiladores).

>[!NOTE]
>El sistema operativo no puede ejercer ningún control sobre SMIs porque el procesador lógico se está ejecutando en un modo especial de mantenimiento, lo que impide que la intervención del sistema operativo.

##  <a name="bkmk_tcp"></a>TCP de optimización del rendimiento

 Puedes optimizar el rendimiento TCP con los siguientes elementos.

###  <a name="bkmk_tcp_params"></a>Ajuste automático de la ventana de recepción de TCP

Antes de Windows Server 2008, la pila de red usa una ventana de lado de recepción de tamaño fijo (65.535 bytes) que limita el rendimiento general posible para las conexiones. Uno de los cambios más importantes de la pila TCP es TCP recibir ajuste automático de la ventana. 

Puede calcular el rendimiento de una única conexión total cuando usas un tamaño fijo TCP recibir ventana como:

**Rendimiento posible total de bytes = TCP recibir el tamaño de ventana en bytes \ * (1 / latencia de conexión en segundos)**

Por ejemplo, el rendimiento puede alcanzarse total es solo 51 Mbps en una conexión con la latencia de 10 ms \ (un valor razonable para infrastructure\ de una red corporativa grande). 

Con el ajuste automático, sin embargo, la ventana de recepción ajustable, y puede crecer para satisfacer las necesidades del remitente. Es posible que una conexión lograr una velocidad de la línea completa de una conexión GB/s 1. Escenarios de uso de red que es posible que han estado limitados en el pasado por el rendimiento puede alcanzarse total de las conexiones TCP ahora pueden usar completamente la red.

#### <a name="deprecated-tcp-parameters"></a>Parámetros TCP desusados

La siguiente configuración del registro de Windows Server 2003 ya no es compatibles y se omite en versiones posteriores.

Todas estas configuraciones tenían la ubicación del registro siguiente:

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a>Plataforma de filtrado de Windows

La plataforma de filtrado de Windows (WFP) que se introdujo en Windows Vista y Windows Server 2008 proporciona API a los proveedores no son de Microsoft de software independientes (ISV) para crear filtros de procesamiento de paquetes. Algunos ejemplos incluyen firewall y antivirus.

>[!NOTE]
>Un filtro WFP mal escrito puede reducir considerablemente el rendimiento de red del servidor. Para obtener más información, consulta [procesamiento de paquetes de migración de controladores y aplicaciones WFP](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) en el centro de desarrollo de Windows.


Para obtener vínculos a todos los temas de esta guía, consulte [optimización del rendimiento de red subsistema](net-sub-performance-top.md).
