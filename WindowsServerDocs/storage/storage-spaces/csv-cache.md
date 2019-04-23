---
title: Almacenamiento espacios directo en memoria caché de lectura
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850556"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Uso de espacios de almacenamiento directo con los CSV en la memoria memoria caché de lectura
> Se aplica a: Windows Server 2016, Windows Server 2019

Este tema describe cómo usar la memoria del sistema para mejorar el rendimiento de [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

Espacios de almacenamiento directo es compatible con el volumen compartido de clúster (CSV) en memoria caché de lectura. Uso de memoria del sistema para lecturas de caché puede mejorar el rendimiento de aplicaciones como Hyper-V, que utiliza E/S sin almacenamiento en búfer para obtener acceso a archivos VHD o VHDX. (No almacenado en búfer IOs son las operaciones que no se almacenan en caché por el Administrador de caché de Windows).

Dado que la memoria caché en memoria es el servidor local, mejora la localidad de los datos para las implementaciones de espacios de almacenamiento directo hiperconvergido: lecturas recientes se almacenan en caché en memoria en el mismo host donde se está ejecutando la máquina virtual, lo que reduce la frecuencia con lecturas vaya a través de la red. Esto da como resultado una menor latencia y mejorar el rendimiento de almacenamiento.

## <a name="planning-considerations"></a>Consideraciones sobre planeación

Leer de la memoria en caché es más eficaz para cargas de trabajo de lectura intensiva, por ejemplo, la infraestructura de Escritorio Virtual (VDI). Por el contrario, si la carga de trabajo es extremadamente escritura intensiva, la memoria caché puede provocar más sobrecarga que el valor y debe deshabilitarse.

Puede usar hasta un 80% de memoria física total para los CSV en la memoria caché de lectura.

  > [!TIP]
  > Para las implementaciones hiperconvergidas, donde de proceso y almacenamiento que se ejecutan en los mismos servidores, asegúrese de dejar suficiente memoria para las máquinas virtuales. Para las implementaciones de servidor de archivos de escalabilidad horizontal (SoFS) convergentes, con menor contención de memoria, esto no se aplica.

  > [!NOTE]
  > Determinadas herramientas de microbenchmarking como DISKSPD y [VM flota](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) puede producir resultados peores con el archivo CSV de lectura en memoria caché habilitada que sin él. De forma predeterminada, flota de máquina virtual crea un VHDX por máquina virtual: aproximadamente 1 TiB de 10 GiB total de 100 máquinas virtuales: y, a continuación, realiza *aleatorio uniforme* lee y escribe en ellas. A diferencia de las cargas de trabajo real, las lecturas no siguen cualquier patrón de predicción o repetitiva, por lo que la memoria caché en memoria no es eficaz y solo produce sobrecarga.

## <a name="configuring-the-in-memory-read-cache"></a>Configuración de la memoria caché de lectura

Los CSV en memoria lee la caché está disponible en Windows Server 2016 y Windows Server 2019 con la misma funcionalidad. En Windows Server 2016, está desactivada de forma predeterminada. En Windows Server 2019, está activado de forma predeterminada con 1 GB asignado.

| Versión del sistema operativo          | Tamaño predeterminado de la caché CSV |
|---------------------|------------------------|
| Windows Server 2016 | 0 (deshabilitado)           |
| Windows Server 2019 | 1 GiB                   |

Para ver la cantidad de memoria se asigna mediante PowerShell, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize
```

El valor devuelto está en mebibytes (MiB) por servidor. Por ejemplo, `1024` representa 1 gibibyte (GiB).

Para cambiar la cantidad de memoria se asigna, modifique este valor mediante PowerShell. Por ejemplo, para asignar 2 GiB por servidor, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Para que los cambios surten efecto inmediatamente, pausar, a continuación, reanudar los volúmenes CSV o moverlos entre servidores. Por ejemplo, puede usar este fragmento de PowerShell para mover cada CSV a otro nodo de servidor y vuelva a hacer:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>Vea también

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
