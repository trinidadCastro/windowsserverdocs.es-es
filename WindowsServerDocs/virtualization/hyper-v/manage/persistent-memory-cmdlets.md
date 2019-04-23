---
title: Cmdlets para configurar dispositivos de memoria persistente para máquinas virtuales de Hyper-V
description: Cómo configurar los dispositivos de memoria persistente para máquinas virtuales de Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: fd1b04ce74f0b8d490529d2a7f65091f5847d0f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878186"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Cmdlets para configurar dispositivos de memoria persistente para máquinas virtuales de Hyper-V

>Se aplica a: Windows Server 2019

Este artículo proporciona los administradores de sistemas y profesionales de TI con información sobre cómo configurar las máquinas virtuales de Hyper-V con memoria persistente (también conocido como memoria de la clase de almacenamiento o NVDIMM). Dispositivos de memoria persistente de NVDIMM-N JDEC compatibles son compatibles con Windows Server 2016 y Windows 10 y proporcionan acceso de nivel de byte a dispositivos no volátil de latencia muy baja. Los dispositivos de memoria persistente de máquina virtual se admiten en Windows Server 2019. 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Crear un dispositivo de memoria persistente para una máquina virtual

Use la **[nuevo VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** para crear un dispositivo de memoria persistente para una máquina virtual. El dispositivo debe crearse en un volumen NTFS DAX existente.  La nueva extensión de nombre de archivo (.vhdpmem) se utiliza para especificar que el dispositivo es un dispositivo de memoria persistente. Se admite solo el formato de archivo del disco duro virtual fijo.

**Ejemplo:** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Crear una máquina virtual con un controlador de memoria persistente



Use la **cmdlet New-VM** para crear una VM de generación 2 con el tamaño de memoria especificado y la ruta de acceso a una imagen de VHDX. A continuación, utilice **agregar VMPmemController** para agregar un controlador de memoria persistente a una máquina virtual.

**Ejemplo:** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Asocie un dispositivo de memoria persistente a una máquina virtual

Use **[Add-VMHardDiskDrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** para conectar un dispositivo de memoria persistente a una máquina virtual

**Ejemplo:** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Los dispositivos de memoria persistente en una máquina virtual de Hyper-V aparecen como un dispositivo de memoria persistente que consumirá y administrados por el sistema operativo invitado. Sistemas operativos invitados puede usar el dispositivo como un bloque o un volumen DAX. Cuando se utilizan dispositivos de memoria persistente dentro de una máquina virtual como un volumen DAX, se benefician de la dirección de capacidad de baja latencia a nivel de byte del dispositivo de host (sin virtualización de E/S en la ruta de acceso del código). 

>[!NOTE] 
>Memoria persistente solo se admite para las máquinas virtuales de Hyper-V Gen2. Migración en vivo y la migración de almacenamiento no se admiten para las máquinas virtuales con memoria persistente. Los puntos de control de producción de las máquinas virtuales no incluyen el estado de memoria persistente. 