---
title: Paso 3 comprobar la implementación de DirectAccess avanzada
description: Este tema forma parte de la Guía de implementación de un único servidor de DirectAccess con avanzada configuración para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 386d10dda9f61724a012801840e6471e48c0905a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283539"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Paso 3 comprobar la implementación de DirectAccess avanzada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe cómo comprobar que ha configurado correctamente la implementación de DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para comprobar el acceso a recursos internos a través de DirectAccess  
  
1.  Conectar un equipo cliente de DirectAccess a la red corporativa y obtener el objeto de directiva de grupo.  
  
2.  Haga clic en el **conexiones de red** icono en el área de notificación para tener acceso al administrador de medios de DirectAccess.  
  
3.  Haga clic en **conexión de DirectAccess**, y verá que el estado es **conectado localmente**.  
  
4.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.  
  
    Debe poder tener acceso a todos los recursos corporativos.  
  
## <a name="BKMK_Links"></a>Paso anterior  
  
-   [Paso 2: Configuración de servidores de DirectAccess](Step-2-Configuring-DirectAccess-Servers.md)  
  


