---
title: Depurar los datos de uso
description: Este tema es parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c41be119099aed4867df1bae1a55e2fbaa5c9064
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="purge-utilization-data"></a>Depurar los datos de uso

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a eliminar los datos de uso de la base de datos IPAM.  

Debe ser miembro del **los administradores de IPAM**, el equipo local **administradores** o un grupo equivalente, para realizar este procedimiento.

## <a name="to-purge-the-ipam-database"></a>Para purgar la base de datos IPAM  
1. Abre el administrador del servidor y, a continuación, busca la interfaz del cliente IPAM.
2. Vaya a una de las siguientes ubicaciones: **bloques de direcciones IP**, **inventario de dirección IP**, o **grupos de intervalo de direcciones IP**.  
3. Haz clic en **tareas**y, a continuación, haz clic en **purgar datos de uso**. La **purgar datos de uso** abre el cuadro de diálogo.
4. En **purgar la utilización de todos los datos o antes**, haz clic en **seleccionar una fecha**.
5. Elige la fecha en que quieres eliminar todos los registros de base de datos en y antes de esa fecha.
6. Haz clic en **Aceptar**. IPAM elimina todos los registros que hayas especificado.
