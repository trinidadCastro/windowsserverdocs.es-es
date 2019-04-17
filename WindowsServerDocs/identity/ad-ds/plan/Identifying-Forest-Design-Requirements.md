---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: "Identificación de los requisitos de diseño del bosque"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 91d551e5d7b6daa6b1476570dad57e85d3bc163f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-forest-design-requirements"></a>Identificación de los requisitos de diseño del bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para crear un diseño de bosque de la organización, debes identificar los requisitos empresariales que deba acomodar la estructura de directorios. Esto implica determinar cuánto autonomía los grupos en la organización necesitan para administrar sus recursos de red y o no debe aislar sus recursos en la red de otros grupos de cada grupo.  
  
Los servicios de dominio de Active Directory (AD DS) te permite diseñar una infraestructura de directorio que aloja a varios grupos dentro de una organización que tienen requisitos únicos de la administración y para conseguir la independencia operativa y estructuras entre grupos según sea necesario.  
  
Grupos de la organización pueden tener parte de los siguientes tipos de requisitos:  
  
-   **Requisitos de la estructura organizativa**. Partes de una organización pueden participar en una infraestructura de recursos compartido para ahorrar costos, pero requieren la capacidad de funcionan de forma independiente con el resto de la organización. Por ejemplo, un grupo de investigación de una organización grande posible que debas mantener el control de todos sus propios datos de investigación.  
  
-   **Los requisitos operativos**. Una parte de una organización podría colocar únicas restricciones en la configuración del servicio de directorio, la disponibilidad o la seguridad o utilizar aplicaciones que establecer restricciones únicas en el directorio. Por ejemplo, las empresas individuales dentro de una organización pueden implementar aplicaciones habilitadas para directorio que modifican el esquema de directorio que no están implementadas por otras empresas. Dado que el esquema de directorio se comparte entre todos los dominios del bosque, crear varios bosques es una única solución para este escenario. Otros ejemplos se encuentran en las siguientes organizaciones y los escenarios:  
  
    -   Organizaciones militares  
  
    -   Escenarios de hospedaje  
  
    -   Organizaciones que mantienen un directorio que está disponible tanto de forma interna y externa (por ejemplo, aquellas que son accesibles públicamente por los usuarios de Internet)  
  
-   **Los requisitos legales**. Algunas organizaciones tienen requisitos legales para operar de forma específica, por ejemplo, restringir el acceso a determinada información tal como se especifica en un contrato de empresas. Algunas organizaciones tienen requisitos de seguridad para operar en redes internas aisladas. Si no se cumple estos requisitos puede provocar la pérdida del contrato y posiblemente legal acción.  
  
Parte de identificar los requisitos de diseño de bosque implica identificar el grado al que puedan confiar en grupos de la organización los posibles propietarios de bosque y sus administradores de servicio e identificar los requisitos de aislamiento y autonomía para cada grupo de la organización.  
  
El equipo de diseño debe documentar los requisitos de aislamiento y autonomía para la administración del servicio y los datos para cada grupo de la organización que quiere usar AD DS. El equipo también debe tener en cuenta las áreas de conectividad limitada que podrían afectar a la implementación de AD DS.  
  
El equipo de diseño debe documentar los requisitos de aislamiento y autonomía para la administración del servicio y los datos para cada grupo de la organización que quiere usar AD DS. El equipo también debe tener en cuenta las áreas de conectividad limitada que podrían afectar a la implementación de AD DS. Para que una hoja de cálculo que le ayudarán a documentar las regiones que has identificado, descarga Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Ámbito del Administrador de servicios de autoridad](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomía frente aislamiento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
  


