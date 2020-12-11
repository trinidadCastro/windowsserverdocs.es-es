---
description: Más información acerca de cómo solucionar problemas de actualizaciones de firmware de unidad
ms.assetid: 13210461-1e92-48a1-91a2-c251957ba256
title: Solución de problemas de actualizaciones de firmware de unidad
ms.author: toklima
manager: masriniv
ms.topic: article
author: toklima
ms.date: 04/18/2017
ms.openlocfilehash: f1fed8ce79fc9918b4b11bb002bd7ba352e337fc
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039193"
---
# <a name="troubleshooting-drive-firmware-updates"></a>Solución de problemas de actualizaciones de firmware de unidad

>Se aplica a: Windows 10, Windows Server (canal semianual),

Windows 10, versión 1703 y versiones más recientes, y Windows Server (canal semianual) incluyen la capacidad de actualizar el firmware de las unidades de disco duro y las SSD que han sido certificadas con el firmware actualizable (Calificador adicional) a través de PowerShell.

Puede encontrar más información sobre esta característica aquí:

- [Actualización del firmware de la unidad en Windows Server 2016](update-firmware.md)
- [Actualice el firmware de la unidad sin tiempo de inactividad en Espacios de almacenamiento directo](https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct)

Es posible que se produzcan errores en las actualizaciones de firmware por varias razones. El propósito de este artículo es ayudarle con la solución avanzada de problemas.

  > [!Note]
  > La información de este artículo, dependiendo del problema, puede no ser suficiente para depurar completamente todos los casos de error posibles.

## <a name="common-issues"></a>Problemas comunes
En la arquitectura, esta nueva funcionalidad se basa en las API implementadas en la pila de almacenamiento de Windows, que PowerShell llama a. La pila de almacenamiento se basa en los controladores y el hardware para implementar correctamente los comandos definidos por el sector. Esto produce varios puntos en los que se pueden producir errores. Los problemas más conocidos son:

1.  Una unidad determinada no implementa correctamente los comandos estándar del sector (no tiene el comando AQ)
2.  Las API necesarias para realizar la actualización no están implementadas o son defectuosas (si se usan controladores de terceros)
3.  Las API funcionan, pero hay un problema con el propio firmware (imagen no válida o dañada,...)

En las secciones siguientes se describe la información de solución de problemas, en función de si se usan controladores de Microsoft o de terceros.

## <a name="identifying-inappropriate-hardware"></a>Identificación de hardware inadecuado
La manera más rápida de identificar si un dispositivo es compatible con el conjunto de comandos correcto es simplemente iniciar PowerShell y pasar el objeto DiscoFísico que representa un disco al cmdlet Get-StorageFirmwareInfo. Este es un ejemplo:

```powershell
Get-PhysicalDisk -SerialNumber 15140F55976D | Get-StorageFirmwareInformation
```
Y este es el resultado del ejemplo:

```
PhysicalDisk          : MSFT_PhysicalDisk (ObjectId = "{1}\\TOKLIMA-DL380\root/Microsoft/Windo...)
SupportsUpdate        : True
NumberOfSlots         : 1
ActiveSlotNumber      : 0
SlotNumber            : {0}
IsSlotWritable        : {True}
FirmwareVersionInSlot : {0013}
```

El campo SupportsUpdate, al menos para dispositivos SATA y NVMe, indicará si la funcionalidad integrada de PowerShell se puede usar para actualizar el firmware.

El campo SupportsUpdate siempre notificará "true" para los dispositivos conectados a SAS, ya que la consulta de la compatibilidad de comando adecuada no es posible con los comandos estándar del sector.

