---
ms.assetid: 898d72f1-01e7-4b87-8eb3-a8e0e2e6e6da
title: Agregar servidores o unidades a espacios de almacenamiento directo
ms.author: cosdar
manager: dongill
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2017
description: Cómo agregar servidores o unidades a un clúster de Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: b9a26d3ac982cccf4471f3a3e03bfdae55b55eed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961069"
---
# <a name="adding-servers-or-drives-to-storage-spaces-direct"></a>Agregar servidores o unidades a espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se describe cómo agregar servidores o unidades a espacios de almacenamiento directo.

## <a name="adding-servers"></a><a name="adding-servers"></a>Agregar servidores

Mediante la adición de servidores, también conocida como escalado horizontal, se agrega capacidad de almacenamiento y también se puede mejorar el rendimiento, así como mejorar la eficiencia del almacenamiento. Si tu implementación es hiperconvergida, la adición de servidores también proporciona más recursos de proceso para la carga de trabajo.

![Animación de la adición de un servidor a un clúster de cuatro nodos](media/add-nodes/Scaling-Out.gif)

Las implementaciones típicas son fáciles de escalar horizontalmente mediante la adición de nodos: Tan solo se necesitan realizar dos pasos:

1. Ejecutar el [asistente para validación de clúster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) usando el complemento de clústeres de conmutación por error o con el cmdlet **Test-Cluster** de PowerShell (ejecutar como administrador). Incluya el nuevo servidor *\<NewNode>* que desee agregar.

   ```PowerShell
   Test-Cluster -Node <Node>, <Node>, <Node>, <NewNode> -Include "Storage Spaces Direct", Inventory, Network, "System Configuration"
   ```

   Esto confirma que el nuevo servidor ejecuta Windows Server 2016 Datacenter Edition, se ha unido al mismo dominio de Active Directory Domain Services que los servidores existentes, dispone de todas las características y los roles necesarios y tiene las redes configuradas correctamente.

   >[!IMPORTANT]
   > Si estás volviendo a usar las unidades que contienen datos antiguos o metadatos que ya no necesitas, desactívalos con **Disk Management** o el cmdlet **Reset-PhysicalDisk**. Si se detectan datos antiguos o metadatos, las unidades de disco no se agruparán.

2. Ejecuta el cmdlet siguiente en el clúster para acabar de agregar el servidor:

```
Add-ClusterNode -Name NewNode
```

   >[!NOTE]
   > La agrupación automática depende de que solo tenga un grupo. Si ha omitido la configuración estándar para crear varios grupos, deberá agregar nuevas unidades al grupo preferido mediante **Add-PhysicalDisk**.

### <a name="from-2-to-3-servers-unlocking-three-way-mirroring"></a>Entre 2 y 3 servidores: desbloquear un reflejo triple

![Agregar un tercer servidor a un clúster de dos nodos](media/add-nodes/Scaling-2-to-3.png)

Con dos servidores, solo puedes crear volúmenes reflejados bidireccionales (compáralo con un volumen RAID-1 distribuido). Con tres servidores, puedes crear volúmenes de reflejo triple para una mayor tolerancia a errores. Le recomendamos que use la creación de reflejo triple siempre que sea posible.

Los volúmenes de reflejos dobles no pueden actualizarse en contexto a los reflejos triples. En su lugar, puedes crear un nuevo volumen y migrar (copiar, como mediante el uso de la [réplica de almacenamiento](../storage-replica/server-to-server-storage-replication.md)) los datos a la base de datos y, a continuación, eliminar el volumen antiguo.

Para empezar a crear volúmenes de reflejo triple, existen buenas opciones: Puedes usar lo que prefieras.

#### <a name="option-1"></a>Opción 1

Especificar **PhysicalDiskRedundancy = 2** en cada volumen nuevo tras la creación.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2
```

#### <a name="option-2"></a>Opción 2

En su lugar, puedes establecer **PhysicalDiskRedundancyDefault = 2** en el grupo de **ResiliencySetting**, en el objeto llamado **Mirror**. A continuación, los nuevos volúmenes reflejados utilizará automáticamente *triple* aunque no especifica de creación de reflejos.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Mirror | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size>
```

#### <a name="option-3"></a>Opción 3

Establece **PhysicalDiskRedundancy = 2** en la plantilla **StorageTier** denominada *Capacity* y, después, crear volúmenes mediante referencias a la capa.

```PowerShell
Set-StorageTier -FriendlyName Capacity -PhysicalDiskRedundancy 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Capacity -StorageTierSizes <Size>
```

