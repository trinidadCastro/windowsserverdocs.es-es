---
title: Aumentar las autenticaciones simultáneas procesadas por NPS
description: Obtenga información sobre cómo mejorar el rendimiento de NPS aumentando el número de autenticaciones simultáneas permitidas entre el NPS y el controlador de dominio.
manager: brianlic
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: fd48dc876cd2b8a24199ffc4fc95b8fa5a6d0d72
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113381"
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