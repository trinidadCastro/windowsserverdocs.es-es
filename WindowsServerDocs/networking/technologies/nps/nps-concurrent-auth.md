---
title: Aumentar las autenticaciones simultáneas procesadas por NPS
description: Este tema proporciona instrucciones sobre la configuración de autenticaciones simultáneas del servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd930e34a4adf6c55812385b691df3e3575a4280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818266"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentar las autenticaciones simultáneas procesadas por NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener instrucciones sobre la configuración de autenticaciones simultáneas del servidor de directivas de red.

Si ha instalado el servidor de directivas de red \(NPS\) en un equipo que no sea un dominio de controlador y el NPS recibe un gran número de solicitudes de autenticación por segundo, puede mejorar el rendimiento de NPS si aumenta el número de autenticaciones simultáneas permitidas entre el NPS y el controlador de dominio.

Para ello, debe editar la clave del registro siguiente: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Agregar un nuevo valor llamado **MaxConcurrentApi** y asígnele un valor entre 2 y 5. 

>[!CAUTION]
>Si asigna un valor a **MaxConcurrentApi** que es demasiado alto, el NPS podría suponer una carga excesiva en el controlador de dominio.

Para obtener más información sobre la administración de NPS, consulte [administrar un servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).