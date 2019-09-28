---
title: Optimización del rendimiento Escritorio remoto hosts de virtualización
description: Optimización del rendimiento para hosts de virtualización de Escritorio remoto
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 6aad1560fa9f9429af94426487d9a33369137ded
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370031"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Optimización del rendimiento Escritorio remoto hosts de virtualización


Host de virtualización de Escritorio remoto (host de virtualización de escritorio remoto) es un servicio de rol que admite escenarios de infraestructura de escritorio virtual (VDI) y permite que varios usuarios simultáneos ejecuten aplicaciones basadas en Windows en máquinas virtuales que se hospedan en un servidor que ejecuta Windows Server 2016 e Hyper-V.

Windows Server 2016 admite dos tipos de escritorios virtuales, escritorios virtuales personales y escritorios virtuales agrupados.

**En este tema:**

-   [Consideraciones generales](#general-considerations)

-   [Optimizaciones de rendimiento](#performance-optimizations)

## <a name="general-considerations"></a>Consideraciones generales


### <a name="storage"></a>Almacenamiento

El almacenamiento es el cuello de botella de rendimiento más probable y es importante ajustar el tamaño del almacenamiento para administrar correctamente la carga de e/s generada por los cambios de estado de la máquina virtual. Si una prueba piloto o simulación no es factible, una buena directriz es aprovisionar un eje de disco para cuatro máquinas virtuales activas. Use configuraciones de disco que tengan un buen rendimiento de escritura (por ejemplo, RAID 1 + 0).

Cuando sea necesario, use la desduplicación de disco y el almacenamiento en caché para reducir la carga de lectura del disco y habilitar la solución de almacenamiento para acelerar el rendimiento mediante el almacenamiento en caché de una parte importante de la imagen.

### <a name="data-deduplication-and-vdi"></a>Desduplicación de datos e VDI

En Windows Server 2012 R2, la desduplicación de datos admite la optimización de archivos abiertos. Para poder usar máquinas virtuales que se ejecutan en un volumen desduplicado, los archivos de la máquina virtual deben almacenarse en un host independiente del host de Hyper-V. Si Hyper-V y la desduplicación se ejecutan en el mismo equipo, las dos características compiten por los recursos del sistema y afectan negativamente al rendimiento general.

El volumen también debe estar configurado para usar el tipo de optimización de desduplicación de la infraestructura de escritorio virtual (VDI). Puede configurarlo mediante administrador del servidor ( **configuración de desduplicación**de **volúmenes**  - &gt; de - &gt; **servicios de archivos y almacenamiento** ) o mediante el siguiente comando de Windows PowerShell:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> La optimización de desduplicación de datos de archivos abiertos solo se admite en escenarios de VDI con Hyper-V mediante el almacenamiento remoto a través de SMB 3,0.

### <a name="memory"></a>Memoria

El uso de memoria del servidor está controlado por tres factores principales:

-   Sobrecarga del sistema operativo

-   Sobrecarga del servicio Hyper-V por máquina virtual

-   Memoria asignada a cada máquina virtual

En el caso de una carga de trabajo típica del trabajador de conocimientos, las máquinas virtuales invitadas que ejecutan la ventana 8 o Windows 8.1 de x86 deben tener ~ 512 MB de memoria como línea de base. Sin embargo, es probable que Memoria dinámica aumente la memoria de la máquina virtual invitada a aproximadamente 800 MB, en función de la carga de trabajo. En el caso de x64, vemos aproximadamente 800 MB a partir de, aumentando hasta 1024 MB.

Por lo tanto, es importante proporcionar suficiente memoria del servidor para satisfacer la memoria necesaria para el número esperado de máquinas virtuales invitadas, además de permitir una cantidad de memoria suficiente para el servidor.

### <a name="cpu"></a>CPU

Al planear la capacidad del servidor para un servidor host de virtualización de escritorio remoto, el número de máquinas virtuales por núcleo físico dependerá de la naturaleza de la carga de trabajo. Como punto de partida, es razonable planificar 12 máquinas virtuales por núcleo físico y, a continuación, ejecutar los escenarios adecuados para validar el rendimiento y la densidad. Se puede lograr una mayor densidad en función de los detalles de la carga de trabajo.

Se recomienda habilitar Hyper-Threading, pero asegúrese de calcular la proporción de sobresuscripción en función del número de núcleos físicos y no del número de procesadores lógicos. Esto garantiza el nivel de rendimiento esperado en cada CPU.

### <a name="virtual-gpu"></a>GPU virtual

Microsoft RemoteFX para el host de virtualización de escritorio remoto ofrece una experiencia de gráficos enriquecida para infraestructura de escritorio virtual (VDI) a través de la comunicación remota en el host, una canalización de representación-captura-codificación, una codificación muy eficaz basada en GPU, una limitación basada en el cliente. actividad y una GPU virtual habilitada para DirectX. RemoteFX para host de virtualización de Escritorio remoto actualiza la GPU virtual de DirectX9 a DirectX11. También mejora la experiencia del usuario al admitir más monitores con resoluciones más altas.

La experiencia DirectX11 de RemoteFX está disponible sin una GPU de hardware, a través de un controlador emulado mediante software. Aunque esta GPU de software proporciona una buena experiencia, la unidad de procesamiento de gráficos virtuales (VGPU) de RemoteFX agrega una experiencia acelerada de hardware a los escritorios virtuales.

Para aprovechar la experiencia de VGPU de RemoteFX en un servidor que ejecuta Windows Server 2016, necesita un controlador de GPU (como DirectX 11.1 o WDDM 1,2) en el servidor host. Para obtener más información sobre las ofertas de GPU que se usan con RemoteFX para host de virtualización de Escritorio remoto, póngase en contacto con el proveedor de GPU.

Si usa la GPU virtual de RemoteFX en la implementación de VDI, la capacidad de implementación variará en función de los escenarios de uso y la configuración de hardware. Cuando planee la implementación, tenga en cuenta lo siguiente:

-   Número de GPU en el sistema

-   Capacidad de memoria de vídeo en las GPU

-   Recursos de hardware y procesador del sistema

### <a name="remotefx-server-system-memory"></a>Memoria del sistema del servidor RemoteFX

Para cada escritorio virtual habilitado con una GPU virtual, RemoteFX usa la memoria del sistema en el sistema operativo invitado y en el servidor habilitado para RemoteFX. El hipervisor garantiza la disponibilidad de la memoria del sistema para un sistema operativo invitado. En el servidor, cada escritorio virtual habilitado para GPU virtual debe anunciar su requisito de memoria del sistema al hipervisor. Cuando el escritorio virtual habilitado para GPU virtual se está iniciando, el hipervisor reserva memoria adicional del sistema en el servidor habilitado para VGPU para el escritorio virtual habilitado para VGPU.

Los requisitos de memoria para el servidor habilitado para RemoteFX es dinámico porque la cantidad de memoria consumida en el servidor habilitado para RemoteFX depende del número de monitores asociados a los escritorios virtuales habilitados para VGPU y la resolución máxima de esos monitores.

### <a name="remotefx-server-gpu-video-memory"></a>Memoria de vídeo GPU de servidor RemoteFX

Cada escritorio virtual con GPU virtual habilitada usa la memoria de vídeo del hardware de la GPU en el servidor host para representar el escritorio. Además de la representación, un códec usa la memoria de vídeo para comprimir la pantalla representada. La cantidad de memoria necesaria se basa directamente en la cantidad de monitores que se aprovisionan en la máquina virtual.

La memoria de vídeo que se reserva varía en función del número de monitores y la resolución de pantalla del sistema. Algunos usuarios pueden necesitar una resolución de pantalla más alta para tareas específicas. Existe una mayor escalabilidad con una configuración de resolución inferior si todas las demás configuraciones permanecen constantes.

### <a name="remotefx-processor"></a>Procesador RemoteFX

El hipervisor programa el servidor habilitado para RemoteFX y los escritorios virtuales habilitados para GPU virtual en la CPU. A diferencia de la memoria del sistema, no hay información relacionada con recursos adicionales que RemoteFX debe compartir con el hipervisor. La sobrecarga de CPU adicional que RemoteFX aporta al escritorio virtual con GPU virtual habilitada está relacionada con la ejecución del controlador de GPU virtual y una pila de Protocolo de escritorio remoto de modo de usuario.

En el servidor habilitado para RemoteFX, se aumenta la sobrecarga, ya que el sistema ejecuta un proceso adicional (rdvgm. exe) por escritorio virtual habilitado para GPU virtual. Este proceso usa el controlador de dispositivo gráfico para ejecutar comandos en la GPU. El códec también usa las CPU para comprimir los datos de pantalla que deben enviarse de vuelta al cliente.

Un mayor número de procesadores virtuales supone una mejor experiencia de usuario. Se recomienda asignar al menos dos CPU virtuales por el escritorio virtual habilitado para GPU virtual. También se recomienda usar la arquitectura x64 para escritorios virtuales habilitados para GPU virtual porque el rendimiento de las máquinas virtuales x64 es mejor en comparación con las máquinas virtuales x86.

### <a name="remotefx-gpu-processing-power"></a>Potencia de procesamiento de GPU de RemoteFX

Para cada escritorio virtual con GPU virtual habilitada, se ejecuta un proceso de DirectX correspondiente en el servidor habilitado para RemoteFX. Este proceso reproduce todos los comandos de gráficos que recibe del escritorio virtual de RemoteFX en la GPU física. En el caso de la GPU física, es equivalente a ejecutar simultáneamente varias aplicaciones de DirectX.

Normalmente, los dispositivos y controladores de gráficos están optimizados para ejecutar algunas aplicaciones en el escritorio. RemoteFX expande las GPU que se usarán de forma única. Para medir el rendimiento de la GPU en un servidor RemoteFX, se han agregado contadores de rendimiento para medir la respuesta de GPU a las solicitudes de RemoteFX.

Normalmente, cuando un recurso de GPU tiene pocos recursos, las operaciones de lectura y escritura en la GPU tardan mucho tiempo en completarse. Mediante el uso de contadores de rendimiento, los administradores pueden tomar medidas preventivas, lo que elimina la posibilidad de que se produzcan tiempos de inactividad para los usuarios finales.

Los siguientes contadores de rendimiento están disponibles en el servidor RemoteFX para medir el rendimiento de la GPU virtual:

**Gráficos RemoteFX**

-   **Tramas omitidas/segundo: recursos de cliente insuficientes** Número de fotogramas omitidos por segundo debido a insuficientes recursos del cliente

-   **Relación de compresión de gráficos** Relación del número de bytes codificados con el número de bytes de entrada

**Administración de GPU raíz de RemoteFX**

-   **Recursos TDRS en GPU** de servidor número total de veces que el TDR agotó el tiempo de espera en la GPU en el servidor

-   **Recursos Máquinas virtuales que ejecutan el número total de máquinas virtuales de RemoteFX** que tienen instalado el adaptador de vídeo RemoteFX 3D

-   **VRAM MB disponibles por cada** cantidad de memoria de vídeo dedicada que no se está usando

-   **VRAM Porcentaje reservado por GPU @ no__t-0% de la memoria de vídeo dedicada que se ha reservado para RemoteFX

**Software RemoteFX**

-   **Velocidad de captura para el monitor** \[1-4\] muestra la velocidad de captura de RemoteFX para monitores 1-4

-   **Razón de compresión** Desusado en Windows 8 y reemplazado por el **Índice de compresión de gráficos**

-   **Tramas retrasadas/s** Número de fotogramas por segundo en los que no se enviaron datos de gráficos en un período de tiempo determinado

-   **Tiempo de respuesta de GPU de la captura** Latencia medida en la captura de RemoteFX (en microsegundos) para que se completen las operaciones de GPU

-   **Tiempo de respuesta de GPU desde Render** Latencia medida dentro de la representación de RemoteFX (en microsegundos) para que se completen las operaciones de GPU

-   **Bytes de salida** Número total de bytes de salida de RemoteFX

-   **Esperando recuento de clientes/s** Desusado en Windows 8 y reemplazado por **fotogramas omitidos/segundo: recursos de cliente insuficientes**

**Administración de vGPU de RemoteFX**

-   **Recursos TDRS el número total de** TDRS de máquinas virtuales que se han producido en esta máquina virtual (TDRS que el servidor propagado a las máquinas virtuales no se incluyen)

-   **Recursos TDRS propagado por** el número total de TDRS del servidor que se produjeron en el servidor y que se han propagado a la máquina virtual

**Rendimiento de vGPU de máquina virtual de RemoteFX**

-   **Data La invocación presenta el** número total (en segundos) de las operaciones presentes en el escritorio de la máquina virtual por segundo.

-   **Data Saliente presenta/s** el número total de operaciones presentes enviadas por la máquina virtual a la GPU del servidor por segundo

-   **Data Bytes de lectura/** s número total de bytes de lectura del servidor habilitado para RemoteFX por segundo

-   **Data Bytes de envío/** s número total de bytes enviados a la GPU del servidor habilitado para RemoteFX por segundo

-   **DMA Promedio de tiempo (en segundos) de** latencia de búferes de comunicación (s) en los búferes de comunicación

-   **DMA Tiempo (en segundos) de** latencia del búfer DMA (en segundos) desde el momento en que se envía el DMA hasta que se completa

-   **DMA Longitud de** cola DMA de longitud de cola para un adaptador de vídeo RemoteFX 3D

-   **Recursos Tiempo de espera de TDR** por número de GPU de tiempo de espera de TDR que se han producido por GPU en la máquina virtual

-   **Recursos Tiempo de espera de TDR por** el número de tiempos de espera del motor de GPU por cada motor de GPU en la máquina virtual

Además de los contadores de rendimiento de la GPU virtual de RemoteFX, también puede medir el uso de la GPU mediante el explorador de procesos, que muestra el uso de la memoria de vídeo y la utilización de la GPU.

## <a name="performance-optimizations"></a>Optimizaciones de rendimiento

### <a name="dynamic-memory"></a>Memoria dinámica

Memoria dinámica permite el uso más eficaz de los recursos de memoria del servidor que ejecuta Hyper-V mediante el equilibrio de la distribución de la memoria entre las máquinas virtuales en ejecución. La memoria se puede reasignar dinámicamente entre máquinas virtuales en respuesta a sus cargas de trabajo variables.

Memoria dinámica le permite aumentar la densidad de las máquinas virtuales con los recursos que ya tiene sin sacrificar el rendimiento o la escalabilidad. El resultado es un uso más eficaz de los recursos de hardware de servidor caros, lo que puede traducirse en una administración más sencilla y reducir los costos.

En los sistemas operativos invitados que ejecutan Windows 8 y versiones posteriores con procesadores virtuales que abarcan varios procesadores lógicos, tenga en cuenta el equilibrio entre ejecutar con Memoria dinámica para minimizar el uso de memoria y deshabilitar Memoria dinámica para mejorar el rendimiento. de una aplicación que tenga en cuenta la topología del equipo. Este tipo de aplicación puede aprovechar la información de la topología para tomar decisiones sobre la programación y la asignación de memoria.

### <a name="tiered-storage"></a>Almacenamiento en capas

El host de virtualización de escritorio remoto admite el almacenamiento en capas para los grupos de escritorios virtuales. El equipo físico compartido por todos los escritorios virtuales agrupados de una colección puede usar una solución de almacenamiento de tamaño pequeño y alto rendimiento, como una unidad de estado sólido (SSD) reflejada. Los escritorios virtuales agrupados se pueden colocar en un almacenamiento más económico y tradicional, como RAID 1 + 0.

El equipo físico debe colocarse en una SSD porque la mayor parte de las operaciones de e/s de lectura de los escritorios virtuales agrupados van al sistema operativo de administración. Por lo tanto, el almacenamiento utilizado por el equipo físico debe admitir e/s de lectura mucho más altas por segundo.

Esta configuración de implementación garantiza un rendimiento rentable en el que se necesita rendimiento. El SSD proporciona mayor rendimiento en un disco de menor tamaño (~ 20 GB por colección, en función de la configuración). El almacenamiento tradicional para escritorios virtuales agrupados (RAID 1 + 0) usa aproximadamente 3 GB por máquina virtual.

### <a name="csv-cache"></a>Caché de CSV

Los clústeres de conmutación por error en Windows Server 2012 y versiones posteriores proporcionan almacenamiento en caché en volúmenes compartidos de clúster (CSV). Esto resulta muy ventajoso para las colecciones de escritorios virtuales agrupados, donde la mayoría de las operaciones de e/s de lectura proceden del sistema operativo de administración. La memoria caché de CSV proporciona mayor rendimiento en varios órdenes de magnitud porque almacena en caché bloques que se leen más de una vez y los entrega desde la memoria del sistema, lo que reduce la e/s. Para obtener más información sobre la memoria caché de CSV, consulte [Cómo habilitar la caché de CSV](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Escritorios virtuales agrupados

De forma predeterminada, los escritorios virtuales agrupados se revierten al estado original después de que un usuario cierre la sesión, por lo que se abandonan los cambios realizados en el sistema operativo Windows desde el último inicio de sesión de usuario.

Aunque es posible deshabilitar la reversión, sigue siendo una condición temporal, ya que normalmente se vuelve a crear una colección de escritorios virtuales agrupados debido a varias actualizaciones de la plantilla de escritorio virtual.

Tiene sentido desactivar las características y servicios de Windows que dependen del estado persistente. Además, tiene sentido desactivar los servicios que se encuentran principalmente en escenarios que no son de empresa.

Cada servicio específico debe evaluarse correctamente antes de cualquier implementación amplia. A continuación se indican algunos aspectos iniciales que se deben tener en cuenta:

| Servicio                                      | ¿Por qué?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Actualización automática                                  | Los escritorios virtuales agrupados se actualizan volviendo a crear la plantilla de escritorio virtual.                                                                                                                          |
| Archivos sin conexión                                | Los escritorios virtuales siempre están en línea y se conectan desde un punto de vista de red.                                                                                                                         |
| Desfragmentación en segundo plano                            | Los cambios del sistema de archivos se descartan después de que un usuario cierre la sesión (debido a una reversión al estado original o al volver a crear la plantilla de escritorio virtual, lo que hace que se vuelvan a crear todos los escritorios virtuales agrupados). |
| Hibernar o suspender                           | No se trata de un concepto de VDI                                                                                                                                                                                   |
| Error al comprobar el volcado de memoria                        | No se trata de ningún concepto para escritorios virtuales agrupados. Una comprobación de errores: el escritorio virtual agrupado se iniciará desde el estado original.                                                                                       |
| Configuración automática de WLAN                              | No hay ninguna interfaz de dispositivo WiFi para VDI                                                                                                                                                                 |
| Servicio de uso compartido de red de Windows Media Player | Servicio centrado en el consumidor                                                                                                                                                                                  |
| Proveedor de grupo de inicio                          | Servicio centrado en el consumidor                                                                                                                                                                                  |
| Conexión compartida a Internet                  | Servicio centrado en el consumidor                                                                                                                                                                                  |
| Servicios extendidos de Media Center               | Servicio centrado en el consumidor                                                                                                                                                                                  |
> [!NOTE]
> Esta lista no pretende ser una lista completa, ya que los cambios afectarán a los objetivos y escenarios previstos. Para obtener más información, vea [las prensas más calientes, obtenerla ahora, el script de optimización de VDI de Windows 8, cortesía de PFE!](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 
> [!NOTE]
> La supercaptura en Windows 8 está habilitada de forma predeterminada. Es compatible con VDI y no debe deshabilitarse. Superfetch puede reducir aún más el consumo de memoria a través del uso compartido de páginas de memoria, lo que es beneficioso para VDI. Los escritorios virtuales agrupados que ejecutan Windows 7, SuperFetch deben estar deshabilitados, pero para escritorios virtuales personales que ejecutan Windows 7, debe dejarse en.

 
