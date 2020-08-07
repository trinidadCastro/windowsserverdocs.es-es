---
title: Optimización del rendimiento Escritorio remoto hosts de virtualización
description: Optimización del rendimiento para hosts de virtualización de Escritorio remoto
ms.topic: article
ms.author: hammadbu; vladmis; denisgun
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 071321249db62c927ee5677a48c52a7f2cd9c20d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896033"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Optimización del rendimiento Escritorio remoto hosts de virtualización

Host de virtualización de Escritorio remoto (host de virtualización de escritorio remoto) es un servicio de rol que admite escenarios de infraestructura de escritorio virtual (VDI) y permite a varios usuarios ejecutar aplicaciones basadas en Windows en máquinas virtuales hospedadas en un servidor que ejecuta Windows Server y Hyper-V.

Windows Server admite dos tipos de escritorios virtuales: escritorios virtuales personales y escritorios virtuales agrupados.

## <a name="general-considerations"></a>Consideraciones generales

### <a name="storage"></a>Storage

El almacenamiento es el cuello de botella de rendimiento más probable y es importante ajustar el tamaño del almacenamiento para administrar correctamente la carga de e/s generada por los cambios de estado de la máquina virtual. Si una prueba piloto o simulación no es factible, una buena directriz es aprovisionar un eje de disco para cuatro máquinas virtuales activas. Use configuraciones de disco que tengan un buen rendimiento de escritura (por ejemplo, RAID 1 + 0).

Cuando sea necesario, use la desduplicación de disco y el almacenamiento en caché para reducir la carga de lectura del disco y habilitar la solución de almacenamiento para acelerar el rendimiento mediante el almacenamiento en caché de una parte importante de la imagen.

### <a name="data-deduplication-and-vdi"></a>Desduplicación de datos e VDI

En Windows Server 2012 R2, la desduplicación de datos admite la optimización de archivos abiertos. Para poder usar máquinas virtuales que se ejecutan en un volumen desduplicado, los archivos de la máquina virtual deben almacenarse en un host independiente del host de Hyper-V. Si Hyper-V y la desduplicación se ejecutan en el mismo equipo, las dos características compiten por los recursos del sistema y afectan negativamente al rendimiento general.

El volumen también debe estar configurado para usar el tipo de optimización de desduplicación de la infraestructura de escritorio virtual (VDI). Puede configurarlo mediante administrador del servidor (configuración de desduplicación de volúmenes de**servicios de archivos y almacenamiento**  - &gt; **Volumes**  - &gt; **Dedup Settings**) o mediante el siguiente comando de Windows PowerShell:

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> La optimización de desduplicación de datos de archivos abiertos solo se admite en escenarios de VDI con Hyper-V mediante el almacenamiento remoto a través de SMB 3,0.

### <a name="memory"></a>Memoria

El uso de memoria del servidor está controlado por tres factores principales:

- Sobrecarga del sistema operativo

- Sobrecarga del servicio Hyper-V por máquina virtual

- Memoria asignada a cada máquina virtual

En el caso de una carga de trabajo típica del trabajador de conocimientos, las máquinas virtuales invitadas que ejecutan la ventana 8 o Windows 8.1 de x86 deben tener ~ 512 MB de memoria como línea de base. Sin embargo, es probable que Memoria dinámica aumente la memoria de la máquina virtual invitada a aproximadamente 800 MB, en función de la carga de trabajo. En el caso de x64, vemos aproximadamente 800 MB a partir de, aumentando hasta 1024 MB.

Por lo tanto, es importante proporcionar suficiente memoria del servidor para satisfacer la memoria necesaria para el número esperado de máquinas virtuales invitadas, además de permitir una cantidad de memoria suficiente para el servidor.

### <a name="cpu"></a>CPU

Al planear la capacidad del servidor para un servidor host de virtualización de escritorio remoto, el número de máquinas virtuales por núcleo físico dependerá de la naturaleza de la carga de trabajo. Como punto de partida, es razonable planificar 12 máquinas virtuales por núcleo físico y, a continuación, ejecutar los escenarios adecuados para validar el rendimiento y la densidad. Se puede lograr una mayor densidad en función de los detalles de la carga de trabajo.

