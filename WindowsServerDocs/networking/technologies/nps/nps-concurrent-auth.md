---
title: Aumentar las autenticaciones simultáneas procesadas por NPS
description: En este tema se proporcionan instrucciones sobre cómo configurar las autenticaciones simultáneas del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 5036ee3299aec9e57c60b960aa9e25fb53d2f18e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948671"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentar las autenticaciones simultáneas procesadas por NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener instrucciones sobre cómo configurar las autenticaciones simultáneas del servidor de directivas de redes.

Si instaló NPS del servidor \( de directivas \) de redes en un equipo que no sea un controlador de dominio y el NPS recibe un gran número de solicitudes de autenticación por segundo, puede mejorar el rendimiento de NPS aumentando el número de autenticaciones simultáneas permitidas entre el NPS y el controlador de dominio.

Para ello, debe editar la siguiente clave del registro:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Agregue un nuevo valor denominado **MaxConcurrentApi** y asígnele un valor comprendido entre 2 y 5.

>[!CAUTION]
>Si asigna un valor a **MaxConcurrentApi** que es demasiado alto, el NPS podría realizar una carga excesiva en el controlador de dominio.

Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).