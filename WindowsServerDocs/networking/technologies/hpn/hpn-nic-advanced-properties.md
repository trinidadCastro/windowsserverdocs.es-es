---
title: Propiedades avanzadas de NIC
description: Puede administrar las NIC y todas las características a través de Windows PowerShell o el panel de control de red.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: 78d320f4309d60fa0396cbd723feafa07a65aea1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317001"
---
# <a name="nic-advanced-properties"></a>Propiedades avanzadas de NIC

Puede administrar las NIC y todas las características a través de Windows PowerShell mediante el cmdlet [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) .  También puede administrar las NIC y todas las características mediante el panel de control de red (NCPA. cpl). 

1. En **Windows PowerShell**, ejecute el cmdlet `Get‑NetAdapterAdvancedProperties` con dos marca o modelo de NIC diferente.

   ![Get-NetAdapterAdvancedProperty M1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty C1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Existen similitudes y diferencias en estas dos listas de propiedades avanzadas de NIC.

2. En el **Panel de control de red** (NCPA. cpl), haga lo siguiente:

   a. Haga clic con el botón derecho en la NIC.

   ![Cuadro de diálogo conexiones de red](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. En el cuadro de diálogo Propiedades, haga clic en **configurar**.

    ![Propiedades de C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Haga clic en la pestaña **avanzadas** para ver las propiedades avanzadas.<p>Los elementos de esta lista se correlacionan con los elementos de la salida `Get-NetAdapterAdvancedProperties`.

   ![Propiedades del adaptador de red de Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---
