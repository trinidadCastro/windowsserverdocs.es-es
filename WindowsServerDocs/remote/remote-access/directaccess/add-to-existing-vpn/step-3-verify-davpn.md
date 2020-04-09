---
title: Paso 3 comprobación de la implementación
description: Este tema forma parte de la guía agregar DirectAccess a una implementación de acceso remoto (VPN) existente para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b187d017a3cf2865a92d95a8ae93bec11ff457f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859538"
---
# <a name="step-3-verify-the-deployment"></a>Paso 3 comprobación de la implementación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación de DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para comprobar el acceso a recursos internos a través de DirectAccess  
  
1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga la directiva de grupo.  
  
2.  Haga clic en el icono **Conexiones de red** del área de notificación para tener acceso al Administrador de medios de DA.  
  
3.  Haga clic en **Conexión de DirectAccess**y verá que el estado es **Conectado localmente**.  
  
4.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.  
  
    Debe poder tener acceso a todos los recursos corporativos.  
  