### <a name="from-3-to-4-servers-unlocking-dual-parity"></a>De 3 a 4 servidores: desbloqueo de la paridad doble

![Agregar un cuarto servidor a un clúster de tres nodos](media/add-nodes/Scaling-3-to-4.png)

Con cuatro servidores puede usar la paridad dual, también denominada codificación de borrado (compárelo con un volumen RAID-6 distribuido). Esto proporciona la misma tolerancia a errores dual que la creación de reflejo triple, pero con una mejor eficiencia de almacenamiento. Para obtener más información, consulta [Eficiencia de almacenamiento y la tolerancia a errores](storage-spaces-fault-tolerance.md).

Si has trabajado en una implementación más pequeña, tienes algunas opciones para empezar a crear volúmenes de paridad doble. Puedes usar lo que prefieras.

#### <a name="option-1"></a>Opción 1

Especificar **PhysicalDiskRedundancy = 2** y **ResiliencySettingName = Parity** en los nuevos volúmenes tras crearlos.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity
```

#### <a name="option-2"></a>Opción 2

Establece **PhysicalDiskRedundancy = 2** en el grupo **ResiliencySetting**, en el objeto llamado **Parity**. A continuación, todos los nuevos volúmenes de paridad utilizarán automáticamente la paridad *dual*, incluso si no se especifica.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Parity | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -ResiliencySettingName Parity
```

Con cuatro servidores, también puede empezar a usar la paridad con aceleración de reflejo, en la que un volumen individual es parte del reflejo y la paridad del elemento.

Para ello, debes actualizar la configuración de las plantillas **StorageTier** para tener las capas *Performance* y *Capacity*, tal y como se hubieran creado si hubieras ejecutado en primer lugar **Enable-ClusterS2D** en cuatro servidores. En concreto, las dos capas deben tener el valor de **MediaType** de los dispositivos de capacidad (por ejemplo, SSD o HDD) y **PhysicalDiskRedundancy = 2**. La capa *Performance* debe ser **ResiliencySettingName = Mirror** y la capa *Capacity* debe ser **ResiliencySettingName = Parity**.

#### <a name="option-3"></a>Opción 3

Tal vez te resulte más sencillo eliminar la plantilla de capa y crear dos nuevas. Esto no afectará a los volúmenes previamente existentes que se crearon mediante la referencia a la plantilla de nivel: es solo una plantilla.

```PowerShell
Remove-StorageTier -FriendlyName Capacity

New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Mirror -FriendlyName Performance
New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity -FriendlyName Capacity
```

Eso es todo. Ahora está listo para crear volúmenes de paridad acelerada para reflejo haciendo referencia a estas plantillas de nivel.

#### <a name="example"></a>Ejemplo

```PowerShell
New-Volume -FriendlyName "Sir-Mix-A-Lot" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes <Size, Size>
```

### <a name="beyond-4-servers-greater-parity-efficiency"></a>Más allá de cuatro servidores: mayor eficiencia de paridad

A medida que escala más allá de cuatro servidores, los volúmenes nuevos se pueden beneficiar de una eficiencia de codificación de paridad cada vez mayor. Por ejemplo, entre seis y siete servidores, la eficiencia mejora de un 50,0 % a un 66,7 %, ya que se puede usar Reed-Solomon 4+2 (en lugar de 2+2). No es necesario realizar ningún paso para empezar a disfrutar de esta nueva eficiencia. La mejor codificación posible se determina automáticamente cada vez que se crea un volumen.

Pero los volúmenes ya existentes *no* se “convertirán” a la codificación nueva más amplia. Una razón es que, para hacerlo, sería necesario realizar un cálculo masivo que afectaría literalmente a *cada fragmento* de toda la implementación. Si quiere que los datos preexistentes se codifiquen con la eficiencia superior, puede migrarlos a volúmenes nuevos.

Para más información, consulta [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md).

### <a name="adding-servers-when-using-chassis-or-rack-fault-tolerance"></a>Agregar servidores al usar la tolerancia a errores de chasis o bastidor

Si la implementación usa la tolerancia a errores de chasis o bastidor, debes especificar el chasis o el bastidor de los servidores nuevos antes de agregarlos al clúster. Esto le indica a Espacios de almacenamiento directo la mejor forma de distribuir los datos para maximizar la tolerancia a errores.

