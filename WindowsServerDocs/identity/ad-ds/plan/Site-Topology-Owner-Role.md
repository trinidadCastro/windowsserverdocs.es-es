---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Rol de propietario de topología de sitio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 69443c2fc1af855c7df002e0ac91d43986eff6da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832116"
---
# <a name="site-topology-owner-role"></a>Rol de propietario de topología de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El administrador que administra la topología del sitio se conoce como el propietario de topología del sitio. El propietario de topología del sitio comprende las condiciones de la red entre sitios y tiene autoridad para cambiar la configuración de servicios de dominio de Active Directory (AD DS) para implementar cambios en la topología del sitio. Los cambios realizados en la topología del sitio afecta a los cambios en la topología de replicación. Responsabilidades del propietario de topología de sitio incluyen:  
  
-   Si cambia la conectividad de red que controlan los cambios realizados en la topología del sitio.  
  
-   Obtener y mantener la información sobre las conexiones de red y los enrutadores del grupo de red. El propietario de topología del sitio debe mantener una lista de direcciones de subred, máscaras de subred y la ubicación a la que cada uno de ellos pertenece. El propietario de topología del sitio también debe comprender los problemas sobre la capacidad y la velocidad de red que afectan a la topología del sitio para establecer eficazmente los costos de vínculos a sitios.  
  
-   Mover objetos de servidor de Active Directory que representan los controladores de dominio entre sitios si la dirección IP de un controlador de dominio cambia a una subred diferente en un sitio diferente, o si la propia subred se asigna a un sitio diferente. En cualquier caso, el propietario de topología del sitio debe mover manualmente el objeto de servidor de Active Directory del controlador de dominio para el nuevo sitio.  
  


