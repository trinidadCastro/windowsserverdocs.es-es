---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Planear la topología de la implementación de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821716"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planear la topología de la implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

El primer paso para planear una implementación de Active Directory Federation Services \(AD FS\) consiste en determinar la topología de implementación adecuada para satisfacer las necesidades de su organización.  
  
Antes de leer este tema, revise cómo se almacenan los datos de AD FS y cómo se replican en otros servidores de federación en una granja de servidores de federación y asegúrese de comprender el propósito de y los métodos de replicación que se pueden usar para los datos subyacentes que se almacenan en AD FS figurar base de datos de configuración.  
  
Hay dos tipos de base de datos que puede usar para almacenar datos de configuración de AD FS: Windows Internal Database \(WID\) y Microsoft SQL Server. Para obtener más información, consulta [Rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Revise las diversas ventajas y limitaciones que están asociadas con el uso de WID o SQL Server como la base de datos de configuración de AD FS, junto con los diversos escenarios de aplicaciones que admiten y, a continuación, realice la selección.  
  
> [!IMPORTANT]  
> Para implementar redundancia básica, equilibrio de carga y la opción para escalar el servicio de federación \(si es necesario\), le recomendamos que implemente al menos dos servidores de federación por granja de servidores de federación para todos los entornos de producción, independientemente del tipo de base de datos que va a utilizar.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Cómo determinar qué tipo de base de datos de configuración de AD FS utilizar  
AD FS usa una base de datos para almacenar la configuración y, en algunos casos, los datos transaccionales relacionados con el servicio de federación. Puede usar el software AD FS para seleccionar la compilación\-en Windows Internal Database \(WID\) o Microsoft SQL Server 2008 o posterior para almacenar los datos en el servicio de federación.  
  
Para la mayoría de los objetivos, los dos tipos de bases de datos son relativamente equivalentes. Sin embargo, hay algunas diferencias a tener en cuenta antes de empezar a leer más sobre las diferentes topologías de implementación que puede usar con AD FS. La siguiente tabla describe las diferencias que existen entre las características de una base de datos de WID y una base de datos de SQL Server.  
  
||Característica|¿Compatible con WID?|¿Compatible con SQL Server?
| --- | --- | --- |--- |
|Características de AD FS|Implementación de una granja de servidores de federación|Sí. Una granja WID tiene un límite de 30 servidores de federación si tiene 100 o menos relaciones de confianza.</br></br>Una granja WID no admite la resolución de artefacto o de detección de reproducción de tokens (parte del protocolo de lenguaje de marcado de aserción de seguridad (SAML)). |Sí. No hay un límite obligatorio para el número de servidores de federación que se pueden implementar en una sola granja  
|Características de AD FS|Resolución de artefactos SAML </br></br>**Nota:** Esta característica no es necesaria para escenarios de Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sí  
|Características de AD FS|SAML\/WS\-detección de reproducción de tokens de federación|No|Sí  
|Características de la base de datos|Redundancia de base de datos básica usando replicación de extracción, donde uno o más servidores que hospeden una lectura\-única copia de los cambios de la solicitud de base de datos que se realizan en un servidor de origen que hospeda una lectura\/escribir la copia de la base de datos|Sí|No 
|Características de la base de datos|Redundancia de base de datos con alta\-soluciones de disponibilidad, como la conmutación por error de agrupación en clústeres o la creación de reflejo \(en el nivel de base de datos solo\) **Nota:** Todas las topologías de implementación de AD FS son compatibles con clústeres en el nivel de servicio de AD FS.|No|Sí  

  
## <a name="sql-server-considerations"></a>Consideraciones sobre SQL Server  
Debes considerar las siguientes cuestiones de implementación si seleccionas SQL Server como base de datos de configuración para tu implementación de AD FS.  
  
-   **Características de SAML y su efecto en el tamaño y el crecimiento de la base de datos**. Cuando se habilitan la característica de resolución de artefactos de SAML o la característica de detección de respuesta de token de SAML, AD FS almacena información en la base de datos de configuración de SQL Server para cada token de AD FS que se emita. El crecimiento de la base de datos de SQL Server como resultado de esta actividad no se considera significativo, y depende del período de retención que se configure para la respuesta de token. Cada registro de artefacto tiene un tamaño aproximado de 30 kilobytes \(KB\).  
  
-   **Número de servidores necesarios para la implementación**. Deberá agregar al menos un servidor adicional \(al número total de servidores necesarios para implementar la infraestructura de AD FS\) que actuará como host dedicado de la instancia de SQL Server. Si planeas utilizar el clúster de conmutación por error o la creación de reflejo para proporcionar tolerancia a errores y escalabilidad a la base de datos de configuración de SQL Server, se necesitan por lo menos dos servidores SQL.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Cómo podría afectar a los recursos de hardware el tipo de base de datos de configuración que selecciones  
El impacto que se registra en los recursos de hardware de un servidor de federación implementado en una granja con WID, en contraposición a un servidor de federación implementado en una granja con la base de datos de SQL Server, no es significativo. Sin embargo, es importante tener en cuenta que, cuando utilizas WID para la granja, cada servidor de federación de dicha granja debe almacenar, administrar y mantener los cambios de replicación para su copia local de la base de datos de configuración de AD FS, a la vez que sigue proporcionando las operaciones normales que requiere el servicio de federación.  
  
En comparación, los servidores de federación que se implementan en una granja que utiliza la base de datos de SQL Server no contienen necesariamente una instancia local de la base de datos de configuración de AD FS. Por tanto, sus demandas de recursos de hardware podrían ser ligeramente inferiores.  
  
## <a name="BKMK_1"></a>Dónde colocar un servidor de federación  
Como procedimiento de seguridad de procedimientos recomendados, colocar los servidores de federación de AD FS delante de un firewall y conéctelos a la red corporativa para evitar la exposición de Internet. Esto es importante porque los servidores de federación tienen autorización completa para conceder tokens de seguridad. Por lo tanto, deben tener la misma protección que un controlador de dominio. Si un servidor de federación está en peligro, un usuario malintencionado tiene la capacidad de emitir tokens de acceso completo a todas las aplicaciones Web y servidores de federación que están protegidos por AD FS.  
  
> [!NOTE]  
> Como procedimiento de seguridad recomendado, evite que los servidores de federación directamente accesibles en Internet. Considere la posibilidad de dar acceso directo a Internet de los servidores de federación solo cuando esté configurando un entorno de laboratorio de pruebas o cuando su organización no tiene una red perimetral.  
  
Para las redes corporativas típicas, una intranet\-accesible desde el servidor de seguridad se establece entre la red corporativa y la red perimetral y de Internet\-accesible desde el servidor de seguridad con frecuencia se establece entre la red perimetral y el Internet. En esta situación, el servidor de federación se encuentra dentro de la red corporativa y no es directamente accesible para los clientes de Internet.  
  
> [!NOTE]  
> Los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de federación mediante la autenticación integrada de Windows.  
  
Un servidor proxy de federación debe colocarse en la red perimetral antes de configurar los servidores de firewall para su uso con AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologías de implementación admitidas  
Los temas siguientes describen las distintas topologías de implementación que puede usar con AD FS. También se describen las ventajas y limitaciones de cada una de ellas para que puedas seleccionar la más adecuada para tus necesidades empresariales específicas.  
  
-   [Granja de servidores de federación con WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Granja de servidores de federación con WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Granja de servidores de federación con SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Vea también  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

