---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 01f231ca730a19ac0e7e868bcb7180377830afe1
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935081"
---
Con el puerto de Hyper-V, los equipos NIC configurados en hosts de Hyper-V proporcionan direcciones MAC independientes de las máquinas virtuales.  La dirección MAC de las máquinas virtuales o la máquina virtual conectada al conmutador de Hyper-V se puede usar para dividir el tráfico de red entre los miembros del equipo NIC. No se pueden configurar los equipos NIC que se crean dentro de las máquinas virtuales con el modo de equilibrio de carga del puerto de Hyper-V. En su lugar, use el modo de hash de dirección. 