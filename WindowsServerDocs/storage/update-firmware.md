---
ms.assetid: e5945bae-4a33-487c-a019-92a69db8cf6c
title: Actualización del firmware de la unidad
ms.author: toklima
manager: dmoss
ms.topic: article
author: toklima
ms.date: 10/04/2016
ms.openlocfilehash: 15e0d6dedc6bb81c0b511479ee342dbd463654e2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946218"
---
# <a name="updating-drive-firmware"></a>Actualización del firmware de la unidad
>Se aplica a: Windows Server 2019, Windows Server 2016 y Windows 10

La actualización del firmware de las unidades ha sido históricamente una tarea complicada con un potencial de tiempo de inactividad, por lo que estamos realizando mejoras en los espacios de almacenamiento, Windows Server y Windows 10, versión 1703 y versiones más recientes. Si tiene unidades que admiten el nuevo mecanismo de actualización de firmware incluido en Windows, puede actualizar el firmware de la unidad de las unidades en producción sin tiempo de inactividad. Sin embargo, si va a actualizar el firmware de una unidad de producción, asegúrese de leer nuestras sugerencias sobre cómo minimizar el riesgo al usar esta eficaz funcionalidad nueva.

  > [!Warning]
  > Las actualizaciones de firmware son una operación de mantenimiento potencialmente peligrosa y solo se deben aplicar después de realizar pruebas minuciosas de la nueva imagen de firmware. Es posible que el nuevo firmware en hardware no compatible afecte negativamente a la estabilidad y la confiabilidad o que incluso provoque la pérdida de datos. Los administradores deben leer las notas de la versión que incluye una actualización determinada para determinar su impacto y aplicabilidad.

## <a name="drive-compatibility"></a>Compatibilidad con las unidades

Para usar Windows Server para actualizar el firmware de la unidad, debe tener unidades compatibles. Para garantizar un comportamiento de dispositivo común, comenzamos definiendo nuevos y-para Windows 10 y Windows Server 2016: requisitos del kit de laboratorio de hardware (HLK) opcionales para dispositivos SAS, SATA y NVMe. Estos requisitos resumen los comandos que un dispositivo SATA, SAS o NVMe debe admitir para que se pueda actualizar su firmware mediante estos nuevos cmdlets de PowerShell nativos de Windows. Para admitir estos requisitos, hay una nueva prueba de HLK para comprobar si los productos del proveedor admiten los comandos adecuados y si estos se implementan en revisiones futuras.

Para obtener información sobre si su hardware admite que Windows actualice el firmware de la unidad, póngase en contacto con su proveedor de soluciones.
A continuación se enumeran vínculos a los distintos requisitos:

-   SATA: [Device.Storage.Hd.Sata](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)#devicestoragehdsata) (en la sección **Descarga y activación del firmware \][si está implementado]**).

-   SAS: [Device.Storage.Hd.Sas](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)#devicestoragehdsas) (en la sección **Descarga y activación del firmware \][si está implementado]**).

-   NVMe: [Device.Storage.ControllerDrive.NVMe](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)#devicestoragecontrollerdrivenvme) (en las secciones **5.7** y **5.8)**.

## <a name="powershell-cmdlets"></a>Cmdlets de PowerShell

Los dos cmdlets que se agregan a Windows son:

-   Get-StorageFirmwareInformation
-   Update-StorageFirmware

El primer cmdlet proporciona información detallada sobre las capacidades del dispositivo, las imágenes de firmware y las revisiones. En este caso, la máquina solo contiene una sola SATA SSD con una ranura de firmware. Este es un ejemplo:

   ```powershell
   Get-PhysicalDisk | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E16101}
   ```

Tenga en cuenta que los dispositivos SAS siempre notifican "SupportsUpdate" como "true", ya que no hay ninguna manera de consultar explícitamente el dispositivo para la compatibilidad con estos comandos.

