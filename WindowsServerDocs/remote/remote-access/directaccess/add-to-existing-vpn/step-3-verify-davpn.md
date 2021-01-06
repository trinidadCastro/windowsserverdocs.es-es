---
title: Paso 3 comprobar la implementación de acceso remoto (VPN)
description: Obtenga información acerca de cómo comprobar que ha configurado correctamente la implementación de DirectAccess.
manager: brianlic
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 75ca3194132b8869d40b40e150eb9ae7fe9a6376
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947031"
---
# <a name="step-3-verify-the-remote-access-vpn-deployment"></a>Paso 3 comprobar la implementación de acceso remoto (VPN)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación de DirectAccess.

### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para comprobar el acceso a recursos internos a través de DirectAccess

1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga la directiva de grupo.

2.  Haga clic en el icono **Conexiones de red** del área de notificación para tener acceso al Administrador de medios de DA.

3.  Haga clic en **Conexión de DirectAccess** y verá que el estado es **Conectado localmente**.

4.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.

    Debe poder tener acceso a todos los recursos corporativos.



