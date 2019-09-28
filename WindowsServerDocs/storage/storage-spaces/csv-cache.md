---
title: Espacios de almacenamiento directo caché de lectura en memoria
ms.prod: windows-server
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 83fc923f505531f955fc0131d7dcc1ce98974daa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394088"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Uso de Espacios de almacenamiento directo con la memoria caché de lectura en memoria de CSV
> Se aplica a: Windows Server 2016, Windows Server 2019

En este tema se describe cómo utilizar la memoria del sistema para aumentar el rendimiento de [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

Espacios de almacenamiento directo es compatible con la memoria caché de lectura en memoria de Volumen compartido de clúster (CSV). El uso de la memoria del sistema para las lecturas en caché puede mejorar el rendimiento de las aplicaciones como Hyper-V, que usa e/s sin almacenamiento en búfer para acceder a los archivos VHD o VHDX. (Las e/s no almacenadas en búfer son operaciones que el administrador de caché de Windows no almacena en caché).

Dado que la caché en memoria es local del servidor, mejora la ubicación de los datos para las implementaciones de Espacios de almacenamiento directo hiperconvergidas: las lecturas recientes se almacenan en la memoria caché en el mismo host en el que se ejecuta la máquina virtual, lo que reduce la frecuencia de las lecturas a través de la red. Esto produce una menor latencia y un mejor rendimiento de almacenamiento.

## <a name="planning-considerations"></a>Consideraciones sobre planeación

La memoria caché de lectura en memoria es más eficaz para las cargas de trabajo de lectura intensiva, como la infraestructura de escritorio virtual (VDI). Por el contrario, si la carga de trabajo requiere una gran cantidad de escritura, la memoria caché puede introducir más sobrecarga que el valor y debe deshabilitarse.

Puede usar hasta el 80% de la memoria física total para la caché de lectura en memoria de CSV.

  > [!TIP]
  > En el caso de las implementaciones hiperconvergidas, en las que el proceso y el almacenamiento se ejecutan en los mismos servidores, tenga cuidado de dejar suficiente memoria para las máquinas virtuales. En el caso de las implementaciones de Servidor de archivos de escalabilidad horizontal convergente (SoFS), con menos contención de memoria, esto no se aplica.

  > [!NOTE]
  > Algunas herramientas de microbenchmarks como DISKSPD y la [flota de máquinas virtuales](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) pueden producir resultados peores con la memoria caché de lectura en memoria de CSV habilitada que sin ella. De forma predeterminada, la flota de VM crea 1 10 GiB VHDX por máquina virtual, aproximadamente un total de 1 TiB para máquinas virtuales de 100 y, a continuación, realiza lecturas y escrituras *aleatorias de forma uniforme* . A diferencia de las cargas de trabajo reales, las lecturas no siguen ningún patrón predecible o repetitivo, por lo que la memoria caché en memoria no es efectiva y solo incurre en sobrecarga.

## <a name="configuring-the-in-memory-read-cache"></a>Configurar la memoria caché de lectura en memoria

La memoria caché de lectura en memoria de CSV está disponible en Windows Server 2016 y Windows Server 2019 con la misma funcionalidad. En Windows Server 2016, está desactivado de forma predeterminada. En Windows Server 2019, está activado de forma predeterminada con 1 GB asignado.

| Versión del sistema operativo          | Tamaño predeterminado de caché de CSV |
|---------------------|------------------------|
| Windows Server 2016 | 0 (deshabilitado)           |
| Windows Server 2019 | 1 GiB                   |

Para ver cuánta memoria se asigna mediante PowerShell, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize
```

El valor devuelto está en mebibytes (MiB) por servidor. Por ejemplo, `1024` representa 1 Gibibyte (GiB).

Para cambiar la cantidad de memoria que se asigna, modifique este valor mediante PowerShell. Por ejemplo, para asignar 2 GiB por servidor, ejecute:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Para que los cambios surtan efecto inmediatamente, PAUSE y reanude los volúmenes CSV o muévalos entre servidores. Por ejemplo, use este fragmento de PowerShell para trasladar cada CSV a otro nodo del servidor y volver de nuevo:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