El segundo cmdlet, Update-StorageFirmware, permite a los administradores actualizar el firmware de la unidad con un archivo de imagen si la unidad es compatible con el nuevo mecanismo de actualización de firmware. Debe obtener este archivo de imagen directamente del OEM o del proveedor de la unidad.

  > [!Note]
  > Antes de actualizar cualquier hardware de producción, pruebe la imagen de firmware concreta en un hardware idéntico en un entorno de laboratorio.

La unidad cargará en primer lugar la nueva imagen de firmware en un área de ensayo interna. Mientras esto sucede, E/S continúa normalmente. La imagen se activa después de la descarga. Durante este tiempo, la unidad no podrá responder a comandos de E/S, ya que, de lo contrario, se producirá una reinicialización interna. Esto significa que esta unidad no proporciona datos durante la activación. Una aplicación que tuviera acceso a los datos de esta unidad tendría que esperar una respuesta hasta que la activación de firmware se completase. Este es un ejemplo del cmdlet en acción:

   ```powershell
   $pd | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
   $pd | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E160@3}
   ```

Normalmente, las unidades no completan las solicitudes de E/S cuando activan una nueva imagen de firmware. El tiempo que una unidad tarda en realizar la activación depende de su diseño y del tipo de firmware que se actualice. Hemos observado que los tiempos de actualización varían entre menos de 5 segundos y más de 30 segundos.

Esta unidad realizó la actualización de firmware en aproximadamente 5,8 segundos, tal como se muestra a continuación:

```powershell
Measure-Command {$pd | Update-StorageFirmware -ImagePath C:\\Firmware\\J3E16101.enc -SlotNumber 0}

 Days : 0
 Hours : 0
 Minutes : 0
 Seconds : 5
 Milliseconds : 791
 Ticks : 57913910
 TotalDays : 6.70299884259259E-05
 TotalHours : 0.00160871972222222
 TotalMinutes : 0.0965231833333333
 TotalSeconds : 5.791391
 TotalMilliseconds : 5791.391
 ```

## <a name="updating-drives-in-production"></a>Actualización de unidades en producción

Antes de colocar un servidor en producción, se recomienda encarecidamente actualizar el firmware de las unidades de disco al firmware recomendado por el OEM o proveedor de hardware que le vendió y que le proporciona asistencia para su solución (contenedores de almacenamiento, unidades y servidores).

Una vez que un servidor se encuentra en producción, es una buena idea realizar tan solo algunos cambios en el servidor, como es práctico. Sin embargo, en ocasiones su proveedor de soluciones le informará de que hay una actualización de firmware crítica para sus unidades. En ese caso, a continuación se describen algunos procedimientos recomendados que debe seguir antes de aplicar cualquier actualización de firmware de la unidad:

1. Revise las notas de la versión del firmware y confirme que la actualización soluciona los problemas que pueden afectar a su entorno y que el firmware no contiene ningún problema conocido que pueda afectar negativamente.

2. Instale el firmware en un servidor de su laboratorio que tenga unidades idénticas (incluida la revisión de la unidad si hay varias revisiones de la misma unidad) y pruebe la unidad bajo carga con el nuevo firmware. Para obtener información sobre cómo realizar pruebas de carga sintética, consulte [Rendimiento de Espacios de almacenamiento de prueba con cargas de trabajo sintéticas en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn894707(v=ws.11)).

## <a name="automated-firmware-updates-with-storage-spaces-direct"></a>Actualizaciones de firmware automatizadas con Espacios de almacenamiento directo

Windows Server 2016 incluye un Servicio de mantenimiento para las implementaciones de Espacios de almacenamiento directo (incluidas las soluciones de Microsoft Azure Stack). El propósito principal del Servicio de mantenimiento es facilitar la supervisión y administración de la implementación del hardware. Como parte de sus funciones de administración, tiene la capacidad de implementar el firmware de la unidad en todo un clúster sin desconectar las cargas de trabajo ni incurrir en tiempo de inactividad. Esta funcionalidad está controlada por directivas, con el control en las manos del administrador.

El uso del Servicio de mantenimiento para implementar el firmware en un clúster es muy sencillo e implica los pasos siguientes:

