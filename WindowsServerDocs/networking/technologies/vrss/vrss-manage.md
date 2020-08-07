---
title: Administrar vRSS
description: En este tema, se usan los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales (VM) y en hosts de Hyper-V.
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 553be94dd3fe94a74ee9deb84a5ac6d47265afec
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955542"
---
# <a name="manage-vrss"></a>Administrar vRSS

En este tema, se usan los comandos de Windows PowerShell para administrar vRSS en máquinas virtuales de máquinas virtuales \( \) y en hosts de Hyper- \- V.

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

En el host de Hyper-V, también debe habilitar vRSS en el \- Puerto del conmutador virtual de Hyper V.

**Vea la configuración actual:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```

Las dos opciones siguientes deben ser **verdaderas**.

- VrssEnabledRequested: true
- VrssEnabled: true

>[!IMPORTANT]
>En algunas condiciones de limitación de recursos, \- es posible que un puerto de conmutador virtual de Hyper V no pueda tener esta característica habilitada. Se trata de una condición temporal y la característica puede estar disponible en un momento posterior.
>
>Si **VrssEnabled** es **true**, la característica está habilitada para este \- Puerto de conmutador virtual de Hyper V, es decir, para esta máquina virtual o VNIC.

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
> La configuración del perfil dentro de la máquina virtual no afecta a la programación del trabajo. Hyper \- V toma todas las decisiones de programación y omite el perfil dentro de la máquina virtual.

## <a name="disable-vrss"></a>Deshabilitar vRSS

Puede deshabilitar vRSS para deshabilitar cualquiera de las opciones de configuración anteriormente mencionadas.

- Deshabilite VMQ para la NIC física o la máquina virtual.

  >[!CAUTION]
  >Deshabilitar VMQ en la NIC física afecta gravemente a la capacidad del host de Hyper- \- V para administrar los paquetes entrantes.

- Deshabilite vRSS para una máquina virtual en el \- Puerto del conmutador virtual de Hyper-v en el host de Hyper- \- v.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Deshabilite vRSS para un host vNIC en el \- Puerto del conmutador virtual de Hyper v en el host de Hyper- \- v.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Deshabilitación de RSS en la máquina virtual \( o el host VNIC \) dentro de la máquina virtual \( o en el host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
