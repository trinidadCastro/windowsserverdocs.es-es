---
title: Crear volúmenes en Espacios de almacenamiento directo
description: Cómo crear volúmenes en espacios de almacenamiento directo con Windows Admin Center y PowerShell.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 05/09/2019
ms.openlocfilehash: d7c842a9b393f67c482dadeaa4090627887a67a3
ms.sourcegitcommit: 75f257d97d345da388cda972ccce0eb29e82d3bc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65613216"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Crear volúmenes en Espacios de almacenamiento directo

>Se aplica a: Windows Server 2019, Windows Server 2016

Este tema describe cómo crear volúmenes en un clúster de espacios de almacenamiento directo con Windows Admin Center, PowerShell o administrador de clústeres de conmutación por error.

   >[!TIP]
   >  Si aún no lo hiciste, echa un vistazo primero a [Planificación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md).

## <a name="create-a-three-way-mirror-volume"></a>Crear un volumen triple

Para crear un volumen triple en Windows Admin Center: 

1. En Windows Admin Center, conectarse a un clúster de espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** desde el **herramientas** panel.
2. En la página de volúmenes, seleccione el **inventario** pestaña y, a continuación, seleccione **crear volumen**.
3. En el **crear volumen** panel, escriba un nombre para el volumen y dejar **resistencia** como **triple**.
4. En **tamaño en disco duro**, especifique el tamaño del volumen. Por ejemplo, 5 TB (terabytes).
5. Selecciona **Crear**.

Según el tamaño, crear el volumen puede tardar unos minutos. Las notificaciones en la esquina superior derecha le permitirá saber cuándo se crea el volumen. El nuevo volumen aparece en la lista de inventario.

Vea un breve vídeo sobre cómo crear un volumen triple.

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>Crear un volumen reflejado acelerada paridad

Paridad acelerada reflejado reduce el espacio del volumen en el disco duro. Por ejemplo, un volumen triple significaría que por cada 10 terabytes de tamaño, necesitará 30 terabytes como superficie. Para reducir la sobrecarga de la superficie, cree un volumen con paridad acelerada reflejado. Esto reduce la superficie de terabytes de 30 a 22 simplemente terabytes, incluso con solo 4 servidores, mediante la creación de reflejo el 20 por ciento más activos de datos y el uso de paridad, que es más eficaz, para almacenar el resto del espacio. Puede ajustar esta relación de paridad y el reflejo para hacer que el rendimiento en comparación con equilibrio de la capacidad adecuada para la carga de trabajo. Por ejemplo, reflejado de paridad y el 10 por ciento del 90 por ciento menor, el rendimiento, pero simplifica aún más la superficie.

Para crear un volumen con paridad acelerada reflejado en Windows Admin Center:

1. En Windows Admin Center, conectarse a un clúster de espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** desde el **herramientas** panel.
2. En la página de volúmenes, seleccione el **inventario** pestaña y, a continuación, seleccione **crear volumen**.
3. En el **crear volumen** panel, escriba un nombre para el volumen.
4. En **resistencia**, seleccione **acelerada reflejado paridad**.
5. En **porcentaje paridad**, seleccione el porcentaje de paridad.
6. Selecciona **Crear**.

Vea un breve vídeo sobre cómo crear un volumen reflejado acelerada de paridad.

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>Abra el volumen y agregar archivos

Para abrir un volumen y agregar los archivos en el volumen en Windows Admin Center:

1. En Windows Admin Center, conectarse a un clúster de espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** desde el **herramientas** panel.
2. En la página de volúmenes, seleccione el **inventario** ficha.
2. En la lista de volúmenes, seleccione el nombre del volumen que desea abrir.

    En la página de detalles de volumen, puede ver la ruta de acceso al volumen.

4. En la parte superior de la página, seleccione **abierto**. Esto inicia la herramienta de archivos en Windows Admin Center.
5. Vaya a la ruta de acceso del volumen. Aquí puede examinar los archivos en el volumen.
6. Seleccione **cargar**y, a continuación, seleccione un archivo para cargarlo.
7. Utilice el explorador **volver** botón para volver al panel de herramientas de Windows Admin Center.

