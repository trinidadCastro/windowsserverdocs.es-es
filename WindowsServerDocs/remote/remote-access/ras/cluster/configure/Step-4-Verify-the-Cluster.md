---
title: Paso 4 comprobar el clúster
description: En este tema forma parte de la Guía de implementación de acceso remoto en un clúster en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b449197265befb7caf7d8d3a5b56accf5c52aef9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821846"
---
# <a name="step-4-verify-the-cluster"></a>Paso 4 comprobar el clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe cómo comprobar que ha configurado correctamente la implementación de clúster DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Para comprobar el acceso a recursos internos a través del clúster  
  
1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga la directiva de grupo.  
  
2.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.  
  
    Debe poder tener acceso a todos los recursos corporativos.  
  
3.  Probar la conectividad a través de cada servidor en el clúster mediante la desactivación o desconectarse de la red externa, todos menos uno de los servidores del clúster. En el equipo cliente, intente acceder a recursos corporativos. Repita la prueba en un servidor distinto del clúster.  
  
    Debe poder tener acceso a todos los recursos corporativos a través de cada servidor del clúster.  
  


