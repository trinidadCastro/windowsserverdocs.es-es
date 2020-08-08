---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Asignación de requisitos a una estrategia de implementación de AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d4ddc2d88e4849ac35eaf21f092f5cc0ed85ec33
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971002"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Asignación de requisitos a una estrategia de implementación de AD DS

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una vez que haya terminado de revisar e identificar los requisitos de diseño e implementación de Active Directory Domain Services (AD DS) y de determinar cuáles están relacionados con su implementación específica, puede asignar esos requisitos a una estrategia de implementación de AD DS específica.

Use la tabla siguiente para determinar qué estrategia de implementación de AD DS se asigna a la combinación adecuada de requisitos de diseño e implementación de AD DS para su organización. ("Sí" significa que es necesario un requisito específico para su estrategia de implementación; "No" significa que no es necesario un requisito específico para su estrategia de implementación).

Esta tabla solo hace referencia a las tres principales estrategias de implementación de AD DS como se describe en esta guía:

-   [Implementación de AD DS en una nueva organización](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)

-   [Implementación de AD DS en una organización de Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)

-   [Implementación de AD DS en una organización de Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)

Sin embargo, puede crear una estrategia de implementación híbrida o personalizada AD DS mediante el uso de cualquier combinación de los requisitos de diseño e implementación de AD DS para satisfacer las necesidades de su organización.

| Requisitos de diseño e implementación de AD DS | Implementación de AD DS en una nueva organización | Implementación de AD DS en una organización de Windows Server 2003 | Implementación de AD DS en una organización de Windows 2000 |
| ---------------------------------------- | ------------------------------------- | ----------------------------------------------------- |----------------------------------------------- |
| [Diseñar la estructura lógica para Windows Server 2008 AD DS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc770806(v=ws.10)) | Sí | Sí | Sí |
| [Diseño de la topología de sitio para Windows Server 2008 AD DS](Designing-the-Site-Topology.md) | Sí | Sí | Sí |
| Planear la capacidad del controlador de dominio | Sí | Sí | Sí |
| [Implementar un dominio raíz del bosque de Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)) | Sí | No | No |
| [Implementación de dominios regionales de Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755118(v=ws.10)) | Sí | Sí | Sí |
| [Habilitación de características avanzadas para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md) | Sí |Sí, pero todos los controladores de dominio del entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional de dominio o bosque en Windows Server 2008. | Sí, pero todos los controladores de dominio del entorno deben ejecutar Windows Server 2008 antes de establecer el nivel funcional de dominio o bosque en Windows Server 2008. |
| [Actualización de dominios de Active Directory a dominios de AD DS de Windows Server 2008 y Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10)) | No | Sí | Sí |
| [Guía de ADMT: migración y reestructuración de dominios de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)) | Sí, si desea migrar un dominio piloto en el entorno de producción, combine con otra organización y consolide las dos infraestructuras de tecnologías de la información (TI), o bien consolide los dominios de cuentas y recursos que ha actualizado en su lugar desde entornos Windows 2000 o Windows Server 2003. | Sí, si desea fusionar mediante combinación con otra organización y consolidar las dos infraestructuras de ti o consolidar los dominios de cuentas y recursos que ha actualizado en su lugar desde entornos Windows 2000 o Windows Server 2003. | Sí, si desea fusionar mediante combinación con otra organización y consolidar las dos infraestructuras de ti o consolidar los dominios de cuentas y recursos que ha actualizado en su lugar desde entornos Windows 2000 o Windows Server 2003. |
| [Guía de ADMT: migración y reestructuración de dominios de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)) | No | Sí, si necesita reducir el número de dominios, reduzca el tráfico de replicación y la cantidad de administración de usuarios y grupos necesaria, o bien simplifique la administración de directiva de grupo. | Sí, si necesita reducir el número de dominios, reduzca el tráfico de replicación y la cantidad de administración de usuarios y grupos necesaria, o bien simplifique la administración de directiva de grupo. |
