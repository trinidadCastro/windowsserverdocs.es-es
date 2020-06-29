---
title: Crear volúmenes en Espacios de almacenamiento directo
description: Cómo crear volúmenes en Espacios de almacenamiento directo mediante el centro de administración de Windows y PowerShell.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 02/25/2020
ms.openlocfilehash: 40750acb260335e858a7763c950dfc4ad2cd7979
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473832"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Crear volúmenes en Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se describe cómo crear volúmenes en un clúster de Espacios de almacenamiento directo mediante el centro de administración de Windows y PowerShell.

> [!TIP]
> Si todavía no lo ha hecho, consulte en primer lugar la [planeación de los volúmenes en espacios de almacenamiento directo](plan-volumes.md) .

## <a name="create-a-three-way-mirror-volume"></a>Crear un volumen de reflejo triple

Para crear un volumen de reflejo triple en el centro de administración de Windows:

1. En el centro de administración de Windows, conéctese a un clúster de Espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** en el panel **herramientas** .
2. En la página volúmenes, seleccione la pestaña **inventario** y, a continuación, seleccione **crear volumen**.
3. En el panel **crear volumen** , escriba un nombre para el volumen y deje **resistencia** como **reflejo triple**.
4. En **tamaño en HDD**, especifique el tamaño del volumen. Por ejemplo, 5 TB (terabytes).
5. Seleccione **Crear**.

En función del tamaño, la creación del volumen puede tardar unos minutos. Las notificaciones en la parte superior derecha le indicarán Cuándo se crea el volumen. El nuevo volumen aparece en la lista de inventario.

Vea un vídeo rápido sobre cómo crear un volumen de reflejo triple.

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>Crear un volumen de paridad acelerado para reflejo

La paridad acelerada para reflejo reduce la superficie del volumen en el HDD. Por ejemplo, un volumen de reflejo triple significa que, para cada 10 terabytes de tamaño, necesitará 30 terabytes como superficie. Para reducir la sobrecarga en la superficie, cree un volumen con paridad acelerada para el reflejo. Esto reduce la superficie de 30 terabytes hasta solo 22 terabytes, incluso con 4 servidores, mediante la creación de reflejo del 20% de los datos más activos y el uso de la paridad, que es más eficaz para almacenar el resto. Puede ajustar esta relación de paridad y reflejo para lograr el equilibrio entre el rendimiento y la capacidad adecuado para su carga de trabajo. Por ejemplo, la paridad del 90 por ciento y el reflejo del 10 por ciento producen menos rendimiento, pero optimiza aún más la superficie.

Para crear un volumen con paridad acelerada para reflejo en el centro de administración de Windows:

1. En el centro de administración de Windows, conéctese a un clúster de Espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** en el panel **herramientas** .
2. En la página volúmenes, seleccione la pestaña **inventario** y, a continuación, seleccione **crear volumen**.
3. En el panel **crear volumen** , escriba un nombre para el volumen.
4. En **resistencia**, seleccione **paridad con aceleración de reflejo**.
5. En **porcentaje de paridad**, seleccione el porcentaje de paridad.
6. Seleccione **Crear**.

Vea un vídeo rápido sobre cómo crear un volumen de paridad acelerado para reflejo.

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>Abrir volumen y agregar archivos

Para abrir un volumen y agregar archivos al volumen en el centro de administración de Windows:

1. En el centro de administración de Windows, conéctese a un clúster de Espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** en el panel **herramientas** .
2. En la página volúmenes, seleccione la pestaña **inventario** .
2. En la lista de volúmenes, seleccione el nombre del volumen que desea abrir.

    En la página Detalles del volumen, puede ver la ruta de acceso al volumen.

4. En la parte superior de la página, seleccione **abrir**. Esto inicia la herramienta archivos en el centro de administración de Windows.
5. Navegue hasta la ruta de acceso del volumen. Aquí puede examinar los archivos del volumen.
6. Seleccione **cargar**y, a continuación, seleccione un archivo para cargar.
7. Use el botón **atrás** del explorador para volver al panel herramientas del centro de administración de Windows.

Vea un vídeo rápido sobre cómo abrir un volumen y agregar archivos.

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>Activar la desduplicación y la compresión

La desduplicación y la compresión se administran por volumen. La desduplicación y la compresión utilizan un modelo de procesamiento posterior, lo que significa que no verá ahorros hasta que se ejecute. Cuando lo hace, funcionará en todos los archivos, incluso en los que estaban allí.

