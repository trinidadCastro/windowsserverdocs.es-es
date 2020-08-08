---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms:topic: include
ms.openlocfilehash: 43a4149d44bd24a7d8b3d68ed9e3c2b68c99e285
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952408"
---
Con el puerto de Hyper-V, los equipos NIC configurados en hosts de Hyper-V proporcionan direcciones MAC independientes de las máquinas virtuales.  La dirección MAC de las máquinas virtuales o la máquina virtual conectada al conmutador de Hyper-V se puede usar para dividir el tráfico de red entre los miembros del equipo NIC. No se pueden configurar los equipos NIC que se crean dentro de las máquinas virtuales con el modo de equilibrio de carga del puerto de Hyper-V. En su lugar, use el modo de hash de dirección.