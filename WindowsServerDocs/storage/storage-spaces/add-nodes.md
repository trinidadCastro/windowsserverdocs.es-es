---
ms.assetid: 898d72f1-01e7-4b87-8eb3-a8e0e2e6e6da
title: Agregar servidores o unidades a espacios de almacenamiento directo
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2017
description: Cómo agregar servidores o unidades a un clúster de espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: ae639b920788911dbc16952d7b61aab85b0a391b
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678604"
---
# Agregar servidores o unidades a Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se describe cómo agregar servidores o unidades a Espacios de almacenamiento directo.

## <a name="adding-servers"></a> Agregar servidores

Mediante la adición de servidores, también conocida como escalado horizontal, se agrega capacidad de almacenamiento y también se puede mejorar el rendimiento, así como mejorar la eficiencia del almacenamiento. Si tu implementación es hiperconvergida, la adición de servidores también proporciona más recursos de proceso para la carga de trabajo.

![Animación de agregar un servidor a un clúster de cuatro nodos](media/add-nodes/Scaling-Out.gif)

Las implementaciones típicas son fáciles de escalar horizontalmente mediante la adición de nodos: Tan solo se necesitan realizar dos pasos:

1. Ejecutar el [asistente para validación de clúster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) usando el complemento de clústeres de conmutación por error o con el cmdlet **Test-Cluster** de PowerShell (ejecutado como administrador). Incluir el nuevo servidor *\<NewNode>* que quieras agregar.

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
   > La agrupación automática depende de que solo tenga un grupo. Si has sorteado la configuración estándar para crear varios grupos, tendrás que agregar unidades nuevas a tu grupo preferido mediante **Add-PhysicalDisk**.

### Entre 2 y 3 servidores: desbloquear un reflejo triple

![Agregar un servidor terceros a un clúster de dos nodos](media/add-nodes/Scaling-2-to-3.png)

Con dos servidores, solo puedes crear volúmenes reflejados bidireccionales (compáralo con un volumen RAID-1 distribuido). Con tres servidores, puedes crear volúmenes de reflejo triple para una mayor tolerancia a errores. Te recomendamos que uses la creación de reflejo triple siempre que sea posible.

Los volúmenes de reflejos dobles no pueden actualizarse en contexto a los reflejos triples. En su lugar, puedes crear un nuevo volumen y migrar (copiar, como mediante el uso de la [réplica de almacenamiento](../storage-replica/server-to-server-storage-replication.md)) los datos a la base de datos y, a continuación, eliminar el volumen antiguo.

Para empezar a crear volúmenes de reflejo triple, existen buenas opciones: Puedes usar lo que prefieras. 

#### Opción 1

Especificar **PhysicalDiskRedundancy = 2** en cada volumen nuevo tras la creación.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2
```

#### Opción 2

En su lugar, puedes establecer **PhysicalDiskRedundancyDefault = 2** en el grupo de **ResiliencySetting**, en el objeto llamado **Mirror**. A continuación, los nuevos volúmenes reflejados utilizará automáticamente *triple* aunque no especifica de creación de reflejos.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Mirror | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size>
```

#### Opción 3

Establece **PhysicalDiskRedundancy = 2** en la plantilla **StorageTier** denominada *Capacity* y, después, crear volúmenes mediante referencias a la capa.

```PowerShell
Set-StorageTier -FriendlyName Capacity -PhysicalDiskRedundancy 2 

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Capacity -StorageTierSizes <Size>
```

### De 3 a 4 servidores: desbloqueo de la paridad doble

![Agregar un cuarto servidor a un clúster de tres nodos](media/add-nodes/Scaling-3-to-4.png)

Con cuatro servidores puede usar la paridad dual, también denominada codificación de borrado (compárelo con un volumen RAID-6 distribuido). Esto proporciona la misma tolerancia a errores dual que la creación de reflejo triple, pero con una mejor eficiencia de almacenamiento. Para obtener más información, consulta [Eficiencia de almacenamiento y la tolerancia a errores](storage-spaces-fault-tolerance.md).

