---
title: Administrar vRSS
description: En este tema, usa los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales (VM) y en hosts de Hyper-V.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856196"
---
# <a name="manage-vrss"></a>Administrar vRSS

En este tema, use los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales \(máquinas virtuales\) y en Hyper\-hosts V.

>[!NOTE]
>Para obtener más información acerca de los comandos que se mencionan en este tema, consulte [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>VMQ en Hosts de Hyper-V

En el host de Hyper-V, debe usar las palabras clave que controlan los procesadores VMQ.

**Ver la configuración actual:** 

```PowerShell
Get-NetAdapterVmq
```

**Configure las opciones de VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>los puertos del conmutador vRSS en Hyper-V

En el host de Hyper-V, también debe habilitar vRSS en Hyper\-puerto del conmutador Virtual V.

**Ver la configuración actual:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Ambos valores de las siguientes opciones deben estar **True**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>En determinadas condiciones de limitación de recursos, un Hyper\-puerto del conmutador Virtual V podrían no tener esta característica habilitada. Se trata de una condición temporal, y puede ser la característica de disponibilidad en un momento posterior.
>
>Si **VrssEnabled** es **True**, a continuación, la característica está habilitada para este Hyper\-puerto del conmutador Virtual V — es decir, para esta máquina virtual o una vNIC.

**Configure las opciones de vRSS del puerto de conmutador:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS en máquinas virtuales y las vNICs de host

Puede usar los mismos comandos que se usa para el RSS nativo configuración vRSS en las máquinas virtuales y la VNIC de host, que también es la forma de habilitar RSS en la VNIC de host.  

**Ver la configuración actual:**

```PowerShell
Get-NetAdapterRSS
```

**Configure las opciones de vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Configurar el perfil dentro de la máquina virtual no afecta a la programación del trabajo. Hyper\-V hace que toda la programación de las decisiones y omite el perfil dentro de la máquina virtual.

## <a name="disable-vrss"></a>Deshabilitar vRSS

Puede deshabilitar vRSS para deshabilitar cualquiera de las opciones mencionadas anteriormente.

- Deshabilitar VMQ para la NIC física o la máquina virtual.

  >[!CAUTION]
  >Deshabilitar VMQ en físico NIC afecta gravemente a la capacidad de su Hyper\-host V para administrar los paquetes entrantes.

- Deshabilitar vRSS para una máquina virtual en Hyper\-puerto Hyper V Virtual Switch\-host V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Deshabilitar vRSS para un vNIC de host de Hyper\-puerto Hyper V Virtual Switch\-host V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Deshabilitar RSS en la máquina virtual \(o vNIC de host\) dentro de la máquina virtual \(o en el host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
