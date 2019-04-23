---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 761deb136ebd4ec22dfeebc47b4eeb7650594d89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863716"
---
Con el puerto de Hyper-V, configurado en hosts de Hyper-V de equipos de NIC, asigne a las direcciones MAC de máquinas virtuales independientes.  La dirección MAC de máquinas virtuales o la máquina virtual portada conectados al conmutador de Hyper-V, puede utilizarse para dividir el tráfico de red entre los miembros del equipo NIC. No se puede configurar los equipos de NIC que cree con el modo de equilibrio de carga del puerto de Hyper-V dentro de las máquinas virtuales. En su lugar, use el modo de Hash de dirección. 