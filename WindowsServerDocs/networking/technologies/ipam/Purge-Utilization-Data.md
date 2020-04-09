---
title: Purgado de datos de utilización
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a4454131c6a73626b0668b3ca3ab4eefb3e3334f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860628"
---
# <a name="purge-utilization-data"></a>Purgado de datos de utilización

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo eliminar datos de uso de la base de datos de IPAM.  

Para llevar a cabo este procedimiento, debe ser miembro de **los administradores de IPAM**, del grupo de **administradores** del equipo local o de un grupo equivalente.

## <a name="to-purge-the-ipam-database"></a>Para purgar la base de datos de IPAM  
1. Abra Administrador del servidor y, a continuación, vaya a la interfaz de cliente de IPAM.
2. Desplácese a una de las siguientes ubicaciones: **bloques de direcciones**IP, inventario de **direcciones IP**o grupos de intervalos de **direcciones IP**.  
3. Haga clic en **tareas**y en **purgar datos de uso**. Se abre el cuadro de diálogo **purgar datos de uso** .
4. En **purgar todos los datos de uso en o antes**, haga clic en **seleccionar una fecha**.
5. Elija la fecha en la que desea eliminar todos los registros de la base de datos y antes de esa fecha.
6. Haga clic en **Aceptar**. IPAM elimina todos los registros que haya especificado.
