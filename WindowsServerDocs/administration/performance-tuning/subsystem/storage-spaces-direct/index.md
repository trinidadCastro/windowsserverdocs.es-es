---
title: Optimización del rendimiento para Espacios de almacenamiento directo
description: Espacios de almacenamiento directo optimiza automáticamente su rendimiento en función de la configuración de caché del hardware que usa, como se describe en este tema.
ms.topic: article
ms.assetid: 15a519fa-37cc-4d84-a9fe-097d33bb71ea
author: phstee
ms.author: vshankar; danlo; clausjor; stevenek
ms.date: 4/14/2017
ms.openlocfilehash: 18dca2080a311a337e0d41055e95e7a7f0f42a80
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991989"
---
# <a name="performance-tuning-for-storage-spaces-direct"></a>Optimización del rendimiento para Espacios de almacenamiento directo

Espacios de almacenamiento directo, como solución de almacenamiento definida de software basada en Windows Server, optimiza automáticamente su rendimiento y, de este modo, elimina la necesidad de especificar manualmente los recuentos de columna, la configuración de caché del hardware que usa y otros factores que se deben definir manualmente con las soluciones de almacenamiento SAS compartidas. Para obtener más información, consulte [Espacios de almacenamiento directo en Windows Server 2016](../../../../storage/storage-spaces/storage-spaces-direct-overview.md).

La memoria caché de bus de almacenamiento del software de Espacios de almacenamiento directo se configura automáticamente en función de los tipos de almacenamiento presentes en el sistema. Tres tipos reconocidos: **HDD**, **SSD** y **NVMe**. La memoria caché reclama el almacenamiento más rápido para el almacenamiento en caché de lectura o escritura, según corresponda, y usa el almacenamiento más lento para el almacenamiento de datos persistente.

En la tabla siguiente se resumen los valores predeterminados:

| Tipos de almacenamiento | Configuración de caché |
| --- | --- |
| Cualquier tipo único | Si solo hay un tipo de almacenamiento presente, la caché de bus de almacenamiento de software no se configura. |
| SSD + HDD o NVMe + HDD | El almacenamiento más rápido se configura como el nivel de caché, y almacena en caché la lectura y la escritura. |
| SSD + SSD o NVMe + NVMe | Estas opciones rápida + rápida se destinan a combinaciones de almacenamiento superiores de menor resistencia; por ejemplo, 10 escrituras de disco al día (DWPD), SSD flash NAND para la memoria caché y SSD flash NAND DWPD 1.5 para la capacidad. Para habilitar estas opciones, se asigna a Espacios de almacenamiento directo un conjunto de cadenas de modelo para identificar los dispositivos de caché. Para obtener más información, consulte la referencia del cmdlet [Enable-StorageSpacesDirect](https://technet.microsoft.com/library/mt589697.aspx) (`CacheDeviceModel`). <br><br>En un sistema rápido + rápido, solo se almacenan en la memoria caché las operaciones de escritura. Las lecturas no se almacenan en caché. |

Tenga en cuenta que, si se almacena en caché a través de un dispositivo SSD o NVMe, de forma predeterminada, solo se almacenan en caché las operaciones de escritura. La intención es que, puesto que el dispositivo de capacidad es rápido, haya un valor limitado a la hora de mover el contenido leído a los dispositivos de caché. Sin embargo, hay casos en que esto podría no aplicarse, aunque es necesario tener cuidado, ya que la habilitación de la caché de lectura podría consumir la resistencia del dispositivo de caché innecesariamente sin ningún aumento del rendimiento. A continuación se incluyen algunos ejemplos:

* **NVme + SSD** Habilitar la caché de lectura permitirá que la E/S de lectura aproveche la conectividad PCIe o un mayor rendimiento de IOPS de los dispositivos NVMe en comparación con el SSD agregado. <br>Esto _puede_ cumplirse para escenarios orientados al ancho de banda debido a las funcionalidades de ancho de banda relativa de los dispositivos NVMe en comparación con la conexión de HBA a SSD. Esto _no puede_ cumplirse para escenarios orientados a IOPS donde los costos de CPU de IOPS pueden limitar los sistemas antes de que pueda conseguirse un aumento del rendimiento.
* **NVMe + NVMe** De forma similar, si la funcionalidad de lectura de NVMe de caché es superior a NVMe de capacidad combinada, puede que habilitar la caché de lectura ofrezca valor. <br>Se espera que haya pocos casos adecuados para la caché de lectura en estas configuraciones.

Para ver y modificar la configuración de caché, use los cmdlets [Get-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt634616.aspx) y [Set-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt763265.aspx). Las propiedades `CacheModeHDD` y `CacheModeSSD` definen cómo funciona la memoria caché en los medios de capacidad del tipo indicado.

## <a name="additional-references"></a>Referencias adicionales

- [Descripción de Espacios de almacenamiento directo](../../../../storage/storage-spaces/understand-the-cache.md)
- [Planificar Espacios de almacenamiento directo](../../../../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)
- [Ajuste del rendimiento para servidores de archivos](../../role/file-server/index.md)
- [Guía de consideraciones de diseño de almacenamiento definidas mediante software](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/mt243829(v=ws.11)) (para Windows Server 2012 R2 y almacenamiento SAS compartido)