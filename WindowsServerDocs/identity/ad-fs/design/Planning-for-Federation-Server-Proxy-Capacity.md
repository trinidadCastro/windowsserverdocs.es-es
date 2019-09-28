---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Planear la capacidad de los servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: eedb0f2ae4b6f600eb578c5db857cc1d79bccbd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407988"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planear la capacidad de los servidores proxy de federación

Planear la capacidad de los servidores proxy de Federación le ayuda a calcular:  
  
-   Los requisitos de hardware adecuados para cada servidor proxy de Federación.  
  
-   El número de servidores de Federación y los proxies de servidor de Federación que se colocarán en cada organización.  
  
Los proxies de servidor de Federación redirigen los tokens de seguridad de un servidor de Federación protegido de la red corporativa a los usuarios federados. El propósito de implementar un servidor proxy de Federación es permitir que los usuarios externos se conecten a un servidor de Federación. No firma realmente los tokens ni escribe en los datos de la base de datos de configuración de AD FS. Por lo tanto, los requisitos de hardware para el servidor proxy de Federación suelen ser menores que los requisitos de hardware para un servidor de Federación.  
  
Dado que cada solicitud a un servidor proxy de Federación produce una solicitud a un servidor de Federación o una granja de servidores de Federación, el planeamiento de la capacidad de los servidores de Federación y los proxies de servidor de Federación debe realizarse en paralelo.  
  
La estimación del signo de pico @ no__t-0ins por segundo para el servidor proxy de Federación requiere la comprensión de los patrones de uso de los usuarios federados que iniciarán sesión a través del servidor proxy de Federación. En muchas implementaciones, los usuarios federados que inician sesión con el servidor proxy de Federación se encuentran en Internet. Puede calcular el signo de pico @ no__t-0ins por segundo examinando los patrones de uso de estos usuarios federados en las aplicaciones Web existentes que se protegerán con AD FS.  
  
> [!NOTE]  
> En el caso de las implementaciones de producción, se recomienda un mínimo de dos servidores proxy de Federación para cada instancia de granja de servidores de Federación que implemente.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Calcular el número de servidores proxy de Federación necesarios para la organización  
Antes de que pueda calcular el número de máquinas de servidor proxy de Federación AD FS necesarias, primero deberá determinar el número total de servidores de Federación que va a implementar en su organización. Para obtener más información acerca de cómo hacerlo, consulte [planear la capacidad del servidor de Federación](Planning-for-Federation-Server-Capacity.md).  
  
Una vez que haya decidido el número de servidores de Federación, multiplique este número de servidores por el porcentaje de solicitudes de autenticación federadas entrantes que espera que se realicen desde usuarios externos \(located fuera de la red corporativa @ no__t-1. El valor de este cálculo le proporcionará el número estimado de servidores proxy de Federación que controlarán las solicitudes de autenticación entrantes para los usuarios externos.  
  
Por ejemplo, si el número de servidores de Federación recomendados es 3 y prevé que el número total de solicitudes de autenticación que se realizarán desde usuarios externos será aproximadamente el 60% del número total de solicitudes de autenticación federada, su el cálculo sería igual a 1,8 \(3 X .60 @ no__t-1, que puede redondear hasta 2.  Por lo tanto, en este caso, tendría que implementar dos máquinas proxy de servidor de Federación para dar cabida a la carga de solicitudes de autenticación de usuarios externos para los tres servidores de Federación.  
  
En las pruebas realizadas por el equipo del producto AD FS, el uso total de la CPU en cada proxy del servidor de federación era mucho menor que el uso de CPU que se observó en los servidores de Federación de la misma granja.  En una prueba, mientras que una CPU del servidor de Federación estaba indicando que estaba completamente saturada, la CPU de un servidor proxy de Federación que proporciona servicios proxy para esa misma granja se observó solo en un 20% de uso. Por lo tanto, las pruebas revelaron que la carga de la CPU de un servidor proxy de Federación, que usa especificaciones de hardware similares, como se explicó anteriormente en esta sección, podría controlar razonablemente la carga de procesamiento de aproximadamente tres servidores de Federación.  
  
Sin embargo, para los fines de tolerancia a errores, se recomienda un mínimo de dos servidores proxy de Federación para cada granja de servidores de Federación que implemente.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
