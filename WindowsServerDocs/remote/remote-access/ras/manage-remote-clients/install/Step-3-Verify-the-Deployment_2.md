---
title: Paso 3 comprobación de la implementación
description: Este tema forma parte de la guía administrar los clientes de DirectAccess de forma remota en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e1a8d3208acb0e0ce891e517492916357b9fbfcc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314356"
---
# <a name="step-3-verify-the-deployment"></a>Paso 3 comprobación de la implementación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación para la administración remota de los clientes de DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Para comprobar la implementación correcta  
  
1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga el objeto de directiva de grupo.  
  
2.  En el equipo cliente, haga clic en el icono **conexiones de red** del área de notificación para tener acceso al administrador de medios de DirectAccess.  
  
3.  Haga clic en **conexión de DirectAccess**y verá que el estado es **conectado localmente**.  
  
4.  Quite el equipo de la red corporativa y conéctelo a una red pública.  
  
5.  En un símbolo del sistema, escriba **NLTEST/dsgetdc: [nombre de dominio completo]** . Este comando comprobará que la red corporativa es accesible para el cliente. Si no se puede tener acceso al controlador de dominio, el siguiente mensaje de error mostrará informes de que el dominio no existe: ERROR_NO_SUCH_DOMAIN.  
  


