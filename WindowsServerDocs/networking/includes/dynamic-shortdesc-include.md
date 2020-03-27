---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: f2065acb89af4bed4dc525453bb5a294a4e2c3ef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316514"
---
Con las cargas dinámicas de salida, se distribuyen en función de un hash de los puertos TCP y las direcciones IP. El modo dinámico también vuelve a equilibrar las cargas en tiempo real para que un flujo de salida determinado pueda moverse entre los miembros del equipo. Por otro lado, las cargas de entrada se distribuyen de la misma manera que el puerto de Hyper-V. En pocas palabras, el modo dinámico emplea los mejores aspectos del hash de dirección y el puerto de Hyper-V, y es el modo de equilibrio de carga de mayor rendimiento. 

