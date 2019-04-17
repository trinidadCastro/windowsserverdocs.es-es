---
title: Implementar espacios de almacenamiento en un servidor independiente
description: Se describe cómo implementar espacios de almacenamiento en un servidor independiente basado en Windows Server 2012.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 40265e767ac9aca05386c0893def259aca3a5633
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992538"
---
# Implementar espacios de almacenamiento en un servidor independiente

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe cómo implementar espacios de almacenamiento en un servidor independiente. Para obtener información sobre cómo crear un espacio de almacenamiento en clúster, consulta [implementar un clúster de espacios de almacenamiento en Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Para crear un espacio de almacenamiento, primero debes crear uno o varios grupos de almacenamiento. Un grupo de almacenamiento es una colección de discos físicos. Un grupo de almacenamiento permite la agregación de almacenamiento, expansión de capacidad elásticos y administración delegada.

Desde un grupo de almacenamiento, puedes crear uno o varios discos virtuales. Estos discos virtuales también se conocen como *espacios de almacenamiento*. Da formato a un disco regular desde el que puedes crear volúmenes, aparece un espacio de almacenamiento para el sistema operativo Windows. Al crear un disco virtual a través de la interfaz de usuario de servicios de archivos y almacenamiento, puedes configurar el tipo de resistencia (simple, reflejo o paridad), el tipo de aprovisionamiento (ligero o fijo) y el tamaño. A través de Windows PowerShell, puedes establecer parámetros adicionales, como el número de columnas, el valor de intercalado y discos qué físicos en el grupo para usar. Para obtener información acerca de estos parámetros adicionales, vea [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) y [¿Cuáles son las columnas y ¿cómo decide espacios de almacenamiento cuántos usar?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) en almacenamiento espacios más preguntas frecuentes (P+F).

>[!NOTE]
>No puedes usar un espacio de almacenamiento para hospedar el sistema operativo Windows.

Desde un disco virtual, puedes crear uno o varios volúmenes. Al crear un volumen, puedes configurar el tamaño, letra de unidad o carpeta, el sistema de archivos (sistema de archivos NTFS o el sistema de archivos resistente (ReFS)), el tamaño de unidad de asignación y una etiqueta de volumen opcional.

La siguiente ilustración muestra el flujo de trabajo de espacios de almacenamiento.

![Flujo de trabajo de espacios de almacenamiento](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**La figura 1: el flujo de trabajo espacios de almacenamiento**

>[!NOTE]
>Este tema incluye cmdlets de Windows PowerShell de muestra que puedes usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulta [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## Requisitos previos

Para usar espacios de almacenamiento en un servidor independiente de 2012−based de Windows Server, asegúrate de que los discos físicos que quieres usar cumplan los siguientes requisitos previos.

> [!IMPORTANT]
> Si quieres obtener información sobre cómo implementar espacios de almacenamiento en un clúster de conmutación por error, consulta [implementar un clúster de espacios de almacenamiento en Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Una implementación de clúster de conmutación por error tiene distintos requisitos previos, como los tipos de bus de disco compatibles, tipos de resistencia compatibles y el número mínimo necesario de discos.

|Área|Requisito|Notas|
|---|---|---|
|Tipos de bus de disco|-Serial Attached SCSI (SAS)<br>-Serie Advanced Technology Attachment (SATA)<br>-iSCSI y los controladores de canal de fibra. |También puedes usar las unidades USB. Sin embargo, no es óptimo usar unidades USB en un entorno de servidor.<br>Espacios de almacenamiento se admite en iSCSI y los controladores de canal de fibra (FC), siempre que los discos virtuales creó encima de ellos son y no resistente (Simple con cualquier número de columnas).<br>|
|Configuración de disco|-Discos físicos deben ser al menos 4 GB<br>-Discos deben estar en blanco y no con formato. No se debe crear volúmenes.||
|Consideraciones de HBA|-Se recomiendan los adaptadores de bus host simple (HBA) que no admiten funcionalidad RAID<br>-Si son compatibles con RAID, HBA debe estar en modo de no RAID con todas las funcionalidades RAID deshabilitada<br>-Adaptadores no deben abstraen los discos físicos, datos de la memoria caché, u oculte todos los dispositivos conectados. Esto incluye los servicios de alojamiento que proporcionan los dispositivos conectados de just-a-montón-de discos (JBOD). |Espacios de almacenamiento es compatible solo con HBA donde puedes deshabilitar por completo todas las funcionalidades de RAID.|
|Contenedores JBOD|-Los contenedores JBOD son opcionales<br>-Recomienda usar espacios de almacenamiento certified contenedores que aparece en el catálogo de Windows Server<br>-Si estás usando un alojamiento JBOD, comprueba con el proveedor de almacenamiento que la carcasa es compatible con espacios de almacenamiento para garantizar una funcionalidad completa<br>-Para determinar si la carcasa JBOD admite la identificación de alojamiento y ranura, ejecuta el siguiente cmdlet de Windows PowerShell:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Si los campos **EnclosureNumber** y **SlotNumber** contienen valores, la carcasa admite estas características.||

Para planear el número de discos físicos y el tipo de resistencia deseado para una implementación de servidor independiente, usa las siguientes directrices.

|Tipo de resistencia|Requisitos de disco|Cuándo usarla|
|---|---|---|
|**Simple**<br><br>-Franjas datos a través de discos físicos<br>-Maximiza la capacidad del disco y aumenta el rendimiento<br>-Sin resistencia (no protege frente a errores de disco)<br><br><br><br><br><br><br>|Requiere al menos un disco físico.|No se usa para hospedar datos irreemplazable. Espacios simple protege contra los errores del disco.<br><br>Se usa para hospedar datos temporales o nueva fácilmente a un coste reducido.<br><br>Adecuado para las cargas de trabajo de alto rendimiento donde resistencia no es necesario o se haya proporcionado por la aplicación.|
|**Mirror**<br><br>-Almacena dos o tres copias de los datos en el conjunto de discos físicos<br>-Aumenta la confiabilidad, pero reduce la capacidad. Se produce una duplicación con cada escritura. Un espacio de reflejo franjas también los datos en varias unidades físicas.<br>-Un mayor rendimiento de los datos y una menor latencia de acceso que paridad<br>-Usa incorrecto región (DRT) para realizar un seguimiento de las modificaciones en los discos en el grupo de seguimiento. Cuando el sistema se reanuda desde un apagado inesperado y los espacios que estén nuevamente en línea, DRT hace que los discos en el grupo coherente entre sí.|Requiere al menos dos discos físicos para proteger contra solo error de disco.<br><br>Requiere al menos 5 discos físicos para proteger frente a dos errores de disco simultáneas.|Se usa para la mayoría de las implementaciones. Por ejemplo, los espacios de reflejo son adecuadas para un recurso compartido de archivos de uso general o una biblioteca de disco duro virtual (VHD).|
|**Paridad**<br><br>-Información de datos y la paridad bandas en discos físicos<br>-Aumenta la confiabilidad cuando se compara con un espacio simple, pero un poco reduce la capacidad<br>-Aumenta la resistencia a través del registro en diario. Esto ayuda a impedir que los datos dañados si se produce un apagado inesperado.|Requiere al menos tres discos físicos para proteger contra solo error de disco.|Se usa para cargas de trabajo que son muy secuenciales, como archivo o copia de seguridad.|

## Paso 1: Crear un grupo de almacenamiento

Debes discos físicos disponibles de primer grupo en uno o varios grupos de almacenamiento.

1. En el panel de navegación del Administrador de servidores, selecciona **File and Storage Services**.

2. En el panel de navegación, selecciona la página de **Grupos de almacenamiento** .
    
    De manera predeterminada, los discos disponibles se incluyen en un grupo que se denomina el grupo *primordial* . Si no se muestra ningún grupo primordial en **Grupos de almacenamiento**, esto indica que el almacenamiento no cumple los requisitos para espacios de almacenamiento. Asegúrate de que los discos cumplan los requisitos que se describen en la sección de requisitos previos.
    
    >[!TIP]
    >Si se selecciona el grupo de almacenamiento **Primordial** , se enumeran los discos físicos disponibles en **Discos físicos**.

3. En **Los grupos de almacenamiento**, selecciona la lista de **tareas** y, a continuación, selecciona el **Nuevo grupo de almacenamiento**. Se abrirá el Asistente para nuevo grupo de almacenamiento.

4. En la página **antes de comenzar** , seleccione **siguiente**.

5. En la página **Especifique un nombre de grupo de almacenamiento y el subsistema** , escribe un nombre y una descripción opcional para el grupo de almacenamiento, selecciona el grupo de discos físicos disponibles que quieres usar y, a continuación, seleccione **siguiente**.

6. En la página **Seleccionar discos físicos para el grupo de almacenamiento** , realiza los siguientes y, a continuación, selecciona **a continuación**:
    
    1. Selecciona la casilla de verificación junto a todos los discos físicos que quieres incluir en el grupo de almacenamiento.
    
    2. Si quieres designar uno o más discos como piezas "en caliente", en la **asignación**, selecciona la flecha desplegable, selecciona **"En caliente" reserva**.

7. En la página **Confirmar selecciones** , comprueba que la configuración es correcta y, a continuación, seleccione **crear**.

8. En la página de **resultados de la vista** , comprueba que todas las tareas completado y, a continuación, seleccione **Cerrar**.
    
    >[!NOTE]
    >Opcionalmente, para continuar directamente en el siguiente paso, puedes seleccionar la casilla de verificación de **crear un disco virtual cuando este asistente se cierre** .

9. En **Los grupos de almacenamiento**, compruebe que aparece el nuevo grupo de almacenamiento.

### Comandos equivalentes de Windows PowerShell para crear grupos de almacenamiento

El siguiente cmdlet de Windows PowerShell o los cmdlets de realizar la misma función que el procedimiento anterior. Escribe cada cmdlet en una sola línea, aunque pueden aparecer con ajuste a través de varias líneas aquí debido a restricciones de formato.

El siguiente ejemplo se muestra qué discos físicos están disponibles en el grupo primordial.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

El siguiente ejemplo crea un nuevo grupo de almacenamiento denominado *StoragePool1* que usa todos los discos disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

El siguiente ejemplo crea un nuevo grupo de almacenamiento, *StoragePool1*, que usa cuatro de los discos disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

La secuencia de los cmdlets de ejemplo siguiente muestra cómo agregar un disco físico disponible *PhysicalDisk5* como una reserva "en caliente" en el grupo de almacenamiento *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## Paso 2: Crear un disco virtual

A continuación, debes crear uno o varios discos virtuales desde el grupo de almacenamiento. Al crear un disco virtual, puedes seleccionar cómo se disponen los datos a través de los discos físicos. Esto afecta al rendimiento y confiabilidad. También puedes seleccionar si quieres crear discos o fijo-aprovisionamiento fino.

1. Si aún no está abierto, en la página de **Grupos de almacenamiento** en el Administrador de servidores, el Asistente para nuevo disco Virtual en **Grupos de almacenamiento**, asegúrate de que esté seleccionado el grupo de almacenamiento deseado.

2. En la lista de **Discos virtuales**, selecciona la lista de **tareas** y, a continuación, selecciona el **Nuevo disco Virtual**. Se abrirá el Asistente para nuevo disco Virtual.

3. En la página **antes de comenzar** , seleccione **siguiente**.

4. En la página **Seleccionar el grupo de almacenamiento** , selecciona el grupo de almacenamiento deseado y, a continuación, seleccione **siguiente**.

5. En la página de **especificar el nombre del disco virtual** , escribe un nombre y una descripción opcional y luego selecciona **siguiente**.

6. En la página **Seleccionar el diseño de almacenamiento** , selecciona el diseño deseado y luego selecciona **siguiente**.
    
    >[!NOTE]
    >Si seleccionas un diseño donde no tienes suficientes discos físicos, recibirás un mensaje de error cuando se selecciona **siguiente**. Para obtener información sobre qué diseño de uso y los requisitos de disco, consulta [los requisitos previos](#prerequisites)).

7. Si has seleccionado el **reflejo** como el diseño de almacenamiento y tienes cinco o más discos en el grupo, aparecerá la página **configurar las opciones de resistencia** . Selecciona una de las siguientes opciones:
    
      - **Reflejo doble**
      - **Reflejo triple**

8. En la página de **especificar el tipo de aprovisionamiento** , selecciona una de las siguientes opciones y luego selecciona **siguiente**.
    
      - **Fino**
        
        Con aprovisionamiento fino, se asigna espacio según sea necesario. Esto optimiza el uso de almacenamiento disponible. Sin embargo, dado que esto te permite asignar exceso de almacenamiento, debe supervisar cuidadosamente cuánto espacio en disco.
    
      - **Fijo**
        
        Con aprovisionamiento fijo, la capacidad de almacenamiento se asigna inmediatamente, en el momento en que se crea un disco virtual. Por lo tanto, fijos aprovisionamiento espacio usa desde el grupo de almacenamiento que sea igual que el tamaño del disco virtual.
    
    >[!TIP]
    >Con espacios de almacenamiento, puedes crear dos discos virtuales y fija-aprovisionamiento fino en el mismo grupo de almacenamiento. Por ejemplo, podrías usar un disco virtual aprovisionamiento fino para hospedar una base de datos y un disco virtual aprovisionado fija para hospedar los archivos de registro asociado.

9. En la página de **especificar el tamaño del disco virtual** , hacer lo siguiente:
    
    Si has seleccionado el aprovisionamiento fino en el paso anterior, en el cuadro de **tamaño del disco Virtual** , especifique un tamaño de disco virtual, seleccione las unidades (**MB**, **GB**o **TB**) y selecciona **a continuación**.
    
    Si has seleccionado el aprovisionamiento fijo en el paso anterior, selecciona una de las siguientes acciones:
    
      - **Especificar el tamaño**
        
        Para especificar un tamaño, escribe un valor en el cuadro de **tamaño del disco Virtual** y luego selecciona las unidades (**MB**, **GB**o **TB**).
        
        Si usas un diseño de almacenamiento que no sea sencilla, el disco virtual usa más espacio libre que el tamaño que especifiques. Para evitar un error posible donde el tamaño del volumen supera el espacio libre del grupo de almacenamiento, puedes seleccionar la casilla de verificación de **crear el disco virtual más amplio posible, hasta el tamaño especificado** .
    
      - **Tamaño máximo**
        
        Selecciona esta opción para crear un disco virtual que usa la capacidad máxima del grupo de almacenamiento.

10. En la página **Confirmar selecciones** , comprueba que la configuración es correcta y, a continuación, seleccione **crear**.

11. En la página de **resultados de la vista** , comprueba que todas las tareas completado y, a continuación, seleccione **Cerrar**.
    
    >[!TIP]
    >De manera predeterminada, se selecciona la casilla de verificación de **crear un volumen cuando este asistente se cierre** . Esto te llevará directamente en el siguiente paso.

### Comandos equivalentes de Windows PowerShell para crear discos virtuales

El siguiente cmdlet de Windows PowerShell o los cmdlets de realizar la misma función que el procedimiento anterior. Escribe cada cmdlet en una sola línea, aunque pueden aparecer con ajuste a través de varias líneas aquí debido a restricciones de formato.

El siguiente ejemplo crea un disco virtual de 50 GB denominado *VirtualDisk1* en un grupo de almacenamiento denominado *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

El siguiente ejemplo crea un disco virtual reflejado denominado *VirtualDisk1* en un grupo de almacenamiento denominado *StoragePool1*. El disco utiliza la capacidad de almacenamiento máxima del grupo de almacenamiento.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

El siguiente ejemplo crea un disco virtual de 50 GB denominado *VirtualDisk1* en un grupo de almacenamiento que se denomina *StoragePool1*. El disco utiliza el tipo de aprovisionamiento fino.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

El siguiente ejemplo crea un disco virtual denominado *VirtualDisk1* en un grupo de almacenamiento denominado *StoragePool1*. El disco virtual usa la creación de reflejos triple y tiene un tamaño fijo de 20 GB.

>[!NOTE]
>Debes tener al menos 5 discos físicos en el grupo de almacenamiento de este cmdlet para que funcione. (Esto no incluye todos los discos que se asignan como piezas "en caliente").

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## Paso 3: Crear un volumen

A continuación, debes crear un volumen desde el disco virtual. Puedes asignar una letra de unidad opcional o carpeta y luego formatear el volumen con un sistema de archivos.

1. Si el Asistente para nuevo volumen ya no está abierto, en la página de **Grupos de almacenamiento** en el administrador del servidor, en la lista de **Discos virtuales**, haz clic en el disco virtual deseado y, a continuación, selecciona el **Nuevo volumen**.
    
    Abre el Asistente para nuevo volumen.

2. En la página **antes de comenzar** , seleccione **siguiente**.

3. En la página **Seleccionar el servidor y el disco** , hacer los siguientes y, a continuación, selecciona **a continuación**.
    
    1. En el área de **servidor** , selecciona el servidor en el que quieres aprovisionar el volumen.
    
    2. En el área de **disco** , selecciona el disco virtual en el que quieres crear el volumen.

4. En la página de **especificar el tamaño del volumen** , especifique un tamaño de volumen, especificar las unidades (**MB**, **GB**o **TB**) y, a continuación, seleccione **siguiente**.

5. En la página **asignar a una letra de unidad o una carpeta** , configurar la opción que quieras y, a continuación, seleccione **siguiente**.

6. En la página **Seleccionar la configuración del sistema de archivos** , realice las siguientes y, a continuación, selecciona **a continuación**.
    
    1. En la lista de **sistema de archivos** , selecciona **NTFS** o **ReFS**.
    
    2. En la lista de **tamaño de unidad de asignación** , deje el valor **predeterminado** o establecer el tamaño de unidad de asignación.
        
        >[!NOTE]
        >Para obtener más información sobre el tamaño de la unidad de asignación, consulta el [tamaño de clúster para NTFS, FAT y exFAT predeterminado](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Opcionalmente, en el cuadro de la **etiqueta de volumen** , escribe un nombre de la etiqueta de volumen, por ejemplo, **Datos de recursos humanos**.

7. En la página **Confirmar selecciones** , comprueba que la configuración es correcta y, a continuación, seleccione **crear**.

8. En la página de **resultados de la vista** , comprueba que todas las tareas completado y, a continuación, seleccione **Cerrar**.

9. Para comprobar que se creó el volumen, en el Administrador de servidores, seleccione la página de **volúmenes** . El volumen se enumera en el servidor donde se creó. También puedes comprobar que el volumen está en el Explorador de Windows.

### Comandos equivalentes de Windows PowerShell para crear volúmenes

El siguiente cmdlet de Windows PowerShell realiza la misma función que el procedimiento anterior. Escribe el comando en una sola línea.

El siguiente ejemplo inicializa los discos de disco virtual *VirtualDisk1*, crea una partición con una letra de unidad asignada y, a continuación, formatea el volumen con el sistema de archivos NTFS de forma predeterminada.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## Información adicional

- [Espacios de almacenamiento](overview.md)
- [Cmdlets de almacenamiento en Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Implementar espacios de almacenamiento en clúster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Preguntas más frecuentes (P+F) de espacios de almacenamiento](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
