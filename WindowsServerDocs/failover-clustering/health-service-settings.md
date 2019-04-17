---
title: "Configuración del servicio de estado"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 569cf7ba30fd3f993394efd3735a56d116c067e0
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-settings"></a>Configuración del servicio de estado
> Se aplica a Windows Server 2016

El servicio de estado es una nueva característica de Windows Server 2016 que mejora la supervisión diarias y de la experiencia operativa clústeres ejecutan directa de los espacios de almacenamiento.

Muchos de los parámetros que controlan el comportamiento del servicio de estado se exponen como opciones de configuración. Puedes modificar estos para ajustar la agresividad de errores o acciones, activa ciertos comportamientos de encendido/apagado y mucho más.

Usa el siguiente cmdlet de PowerShell para establecer o modificar la configuración.

### <a name="usage"></a>Uso de

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name <SettingName> -Value <Value>  
```

#### <a name="example"></a>Ejemplo

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.Volume.CapacityThreshold.Warning" -Value 70
```

### <a name="common-settings"></a>Opciones de configuración comunes

Algunas opciones de configuración modificadas normalmente se enumeran a continuación, junto con sus valores predeterminados.

#### <a name="volume-capacity-threshold"></a>Umbral de capacidad de volumen

```
"System.Storage.Volume.CapacityThreshold.Enabled"  = True
"System.Storage.Volume.CapacityThreshold.Warning"  = 80
"System.Storage.Volume.CapacityThreshold.Critical" = 90
```

#### <a name="pool-reserve-capacity-threshold"></a>Umbral de capacidad de reserva de grupo

```
"System.Storage.StoragePool.CheckPoolReserveCapacity.Enabled" = True
```

#### <a name="physical-disk-lifecycle"></a>Ciclo de vida de disco físico

```
"System.Storage.PhysicalDisk.AutoPool.Enabled"                             = True
"System.Storage.PhysicalDisk.AutoRetire.OnLostCommunication.Enabled"       = True
"System.Storage.PhysicalDisk.AutoRetire.OnUnresponsive.Enabled"            = True
"System.Storage.PhysicalDisk.AutoRetire.DelayMs"                           = 900000 (i.e. 15 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountResetIntervalSeconds" = 360 (i.e. 60 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountAllowed"              = 3
```

#### <a name="supported-components-document"></a>Documento de componentes compatibles

Consulta la sección anterior.

#### <a name="firmware-rollout"></a>Implementación de firmware

```
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.SingleDrive.Enabled"       = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled"           = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelaySeconds"  = 604800 (i.e. 7 days)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.ShortDelaySeconds" = 86400 (i.e. 1 day)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelayCount"    = 1
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.FailureTolerance"  = 3
```

#### <a name="platform--quiescence"></a>Plataforma / o inactividad

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

## <a name="see-also"></a>Consulta también

- [Servicio de estado en Windows Server 2016](health-service-overview.md)
- [Espacios de almacenamiento dirigen en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
