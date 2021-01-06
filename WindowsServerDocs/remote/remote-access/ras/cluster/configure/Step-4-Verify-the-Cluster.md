---
title: Paso 4 comprobación del clúster
description: Obtenga información acerca de cómo comprobar que ha configurado correctamente la implementación del clúster de DirectAccess.
manager: brianlic
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 92f51a3b6fa00a06006223abe4f6d28bc40d744f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947781"
---
# <a name="step-4-verify-the-cluster"></a>Paso 4 comprobación del clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación del clúster de DirectAccess.

### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Para comprobar el acceso a los recursos internos a través del clúster

1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga la directiva de grupo.

2.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.

    Debe poder tener acceso a todos los recursos corporativos.

3.  Pruebe la conectividad a través de cada servidor del clúster desactivando o desconectando de la red externa, excepto uno de los servidores de clúster. En el equipo cliente, intente tener acceso a los recursos corporativos. Repita la prueba en un servidor de clúster diferente.

    Debe poder tener acceso a todos los recursos corporativos a través de cada servidor de clúster.



