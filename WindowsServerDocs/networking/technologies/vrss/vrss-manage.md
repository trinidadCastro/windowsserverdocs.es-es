---
title: Administrar vRSS
description: En este tema, se usan los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales (VM) y en hosts de Hyper-V.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d528f7e658d61f613eedc635fb81d8f18fd59aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405169"
---
# <a name="manage-vrss"></a>Administrar vRSS

En este tema, se usan los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales \(VMs @ no__t-1 y en hosts de Hyper @ no__t-2V.

>[!NOTE]
>Para obtener más información sobre los comandos que se mencionan en este tema, vea [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>VMQ en hosts de Hyper-V

En el host de Hyper-V, debe usar las palabras clave que controlan los procesadores VMQ.

**Vea la configuración actual:** 

```PowerShell
Get-NetAdapterVmq
```

**Configure las opciones de VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>vRSS en puertos de conmutador de Hyper-V

En el host de Hyper-V, también debe habilitar vRSS en el puerto del conmutador virtual de Hyper @ no__t-0V.

**Vea la configuración actual:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Las dos opciones siguientes deben ser **verdaderas**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>En algunas condiciones de limitación de recursos, un puerto de conmutador virtual de Hyper @ no__t-0V podría no ser capaz de habilitar esta característica. Se trata de una condición temporal y la característica puede estar disponible en un momento posterior.
>
>Si **VrssEnabled** es **true**, la característica está habilitada para este puerto de conmutador virtual de Hyper-no__t-2V, es decir, para esta máquina virtual o VNIC.

**Configure la configuración de vRSS del puerto del conmutador:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS en máquinas virtuales y host VNIC

Puede usar los mismos comandos usados para RSS nativo para configurar las opciones de vRSS en máquinas virtuales y host VNIC, que también es la forma de habilitar RSS en host VNIC.  

**Vea la configuración actual:**

```PowerShell
Get-NetAdapterRSS
```

**Configure las opciones de vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> La configuración del perfil dentro de la máquina virtual no afecta a la programación del trabajo. Hyper @ no__t-0V toma todas las decisiones de programación y omite el perfil dentro de la máquina virtual.

## <a name="disable-vrss"></a>Deshabilitar vRSS

Puede deshabilitar vRSS para deshabilitar cualquiera de las opciones de configuración anteriormente mencionadas.

- Deshabilite VMQ para la NIC física o la máquina virtual.

  >[!CAUTION]
  >Deshabilitar VMQ en la NIC física afecta gravemente a la capacidad del host de Hyper-no__t-0V para controlar los paquetes entrantes.

- Deshabilite vRSS para una máquina virtual en el puerto del conmutador virtual de Hyper @ no__t-0V en el host de Hyper-no__t-1V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Deshabilite vRSS para un host vNIC en el puerto del conmutador virtual de Hyper @ no__t-0V en el host de Hyper @ no__t-1V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Deshabilite RSS en el host \(or de la máquina virtual vNIC @ no__t-1 dentro de la máquina virtual \(or en el host @ no__t-3.

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