Vea un breve vídeo sobre cómo abrir un volumen y agregar archivos.

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>Activar la compresión y desduplicación

Desduplicación y compresión se administra por volumen. Desduplicación y compresión utiliza un modelo de procesamiento posterior, lo que significa que no verá ahorro hasta que se ejecuta. Cuando lo haga, trabajará en todos los archivos, incluso aquellos que estaban allí desde antes.

1. En Windows Admin Center, conectarse a un clúster de espacios de almacenamiento directo y, a continuación, seleccione **volúmenes** desde el **herramientas** panel.
2. En la página de volúmenes, seleccione el **inventario** ficha.
3. En la lista de volúmenes, seleccione el nombre del volumen que desea administrar.
4. En la página de detalles de volumen, haga clic en el interruptor denominado **desduplicación y compresión**.
5. En el panel de habilitar la desduplicación, seleccione el modo de desduplicación.

    En lugar de configuración complicada, Windows Admin Center le permite elegir entre listas para usar perfiles para diferentes cargas de trabajo. Si no está seguro, use la configuración predeterminada.

6. Selecciona **Habilitar**.

Vea un breve vídeo sobre cómo activar la compresión y desduplicación.

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>Crear volúmenes con PowerShell

Te recomendamos usar el cmdlet **New-Volume** para crear volúmenes para Espacios de almacenamiento directo. Proporciona la experiencia más rápida y sencilla. Este cmdlet crea automáticamente por sí solo el disco virtual, lo particiona y formatea, crea el volumen con nombre coincidente y lo agrega a los volúmenes compartidos de clúster: todo en un paso sencillo.

El cmdlet **New-Volume** tiene cuatro parámetros que siempre tendrás que proporcionar:

- **FriendlyName:** Cualquier cadena que desee, por ejemplo *"Volume1"*
- **FileSystem:** Cualquier **CSVFS_ReFS** (recomendado) o **CSVFS_NTFS**
- **StoragePoolFriendlyName:** El nombre de su almacenamiento de grupo, por ejemplo *"S2D en ClusterName"*
- **Tamaño:** El tamaño del volumen, por ejemplo *"10 TB"*

   >[!NOTE]
   >  Windows, incluido PowerShell, cuenta con números binarios (base 2), mientras que las unidades a menudo se etiquetan con números decimales (base 10). Esto explica por qué una unidad de "un terabyte", definida como 1 000 000 000 000 bytes, en Windows aparece como aproximadamente "909 GB". Esto es de esperar. Al crear volúmenes con **New-Volume**, debes especificar el parámetro **Size** en números binarios (base 2). Por ejemplo, especificar "909GB" o "0.909495 TB" creará un volumen de aproximadamente 1 000 000 000 000 bytes.

### <a name="example-with-2-or-3-servers"></a>Por ejemplo: Con los servidores 2 ó 3

Para facilitar las cosas, si la implementación tiene tan solo dos servidores, Espacios de almacenamiento directo usará automáticamente la creación de reflejos dobles para resistencia. Si la implementación tiene solo tres servidores, usará automáticamente la creación de reflejos triple.

```
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Por ejemplo: Con los servidores de 4 +

Si tienes cuatro servidores o más, puedes usar el parámetro **ResiliencySettingName** opcional para elegir el tipo de resistencia.

-   **ResiliencySettingName:** Cualquier **reflejado** o **paridad**.

En el siguiente ejemplo, *"Volume2"* usa la creación de reflejos triple, y *"Volume3"* usa paridad dual (a menudo llamada "codificación de borrado").

```
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Por ejemplo: Uso de capas de almacenamiento

En las implementaciones con tres tipos de unidades, un volumen puede abarcar las capas SSD y HDD para residir parcialmente en cada una. De igual modo, en las implementaciones con cuatro servidores o más, un volumen puede combinar creación de reflejos y paridad dual para residir parcialmente en cada una.

