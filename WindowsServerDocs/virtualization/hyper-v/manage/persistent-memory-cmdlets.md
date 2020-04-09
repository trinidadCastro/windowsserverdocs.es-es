---
title: Cmdlets para configurar dispositivos de memoria persistentes para máquinas virtuales de Hyper-V
description: Configuración de dispositivos de memoria persistentes para máquinas virtuales de Hyper-V
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: b58e2a4e2f31c5bf3e49b89da912b77060e334ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860428"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Cmdlets para configurar dispositivos de memoria persistentes para máquinas virtuales de Hyper-V

>Se aplica a: Windows Server 2019

En este artículo se proporcionan a los administradores de sistemas y profesionales de ti información sobre la configuración de máquinas virtuales de Hyper-V con memoria persistente (también conocido como memoria de clase de almacenamiento o NVDIMM). Los dispositivos de memoria persistentes basados en JDEC se admiten en Windows Server 2016 y Windows 10, y proporcionan acceso de nivel de bytes a los dispositivos no volátiles de latencia muy baja. Los dispositivos de memoria persistente de máquina virtual se admiten en Windows Server 2019. 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Creación de un dispositivo de memoria persistente para una máquina virtual

Use el cmdlet **[New-VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** para crear un dispositivo de memoria persistente para una máquina virtual. El dispositivo debe crearse en un volumen NTFS DAX existente.  La nueva extensión de nombre de archivo (. vhdpmem) se usa para especificar que el dispositivo es un dispositivo de memoria persistente. Solo se admite el formato de archivo VHD fijo.

**Ejemplo:** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Creación de una máquina virtual con un controlador de memoria persistente



Use el **cmdlet New-VM** para crear una máquina virtual de generación 2 con el tamaño de memoria especificado y la ruta de acceso a una imagen VHDX. A continuación, use **Add-VMPmemController** para agregar un controlador de memoria persistente a una máquina virtual.

**Ejemplo:** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Conexión de un dispositivo de memoria persistente a una máquina virtual

Uso de **[Add-VMHardDiskDrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** para adjuntar un dispositivo de memoria persistente a una máquina virtual

**Ejemplo:** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Los dispositivos de memoria persistentes dentro de una máquina virtual de Hyper-V aparecen como un dispositivo de memoria persistente que el sistema operativo invitado consumirá y administrará. Los sistemas operativos invitados pueden usar el dispositivo como un volumen de bloque o DAX. Cuando se usan dispositivos de memoria persistentes dentro de una máquina virtual como volumen DAX, se benefician de la capacidad de dirección de nivel de bytes de baja latencia del dispositivo host (sin virtualización de e/s en la ruta de acceso del código). 

>[!NOTE] 
>La memoria persistente solo se admite para las máquinas virtuales de Hyper-V. La migración de Migración en vivo y almacenamiento no se admite para las máquinas virtuales con memoria persistente. Los puntos de control de producción de las máquinas virtuales no incluyen el estado de la memoria persistente. 