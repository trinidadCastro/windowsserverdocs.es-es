---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Planear la topología de la implementación de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 53364e076a8c3b7d95e8c834a5a7621071ed6061
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858678"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planear la topología de la implementación de AD FS

El primer paso en la planeación de una implementación de Servicios de federación de Active Directory (AD FS) \(AD FS\) es determinar la topología de implementación correcta para satisfacer las necesidades de su organización.  
  
Antes de leer este tema, revise el modo en que los datos de AD FS se almacenan y replican en otros servidores de Federación en una granja de servidores de Federación y asegúrese de que entiende el propósito de y los métodos de replicación que se pueden usar para los datos subyacentes que se almacenan en la base de datos de configuración de AD FS.  
  
Hay dos tipos de bases de datos que puede usar para almacenar AD FS datos de configuración: Windows Internal Database \(WID\) y Microsoft SQL Server. Para obtener más información, consulta [Rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Revise las distintas ventajas y limitaciones que se asocian al uso de WID o SQL Server como base de datos de configuración AD FS, junto con los distintos escenarios de aplicación que admiten y, a continuación, realice la selección.  
  
> [!IMPORTANT]  
> Para implementar la redundancia básica, el equilibrio de carga y la opción para escalar el Servicio de federación \(si es necesario\), se recomienda que implemente al menos dos servidores de Federación por granja de servidores de Federación para todos los entornos de producción, independientemente del tipo de base de datos que vaya a utilizar.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Cómo determinar qué tipo de base de datos de configuración de AD FS utilizar  
AD FS utiliza una base de datos para almacenar la configuración y, en algunos casos, los datos transaccionales relacionados con el Servicio de federación. Puede usar el software de AD FS para seleccionar el\-compilado en Windows Internal Database \(WID\) o Microsoft SQL Server 2008 o una versión más reciente para almacenar los datos en el Servicio de federación.  
  
Para la mayoría de los objetivos, los dos tipos de bases de datos son relativamente equivalentes. Sin embargo, hay algunas diferencias que debe tener en cuenta antes de empezar a leer más sobre las distintas topologías de implementación que puede usar con AD FS. La siguiente tabla describe las diferencias que existen entre las características de una base de datos de WID y una base de datos de SQL Server.  
  
||Característica|¿Compatible con WID?|¿Compatible con SQL Server?
| --- | --- | --- |--- |
|AD FS características|Implementación de una granja de servidores de federación|Sí. Una granja WID tiene un límite de 30 servidores de Federación si tiene 100 o menos relaciones de confianza para usuario autenticado.</br></br>Una granja de WID no admite la detección de reproducción de tokens ni la resolución de artefactos (parte del protocolo Lenguaje de marcado de aserción de seguridad (SAML)). |Sí. No hay un límite obligatorio para el número de servidores de federación que se pueden implementar en una sola granja  
|AD FS características|Resolución de artefactos SAML </br></br>**Nota:** Esta característica no es necesaria para los escenarios de Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sí  
|AD FS características|Detección de reproducción de tokens de Federación de SAML\/WS\-|No|Sí  
|Características de la base de datos|Redundancia básica de bases de datos mediante la replicación de extracción, en la que uno o más servidores que hospedan una\-de lectura solo de la solicitud de la base de datos cambian los cambios que se realizan en un servidor de origen que hospeda una copia de lectura\/escritura de la base de datos|Sí|No 
|Características de la base de datos|Redundancia de bases de datos mediante soluciones de alta disponibilidad\-, como clústeres de conmutación por error o creación de reflejo \(solo en el nivel de base de datos\) **Nota:** todas AD FS topologías de implementación admiten la agrupación en clústeres en el nivel de servicio AD FS.|No|Sí  

  
## <a name="sql-server-considerations"></a>Consideraciones sobre SQL Server  
Debes considerar las siguientes cuestiones de implementación si seleccionas SQL Server como base de datos de configuración para tu implementación de AD FS.  
  
-   **Características de SAML y su efecto en el tamaño y el crecimiento de la base de datos**. Cuando se habilitan la característica de resolución de artefactos de SAML o la característica de detección de respuesta de token de SAML, AD FS almacena información en la base de datos de configuración de SQL Server para cada token de AD FS que se emita. El crecimiento de la base de datos de SQL Server como resultado de esta actividad no se considera significativo, y depende del período de retención que se configure para la respuesta de token. Cada registro de artefacto tiene un tamaño aproximado de 30 kilobytes \(KB\).  
  
-   **Número de servidores necesarios para la implementación**. Tendrá que agregar al menos un servidor adicional \(al número total de servidores necesarios para implementar la infraestructura de AD FS\) que actuará como host dedicado de la instancia de SQL Server. Si planeas utilizar el clúster de conmutación por error o la creación de reflejo para proporcionar tolerancia a errores y escalabilidad a la base de datos de configuración de SQL Server, se necesitan por lo menos dos servidores SQL.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Cómo podría afectar a los recursos de hardware el tipo de base de datos de configuración que selecciones  
El impacto que se registra en los recursos de hardware de un servidor de federación implementado en una granja con WID, en contraposición a un servidor de federación implementado en una granja con la base de datos de SQL Server, no es significativo. Sin embargo, es importante tener en cuenta que, cuando utilizas WID para la granja, cada servidor de federación de dicha granja debe almacenar, administrar y mantener los cambios de replicación para su copia local de la base de datos de configuración de AD FS, a la vez que sigue proporcionando las operaciones normales que requiere el servicio de federación.  
  
En comparación, los servidores de federación que se implementan en una granja que utiliza la base de datos de SQL Server no contienen necesariamente una instancia local de la base de datos de configuración de AD FS. Por tanto, sus demandas de recursos de hardware podrían ser ligeramente inferiores.  
  
## <a name="where-to-place-a-federation-server"></a><a name="BKMK_1"></a>Dónde colocar un servidor de Federación  
Como práctica recomendada de seguridad, coloque AD FS servidores de Federación delante de un firewall y conéctelos a la red corporativa para evitar la exposición de Internet. Esto es importante, ya que los servidores de Federación tienen autorización completa para conceder tokens de seguridad. Por lo tanto, deben tener la misma protección que un controlador de dominio. Si un servidor de Federación está en peligro, un usuario malintencionado tiene la capacidad de emitir tokens de acceso completo a todas las aplicaciones web y a los servidores de Federación protegidos por AD FS.  
  
> [!NOTE]  
> Como procedimiento de seguridad recomendado, evite que sus servidores de Federación estén directamente accesibles en Internet. Considere la posibilidad de asignar a los servidores de Federación acceso directo a Internet solo cuando esté configurando un entorno de laboratorio de pruebas o cuando su organización no disponga de una red perimetral.  
  
En el caso de las redes corporativas típicas, se establece una intranet\-servidor de seguridad entre la red corporativa y la red perimetral, y a menudo se establece un firewall con conexión a Internet\-a través de la red perimetral e Internet. En esta situación, el servidor de Federación se encuentra dentro de la red corporativa y no es accesible directamente para los clientes de Internet.  
  
> [!NOTE]  
> Los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de Federación a través de la autenticación integrada de Windows.  
  
Se debe colocar un servidor proxy de Federación en la red perimetral antes de configurar los servidores de Firewall para su uso con AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologías de implementación admitidas  
En los temas siguientes se describen las diversas topologías de implementación que puede utilizar con AD FS. También se describen las ventajas y limitaciones de cada una de ellas para que puedas seleccionar la más adecuada para tus necesidades empresariales específicas.  
  
-   [Granja de servidores de federación con WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Granja de servidores de federación con WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Granja de servidores de federación con SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Consulta también  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

