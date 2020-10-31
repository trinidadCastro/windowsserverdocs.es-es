---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Rol de propietario de topología de sitio
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 162256df7b35b1d0d7033036bbfc097187670762
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070137"
---
# <a name="site-topology-owner-role"></a>Rol de propietario de topología de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El administrador que administra la topología del sitio se conoce como propietario de la topología del sitio. El propietario de la topología de sitio entiende las condiciones de la red entre sitios y tiene la autoridad para cambiar la configuración de Active Directory Domain Services (AD DS) para implementar cambios en la topología de sitio. Los cambios en la topología de sitio afectan a los cambios en la topología de replicación. Las responsabilidades del propietario de la topología de sitio incluyen:

-   Control de los cambios en la topología de sitio si cambia la conectividad de red.

-   Obtener y mantener información acerca de las conexiones de red y los enrutadores del grupo de red. El propietario de la topología de sitio debe mantener una lista de direcciones de subred, máscaras de subred y la ubicación a la que pertenece cada una. El propietario de la topología de sitio también debe comprender cualquier problema sobre la velocidad de red y la capacidad que afectan a la topología del sitio para establecer de forma eficaz los costos de los vínculos a sitios.

-   Mover Active Directory objetos de servidor que representan controladores de dominio entre sitios si la dirección IP de un controlador de dominio cambia a una subred diferente en un sitio diferente, o si la propia subred está asignada a un sitio diferente. En cualquier caso, el propietario de la topología de sitio debe trasladar manualmente el objeto de servidor Active Directory del controlador de dominio al nuevo sitio.



