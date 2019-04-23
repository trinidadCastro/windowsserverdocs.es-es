---
title: Habilite vRSS en un adaptador de red Virtual
description: En este tema, aprenderá a habilitar vRSS en Windows Server mediante el Administrador de dispositivos o Windows PowerShell.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19e8011fb98b84c20e8237792664551d2362d589
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882686"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Habilite vRSS en un adaptador de red Virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

RSS virtual \(vRSS\) requiere Virtual Machine Queue \(VMQ\) admite el adaptador físico. Si VMQ está deshabilitado o no se admite, ajuste de escala en lado de recepción Virtual se deshabilita. 

Para obtener más información, consulte [planear el uso de vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Habilite vRSS en una máquina virtual
 
Use los procedimientos siguientes para habilitar vRSS mediante Windows PowerShell o el Administrador de dispositivos.

-   Administrador de dispositivos
-   Windows PowerShell
  
### <a name="device-manager"></a>Administrador de dispositivos

Puede usar este procedimiento para habilitar vRSS mediante el Administrador de dispositivos.

>[!NOTE]
>El primer paso en este procedimiento es específico para las máquinas virtuales que ejecutan Windows 10 o Windows Server 2016. Si la máquina virtual se está ejecutando un sistema operativo diferente, puede abrir el Administrador de dispositivos por primera abrir Panel de Control, a continuación, buscar y abrir el Administrador de dispositivos.
  
1.  En la barra de tareas de máquina virtual, en **escriba aquí para buscar**, tipo **dispositivo**. 

2.  En los resultados de búsqueda, haga clic en **Device Manager**.

3.  En el Administrador de dispositivos, haga clic para expandir **adaptadores de red**. 

4.  Haga clic en el adaptador de red que desea configurar y, a continuación, haga clic en **propiedades**.<p>El adaptador de red **propiedades** abre el cuadro de diálogo.

5.  En el adaptador de red **propiedades**, haga clic en el **avanzadas** ficha. 

6.  En **propiedad**, desplácese hacia abajo y haga clic en **ajuste de escala en lado de recepción**. 

7.  Asegúrese de que la selección en **valor** es **habilitado**. 

8.  Haga clic en **Aceptar**.
  
> [!NOTE]
> En el **avanzadas** ficha, algunos adaptadores de red también muestran el número de colas RSS que son compatibles con el adaptador.

---

### <a name="windows-powershell"></a>Windows PowerShell

Utilice el procedimiento siguiente para habilitar vRSS mediante Windows PowerShell.

1. En la máquina virtual, abra **Windows PowerShell**.

2. Escriba el siguiente comando, lo que garantiza que reemplace el *AdapterName* valor para el **-nombre** parámetro con el nombre del adaptador de red que desea configurar y, a continuación, presione ENTRAR. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Como alternativa, puede usar el siguiente comando para habilitar vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Para obtener más información, consulte [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

---