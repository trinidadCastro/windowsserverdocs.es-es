---
title: Paso 4 comprobación del clúster
description: Este tema forma parte de la guía deploy Remote Access in a Cluster in Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4fa053519116ddadbe1469223001a33384aebe91
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963901"
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



