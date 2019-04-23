---
title: Ampliar volúmenes en Espacios de almacenamiento directo
ms.assetid: fa48f8f7-44e7-4a0b-b32d-d127eff470f0
ms.prod: windows-server-threshold
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/23/2017
ms.localizationpriority: medium
ms.openlocfilehash: 51f58ec23c55a6cb1664d800d6f4a75dae545899
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824976"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Ampliar volúmenes en Espacios de almacenamiento directo
> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones para cambiar el tamaño de los volúmenes en [Espacios de almacenamiento directo](storage-spaces-direct-overview.md).

## <a name="prerequisites"></a>Requisitos previos

### <a name="capacity-in-the-storage-pool"></a>Capacidad del grupo de almacenamiento

Antes de cambiar el tamaño de un volumen, asegúrate de que haya suficiente capacidad en el grupo de almacenamiento para dar cabida a la nueva superficie más grande. Por ejemplo, al cambiar el tamaño de un volumen de reflejo triple de 1 TB a 2 TB, su superficie crecería de 3 TB a 6 TB. Para que el cambio de tamaño sea correcto, necesitarás como mínimo (6 - 3) = 3 TB de capacidad disponible en el grupo de almacenamiento.

### <a name="familiarity-with-volumes-in-storage-spaces"></a>Familiaridad con los volúmenes en Espacios de almacenamiento

En Espacios de almacenamiento directo, cada volumen está formado por varios objetos apilados: el volumen compartido de clúster (CSV), que es un volumen; la partición; el disco, que es un disco virtual; y una capa de almacenamiento o más (si procede). Para cambiar el tamaño de un volumen, tendrás que cambiar el tamaño de varios de estos objetos.

![volumes-in-smapi](media/resize-volumes/volumes-in-smapi.png)

Para familiarizarte con ellos, intenta ejecutar **Get-** con el nombre correspondiente en PowerShell.

Por ejemplo:

```PowerShell
Get-VirtualDisk
```

Para seguir las asociaciones entre objetos en la pila, canaliza un cmdlet **Get -** con el siguiente.

Por ejemplo, aquí tienes cómo llegar desde un disco virtual hasta su volumen:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume 
```

## <a name="step-1--resize-the-virtual-disk"></a>Paso 1: cambiar el tamaño del disco virtual

El disco virtual puede usar capas de almacenamiento, o no, según cómo se haya creado.

Para comprobarlo, ejecuta el cmdlet siguiente:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

Si el cmdlet no devuelve nada, el disco virtual no usa capas de almacenamiento.

### <a name="no-storage-tiers"></a>Sin capas de almacenamiento

Si el disco virtual no tiene ninguna capa de almacenamiento, puedes cambiar el tamaño directamente mediante el cmdlet **Resize-VirtualDisk**.

Indica el nuevo tamaño en el parámetro **-Size**.

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Cuando cambies el tamaño de **VirtualDisk**, **Disk** lo seguirá automáticamente y también cambiará de tamaño.

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

### <a name="with-storage-tiers"></a>Con capas de almacenamiento

Si el disco virtual usa capas de almacenamiento, puedes cambiar el tamaño de cada capa por separado mediante el cmdlet **Resize-StorageTier**.

Para obtener los nombres de las capas de almacenamiento, sigue las asociaciones desde el disco virtual.

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier | Select FriendlyName
```

Luego, para cada capa, indica el nuevo tamaño en el parámetro **-Size**.

```PowerShell
Get-StorageTier <FriendlyName> | Resize-StorageTier -Size <Size>
```

> [!TIP]
> Si las capas son tipos de medios físicos diferentes (como **MediaType = SSD** y **MediaType = HDD**), debes asegurarte de tener capacidad suficiente de cada tipo de medios en el grupo de almacenamiento para dar cabida a la nueva superficie más grande de cada capa.

Cuando cambies el tamaño de **StorageTier**, **VirtualDisk** y **Disk** lo seguirán automáticamente y también cambiarán de tamaño.

![Resize-StorageTier](media/resize-volumes/Resize-StorageTier.gif)

## <a name="step-2--resize-the-partition"></a>Paso 2: cambiar el tamaño de la partición

Luego cambia el tamaño de la partición con el cmdlet **Resize-Partition**. Se espera que el disco virtual tenga dos particiones: la primera es reservada y no se debe modificar; la que tienes que cambiar de tamaño tiene **PartitionNumber = 2** y **Type = Basic**.

Indica el nuevo tamaño en el parámetro **-Size**. Te recomendamos que uses el tamaño máximo admitido, como se muestra a continuación.

```PowerShell
# Choose virtual disk
$VirtualDisk = Get-VirtualDisk <FriendlyName>

# Get its partition
$Partition = $VirtualDisk | Get-Disk | Get-Partition | Where PartitionNumber -Eq 2

# Resize to its maximum supported size 
$Partition | Resize-Partition -Size ($Partition | Get-PartitionSupportedSize).SizeMax
```

Cuando cambies el tamaño de **Partition**, **Volume** y **ClusterSharedVolume** lo seguirán automáticamente y también cambiarán de tamaño.

![Resize-Partition](media/resize-volumes/Resize-Partition.gif)

Ya está.

> [!TIP]
> Para comprobar que el volumen tiene el nuevo tamaño, ejecuta **Get-Volume**.

## <a name="see-also"></a>Vea también

- [Espacios de almacenamiento directo en Windows Server 2016](storage-spaces-direct-overview.md)
- [Planificación de volúmenes en espacios de almacenamiento directo](plan-volumes.md)
- [Creación de volúmenes en espacios de almacenamiento directo](create-volumes.md)