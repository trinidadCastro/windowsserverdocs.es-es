---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modelos de diseño de bosque
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aca59a31f75628b311a92bd842d93ee91408dc4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819796"
---
# <a name="forest-design-models"></a>Modelos de diseño de bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede aplicar uno de los siguientes modelos de diseño de tres bosque en su entorno de Active Directory:  
  
-   Modelo de bosque organizativo  
  
-   Modelo de bosque de recursos  
  
-   Modelo de bosque de acceso restringido  
  
Es probable que necesite usar una combinación de estos modelos para satisfacer las necesidades de todos los grupos diferentes de su organización.  
  
## <a name="organizational-forest-model"></a>Modelo de bosque organizativo  
En el modelo de bosque organizativo, recursos y cuentas de usuario están contenidos en el bosque y administran de forma independiente. El bosque organizativo puede utilizarse para proporcionar la autonomía del servicio, el aislamiento del servicio o el aislamiento de datos, si el bosque está configurado para impedir el acceso a cualquier persona fuera del bosque.  
  
Si los usuarios en un bosque de la organización necesitan tener acceso a recursos en otros bosques (o viceversa), se pueden establecer relaciones de confianza entre un bosque de organización y los otros bosques. Esto permite a los administradores conceder acceso a los recursos en el otro bosque. La siguiente ilustración muestra el modelo de bosque organizativo.  
  
![modelos de diseño de bosque](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Cada diseño de Active Directory incluye al menos un bosque organizativo.  
  
## <a name="resource-forest-model"></a>Modelo de bosque de recursos  
En el modelo de bosque de recursos, un bosque independiente se utiliza para administrar los recursos. Bosques de recursos no tienen cuentas de usuario que no eran necesarios para la administración de servicios y los necesarios para proporcionar acceso alternativa a los recursos de ese bosque, si las cuentas de usuario en el bosque de la organización dejen de estar disponibles. Las confianzas de bosque se establecen para que los usuarios de otros bosques pueden acceder a los recursos contenidos en el bosque de recursos. La siguiente ilustración muestra el modelo de bosque de recursos.  
  
![modelos de diseño de bosque](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Bosques de recursos proporcionan aislamiento del servicio que se usa para proteger las áreas de la red que necesita para mantener un estado de alta disponibilidad. Por ejemplo, si su empresa incluye un proceso de fabricación que debe seguir funcionando cuando hay problemas en el resto de la red, puede crear un bosque de recursos independiente para el grupo de fabricación.  
  
## <a name="restricted-access-forest-model"></a>Modelo de bosque de acceso restringido  
En el modelo de bosque de acceso restringido, se crea un bosque independiente para contener las cuentas de usuario y los datos que deben estar aislados del resto de la organización. Acceso restringido bosques proporcionan aislamiento de los datos en situaciones donde las consecuencias de poner en peligro los datos del proyecto son graves. La siguiente ilustración muestra un modelo de bosque de acceso restringido.  
  
![modelos de diseño de bosque](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
Los usuarios de otros bosques no se puede conceder acceso a los datos restringidos porque no existe ninguna confianza. En este modelo, los usuarios tienen una cuenta en un bosque de organización para tener acceso a recursos de la organización generales y una cuenta de usuario independiente en el bosque de acceso restringido para el acceso a los datos clasificados. Estos usuarios deben tener dos estaciones de trabajo independientes, uno conectado a la organización bosque y el otro conectado al bosque acceso restringido. Esto protege contra la posibilidad de que un administrador de servicios de un bosque puede obtener acceso a una estación de trabajo en el bosque restringido.  
  
En casos extremos, es posible que se mantiene el bosque de acceso restringido en una red física independiente. Las organizaciones que trabajan en proyectos del gobierno clasificadas en ocasiones mantienen bosques acceso restringido en redes independientes para satisfacer los requisitos de seguridad.  
  


