---
title: Resolver problemas de vRSS
description: Resolver problemas de vRSS si no ves la carga del tráfico equilibrio el lógicos (LP) de máquina virtual vRSS.
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
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232524"
---
## Resolver problemas de vRSS

Si has completado todos los pasos de preparación y todavía no ves vRSS la carga del tráfico equilibrio el lógicos (LP) de máquina virtual, hay diferentes posibles problemas.

1. Antes de realizar los pasos de preparación, vRSS ha deshabilitado - y ahora debe estar habilitado. Puedes ejecutar **Conjunto VMNetworkAdapter** para habilitar vRSS para la máquina virtual.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS was disabled in the VM or on the host vNIC. Windows Server 2016 enables RSS by default; someone might have disabled it. 

   - Habilitado = **True**

   **Ver la configuración actual:** 

   Ejecuta el siguiente cmdlet de PowerShell en el VM\ (para vRSS en un VM\) o en el host \ (para vRSS\ de vNIC de host).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Habilitar la característica:** 

   Para cambiar el valor de False en True, ejecuta el siguiente cmdlet de PowerShell.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Another system-wide way to configure RSS is using netsh. Usar 
   
    ```cmd
   netsh int tcp show global
   ```
   
   to make sure that RSS isn't disabled globally. Y habilitarlo si es necesario. Esta configuración no se tocó por *-NetAdapterRSS.

3. Si encuentras que VMMQ no está habilitado después de configurar vRSS, comprueba la configuración siguiente en cada adaptador conectado al conmutador virtual:

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq habilitado](../../media/vmmq-enabled.png)

   **Ver la configuración actual:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Habilitar la característica:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ No se puede habilitar VMMQ (VmmqEnabled = "false") durante la configuración del **VrssQueueSchedulingMode** a **dinámico**. El VrssQueueSchedulingMode no cambia a dinámico una vez habilitada la VMMQ.<p>El **VrssQueueSchedulingMode** de **Dynamic** requiere compatibilidad con los controladores cuando se habilita VMMQ.  VMMQ es una descarga de la colocación de paquetes en procesadores lógicos y como tal, requiere compatibilidad con los controladores para aprovechar el algoritmo dinámico.  Instale el proveedor NIC controladores y firmware que admita VMMQ dinámica.



---
