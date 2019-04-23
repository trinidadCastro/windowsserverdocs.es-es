---
title: Comandos de Windows PowerShell para RSS y vRSS
description: En este tema, aprenderá a buscar rápidamente la información de referencia técnica acerca de los comandos de Windows PowerShell para escalar de lado de recepción (RSS) y RSS virtual (vRSS).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: 10039388009e32c10d71067b835bad65db5607ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833266"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Comandos de Windows PowerShell para RSS y vRSS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, aprenderá a buscar rápidamente la información de referencia técnica acerca de los comandos de Windows PowerShell para la escala de recepción \(RSS\) y RSS virtual \(vRSS\).

Use los siguientes comandos RSS configurar RSS en un equipo físico con varios procesadores o varios núcleos. Puede usar los mismos comandos para configurar vRSS en una máquina virtual \(VM\) que se está ejecutando un sistema operativo compatible. Para obtener más información, consulte [Cmdlets de adaptador de red en Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configurar VMQ

vRSS requiere que VMQ está habilitado y configurado. Puede usar los siguientes comandos de Windows PowerShell para administrar la configuración de VMQ.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Habilitar y configurar RSS en un host nativo

Use los siguientes comandos de PowerShell para configurar RSS en un host nativo así como administrar RSS en una máquina virtual o en un host de NIC virtual (vNIC). Algunos de los parámetros de estos comandos también pueden afectar a Virtual Machine Queue \(VMQ\) en el host de Hyper-V.  

>[!IMPORTANT]
>Al habilitar RSS en una máquina virtual o un VNIC de host es un requisito previo para habilitar y usar vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Habilite vRSS en Hyper\-puerto del conmutador Virtual V

Además de habilitar RSS en la máquina virtual, vRSS requiere que habilite vRSS en Hyper\-puerto del conmutador Virtual V. 

Determinar la configuración presente para vRSS y habilitar o deshabilitar la característica para una máquina virtual.

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Habilitar la característica:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Habilitar o deshabilitar vRSS un VNIC de host

Determinar la configuración presente para vRSS y habilitar o deshabilitar la característica de un vNIC de host.

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Habilitar o deshabilitar la característica:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurar el modo de programación en el puerto de conmutador virtual de Hyper-V 
>Se aplica a: Windows Server 2019

En Windows Server 2019, vRSS puede actualizar los procesadores lógicos que se usa para procesar el tráfico de red de forma dinámica.  Los dispositivos con controladores admitidos tienen esta programación el modo habilitado de forma predeterminada. 

Determinar el modo de programación está presente en un sistema o modificar el modo de programación para una máquina virtual.

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Establecer o modificar el modo de programación:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurar el modo de programación en un vNIC de host
>Se aplica a: Windows Server 2019

Para determinar el modo de programación está presente o para modificar el modo de programación de un vNIC de host, use los siguientes comandos de Windows PowerShell:

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Establecer o modificar el modo de programación:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Temas relacionados 
Para obtener más información, consulte los siguientes temas de referencia.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Para obtener más información, consulte [Virtual escala de recepción (vRSS)](vrss-top.md).