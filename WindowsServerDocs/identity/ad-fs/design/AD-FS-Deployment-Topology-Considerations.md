---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Consideraciones sobre la topología de implementación de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 260d86c0feae0179620ece09e06f12729691b5a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359224"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Consideraciones sobre la topología de implementación de AD FS

En este tema se describen consideraciones importantes para ayudarle a planear y diseñar qué Servicios de federación de Active Directory (AD FS) \(AD FS\) topología de implementación que se va a usar en el entorno de producción. Este tema es un punto de partida para revisar y evaluar las consideraciones que afectan a las características o capacidades que estarán disponibles después de implementar AD FS. Por ejemplo, en función del tipo de base de datos que elija para almacenar la base de datos de configuración de AD FS, determinará en última instancia si puede implementar ciertas Lenguaje de marcado de aserción de seguridad \(SAML\) características que requieren SQL Server.  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinar el tipo de AD FS base de datos de configuración que se va a usar  
AD FS utiliza una base de datos para almacenar la configuración y, en algunos casos, los datos transaccionales relacionados con el Servicio de federación. Puede usar el software de AD FS para seleccionar el\-compilado en Windows Internal Database \(WID\) o Microsoft SQL Server 2005 o una versión más reciente para almacenar los datos en el Servicio de federación.  

Para la mayoría de los objetivos, los dos tipos de bases de datos son relativamente equivalentes. Sin embargo, hay algunas diferencias que debe tener en cuenta antes de empezar a leer más sobre las distintas topologías de implementación que puede usar con AD FS. En la tabla siguiente se describen las diferencias en las características admitidas entre una base de datos WID y una base de datos SQL Server.  

AD FS características  

|Característica|¿Compatible con WID?|¿Compatible con SQL Server?|Más información sobre esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Implementación de una granja de servidores de federación|Sí, con un límite de 30 servidores de Federación para cada granja|Sí. No hay un límite obligatorio para el número de servidores de federación que se pueden implementar en una sola granja|[Determinar la topología de la implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Resolución de artefactos SAML **Nota:** esta característica no es necesaria para los escenarios de Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sí|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedimientos recomendados para planear e implementar AD FS de forma segura](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|Detección de reproducción de tokens de Federación de SAML\/WS\-|No|Sí|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedimientos recomendados para planear e implementar AD FS de forma segura](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

Características de la base de datos  

|Característica|¿Compatible con WID?|¿Compatible con SQL Server?|Más información sobre esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redundancia básica de bases de datos mediante la replicación de extracción, en la que uno o más servidores que hospedan una\-de lectura solo de la solicitud de la base de datos cambian los cambios que se realizan en un servidor de origen que hospeda una copia de lectura\/escritura de la base de datos|Sí|No|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redundancia de bases de datos mediante soluciones de alta disponibilidad\-, como clústeres de conmutación por error o creación de reflejo \(solo en el nivel de base de datos\) **Nota:** todas AD FS topologías de implementación admiten la agrupación en clústeres en el nivel de servicio AD FS.|No|Sí|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Información general sobre soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>Consideraciones sobre SQL Server  
Debe tener en cuenta los siguientes hechos de implementación si selecciona SQL Server como base de datos de configuración para la implementación de AD FS.  

-   **Características de SAML y su efecto en el tamaño y el crecimiento de la base de datos**. Cuando se habilitan las características resolución de artefactos de SAML o detección de reproducción de tokens SAML, AD FS almacena información en la base de datos de configuración de SQL Server para cada token de AD FS que se emite. El crecimiento de la base de datos de SQL Server como resultado de esta actividad no se considera significativo y depende del período de retención de reproducción de tokens configurado. Cada registro de artefacto tiene un tamaño aproximado de 30 kilobytes \(KB\).  

-   **Número de servidores necesarios para la implementación**. Tendrá que agregar al menos un servidor adicional \(al número total de servidores necesarios para implementar la infraestructura de AD FS\) que actuará como host dedicado de la instancia de SQL Server. Si planea usar el clúster de conmutación por error o la creación de reflejo para proporcionar tolerancia a errores y escalabilidad para la base de datos de configuración de SQL Server, se requiere un mínimo de dos servidores SQL Server.  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Cómo podría afectar a los recursos de hardware el tipo de base de datos de configuración que selecciones  
El impacto en los recursos de hardware de un servidor de Federación implementado en una granja de servidores con WID en lugar de un servidor de Federación implementado en una granja de servidores con la SQL Server base de datos no es significativo. Sin embargo, es importante tener en cuenta que cuando se usa WID para la granja, cada servidor de Federación de dicha granja debe almacenar, administrar y mantener los cambios de replicación para su copia local de la base de datos de configuración de AD FS y, al mismo tiempo, seguir proporcionando el operaciones que requiere el Servicio de federación.  

En comparación, los servidores de Federación que se implementan en una granja que utiliza la base de datos SQL Server no contienen necesariamente una instancia local de la base de datos de configuración AD FS. Por tanto, sus demandas de recursos de hardware podrían ser ligeramente inferiores.  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Comprobar que el entorno de producción es compatible con la implementación de AD FS  
Además de los servidores de Federación que va a implementar y según cómo esté configurado el entorno de producción existente, pueden ser necesarios los siguientes servidores adicionales para proporcionar la infraestructura necesaria para admitir la nueva implementación de AD FS:  

-   Controlador de dominio de Active Directory  

-   Entidad de certificación \(CA\)  

-   Servidor web para hospedar metadatos de federación  

-   Equilibrio de carga de red \(NLB\)  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
