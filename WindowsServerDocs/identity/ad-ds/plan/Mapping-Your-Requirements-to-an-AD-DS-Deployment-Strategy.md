---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Asignación de requisitos a una estrategia de implementación de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a61e5e6d429acd92e48a353bc829cb620dd66054
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881836"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Asignación de requisitos a una estrategia de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una vez que termine de revisar e identificar el diseño de servicios de dominio de Active Directory (AD DS) y los requisitos de implementación y determinar cuál de ellos están relacionados con la implementación específica, puede asignar esos requisitos para una implementación específica de AD DS estrategia.  
  
Utilice la siguiente tabla para determinar qué estrategia de implementación de AD DS que asigna a la combinación adecuada de diseño de AD DS e implementación de los requisitos de su organización. ("Sí" significa que un requisito específico es necesario para la estrategia de implementación "No" significa que un requisito específico no es necesario para la estrategia de implementación).  
  
Esta tabla solo hace referencia a las tres estrategias de implementación principales de AD DS como se describe en esta guía:  
  
-   [Implementación de AD DS en una nueva organización](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Implementación de AD DS en una organización de Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Implementación de AD DS en una organización de Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Sin embargo, puede crear un híbrido o personalizado estrategia de implementación de AD DS mediante el uso de cualquier combinación de los requisitos de diseño e implementación de AD DS para satisfacer las necesidades de su organización.  
  
|Requisitos de diseño e implementación de AD DS|Implementación de ADDS en una nueva organización|Implementación de ADDS en una organización de Windows Server2003|Implementación de ADDS en una organización de Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Diseñar la estructura lógica para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Sí|Sí|Sí|  
|[Diseñar la topología de sitios para Windows Server 2008 AD DS](Designing-the-Site-Topology.md)|Sí|Sí|Sí|  
|Planear la capacidad del controlador de dominio|Sí|Sí|Sí|  
|[Implementación de un dominio de raíz de bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Sí|No|No|  
|[Implementar dominios regionales de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Sí|Sí|Sí|  
|[Habilitación de características avanzadas para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Sí|Sí, pero todos los controladores de dominio en el entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional del dominio o bosque a Windows Server 2008.|Sí, pero todos los controladores de dominio en el entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional del dominio o bosque a Windows Server 2008.|  
|[Actualización de dominios de Active Directory para Windows Server 2008 y dominios de AD DS de Windows Server 2008 R2](https://technet.microsoft.com/library/cc731188.aspx)|No|Sí|Sí|  
|[Reestructuración de dominios de AD DS entre bosques](https://go.microsoft.com/fwlink/?LinkId=93678)|Sí, si van a migrar un dominio piloto en su entorno de producción, se combinan con otra organización y consolidar las dos infraestructuras de información (TI) de la tecnología o consolidar los dominios de recursos y cuentas que actualizan en contexto de Windows 2000 o entornos de Windows Server 2003.|Sí, si desea combinar con otra organización y consolidar las dos infraestructuras de TI o consolidar los dominios de recursos y cuentas que actualizan en contexto desde entornos de Windows 2000 o Windows Server 2003.|Sí, si desea combinar con otra organización y consolidar las dos infraestructuras de TI o consolidar los dominios de recursos y cuentas que actualizan en contexto desde entornos de Windows 2000 o Windows Server 2003.|  
|[Reestructuración de dominios de AD DS en bosques](https://go.microsoft.com/fwlink/?LinkId=82740))|No|Sí, si necesita reducir el número de dominios, reducir el tráfico de replicación y la cantidad de usuarios requerido y administración de grupos o simplificar la administración de directivas de grupo.|Sí, si necesita reducir el número de dominios, reducir el tráfico de replicación y la cantidad de usuarios requerido y administración de grupos o simplificar la administración de directivas de grupo.|  
  


