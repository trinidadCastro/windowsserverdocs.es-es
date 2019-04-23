---
title: Implementar espacios de almacenamiento en un servidor independiente
description: Describe cómo implementar espacios de almacenamiento en un servidor independiente basado en Windows Server 2012.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 40265e767ac9aca05386c0893def259aca3a5633
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868276"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Implementar espacios de almacenamiento en un servidor independiente

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describe cómo implementar espacios de almacenamiento en un servidor independiente. Para obtener información sobre cómo crear un espacio de almacenamiento en clúster, consulte [implementar un clúster de espacios de almacenamiento en Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Para crear un espacio de almacenamiento, debes crear primero uno o varios grupos de almacenamiento. Un grupo de almacenamiento es una colección de discos físicos. Un grupo de almacenamiento permite agregar almacenamiento, expandir la capacidad flexible y delegar la administración.

Desde un grupo de almacenamiento, puedes crear uno o varios discos virtuales. Los discos virtuales también se denominan *espacios de almacenamiento*. Un espacio de almacenamiento aparece en el sistema operativo Windows como un disco normal desde el que puedes crear volúmenes formateados. Cuando creas un disco virtual a través de la interfaz de usuario de Servicios de archivos y almacenamiento, puedes configurar el tipo de resistencia (simple, reflejo o paridad), el tipo de aprovisionamiento (fino o fijo) y el tamaño. Mediante Windows PowerShell, puedes configurar otros parámetros, como el número de columnas, el valor de intercalación y qué discos físicos del grupo deben usarse. Para obtener información acerca de estos parámetros adicionales, consulte [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) y [¿cuáles son las columnas y cómo decide espacios de almacenamiento cuántas usar?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) en almacenamiento de espacios de preguntas más frecuentes (P+F).

>[!NOTE]
>No se puede usar un espacio de almacenamiento para hospedar el sistema operativo Windows.

Desde un disco virtual, puedes crear uno o varios volúmenes. Cuando se crea un volumen, puede configurar el tamaño, letra de unidad o carpeta, el sistema de archivos (sistema de archivos NTFS o sistema de archivos resistente (ReFS)), el tamaño de unidad de asignación y una etiqueta de volumen opcional.

La siguiente figura ilustra el flujo de trabajo de los Espacios de almacenamiento.

![Flujo de trabajo de Espacios de almacenamiento](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: Flujo de trabajo de espacios de almacenamiento**

>[!NOTE]
>Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulte [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Requisitos previos

Para usar espacios de almacenamiento en un servidor independiente de 2012−based de Windows Server, asegúrese de que los discos físicos que desea utilizar cumplan los siguientes requisitos previos.

> [!IMPORTANT]
> Si desea obtener información sobre cómo implementar espacios de almacenamiento en un clúster de conmutación por error, consulte [implementar un clúster de espacios de almacenamiento en Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Implementación de un clúster de conmutación por error tiene diferentes requisitos previos, como tipos de bus de disco compatibles, tipos de resistencia compatibles y el número mínimo necesario de discos.

|Área|Requisitos|Notas|
|---|---|---|
|Tipos de bus de disco|-Serial Attached SCSI (SAS)<br>-Serie Advanced Technology Attachment (SATA)<br>-iSCSI y Fibre Channel controladores. |También puedes usar unidades USB. Sin embargo, no resulta óptimo usar unidades USB en un entorno de servidor.<br>Se admite espacios de almacenamiento de iSCSI y los controladores de canal de fibra (FC) siempre que los discos virtuales que se crean encima de ellos son que no sean resistentes (Simple con cualquier número de columnas).<br>|
|Configuración del disco|-Discos físicos deben ser al menos 4 GB<br>-Los discos deben estar vacíos y no tener formato. No crees volúmenes.||
|Consideraciones sobre HBA|-Se recomiendan los adaptadores de bus host simple (HBA) que no admiten la funcionalidad RAID<br>-Si son compatibles con RAID, HBA debe estar en modo de no RAID con todas las funciones RAID deshabilitadas<br>-Adaptadores no deben resumir los discos físicos, los datos en caché, o interferir con los dispositivos conectados. Esto incluye los servicios de contenedor proporcionados por los dispositivos de un grupo de discos (JBOD) conectados. |Los Espacios de almacenamiento son compatibles con HBA solo si se pueden deshabilitar por completo todas las funciones RAID.|
|Contenedores JBOD|-Contenedores JBOD son opcionales<br>-Se recomienda utilizar espacios de almacenamiento certified alojamientos aparece en el catálogo de Windows Server<br>-Si usa un contenedor JBOD, compruebe con su proveedor de almacenamiento que el contenedor admite espacios de almacenamiento para garantizar una funcionalidad completa<br>-Para determinar si el contenedor JBOD es compatible con la identificación y ranuras, ejecute el siguiente cmdlet de Windows PowerShell:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Si el **EnclosureNumber** y **SlotNumber** campos contienen valores, a continuación, el contenedor admite estas características.||

Para planear el número de discos físicos y el tipo de resistencia deseado para una implementación de servidor independiente, usa las siguientes directrices.

|Tipo de resistencia|Requisitos de disco|Cuándo se debe utilizar|
|---|---|---|
|**Simple**<br><br>-Secciona datos en discos físicos<br>-Maximiza la capacidad de disco y aumenta el rendimiento<br>-Sin resistencia (no protege contra errores de disco)<br><br><br><br><br><br><br>|Necesita por lo menos un disco físico.|No debe usarse para hospedar datos irreemplazables. Espacios simples no protegen frente a errores de disco.<br><br>Se usa para hospedar datos temporales o que puedan crearse de nuevo fácilmente a bajo costo.<br><br>Adecuado para cargas de trabajo de alto rendimiento donde la resistencia no es necesaria o ya proporciona la aplicación.|
|**Reflejo**<br><br>-Almacena dos o tres copias de los datos en todo el conjunto de discos físicos<br>-Aumenta la fiabilidad pero reduce la capacidad. Se produce duplicación con cada escritura. Un espacio de reflejo también secciona los datos en varias unidades físicas.<br>-Mayor rendimiento de datos y una menor latencia de acceso que paridad<br>-Usa desfasadas (DRT) para realizar un seguimiento de las modificaciones en los discos en el grupo de seguimiento de regiones. Cuando el sistema se reanuda tras apagarse inesperadamente y los espacios vuelven a conectarse, DRT hace que los discos del grupo sean coherentes entre sí.|Necesita por lo menos dos discos físicos para proteger contra errores en uno de los discos.<br><br>Necesita por lo menos cinco discos físicos para proteger contra errores simultáneos en dos de los discos.|Se usa para la mayoría de las implementaciones. Por ejemplo, los espacios reflejados son adecuados para un recurso compartido de uso general o para una biblioteca de disco duro virtual (VHD).|
|**Paridad**<br><br>-Secciona datos e información de paridad en discos físicos<br>-Aumenta la fiabilidad cuando se compara con un espacio simple, pero reduce en cierta medida capacidad<br>-Aumenta la resistencia por medio de registro en diario. Esto ayuda a impedir la corrupción de los datos en caso de que se produzca un apagado de manera inesperada.|Necesita por lo menos tres discos físicos para proteger contra errores en uno de los discos.|Se usa para cargas de trabajo altamente secuenciales, como archivo o copia de seguridad.|

## <a name="step-1-create-a-storage-pool"></a>Paso 1: Crear un grupo de almacenamiento

Primero debes agrupar los discos físicos disponibles en uno o varios grupos de almacenamiento.

1. En el panel de navegación del administrador del servidor, seleccione **File and Storage Services**.

2. En el panel de navegación, seleccione el **grupos de almacenamiento** página.
    
    De manera predeterminada, los discos disponibles se incluyen en un grupo llamado *primordial*. Si no se muestra ningún grupo primordial en **GRUPOS DE ALMACENAMIENTO**, el almacenamiento no cumple los requisitos de los Espacios de almacenamiento. Asegúrate de que los discos cumplen los requisitos indicados en la sección Requisitos previos.
    
    >[!TIP]
    >Si seleccionas el grupo de almacenamiento **Primordial**, los discos físicos disponibles se muestran en **DISCOS FÍSICOS**.

3. En **grupos de almacenamiento**, seleccione el **tareas** lista y, a continuación, seleccione **nuevo grupo de almacenamiento**. Se abrirá el Asistente para nuevo grupo de almacenamiento.

4. En el **antes de comenzar** página, seleccione **siguiente**.

5. En el **especifique un nombre de grupo de almacenamiento y subsistema** página, escriba un nombre y una descripción opcional para el grupo de almacenamiento, seleccione el grupo de discos físicos disponibles que desea usar y, a continuación, seleccione **siguiente**.

6. En el **seleccionar discos físicos para el grupo de almacenamiento** página, haga lo siguiente y, a continuación, seleccione **siguiente**:
    
    1. Selecciona la casilla situada junto a cada disco físico que desees incluir en el grupo de almacenamiento.
    
    2. Si desea designar uno o más discos como reservas activas, en **asignación**, seleccione la flecha desplegable y luego seleccione **reserva activa**.

7. En el **Confirmar selecciones de** , comprueba que la configuración es correcta y, a continuación, seleccione **crear**.

8. En el **ver resultados** , comprueba que todas las tareas completado y, a continuación, seleccione **cerrar**.
    
    >[!NOTE]
    >Si deseas ir directamente al siguiente paso, selecciona la casilla **Crear un disco virtual cuando este asistente se cierre**.

9. En **GRUPOS DE ALMACENAMIENTO**, comprueba que aparece el nuevo grupo de almacenamiento.

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Comandos equivalentes de Windows PowerShell para crear grupos de almacenamiento

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

El siguiente ejemplo muestra qué discos físicos están disponibles en el grupo primordial.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

En el ejemplo siguiente se crea un nuevo grupo de almacenamiento denominado *StoragePool1* que usa todos los discos disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

En el ejemplo siguiente se crea un nuevo grupo de almacenamiento, *StoragePool1*, que usa cuatro de los discos disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

La siguiente secuencia de cmdlets de ejemplo muestra cómo agregar un disco físico disponible llamado *PhysicalDisk5* como reserva activa al grupo de almacenamiento *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>Paso 2: Crear un disco virtual

A continuación, debes crear uno o más discos virtuales desde el grupo de almacenamiento. Al crear un disco virtual, puedes seleccionar cómo se distribuyen los datos en los discos físicos. Esto afecta a la fiabilidad y al rendimiento. También puedes elegir entre crear discos con aprovisionamiento fino o fijo.

1. Si el Asistente para nuevo disco virtual todavía no está abierto, en la página **Grupos de almacenamiento** en el Administrador del servidor, en **GRUPOS DE ALMACENAMIENTO**, asegúrate de que esté seleccionado el grupo de almacenamiento deseado.

2. En **discos virtuales**, seleccione el **tareas** lista y, a continuación, seleccione **nuevo disco Virtual**. Se abrirá el Asistente para nuevo disco Virtual.

3. En el **antes de comenzar** página, seleccione **siguiente**.

4. En el **seleccione el grupo de almacenamiento** , seleccione el grupo de almacenamiento deseado y, a continuación, seleccione **siguiente**.

5. En el **especificar el nombre del disco virtual** página, escriba un nombre y una descripción opcional y luego seleccione **siguiente**.

6. En el **seleccionar el diseño de almacenamiento** página, seleccione el diseño deseado y luego seleccione **siguiente**.
    
    >[!NOTE]
    >Si selecciona un diseño donde no tiene suficientes discos físicos, recibirá un mensaje de error cuando se selecciona **siguiente**. Para obtener información sobre qué distribución utilizar y los requisitos de disco, consulte [requisitos previos](#prerequisites)).

7. Si seleccionó **reflejado** como el diseño de almacenamiento y tiene cinco o más discos en el grupo, el **configurar las opciones de resistencia** aparecerá la página. Seleccione una de las siguientes opciones:
    
      - **Reflejo doble**
      - **Reflejo triple**

8. En el **especificar el tipo de aprovisionamiento** página, seleccione una de las opciones siguientes y luego seleccione **siguiente**.
    
      - **Fino**
        
        Con el aprovisionamiento fino, el espacio se asigna según haga falta. De este modo, se optimiza el uso del almacenamiento disponible. Sin embargo, como esto te permite sobreasignar almacenamiento, debes supervisar cuidadosamente de cuánto espacio en disco dispones.
    
      - **Fixed**
        
        Con el aprovisionamiento fijo, la capacidad de almacenamiento se asigna inmediatamente, cuando se crea un disco virtual. Por lo tanto, el aprovisionamiento fijo utiliza un espacio del grupo de almacenamiento que es igual al tamaño del disco virtual.
    
    >[!TIP]
    >Con los Espacios de almacenamiento, puedes crear discos virtuales con aprovisionamiento fino y fijo en el mismo grupo de almacenamiento. Por ejemplo, puedes usar un disco virtual con aprovisionamiento fino para hospedar una base de datos y un disco virtual con aprovisionamiento fijo para hospedar los archivos de registro asociados.

9. En la página **Especificar el tamaño del disco virtual**, haz lo siguiente:
    
    Si seleccionaste el aprovisionamiento fino en el paso anterior, en el **tamaño de disco Virtual** cuadro, escriba un tamaño de disco virtual, seleccione las unidades (**MB**, **GB**, o **TB** ), a continuación, seleccione **siguiente**.
    
    Si seleccionaste el aprovisionamiento fijo en el paso anterior, seleccione una de las siguientes acciones:
    
      - **Especificar tamaño**
        
        Para especificar un tamaño, escriba un valor en el **tamaño de disco Virtual** cuadro y, después, seleccione las unidades (**MB**, **GB**, o **TB**).
        
        Si usas una distribución de almacenamiento que no sea la simple, el disco virtual usará más espacio libre que el especificado. Para evitar un posible error si el tamaño del volumen supera el espacio libre del grupo de almacenamiento, puedes seleccionar la casilla **Crear el disco virtual más grande posible, hasta el tamaño especificado**.
    
      - **Tamaño máximo**
        
        Selecciona esta opción para crear un disco virtual que use la capacidad máxima del grupo de almacenamiento.

10. En el **Confirmar selecciones de** , comprueba que la configuración es correcta y, a continuación, seleccione **crear**.

11. En el **ver resultados** , comprueba que todas las tareas completado y, a continuación, seleccione **cerrar**.
    
    >[!TIP]
    >De manera predeterminada, está seleccionada la casilla **Crear un volumen cuando este asistente se cierre**. Esto te llevará directamente al paso siguiente.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Comandos equivalentes de Windows PowerShell para crear discos virtuales

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

En el ejemplo siguiente se crea un disco virtual de 50 GB llamado *VirtualDisk1* en un bloque de almacenamiento denominada *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

En el ejemplo siguiente se crea un disco virtual reflejado llamado *VirtualDisk1* en un bloque de almacenamiento denominada *StoragePool1*. El disco usa la capacidad de almacenamiento máximo del grupo de almacenamiento.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

En el ejemplo siguiente se crea un disco virtual de 50 GB llamado *VirtualDisk1* en un grupo de almacenamiento que se denomina *StoragePool1*. El disco usa el tipo de aprovisionamiento fino.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

En el ejemplo siguiente se crea un disco virtual llamado *VirtualDisk1* en un bloque de almacenamiento denominada *StoragePool1*. El disco virtual usa el reflejo triple y tiene un tamaño fijo de 20 GB.

>[!NOTE]
>Debes tener por lo menos cinco discos físicos en el grupo de almacenamiento para que este cmdlet funcione. (No se incluyen los discos asignados como reservas activas).

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>Paso 3: Crear un volumen

A continuación, debes crear un volumen desde el disco virtual. Puede asignar una letra de unidad opcional o una carpeta y después formatear el volumen con un sistema de archivos.

1. Si el Asistente para nuevo volumen ya no está abierto, en el **grupos de almacenamiento** página en el administrador del servidor, en **discos virtuales**, haga clic en el disco virtual deseado y, a continuación, seleccione **nuevo volumen**.
    
    Se abre el Asistente para nuevo volumen.

2. En el **antes de comenzar** página, seleccione **siguiente**.

3. En el **seleccionar el servidor y disco** página, haga lo siguiente y, a continuación, seleccione **siguiente**.
    
    1. En el **Server** área, seleccione el servidor donde va a aprovisionar el volumen.
    
    2. En el **disco** área, seleccione el disco virtual en el que desea crear el volumen.

4. En el **especificar el tamaño del volumen** , escriba un tamaño de volumen, especifique las unidades (**MB**, **GB**, o **TB**) y, a continuación, seleccione **Siguiente**.

5. En el **asignar a una letra de unidad o carpeta** página, configurar la opción deseada y, a continuación, seleccione **siguiente**.

6. En el **seleccione Configuración del sistema de archivos** página, haga lo siguiente y, a continuación, seleccione **siguiente**.
    
    1. En el **sistema de archivos** lista, seleccione **NTFS** o **ReFS**.
    
    2. En la lista **Tamaño de unidad de asignación**, deja el valor en **Predeterminado** o establece el tamaño de la unidad de asignación.
        
        >[!NOTE]
        >Para obtener más información sobre el tamaño de unidad de asignación, consulte [tamaño de clúster predeterminado para NTFS, FAT y exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Opcionalmente, en el cuadro **Etiqueta de volumen**, escribe un nombre para la etiqueta de volumen, por ejemplo **Datos de RR. HH.**

7. En el **Confirmar selecciones de** , comprueba que la configuración es correcta y, a continuación, seleccione **crear**.

8. En el **ver resultados** , comprueba que todas las tareas completado y, a continuación, seleccione **cerrar**.

9. Para comprobar que el volumen se creó, en el administrador del servidor, seleccione el **volúmenes** página. El volumen aparece bajo el servidor donde se ha creado. También puedes comprobar que el volumen se encuentra en el Explorador de Windows.

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Comandos equivalentes de Windows PowerShell para crear volúmenes

El siguiente cmdlet de Windows PowerShell realiza la misma función que el procedimiento anterior. Escribe el comando en una sola línea.

El siguiente ejemplo inicializa los discos para el disco virtual *VirtualDisk1*, crea una partición con una letra de unidad asignada y después formatea el volumen con el sistema de archivos NTFS predeterminado.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>Información adicional

- [Espacios de almacenamiento](overview.md)
- [Cmdlets de almacenamiento en Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Implementar espacios de almacenamiento en clúster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Preguntas más frecuentes (P+F) de espacios de almacenamiento](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
