---
title: Paso 3 comprobar la implementación de DirectAccess avanzada
description: Obtenga información acerca de cómo comprobar que ha configurado correctamente la implementación de DirectAccess avanzada.
manager: brianlic
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 38e93e9c40b1e87557bd077f7b9d2129a2e9bae4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949821"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Paso 3 comprobar la implementación de DirectAccess avanzada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación de DirectAccess.

### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para comprobar el acceso a recursos internos a través de DirectAccess

1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga el objeto de directiva de grupo.

2.  Haga clic en el icono **conexiones de red** en el área de notificación para tener acceso al administrador de medios de DirectAccess.

3.  Haga clic en **conexión de DirectAccess** y verá que el estado es **conectado localmente**.

4.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.

    Debe poder tener acceso a todos los recursos corporativos.

## <a name="previous-step"></a><a name="BKMK_Links"></a>Paso anterior

-   [Paso 2: configuración de los servidores de DirectAccess](./da-adv-configure-s2-servers.md)