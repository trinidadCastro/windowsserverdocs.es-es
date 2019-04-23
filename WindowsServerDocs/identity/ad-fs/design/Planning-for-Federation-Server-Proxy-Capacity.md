---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: Planear la capacidad de los servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e57f34b173c10e9e753c7f3b8dcd88d7bf6742c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888906"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planear la capacidad de los servidores proxy de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Planear la capacidad de los servidores proxy de federación ayuda a calcular:  
  
-   Los requisitos de hardware adecuado para cada servidor proxy de federación.  
  
-   El número de servidores de federación y servidores proxy de federación va a colocar en cada organización.  
  
Servidores proxy de federación redirigen los tokens de seguridad de un servidor de federación protegido en la red corporativa para usuarios federados. Es el fin de implementar a un servidor proxy de federación permitir que los usuarios externos para conectarse a un servidor de federación. Lo hace realmente no firmar los tokens o escribir datos en la base de datos de configuración de AD FS. Por lo tanto, los requisitos de hardware para el servidor proxy de federación son normalmente menores que los requisitos de hardware para un servidor de federación.  
  
Dado que cada solicitud a un servidor proxy de federación da como resultado una solicitud a un servidor de federación o granja de servidores de federación, planear la capacidad de los servidores de federación y servidores proxy de federación debe realizarse en paralelo.  
  
Estimar el inicio de sesión máxima\-inicios por segundo para el servidor proxy de federación requiere una comprensión de los patrones de uso de los usuarios federados que vayan a iniciar sesión a través del proxy de servidor de federación. En muchas implementaciones, los usuarios federados que inician sesión con el servidor proxy de federación se encuentran en Internet. Puede calcular el inicio de sesión máxima\-federada de inicios por segundo examinando los patrones de uso de estos usuarios en las aplicaciones Web existentes que estarán protegidos por AD FS.  
  
> [!NOTE]  
> Para las implementaciones de producción, se recomienda un mínimo de dos servidores proxy de federación para cada instancia de granja de servidor de federación que implemente.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Calcular el número de servidores proxy de federación necesario para su organización  
Antes de que se puede calcular el número de máquinas de proxy de servidor de federación de AD FS requiere, primero deberá determinar el número total de servidores de federación que va a implementar en su organización. Para obtener más información acerca de cómo hacerlo, consulte [planear la capacidad del servidor de federación](Planning-for-Federation-Server-Capacity.md).  
  
Una vez que haya decidido el número de servidores de federación, multiplicar este número de servidores por el porcentaje de la autenticación federada entrante de solicitudes que esperan de los usuarios externos se realizarán \(ubicados fuera de la red corporativa\). El valor de este cálculo le proporcionará el número estimado de los servidores proxy de federación que se va a controlar las solicitudes de autenticación entrantes para los usuarios externos.  
  
Por ejemplo, si el número de servidores de federación recomendado es 3, y espera que el número total de solicitudes de autenticación que se realizarán de los usuarios externos será aproximadamente el 60% del número total de solicitudes de autenticación federada, su cálculo equivaldría 1.8 \(3 X.60\) que se puede redondear hasta 2.  Por lo tanto, en este caso, deberá implementar dos máquinas de proxy de servidor de federación para acomodar la carga de solicitudes de autenticación de usuario externo para los servidores de federación de tres.  
  
En las pruebas realizadas por el equipo de productos de AD FS, el uso de CPU total en cada servidor proxy de federación resultó para ser significativamente menor que el uso de CPU que se ha observado en los servidores de federación para la misma granja.  En una prueba, mientras que un servidor de federación CPU fue que indica que completamente era saturado, la CPU para un servidor proxy de federación que proporciona servicios de proxy para esa misma granja de servidores se ha observado en sólo 20% de utilización. Por lo tanto, en nuestras pruebas han detectado que la carga de la CPU del servidor de un proxy de federación, que usa las especificaciones de hardware similar como se explicó anteriormente en esta sección, razonablemente podría controlar la carga de procesamiento para aproximadamente tres servidores de federación.  
  
Sin embargo, para fines de tolerancia a errores, se recomienda un mínimo de dos servidores proxy de federación para cada granja de servidores de federación que implementa.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
