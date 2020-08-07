---
title: Propiedades avanzadas de NIC
description: Puede administrar las NIC y todas las características a través de Windows PowerShell o el panel de control de red.
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: aa26e3ef83ec6255da6a386ecfce34a7501ae3f5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955642"
---
# <a name="nic-advanced-properties"></a>Propiedades avanzadas de NIC

Puede administrar las NIC y todas las características a través de Windows PowerShell mediante el cmdlet [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) .  También puede administrar las NIC y todas las características mediante el panel de control de red (ncpa.cpl).

1. En **Windows PowerShell**, ejecute el `Get‑NetAdapterAdvancedProperties` cmdlet con dos marca o modelo de NIC diferente.

   ![Get-NetAdapterAdvancedProperty M1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty C1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Existen similitudes y diferencias en estas dos listas de propiedades avanzadas de NIC.

2. En el **Panel de control de red** (ncpa.cpl), haga lo siguiente:

   a. Haga clic con el botón derecho en la NIC.

   ![Cuadro de diálogo conexiones de red](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. En el cuadro de diálogo Propiedades, haga clic en **configurar**.

    ![Propiedades de C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Haga clic en la pestaña **avanzadas** para ver las propiedades avanzadas.<p>Los elementos de esta lista se correlacionan con los elementos de la `Get-NetAdapterAdvancedProperties` salida.

   ![Propiedades del adaptador de red de Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---
