---
title: Extensión de volúmenes en Espacios de almacenamiento directo
description: Cómo cambiar el tamaño de los volúmenes en Espacios de almacenamiento directo mediante el centro de administración de Windows y PowerShell.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 03/10/2020
ms.openlocfilehash: 4526bdc87bfbb8cdaf6cc3b0e8f3cd1cd80f4a9d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474612"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Extensión de volúmenes en Espacios de almacenamiento directo
> Se aplica a: Windows Server 2019, Windows Server 2016

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

### <a name="capacity-in-the-storage-pool"></a>Capacidad en el bloque de almacenamiento

Antes de cambiar el tamaño de un volumen, asegúrese de que tiene suficiente capacidad en el grupo de almacenamiento para acomodar la nueva y mayor superficie. Por ejemplo, al cambiar el tamaño de un volumen de reflejo triple de 1 TB a 2 TB, su superficie crecería de 3 TB a 6 TB. Para que el tamaño se realice correctamente, necesitaría al menos (6-3) = 3 TB de capacidad disponible en el grupo de almacenamiento.

### <a name="familiarity-with-volumes-in-storage-spaces"></a>Familiaridad con los volúmenes de los espacios de almacenamiento

En Espacios de almacenamiento directo, cada volumen consta de varios objetos apilados: el volumen compartido de clúster (CSV), que es un volumen; la partición; el disco, que es un disco virtual. y uno o más niveles de almacenamiento (si procede). Para cambiar el tamaño de un volumen, deberá cambiar el tamaño de algunos de estos objetos.

![volúmenes en-SMAPI](media/resize-volumes/volumes-in-smapi.png)

Para familiarizarse con ellos, intente ejecutar **Get-** con el nombre correspondiente en PowerShell.

Por ejemplo:

```PowerShell
Get-VirtualDisk
```

Para seguir las asociaciones entre los objetos de la pila, canalice un cmdlet **Get-** cmdlet en el siguiente.

Por ejemplo, aquí se muestra cómo obtener un disco virtual hasta su volumen:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume
```

### <a name="step-1--resize-the-virtual-disk"></a>Paso 1: cambiar el tamaño del disco virtual

El disco virtual puede usar capas de almacenamiento, o no, en función de cómo se haya creado.

Para comprobarlo, ejecute el siguiente cmdlet:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier
```

Si el cmdlet no devuelve nada, el disco virtual no usa capas de almacenamiento.

#### <a name="no-storage-tiers"></a>No hay niveles de almacenamiento

Si el disco virtual no tiene capas de almacenamiento, puede cambiar su tamaño directamente mediante el cmdlet **Resize-VirtualDisk** .

Proporcione el nuevo tamaño en el parámetro **-size** .

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Al cambiar el tamaño de **VirtualDisk**, el **disco** sigue automáticamente y cambia de tamaño.

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

#### <a name="with-storage-tiers"></a>Con capas de almacenamiento

Si el disco virtual usa capas de almacenamiento, puede cambiar el tamaño de cada nivel por separado mediante el cmdlet **Resize-StorageTier** .

Para obtener los nombres de las capas de almacenamiento, siga las asociaciones del disco virtual.

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier | Select FriendlyName
```

A continuación, para cada nivel, proporcione el nuevo tamaño en el parámetro **-size** .

```PowerShell
Get-StorageTier <FriendlyName> | Resize-StorageTier -Size <Size>
```

> [!TIP]
> Si los niveles son diferentes tipos de medios físicos (como **mediatype = SSD** y **mediatype = HDD**), debe asegurarse de que tiene suficiente capacidad de cada tipo de medio en el grupo de almacenamiento para acomodar la nueva superficie más grande de cada nivel.

Al cambiar el tamaño de los **StorageTier**, el **VirtualDisk** y el **disco** siguen automáticamente y también se cambia el tamaño.

![Resize-StorageTier](media/resize-volumes/Resize-StorageTier.gif)

### <a name="step-2--resize-the-partition"></a>Paso 2: cambiar el tamaño de la partición

A continuación, cambie el tamaño de la partición mediante el cmdlet **Resize-Partition** . Se espera que el disco virtual tenga dos particiones: la primera está reservada y no debe modificarse; el que necesita para cambiar de tamaño tiene **PartitionNumber = 2** y **Type = Basic**.

Proporcione el nuevo tamaño en el parámetro **-size** . Se recomienda usar el tamaño máximo admitido, como se muestra a continuación.

```PowerShell
# Choose virtual disk
$VirtualDisk = Get-VirtualDisk <FriendlyName>

# Get its partition
$Partition = $VirtualDisk | Get-Disk | Get-Partition | Where PartitionNumber -Eq 2

# Resize to its maximum supported size
$Partition | Resize-Partition -Size ($Partition | Get-PartitionSupportedSize).SizeMax
```

Al cambiar el tamaño de la **partición**, el **volumen** y **ClusterSharedVolume** siguen automáticamente y también se cambia el tamaño.

![Resize-Partition](media/resize-volumes/Resize-Partition.gif)

Eso es todo.

> [!TIP]
> Para comprobar que el volumen tiene el nuevo tamaño, ejecute **Get-Volume**.

## <a name="additional-references"></a>Referencias adicionales

- [Espacios de almacenamiento directo en Windows Server 2016](storage-spaces-direct-overview.md)
- [Planeación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
- [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md)
- [Eliminar volúmenes en Espacios de almacenamiento directo](delete-volumes.md)
