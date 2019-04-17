---
title: Administrar vRSS
description: En este tema, usan los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales (VM) y en los hosts de Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8af800608bee7037b48141a7a2edb0c872a7aac0
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133841"
---
# Administrar vRSS

En este tema, usan los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales \(VMs\) y en hosts de Hyper\-V.

>[!NOTE]
>For more information about the commands mentioned in this topic, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

## VMQ en Hosts de Hyper-V

En el host de Hyper-V, debes usar las palabras clave que controlan los procesadores VMQ.

**Ver la configuración actual:** 

```PowerShell
Get-NetAdapterVmq
```

**Configura las opciones de VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## puertos de conmutador vRSS en Hyper-V

En el host de Hyper-V, también debes habilitar vRSS en el puerto de conmutador Virtual de Hyper\-V.

**Ver la configuración actual:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Ambos de la siguiente configuración deben ser **True**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>En algunas condiciones de limitación de recursos, un puerto de conmutador Virtual de Hyper\-V puede que no puedo tener esta característica está habilitada. Se trata de una condición temporal y la característica puede estar disponible en un momento posterior.
>
>Si **VrssEnabled** es **True**, la característica está habilitada para este puerto de conmutador Virtual de Hyper\-V; es decir, para esta máquina virtual o vNIC.

**Configura las opciones de vRSS de puerto de conmutador:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## vRSS en las máquinas virtuales y las vNICs de host

You can use the same commands used for native RSS to configure vRSS settings in VMs and host vNICs, which is also the way to enable RSS on host vNICs.  

**Ver la configuración actual:**

```PowerShell
Get-NetAdapterRSS
```

**Configura las opciones de vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Configurar el perfil dentro de la máquina virtual no afecta a la programación del trabajo. Hyper\-V hace que todas las decisiones de programación y omite el perfil dentro de la máquina virtual.

## Deshabilitar vRSS

Puedes deshabilitar vRSS para deshabilitar cualquiera de las opciones de configuración se mencionó anteriormente.

- Deshabilitar VMQ para la NIC física o la máquina virtual.

  >[!CAUTION]
  >Al deshabilitar VMQ en la física NIC gravemente afecta a la capacidad del host de Hyper\-V para controlar los paquetes entrantes.

- Deshabilitar vRSS para una máquina virtual en el puerto de conmutador Virtual de Hyper\-V en el host de Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Deshabilitar vRSS para una vNIC de host en el puerto de conmutador Virtual de Hyper\-V en el host de Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Disable RSS in the VM \(or host vNIC\) inside the VM \(or on the host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