1. En el centro de administración de Windows, conéctese a un clúster de Espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** en el panel **herramientas** .
2. En la página volúmenes, seleccione la pestaña **inventario** .
3. En la lista de volúmenes, seleccione el nombre del volumen que desea administrar.
4. En la página Detalles del volumen, haga clic en el conmutador con la etiqueta **desduplicación y compresión**.
5. En el panel habilitar desduplicación, seleccione el modo de desduplicación.

    En lugar de configuraciones complicadas, el centro de administración de Windows permite elegir entre perfiles preparados para diferentes cargas de trabajo. Si no está seguro, use la configuración predeterminada.

6. Seleccione **Habilitar**.

Vea un vídeo rápido sobre cómo activar la desduplicación y la compresión.

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>Creación de volúmenes con PowerShell

Se recomienda usar el cmdlet **New-Volume** para crear volúmenes para espacios de almacenamiento directo. Ofrece la experiencia más rápida y sencilla. Este cmdlet crea automáticamente el disco virtual, las particiones y los formatea, crea el volumen con el nombre correspondiente y lo agrega a los volúmenes compartidos del clúster, todo en un solo paso.

El cmdlet **New-Volume** tiene cuatro parámetros que debe proporcionar siempre:

- **FriendlyName:** cualquier cadena que desee, por ejemplo *"Volume1"* .
- **FileSystem:** **CSVFS_ReFS** (recomendado) o **CSVFS_NTFS**.
- **StoragePoolFriendlyName:** el nombre del bloque de almacenamiento; por ejemplo *"S2D en ClusterName"* .
- **Size:** el tamaño del volumen, por ejemplo *"10TB"* .

   > [!NOTE]
   > Windows, incluido PowerShell, cuenta con números binarios (en base 2), mientras que las unidades suelen etiquetarte con números decimales (en base 10). Esto explica por qué una unidad de "un terabyte", definida como 1 billón de bytes, aparece en Windows como unos "909 GB". Se espera que esto sea así. Al crear volúmenes mediante **New-Volume**, debe especificar el parámetro **Size** (Tamaño) en números binarios (en base 2). Por ejemplo, si se especifica "909GB" o "0,909495TB", se creará un volumen de aproximadamente 1 billón de bytes.

### <a name="example-with-2-or-3-servers"></a>Ejemplo: Con dos o tres servidores

Para facilitar las cosas, si la implementación tiene solo dos servidores, Espacios de almacenamiento directo usará automáticamente la resistencia de reflejo bidireccional. Si la implementación tiene solo tres servidores, usará automáticamente el reflejo triple.

```PowerShell
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Ejemplo: Con más de cuatro servidores

Si tiene cuatro o más servidores, puede usar el parámetro opcional **ResiliencySettingName** para elegir el tipo de resistencia.

-   **ResiliencySettingName:** **Mirror** o **Parity**.

En el ejemplo siguiente, *"Volume2"* usa el reflejo triple y *"Volume3"* usa paridad dual (también conocidad como "codificación de borrado").

```PowerShell
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Ejemplo: Uso de capas de almacenamiento

En las implementaciones con tres tipos de unidades, un volumen puede abarcar las capas SSD y HDD, y residir parcialmente en cada uno. Del mismo modo, en implementaciones con cuatro o más servidores, un volumen puede mezclar reflejo y paridad dual para residir parcialmente en cada uno de ellos.

Para ayudarle a crear estos volúmenes, Espacios de almacenamiento directo proporciona plantillas de capa predeterminadas llamadas *Performance* (Rendimiento) y *Capacity* (Capacidad). Incluyen definiciones para la creación de reflejo triple en las unidades más rápidas (si procede) y paridad dual en las unidades más lentas (si procede).

Para verlos, puede ejecutar el cmdlet **Get-StorageTier**.

```PowerShell
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![Captura de pantalla de las capas de almacenamiento de PowerShell](media/creating-volumes/storage-tiers-screenshot.png)

Para crear volúmenes en capas, haga referencia a estas plantillas de capa con los parámetros **StorageTierFriendlyNames** y **StorageTierSizes** del cmdlet **New-Volume**. Por ejemplo, el siguiente cmdlet crea un volumen que combina reflejo triple y paridad dual en una proporción 30:70.

```PowerShell
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

¡Y ya está! Repita el procedimiento si necesita crear más de un volumen.

## <a name="additional-references"></a>Referencias adicionales

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Planeación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
- [Extensión de volúmenes en Espacios de almacenamiento directo](resize-volumes.md)
- [Eliminar volúmenes en Espacios de almacenamiento directo](delete-volumes.md)
