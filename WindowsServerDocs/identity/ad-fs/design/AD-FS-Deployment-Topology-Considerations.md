---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: "Consideraciones de la topología de implementación de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eee14ee7bb50e1a82f35caf9fbacda0b86d3a1ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-deployment-topology-considerations"></a>Consideraciones de la topología de implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describen consideraciones importantes para ayudarte a planear y diseñar la topología de implementación de los servicios de federación de Active Directory \(AD FS\) para usar en el entorno de producción. Este tema es un punto de partida para revisar y evaluar las consideraciones que afectan a las características o las funcionalidades estarán disponibles para después de implementar AD FS. Por ejemplo, en función de la base de datos que decides almacenar la base de datos de configuración de AD FS de tipo en última instancia determinará si se pueden implementar determinadas características del lenguaje de marcado de seguridad aserción \(SAML\) que requieren SQL Server.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinar qué tipo de base de datos de configuración de AD FS usar  
AD FS utiliza una base de datos para almacenar la configuración y, en algunos casos, relacionados con los servicios de federación de datos transaccionales. Puedes usar el software de AD FS que seleccione el built\ en Windows Internal Database \(WID\) o Microsoft SQL Server 2005 o más reciente para almacenar los datos en el servicio de federación.  
  
Para la mayoría de los casos, los tipos de base de dos datos son relativamente equivalentes. Sin embargo, hay algunas diferencias que debes tener en cuenta antes de comenzar a leer más acerca de las diversas topologías de implementación que se pueden usar con AD FS. La siguiente tabla describe las diferencias en las características compatibles entre una base de datos WID y una base de datos de SQL Server.  
  
Características de AD FS  
  
|Característica|¿Admite WID?|¿Compatible con SQL Server?|Para obtener más información acerca de esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Implementación de granja de servidor de federación|Sí, con un límite de cinco servidores de federación para cada granja|Sí. No tiene ningún límite para el número de servidores de federación que se pueden implementar en un solo conjunto aplicada|[Determinar la topología de implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Resolución de artefacto SAML **Nota:** esta característica no es necesaria para escenarios de Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sí|[La función de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Los procedimientos recomendados para el diseño seguro y la implementación de AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|Detección de reproducción de token WS\/SAML\-federación|No|Sí|[La función de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Los procedimientos recomendados para el diseño seguro y la implementación de AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
Características de la base de datos  
  
|Característica|¿Admite WID?|¿Compatible con SQL Server?|Para obtener más información acerca de esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redundancia básica de la base de datos mediante la replicación de extracción, donde uno o varios servidores de una copia solo read\ de los cambios de la solicitud de base de datos que se realizan en un servidor de origen que hospeda una copia de la base de datos read\ y escritura|Sí|No|[La función de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redundancia de base de datos con soluciones de disponibilidad de high\, como la conmutación por error clústeres o la creación de reflejos \ (en only\ de capa de base de datos) **Nota:** todas las topologías de implementación de AD FS compatible con clústeres en el nivel de servicio de AD FS.|No|Sí|[La función de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Información general de las soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>Consideraciones de SQL Server  
Considera los siguientes aspectos de la implementación si selecciona SQL Server como la base de datos de configuración para la implementación de AD FS.  
  
-   **SAML características y su efecto en el tamaño de la base de datos y el crecimiento**. Cuando se habilitan las características de detección de reproducción de token de SAML o de resolución de artefacto SAML, AD FS almacena información en la base de datos de configuración de SQL Server para cada token de AD FS que se emitió. No se considera importante el crecimiento de la base de datos de SQL Server como resultado de esta actividad, y depende del período de retención de reproducción de token configurado. Cada registro artefacto tiene un tamaño de aproximadamente 30 kilobytes \(KB\).  
  
-   **Número de servidores necesarios para la implementación**. Tendrás que agregar al menos un servidor adicional \ (en el número total de servidores necesarios para implementar tu infrastructure\ AD FS) que actuará como un host de la instancia de SQL Server dedicado. Si tienes previsto usar la conmutación por error clústeres o la creación de reflejos para proporcionar tolerancia a errores y la escalabilidad de la base de datos de configuración de SQL Server, es necesario un mínimo de dos servidores de SQL Server.  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Cómo el tipo de base de datos de configuración que seleccione puede afectar a los recursos de hardware  
El impacto en los recursos de hardware en un servidor de federación que se implementa en una granja de servidores con WID en contraposición a un servidor de federación que se implementa en una granja de servidores con la base de datos de SQL Server no es significativo. Sin embargo, es importante tener en cuenta que cuando usas WID para la granja de servidores, cada servidor de federación de ese conjunto debe almacenar, administrar y mantener los cambios de replicación de su copia local de la base de datos de configuración de AD FS mientras también sigue ofreciendo el funcionamiento normal que el servicio de federación requiere.  
  
En comparación, servidores de federación que se implementan en un conjunto que utiliza la base de datos de SQL Server no necesariamente contienen una instancia local de la base de datos de configuración de AD FS. Por lo tanto, podrán hacer ligeramente menos demandas en recursos de hardware.  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Comprobar que el entorno de producción puede admitir una implementación de AD FS  
Además de los servidores de federación que se va a implementar y según cómo esté configurado el entorno de producción existentes, los siguientes servidores adicionales podrán que deba proporcionar la infraestructura necesaria para admitir la nueva implementación de AD FS:  
  
-   Controlador de dominio de Active Directory  
  
-   Entidad de certificación \(CA\)  
  
-   Servidor Web a los metadatos de federación de host  
  
-   \(NLB\) equilibrio de carga de red  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
