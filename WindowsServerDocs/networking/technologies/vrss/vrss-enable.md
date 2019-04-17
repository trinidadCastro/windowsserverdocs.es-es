---
title: Habilitar vRSS en un adaptador de red Virtual
description: En este tema, aprende a habilitar vRSS en Windows Server mediante el Administrador de dispositivos o Windows PowerShell.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133421"
---
# Habilitar vRSS en un adaptador de red Virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Virtual RSS \(vRSS\) requires Virtual Machine Queue \(VMQ\) support from the physical adapter. Si VMQ está deshabilitada o no se admite se deshabilita el ajuste de escala de lado de recepción Virtual. 

Para obtener más información, consulta [Planear el uso de vRSS](vrss-plan.md).

## Habilitar vRSS en una máquina virtual
 
Usa los siguientes procedimientos para habilitar vRSS mediante Windows PowerShell o el Administrador de dispositivos.

-   Administrador de dispositivos 
-   Windows PowerShell
  
### Administrador de dispositivos 

Puedes usar este procedimiento para habilitar vRSS mediante el Administrador de dispositivos.

>[!NOTE]
>El primer paso en este procedimiento es específico de las máquinas virtuales que ejecutan Windows 10 o Windows Server 2016. Si la máquina virtual se está ejecutando un sistema operativo diferente, puedes abrir el Administrador de dispositivos al primer abrir Panel de Control, a continuación, localizar y abrir el Administrador de dispositivos.
  
1.  En la barra de tareas de la máquina virtual, en el **tipo aquí para buscar**, escriba el **dispositivo**. 

2.  En los resultados de búsqueda, haz clic en **El Administrador de dispositivos**.

3.  En el Administrador de dispositivos, haga clic para expandir los **adaptadores de red**. 

4.  Haz clic en el adaptador de red que quieras configurar y, a continuación, haga clic en **Propiedades**.<p>Abre el cuadro de diálogo de **Propiedades** del adaptador de red.

5.  En **las propiedades**del adaptador de red, haz clic en la pestaña **Opciones avanzadas** . 

6.  En la **propiedad**, desplázate hacia abajo hasta y haz clic en el **lado de recepción de escala**. 

7.  Asegúrate de que la selección en el **valor** está **habilitado**. 

8.  Haz clic en **Aceptar**.
  
> [!NOTE]
> On the **Advanced** tab, some network adapters also display the number of RSS queues that are supported by the adapter.

---

### Windows PowerShell

Usar el siguiente procedimiento para habilitar vRSS mediante Windows PowerShell.

1. En la máquina virtual, abre **Windows PowerShell**.

2. Escribe el siguiente comando, garantizando que reemplazar el valor *AdapterName* para el **-nombre** parámetro con el nombre del adaptador de red que quieras configurar y, a continuación, presiona ENTRAR. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Como alternativa, puedes usar el siguiente comando para habilitar vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

For more information, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

---