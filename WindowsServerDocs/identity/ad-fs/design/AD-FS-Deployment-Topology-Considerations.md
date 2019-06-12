---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Consideraciones sobre la topología de implementación de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cf646dedef85add8607c7940275e3c3fae90a661
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445344"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Consideraciones sobre la topología de implementación de AD FS

En este tema se describe consideraciones importantes para ayudarle a planear y diseñar los servicios de federación de Active Directory \(AD FS\) topología de implementación que utilice en su entorno de producción. En este tema es un punto de partida para revisar y valorar las consideraciones que afectan a las características o capacidades estarán disponibles después de implementar AD FS. Por ejemplo, dependiendo de qué base de datos de tipo que elija para almacenar la base de datos de configuración de AD FS determinarán en última instancia si puedes implementar ciertas lenguaje de marcado de aserción de seguridad \(SAML\) características que requieren SQL Servidor.  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinar qué tipo de base de datos de configuración de AD FS para usar  
AD FS usa una base de datos para almacenar la configuración y, en algunos casos, los datos transaccionales relacionados con el servicio de federación. Puede usar el software AD FS para seleccionar la compilación\-en Windows Internal Database \(WID\) o Microsoft SQL Server 2005 o posterior para almacenar los datos en el servicio de federación.  

Para la mayoría de los objetivos, los dos tipos de bases de datos son relativamente equivalentes. Sin embargo, hay algunas diferencias a tener en cuenta antes de empezar a leer más sobre las diferentes topologías de implementación que puede usar con AD FS. En la tabla siguiente se describe las diferencias en las características admitidas entre una base de datos WID y una base de datos de SQL Server.  

Características de AD FS  

|Característica|¿Compatible con WID?|¿Compatible con SQL Server?|Más información sobre esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Implementación de una granja de servidores de federación|Sí, con un límite de 30 servidores de federación para cada granja de servidores|Sí. No hay un límite obligatorio para el número de servidores de federación que se pueden implementar en una sola granja|[Determinar la topología de la implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Resolución de artefactos SAML **Nota:** Esta característica no es necesaria para escenarios de Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sí|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedimientos recomendados para planear e implementar AD FS de forma segura](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-detección de reproducción de tokens de federación|No|Sí|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedimientos recomendados para planear e implementar AD FS de forma segura](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

Características de la base de datos  

|Característica|¿Compatible con WID?|¿Compatible con SQL Server?|Más información sobre esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redundancia de base de datos básica usando replicación de extracción, donde uno o más servidores que hospeden una lectura\-única copia de los cambios de la solicitud de base de datos que se realizan en un servidor de origen que hospeda una lectura\/escribir la copia de la base de datos|Sí|No|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redundancia de base de datos con alta\-soluciones de disponibilidad, como la conmutación por error de agrupación en clústeres o la creación de reflejo \(en el nivel de base de datos solo\) **Nota:** Todas las topologías de implementación de AD FS son compatibles con clústeres en el nivel de servicio de AD FS.|No|Sí|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Información general sobre soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>Consideraciones sobre SQL Server  
Debe considerar los siguientes hechos de la implementación si seleccionas SQL Server como la base de datos de configuración para la implementación de AD FS.  

-   **Características de SAML y su efecto en el tamaño y el crecimiento de la base de datos**. Cuando se habilitan la resolución de artefactos SAML o las características de detección de reproducción de tokens SAML, AD FS almacena información en la base de datos de configuración de SQL Server para cada token de AD FS que se emitió. El crecimiento de la base de datos de SQL Server como resultado de esta actividad no se considera significativos y depende del período de retención de repetición de token configurada. Cada registro de artefacto tiene un tamaño aproximado de 30 kilobytes \(KB\).  

-   **Número de servidores necesarios para la implementación**. Deberá agregar al menos un servidor adicional \(al número total de servidores necesarios para implementar la infraestructura de AD FS\) que actuará como host dedicado de la instancia de SQL Server. Si tiene previsto usar conmutación por error de agrupación en clústeres o la creación de reflejo para proporcionar tolerancia a errores y la escalabilidad de la base de datos de configuración de SQL Server, se requiere un mínimo de dos servidores SQL Server.  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Cómo podría afectar a los recursos de hardware el tipo de base de datos de configuración que selecciones  
El impacto en los recursos de hardware en un servidor de federación que se implementa en una granja con WID en lugar de un servidor de federación que se implementa en una granja de servidores con la base de datos de SQL Server no es significativo. Sin embargo, es importante tener en cuenta que cuando se usa WID para la granja de servidores, cada servidor de federación de la granja debe almacenar, administrar y mantener los cambios de replicación para su copia local de la base de datos de configuración de AD FS mientras sigue proporcionando el valor normal operaciones que requiere el servicio de federación.  

En comparación, los servidores de federación que se implementan en una granja que utiliza la base de datos de SQL Server no contienen necesariamente una instancia local de la base de datos de configuración de AD FS. Por tanto, sus demandas de recursos de hardware podrían ser ligeramente inferiores.  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Comprobar que el entorno de producción es compatible con la implementación de AD FS  
Además de los servidores de federación que va a implementar y dependiendo de cómo se configura el entorno de producción existente, los siguientes servidores adicionales es posible que deba proporcionar la infraestructura necesaria para admitir la nueva implementación de AD FS:  

-   Controlador de dominio de Active Directory  

-   Entidad de certificación \(CA\)  

-   Servidor web para hospedar metadatos de federación  

-   Equilibrio de carga de red \(NLB\)  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
