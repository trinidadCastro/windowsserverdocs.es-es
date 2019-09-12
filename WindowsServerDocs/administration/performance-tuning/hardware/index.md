---
title: Consideraciones de rendimiento del hardware de servidor
description: Consideraciones de rendimiento del hardware de servidor para Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 01/08/2018
ms.openlocfilehash: f9c7f0e589980f7d985f165e318667ebe2e5d5c5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866748"
---
# <a name="server-hardware-performance-considerations"></a>Consideraciones de rendimiento del hardware de servidor

En la sección siguiente se muestran los elementos importantes que se deben tener en cuenta al elegir el hardware de servidor. Estas instrucciones pueden ayudar a eliminar los cuellos de botella de rendimiento que dificultan el rendimiento del servidor.

## <a name="processor-recommendations"></a>Recomendaciones de procesador

Elija procesadores de 64 bits para los servidores. Los procesadores de 64 bits tienen mucho más espacio de direcciones y son necesarios para Windows Server 2016. No se proporcionará ninguna edición de 32 bits del sistema operativo, pero las aplicaciones de 32 bits sí se ejecutarán en el sistema operativo Windows Server 2016 de 64 bits.

Para aumentar los recursos informáticos de un servidor, puede usar un procesador con núcleos de mayor frecuencia o puede aumentar la cantidad de núcleos de procesador. Si la CPU es el recurso limitante del sistema, un núcleo con una frecuencia de 2x por lo general ofrece una mayor mejora de rendimiento que dos núcleos con una frecuencia de 1x.

No se espera que varios núcleos brinden una escala lineal perfecta y el factor de escala puede ser incluso menor si está habilitada la tecnología Hyper-Threading, porque esta se basa en el uso compartido de los recursos del mismo núcleo físico.


>[!Important]
> Haga coincidir y escale la memoria y el subsistema de E/S con el rendimiento de la CPU y viceversa.

No compare las frecuencias de CPU de distintos fabricantes y generaciones de procesadores, porque la comparación puede ser un indicador de velocidad erróneo.

Para Hyper-V, asegúrese de que el procesador admita SLAT (traducción de direcciones de segundo nivel). Intel la implementa como tablas de páginas extendidas (EPT) y AMD, como tablas de páginas anidadas (NPT). Para comprobar que esta característica existe, use SystemInfo.exe en el servidor.

## <a name="cache-recommendations"></a>Recomendaciones de caché

Elija cachés de procesador L2 o L3 de gran tamaño. En arquitecturas más recientes, como Haswell o Skylake, hay una caché de último nivel (LLC) unificada o L4. Por lo general, las cachés más grandes brindan un mejor rendimiento y, a menudo, desempeñan un rol mayor que la frecuencia de CPU sin procesar.

## <a name="memory-ram-and-paging-storage-recommendations"></a>Recomendaciones de almacenamiento de paginación y memoria (RAM)

>[!Note] 
> Algunos sistemas pueden presentar un rendimiento de almacenamiento reducido al ejecutar una nueva instalación de Windows Server 2016 frente a Windows Server 2012 R2. Durante el desarrollo de Windows Server 2016 se realizaron una serie de cambios para mejorar la seguridad y confiabilidad de la plataforma. Algunos de esos cambios, como la habilitación de Windows Defender de manera predeterminada, dan como resultado rutas de acceso de E/S más largas, que pueden reducir el rendimiento de E/S en cargas de trabajo y patrones específicos. Microsoft no recomienda deshabilitar Windows Defender, ya que es una importante capa de protección para tus sistemas. 

Aumente la cantidad de RAM para cumplir con sus necesidades de memoria.
Cuando el equipo se queda sin memoria y necesita más de forma inmediata, Windows usa el espacio en disco duro para complementar la RAM del sistema a través de un procedimiento denominado paginación. Demasiada paginación degrada el rendimiento general del sistema.
Puede optimizar la paginación si usa estas instrucciones para la ubicación del archivo de paginación:
- Aísle el archivo de paginación en su propio dispositivo de almacenamiento o al menos asegúrese de que no comparte los mismos dispositivos de almacenamiento que otros archivos a los que se tiene acceso con frecuencia. Por ejemplo, coloque el archivo de paginación y los archivos del sistema operativo en unidades de disco físicas independientes.

