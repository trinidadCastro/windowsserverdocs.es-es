---
title: Paso 3 comprobar la implementación
description: Este tema forma parte de la Guía de agregar DirectAccess a una implementación de acceso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 639431405635b7b0792b7a73bd9bb524e0827832
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281817"
---
# <a name="step-3-verify-the-deployment"></a>Paso 3 comprobar la implementación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe cómo comprobar que ha configurado correctamente la implementación de DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para comprobar el acceso a recursos internos a través de DirectAccess  
  
1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga la directiva de grupo.  
  
2.  Haga clic en el icono **Conexiones de red** del área de notificación para tener acceso al Administrador de medios de DA.  
  
3.  Haga clic en **Conexión de DirectAccess**y verá que el estado es **Conectado localmente**.  
  
4.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.  
  
    Debe poder tener acceso a todos los recursos corporativos.  
  


