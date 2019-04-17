---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: "Modelos de diseño del bosque"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b24eba7173a4878102ac7b7a84f7322425df8a52
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="forest-design-models"></a>Modelos de diseño del bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes aplicar uno de los siguientes modelos de diseño de tres bosque en el entorno de Active Directory:  
  
-   Modelo de bosque organizativa  
  
-   Modelo de bosque de recursos  
  
-   Modelo de bosque de acceso restringido  
  
Es probable que tendrás que usar una combinación de estos modelos para satisfacer las necesidades de los distintos grupos de la organización.  
  
## <a name="organizational-forest-model"></a>Modelo de bosque organizativa  
En el modelo de bosque organizativa, recursos y las cuentas de usuario están contenidos en el bosque y administra de forma independiente. El bosque de la organización puede usarse para proporcionar el servicio autonomía, aislamiento del servicio o datos, si el bosque está configurado para impedir el acceso a las personas fuera del bosque.  
  
Si los usuarios de un bosque de la organización necesitan acceder a recursos en otros bosques (o viceversa), se pueden establecer relaciones de confianza entre un bosque organizativo y los demás bosques. Esto hace posible para los administradores conceder acceso a recursos en el otro bosque. La siguiente ilustración muestra el modelo de bosque organizativa.  
  
![modelos de diseño del bosque](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Cada diseño de Active Directory incluye al menos un bosque de organización.  
  
## <a name="resource-forest-model"></a>Modelo de bosque de recursos  
En el modelo de bosque de recursos, un bosque independiente se usa para administrar recursos. Bosques de recurso no contienen cuentas de usuario aparte de los necesarios para la administración del servicio y los obligados a proporcionar alternativas de acceso a los recursos de ese bosque, si las cuentas de usuario en el bosque organizativa no estén disponibles. Confianzas de bosque se establecen de modo que los usuarios de otros bosques pueden acceder a los recursos contenidos en el bosque de recursos. La siguiente ilustración muestra el modelo de bosque de recursos.  
  
![modelos de diseño del bosque](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Bosques de recursos proporcionan aislar el servicio que se usa para proteger las áreas de la red que necesitan para mantener un estado de alta disponibilidad. Por ejemplo, si tu empresa incluye una instalación de fabricación que debe seguir funcionando cuando hay problemas en el resto de la red, puedes crear un bosque de recursos independiente para el grupo de fabricación.  
  
## <a name="restricted-access-forest-model"></a>Modelo de bosque de acceso restringido  
En el modelo de bosque de acceso restringido, se crea un bosque independiente para incluir cuentas de usuario y los datos que se deben aisladas entre el resto de la organización. Acceso restringido bosques proporcionan aislamiento de datos en situaciones en que las consecuencias de comprometer los datos del proyecto graves. La siguiente ilustración muestra un modelo de bosque de acceso restringido.  
  
![modelos de diseño del bosque](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
No se puede conceder acceso a los datos restringidos a los usuarios de otros bosques porque no se confía existe. En este modelo, los usuarios tienen una cuenta en un bosque organizativa para acceder a recursos generales de la organización y otra cuenta de usuario en el bosque de acceso restringido para acceder a los datos clasificados. Estos usuarios deben tener dos independientes estaciones de trabajo, una conectada a la organización bosque y el otro conectado al bosque de acceso restringido. Esto protege contra la posibilidad de que un administrador de servicios de un bosque puede tener acceso a una estación de trabajo en el bosque restringido.  
  
En casos extremos, el bosque de acceso restringido puede mantenerse en una red física independiente. Las organizaciones que funcionan en proyectos gubernamentales clasificada a veces, mantén bosques de acceso restringido en redes independientes para cumplir los requisitos de seguridad.  
  


