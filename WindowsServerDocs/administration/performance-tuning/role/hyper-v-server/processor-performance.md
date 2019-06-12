---
title: Rendimiento del procesador de Hyper-V
description: Consideraciones de rendimiento de procesador en el ajuste del rendimiento de Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f16ee9cff9c244a8c579e008bced1e90b1a20673
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435593"
---
# <a name="hyper-v-processor-performance"></a>Rendimiento del procesador de Hyper-V


## <a name="virtual-machine-integration-services"></a>Servicios de integración de la máquina virtual

Los servicios de integración de la máquina Virtual incluye controladores habilitados para los dispositivos de E/S específico de Hyper-V, lo que reduce significativamente CPU sobrecarga de E/S en comparación con dispositivos emulados. Debe instalar la versión más reciente de los servicios de integración de la máquina Virtual en todas las máquinas virtuales compatibles. El uso de CPU de los invitados, estado de inactividad los invitados en gran medida la disminución del servicios utiliza a los invitados y mejora el rendimiento de E/S. Este es el primer paso para la optimización de rendimiento en un servidor que ejecuta Hyper-V. Para obtener una lista de sistemas operativos invitados compatibles, consulte [Introducción a Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Procesadores virtuales

Hyper-V en Windows Server 2016 admite un máximo de 240 procesadores virtuales por máquina virtual. Máquinas virtuales que tienen cargas que no son de uso intensivo de CPU debe configurarse para usar un procesador virtual. Esto es debido a la sobrecarga adicional que está asociada con varios procesadores virtuales, como los costos de una sincronización adicional en el sistema operativo invitado.

Aumentar el número de procesadores virtuales si la máquina virtual requiere más de una CPU del procesamiento de carga máxima.

## <a name="background-activity"></a>Actividad en segundo plano

Minimizar la actividad en segundo plano en máquinas virtuales inactivas libera ciclos de CPU que se pueden usar en otro lugar en otras máquinas virtuales. Invitados de Windows suele utilizan menos del uno por ciento de una CPU cuando están inactivas. Los siguientes son varios procedimientos recomendados para minimizar el uso de CPU de una máquina virtual de fondo:

-   Instale la versión más reciente de los servicios de integración de la máquina Virtual.

-   Quitar el adaptador de red emulado mediante el cuadro de diálogo de configuración de máquina virtual (adaptador de Microsoft Hyper-V-específico del uso).

-   Quite los dispositivos no usados, como el puerto de CD-ROM y COM o desconecte los medios.

-   Mantener el sistema operativo de Windows en la pantalla de inicio de sesión cuando no se utiliza y deshabilita el protector de pantalla.

-   Revise las tareas programadas y servicios que están habilitados de forma predeterminada.

-   Revise los proveedores de seguimiento ETW que están activadas de forma predeterminada mediante la ejecución **logman.exe consultar - ets**

-   Mejorar las aplicaciones de servidor para reducir la actividad periódica (por ejemplo, los temporizadores).

-   Cierre el administrador del servidor en los sistemas operativos host e invitado.

-   No deje el Administrador de Hyper-V en ejecución ya que actualiza constantemente en miniatura de la máquina virtual.

Los siguientes son recomendaciones adicionales para configurar un *versión del cliente* de Windows en una máquina virtual para reducir el uso de CPU general:

-   Deshabilite los servicios en segundo plano, como SuperFetch y Windows Search.

-   Deshabilitar las tareas programadas como desfragmentación programada.

## <a name="virtual-numa"></a>NUMA virtual

Para habilitar la virtualización de grandes cargas de trabajo de escalado vertical, Hyper-V en Windows Server 2016 expande los límites de escalado de máquinas virtuales. Una sola máquina virtual se puede asignar hasta 240 procesadores virtuales y 12 TB de memoria. Al crear dichas máquinas virtuales grandes, es probable que se utilizará memoria de varios nodos NUMA en el sistema host. En dicha configuración de máquina virtual, si no se asignan memoria y procesadores virtuales desde el mismo nodo NUMA, las cargas de trabajo pueden tener un rendimiento incorrecto debido a la incapacidad para aprovechar las ventajas de las optimizaciones de NUMA.

En Windows Server 2016, Hyper-V presenta una topología NUMA virtual a las máquinas virtuales. De forma predeterminada, esta topología NUMA virtual está optimizada de forma que coincida con la topología NUMA del equipo host subyacente. Al exponer una topología NUMA virtual en una máquina virtual, el sistema operativo invitado y todas las aplicaciones habilitadas para NUMA que se ejecutan dentro de él pueden aprovechar las optimizaciones del rendimiento de NUMA, igual que si se estuvieran ejecutando en un equipo físico.

Desde el punto de vista de la carga de trabajo, no hay distinción entre un NUMA virtual y uno físico. Dentro de una máquina virtual, cuando una carga de trabajo asigna memoria local para los datos y accede a esos datos en el mismo nodo NUMA, en el sistema físico subyacente, se accede rápidamente a la memoria local. Se logran evitar las penalizaciones del rendimiento debidas al acceso a la memoria remota. Solo las aplicaciones habilitadas para NUMA pueden beneficiarse de vNUMA.

Microsoft SQL Server es un ejemplo de aplicación compatible con NUMA. Para obtener más información, consulte [el acceso a memoria no uniforme descripción](https://technet.microsoft.com/library/ms178144.aspx).

Las características de NUMA virtual y Memoria dinámica no se pueden usar al mismo tiempo. Una máquina virtual que tenga habilitada la memoria dinámica, en la práctica, tendrá solo un nodo NUMA virtual y no se presentará ninguna topología NUMA a la máquina virtual, sea cual sea la configuración de NUMA virtual.

Para obtener más información sobre NUMA Virtual, consulte [Introducción a NUMA Virtual de Hyper-V](https://technet.microsoft.com/library/dn282282.aspx).

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
