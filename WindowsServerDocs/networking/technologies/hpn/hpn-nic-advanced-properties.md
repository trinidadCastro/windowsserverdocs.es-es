---
title: Propiedades avanzadas de NIC
description: Puede administrar las NIC y todas las características a través de Windows PowerShell o el Panel de Control de la red.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: d1a5fb57bf71fd981e001cfd9ac595ab5bc3cfc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819906"
---
# <a name="nic-advanced-properties"></a>Propiedades avanzadas de NIC

Puede administrar las NIC y todas las características a través de Windows PowerShell mediante el [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) cmdlet.  También puede administrar las NIC y todas las características mediante el Panel de Control de red (ncpa.cpl). 

1. En **Windows PowerShell**, ejecute el `Get‑NetAdapterAdvancedProperties` cmdlet en dos diferentes marca y modelo de NIC.

   ![Get-NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Existen algunas similitudes y diferencias en estos dos NIC avanzada propiedades se muestran.

2. En el **red del Panel de Control** (ncpa.cpl), haga lo siguiente:

   a. y haga clic en la NIC.

   ![Cuadro de diálogo de conexiones de red](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. En el cuadro de diálogo Propiedades, haga clic en **configurar**.

    ![Propiedades de C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Haga clic en el **avanzadas** pestaña para ver las propiedades avanzadas.<p>Los elementos de esta lista se correlaciona con los elementos de la `Get-NetAdapterAdvancedProperties` salida.

   ![Propiedades del adaptador de red Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---