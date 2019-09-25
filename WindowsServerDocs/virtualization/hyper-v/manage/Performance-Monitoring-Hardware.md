---
title: Habilitación del hardware de supervisión del rendimiento de Intel en una máquina virtual de Hyper-V
description: Cómo habilitar el hardware de supervisión de rendimiento de Intel en una máquina de Hyper-V. También toca cómo habilitar la supervisión de rendimiento de los efectos de hardware en la migración en vivo.
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 67f32a6e2ceeaf07701d558f473e2f997fbd8219
ms.sourcegitcommit: d12d9e6afd71d23e8a24682ad80d2cf3bc486588
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71226026"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>Habilitación del hardware de supervisión del rendimiento de Intel en una máquina virtual de Hyper-V

Los procesadores Intel contienen características denominados colectivamente hardware de supervisión de rendimiento (por ejemplo, PMU, PEBS, LBR). Estas características las usa el software de ajuste del rendimiento como el amplificador Intel VTune para analizar el rendimiento del software.  Antes de Windows Server 2019 y Windows 10 versión 1809, ni el sistema operativo host ni las máquinas virtuales invitadas de Hyper-V podían usar hardware de supervisión de rendimiento cuando se habilitaba Hyper-V.  A partir de Windows Server 2019 y Windows 10 versión 1809, el sistema operativo host tiene acceso de forma predeterminada al hardware de supervisión de rendimiento.  Las máquinas virtuales invitadas de Hyper-V no tienen acceso de forma predeterminada, pero los administradores de Hyper-V pueden optar por conceder acceso a una o varias máquinas virtuales invitadas.  En este documento se describen los pasos necesarios para exponer el hardware de supervisión de rendimiento a las máquinas virtuales invitadas.

## <a name="requirements"></a>Requisitos

Para habilitar el hardware de supervisión de rendimiento en una máquina virtual, necesitará lo siguiente:

- Un procesador Intel con hardware de supervisión de rendimiento (es decir, PMU, PEBS, IPT)
- Windows Server 2019 o Windows 10 versión 1809 (actualización de octubre de 2018) o posterior
- Una máquina virtual de Hyper-V _sin_ [Virtualización anidada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) que también esté en estado detenido
 
## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>Habilitación de los componentes de supervisión de rendimiento en una máquina virtual

Para habilitar distintos componentes de supervisión de rendimiento para una máquina virtual invitada específica `Set-VMProcessor` , use el cmdlet de PowerShell:
 
``` Powershell
# Enable all components
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```
 
``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```
 
``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
> [!NOTE]
> Cuando se habilitan los componentes de supervisión `"pebs"` de rendimiento, se `"pmu"` debe especificar si se especifica.  Además, la habilitación de un componente que no sea compatible con los procesadores físicos del host producirá un error de inicio de la máquina virtual.
 
## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>Efectos de habilitar el hardware de supervisión de rendimiento en guardar/restaurar, exportar y migración en vivo
 
Microsoft no recomienda la migración en vivo o el almacenamiento o la restauración de máquinas virtuales con hardware de supervisión de rendimiento entre sistemas con hardware de Intel diferente. El comportamiento específico del hardware de supervisión de rendimiento suele ser no arquitectónico y cambios entre los sistemas de hardware de Intel.  Mover una máquina virtual en ejecución entre diferentes sistemas puede producir un comportamiento impredecible de los contadores no arquitectónicos.