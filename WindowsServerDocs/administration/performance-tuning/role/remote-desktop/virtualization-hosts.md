---
title: Hosts de virtualización de escritorio remoto de optimización del rendimiento
description: Optimizar el rendimiento de Hosts de virtualización de escritorio remoto
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1893c0d2689657a5213b2d59e8d83cea0fc3a0db
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63722724"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Hosts de virtualización de escritorio remoto de optimización del rendimiento


Host de virtualización de escritorio remoto (Host de virtualización de escritorio remoto) es un servicio de rol que admite escenarios de infraestructura de Escritorio Virtual (VDI) y permite que varios usuarios simultáneos ejecutar aplicaciones basadas en Windows en máquinas virtuales que se hospedan en un servidor que ejecuta E Hyper-V de Windows Server 2016.

Windows Server 2016 admite dos tipos de escritorios virtuales, escritorios virtuales personales y escritorios virtuales agrupados.

**En este tema:**

-   [Consideraciones generales](#general)

-   [Optimizaciones de rendimiento](#perfopt)

## <a href="" id="general"></a>Consideraciones generales


### <a name="storage"></a>Almacenamiento

El almacenamiento es el cuello de botella de rendimiento más probable, y es importante cambiar el tamaño de su almacenamiento para tratar la carga de E/S generada por los cambios de estado de máquina virtual. Si una prueba piloto o simulación no es posible, es una buena directriz aprovisionar un cilindro de discos para cuatro máquinas virtuales activas. Utilice las configuraciones de disco que tienen un rendimiento de escritura buena (tales como RAID 1 + 0).

Cuando sea adecuado, use la desduplicación de disco y el almacenamiento en caché para reducir la carga de lectura del disco y habilitar la solución de almacenamiento acelerar el rendimiento al almacenar en caché una parte importante de la imagen.

### <a name="data-deduplication-and-vdi"></a>VDI y desduplicación de datos

Se introdujo en Windows Server 2012 R2, desduplicación de datos admite la optimización de archivos abiertos. Para poder usar las máquinas virtuales que se ejecutan en un volumen desduplicado, los archivos de máquina virtual deben almacenarse en un host desde el host de Hyper-V independiente. Si están ejecutando Hyper-V y la desduplicación en el mismo equipo, las dos características se competir por los recursos del sistema y afectar negativamente al rendimiento general.

¿El volumen también debe configurarse para usar el "Desktop Infraestructura Virtual (VDI)? tipo de optimización de la desduplicación. Puede configurar mediante el administrador del servidor (**File and Storage Services**  - &gt; **volúmenes**  - &gt; **deconfiguracióndedesduplicación**) o bien, mediante el siguiente comando de Windows PowerShell comando:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

**Tenga en cuenta**    optimización de la desduplicación de datos de archivos abiertos solo se admite para escenarios VDI con Hyper-V utiliza almacenamiento remoto sobre SMB 3.0.


### <a name="memory"></a>Memoria

Uso de memoria de servidor se controla mediante los tres factores principales:

-   Sobrecarga del sistema operativo

-   Servicio de Hyper-V sobrecarga por máquina virtual

-   Memoria asignada a cada máquina virtual

Cargas de trabajo trabajador del conocimiento típica, invitado virtual máquinas x86 ejecutando Windows 8 o Windows 8.1 se indicarán ~ 512 MB de memoria como la línea base. Sin embargo, la memoria dinámica probablemente aumentará la memoria de la máquina virtual de invitado a alrededor de 800 MB, dependiendo de la carga de trabajo. Para x64, vemos acerca de cómo iniciar de 800 MB, si se aumenta a 1024 MB.

Por lo tanto, es importante proporcionar suficiente memoria de servidor para satisfacer la memoria requerida por el número esperado de las máquinas virtuales invitadas, además de permitir una cantidad suficiente de memoria para el servidor.

### <a name="cpu"></a>CPU

Al planear la capacidad del servidor para un servidor Host de virtualización de escritorio remoto, el número de máquinas virtuales por núcleo físico dependerá de la naturaleza de la carga de trabajo. Como punto de partida, es razonable planear 12 máquinas virtuales por núcleo físico y, a continuación, ejecute los escenarios adecuados para validar el rendimiento y la densidad. Una mayor densidad puede ser factible según las particularidades de la carga de trabajo.

Se recomienda habilitar hyper-threading, pero asegúrese de calcular la proporción de la suscripción excesiva según el número de núcleos físicos y no el número de procesadores lógicos. Esto garantiza que el nivel de rendimiento en una CPU por esperado.

### <a name="virtual-gpu"></a>GPU virtual

Microsoft RemoteFX para el Host de virtualización de escritorio remoto ofrece una experiencia gráfica enriquecida para codificar Virtual Desktop Infrastructure (VDI) a través del host remoto, una representación de capturar y codificar, muy eficaz basada en GPU, limitación basada en cliente actividad y una GPU virtual habilitada para DirectX. RemoteFX para el Host de virtualización de escritorio remoto actualiza la GPU virtual desde DirectX9 a DirectX11. También se mejora la experiencia del usuario admitiendo varios monitores con resoluciones más altas.

La experiencia de RemoteFX DirectX11 está disponible sin un hardware GPU, a través de un controlador de software emulados. Aunque este software GPU proporciona una buena experiencia, la unidad de procesamiento de gráficos virtuales RemoteFX (VGPU) agrega una experiencia de aceleración de hardware a escritorios virtuales.

Para aprovechar las ventajas de la experiencia de la VGPU de RemoteFX en un servidor que ejecuta Windows Server 2016, necesita un controlador de GPU (como DirectX11.1 o WDDM 1.2) en el servidor host. Para obtener más información acerca de las ofertas de GPU para usar con RemoteFX para el Host de virtualización de escritorio remoto, póngase en contacto con su proveedor de GPU.

Si usa la GPU virtual RemoteFX en la implementación de VDI, la capacidad de implementación varían en función de los escenarios de uso y configuración de hardware. Al planear la implementación, tenga en cuenta lo siguiente:

-   Número de GPU en el sistema

-   Capacidad de memoria de vídeo en la GPU

-   Recursos de procesador y el hardware del sistema

### <a name="remotefx-server-system-memory"></a>Memoria del sistema de servidor de RemoteFX

Para cada escritorio virtual habilitado con una GPU virtual, RemoteFX utiliza memoria del sistema en el sistema operativo invitado y en el servidor habilitadas para RemoteFX. El hipervisor garantiza la disponibilidad de memoria del sistema para un sistema operativo invitado. En el servidor, cada escritorio virtual virtual habilitadas para GPU debe anunciar su requisito de memoria del sistema para el hipervisor. Cuando se inicia el escritorio virtual virtual basados en GPU, el hipervisor reserva memoria adicional del sistema en el servidor habilitadas para RemoteFX para el escritorio virtual VGPU habilitado.

El requisito de memoria para el servidor habilitadas para RemoteFX es dinámico, dado que la cantidad de memoria consumida en el servidor habilitadas para RemoteFX es depende del número de monitores que están asociados a los escritorios virtuales VGPU habilitado y la resolución máxima de los monitores.

### <a name="remotefx-server-gpu-video-memory"></a>Servidor RemoteFX memoria de vídeo de GPU

Cada escritorio virtual habilitadas para GPU virtual usa la memoria de vídeo en el hardware GPU en el servidor host para representar el escritorio. Además de representación, la memoria de vídeo está usando un códec para comprimir la pantalla representada. La cantidad de memoria necesaria se basa directamente en la cantidad de monitores que se aprovisionan en la máquina virtual.

La memoria de vídeo que se ha reservado varía en función del número de monitores y la resolución de pantalla del sistema. Algunos usuarios pueden requerir una mayor resolución de pantalla para tareas específicas. Hay una mayor escalabilidad con la configuración de resolución inferior si todas las demás opciones permanezcan constantes.

### <a name="remotefx-processor"></a>Procesador de RemoteFX

El hipervisor programa el servidor habilitadas para RemoteFX y el virtuales escritorios virtuales basados en GPU en la CPU. A diferencia de la memoria del sistema, no hay información relacionada con recursos adicionales que se debe compartir con el hipervisor RemoteFX. Está relacionado con la CPU adicional que sobrecarga que RemoteFX incorpora en el escritorio virtual habilitadas para GPU virtual que ejecuta el controlador de GPU virtual y una pila de protocolo de escritorio remoto de modo de usuario.

En el servidor habilitadas para RemoteFX, la sobrecarga aumenta, porque el sistema ejecuta un proceso adicional (rdvgm.exe) por un escritorio virtual virtual habilitadas para GPU. Este proceso usa el controlador de dispositivo de gráficos para ejecutar comandos en la GPU. El códec usa también la CPU para comprimir los datos de pantalla que deben enviarse al cliente.

Más procesadores virtuales significan una mejor experiencia de usuario. Se recomienda asignar al menos dos CPU virtuales por escritorio virtual virtual habilitadas para GPU. También se recomienda usar el x64 arquitectura para escritorios virtuales habilitadas para GPU virtuales porque el rendimiento en las máquinas virtuales es mejor en comparación con x86 de x64 máquinas virtuales.

### <a name="remotefx-gpu-processing-power"></a>Capacidad de procesamiento de RemoteFX GPU

Para cada escritorio virtual habilitadas para GPU virtual, hay un proceso de DirectX correspondiente que se ejecutan en el servidor habilitadas para RemoteFX. Este proceso reproduce todos los comandos de gráficos que recibe desde el escritorio virtual RemoteFX en la GPU físico. Para la GPU física, es equivalente a ejecutar simultáneamente varias aplicaciones de DirectX.

Normalmente, los controladores y dispositivos de gráficos se ajustan para ejecutar algunas aplicaciones en el escritorio. RemoteFX ajusta las GPU que se usará de forma única. Para medir el funcionamiento de la GPU en un servidor RemoteFX, se han agregado los contadores de rendimiento para medir la respuesta de la GPU a las solicitudes de RemoteFX.

Normalmente, cuando un recurso de la GPU está quedando sin recursos, lea y las operaciones de escritura para la toma GPU mucho tiempo en completarse. Mediante el uso de contadores de rendimiento, los administradores pueden adoptar medidas preventivas, lo que elimina la posibilidad de que los tiempos de inactividad para sus usuarios finales.

Los siguientes contadores de rendimiento están disponibles en el servidor de RemoteFX para medir el rendimiento de la GPU virtual:

**Gráficos de RemoteFX**

-   **Marcos omitidos por segundo - insuficientes recursos del cliente** omite el número de fotogramas por segundo debido a insuficiente de recursos

-   **Razón de compresión de gráficos** proporción del número de bytes codificados en el número de bytes de entrada

**RemoteFX raíz administración de GPU**

-   **Recursos: Los TdR en servidor GPU** número Total de veces que los TDR se agota el tiempo en la GPU en el servidor

-   **Recursos: Máquinas virtuales que ejecutan RemoteFX** número Total de máquinas virtuales que tengan el adaptador de vídeo RemoteFX 3D instalado

-   **VRAM: MB disponible por GPU** cantidad de memoria de vídeo dedicada que no se está usando.

-   **VRAM: % Reservada por GPU** por ciento de memoria de vídeo dedicada que se ha reservado para RemoteFX

**Software de RemoteFX**

-   **Tasa de captura de monitor** \[1-4\] muestra la tasa de captura de RemoteFX para monitores 1-4

-   **Razón de compresión** funcionalidades desusadas en Windows 8 y se sustituye por **razón de compresión de gráficos**

-   **Retrasa fotogramas/segundo** número de fotogramas por segundo que no se ha enviado datos de gráficos en un período de tiempo determinado

-   **Tiempo de respuesta GPU de captura** medir la latencia en la captura de RemoteFX (en microsegundos) para que completar las operaciones de GPU

-   **Tiempo de respuesta GPU de representación** medir la latencia en la representación de RemoteFX (en microsegundos) para que completar las operaciones de GPU

-   **Bytes de salida** bytes de salida del número Total de RemoteFX

-   **Esperando los recuentos de cliente/seg.** funcionalidades desusadas en Windows 8 y se sustituye por **fotogramas omitidos por segundo - insuficientes recursos del cliente**

**Administración de la vGPU de RemoteFX**

-   **Recursos: Local a las máquinas virtuales de los TdR** número Total de los TdR que se han producido en esta máquina virtual (de que el servidor que se propaga a las máquinas virtuales no se incluyen los TdR)

-   **Recursos: Los TdR propagados servidor** número Total de los TdR que se produjo en el servidor y que se han propagado a la máquina virtual

**Rendimiento de vGPU de RemoteFX máquinas virtuales**

-   **Datos: Invoca presenta/seg.** número Total (en segundos) de operaciones está presentes para que se van a representar en el escritorio de la máquina virtual por segundo

-   **Datos: Presenta/seg. de salida** número Total de operaciones presentes enviados por la máquina virtual a la GPU del servidor por segundo

-   **Datos: Bytes leídos/s** número Total de bytes leídos desde el servidor habilitadas para RemoteFX por segundo

-   **Datos: Bytes/seg. de envío** número Total de bytes enviados en el servidor habilitadas para RemoteFX GPU por segundo

-   **DMA: Los búferes de comunicaciones promedio de latencia (s)** promedio de tiempo (en segundos) empleado en los búferes de comunicación

-   **DMA: Latencia de búfer DMA (s)** cantidad de tiempo (en segundos) desde el DMA se envía hasta que haya completado

-   **DMA: Longitud de cola** longitud de cola de DMA para un adaptador de vídeo RemoteFX 3D

-   **Recursos: Tiempos de espera TDR por GPU** tiempos de espera del recuento de TDR que se han producido por GPU en la máquina virtual

-   **Recursos: Tiempos de espera TDR por el motor de GPU** del motor de tiempos de espera del recuento de TDR que se han producido por GPU en la máquina virtual

Además de los contadores de rendimiento de GPU de virtual RemoteFX, también puede medir el uso de GPU mediante el uso de Process Explorer, que muestra el uso de memoria de vídeo y el uso de GPU.

## <a href="" id="perfopt"></a>Optimizaciones de rendimiento


### <a name="dynamic-memory"></a>Memoria dinámica

Memoria dinámica permite con mayor eficacia de uso de los recursos de memoria del servidor que ejecuta Hyper-V mediante Equilibrio de cómo se distribuye la memoria entre las máquinas virtuales en ejecución. Memoria se puede reasignar de manera dinámica entre las máquinas virtuales en respuesta a las cambiantes cargas de trabajo.

Memoria dinámica permite aumentar la densidad de la máquina virtual con los recursos que ya tiene sin sacrificar el rendimiento o escalabilidad. El resultado es un uso más eficaz de los recursos de hardware de servidor costoso, que se puede traducir a facilitar la administración y reducir los costos.

En sistemas operativos invitados que ejecutan Windows 8 y anteriormente con procesadores virtuales que abarcan varios procesadores lógicos, tenga en cuenta el equilibrio entre la ejecución con la memoria dinámica para ayudar a minimizar el uso de memoria y deshabilitar la memoria dinámica para mejorar el rendimiento de una aplicación que es consciente de topología de equipos. Este tipo de aplicación puede aprovechar la información de topología para decisiones de programación y la memoria asignación.

### <a name="tiered-storage"></a>Almacenamiento en capas

Host de virtualización de escritorio remoto admite el almacenamiento en capas para grupos de escritorios virtuales. El equipo físico que se comparte entre todos los escritorios virtuales dentro de una colección puede usar una solución de almacenamiento de tamaño pequeño y de alto rendimiento, como una unidad reflejada de estado sólida (SSD). Los escritorios virtuales se pueden colocar en un almacenamiento menos costoso y tradicionales como RAID 1 + 0.

El equipo físico debe colocarse en una unidad SSD es porque la mayoría de la lectura-/ Os de escritorios virtuales van al sistema operativo de administración. Por lo tanto, el almacenamiento utilizado por el equipo físico debe admitir más altas de lectura de E/s por segundo.

Esta configuración de implementación garantiza el rendimiento más rentable que se necesita el rendimiento. El SSD ofrece un mayor rendimiento en un disco de tamaño más pequeño (aproximadamente 20 GB por colección, según la configuración). Almacenamiento tradicional para escritorios virtuales agrupados (RAID 1 + 0) utiliza aproximadamente 3 GB por máquina virtual.

### <a name="csv-cache"></a>Memoria caché de CSV

Agrupación en clústeres de conmutación por error en Windows Server 2012 y versiones posteriores proporciona almacenamiento en caché en volúmenes compartidos de clúster (CSV). Esto es extremadamente útil para las colecciones de escritorios virtuales agrupados que proceden de las operaciones de E/s de la mayor parte de la lectura desde el sistema operativo de administración. La caché de CSV proporciona un mayor rendimiento en varios órdenes de magnitud porque almacena en caché de bloques que se leen varias veces y entrega de la memoria del sistema, lo que reduce la E/S. Para obtener más información sobre la memoria caché de CSV, consulte [cómo habilitar la memoria caché de CSV](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Escritorios virtuales agrupados

De forma predeterminada, escritorios virtuales agrupados se revierten al estado original una vez que un usuario cierra la sesión, por lo que los cambios realizados al sistema operativo Windows desde el último inicio de sesión del usuario se abandonan.

Aunque es posible deshabilitar la reversión, todavía es una condición temporal porque normalmente una colección de escritorios virtuales agrupados se vuelve a crear debido a varias actualizaciones a la plantilla de escritorio virtual.

Tiene sentido para desactivar las características de Windows y servicios que dependen de un estado persistente. Además, tiene sentido para desactivar todos los servicios que son principalmente para escenarios de clientes no empresariales.

Cada servicio específico se debe evaluar adecuadamente antes de cualquier implementación amplia. Estos son algunos aspectos que deben considerarse iniciales:

| Servicio                                      | ¿Por qué?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Actualización automática                                  | Escritorios virtuales agrupados se actualizan volviendo a crear la plantilla de escritorio virtual.                                                                                                                          |
| Archivos sin conexión                                | Escritorios virtuales están siempre en línea y conectado desde una red punto de vista.                                                                                                                         |
| Desfragmentación en segundo plano                            | Después de que un usuario apruebe (debido a una reversión al estado original, o volver a crear la plantilla de escritorio virtual, lo que resulta en volver a crear todos los escritorios virtuales), se descartan los cambios del sistema de archivos. |
| Modo de hibernación o suspensión                           | Ningún concepto de VDI                                                                                                                                                                                   |
| Volcado de memoria de comprobación de errores                        | Sin esos conceptos para escritorios virtuales agrupados. Se iniciará una comprobación de errores escritorios virtuales desde el estado original.                                                                                       |
| Configuración automática de WLAN                              | No hay ninguna interfaz de dispositivo Wi-Fi para VDI                                                                                                                                                                 |
| Servicio de uso compartido de red de Windows Media Player | Centrado en el servicio de consumidor                                                                                                                                                                                  |
| Proveedor de grupo en el hogar                          | Centrado en el servicio de consumidor                                                                                                                                                                                  |
| Conexión compartida a Internet                  | Centrado en el servicio de consumidor                                                                                                                                                                                  |
| Servicios ampliados de Media Center               | Centrado en el servicio de consumidor                                                                                                                                                                                  |

 

**Tenga en cuenta**    esta lista no pretende ser una lista completa, porque los cambios afectarán a los objetivos previstos y escenarios. Para obtener más información, consulte [caliente desactivado el presiona, Obténgalo ahora, la secuencia de comandos de la optimización de Windows 8 VDI, cortesía de PFE!](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 

**Tenga en cuenta**    SuperFetch en Windows 8 está habilitado de forma predeterminada. Es compatible con VDI y no debe deshabilitarse. SuperFetch puede reducir aún más el consumo de memoria a través de memoria compartida en la página, que es beneficioso para VDI. Se debe deshabilitar escritorios virtuales agrupados que ejecutan Windows 7, SuperFetch, pero para los escritorios virtuales personales que ejecutan Windows 7, debe dejarse en.

 
