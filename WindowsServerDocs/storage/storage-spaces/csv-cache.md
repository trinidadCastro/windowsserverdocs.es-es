---
description: Más información acerca de cómo usar Espacios de almacenamiento directo con la memoria caché de lectura en memoria de CSV
title: Espacios de almacenamiento directo caché de lectura en memoria
ms.author: eldenc
manager: siroy
ms.topic: article
author: eldenchristensen
ms.date: 09/21/2020
ms.localizationpriority: medium
ms.openlocfilehash: abda5d2bfeb5bcf65c66609c1e89ab2371089330
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040533"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Uso de Espacios de almacenamiento directo con la memoria caché de lectura en memoria de CSV

> Se aplica a: Windows Server 2019 y Windows Server 2016

En este tema se describe cómo utilizar la memoria del sistema para aumentar el rendimiento de [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

Espacios de almacenamiento directo es compatible con la memoria caché de lectura en memoria de Volumen compartido de clúster (CSV). El uso de la memoria del sistema para las lecturas de caché puede mejorar el rendimiento de aplicaciones como Hyper-V, que usa E/S sin almacenamiento en búfer para acceder a los archivos VHD o VHDX. (Las e/s no almacenadas en búfer son operaciones que el administrador de caché de Windows no almacena en caché).

Dado que la caché en memoria es local del servidor, mejora la ubicación de los datos para las implementaciones de Espacios de almacenamiento directo hiperconvergidas: las lecturas recientes se almacenan en la memoria caché en el mismo host en el que se ejecuta la máquina virtual, lo que reduce la frecuencia de las lecturas a través de la red. Esto produce una menor latencia y un mejor rendimiento de almacenamiento.

## <a name="planning-considerations"></a>Consideraciones de planeación

La caché de lectura en memoria es más eficaz con cargas de trabajo de lectura intensiva como, por ejemplo, la infraestructura de escritorio virtual. Por el contrario, si la carga de trabajo requiere una gran cantidad de escritura, la caché puede suponer más sobrecarga que beneficios y debe deshabilitarse.

Puede usar hasta el 80 % de la memoria física total para la caché de lectura en memoria de Volumen compartido de clúster.

  > [!TIP]
  > En el caso de las implementaciones hiperconvergidas, en las que el proceso y el almacenamiento se ejecutan en los mismos servidores, tenga cuidado de dejar suficiente memoria para las máquinas virtuales. En el caso de las implementaciones de servidor de archivos Scale-Out convergente (SoFS), con menos contención de memoria, esto no se aplica.

  > [!NOTE]
  > Ciertas herramientas de microrealización de pruebas comparativas, como DISKSPD y [VM Fleet](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) pueden obtener peores resultados con la caché de lectura en memoria de Volumen compartido de clúster habilitada que sin ella. De forma predeterminada, la flota de VM crea 1 10 Gibibyte (GiB) VHDX por máquina virtual, aproximadamente un total de 1 TiB para máquinas virtuales de 100 y, a continuación, realiza lecturas y escrituras *aleatorias de forma uniforme* . A diferencia de las cargas de trabajo reales, las lecturas no siguen ningún patrón predecible o repetitivo, por lo que la caché en memoria no es efectiva y solo produce sobrecarga.

## <a name="configuring-the-in-memory-read-cache"></a>Configuración de la caché de lectura en memoria

La memoria caché de lectura en memoria de CSV está disponible en Windows Server 2019 y Windows Server 2016 con la misma funcionalidad. En Windows Server 2019, está activado de forma predeterminada con 1 GiB asignado. En Windows Server 2016, está desactivada de forma predeterminada.

| Versión del SO             | Tamaño predeterminado de caché de Volumen compartido de clúster           |
|------------------------|----------------------------------|
| Windows Server 2019    | 1 GiB                            |
| Windows Server 2016    | 0 (deshabilitado)                     |
| Windows Server 2012 R2 | habilitado: el usuario debe especificar el tamaño |
| Windows Server 2012    | 0 (deshabilitado)                     |

Para ver cuánta memoria está asignada con PowerShell, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize
```

El valor devuelto está en mebibytes (MiB) por servidor. Por ejemplo, `1024` representa 1 Gibibyte (GIB).

Para cambiar la cantidad de memoria que se asigna, modifique este valor mediante PowerShell. Por ejemplo, para asignar 2 GiB por servidor, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Para que los cambios surtan efecto inmediatamente, pause y reanude a continuación los volúmenes de CSV o muévalos entre servidores. Por ejemplo, use este fragmento de PowerShell para trasladar cada volumen compartido de clúster a otro nodo de servidor y viceversa:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="additional-references"></a>Referencias adicionales

- [Introducción a Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
