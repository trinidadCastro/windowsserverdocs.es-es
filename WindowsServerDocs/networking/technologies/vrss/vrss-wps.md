---
title: Windows PowerShell Commands for RSS and vRSS
description: In this topic, you learn how to quickly locate technical reference information about Windows PowerShell commands for Receive Side Scaling (RSS) and virtual RSS (vRSS).
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
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339273"
---
# Windows PowerShell Commands for RSS and vRSS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

In this topic, you learn how to quickly locate technical reference information about Windows PowerShell commands for Receive Side Scaling \(RSS\) and virtual RSS \(vRSS\).

Use the following RSS commands to configure RSS on a physical computer with multiple processors or multiple cores. Puedes usar los mismos comandos para configurar vRSS en una máquina virtual \(VM\) que se está ejecutando un sistema operativo compatible. Para obtener más información, consulta [Los Cmdlets de adaptador de red en Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## Configurar VMQ

vRSS requiere que VMQ está habilitado y configurado. Puedes usar los siguientes comandos de Windows PowerShell para administrar la configuración de VMQ.

- [Deshabilitar NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## Enable and configure RSS on a native host

Use the following PowerShell commands to configure RSS on a native host as well as manage RSS in a VM or on a host virtual NIC (vNIC). Algunos de los parámetros de estos comandos también pueden afectar a la cola de máquina Virtual \(VMQ\) en el host de Hyper-V.  

>[!IMPORTANT]
>Enabling RSS in a VM or on a host vNIC is a prerequisite for enabling and using vRSS.

- [Deshabilitar NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## Habilitar vRSS en el puerto de conmutador Virtual de Hyper\-V

In addition to enabling RSS in the VM, vRSS requires that you enable vRSS on the Hyper\-V Virtual Switch port. 

Determinar la configuración presente para vRSS y habilitar o deshabilitar la característica para una máquina virtual.

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Habilitar la característica:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## Habilitar o deshabilitar vRSS en una vNIC de host

Determinar la configuración presente para vRSS y habilitar o deshabilitar la característica para una vNIC de host.

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Habilitar o deshabilitar la característica:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## Configurar el modo de programación en el puerto de conmutador virtual de Hyper-V 
>Se aplica a: Windows Server 2019

En Windows Server 2019, vRSS actualizar los procesadores lógicos que se usa para procesar el tráfico de red de forma dinámica.  Los dispositivos con controladores compatibles tienen este modo programación habilitada de manera predeterminada. 

Determinar el modo de programación presente en un sistema, o modifique el modo de programación para una máquina virtual.

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Establecer o modificar el modo de programación:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## Configurar el modo de programación en una vNIC de host
>Se aplica a: Windows Server 2019

Para determinar el modo de programación presente o para modificar el modo de programación para una vNIC de host, usa los siguientes comandos de Windows PowerShell:

   **Ver la configuración actual:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Establecer o modificar el modo de programación:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## Temas relacionados 
Para obtener más información, consulta los siguientes temas de referencia.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Para obtener más información, consulta [Recibir lado del ajuste de escala Virtual (vRSS)](vrss-top.md).