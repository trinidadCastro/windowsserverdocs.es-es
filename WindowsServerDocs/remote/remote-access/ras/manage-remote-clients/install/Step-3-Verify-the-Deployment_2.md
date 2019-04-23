---
title: Paso 3 comprobar la implementación
description: En este tema forma parte de la Guía de los clientes de DirectAccess de administrar remotamente en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 89b9e53b7321ebeddda50a448829ad73395eeed0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835616"
---
# <a name="step-3-verify-the-deployment"></a>Paso 3 comprobar la implementación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe cómo comprobar que ha configurado correctamente la implementación para la administración remota de los clientes de DirectAccess.  
  
### <a name="to-verify-proper-deployment"></a>Para comprobar la implementación apropiada  
  
1.  Conectar un equipo cliente de DirectAccess a la red corporativa y obtener el objeto de directiva de grupo.  
  
2.  En el equipo cliente, haga clic en el **conexiones de red** icono en el área de notificación para tener acceso al administrador de medios de DirectAccess.  
  
3.  Haga clic en **conexión de DirectAccess**, y verá que el estado es **conectado localmente**.  
  
4.  Quitar el equipo de la red corporativa y conectarlo a una red pública.  
  
5.  En un símbolo del sistema, escriba **nltest/dsgetdc: [nombre de dominio completo]**. Este comando comprobará que la red corporativa es accesible al cliente. Si el controlador de dominio no está accesible, mostrará el siguiente mensaje de error reporting que no existe el dominio: ERROR_NO_SUCH_DOMAIN.  
  


