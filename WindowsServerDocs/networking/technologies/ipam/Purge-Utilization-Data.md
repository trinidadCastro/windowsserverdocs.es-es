---
title: Purgado de datos de utilización
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab2bd3ad1ef8965400e09fa74c6eb89ffc5ebcef
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283842"
---
# <a name="purge-utilization-data"></a>Purgado de datos de utilización

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre cómo eliminar datos de uso de la base de datos IPAM.  

Debe ser miembro del **administradores IPAM**, el equipo local **administradores** , o un grupo equivalente para realizar este procedimiento.

## <a name="to-purge-the-ipam-database"></a>Para purgar la base de datos IPAM  
1. Abra el administrador del servidor y, a continuación, vaya a la interfaz de cliente IPAM.
2. Examine una de las siguientes ubicaciones: **Bloques de direcciones IP**, **inventario de direcciones IP**, o **grupos de intervalos de direcciones IP**.  
3. Haga clic en **tareas**y, a continuación, haga clic en **purgar datos de uso**. El **purgar datos de uso** abre el cuadro de diálogo.
4. En **purgar la utilización de todos los datos en o antes de**, haga clic en **seleccionar una fecha**.
5. Elija la fecha para el que desea eliminar todos los registros de base de datos en y antes de esa fecha.
6. Haga clic en **Aceptar**. IPAM elimina todos los registros que ha especificado.
