---
title: Resolver problemas de vRSS
description: Resuelva los problemas de vRSS si no ve el tráfico de equilibrio de carga de vRSS en el LPs de VM.
ms.topic: article
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/04/2018
ms.openlocfilehash: a7f0a3b190232cafb68e3a39104c357972831441
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951955"
---
# <a name="resolve-vrss-issues"></a>Resolver problemas de vRSS

Si ha completado todos los pasos de preparación y sigue sin ver el tráfico de equilibrio de carga de vRSS en los LPs de la máquina virtual, hay diferentes problemas posibles.

1. Antes de realizar los pasos de preparación, vRSS estaba deshabilitado y ahora debe estar habilitado. Puede ejecutar **set-VMNetworkAdapter** para habilitar vRSS para la máquina virtual.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS se ha deshabilitado en la máquina virtual o en el host vNIC. Windows Server 2016 habilita RSS de forma predeterminada; es posible que alguien lo haya deshabilitado.

   - Enabled = **true**

   **Vea la configuración actual:**

   Ejecute el siguiente cmdlet de PowerShell en la máquina virtual \( para vrss en una máquina virtual \) o en el host para el host \( VNIC vrss \) .

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Habilite la característica:**

   Para cambiar el valor de false a true, ejecute el siguiente cmdlet de PowerShell.

   ```PowerShell
   Enable-NetAdapterRss *
   ```

   Otra forma de configurar RSS es usar netsh. Uso

    ```cmd
   netsh int tcp show global
   ```

   para asegurarse de que RSS no esté deshabilitado globalmente. Y habilítelo si es necesario. *-NetAdapterRSS no toca esta configuración.

3. Si VMMQ no está habilitado después de configurar vRSS, Compruebe la siguiente configuración en cada adaptador conectado al conmutador virtual:

   - VmmqEnabled = **false**
   - VmmqEnabledRequested = **true**

   ![habilitada para vmmq](../../media/vmmq-enabled.png)

   **Vea la configuración actual:**

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Habilite la característica:**

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```

4. _(Windows Server 2019)_ No se puede habilitar VMMQ (VmmqEnabled = false) mientras se establece **VrssQueueSchedulingMode** en **Dynamic**. VrssQueueSchedulingMode no cambia a Dynamic una vez que VMMQ está habilitado.<p>La **VrssQueueSchedulingMode** de **Dynamic** requiere compatibilidad con controladores cuando se habilita VMMQ.  VMMQ es una descarga de la ubicación de paquetes en procesadores lógicos y, como tal, requiere compatibilidad con el controlador para aprovechar el algoritmo dinámico.  Instale el controlador y el firmware del proveedor de la NIC que admita VMMQ dinámica.



---
