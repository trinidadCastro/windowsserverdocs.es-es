---
title: Almacenamiento espacios en memoria caché de lectura
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122058"
---
# Uso de espacios de almacenamiento directo con el CSV en la memoria caché de lectura
> Se aplica a: Windows Server 2016, Windows Server 2019

En este tema se describe cómo usar la memoria del sistema para aumentar el rendimiento de [Espacios de almacenamiento directo](storage-spaces-direct-overview.md).

Espacios de almacenamiento directo es compatible con el volumen compartido de clúster (CSV) en memoria caché de lectura. Uso de memoria del sistema en caché las lecturas puede mejorar el rendimiento de aplicaciones, como Hyper-V, que usa E/S no almacenado en búfer para acceder a los archivos VHD o VHDX. (No almacenado en búfer IOs son las operaciones que no se almacenan en caché por el Administrador de caché de Windows).

Dado que la memoria caché es local del servidor, mejora la ubicación de datos para las implementaciones de espacios de almacenamiento directo hiperconvergido: lecturas recientes se almacenan en caché en memoria en el mismo host donde se ejecuta la máquina virtual, lo que reduce la frecuencia con lecturas vaya a través de la red. Esto da como resultado una menor latencia y rendimiento del almacenamiento mejor.

## Consideraciones de diseño

Lectura en la memoria caché es más eficaz para cargas de trabajo de lectura intensiva, como la infraestructura de Escritorio Virtual (VDI). Por el contrario, si la carga de trabajo es extremadamente escritura intensiva, la memoria caché puede introducir más sobrecarga que el valor y debe estar deshabilitada.

Puedes usar hasta un 80% del total de memoria física para la CSV en la memoria caché de lectura.

  > [!TIP]
  > Para las implementaciones hiperconvergida, donde cálculo y almacenamiento que se ejecuta en los mismos servidores, ten cuidado de dejar suficiente memoria para las máquinas virtuales. Para las implementaciones de servidor de archivos de escalabilidad horizontal (SoFS) convergidas, con menos contención de memoria, esto no es aplicable.

  > [!NOTE]
  > Habilitado que sin él de caché de lectura de ciertas herramientas microbenchmarking como DISKSPD y la [Máquina virtual flota](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) pueden peor producir resultados con el en memoria CSV. De manera predeterminada flota de máquina virtual crea un total de máquinas virtuales de 100 – GiB 10 VHDX por máquina virtual de aproximadamente 1 TiB y realiza lecturas *uniformemente aleatorias* y escribe en ellos. A diferencia de las cargas de trabajo reales, las lecturas no siguen ningún patrón predecible o repetitivo, por lo que la memoria caché en memoria no es eficaz y solo incurre en una sobrecarga.

## Configuración de la memoria caché de lectura

Lectura del CSV en memoria caché está disponible en Windows Server 2016 y Windows Server 2019 con la misma funcionalidad. En Windows Server 2016, está desactivada de forma predeterminada. En Windows Server 2019, se encuentra en de forma predeterminada con 1 GB asignado.

| Versión de SO          | Tamaño predeterminado de la caché CSV |
|---------------------|------------------------|
| Windows Server 2016 | 0 (deshabilitado)           |
| Windows Server 2019 | 1 giB                   |

Para ver cuánta memoria se asigna con PowerShell, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize
```

El valor devuelto está en mebibytes (MiB) por servidor. Por ejemplo, `1024` representa 1 gibibyte (GiB).

Para cambiar la cantidad de memoria asignada, modificar este valor mediante PowerShell. Por ejemplo, para asignar GiB 2 por servidor, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Los cambios surtan efecto inmediatamente, pausar y reanudación los volúmenes CSV o moverlos entre servidores. Por ejemplo, usa este fragmento de PowerShell para mover cada CSV a otro nodo de servidor y vuelva a:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## Consulta también

- [Información general de Espacios de almacenamiento directos](storage-spaces-direct-overview.md)
