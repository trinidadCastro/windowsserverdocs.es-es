---
title: Cmdlets para configurar dispositivos de memoria persistentes para máquinas virtuales de Hyper-V
description: Configuración de dispositivos de memoria persistentes para máquinas virtuales de Hyper-V
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.openlocfilehash: 68f4d4121513973e97a28ad26cea7856a842b302
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640200"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Cmdlets para configurar dispositivos de memoria persistentes para máquinas virtuales de Hyper-V

>Se aplica a: Windows Server 2019

En este artículo se proporcionan a los administradores de sistemas y profesionales de ti información sobre la configuración de máquinas virtuales de Hyper-V con memoria persistente (también conocido como memoria de clase de almacenamiento o NVDIMM). Los dispositivos de memoria persistentes basados en JDEC se admiten en Windows Server 2016 y Windows 10, y proporcionan acceso de nivel de bytes a los dispositivos no volátiles de latencia muy baja. Los dispositivos de memoria persistente de máquina virtual se admiten en Windows Server 2019.

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Creación de un dispositivo de memoria persistente para una máquina virtual

Use el cmdlet **[New-VHD](/powershell/module/hyper-v/new-vhd?view=win10-ps)** para crear un dispositivo de memoria persistente para una máquina virtual. El dispositivo debe crearse en un volumen NTFS DAX existente.  La nueva extensión de nombre de archivo (. vhdpmem) se usa para especificar que el dispositivo es un dispositivo de memoria persistente. Solo se admite el formato de archivo VHD fijo.

**Ejemplo:** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Creación de una máquina virtual con un controlador de memoria persistente

Use el **cmdlet New-VM** para crear una máquina virtual de generación 2 con el tamaño de memoria especificado y la ruta de acceso a una imagen VHDX. A continuación, use **Add-VMPmemController** para agregar un controlador de memoria persistente a una máquina virtual.

**Ejemplo**:

```powershell
New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

Add-VMPmemController ProductionVM1x
```

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Conexión de un dispositivo de memoria persistente a una máquina virtual

Uso de **[Add-VMHardDiskDrive](/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** para adjuntar un dispositivo de memoria persistente a una máquina virtual

**Ejemplo:** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Los dispositivos de memoria persistentes dentro de una máquina virtual de Hyper-V aparecen como un dispositivo de memoria persistente que el sistema operativo invitado consumirá y administrará. Los sistemas operativos invitados pueden usar el dispositivo como un volumen de bloque o DAX. Cuando se usan dispositivos de memoria persistentes dentro de una máquina virtual como volumen DAX, se benefician de la capacidad de dirección de nivel de bytes de baja latencia del dispositivo host (sin virtualización de e/s en la ruta de acceso del código).

>[!NOTE]
>La memoria persistente solo se admite para las máquinas virtuales de Hyper-V. La migración de Migración en vivo y almacenamiento no se admite para las máquinas virtuales con memoria persistente. Los puntos de control de producción de las máquinas virtuales no incluyen el estado de la memoria persistente.