---
title: Rendimiento del procesador de Hyper-V
description: Consideraciones de rendimiento del procesador en el ajuste del rendimiento de Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d165413dfbf89b2debd77806110ca80e9b6af7c8
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471300"
---
# <a name="hyper-v-processor-performance"></a>Rendimiento del procesador de Hyper-V


## <a name="virtual-machine-integration-services"></a>Servicios de integración de máquinas virtuales

La máquina virtual Integration Services incluye controladores habilitados para los dispositivos de e/s específicos de Hyper-V, lo que reduce significativamente la sobrecarga de la CPU para la e/s en comparación con los dispositivos emulados. Debe instalar la versión más reciente de la máquina virtual Integration Services en todas las máquinas virtuales compatibles. Los servicios reducen el uso de la CPU de los invitados, de los invitados inactivos a los invitados de uso intensivo y mejoran el rendimiento de e/s. Este es el primer paso para optimizar el rendimiento en un servidor que ejecuta Hyper-V. Para obtener una lista de los sistemas operativos invitados admitidos, consulte [Introducción a Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Procesadores virtuales

Hyper-V en Windows Server 2016 admite un máximo de 240 procesadores virtuales por máquina virtual. Las máquinas virtuales que tienen cargas que no usan un uso intensivo de la CPU deben configurarse para usar un procesador virtual. Esto se debe a la sobrecarga adicional que está asociada a varios procesadores virtuales, como los costos de sincronización adicionales en el sistema operativo invitado.

Aumente el número de procesadores virtuales si la máquina virtual requiere más de una CPU de procesamiento con una carga máxima.

## <a name="background-activity"></a>Actividad en segundo plano

Minimizar la actividad en segundo plano en máquinas virtuales inactivas libera ciclos de CPU que se pueden usar en otras máquinas virtuales en otro lugar. Los invitados de Windows suelen usar menos de un porcentaje de una CPU cuando están inactivas. A continuación se muestran varios procedimientos recomendados para minimizar el uso de CPU en segundo plano de una máquina virtual:

-   Instale la versión más reciente de la máquina virtual Integration Services.

-   Quite el adaptador de red emulado a través del cuadro de diálogo Configuración de la máquina virtual (use el adaptador específico de Hyper-V de Microsoft).

-   Quite los dispositivos que no se usen, como el CD-ROM y el puerto COM, o desconecte sus medios.

-   Mantenga el sistema operativo invitado de Windows en la pantalla de inicio de sesión cuando no se esté usando y deshabilite el protector de pantalla.

-   Revise las tareas programadas y los servicios que están habilitados de forma predeterminada.

-   Revisar los proveedores de seguimiento de ETW que están activados de forma predeterminada ejecutando **logman.exe Query-ETS**

-   Mejorar las aplicaciones de servidor para reducir la actividad periódica (por ejemplo, temporizadores).

-   Cierre Administrador del servidor tanto en el host como en el sistema operativo invitado.

-   No deje que se ejecute el administrador de Hyper-V, ya que actualiza constantemente la miniatura de la máquina virtual.

A continuación se muestran procedimientos recomendados adicionales para configurar una *versión de cliente* de Windows en una máquina virtual para reducir el uso general de la CPU:

-   Deshabilite los servicios en segundo plano como SuperFetch y búsqueda de Windows.

-   Deshabilite las tareas programadas como desfragmentación programada.

## <a name="virtual-numa"></a>NUMA virtual

Para habilitar la virtualización de grandes cargas de trabajo de escalado vertical, Hyper-V en Windows Server 2016 los límites de escala de máquinas virtuales. Una sola máquina virtual se puede asignar hasta 240 procesadores virtuales y 12 TB de memoria. Al crear estas máquinas virtuales de gran tamaño, es probable que se use la memoria de varios nodos NUMA del sistema host. En esta configuración de máquina virtual, si los procesadores virtuales y la memoria no se asignan desde el mismo nodo NUMA, las cargas de trabajo pueden tener un rendimiento incorrecto debido a la incapacidad de aprovechar las optimizaciones de NUMA.

En Windows Server 2016, Hyper-V presenta una topología NUMA virtual a las máquinas virtuales. De forma predeterminada, esta topología NUMA virtual está optimizada de forma que coincida con la topología NUMA del equipo host subyacente. Al exponer una topología NUMA virtual en una máquina virtual, el sistema operativo invitado y todas las aplicaciones habilitadas para NUMA que se ejecutan dentro de él pueden aprovechar las optimizaciones del rendimiento de NUMA, igual que si se estuvieran ejecutando en un equipo físico.

No hay distinción entre un NUMA virtual y uno físico desde la perspectiva de la carga de trabajo. Dentro de una máquina virtual, cuando una carga de trabajo asigna memoria local para los datos y accede a esos datos en el mismo nodo NUMA, en el sistema físico subyacente, se accede rápidamente a la memoria local. Se logran evitar las penalizaciones del rendimiento debidas al acceso a la memoria remota. Solo las aplicaciones compatibles con NUMA pueden beneficiarse de vNUMA.

Microsoft SQL Server es un ejemplo de aplicación compatible con NUMA. Para obtener más información, consulte [Descripción del acceso no uniforme a memoria](https://technet.microsoft.com/library/ms178144.aspx).

Las características de NUMA virtual y Memoria dinámica no se pueden usar al mismo tiempo. Una máquina virtual que tenga habilitada la memoria dinámica, en la práctica, tendrá solo un nodo NUMA virtual y no se presentará ninguna topología NUMA a la máquina virtual, sea cual sea la configuración de NUMA virtual.

Para obtener más información sobre NUMA virtual, consulte [Introducción a Numa virtual de Hyper-V](https://technet.microsoft.com/library/dn282282.aspx).

## <a name="additional-references"></a>Referencias adicionales

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
