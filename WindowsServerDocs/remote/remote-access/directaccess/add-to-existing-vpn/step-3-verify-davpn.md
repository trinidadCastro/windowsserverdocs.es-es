---
title: Paso 3 comprobar la implementación de acceso remoto (VPN)
description: Este tema forma parte de la guía agregar DirectAccess a una implementación de acceso remoto (VPN) existente para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9da14f076f177e7e819c1529f9de647b5dde0500
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969222"
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



