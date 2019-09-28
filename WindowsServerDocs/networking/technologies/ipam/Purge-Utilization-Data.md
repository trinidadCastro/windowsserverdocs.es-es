---
title: Purgado de datos de utilización
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9e1db31e4d2d714c358f2a67c2165aef91b314ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405605"
---
# <a name="purge-utilization-data"></a>Purgado de datos de utilización

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo eliminar datos de uso de la base de datos de IPAM.  

Para llevar a cabo este procedimiento, debe ser miembro de **los administradores de IPAM**, del grupo de **administradores** del equipo local o de un grupo equivalente.

## <a name="to-purge-the-ipam-database"></a>Para purgar la base de datos de IPAM  
1. Abra Administrador del servidor y, a continuación, vaya a la interfaz de cliente de IPAM.
2. Examine una de las siguientes ubicaciones: **Bloques de direcciones**IP, **inventario de direcciones IP**o grupos de intervalos de **direcciones IP**.  
3. Haga clic en **tareas**y en **purgar datos de uso**. Se abre el cuadro de diálogo **purgar datos de uso** .
4. En **purgar todos los datos de uso en o antes**, haga clic en **seleccionar una fecha**.
5. Elija la fecha en la que desea eliminar todos los registros de la base de datos y antes de esa fecha.
6. Haga clic en **Aceptar**. IPAM elimina todos los registros que haya especificado.
