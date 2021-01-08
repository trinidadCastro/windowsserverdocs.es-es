---
title: Purgado de datos de utilización
description: Obtenga información sobre cómo eliminar datos de uso de la base de datos de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b144c1896a308eabdd5b8c03c34cc9b43766b699
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039475"
---
# <a name="purge-utilization-data"></a>Purgado de datos de utilización

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo eliminar datos de uso de la base de datos de IPAM.

Para llevar a cabo este procedimiento, debe ser miembro de **los administradores de IPAM**, del grupo de **administradores** del equipo local o de un grupo equivalente.

## <a name="to-purge-the-ipam-database"></a>Para purgar la base de datos de IPAM
1. Abra Administrador del servidor y, a continuación, vaya a la interfaz de cliente de IPAM.
2. Desplácese a una de las siguientes ubicaciones: **bloques de direcciones** IP, inventario de **direcciones IP** o grupos de intervalos de **direcciones IP**.
3. Haga clic en **tareas** y en **purgar datos de uso**. Se abre el cuadro de diálogo **purgar datos de uso** .
4. En **purgar todos los datos de uso en o antes**, haga clic en **seleccionar una fecha**.
5. Elija la fecha en la que desea eliminar todos los registros de la base de datos y antes de esa fecha.
6. Haga clic en **Aceptar**. IPAM elimina todos los registros que haya especificado.
