---
title: Habilitar vRSS en un adaptador de Virtual Network
description: En este tema, aprenderá a habilitar vRSS en Windows Server mediante Device Manager o Windows PowerShell.
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1bc5756bde740e367df44701f6cc0db1284b5e2c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953841"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Habilitar vRSS en un adaptador de Virtual Network

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

\( \) La VRSS de RSS virtual \( requiere \) la compatibilidad con Virtual Machine Queue VMQ del adaptador físico. Si VMQ está deshabilitado o no se admite, se deshabilita el ajuste de escala en lado de recepción virtual.

Para obtener más información, consulte [planear el uso de vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Habilitación de vRSS en una máquina virtual

Use los procedimientos siguientes para habilitar vRSS mediante Windows PowerShell o Device Manager.

-   Administrador de dispositivos
-   Windows PowerShell

### <a name="device-manager"></a>Administrador de dispositivos

Puede usar este procedimiento para habilitar vRSS mediante Device Manager.

>[!NOTE]
>El primer paso de este procedimiento es específico de las máquinas virtuales que ejecutan Windows 10 o Windows Server 2016. Si la máquina virtual ejecuta un sistema operativo diferente, puede abrir Device Manager abriendo primero el panel de control y localizando y abriendo Device Manager.

1.  En la barra de tareas de la máquina virtual, en **Escriba aquí para buscar**, escriba **dispositivo**.

2.  En los resultados de la búsqueda, haga clic en **Device Manager**.

3.  En Device Manager, haga clic para expandir **adaptadores de red**.

4.  Haga clic con el botón secundario en el adaptador de red que desea configurar y, a continuación, haga clic en **propiedades**.<p>Se abrirá el cuadro de diálogo **propiedades** de adaptador de red.

5.  En **propiedades**de adaptador de red, haga clic en la pestaña **Opciones avanzadas** .

6.  En **propiedad**, desplácese hacia abajo hasta y haga clic en **ajuste de escala en lado de recepción**.

7.  Asegúrese de que la selección de **valor** está **habilitada**.

8.  Haga clic en **Aceptar**.

> [!NOTE]
> En la pestaña **Opciones avanzadas** , algunos adaptadores de red también muestran el número de colas RSS que admite el adaptador.

---

### <a name="windows-powershell"></a>Windows PowerShell

Use el procedimiento siguiente para habilitar vRSS mediante Windows PowerShell.

1. En la máquina virtual, Abra **Windows PowerShell**.

2. Escriba el siguiente comando, asegurándose de reemplazar el valor de *AdapterName* para el parámetro **-Name** por el nombre del adaptador de red que desea configurar y, a continuación, presione Entrar.

   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Como alternativa, puede usar el siguiente comando para habilitar vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True
   >```

Para obtener más información, vea [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

---