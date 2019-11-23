---
title: Implementar espacios de almacenamiento en un servidor independiente
description: Describe cómo implementar espacios de almacenamiento en un servidor independiente basado en Windows Server 2012.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 52e6a4d53271a73bc0913e2ac500c4328f2e7009
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393729"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Implementar espacios de almacenamiento en un servidor independiente

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema se describe cómo implementar espacios de almacenamiento en un servidor independiente. Para obtener información sobre cómo crear un espacio de almacenamiento en clúster, vea [implementar un clúster de espacios de almacenamiento en Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Para crear un espacio de almacenamiento, debes crear primero uno o varios grupos de almacenamiento. Un grupo de almacenamiento es una colección de discos físicos. Un grupo de almacenamiento permite agregar almacenamiento, expandir la capacidad flexible y delegar la administración.

Desde un grupo de almacenamiento, puedes crear uno o varios discos virtuales. Los discos virtuales también se denominan *espacios de almacenamiento*. Un espacio de almacenamiento aparece en el sistema operativo Windows como un disco normal desde el que puedes crear volúmenes formateados. Cuando creas un disco virtual a través de la interfaz de usuario de Servicios de archivos y almacenamiento, puedes configurar el tipo de resistencia (simple, reflejo o paridad), el tipo de aprovisionamiento (fino o fijo) y el tamaño. Mediante Windows PowerShell, puedes configurar otros parámetros, como el número de columnas, el valor de intercalación y qué discos físicos del grupo deben usarse. Para obtener información sobre estos parámetros adicionales, consulte [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) y [¿Qué son las columnas y cómo deciden los espacios de almacenamiento cuántos usar? en las](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) preguntas más frecuentes (p + f) sobre los espacios de almacenamiento.

>[!NOTE]
>No se puede usar un espacio de almacenamiento para hospedar el sistema operativo Windows.

Desde un disco virtual, puedes crear uno o varios volúmenes. Al crear un volumen, puede configurar el tamaño, la letra de unidad o la carpeta, el sistema de archivos (sistema de archivos NTFS o sistema de archivos resistente (ReFS)), el tamaño de la unidad de asignación y una etiqueta de volumen opcional.

La siguiente figura ilustra el flujo de trabajo de los Espacios de almacenamiento.

![Flujo de trabajo de Espacios de almacenamiento](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: flujo de trabajo de espacios de almacenamiento**

>[!NOTE]
>Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, vea [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Requisitos previos

Para usar espacios de almacenamiento en un servidor independiente basado en Windows Server 2012 −, asegúrese de que los discos físicos que desea utilizar cumplan los siguientes requisitos previos.

> [!IMPORTANT]
> Si quiere obtener información sobre cómo implementar espacios de almacenamiento en un clúster de conmutación por error, consulte [implementar un clúster de espacios de almacenamiento en Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Una implementación de clústeres de conmutación por error tiene diferentes requisitos previos, como los tipos de bus de disco compatibles, los tipos de resistencia compatibles y el número mínimo de discos necesarios.

|Área|Requisitos|Notas|
|---|---|---|
|Tipos de bus de disco|-SCSI conectado en serie (SAS)<br>-Datos adjuntos de tecnología avanzada de serie (SATA)<br>-Controladores iSCSI y Canal de fibra. |También puedes usar unidades USB. Sin embargo, no es óptimo usar unidades USB en un entorno de servidor.<br>Los espacios de almacenamiento son compatibles con los controladores iSCSI y Canal de fibra (FC) siempre que los discos virtuales creados sobre ellos no sean resistentes (sencillos con cualquier número de columnas).<br>|
|Configuración del disco|-Los discos físicos deben ser de al menos 4 GB<br>-Los discos deben estar en blanco y no tener formato. No crees volúmenes.||
|Consideraciones sobre HBA|-Se recomienda usar adaptadores de bus de host (HBA) simples que no admitan la funcionalidad RAID<br>-Si es compatible con RAID, los HBA deben estar en modo no RAID con todas las funciones de RAID deshabilitadas.<br>-Los adaptadores no deben abstraer los discos físicos, los datos de la memoria caché ni ocultar los dispositivos conectados. Esto incluye los servicios de contenedor proporcionados por los dispositivos de un grupo de discos (JBOD) conectados. |Los Espacios de almacenamiento son compatibles con HBA solo si se pueden deshabilitar por completo todas las funciones RAID.|
|Contenedores JBOD|-Los contenedores JBOD son opcionales<br>-Se recomienda usar contenedores certificados de espacios de almacenamiento que se enumeran en el catálogo de Windows Server<br>-Si usa un contenedor JBOD, compruebe con el proveedor de almacenamiento que el gabinete admite espacios de almacenamiento para garantizar una funcionalidad completa.<br>-Para determinar si el contenedor JBOD es compatible con la identificación de alojamientos y ranuras, ejecute el siguiente cmdlet de Windows PowerShell:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Si los campos **campos EnclosureNumber** y **SlotNumber** contienen valores, el contenedor es compatible con estas características.||

Para planear el número de discos físicos y el tipo de resistencia deseado para una implementación de servidor independiente, usa las siguientes directrices.

|Tipo de resistencia|Requisitos de disco|Cuándo se debe utilizar|
|---|---|---|
|**Simple**<br><br>-Secciona datos entre discos físicos<br>-Maximiza la capacidad del disco y aumenta el rendimiento<br>-Sin resistencia (no protege contra errores de disco)<br><br><br><br><br><br><br>|Necesita por lo menos un disco físico.|No debe usarse para hospedar datos irreemplazables. Los espacios simples no protegen contra errores de disco.<br><br>Se usa para hospedar datos temporales o que puedan crearse de nuevo fácilmente a bajo costo.<br><br>Adecuado para cargas de trabajo de alto rendimiento en las que no se necesita resistencia o que la aplicación ya proporciona.|
|**Reflejo**<br><br>: Almacena dos o tres copias de los datos en el conjunto de discos físicos.<br>-Aumenta la confiabilidad, pero reduce la capacidad. Se produce duplicación con cada escritura. Un espacio de reflejo también secciona los datos en varias unidades físicas.<br>-Mayor rendimiento de datos y menor latencia de acceso que la paridad<br>-Usa el seguimiento de regiones desfasadas (DRT) para realizar el seguimiento de las modificaciones en los discos del grupo. Cuando el sistema se reanuda tras apagarse inesperadamente y los espacios vuelven a conectarse, DRT hace que los discos del grupo sean coherentes entre sí.|Necesita por lo menos dos discos físicos para proteger contra errores en uno de los discos.<br><br>Necesita por lo menos cinco discos físicos para proteger contra errores simultáneos en dos de los discos.|Se usa para la mayoría de las implementaciones. Por ejemplo, los espacios reflejados son adecuados para un recurso compartido de uso general o para una biblioteca de disco duro virtual (VHD).|
|**Parity**<br><br>-Secciona la información de datos y de paridad entre discos físicos<br>-Aumenta la fiabilidad cuando se compara con un espacio simple, pero reduce en cierta medida la capacidad.<br>-Aumenta la resistencia a través del registro en diario. Esto ayuda a impedir la corrupción de los datos en caso de que se produzca un apagado de manera inesperada.|Necesita por lo menos tres discos físicos para proteger contra errores en uno de los discos.|Se usa para cargas de trabajo altamente secuenciales, como archivo o copia de seguridad.|

## <a name="step-1-create-a-storage-pool"></a>Paso 1: Crear un grupo de almacenamiento

Primero debes agrupar los discos físicos disponibles en uno o varios grupos de almacenamiento.

1. En el panel de navegación Administrador del servidor, seleccione **servicios de archivos y almacenamiento**.

2. En el panel de navegación, seleccione la página **grupos de almacenamiento** .
    
    De manera predeterminada, los discos disponibles se incluyen en un grupo llamado *primordial*. Si no se muestra ningún grupo primordial en **GRUPOS DE ALMACENAMIENTO**, el almacenamiento no cumple los requisitos de los Espacios de almacenamiento. Asegúrate de que los discos cumplen los requisitos indicados en la sección Requisitos previos.
    
    >[!TIP]
    >Si seleccionas el grupo de almacenamiento **Primordial**, los discos físicos disponibles se muestran en **DISCOS FÍSICOS**.

3. En **grupos de almacenamiento**, seleccione la lista **tareas** y, después, seleccione **nuevo grupo de almacenamiento**. Se abrirá el Asistente para nuevo grupo de almacenamiento.

4. En la página **antes de comenzar** , seleccione **siguiente**.

5. En la página **Especifique un nombre y un subsistema** para el grupo de almacenamiento, escriba un nombre y una descripción opcional para el grupo de almacenamiento, seleccione el grupo de discos físicos disponibles que desea usar y, después, seleccione **siguiente**.

6. En la página **seleccionar discos físicos para el grupo de almacenamiento** , haga lo siguiente y, a continuación, seleccione **siguiente**:
    
    1. Selecciona la casilla situada junto a cada disco físico que desees incluir en el grupo de almacenamiento.
    
    2. Si desea designar uno o varios discos como reservas activas, en **asignación**, seleccione la flecha desplegable y, a continuación, seleccione **reserva activa**.

7. En la página **confirmar selecciones** , compruebe que la configuración sea correcta y, a continuación, seleccione **crear**.

8. En la página **ver resultados** , compruebe que todas las tareas se han completado y, a continuación, seleccione **cerrar**.
    
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

2. En **discos virtuales**, seleccione la lista **tareas** y, después, seleccione **nuevo disco virtual**. Se abrirá el Asistente para nuevo disco virtual.

3. En la página **antes de comenzar** , seleccione **siguiente**.

4. En la página **seleccionar el grupo de almacenamiento** , seleccione el grupo de almacenamiento que desee y, a continuación, seleccione **siguiente**.

5. En la página **especificar el nombre del disco virtual** , escriba un nombre y una descripción opcional y, a continuación, seleccione **siguiente**.

6. En la página **seleccionar la distribución de almacenamiento** , seleccione el diseño que desee y, a continuación, seleccione **siguiente**.
    
    >[!NOTE]
    >Si selecciona un diseño en el que no tiene suficientes discos físicos, recibirá un mensaje de error al seleccionar **siguiente**. Para obtener información acerca del diseño que se va a usar y los requisitos de disco, consulte [requisitos previos](#prerequisites).

7. Si seleccionó **reflejo** como distribución de almacenamiento y tiene cinco o más discos en el grupo, aparecerá la página **configurar las opciones de resistencia** . Seleccione una de las siguientes opciones:
    
      - **Reflejo doble**
      - **Reflejo triple**

8. En la página **especificar el tipo de aprovisionamiento** , seleccione una de las siguientes opciones y, a continuación, seleccione **siguiente**.
    
   - **Fino**
        
     Con el aprovisionamiento fino, el espacio se asigna según haga falta. De este modo, se optimiza el uso del almacenamiento disponible. Sin embargo, como esto te permite sobreasignar almacenamiento, debes supervisar cuidadosamente de cuánto espacio en disco dispones.
    
   - **Resuelto**
        
     Con el aprovisionamiento fijo, la capacidad de almacenamiento se asigna inmediatamente, cuando se crea un disco virtual. Por lo tanto, el aprovisionamiento fijo utiliza un espacio del grupo de almacenamiento que es igual al tamaño del disco virtual.
    
     >[!TIP]
     >Con los Espacios de almacenamiento, puedes crear discos virtuales con aprovisionamiento fino y fijo en el mismo grupo de almacenamiento. Por ejemplo, puedes usar un disco virtual con aprovisionamiento fino para hospedar una base de datos y un disco virtual con aprovisionamiento fijo para hospedar los archivos de registro asociados.

9. En la página **Especificar el tamaño del disco virtual**, haz lo siguiente:
    
    Si seleccionó el aprovisionamiento fino en el paso anterior, en el cuadro **tamaño de disco virtual** , escriba un tamaño de disco virtual, seleccione las unidades (**MB**, **GB**o **TB**) y, después, seleccione **siguiente**.
    
    Si seleccionó aprovisionamiento fijo en el paso anterior, seleccione una de las siguientes opciones:
    
      - **Especificar tamaño**
        
        Para especificar un tamaño, escriba un valor en el cuadro **tamaño de disco virtual** y, a continuación, seleccione las unidades (**MB**, **GB**o **TB**).
        
        Si usas una distribución de almacenamiento que no sea la simple, el disco virtual usará más espacio libre que el especificado. Para evitar un posible error si el tamaño del volumen supera el espacio libre del grupo de almacenamiento, puedes seleccionar la casilla **Crear el disco virtual más grande posible, hasta el tamaño especificado**.
    
      - **Tamaño máximo**
        
        Selecciona esta opción para crear un disco virtual que use la capacidad máxima del grupo de almacenamiento.

10. En la página **confirmar selecciones** , compruebe que la configuración sea correcta y, a continuación, seleccione **crear**.

11. En la página **ver resultados** , compruebe que todas las tareas se han completado y, a continuación, seleccione **cerrar**.
    
    >[!TIP]
    >De manera predeterminada, está seleccionada la casilla **Crear un volumen cuando este asistente se cierre**. Esto te llevará directamente al paso siguiente.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Comandos equivalentes de Windows PowerShell para crear discos virtuales

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

En el ejemplo siguiente se crea un disco virtual de 50 GB denominado *VirtualDisk1* en un grupo de almacenamiento denominado *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

En el ejemplo siguiente se crea un disco virtual reflejado denominado *VirtualDisk1* en un grupo de almacenamiento denominado *StoragePool1*. El disco usa la capacidad de almacenamiento máxima del grupo de almacenamiento.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

En el ejemplo siguiente se crea un disco virtual de 50 GB denominado *VirtualDisk1* en un grupo de almacenamiento denominado *StoragePool1*. El disco usa el tipo de aprovisionamiento fino.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

En el ejemplo siguiente se crea un disco virtual denominado *VirtualDisk1* en un grupo de almacenamiento denominado *StoragePool1*. El disco virtual usa el reflejo triple y tiene un tamaño fijo de 20 GB.

>[!NOTE]
>Debes tener por lo menos cinco discos físicos en el grupo de almacenamiento para que este cmdlet funcione. (No se incluyen los discos asignados como reservas activas).

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>Paso 3: Crear un volumen

A continuación, debes crear un volumen desde el disco virtual. Puede asignar una letra de unidad o carpeta opcional, y después formatear el volumen con un sistema de archivos.

1. Si el Asistente para nuevo volumen todavía no está abierto, en la página **grupos de almacenamiento** en Administrador del servidor, en **discos virtuales**, haga clic con el botón secundario en el disco virtual deseado y, a continuación, seleccione **nuevo volumen**.
    
    Se abre el Asistente para nuevo volumen.

2. En la página **antes de comenzar** , seleccione **siguiente**.

3. En la página **seleccionar el servidor y el disco** , realice lo siguiente y, a continuación, seleccione **siguiente**.
    
    1. En el área **servidor** , seleccione el servidor en el que desea aprovisionar el volumen.
    
    2. En el área **disco** , seleccione el disco virtual en el que desea crear el volumen.

4. En la página **especificar el tamaño del volumen** , escriba un tamaño de volumen, especifique las unidades (**MB**, **GB**o **TB**) y, a continuación, seleccione **siguiente**.

5. En la página **asignar a una letra de unidad o carpeta** , configure la opción deseada y, a continuación, seleccione **siguiente**.

6. En la página **seleccionar configuración del sistema de archivos** , haga lo siguiente y, a continuación, seleccione **siguiente**.
    
    1. En la lista **sistema de archivos** , seleccione **NTFS** o **ReFS**.
    
    2. En la lista **Tamaño de unidad de asignación**, deja el valor en **Predeterminado** o establece el tamaño de la unidad de asignación.
        
        >[!NOTE]
        >Para obtener más información sobre el tamaño de la unidad de asignación, consulte [tamaño de clúster predeterminado para NTFS, FAT y exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Opcionalmente, en el cuadro **Etiqueta de volumen**, escribe un nombre para la etiqueta de volumen, por ejemplo **Datos de RR. HH.**

7. En la página **confirmar selecciones** , compruebe que la configuración sea correcta y, a continuación, seleccione **crear**.

8. En la página **ver resultados** , compruebe que todas las tareas se han completado y, a continuación, seleccione **cerrar**.

9. Para comprobar que se ha creado el volumen, en Administrador del servidor, seleccione la página **volúmenes** . El volumen aparece bajo el servidor donde se ha creado. También puedes comprobar que el volumen se encuentra en el Explorador de Windows.

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
- [Preguntas más frecuentes (p + f) sobre espacios de almacenamiento](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