-   Identificar qué unidades HDD y SSD espera que formen parte del clúster de Espacios de almacenamiento directo y si estas unidades admiten que Windows realice actualizaciones de firmware.
-   Enumerar estas unidades en el archivo XML Componentes compatibles.
-   Identificar las versiones de firmware que se espera que las unidades tengan el archivo XML Componentes compatibles (incluidas las rutas de acceso de ubicación de las imágenes de firmware).
-   Cargar el archivo XML en la base de datos del clúster.

En este punto, el Servicio de mantenimiento inspeccionará y analizará el archivo XML e identificará las unidades que no tienen implementada la versión de firmware deseada. A continuación, pasará a redirigir E/S fuera de las unidades afectadas, yendo nodo por nodo y actualizando el firmware que contienen. Un clúster de Espacios de almacenamiento directo consigue resistencia al propagar datos en varios nodos de servidor; el Servicio de mantenimiento puede aislar un nodo completo de las unidades para actualizaciones. Una vez actualizo un nodo, este iniciará una reparación en Espacios de almacenamiento que hará que todas las copias de datos en el clúster vuelvan a sincronizarse entre sí, antes de continuar con el nodo siguiente. Se espera y es normal que Espacios de almacenamiento pase un modo de operación "degradado" mientras se implementa el firmware.

Para garantizar una implementación estable y suficiente tiempo de validación de una nueva imagen de firmware, se produce un retraso significativo entre las actualizaciones de varios servidores. De forma predeterminada, el Servicio de mantenimiento esperará 7 días antes de actualizar el 2<sup></sup> servidor. Los servidores siguientes (3<sup></sup>, 4<sup></sup>, etc.) se actualizan con un retraso de un día. Si un administrador detecta que el firmware es inestable o no deseado, puede detener la implementación que realiza el Servicio de mantenimiento en cualquier momento. Si el firmware se ha validado previamente y se desea una implementación más rápida, se pueden modificar estos valores predeterminados de días a horas o minutos.

A continuación se muestra un ejemplo del archivo XML Componentes compatibles para un clúster genérico de Espacios de almacenamiento directo:

```xml
 <Components>
     <Disks>
        <Disk>
            <Manufacturer>Contoso</Manufacturer>
            <Model>XYZ9000</Model>
            <AllowedFirmware>
              <Version>2.0</Version>
              <Version>2.1>/Version>
              <Version>2.2</Version>
            </AllowedFirmware>
            <TargetFirmware>
              <Version>2.2</Version>
              <BinaryPath>\\path\to\image.bin</BinaryPath>
            </TargetFirmware>
        </Disk>
        ...
        ...
    </Disks>
 </Components>
```

Para obtener la implementación del nuevo firmware iniciado en este clúster de Espacios de almacenamiento directo, simplemente cargue el archivo .xml en la base de datos del clúster:

```powershell
$SpacesDirect = Get-StorageSubSystem Clus*

$CurrentDoc = $SpacesDirect | Get-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document"

$CurrentDoc.Value | Out-File <Path>
```

Edite el archivo en su editor favorito, como código de Visual Studio o el Bloc de notas, y guárdelo.

```powershell
$NewDoc = Get-Content <Path> | Out-String

$SpacesDirect | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $NewDoc
```

Si desea ver el Servicio de mantenimiento en acción y obtener más información sobre su mecanismo de implementación, consulte este vídeo:https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Vea también [solución de problemas de actualizaciones de firmware de unidad](troubleshoot-firmware-update.md).

### <a name="will-this-work-on-any-storage-device"></a>¿Funcionará en cualquier dispositivo de almacenamiento?

Funcionará en dispositivos de almacenamiento que implementen los comandos correctos en su firmware. El cmdlet Get-StorageFirmwareInformation mostrará si el firmware de una unidad es compatible con los comandos correctos (para SATA/NVMe) y la prueba de HLK permite a los proveedores y OEM probar este comportamiento.

### <a name="after-i-update-a-sata-drive-it-reports-to-no-longer-support-the-update-mechanism-is-something-wrong-with-the-drive"></a>Después de actualizar una unidad SATA, informa de que el mecanismo de actualización ya no es compatible. ¿Hay algún problema con la unidad?