Si has trabajado en una implementación más pequeña, tienes algunas opciones para empezar a crear volúmenes de paridad doble. Puedes usar lo que prefieras.

#### Opción 1

Especificar **PhysicalDiskRedundancy = 2** y **ResiliencySettingName = Parity** en los nuevos volúmenes tras crearlos.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity
```

#### Opción 2

Establece **PhysicalDiskRedundancy = 2** en el grupo **ResiliencySetting**, en el objeto llamado **Parity**. A continuación, todos los nuevos volúmenes de paridad utilizarán automáticamente la paridad *dual*, incluso si no se especifica.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Parity | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -ResiliencySettingName Parity
```

Con cuatro servidores también puedes empezar a usar la paridad acelerada por reflejos, en cuyo caso un volumen individual es en parte reflejo y en parte paridad.

Para ello, debes actualizar la configuración de las plantillas **StorageTier** para tener las capas *Performance* y *Capacity*, tal y como se hubieran creado si hubieras ejecutado en primer lugar **Enable-ClusterS2D** en cuatro servidores. En concreto, las dos capas deben tener el valor de **MediaType** de los dispositivos de capacidad (por ejemplo, SSD o HDD) y **PhysicalDiskRedundancy = 2**. La capa *Performance* debe ser **ResiliencySettingName = Mirror** y la capa *Capacity* debe ser **ResiliencySettingName = Parity**.

#### Opción 3

Tal vez te resulte más sencillo eliminar la plantilla de capa y crear dos nuevas. Esto no afectará a los volúmenes ya existentes que se crearon haciendo referencia a la plantilla de capa: es solo una plantilla.

```PowerShell
Remove-StorageTier -FriendlyName Capacity

New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Mirror -FriendlyName Performance
New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity -FriendlyName Capacity
```

Eso es todo. Ya estás listo para crear volúmenes de paridad acelerada por reflejos haciendo referencia a estas plantillas de capa.

#### Ejemplo

```PowerShell
New-Volume -FriendlyName "Sir-Mix-A-Lot" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes <Size, Size> 
```

### Más allá de cuatro servidores: mayor eficiencia de paridad

A medida que escala más allá de cuatro servidores, los volúmenes nuevos se pueden beneficiar de una eficiencia de codificación de paridad cada vez mayor. Por ejemplo, entre seis y siete servidores, la eficiencia mejora de un 50,0 % a un 66,7 %, ya que se puede usar Reed-Solomon 4+2 (en lugar de 2+2). No es necesario realizar ningún paso para empezar a disfrutar de esta nueva eficiencia. La mejor codificación posible se determina automáticamente cada vez que se crea un volumen.

Pero los volúmenes ya existentes *no* se “convertirán” a la codificación nueva más amplia. Una razón es que, para hacerlo, sería necesario realizar un cálculo masivo que afectaría literalmente a *cada fragmento* de toda la implementación. Si quieres que los datos preexistentes se codifiquen con la eficiencia superior, puedes migrarlos a volúmenes nuevos.

Para más información, consulta [Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md).

### Agregar servidores al usar la tolerancia a errores de chasis o bastidor

Si la implementación usa la tolerancia a errores de chasis o bastidor, debes especificar el chasis o el bastidor de los servidores nuevos antes de agregarlos al clúster. Esto le indica a Espacios de almacenamiento directo la mejor forma de distribuir los datos para maximizar la tolerancia a errores.

1. Crea un dominio de error temporal para el nodo. Para ello, abre una sesión de PowerShell con privilegios elevados y después usa el comando siguiente, donde *\<NewNode>* es el nombre del nuevo nodo de clúster:

   ```PowerShell
   New-ClusterFaultDomain -Type Node -Name <NewNode> 
   ```

