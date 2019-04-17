---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: "Rol de propietario de la topología de sitio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7c98d313cac28ab07380791a384a87bffacfbda2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="site-topology-owner-role"></a>Rol de propietario de la topología de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El administrador que administra la topología de sitio se conoce como el propietario de la topología de sitio. El propietario de la topología de sitio comprende las condiciones de la red entre sitios y tiene autoridad para cambiar la configuración de servicios de dominio de Active Directory (AD DS) para implementar cambios en la topología de sitio. Cambios en la topología afectan a cambios en la topología de replicación. Responsabilidades del propietario del sitio topología incluyen:  
  
-   Controlar los cambios en la topología si cambia la conectividad de red.  
  
-   Obtener y mantener la información sobre las conexiones de red y enrutadores desde el grupo de red. El propietario de la topología de sitio debe mantener una lista de direcciones de subred, máscaras y la ubicación a la que cada uno pertenece. El propietario de la topología de sitio también debe comprender los problemas relacionados con la velocidad de red y la capacidad que afectan a la topología de sitio para establecer los costos de vínculos a sitios de manera eficaz.  
  
-   Mover los objetos de servidor de Active Directory que representa los controladores de dominio entre sitios si dirección IP del controlador de dominio cambia a una subred diferente en un sitio diferente, o si la propia subred se asigna a un sitio diferente. En cualquier caso, el propietario de la topología de sitio debe mover manualmente el objeto de servidor de Active Directory del controlador de dominio al nuevo sitio.  
  


