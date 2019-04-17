---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: "Planear la capacidad de Proxy del servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e57f34b173c10e9e753c7f3b8dcd88d7bf6742c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-proxy-capacity"></a>Planear la capacidad de Proxy del servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diseño para los servidores proxy de servidor de federación de capacidad te ayuda a calcular:  
  
-   Los requisitos de hardware adecuados para cada proxy del servidor de federación.  
  
-   El número de servidores de federación y los proxies de servidor de federación colocar en cada organización.  
  
Servidores proxy de federación servidor redirigen tokens de seguridad de un servidor de federación protegido en la red corporativa a los usuarios federados. El propósito de la implementación de un proxy de servidor de federación es permitir que los usuarios externos para conectarse a un servidor de federación. Realmente no lo hace firmar tokens o escribir datos en la base de datos de configuración de AD FS. Por lo tanto, los requisitos de hardware para el proxy de servidor de federación son normalmente inferiores a los requisitos de hardware de un servidor de federación.  
  
Dado que cada solicitud a un proxy de servidor de federación como resultado una solicitud a un servidor de federación o granja de servidores de federación, planificación la capacidad para servidores de federación y los proxies de servidor de federación debe realizarse en paralelo.  
  
Estimación de los complementos sign\ pico por segundo para el proxy de servidor de federación requiere un conocimiento de los patrones de uso de los usuarios federados que iniciará la sesión en a través del proxy del servidor de federación. En muchas de las implementaciones, los usuarios federados que iniciar sesión mediante el proxy de federación de servidor se encuentran en Internet. Los complementos sign\ pico por segundo se pueden calcular echando un vistazo a los patrones de uso de estos usuarios federados en las aplicaciones Web existentes que se protegerán por AD FS.  
  
> [!NOTE]  
> Para entornos de producción, se recomienda un mínimo de dos proxies de servidor de federación de cada instancia de granja de servidor de federación que implementas.  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>Calcular el número de servidores proxy de servidor de federación necesarios para la organización  
Antes de que puede estimar la cantidad de máquinas de proxy de servidor de federación de AD FS necesario, primero debes determinar el número total de los servidores de federación que implementas en la organización. Para obtener más información sobre cómo hacerlo, consulta [planear la capacidad de servidor de federación](Planning-for-Federation-Server-Capacity.md).  
  
Después de decidir el número de servidores de federación multiplicar este número de servidores por el porcentaje de autenticación federados entrante solicitudes que esperan los usuarios externos estarán \ (que se encuentra fuera de la empresa red\). El valor de este cálculo proporciona con el número estimado de proxies de servidor de federación que controlará las solicitudes entrantes de autenticación para los usuarios externos.  
  
Por ejemplo, si el número de servidores de federación recomendada es 3 y esperas que el número total de solicitudes de autenticación que los usuarios externos estarán será aproximadamente 60% del total de solicitudes de autenticación federados, el cálculo equivaldría 1,8 \ (3 X. 60\) que puedes redondear hasta 2.  Por lo tanto, en este caso, tendrías que implementar dos máquinas de proxy de servidor de federación para dar cabida a la carga de las solicitudes de autenticación de usuarios externos para los servidores de tres federación.  
  
En las pruebas realizadas por el equipo de AD FS, el uso de CPU general en cada proxy del servidor de federación resultó para ser mucho menor que el uso de CPU que se ha observado en los servidores de federación para el mismo conjunto.  En una prueba, mientras que un servidor de federación CPU fue que indica que completamente se saturado, la CPU para proporcionar servicios de servidor proxy para ese mismo conjunto de un proxy de servidor de federación observó solo utilización del 20%. Por lo tanto, nuestras pruebas revelaron que la carga de la CPU de un proxy de servidor de federación, que usa las especificaciones de hardware similar como se explicó anteriormente en esta sección, razonablemente podría controlar la carga de procesamiento para los servidores de federación aproximadamente tres.  
  
Sin embargo, para fines de tolerancia a errores, se recomienda un mínimo de dos proxies de servidor de federación de cada granja de servidores de federación que implementas.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
