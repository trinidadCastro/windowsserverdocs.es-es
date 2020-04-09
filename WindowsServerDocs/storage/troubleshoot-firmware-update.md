---
ms.assetid: 13210461-1e92-48a1-91a2-c251957ba256
title: Solución de problemas de las actualizaciones de firmware de unidad
ms.prod: windows-server
ms.author: toklima
manager: masriniv
ms.technology: storage
ms.topic: article
author: toklima
ms.date: 04/18/2017
ms.openlocfilehash: b62fdfe64ea579f61239dc582c639fb10ec1371c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820888"
---
# <a name="troubleshooting-drive-firmware-updates"></a>Solución de problemas de las actualizaciones de firmware de unidad

>Se aplica a: Windows 10, Windows Server (canal semianual),

Windows 10, versión 1703 y versiones más recientes, y Windows Server (canal semianual) incluyen la capacidad de actualizar el firmware de unidades de disco duro y SSD que haya sido certificado con AQ (Calificador adicional) actualizable de firmware mediante PowerShell.

Puedes encontrar más información acerca de esta característica aquí:

- [Actualización del firmware de la unidad en Windows Server 2016](update-firmware.md)
- [Actualice el firmware de la unidad sin tiempo de inactividad en Espacios de almacenamiento directo](https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct)

Las actualizaciones de firmware pueden sufrir errores por varias razones. La finalidad de este artículo es ayudar en la solución avanzada de problemas.

  > [!Note]
  > La información de este artículo, en función del problema, no puede ser suficiente para depurar completamente todos los casos de error posibles.

## <a name="common-issues"></a>Problemas comunes
Desde el punto de vista de la arquitectura, esta nueva funcionalidad depende de las API implementadas en la pila de almacenamiento de Windows, en las que PowerShell realiza sus llamadas. La pila de almacenamiento depende de los controladores y el hardware para implementar correctamente los comandos definidos del sector. Esto proporciona varios puntos en los que pueden producirse errores. Los problemas más observados más habitualmente son:

1.  Una unidad determinada no implementa correctamente los comandos estándar del sector (no tiene el AQ).
2.  Las API necesarias para realizar la actualización no se han implementado o presentan problemas (si se usan controladores de terceros).
3.  Las API funcionan pero hay un problema en el propio firmware (no es válido, la imagen está dañada…).

Las siguientes secciones proporcionan información sobre solución de problemas, dependiendo de si se usan controladores de Microsoft o de terceros.

## <a name="identifying-inappropriate-hardware"></a>Identificación del hardware inapropiado
La forma más rápida de identificar si un dispositivo admite el conjunto de comandos correcto es simplemente iniciar PowerShell y pasar un objeto PhysicalDisk que represente al disco en el cmdlet Get-StorageFirmwareInfo. He aquí un ejemplo:

```powershell
Get-PhysicalDisk -SerialNumber 15140F55976D | Get-StorageFirmwareInformation
```
Y este es un ejemplo de los resultados:

```
PhysicalDisk          : MSFT_PhysicalDisk (ObjectId = "{1}\\TOKLIMA-DL380\root/Microsoft/Windo...)
SupportsUpdate        : True
NumberOfSlots         : 1
ActiveSlotNumber      : 0
SlotNumber            : {0}
IsSlotWritable        : {True}
FirmwareVersionInSlot : {0013}
```

El campo SupportsUpdate, al menos para dispositivos SATA y NVMe, indicará si la funcionalidad integrada de PowerShell puede usarse para actualizar el firmware.

El campo SupportsUpdate siempre notificará "True" para los dispositivos conectados por SAS, ya que no es posible consultar la compatibilidad del comando correspondiente con los comandos estándar del sector.

