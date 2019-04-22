---
title: Resolver problemas de vRSS
description: Resolver problemas de vRSS si no ve la carga del tráfico de equilibrio para la máquina virtual de LPs vRSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: a2d6eb43149361b4270565b63fc99f483f364f74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824036"
---
## <a name="resolve-vrss-issues"></a>Resolver problemas de vRSS

Si ha completado todos los pasos de preparación y aún no ve vRSS la carga del tráfico de equilibrio para la máquina virtual de LPs, hay diferentes posibles problemas.

1. Antes de realizar los pasos de preparación, vRSS se deshabilitó - y ahora debe estar habilitada. Puede ejecutar **Set-VMNetworkAdapter** habilitar vRSS para la máquina virtual.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. Se deshabilitó RSS en la máquina virtual o en la vNIC de host. Windows Server 2016 habilita RSS de forma predeterminada; alguien podría haber deshabilitado. 

   - Habilitado = **True**

   **Ver la configuración actual:** 

   Ejecute el siguiente cmdlet de PowerShell en la máquina virtual\(para vRSS en una máquina virtual\) o en el host \(para host vNIC vRSS\).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Habilitar la característica:** 

   Para cambiar el valor de False a True, ejecute el siguiente cmdlet de PowerShell.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Otra manera de todo el sistema para configurar RSS usa netsh. Usar 
   
    ```cmd
   netsh int tcp show global
   ```
   
   para asegurarse de que ese RSS no está deshabilitado globalmente. Y habilítela si es necesario. Esta configuración no usada por *-NetAdapterRSS.

3. Si encuentra que VMMQ no está habilitada después de configurar vRSS, compruebe la siguiente configuración en cada adaptador conectado al conmutador virtual:

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq-enabled](../../media/vmmq-enabled.png)

   **Ver la configuración actual:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Habilitar la característica:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_  No se puede habilitar VMMQ (VmmqEnabled = False) durante la instalación **VrssQueueSchedulingMode** a **dinámica**. El VrssQueueSchedulingMode no cambia a dinámico una vez habilitada la VMMQ.<p>El **VrssQueueSchedulingMode** de **dinámica** requiere la compatibilidad de controladores cuando se habilita VMMQ.  VMMQ es una descarga de la colocación de paquetes en procesadores lógicos y por lo tanto, requiere la compatibilidad de controladores para aprovechar el algoritmo dinámico.  Instale el proveedor de tarjetas NIC controladores y firmware que admita VMMQ dinámica.



---