Se recomienda habilitar Hyper-Threading, pero asegúrese de calcular la proporción de sobresuscripción en función del número de núcleos físicos y no del número de procesadores lógicos. Esto garantiza el nivel de rendimiento esperado en cada CPU.

## <a name="performance-optimizations"></a>Optimizaciones de rendimiento

### <a name="dynamic-memory"></a>Memoria dinámica

Memoria dinámica permite el uso más eficaz de los recursos de memoria del servidor que ejecuta Hyper-V mediante el equilibrio de la distribución de la memoria entre las máquinas virtuales en ejecución. La memoria se puede reasignar dinámicamente entre máquinas virtuales en respuesta a sus cargas de trabajo variables.

Memoria dinámica le permite aumentar la densidad de las máquinas virtuales con los recursos que ya tiene sin sacrificar el rendimiento o la escalabilidad. El resultado es un uso más eficaz de los recursos de hardware de servidor caros, lo que puede traducirse en una administración más sencilla y reducir los costos.

En los sistemas operativos invitados que ejecutan Windows 8 y versiones posteriores con procesadores virtuales que abarcan varios procesadores lógicos, tenga en cuenta el equilibrio entre ejecutar con Memoria dinámica para minimizar el uso de memoria y deshabilitar Memoria dinámica para mejorar el rendimiento de una aplicación que tenga en cuenta la topología del equipo. Este tipo de aplicación puede aprovechar la información de la topología para tomar decisiones sobre la programación y la asignación de memoria.

### <a name="tiered-storage"></a>Almacenamiento en capas

El host de virtualización de escritorio remoto admite el almacenamiento en capas para los grupos de escritorios virtuales. El equipo físico compartido por todos los escritorios virtuales agrupados de una colección puede usar una solución de almacenamiento de tamaño pequeño y alto rendimiento, como una unidad de estado sólido (SSD) reflejada. Los escritorios virtuales agrupados se pueden colocar en un almacenamiento más económico y tradicional, como RAID 1 + 0.

El equipo físico debe colocarse en una SSD porque la mayor parte de las operaciones de e/s de lectura de los escritorios virtuales agrupados van al sistema operativo de administración. Por lo tanto, el almacenamiento utilizado por el equipo físico debe admitir e/s de lectura mucho más altas por segundo.

Esta configuración de implementación garantiza un rendimiento rentable en el que se necesita rendimiento. El SSD proporciona mayor rendimiento en un disco de menor tamaño (~ 20 GB por colección, en función de la configuración). El almacenamiento tradicional para escritorios virtuales agrupados (RAID 1 + 0) usa aproximadamente 3 GB por máquina virtual.

### <a name="csv-cache"></a>Caché de CSV

Los clústeres de conmutación por error en Windows Server 2012 y versiones posteriores proporcionan almacenamiento en caché en volúmenes compartidos de clúster (CSV). Esto resulta muy ventajoso para las colecciones de escritorios virtuales agrupados, donde la mayoría de las operaciones de e/s de lectura proceden del sistema operativo de administración. La memoria caché de CSV proporciona mayor rendimiento en varios órdenes de magnitud porque almacena en caché bloques que se leen más de una vez y los entrega desde la memoria del sistema, lo que reduce la e/s. Para obtener más información sobre la memoria caché de CSV, consulte [Cómo habilitar la caché de CSV](https://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

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
> Esta lista no pretende ser una lista completa, ya que los cambios afectarán a los objetivos y escenarios previstos. Para obtener más información, vea [las prensas más calientes, obtenerla ahora, el script de optimización de VDI de Windows 8, cortesía de PFE!](https://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).


> [!NOTE]
> La supercaptura en Windows 8 está habilitada de forma predeterminada. Es compatible con VDI y no debe deshabilitarse. Superfetch puede reducir aún más el consumo de memoria a través del uso compartido de páginas de memoria, lo que es beneficioso para VDI. Los escritorios virtuales agrupados que ejecutan Windows 7, SuperFetch deben estar deshabilitados, pero para escritorios virtuales personales que ejecutan Windows 7, debe dejarse en.
