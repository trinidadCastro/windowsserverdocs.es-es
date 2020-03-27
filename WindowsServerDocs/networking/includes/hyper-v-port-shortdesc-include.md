---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 47a91c86ac75aedf532289055c94b34899fa01df
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316477"
---
Con el puerto de Hyper-V, los equipos NIC configurados en hosts de Hyper-V proporcionan direcciones MAC independientes de las máquinas virtuales.  La dirección MAC de las máquinas virtuales o la máquina virtual conectada al conmutador de Hyper-V se puede usar para dividir el tráfico de red entre los miembros del equipo NIC. No se pueden configurar los equipos NIC que se crean dentro de las máquinas virtuales con el modo de equilibrio de carga del puerto de Hyper-V. En su lugar, use el modo de hash de dirección. 