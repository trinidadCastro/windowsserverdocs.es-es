---
title: Ampliar volúmenes en Espacios de almacenamiento directo
description: Cómo cambiar el tamaño de los volúmenes en Espacios de almacenamiento directo mediante el centro de administración de Windows y PowerShell.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 03/10/2020
ms.openlocfilehash: 4ce41da1da3dc90f698008902170d7cc1541619c
ms.sourcegitcommit: bb2eb0b12f2a32113899a59aa5644bc6e8cab3d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79089356"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Ampliar volúmenes en Espacios de almacenamiento directo
> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones para cambiar el tamaño de los volúmenes en un clúster de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) mediante el centro de administración de Windows.

> [!WARNING]
> **No se admite: cambiar el tamaño del almacenamiento subyacente utilizado por Espacios de almacenamiento directo.** Si está ejecutando Espacios de almacenamiento directo en un entorno de almacenamiento virtualizado, incluido en Azure, no se admite el cambio de tamaño o la modificación de las características de los dispositivos de almacenamiento usados por las máquinas virtuales y se producirán inaccesibles los datos. En su lugar, siga las instrucciones de la sección [agregar servidores o unidades](add-nodes.md) para agregar capacidad adicional antes de ampliar los volúmenes.

Vea un vídeo rápido sobre cómo cambiar el tamaño de un volumen.

> [!VIDEO https://www.youtube-nocookie.com/embed/hqyBzipBoTI]

## <a name="extending-volumes-using-windows-admin-center"></a>Extender volúmenes mediante el centro de administración de Windows

1. En el centro de administración de Windows, conéctese a un clúster de Espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** en el panel **herramientas** .
2. En la página volúmenes, seleccione la pestaña **inventario** y, a continuación, seleccione el volumen cuyo tamaño desea cambiar.

    En la página de detalles del volumen, se indica la capacidad de almacenamiento del volumen. También puede abrir la página de detalles de los volúmenes directamente desde el panel. En el panel, en el panel alertas, seleccione la alerta, que le notifica si un volumen se está quedando sin capacidad de almacenamiento y, a continuación, seleccione **ir al volumen**.

4. En la parte superior de la página de detalles de los volúmenes, seleccione **cambiar tamaño**.
5. Escriba un nuevo tamaño mayor y seleccione **cambiar tamaño**.

    En la página de detalles de los volúmenes, se indica la capacidad de almacenamiento más grande para el volumen y se borra la alerta en el panel.

## <a name="extending-volumes-using-powershell"></a>Extensión de volúmenes con PowerShell

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

### <a name="step-1--resize-the-virtual-disk"></a>Paso 1: cambiar el tamaño del disco virtual

El disco virtual puede usar capas de almacenamiento, o no, según cómo se haya creado.

Para comprobarlo, ejecuta el cmdlet siguiente:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

Si el cmdlet no devuelve nada, el disco virtual no usa capas de almacenamiento.

#### <a name="no-storage-tiers"></a>Sin capas de almacenamiento

Si el disco virtual no tiene ninguna capa de almacenamiento, puedes cambiar el tamaño directamente mediante el cmdlet **Resize-VirtualDisk**.

Indica el nuevo tamaño en el parámetro **-Size**.

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Cuando cambies el tamaño de **VirtualDisk**, **Disk** lo seguirá automáticamente y también cambiará de tamaño.

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

#### <a name="with-storage-tiers"></a>Con capas de almacenamiento

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

### <a name="step-2--resize-the-partition"></a>Paso 2: cambiar el tamaño de la partición

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
- [Planeación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
- [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md)
- [Eliminar volúmenes en Espacios de almacenamiento directo](delete-volumes.md)