Para validar si un dispositivo SAS admite el conjunto de comandos necesarios, existen dos opciones:
1.  Probarlo mediante el cmdlet Update-StorageFirmware con una imagen del firmware adecuada.
2.  Consulte el catálogo de Windows Server para identificar qué dispositivos SAS han conseguido correctamente la actualización de FW AQ (https://www.windowsservercatalog.com/)

### <a name="remediation-options"></a>Opciones de corrección
Si un dispositivo determinado que se está probando no admite el conjunto de comandos adecuado, consulta con el proveedor para ver si hay disponible un firmware actualizado que proporcione el conjunto de comandos necesarios, o consulte el catálogo de Windows Server para identificar los dispositivos que implementan dicho conjunto de comando adecuado.

## <a name="troubleshooting-with-3rd-party-drivers-sas"></a>Solución de problemas de controladores de terceros (SAS)
Los componentes de software que interactúan más estrechamente con el hardware son los controladores de minipuerto de la pila de almacenamiento de Windows. En el caso de algunos protocolos de almacenamiento, como SATA y NVMe, Microsoft proporciona controladores nativos de Windows. Estos controladores permiten obtener información adicional para la depuración de errores. Sin embargo, los proveedores de hardware y software de terceros son libres de escribir sus propios controladores de minipuerto para sus dispositivos, y su nivel de compatibilidad para obtener información para la depuración de errores puede variar.

Para identificar qué ha ocurrido con la descarga de firmware y activar las API enviadas a la pila de almacenamiento, independientemente del controlador de minipuerto, consulta el siguiente canal de registro de eventos:

Visor de eventos - Registros de aplicaciones y servicios - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-ClassPnP/Operational**

Este canal registra información sobre las API de Windows enviadas a los controladores de minipuerto y cuáles son sus respuestas. Por ejemplo, la condición de error que se muestra directamente a continuación se presenta al intentar descargar una imagen de firmware en un dispositivo SATA, que se conecta mediante un SAS HBA que no implementa correctamente la conversión necesaria de SAS a SATA:

```powershell
Get-PhysicalDisk -SerialNumber 44GS103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
```
A continuación presentamos un ejemplo de la salida:

```
Update-StorageFirmware : Failed

Extended information:
A warning or error has been encountered during storage firmware update.
Incorrect function.

Activity ID: {1224482b-2315-4a38-81eb-27bb7de19c00}
At line:1 char:47
+ ... S103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.en ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Update-StorageFirmware], CimException
+ FullyQualifiedErrorId : StorageWMI 4,Microsoft.Management.Infrastructure.CimCmdlets.InvokeCimMethodCommand,Update-StorageFirmware
```
    
PowerShell generará un error y ha recibido la información de que la función llamada (es decir, la API de Kernel) es incorrecta. Esto podría significar que el controlador de minipuerto SAS de terceros no ha implementado la API ("true" en este caso) o que se ha producido un error en la API por algún otro motivo, como un error de alineación de los segmentos de descarga.

```
EventData
DeviceGUID  {132EDB55-6BAC-A3A0-C2D5-203C7551D700}
DeviceNumber    1
Vendor  ATA 
Model   TOSHIBA THNSNJ12
FirmwareVersion 6101
SerialNumber    44GS103UT5EW
DownLevelIrpStatus  0xc0000185
SrbStatus   132
ScsiStatus  2
SenseKey    5
AdditionalSenseCode 36
AdditionalSenseCodeQualifier    0
CdbByteCount    10
CdbBytes    3B0E0000000001000000
NumberOfRetriesDone 0
```

El evento de ETW 507 del canal muestra que se ha producido un error en una solicitud SRB de SCSI y proporciona la información adicional que SenseKey era ' 5 ' (solicitud no válida) y que la información de AdditionalSense era ' 36 ' (campo no válido en CDB).

   > [!Note]
   > Esta información la proporciona directamente el minipuerto en cuestión, y su precisión dependerá de la implementación y de la sofisticación del controlador del minipuerto.

Es posible que distintas condiciones de error presenten los mismos códigos de error, si el controlador del minipuerto no elimina la ambigüedad entre ellos. Por ejemplo, intentar descargar una imagen del firmware válida a través de un HBA SAS a un dispositivo SATA (que se espera que el dispositivo no pueda realizar) puede traducirse en los mismos códigos de error.

En aquellos casos en los que se mezclan protocolos y se producen conversiones, es decir, SATA detrás de SAS, se recomienda probar el dispositivo SATA conectado directamente a un controlador SATA para descartarlo como un posible problema.

### <a name="remediation-options"></a>Opciones de corrección
Si se identifica que el controlador de terceros no implementa las API o las conversiones necesarias, es posible cambiar a las alternativas a SATA (StorAHCI.sys) y NVMe (StorNVMe.sys) proporcionadas por Microsoft o contactar con el proveedor de OEM o HBA que proporcionara el controlador SAS y consultar si existe una versión más reciente con la compatibilidad adecuada.

## <a name="additional-troubleshooting-with-microsoft-drivers-satanvme"></a>Solución de problemas adicional para los controladores de Microsoft (SATA/NVMe)
Si se usan los controladores nativos de Windows, como StorAHCI.sys o StorNVMe.sys, para dispositivos de almacenamiento, es posible obtener información adicional acerca de los posibles casos de error durante las operaciones de actualización del firmware.

Más allá del canal operativo ClassPnP, StorAHCI y StorNVMe registrarán los códigos de retorno específicos del protocolo del dispositivo en el siguiente canal ETW:

Visor de eventos - Registros de aplicaciones y servicios - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-StorPort/Diagnose**

Los registros de diagnóstico no se muestran de forma predeterminada; si quieres activarlos o mostrarlos, haz en "Vista" en EventViewer y selecciona "Mostrar registros de análisis y depuración" en el menú desplegable.

Para recopilar estas entradas del registro avanzadas, habilita el registro, reproduce el error de actualización del firmware y guarda el registro de diagnóstico.

Este es un ejemplo de una actualización del firmware en un dispositivo SATA que sufre errores porque la imagen que se ha de descargar no era válida (identificador de evento: 258):

``` 
EventData
MiniportName    storahci
MiniportEventId 19
MiniportEventDescription    Firmware Activate Completion
PortNumber  0
Bus 2
Target  0
LUN 0
Irp 0xffff8c84cd45aca0
Srb 0xffffab0024030bc0
Parameter1Name  SrbStatus
Parameter1Value 130
Parameter2Name  ReturnCode
Parameter2Value 0
Parameter3Name  FeaturesReg
Parameter3Value 15
Parameter4Name  SectorCountReg
Parameter4Value 0
Parameter5Name  DriveHeadReg
Parameter5Value 160
Parameter6Name  CommandReg
Parameter6Value 146
Parameter7Name  NULL
Parameter7Value 0
Parameter8Name  NULL
Parameter8Value 0
```

El evento anterior contiene información detallada del dispositivo en los valores de parámetro del 2 al 6. Aquí podemos observar distintos valores de registro de ATA. La especificación ATA ACS puede usarse para descodificar los siguientes de valores correspondientes a errores de un comando Download Microcode:
- Código de retorno: 0 (0000 0000) (N/A: no significa nada, ya que no se ha transferido ninguna carga)
- Características: 15 (0000 1111) (bit 1 se establece en ' 1 ' e indica "anular")
- SectorCount: 0 (0000 0000) (N/A)
- DriveHead: 160 (1010 0000) (N/A: solo se establecen bits obsoletos)
- Comando: 146 (1001 0010) (bit 1 está establecido en "1" que indica la disponibilidad de los datos de detección)

Esto nos indica que el dispositivo ha cancelado la operación de actualización del firmware.

Un nivel similar de información para depuración de errores está disponible en este canal si se usan dispositivos NVMe con el controlador NVMe nativo de Windows (StorNVMe.sys).
