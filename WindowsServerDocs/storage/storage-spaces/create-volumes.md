---
ms.assetid: a9f229eb-bef4-4231-97d0-0899e17cef32
title: Crear volúmenes en Espacios de almacenamiento directo
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/11/2017
ms.localizationpriority: medium
ms.openlocfilehash: 277a676d8e53a7847d54039aab6607be8e5a78c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823616"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Crear volúmenes en Espacios de almacenamiento directo

>Se aplica a: Windows Server 2016

En este tema se describe cómo crear volúmenes en Espacios de almacenamiento directo con PowerShell o con el Administrador de clústeres de conmutación por error.

   >[!TIP]
   >  Si aún no lo hiciste, echa un vistazo primero a [Planificación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md).

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
3. Seleccione el grupo de almacenamiento y haz clic en **Aceptar**. Se abre el *Asistente para nuevo disco virtual (Espacios de almacenamiento directo)*.
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
