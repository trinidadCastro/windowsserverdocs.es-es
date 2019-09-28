---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Asignación de requisitos a una estrategia de implementación de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 412e1ccb13b4d92801faf019b1ed6d01eff8b8d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402541"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Asignación de requisitos a una estrategia de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una vez que haya terminado de revisar e identificar los requisitos de diseño e implementación de Active Directory Domain Services (AD DS) y de determinar cuáles están relacionados con su implementación específica, puede asignar esos requisitos a una implementación de AD DS específica. estrategia.  
  
Use la tabla siguiente para determinar qué estrategia de implementación de AD DS se asigna a la combinación adecuada de requisitos de diseño e implementación de AD DS para su organización. ("Sí" significa que es necesario un requisito específico para su estrategia de implementación; "No" significa que no es necesario un requisito específico para su estrategia de implementación).  
  
Esta tabla solo hace referencia a las tres principales estrategias de implementación de AD DS como se describe en esta guía:  
  
-   [Implementación de AD DS en una nueva organización](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Implementación de AD DS en una organización de Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Implementación de AD DS en una organización de Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Sin embargo, puede crear una estrategia de implementación híbrida o personalizada AD DS mediante el uso de cualquier combinación de los requisitos de diseño e implementación de AD DS para satisfacer las necesidades de su organización.  
  
|Requisitos de diseño e implementación de AD DS|Implementación de ADDS en una nueva organización|Implementación de ADDS en una organización de Windows Server2003|Implementación de ADDS en una organización de Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Diseñar la estructura lógica para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Sí|Sí|Sí|  
|[Diseño de la topología de sitio para Windows Server 2008 AD DS](Designing-the-Site-Topology.md)|Sí|Sí|Sí|  
|Planear la capacidad del controlador de dominio|Sí|Sí|Sí|  
|[Implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Sí|No|No|  
|[Implementación de dominios regionales de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Sí|Sí|Sí|  
|[Habilitación de características avanzadas para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Sí|Sí, pero todos los controladores de dominio del entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional de dominio o bosque en Windows Server 2008.|Sí, pero todos los controladores de dominio del entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional de dominio o bosque en Windows Server 2008.|  
|[Actualización de dominios de Active Directory a dominios de AD DS de Windows Server 2008 y Windows Server 2008 R2](https://technet.microsoft.com/library/cc731188.aspx)|No|Sí|Sí|  
|[Reestructurar dominios de AD DS entre bosques](https://go.microsoft.com/fwlink/?LinkId=93678)|Sí, si desea migrar un dominio piloto en el entorno de producción, combine con otra organización y consolide las dos infraestructuras de tecnologías de la información (TI), o bien consolide los dominios de cuentas y recursos que ha actualizado en su lugar desde Windows entornos 2000 o Windows Server 2003.|Sí, si desea fusionar mediante combinación con otra organización y consolidar las dos infraestructuras de ti o consolidar los dominios de cuentas y recursos que ha actualizado en su lugar desde entornos Windows 2000 o Windows Server 2003.|Sí, si desea fusionar mediante combinación con otra organización y consolidar las dos infraestructuras de ti o consolidar los dominios de cuentas y recursos que ha actualizado en su lugar desde entornos Windows 2000 o Windows Server 2003.|  
|[Reestructurar dominios de AD DS dentro de bosques](https://go.microsoft.com/fwlink/?LinkId=82740))|No|Sí, si necesita reducir el número de dominios, reduzca el tráfico de replicación y la cantidad de administración de usuarios y grupos necesaria, o bien simplifique la administración de directiva de grupo.|Sí, si necesita reducir el número de dominios, reduzca el tráfico de replicación y la cantidad de administración de usuarios y grupos necesaria, o bien simplifique la administración de directiva de grupo.|  
  