No, la unidad está bien, a menos que el firmware nuevo ya no permita actualizaciones. Está llegando a un problema conocido por el que una versión en caché de las capacidades de la unidad es incorrecta. Al ejecutar "Update-StorageProviderCache-DiscoveryLevel Full", se volverán a enumerar las capacidades de la unidad y se actualizará la copia en caché. Como solución alternativa, se recomienda ejecutar el comando anterior una vez antes de iniciar una actualización de firmware o completar la implementación en un clúster de Espacios de almacenamiento directo.

### <a name="can-i-update-firmware-on-my-san-through-this-mechanism"></a>¿Puedo actualizar firmware en mi dispositivo SAN a mediante este mecanismo?
No. Normalmente, los dispositivos SAN tienen sus propias utilidades e interfaces para esas operaciones de mantenimiento. Este nuevo mecanismo es para el almacenamiento conectado directamente, como los dispositivos SATA, SAS o NVMe.

### <a name="from-where-do-i-get-the-firmware-image"></a>¿De dónde obtengo la imagen de firmware?

Siempre debe obtener el firmware directamente de su OEM, de su proveedor de soluciones o del proveedor de su unidad y no descargarlo de terceros. Windows proporciona el mecanismo para obtener la imagen en la unidad, pero no puede comprobar su integridad.

### <a name="will-this-work-on-clustered-drives"></a>¿Funcionará en las unidades agrupadas?

Los cmdlets también pueden realizar su función en unidades agrupadas, pero tenga en cuenta que la orquestación del Servicio de mantenimiento reduce el impacto de E/S en las cargas de trabajo en ejecución. Si los cmdlets se usan directamente en unidades agrupadas, es probable que E/S se detenga. En general, es recomendable realizar las actualizaciones de firmware de la unidad cuando no haya ninguna carga de trabajo en las unidades subyacentes o esta sea mínima.

### <a name="what-happens-when-i-update-firmware-on-storage-spaces"></a>¿Qué ocurre al actualizar el firmware en Espacios de almacenamiento?

En Windows Server 2016 con el Servicio de mantenimiento implementado en Espacios de almacenamiento directo, puede realizar esta operación sin desconectar las cargas de trabajo, siempre y cuando las unidades permitan que Windows Server actualice el firmware.

### <a name="what-happens-if-the-update-fails"></a>¿Qué ocurre si se produce un error en la actualización?

Se puede producir un error en la actualización por varias razones, algunas de ellas son: 1) la unidad no es compatible con los comandos correctos para que Windows actualice su firmware. En este caso, la nueva imagen de firmware nunca se activa y la unidad sigue funcionando con la imagen anterior. (2) La imagen no se puede descargar en esta unidad ni aplicar a ella (error de coincidencia de versiones, imagen dañada, etc.). En este caso, la unidad provoca un error en el comando Activate. De nuevo, la imagen de firmware anterior seguirá funcionando.

Si la unidad no responde después de una actualización de firmware, probablemente está experimentando un error en el propio firmware de la unidad. Pruebe todas las actualizaciones de firmware en un entorno de laboratorio antes de ponerlas en producción. Puede que la única corrección sea reemplazar la unidad.

Para obtener más información, consulte [solución de problemas de actualizaciones de firmware de unidad](troubleshoot-firmware-update.md).

### <a name="how-do-i-stop-an-in-progress-firmware-roll-out"></a>¿Cómo detengo la implementación de un firmware en progreso?

Deshabilite la implementación en PowerShell mediante:
```powershell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled" -Value false
```

### <a name="i-am-seeing-an-access-denied-or-path-not-found-error-during-roll-out-how-do-i-fix-this"></a>Veo un error de acceso denegado o de ruta de acceso no encontrada durante la implementación. Cómo corregir este

Asegúrese de que todos los nodos del clúster tienen acceso a la imagen de firmware que desea usar para la actualización. La forma más sencilla de asegurarse de esto es colocar la imagen en un volumen compartido de clúster.