1. Cree un dominio de error temporal para el nodo. para ello, abra una sesión de PowerShell con privilegios elevados y, después, use el siguiente comando, donde *\<NewNode>* es el nombre del nuevo nodo de clúster:

   ```PowerShell
   New-ClusterFaultDomain -Type Node -Name <NewNode>
   ```

2. Mueva este dominio de error temporal al chasis o bastidor en el que se encuentra el nuevo servidor en el mundo real, tal como se especifica en *\<ParentName>* :

   ```PowerShell
   Set-ClusterFaultDomain -Name <NewNode> -Parent <ParentName>
   ```

   Para obtener más información, consulte [Fault domain awareness in Windows Server 2016](../../failover-clustering/fault-domains.md) (Conocimiento de dominio de error en Windows Server 2016).

3. Agrega el servidor al clúster como se describe en [Agregar servidores](#adding-servers). Cuando el servidor nuevo se une al clúster, se asocia automáticamente (mediante su nombre) al dominio de error del marcador de posición.

## <a name="adding-drives"></a><a name="adding-drives"></a>Agregar unidades

Mediante la adición de unidades (también conocida como escalado vertical) se agrega capacidad de almacenamiento y se puede mejorar el rendimiento. Si tienes ranuras disponibles, puedes agregar unidades a cada servidor para expandir la capacidad de almacenamiento sin agregar servidores. Puedes agregar unidades de caché o unidades de capacidad independientemente en cualquier momento.

   >[!IMPORTANT]
   > Te recomendamos encarecidamente que todos los servidores tengan una configuración de almacenamiento idéntica.

![Animación que muestra cómo agregar unidades a un mismo](media/add-nodes/Scale-Up.gif)

Para escalar verticalmente, conecte las unidades y compruebe que Windows las detecta. Deben aparecer en la salida del cmdlet **Get-PhysicalDisk** de PowerShell con la propiedad de **CanPool** establecida como **True**. Si se muestran como **CanPool = False**, puedes ver porqué echando un vistazo a la propiedad **CannotPoolReason**.

```PowerShell
Get-PhysicalDisk | Select SerialNumber, CanPool, CannotPoolReason
```

En un breve período de tiempo, Espacios de almacenamiento directo se reclamarán automáticamente las unidades válidas, se agregarán al bloque de almacenamiento y los volúmenes se [redistribuirán de forma automática en todas las unidades](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deep-dive-the-storage-pool-in-storage-spaces-direct/ba-p/425959). En este momento, ha terminado y está listo para [ampliar los volúmenes](resize-volumes.md) o [crear otros nuevos](create-volumes.md).

Si las unidades no aparecen, busque manualmente si se han producido cambios en el hardware. Esto puede hacerse mediante el **Administrador de dispositivos** en el menú **Acción**. Si contienen datos o metadatos antiguos, considere la posibilidad de volver a formatearlas. Esto puede hacerse con **Disk Management** o con el cmdlet **Reset-PhysicalDisk**.

   >[!NOTE]
   > La agrupación automática depende de que solo tenga un grupo. Si ha omitido la configuración estándar para crear varios grupos, deberá agregar nuevas unidades al grupo preferido mediante **Add-PhysicalDisk**.

## <a name="optimizing-drive-usage-after-adding-drives-or-servers"></a>Optimizar el uso de la unidad después de agregar unidades o servidores

Con el tiempo, a medida que se agregan o se quitan unidades, la distribución de los datos entre las unidades del grupo puede ser desigual. En algunos casos, esto puede dar lugar a que ciertas unidades se llenen mientras otras unidades del grupo tienen un consumo mucho menor.

Para ayudar a mantener la asignación de la unidad incluso en el grupo, Espacios de almacenamiento directo optimiza automáticamente el uso de la unidad después de agregar unidades o servidores al grupo (se trata de un proceso manual para sistemas de espacios de almacenamiento que usan alojamientos de SAS compartidos). La optimización se inicia 15 minutos después de agregar una nueva unidad al grupo. La optimización de grupo se ejecuta como una operación en segundo plano de prioridad baja, por lo que puede tardar horas o días en completarse, especialmente si usa unidades de disco duro de gran tamaño.

La optimización usa dos trabajos: uno denominado *optimizar* y otro denominado *reequilibrio* , y puede supervisar su progreso con el siguiente comando:

```powershell
Get-StorageJob
```

Puede optimizar manualmente un grupo de almacenamiento con el cmdlet [Optimize-StoragePool](/powershell/module/storage/optimize-storagepool?view=win10-ps) . Veamos un ejemplo:

```powershell
Get-StoragePool <PoolName> | Optimize-StoragePool
```
