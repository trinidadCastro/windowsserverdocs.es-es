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
ms.openlocfilehash: bbd3ec26e5fb0ce9857f2c9e5321300fb835b303
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834596"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Consideraciones sobre la topología de implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe consideraciones importantes para ayudarle a planear y diseñar los servicios de federación de Active Directory \(AD FS\) topología de implementación que utilice en su entorno de producción. En este tema es un punto de partida para revisar y valorar las consideraciones que afectan a las características o capacidades estarán disponibles después de implementar AD FS. Por ejemplo, dependiendo de qué base de datos de tipo que elija para almacenar la base de datos de configuración de AD FS determinarán en última instancia si puedes implementar ciertas lenguaje de marcado de aserción de seguridad \(SAML\) características que requieren SQL Servidor.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Cómo determinar qué tipo de base de datos de configuración de AD FS utilizar  
AD FS usa una base de datos para almacenar la configuración y, en algunos casos, los datos transaccionales relacionados con el servicio de federación. Puede usar el software AD FS para seleccionar la compilación\-en Windows Internal Database \(WID\) o Microsoft SQL Server 2005 o posterior para almacenar los datos en el servicio de federación.  
  
Para la mayoría de los objetivos, los dos tipos de bases de datos son relativamente equivalentes. Sin embargo, hay algunas diferencias a tener en cuenta antes de empezar a leer más sobre las diferentes topologías de implementación que puede usar con AD FS. La siguiente tabla describe las diferencias que existen entre las características de una base de datos de WID y una base de datos de SQL Server.  
  
Características de AD FS  
  
|Característica|¿Compatible con WID?|¿Compatible con SQL Server?|Más información sobre esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Implementación de una granja de servidores de federación|Sí, con un límite de 30 servidores de federación para cada granja de servidores|Sí. No hay un límite obligatorio para el número de servidores de federación que se pueden implementar en una sola granja|[Determinar la topología de implementación de AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Resolución de artefactos SAML **Nota:** Esta característica no es necesaria para escenarios de Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sí|[El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedimientos recomendados para segura de planeación e implementación de AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-detección de reproducción de tokens de federación|No|Sí|[El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedimientos recomendados para segura de planeación e implementación de AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
Características de la base de datos  
  
|Característica|¿Compatible con WID?|¿Compatible con SQL Server?|Más información sobre esta característica|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redundancia de base de datos básica usando replicación de extracción, donde uno o más servidores que hospeden una lectura\-única copia de los cambios de la solicitud de base de datos que se realizan en un servidor de origen que hospeda una lectura\/escribir la copia de la base de datos|Sí|No|[El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redundancia de base de datos con alta\-soluciones de disponibilidad, como la conmutación por error de agrupación en clústeres o la creación de reflejo \(en el nivel de base de datos solo\) **Nota:** Todas las topologías de implementación de AD FS son compatibles con clústeres en el nivel de servicio de AD FS.|No|Sí|[El rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Información general sobre soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>Consideraciones sobre SQL Server  
Debes considerar las siguientes cuestiones de implementación si seleccionas SQL Server como base de datos de configuración para tu implementación de AD FS.  
  
-   **Características de SAML y su efecto en el tamaño y el crecimiento de la base de datos**. Cuando se habilitan la característica de resolución de artefactos de SAML o la característica de detección de respuesta de token de SAML, AD FS almacena información en la base de datos de configuración de SQL Server para cada token de AD FS que se emita. El crecimiento de la base de datos de SQL Server como resultado de esta actividad no se considera significativo, y depende del período de retención que se configure para la respuesta de token. Cada registro de artefacto tiene un tamaño aproximado de 30 kilobytes \(KB\).  
  
-   **Número de servidores necesarios para la implementación**. Deberá agregar al menos un servidor adicional \(al número total de servidores necesarios para implementar la infraestructura de AD FS\) que actuará como host dedicado de la instancia de SQL Server. Si planeas utilizar el clúster de conmutación por error o la creación de reflejo para proporcionar tolerancia a errores y escalabilidad a la base de datos de configuración de SQL Server, se necesitan por lo menos dos servidores SQL.  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Cómo podría afectar a los recursos de hardware el tipo de base de datos de configuración que selecciones  
El impacto que se registra en los recursos de hardware de un servidor de federación implementado en una granja con WID, en contraposición a un servidor de federación implementado en una granja con la base de datos de SQL Server, no es significativo. Sin embargo, es importante tener en cuenta que, cuando utilizas WID para la granja, cada servidor de federación de dicha granja debe almacenar, administrar y mantener los cambios de replicación para su copia local de la base de datos de configuración de AD FS, a la vez que sigue proporcionando las operaciones normales que requiere el servicio de federación.  
  
En comparación, los servidores de federación que se implementan en una granja que utiliza la base de datos de SQL Server no contienen necesariamente una instancia local de la base de datos de configuración de AD FS. Por tanto, sus demandas de recursos de hardware podrían ser ligeramente inferiores.  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Comprobar que el entorno de producción es compatible con la implementación de AD FS  
Además de los servidores de federación que va a implementar y dependiendo de cómo se configura el entorno de producción existente, los siguientes servidores adicionales es posible que deba proporcionar la infraestructura necesaria para admitir la nueva implementación de AD FS:  
  
-   Controlador de dominio de Active Directory  
  
-   Entidad de certificación \(CA\)  
  
-   Servidor web para hospedar metadatos de federación  
  
-   Equilibrio de carga de red \(NLB\)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
