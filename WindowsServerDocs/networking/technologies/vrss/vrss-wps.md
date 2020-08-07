---
title: Comandos de Windows PowerShell para RSS y vRSS
description: En este tema, aprenderá a ubicar rápidamente información de referencia técnica sobre los comandos de Windows PowerShell para el ajuste de escala en lado de recepción (RSS) y el RSS virtual (vRSS).
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/05/2018
ms.openlocfilehash: 6b44cdfec4778cf7f36f541021f23a073cb17806
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964011"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Comandos de Windows PowerShell para RSS y vRSS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, aprenderá a ubicar rápidamente información de referencia técnica sobre los comandos de Windows PowerShell para el ajuste de escala en lado de recepción \( RSS \) y la VRSS de RSS virtual \( \) .

Use los siguientes comandos RSS para configurar RSS en un equipo físico con varios procesadores o varios núcleos. Puede usar los mismos comandos para configurar vRSS en una VM de máquina \( virtual \) que ejecuta un sistema operativo compatible. Para obtener más información, consulte [cmdlets de adaptador de red en Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configuración de VMQ

vRSS requiere que VMQ esté habilitado y configurado. Puede usar los siguientes comandos de Windows PowerShell para administrar la configuración de VMQ.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Habilitar y configurar RSS en un host nativo

Use los siguientes comandos de PowerShell para configurar RSS en un host nativo, así como para administrar RSS en una máquina virtual o en una NIC virtual de host (vNIC). Algunos de los parámetros de estos comandos también pueden afectar a Virtual Machine Queue \( VMQ \) en el host de Hyper-V.

>[!IMPORTANT]
>Habilitar RSS en una máquina virtual o en un host vNIC es un requisito previo para habilitar y usar vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Habilitar vRSS en el \- Puerto del conmutador virtual de Hyper V

Además de habilitar RSS en la máquina virtual, vRSS requiere que habilite vRSS en el puerto del \- conmutador virtual de Hyper V.

Determine la configuración actual de vRSS y habilite o deshabilite la característica para una máquina virtual.

   **Vea la configuración actual:**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Habilitada la característica:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Habilitar o deshabilitar vRSS en un host vNIC

Determine la configuración actual de vRSS y habilite o deshabilite la característica para un host vNIC.

   **Vea la configuración actual:**

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Habilitar o deshabilitar la característica:**

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurar el modo de programación en el puerto del conmutador virtual de Hyper-V
>Se aplica a: Windows Server 2019

En Windows Server 2019, vRSS puede actualizar los procesadores lógicos que se usan para procesar el tráfico de red de forma dinámica.  Los dispositivos con controladores admitidos tienen este modo de programación habilitado de forma predeterminada.

Determine el modo de programación presente en un sistema o modifique el modo de programación de una máquina virtual.

   **Vea la configuración actual:**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Establecer o modificar el modo de programación:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurar el modo de programación en un host vNIC
>Se aplica a: Windows Server 2019

Para determinar el modo de programación actual o para modificar el modo de programación de un host vNIC, use los siguientes comandos de Windows PowerShell:

   **Vea la configuración actual:**

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Establecer o modificar el modo de programación:**

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Temas relacionados
Para obtener más información, vea los temas de referencia siguientes.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Establecer-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Para obtener más información, vea [ajuste de escala en lado de recepción virtual (vRSS)](vrss-top.md).