- Coloque el archivo de paginación en una unidad que no tolere errores. Si se produce un error en el disco, es probable que el sistema se bloquee. Si pone el archivo de paginación en una unidad con tolerancia a errores, recuerde que los sistemas que toleran errores suelen ser más lentos para escribir datos, porque los escriben en distintas ubicaciones.

- Use varios discos o una matriz de discos si necesita ancho de banda de disco adicional para la paginación. No coloque varios archivos de paginación en particiones distintas de la misma unidad de disco físico.

## <a name="peripheral-bus-recommendations"></a>Recomendaciones de bus periférico
En Windows Server 2016, las interfaces principales de almacenamiento y red deben ser PCI Express (PCIe), por lo que se recomiendan servidores con buses PCIe. Para evitar la limitaciones de velocidad de bus, use PCIe x8 y ranuras superiores para los adaptadores Ethernet de más de 10 GB.

## <a name="disk-recommendations"></a>Recomendaciones de disco
Elija discos con velocidades de rotación superiores para disminuir los tiempos de servicio de solicitudes aleatorias (alrededor de 2 ms en promedio cuando compara unidades de 7200 rpm y 15 000 rpm) y para aumentar el ancho de banda de solicitudes secuenciales. Sin embargo, existen consideraciones de costos, potencia y de otro tipo asociadas con los discos que tienen altas velocidades de rotación.

Los discos de 2,5 pulgadas de calidad empresarial pueden atender a un número mucho mayor de solicitudes aleatorias por segundo en comparación con las unidades equivalentes de 3,5 pulgadas.

Almacene los datos a los que se accede con frecuencia, especialmente los datos a los que se accede en secuencia, cerca del inicio de un disco porque esto corresponde aproximadamente a las pistas exteriores (las más rápidas).

Consolidar unidades pequeñas en menos unidades de alta capacidad puede reducir el rendimiento general del almacenamiento. Menos ejes significa menos simultaneidad de servicios de solicitud y, por lo tanto, probablemente menos rendimiento y tiempos de respuesta más prolongados (dependiendo de la intensidad de las cargas de trabajo).

El uso de SSD y de discos flash de alta velocidad es útil para leer principalmente discos con altas velocidades de E/S o E/S sensibles a la latencia. Los discos de arranque son buenos candidatos para el uso de SSD o de discos flash de alta velocidad, porque estos pueden mejorar considerablemente los tiempos de arranque.

Las SSD de NVMe ofrecen un rendimiento superior con profundidades de cola de comandos mayores, procesamiento de interrupciones más eficaz y una mayor eficacia para los comandos de 4 KB. Esto beneficia especialmente a los escenarios que requieren E/S simultáneas intensivas.


## <a name="network-and-storage-adapter-recommendations"></a>Recomendaciones de adaptador de almacenamiento y red

En la sección siguiente se muestran las características recomendadas para los adaptadores de almacenamiento y red de servidores de alto rendimiento. Esta configuración puede ayudar a evitar que los cuellos de botella en el hardware de almacenamiento o red cuando se está bajo una carga intensa.

### <a name="certified-adapter-usage"></a>Uso de adaptadores certificados
Use un adaptador que haya pasado el conjunto de pruebas de certificación de hardware para Windows.

### <a name="64-bit-capability"></a>Funcionalidad de 64 bits
Los adaptadores con funcionalidad de 64 bits puede realizar operaciones de acceso directo a memoria (DMA) desde y hacia ubicaciones de gran memoria física (más de 4 GB). Si el controlador no es compatible con DMA de más de 4 GB, el sistema almacena doblemente en caché la E/S en un espacio de direcciones físicas de menos de 4 GB.