2. Mueve este dominio de error temporal al chasis o al bastidor donde se encuentra en realidad el nuevo servidor, como especifica *\<ParentName>*:

   ```PowerShell
   Set-ClusterFaultDomain -Name <NewNode> -Parent <ParentName> 
   ```

   Para obtener más información, consulta [Conocimiento de dominio de error en Windows Server 2016](../../failover-clustering/fault-domains.md).

3. Agrega el servidor al clúster como se describe en [Agregar servidores](#adding-servers). Cuando el servidor nuevo se une al clúster, se asocia automáticamente (mediante su nombre) al dominio de error del marcador de posición.

## <a name="adding-drives"></a> Agregar unidades

Mediante la adición de unidades (también conocida como escalado vertical) se agrega capacidad de almacenamiento y se puede mejorar el rendimiento. Si tienes ranuras disponibles, puedes agregar unidades a cada servidor para expandir la capacidad de almacenamiento sin agregar servidores. Puedes agregar unidades de caché o unidades de capacidad independientemente en cualquier momento.

   >[!IMPORTANT]
   > Te recomendamos encarecidamente que todos los servidores tengan una configuración de almacenamiento idéntica.

![Animación que muestra la adición de unidades a un sistema](media/add-nodes/Scale-Up.gif)

Para escalar verticalmente, conecta las unidades y comprueba que Windows las detecta. Deben aparecer en la salida del cmdlet **Get-PhysicalDisk** de PowerShell con la propiedad de **CanPool** establecida como **True**. Si se muestran como **CanPool = False**, puedes ver porqué echando un vistazo a la propiedad **CannotPoolReason**.

```PowerShell
Get-PhysicalDisk | Select SerialNumber, CanPool, CannotPoolReason
```

Al poco tiempo, Espacios de almacenamiento directo reclamará automáticamente las unidades válidas, las agregará al grupo de almacenamiento, y los volúmenes [se redistribuirán automáticamente de manera uniforme entre todas las unidades.](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/). En este punto, has terminado y estás listo para [ampliar los volúmenes](resize-volumes.md) o [crea otros nuevos](create-volumes.md).

Si las unidades no aparecen, busca manualmente si se han producido cambios en el hardware. Esto puede hacerse mediante el **Administrador de dispositivos** en el menú **Acción**. Si contienen datos o metadatos antiguos, considera la posibilidad de volver a formatearlas. Esto puede hacerse con **Disk Management** o con el cmdlet **Reset-PhysicalDisk**.

   >[!NOTE]
   > La agrupación automática depende de que solo tengas un grupo. Si has sorteado la configuración estándar para crear varios grupos, tendrás que agregar unidades nuevas a tu grupo preferido mediante **Add-PhysicalDisk**.

## Optimizar el uso de la unidad después de agregar servidores o unidades

Con el tiempo, como las unidades se agregan o eliminan, la distribución de los datos entre las unidades en el grupo puede ser desigual. En algunos casos, esto puede provocar ciertas unidades llenen, mientras que otras unidades en grupo tengan mucho menor consumo.

Para ayudar a mantener la asignación de unidad incluso en todo el grupo, espacios de almacenamiento directo optimiza automáticamente el uso de la unidad después de agregar servidores o unidades al grupo (este es un proceso manual de los sistemas de espacios de almacenamiento que usan contenedores SAS compartidos). Optimización inicia 15 minutos después de agregar una nueva unidad al grupo. Optimización de grupo se ejecuta como una operación en segundo plano de baja prioridad, por lo que puede tardar horas o días en completarse, especialmente si estás usando unidades de disco duros grandes.

Optimización usa dos trabajos, uno llamado *optimizar* y *equilibrar* la función de llamada de una, y puede supervisar su progreso con el siguiente comando:

```powershell
Get-StorageJob
```

Puede optimizar un grupo de almacenamiento con el cmdlet [Optimize-StoragePool](https://docs.microsoft.com/powershell/module/storage/optimize-storagepool?view=win10-ps) manualmente. A continuación te mostramos un ejemplo:

```powershell
Get-StoragePool <PoolName> | Optimize-StoragePool
```