Para ayudarte a crear estos volúmenes, Espacios de almacenamiento directo proporciona plantillas de capa predeterminadas llamadas *Rendimiento* y *Capacidad*. Estas encapsulan las definiciones para la creación de reflejos triple en las unidades de capacidad más rápida (si procede) y paridad dual en las unidades de capacidad más lenta (si procede).

Para verlas, puedes ejecutar el cmdlet **Get-StorageTier**.

```
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![Captura de pantalla de capas de almacenamiento en PowerShell](media/creating-volumes/storage-tiers-screenshot.png)

Para crear volúmenes en capas, haz referencia a estas plantillas de capas con los parámetros **StorageTierFriendlyNames** y **StorageTierSizes** del cmdlet **New-Volume**. Por ejemplo, el siguiente cmdlet crea un volumen que combina la creación de reflejos triple y la paridad dual en proporciones de 30:70.

```
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

## <a name="create-volumes-using-failover-cluster-manager"></a>Crear volúmenes con el Administrador de clústeres de conmutación por error

También puedes crear volúmenes con el *Asistente para nuevo disco virtual (Espacios de almacenamiento directo)* seguido por el *Asistente para nuevo volumen* desde el Administrador de clústeres de conmutación por error, si bien este flujo de trabajo tiene muchos más pasos manuales y no se recomienda.

Hay tres pasos principales:

### <a name="step-1-create-virtual-disk"></a>Paso 1: Crear disco virtual

![Nuevo disco Virtual](media/creating-volumes/GUI-Step-1.png)

1. En el Administrador de clústeres de conmutación por error, navega a **Almacenamiento** -> **Grupos**.
2. Selecciona **Nuevo disco virtual** desde el panel Acciones a la derecha, o haz clic con el botón derecho en el grupo y selecciona **Nuevo disco virtual**.
3. Seleccione el grupo de almacenamiento y haz clic en **Aceptar**. Se abre el *Asistente para nuevo disco virtual (Espacios de almacenamiento directo)* .
4. Usa el asistente para ponerle nombre al disco virtual y especificar su tamaño.
5. Revisa las opciones seleccionadas y haz clic en **Crear**.
6. Asegúrate de marcar la casilla **Crear un volumen cuando este asistente se cierre** antes de cerrar.

### <a name="step-2-create-volume"></a>Paso 2: Crear volumen

Se abre el *Asistente para nuevo volumen*.

7. Selecciona el disco virtual que acabas de crear y haz clic en **Siguiente**.
8. Especifica el tamaño del volumen (valor predeterminado: el mismo tamaño que el disco virtual) y haz clic en **Siguiente**. 
9. Asigna el volumen a una letra de unidad o elige **No asignar a una letra de unidad** y haz clic en **Siguiente**.
10. Especifica el sistema de archivos que se usará, deja el tamaño de unidad de asignación como *Predeterminado*, ponle nombre al volumen y haz clic en **Siguiente**.
11. Revisa las opciones seleccionadas y haz clic en **Crear** y luego en **Cerrar**.

### <a name="step-3-add-to-cluster-shared-volumes"></a>Paso 3: Agregar a volúmenes compartidos de clúster

![Agregar a volúmenes compartidos de clúster](media/creating-volumes/GUI-Step-2.png)

12. En el Administrador de clústeres de conmutación por error, navega a **Almacenamiento** -> **Discos**.
13. Selecciona el disco virtual que acabas de crear y selecciona **Agregar a volúmenes compartidos de clúster** desde el panel Acciones a la derecha, o haz clic con el botón derecho en el disco virtual y selecciona **Agregar a volúmenes compartidos de clúster**.

¡Has terminado! Repite según sea necesario para crear más de un volumen.

## <a name="see-also"></a>Vea también

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
- [Planificación de volúmenes en espacios de almacenamiento directo](plan-volumes.md)
- [Ampliación de volúmenes en espacios de almacenamiento directo](resize-volumes.md)
- [Eliminando los volúmenes en espacios de almacenamiento directo](delete-volumes.md)
