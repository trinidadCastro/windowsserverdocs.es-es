---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificar los requisitos de diseño de bosque
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bf8d9d164bf07151572785cda906be911f97b53e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835336"
---
# <a name="identifying-forest-design-requirements"></a>Identificar los requisitos de diseño de bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para crear un diseño de bosque para su organización, debe identificar los requisitos empresariales que se debe dar cabida a la estructura de directorios. Esto implica determinar cuánta autonomía de los grupos de necesidades de su organización para administrar sus recursos de red y si no necesita aislar sus recursos de la red de otros grupos de cada grupo.  
  
Los servicios de dominio de Active Directory (AD DS) le permite diseñar una infraestructura de directorio que dé cabida a varios grupos dentro de una organización que tienen requisitos únicos de la administración y para lograr independencia estructural y en funcionamiento entre grupos según sea necesario.  
  
Grupos de la organización podrían tener algunos de los siguientes tipos de requisitos:  
  
-   **Requisitos de la estructura organizativa**. Partes de una organización pueden participar en una infraestructura compartida para ahorrar costos, pero requieren la capacidad de funcionar de forma independiente del resto de la organización. Por ejemplo, un grupo de investigación de una organización grande es posible que deba mantener el control sobre todos sus propios datos de investigación.  
  
-   **Los requisitos operativos**. Una parte de una organización podría colocar restricciones unique en la configuración del servicio de directorio, la disponibilidad o seguridad, o usar las aplicaciones que colocan restricciones únicas en el directorio. Por ejemplo, unidades de negocio individuales dentro de una organización podrían implementar aplicaciones habilitadas para directorio que modifican el esquema de directorio que no se implementan con otras unidades de negocio. Dado que el esquema de directorio se comparte entre todos los dominios del bosque, creación de varios bosques es una solución para este escenario. Otros ejemplos se encuentran en los escenarios y organizaciones siguientes:  
  
    -   Organizaciones militares  
  
    -   Escenarios de hospedaje  
  
    -   Organizaciones que mantienen un directorio que está disponible tanto interna como externamente (por ejemplo, aquellas que sean accesibles públicamente por usuarios de Internet)  
  
-   **Los requisitos legales**. Algunas organizaciones tienen requisitos legales para operar de forma específica, por ejemplo, restringir el acceso a determinada información tal como se especifica en un contrato de negocio. Algunas organizaciones tienen requisitos de seguridad para operar en redes internas aisladas. Si no se cumplen estos requisitos puede dar lugar a pérdida del contrato y acciones legales posiblemente.  
  
Parte de identificar los requisitos de diseño de bosque consiste en identificar el grado al que pueden confiar en grupos de la organización los posibles propietarios del bosque y sus administradores de servicio e identificar los requisitos de aislamiento y autonomía para cada grupo de su organización.  
  
El equipo de diseño debe documentar los requisitos de aislamiento y la autonomía de la administración de servicio y los datos para cada grupo de la organización que pretende usar AD DS. El equipo también debe tener en cuenta las áreas de conectividad limitada que podrían afectar a la implementación de AD DS.  
  
El equipo de diseño debe documentar los requisitos de aislamiento y la autonomía de la administración de servicio y los datos para cada grupo de la organización que pretende usar AD DS. El equipo también debe tener en cuenta las áreas de conectividad limitada que podrían afectar a la implementación de AD DS. Para que una hoja de cálculo que le ayudarán a documentar las regiones que identificó, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip desde [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) y abra "bosque Requisitos de diseño"(DSSLOGI_2.doc).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Administrador de servicios de ámbito de autoridad](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomía frente a Aislamiento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