Para validar si un dispositivo SAS admite el conjunto de comandos necesario, existen dos opciones:
1.  Pruébelo a través del cmdlet Update-StorageFirmware con una imagen de firmware adecuada, o bien
2.  Consulte el catálogo de Windows Server para identificar qué dispositivos SAS han conseguido correctamente la actualización de FW AQ (https://www.windowsservercatalog.com/)

### <a name="remediation-options"></a>Opciones de corrección
Si un dispositivo determinado que está probando no admite el conjunto de comandos adecuado, consulte al proveedor para ver si hay disponible un firmware actualizado que proporcione el conjunto de comandos necesario o consulte el catálogo de Windows Server para identificar los dispositivos de origen que implementan el conjunto de comandos adecuado.

## <a name="troubleshooting-with-3rd-party-drivers-sas"></a>Solución de problemas con controladores de terceros (SAS)
Los componentes de software que interactúan mejormente con el hardware son controladores de pocos puertos en la pila de almacenamiento de Windows. En algunos protocolos de almacenamiento, como SATA y NVMe, Microsoft proporciona controladores nativos de Windows. Estos controladores permiten obtener información de depuración adicional. sin embargo, los proveedores de hardware y software de terceros pueden escribir sus propios controladores de minipuerto para sus dispositivos y su nivel de soporte técnico para la información de depuración puede variar.

Para identificar lo que ha ocurrido con las API de descarga y activación de firmware enviadas a través de la pila de almacenamiento, independientemente del controlador de minipuerto, consulte el siguiente canal de registro de eventos:

Visor de eventos: registros de aplicaciones y servicios-Microsoft-Windows-StorDiag- **Microsoft-Windows-Storage-ClassPnP/Operational**

Este canal registra información acerca de las API de Windows que se envían a los controladores de minipuerto y cuáles son sus respuestas. Por ejemplo, la condición de error que se muestra directamente a continuación se muestra al intentar descargar una imagen de firmware en un dispositivo SATA, que se conecta a través de un HBA SAS que no implementa correctamente la traducción necesaria de SAS a SATA:

```powershell
Get-PhysicalDisk -SerialNumber 44GS103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
```
Este es un ejemplo de la salida:

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

PowerShell producirá un error y habrá recibido la información que la función llamó (es decir, API de kernel) era incorrecta. Esto podría significar que la API no se implementó mediante el controlador de minipuerto SAS de terceros (true en este caso), o que se produjo un error en la API por otro motivo, como una falta de alineación de los segmentos de descarga.

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
   > Esta información la proporciona directamente el minipuerto en cuestión y la exactitud de esta información dependerá de la implementación y la sofisticación del controlador del minipuerto.

Es posible que la condición de error diferente muestre los mismos códigos de error si el controlador del minipuerto no elimina la ambigüedad entre ellos. Por ejemplo, intentar descargar una imagen de firmware no válida a través de un HBA SAS en un dispositivo SATA (que se espera que el dispositivo produzca un error) se puede traducir a los mismos códigos de error.

En los casos en los que se mezclan los protocolos y se producen traducciones, es decir, SATA detrás de SAS, es mejor probar el dispositivo SATA directamente conectado a una controladora SATA para que quede como un posible problema.

### <a name="remediation-options"></a>Opciones de corrección
Si el controlador de terceros se identifica como no implementar las API o las traducciones necesarias, es posible intercambiar con las alternativas proporcionadas por Microsoft para SATA (StorAHCI.sys) y NVMe (StorNVMe.sys), o ponerse en contacto con el fabricante del OEM o del HBA que proporcionó el controlador SAS y consultar si existe una versión más reciente con la compatibilidad adecuada.

## <a name="additional-troubleshooting-with-microsoft-drivers-satanvme"></a>Solución de problemas adicionales con los controladores de Microsoft (SATA/NVMe)
Cuando se usan controladores nativos de Windows, como StorAHCI.sys o StorNVMe.sys para los dispositivos de almacenamiento, es posible obtener información adicional sobre los posibles casos de error durante las operaciones de actualización de firmware.

Más allá del canal operativo ClassPnP, StorAHCI y StorNVMe registrarán los códigos de retorno específicos del protocolo del dispositivo en el siguiente canal ETW:

Visor de eventos: registros de aplicaciones y servicios-Microsoft-Windows-StorDiag- **Microsoft-Windows-Storage-StorPort/diagnosticate**

Los registros de diagnóstico no se muestran de forma predeterminada y se pueden activar o mostrar haciendo clic en "ver" en EventViewer y seleccionando "Mostrar análisis y depurar registros" en el menú desplegable.

Para recopilar estas entradas de registro avanzadas, habilite el registro, reproduzca el error de actualización de firmware y guarde el registro de diagnóstico.

Este es un ejemplo de una actualización de firmware en un dispositivo SATA que no se puede realizar porque la imagen que se va a descargar no era válida (ID. de evento: 258):

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

El evento anterior contiene información detallada del dispositivo en los valores de parámetro del 2 al 6. Aquí veremos varios valores de registro de ATA. La especificación de ACS de ATA se puede usar para descodificar los valores siguientes en caso de error de un comando de microcódigo de descarga:
- Código de retorno: 0 (0000 0000) (N/A-no tiene sentido porque no se transfirió ninguna carga)
- Características: 15 (0000 1111) (bit 1 se establece en ' 1 ' e indica "anular")
- SectorCount: 0 (0000 0000) (N/A)
- DriveHead: 160 (1010 0000) (N/A: solo se establecen los bits obsoletos)
- Comando: 146 (1001 0010) (bit 1 está establecido en "1" que indica la disponibilidad de los datos de detección)

Esto nos indica que el dispositivo anuló la operación de actualización de firmware.

En este canal hay disponible un nivel similar de información de depuración cuando se usan dispositivos de NVMe con el controlador de Windows-Native NVMe (StorNVMe.sys).
