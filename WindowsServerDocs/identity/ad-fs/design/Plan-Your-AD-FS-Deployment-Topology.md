---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: "Planear la topología de implementación de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planear la topología de implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

El primer paso para planear una implementación de servicios de federación de Active Directory \(AD FS\) es determinar la topología de implementación adecuada para satisfacer las necesidades de la organización.  
  
Antes de leer este tema, revisar cómo se almacenan los datos de AD FS y cómo se replica en otros servidores de federación de una granja de servidores de federación y asegúrate de que conoces el propósito de y los métodos de replicación que pueden usarse para los datos subyacentes que se almacenan en la base de datos de configuración de AD FS.  
  
Hay dos tipos de base de datos que puedes usar para almacenar datos de configuración de AD FS: \(WID\) base de datos interna de Windows y Microsoft SQL Server. Para obtener más información, consulta [el rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Revisa las distintas ventajas y limitaciones que se asocian con WID o SQL Server como la base de datos de configuración de AD FS, junto con los distintos escenarios de aplicación que admite y, a continuación, haz tu selección.  
  
> [!IMPORTANT]  
> Para implementar la redundancia básica, equilibrio de carga y la opción para escalar el \(if required\) servicios de federación, te recomendamos que implementes al menos dos servidores de federación por granja de servidores de federación de todos los entornos de producción, independientemente del tipo de base de datos que se usará.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinar qué tipo de base de datos de configuración de AD FS usar  
AD FS utiliza una base de datos para almacenar la configuración y, en algunos casos, relacionados con los servicios de federación de datos transaccionales. Puedes usar el software de AD FS que seleccione el built\ en Windows Internal Database \(WID\) o Microsoft SQL Server 2008 o posterior para almacenar los datos en el servicio de federación.  
  
Para la mayoría de los casos, los tipos de base de dos datos son relativamente equivalentes. Sin embargo, hay algunas diferencias que debes tener en cuenta antes de comenzar a leer más acerca de las diversas topologías de implementación que se pueden usar con AD FS. La siguiente tabla describe las diferencias en las características compatibles entre una base de datos WID y una base de datos de SQL Server.  
  
||Característica|¿Admite WID?|¿Compatible con SQL Server?
| --- | --- | --- |--- |
|Características de AD FS|Implementación de granja de servidor de federación|Sí. Una granja de servidores WID tiene un límite de 30 servidores de federación si tienes 100 confianzas de terceros de confianza.</br></br>Una granja de servidores WID no admite la resolución de detección o artefacto de reproducción de token (parte del protocolo de lenguaje de marcado de aserción de seguridad (SAML)). |Sí. No tiene ningún límite para el número de servidores de federación que se pueden implementar en un solo conjunto aplicada  
|Características de AD FS|Resolución de artefacto SAML </br></br>**Nota:** esta característica no es necesaria para escenarios de Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sí  
|Características de AD FS|Detección de reproducción de token WS\/SAML\-federación|No|Sí  
|Características de la base de datos|Redundancia básica de la base de datos mediante la replicación de extracción, donde uno o varios servidores de una copia solo read\ de los cambios de la solicitud de base de datos que se realizan en un servidor de origen que hospeda una copia de la base de datos read\ y escritura|Sí|No 
|Características de la base de datos|Redundancia de base de datos con soluciones de disponibilidad de high\, como la conmutación por error clústeres o la creación de reflejos \ (en only\ de capa de base de datos) **Nota:** todas las topologías de implementación de AD FS compatible con clústeres en el nivel de servicio de AD FS.|No|Sí  

  
## <a name="sql-server-considerations"></a>Consideraciones de SQL Server  
Considera los siguientes aspectos de la implementación si selecciona SQL Server como la base de datos de configuración para la implementación de AD FS.  
  
-   **SAML características y su efecto en el tamaño de la base de datos y el crecimiento**. Cuando se habilitan las características de detección de reproducción de token de SAML o de resolución de artefacto SAML, AD FS almacena información en la base de datos de configuración de SQL Server para cada token de AD FS que se emitió. No se considera importante el crecimiento de la base de datos de SQL Server como resultado de esta actividad, y depende del período de retención de reproducción de token configurado. Cada registro artefacto tiene un tamaño de aproximadamente 30 kilobytes \(KB\).  
  
-   **Número de servidores necesarios para la implementación**. Tendrás que agregar al menos un servidor adicional \ (en el número total de servidores necesarios para implementar tu infrastructure\ AD FS) que actuará como un host de la instancia de SQL Server dedicado. Si tienes previsto usar la conmutación por error clústeres o la creación de reflejos para proporcionar tolerancia a errores y la escalabilidad de la base de datos de configuración de SQL Server, es necesario un mínimo de dos servidores de SQL Server.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Cómo el tipo de base de datos de configuración que seleccione puede afectar a los recursos de hardware  
El impacto en los recursos de hardware en un servidor de federación que se implementa en una granja de servidores con WID en contraposición a un servidor de federación que se implementa en una granja de servidores con la base de datos de SQL Server no es significativo. Sin embargo, es importante tener en cuenta que cuando usas WID para la granja de servidores, cada servidor de federación de ese conjunto debe almacenar, administrar y mantener los cambios de replicación de su copia local de la base de datos de configuración de AD FS mientras también sigue ofreciendo el funcionamiento normal que el servicio de federación requiere.  
  
En comparación, servidores de federación que se implementan en un conjunto que utiliza la base de datos de SQL Server no necesariamente contienen una instancia local de la base de datos de configuración de AD FS. Por lo tanto, podrán hacer ligeramente menos demandas en recursos de hardware.  
  
## <a name="BKMK_1"></a>Ubicación de un servidor de federación  
Seguridad mejores prácticas, coloca los servidores de federación de AD FS delante de un firewall y conectarlos a la red corporativa para evitar la exposición de Internet. Esto es importante porque los servidores de federación tienen autorización completa para conceder los tokens de seguridad. Por lo tanto, deben tener la misma protección que un controlador de dominio. Si un servidor de federación se ve comprometido, un usuario malintencionado tiene la capacidad para emitir tokens de acceso completa a todas las aplicaciones Web y a los servidores de federación que están protegidos por AD FS.  
  
> [!NOTE]  
> Seguridad recomendada, evitar que los servidores de federación accesibles directamente en Internet. Deberías asignarle a los servidores de federación acceso directo a Internet durante la configuración de un entorno de laboratorio de prueba o cuando la organización no tiene una red de perímetro.  
  
Para las redes corporativas típicas, se establece un firewall intranet\ frontal entre la red corporativa y la red perimetral y un firewall Internet\ orientados a menudo se establece entre la red perimetral e Internet. En esta situación, el servidor de federación se ubica en la red corporativa y no es accesible directamente por los clientes de Internet.  
  
> [!NOTE]  
> Los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de federación mediante la autenticación integrada de Windows.  
  
Un proxy de servidor de federación debe colocarse en la red perimetral antes de configurar los servidores de firewall para su uso con AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologías de implementación admitidas  
Los temas siguientes describen las diversas topologías de implementación que se pueden usar con AD FS. También se describen las ventajas y limitaciones asociadas con cada topología de implementación para que pueda seleccionar la topología más adecuada para tus necesidades empresariales específicos.  
  
-   [Federación granja de servidores con WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Federación granja de servidores con WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Federación granja de servidores con SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Consulta también  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