### <a name="copper-and-fiber-adapters"></a>Adaptadores de cobre y fibra
Por lo general, los adaptadores de cobre tienen el mismo rendimiento que sus homólogos de fibra y ambos están disponibles en algunos adaptadores de canal de fibra. Ciertos entornos se adaptan mejor a los adaptadores de cobre, mientras que otros se adaptan mejor a los adaptadores de fibra.

### <a name="dual--or-quad-port-adapters"></a>Adaptadores de dos o cuatro puertos
Los adaptadores de varios puertos son útiles en los servidores que tienen un número limitado de ranuras PCI.

Para solucionar las limitaciones de SCSI sobre el número de discos que se pueden conectar a un bus SCSI, algunos adaptadores entregan dos o cuatro buses SCSI en una sola tarjeta adaptadora. Por lo general, los adaptadores de canal de fibra no tienen límites en el número de discos que se conectan a un adaptador, a menos que estén ocultos detrás de una interfaz SCSI.

Los adaptadores SCSI conectados en serie (SAS) o ATA en serie (SATA) también tienen un número limitado de conexiones, debido a la naturaleza serial de los protocolos, pero puede conectar más discos si usa conmutadores.

Los adaptadores de red tienen esta característica para los escenarios de conmutación por error o equilibrio de carga. Por lo general, usar dos adaptadores de red de un solo puerto brinda un mejor rendimiento que usar un solo adaptador de red de dos puertos para la misma carga de trabajo.

La limitación de bus PCI puede ser un factor importante en la limitación del rendimiento de los adaptadores con varios puertos. Por lo tanto, es importante considerar la posibilidad de colocarlos en una ranura PCIe de alto rendimiento con el ancho de banda suficiente.

### <a name="interrupt-moderation"></a>Moderación de interrupciones
Algunos adaptadores pueden moderar la frecuencia con que interrumpen los procesadores de host para indicar una actividad o su realización. A menudo, moderar las interrupciones puede generar una menor carga de CPU en el host pero, a menos que la moderación de las interrupciones se realice de manera inteligente, el ahorro de CPU puede aumentar la latencia.

### <a name="receive-side-scaling-rss-support"></a>Compatibilidad con Escalado del lado de recepción (RSS)
RSS permite escalar el procesamiento de recepción de paquetes con el número de procesadores de equipo disponibles. Esto resulta especialmente importante con Ethernet de 10 GB y más rápido.

### <a name="offload-capability-and-other-advanced-features-such-as-message-signaled-interrupt-msi-x"></a>Funcionalidad de descarga y otras características avanzadas, como las interrupciones señalizadas por mensajes (MSI)-X
Los adaptadores compatibles con la descarga ofrecen ahorros de CPU que generan un mejor rendimiento.

### <a name="dynamic-interrupt-and-deferred-procedure-call-dpc-redirection"></a>Redireccionamiento de llamada a procedimiento diferida (DPC) e interrupción dinámica
En Windows Server 2016, la E/S de Numa permite que los adaptadores de almacenamiento PCIe redireccionen de manera dinámica las interrupciones y las DPC y pueden ayudar a cualquier sistema multiprocesador al mejorar la creación de particiones de carga de trabajo, almacenar en caché las frecuencias de aciertos e incorporar el uso de interconexiones de hardware para las cargas de trabajo de E/S intensiva.

## <a name="see-also"></a>Consulta también
- [Server Hardware Power Considerations](power.md) (Consideraciones de alimentación del hardware de servidor)
- [Power and Performance Tuning](power/power-performance-tuning.md) (Optimización de potencia y rendimiento)
- [Processor Power Management Tuning](power/processor-power-management-tuning.md) (Optimización de la administración de energía del procesador)
- [Recommended Balanced Plan Parameters](power/recommended-balanced-plan-parameters.md) (Parámetros recomendados del plan equilibrado)
