---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms:topic: include
ms.openlocfilehash: f07840220dbbe955a47879b3090ddd340c8c514f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952409"
---
Con las cargas dinámicas de salida, se distribuyen en función de un hash de los puertos TCP y las direcciones IP. El modo dinámico también vuelve a equilibrar las cargas en tiempo real para que un flujo de salida determinado pueda moverse entre los miembros del equipo. Por otro lado, las cargas de entrada se distribuyen de la misma manera que el puerto de Hyper-V. En pocas palabras, el modo dinámico emplea los mejores aspectos del hash de dirección y el puerto de Hyper-V, y es el modo de equilibrio de carga de mayor rendimiento.

