---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: "Asignar los requisitos de a una estrategia de implementación de AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b4f625f06fede5b9dc751282cda19b68d7f9e535
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Asignar los requisitos de a una estrategia de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una vez que termines de revisar e identificar el diseño de los servicios de dominio de Active Directory (AD DS) y requisitos de implementación y determinar cuál de ellos están relacionados con la implementación específica, puedes asignar estos requisitos para una estrategia de implementación específica de AD DS.  
  
Usa la tabla siguiente para determinar qué estrategia de implementación de AD DS que asigna a la combinación adecuada de AD DS diseño e implementación requisitos de la organización. ("Sí" significa que un requisito específico es necesario para la estrategia de implementación; "No" significa que un requisito específico no es necesario para tu estrategia de implementación).  
  
En esta tabla hace referencia solamente a las tres estrategias de implementación principales de AD DS, tal como se describe en esta guía:  
  
-   [Implementación de AD DS en una organización nueva](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Implementación de AD DS en una organización de Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Implementación de AD DS en una organización de Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Sin embargo, puedes crear una estrategia de implementación personalizada de AD DS o híbridas mediante cualquier combinación de los requisitos de diseño e implementación de AD DS para satisfacer las necesidades de la organización.  
  
|Requisitos de diseño e implementación de AD DS|Implementación de AD DS en una organización nueva|Implementación de AD DS en una organización de Windows Server 2003|Implementación de AD DS en una organización de Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Diseñar la estructura lógica para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Sí|Sí|Sí|  
|[Diseño de la topología de sitio para Windows Server 2008 AD DS](Designing-the-Site-Topology.md)|Sí|Sí|Sí|  
|Planear la capacidad del controlador de dominio|Sí|Sí|Sí|  
|[Implementación de un dominio de raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Sí|No|No|  
|[Implementación de dominios regionales de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Sí|Sí|Sí|  
|[Habilitar las características avanzadas de AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Sí|Sí, pero todos los controladores de dominio en el entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional del dominio o bosque en Windows Server 2008.|Sí, pero todos los controladores de dominio en el entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional del dominio o bosque en Windows Server 2008.|  
|[Actualización de dominios de Active Directory para Windows Server 2008 y Windows Server 2008 R2 AD DS dominios](https://technet.microsoft.com/library/cc731188.aspx)|No|Sí|Sí|  
|[Reestructuración de dominios de AD DS entre bosques](https://go.microsoft.com/fwlink/?LinkId=93678)|Sí, si vas a migrar un dominio piloto en el entorno de producción, combinar con otra organización y consolidar las dos infraestructuras de tecnología de información o consolidar los dominios de recursos y la cuenta que has actualizado en lugar de entornos Windows 2000 o Windows Server 2003.|Sí, si quieres combinar con otra organización y consolidar las dos infraestructuras de TI o consolidar los dominios de recursos y la cuenta que has actualizado en lugar de entornos Windows 2000 o Windows Server 2003.|Sí, si quieres combinar con otra organización y consolidar las dos infraestructuras de TI o consolidar los dominios de recursos y la cuenta que has actualizado en lugar de entornos Windows 2000 o Windows Server 2003.|  
|[Reestructuración de dominios de AD DS de bosques](https://go.microsoft.com/fwlink/?LinkId=82740))|No|Sí, si es necesario reducir el número de dominios, reducir el tráfico de replicación y la cantidad de administración de grupo y de usuario necesaria o simplificar la administración de directiva de grupo.|Sí, si es necesario reducir el número de dominios, reducir el tráfico de replicación y la cantidad de administración de grupo y de usuario necesaria o simplificar la administración de directiva de grupo.|  
  


