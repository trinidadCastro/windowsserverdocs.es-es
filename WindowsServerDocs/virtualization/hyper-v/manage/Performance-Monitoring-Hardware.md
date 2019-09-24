---
title: Exposición del hardware de supervisión del rendimiento de Intel a una máquina virtual de Hyper-V
description: Describe cómo exponer el hardware de Monitorning de rendimiento de Intel a un equipo de Hyper-V. También toca el modo en que los efectos de habilitación de la migración en vivo.
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 1bc821cc46a3a402778e1c76f4a2d8feb862fd75
ms.sourcegitcommit: 45415ba58907d650cfda45f4c57f6ddf1255dcbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213621"
---
# <a name="exposing-intel-performance-monitoring-hardware-to-a-hyper-v-virtual-machine"></a>Exposición del hardware de supervisión del rendimiento de Intel a una máquina virtual de Hyper-V
 
## <a name="background"></a>Background
Los procesadores Intel contienen características denominados colectivamente hardware de supervisión de rendimiento (por ejemplo, PMU, PEBS, LBR). Estas características las usa el software de ajuste del rendimiento como el amplificador Intel VTune para analizar el rendimiento del software.  Antes de Windows Server 2019 y Windows 10 versión 1809, ni el sistema operativo host ni las máquinas virtuales invitadas de Hyper-V podían usar hardware de supervisión de rendimiento cuando se habilitaba Hyper-V.  A partir de Windows Server 2019 y Windows 10 versión 1809, el sistema operativo host tiene acceso de forma predeterminada al hardware de supervisión de rendimiento.  Las máquinas virtuales invitadas de Hyper-V no tienen acceso de forma predeterminada, pero los administradores de Hyper-V pueden optar por conceder acceso a una o varias máquinas virtuales invitadas.  En este documento se describen los pasos necesarios para exponer el hardware de supervisión de rendimiento a las máquinas virtuales invitadas.
 
## <a name="requirements"></a>Requisitos 
Para exponer el hardware de supervisión de rendimiento en una máquina virtual, necesitará lo siguiente:
- Un procesador Intel con hardware de supervisión de rendimiento (es decir, PMU, PEBS, IPT)
- Windows Server 2019 o Windows 10 versión 1809 (actualización de octubre de 2018) o posterior
- Una máquina virtual de Hyper-V _sin_ [Virtualización anidada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) en estado detenido
 
## <a name="exposing-the-pmu-capabilities-pmu-lbr-pebs-to-virtual-machines-via-powershells-set-vmprocessor-cmdlet"></a>Exposición de las funcionalidades de PMU (PMU, LBR, PEBS) a las máquinas virtuales mediante el cmdlet Set-VMProcessor de PowerShell
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
 
>Nota: Al habilitar los componentes de Perfmon, `"pebs"` si se especifica `"pmu"` , se debe especificar.  Además, la habilitación de un componente de Perfmon que no sea compatible con los procesadores físicos del host producirá un error de inicio de la máquina virtual.
 
## <a name="effect-of-pmu-enabelement-on-saverestore-export-and-live-migration"></a>Efecto de PMU enabelement en el guardado, la restauración, la exportación y la migración en vivo
 
Microsoft no recomienda la migración en vivo o el almacenamiento o la restauración de máquinas virtuales con hardware de supervisión de rendimiento entre sistemas con hardware de Intel diferente. El comportamiento específico del hardware de supervisión de rendimiento suele ser no arquitectónico y cambios entre los sistemas de hardware de Intel.  Mover una máquina virtual en ejecución entre diferentes sistemas puede producir un comportamiento impredecible de los contadores no arquitectónicos.