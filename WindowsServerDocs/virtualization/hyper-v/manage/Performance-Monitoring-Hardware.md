---
title: Habilitación del hardware de supervisión del rendimiento de Intel en una máquina virtual de Hyper-V
description: Cómo habilitar el hardware de supervisión de rendimiento de Intel en una máquina de Hyper-V. También toca cómo habilitar la supervisión de rendimiento de los efectos de hardware en la migración en vivo.
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 73ab88d6ee5d50ae8b433a064a7cea7c249094a2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996751"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>Habilitación del hardware de supervisión del rendimiento de Intel en una máquina virtual de Hyper-V

Los procesadores Intel contienen características denominados colectivamente hardware de supervisión de rendimiento (por ejemplo, PMU, PEBS, LBR). Estas características las usa el software de ajuste del rendimiento como el amplificador Intel VTune para analizar el rendimiento del software.  Antes de Windows Server 2019 y Windows 10 versión 1809, ni el sistema operativo host ni las máquinas virtuales invitadas de Hyper-V podían usar hardware de supervisión de rendimiento cuando se habilitaba Hyper-V.  A partir de Windows Server 2019 y Windows 10 versión 1809, el sistema operativo host tiene acceso de forma predeterminada al hardware de supervisión de rendimiento.  Las máquinas virtuales invitadas de Hyper-V no tienen acceso de forma predeterminada, pero los administradores de Hyper-V pueden optar por conceder acceso a una o varias máquinas virtuales invitadas.  En este documento se describen los pasos necesarios para exponer el hardware de supervisión de rendimiento a las máquinas virtuales invitadas.

## <a name="requirements"></a>Requisitos

Para habilitar el hardware de supervisión de rendimiento en una máquina virtual, necesitará lo siguiente:

- Un procesador Intel con hardware de supervisión de rendimiento (es decir, PMU, PEBS, LBR).  Consulte [este documento]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis) de Intel para determinar qué hardware de supervisión de rendimiento es compatible con el sistema.
- Windows Server 2019 o Windows 10 versión 1809 (actualización de octubre de 2018) o posterior
- Una máquina virtual de Hyper-V _sin_ [Virtualización anidada](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) que también esté en estado detenido

Para habilitar el siguiente hardware de supervisión de rendimiento de seguimiento del procesador Intel (IPT) en una máquina virtual, necesitará:

- Procesador Intel compatible con IPT y la característica PT2GPA.  Consulte [este documento]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis) de Intel para determinar qué hardware de supervisión de rendimiento es compatible con el sistema.
- Windows Server versión 1903 (SAC) o Windows 10 versión 1903 (actualización de mayo de 2019) o posterior
- Una máquina virtual de Hyper-V _sin_ [Virtualización anidada](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) que también esté en estado detenido

## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>Habilitación de los componentes de supervisión de rendimiento en una máquina virtual

Para habilitar distintos componentes de supervisión de rendimiento para una máquina virtual invitada específica, use el `Set-VMProcessor` cmdlet de PowerShell mientras se ejecuta como administrador:

``` Powershell
# Enable all components except IPT
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```

``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```

``` Powershell
# Enable IPT
Set-VMProcessor MyVMName -Perfmon @("ipt")
```

``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
> [!NOTE]
> Al habilitar los componentes de supervisión de rendimiento, si `"pebs"` se especifica, `"pmu"` también se debe especificar.
> PEBS solo se admite en hardware con una versión de PMU >= 4.
> La habilitación de un componente que no sea compatible con los procesadores físicos del host producirá un error de inicio de la máquina virtual.

## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>Efectos de habilitar el hardware de supervisión de rendimiento en guardar/restaurar, exportar y migración en vivo

Microsoft no recomienda la migración en vivo o el almacenamiento o la restauración de máquinas virtuales con hardware de supervisión de rendimiento entre sistemas con hardware de Intel diferente. El comportamiento específico del hardware de supervisión de rendimiento suele ser no arquitectónico y cambios entre los sistemas de hardware de Intel.  Mover una máquina virtual en ejecución entre diferentes sistemas puede producir un comportamiento impredecible de los contadores no arquitectónicos.