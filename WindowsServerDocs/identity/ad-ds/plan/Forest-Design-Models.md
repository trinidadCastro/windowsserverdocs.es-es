---
description: 'Más información sobre: modelos de diseño de bosque'
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modelos de diseño de bosque
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 40701cbea94da12aea169a4d53b51024c8d31bfe
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049693"
---
# <a name="forest-design-models"></a>Modelos de diseño de bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede aplicar uno de los tres modelos de diseño de bosque siguientes en el entorno de Active Directory:

-   Modelo de bosque de la organización

-   Modelo de bosque de recursos

-   Modelo de bosque de acceso restringido

Es probable que deba usar una combinación de estos modelos para satisfacer las necesidades de todos los distintos grupos de la organización.

## <a name="organizational-forest-model"></a>Modelo de bosque de la organización
En el modelo de bosque de la organización, las cuentas de usuario y los recursos se encuentran en el bosque y se administran de forma independiente. El bosque de la organización se puede usar para proporcionar autonomía de servicio, aislamiento de servicio o aislamiento de datos, si el bosque está configurado para impedir el acceso a cualquier persona fuera del bosque.

Si los usuarios de un bosque de la organización necesitan tener acceso a los recursos de otros bosques (o a la inversa), se pueden establecer relaciones de confianza entre un bosque de la organización y los otros bosques. Esto permite a los administradores conceder acceso a los recursos del otro bosque. En la ilustración siguiente se muestra el modelo de bosque de la organización.

![modelos de diseño de bosque](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)

Cada diseño de Active Directory incluye al menos un bosque de la organización.

## <a name="resource-forest-model"></a>Modelo de bosque de recursos
En el modelo de bosque de recursos, se usa un bosque independiente para administrar los recursos. Los bosques de recursos no contienen cuentas de usuario distintas de las necesarias para la administración del servicio y las necesarias para proporcionar acceso alternativo a los recursos de ese bosque, si las cuentas de usuario del bosque de la organización dejan de estar disponibles. Las confianzas de bosque se establecen para que los usuarios de otros bosques puedan tener acceso a los recursos contenidos en el bosque de recursos. En la ilustración siguiente se muestra el modelo de bosque de recursos.

![modelos de diseño de bosque](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)

Los bosques de recursos proporcionan aislamiento de servicio que se usa para proteger las áreas de la red que necesitan mantener un estado de alta disponibilidad. Por ejemplo, si su empresa incluye una instalación de fabricación que debe seguir funcionando cuando haya problemas en el resto de la red, puede crear un bosque de recursos independiente para el grupo de fabricación.

## <a name="restricted-access-forest-model"></a>Modelo de bosque de acceso restringido
En el modelo de bosque de acceso restringido, se crea un bosque independiente que contiene las cuentas de usuario y los datos que deben aislarse del resto de la organización. Los bosques de acceso restringido proporcionan aislamiento de datos en situaciones en las que las consecuencias de poner en peligro los datos del proyecto son graves. En la ilustración siguiente se muestra un modelo de bosque de acceso restringido.

![modelos de diseño de bosque](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)

A los usuarios de otros bosques no se les puede conceder acceso a los datos restringidos porque no existe confianza. En este modelo, los usuarios tienen una cuenta en un bosque de la organización para tener acceso a los recursos generales de la organización y una cuenta de usuario independiente en el bosque de acceso restringido para el acceso a los datos clasificados. Estos usuarios deben tener dos estaciones de trabajo independientes, una conectada al bosque de la organización y la otra conectada al bosque de acceso restringido. Esto protege frente a la posibilidad de que un administrador de servicios de un bosque pueda obtener acceso a una estación de trabajo en el bosque restringido.

En casos extremos, el bosque de acceso restringido podría mantenerse en una red física independiente. Las organizaciones que trabajan en proyectos gubernamentales clasificados a veces mantienen bosques de acceso restringido en redes independientes para cumplir los requisitos de seguridad.



