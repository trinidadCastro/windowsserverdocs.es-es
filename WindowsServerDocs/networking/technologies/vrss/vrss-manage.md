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
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fc2feb9101c84566304910b1ee24f13f1ac277f1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315353"
---
# <a name="manage-vrss"></a>Administrar vRSS

En este tema, se usan los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales \(máquinas virtuales\) y en hosts de Hyper\-V.

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

En el host de Hyper-V, también debe habilitar vRSS en el puerto del conmutador virtual de Hyper\-V.

**Vea la configuración actual:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Las dos opciones siguientes deben ser **verdaderas**. 

- VrssEnabledRequested: true
- VrssEnabled: true
    
>[!IMPORTANT]
>En algunas condiciones de limitación de recursos, un puerto de conmutador virtual de Hyper\-V podría no ser capaz de habilitar esta característica. Se trata de una condición temporal y la característica puede estar disponible en un momento posterior.
>
>Si **VrssEnabled** es **true**, la característica está habilitada para este puerto de conmutador virtual de Hyper\-V, es decir, para esta máquina virtual o VNIC.

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
> La configuración del perfil dentro de la máquina virtual no afecta a la programación del trabajo. Hyper\-V toma todas las decisiones de programación y omite el perfil dentro de la máquina virtual.

## <a name="disable-vrss"></a>Deshabilitar vRSS

Puede deshabilitar vRSS para deshabilitar cualquiera de las opciones de configuración anteriormente mencionadas.

- Deshabilite VMQ para la NIC física o la máquina virtual.

  >[!CAUTION]
  >Deshabilitar VMQ en la NIC física afecta gravemente a la capacidad del host de Hyper\-V para administrar los paquetes entrantes.

- Deshabilite vRSS para una máquina virtual en el puerto del conmutador virtual de Hyper\-V en el host de Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Deshabilite vRSS para un host vNIC en el puerto del conmutador virtual de Hyper\-V en el host de Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Deshabilite RSS en la máquina virtual \(o el host vNIC\) dentro de la máquina virtual \(o en el host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
