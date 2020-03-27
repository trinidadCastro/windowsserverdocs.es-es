---
title: Paso 3 comprobar la implementación de DirectAccess avanzada
description: Este tema forma parte de la guía implementar un único servidor de DirectAccess con configuración avanzada para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6d8f9b5ecbc322de12cb00179a4ebb044fbd2baf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309093"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Paso 3 comprobar la implementación de DirectAccess avanzada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación de DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para comprobar el acceso a recursos internos a través de DirectAccess  
  
1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga el objeto de directiva de grupo.  
  
2.  Haga clic en el icono **conexiones de red** en el área de notificación para tener acceso al administrador de medios de DirectAccess.  
  
3.  Haga clic en **conexión de DirectAccess**y verá que el estado es **conectado localmente**.  
  
4.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.  
  
    Debe poder tener acceso a todos los recursos corporativos.  
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Paso anterior  
  
-   [Paso 2: configuración de los servidores de DirectAccess](Step-2-Configuring-DirectAccess-Servers.md)  
  


