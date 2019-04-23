---
title: Configuración del servicio de mantenimiento
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 569cf7ba30fd3f993394efd3735a56d116c067e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858336"
---
# <a name="health-service-settings"></a>Configuración del servicio de mantenimiento
> Se aplica a Windows Server 2016

El servicio de mantenimiento es una característica nueva en Windows Server 2016 que mejora la supervisión diaria y la experiencia operativa para los clústeres que ejecutan espacios de almacenamiento directo.

Muchos de los parámetros que controlan el comportamiento del servicio de mantenimiento se exponen como configuración. Puede modificar estos para ajustar la intensidad de los errores o acciones, activar o desactivar ciertos comportamientos de y mucho más.

Use el siguiente cmdlet de PowerShell para establecer o modificar la configuración.

### <a name="usage"></a>Uso

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name <SettingName> -Value <Value>  
```

#### <a name="example"></a>Ejemplo

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.Volume.CapacityThreshold.Warning" -Value 70
```

### <a name="common-settings"></a>Configuración común

Algunas configuraciones que suelen modificarse aparecen a continuación, junto con sus valores predeterminados.

#### <a name="volume-capacity-threshold"></a>Umbral del volumen de capacidad

```
"System.Storage.Volume.CapacityThreshold.Enabled"  = True
"System.Storage.Volume.CapacityThreshold.Warning"  = 80
"System.Storage.Volume.CapacityThreshold.Critical" = 90
```

#### <a name="pool-reserve-capacity-threshold"></a>Umbral de capacidad de reserva de grupo

```
"System.Storage.StoragePool.CheckPoolReserveCapacity.Enabled" = True
```

#### <a name="physical-disk-lifecycle"></a>Ciclo de vida del disco físico

```
"System.Storage.PhysicalDisk.AutoPool.Enabled"                             = True
"System.Storage.PhysicalDisk.AutoRetire.OnLostCommunication.Enabled"       = True
"System.Storage.PhysicalDisk.AutoRetire.OnUnresponsive.Enabled"            = True
"System.Storage.PhysicalDisk.AutoRetire.DelayMs"                           = 900000 (i.e. 15 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountResetIntervalSeconds" = 360 (i.e. 60 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountAllowed"              = 3
```

#### <a name="supported-components-document"></a>Documento de componentes compatibles

Consulte la sección anterior.

#### <a name="firmware-rollout"></a>Implementación de firmware

```
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.SingleDrive.Enabled"       = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled"           = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelaySeconds"  = 604800 (i.e. 7 days)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.ShortDelaySeconds" = 86400 (i.e. 1 day)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelayCount"    = 1
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.FailureTolerance"  = 3
```

#### <a name="platform--quiescence"></a>Plataforma / inactividad

```
"Platform.Quiescence.MinDelaySeconds" = 120 (i.e. 2 minutes)
"Platform.Quiescence.MaxDelaySeconds" = 420 (i.e. 7 minutes)
```

#### <a name="metrics"></a>Métricas

```
"System.Reports.ReportingPeriodSeconds" = 1
```

#### <a name="debugging"></a>Depuración

```
"System.LogLevel" = 4
```

## <a name="see-also"></a>Vea también

- [Servicio de mantenimiento de Windows Server 2016](health-service-overview.md)
- [Espacios de almacenamiento directo en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
