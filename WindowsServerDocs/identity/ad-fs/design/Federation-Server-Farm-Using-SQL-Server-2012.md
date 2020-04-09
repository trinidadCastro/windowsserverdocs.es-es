---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Granja de servidores de federación con SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 862cbc74833e2d4e9f385ba961b58a1f703e6611
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853138"
---
# <a name="federation-server-farm-using-sql-server"></a>Granja de servidores de federación con SQL Server

Esta topología para Servicios de federación de Active Directory (AD FS) \(AD FS\) difiere de la granja de servidores de Federación con la topología de implementación de Windows Internal Database \(WID\) en que no replica los datos en cada servidor de Federación de la granja. En su lugar, todos los servidores de Federación de la granja pueden leer y escribir datos en una base de datos común que se almacena en un servidor que ejecuta Microsoft SQL Server que se encuentra en la red corporativa.  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
En esta sección se describen varias consideraciones sobre la audiencia, las ventajas y las limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Organizaciones de gran tamaño con más de 100 relaciones de confianza que necesitan proporcionar a los usuarios internos y a los usuarios externos con el inicio de sesión único\-en \(SSO\) acceso a servicios o aplicaciones federados  
  
-   Organizaciones que ya usan SQL Server y quieren aprovechar sus herramientas y conocimientos existentes  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Compatibilidad con un número mayor de relaciones de confianza \(más de 100\)  
  
-   Compatibilidad con la detección de reproducción de tokens \(un\) de características de seguridad y resolución de artefactos \(parte de Lenguaje de marcado de aserción de seguridad \(protocolo SAML\) 2,0\)  
  
-   Compatibilidad con las ventajas completas de SQL Server, como la creación de reflejo de la base de datos, los clústeres de conmutación por error, la creación de informes y las herramientas de administración  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Esta topología no proporciona redundancia de base de datos de forma predeterminada. Aunque una granja de servidores de Federación con topología WID replica automáticamente la base de datos WID en cada servidor de Federación de la granja, la granja de servidores de Federación con SQL Server topología solo contiene una copia de la base de datos.  
  
> [!NOTE]  
> SQL Server admite muchos datos diferentes y opciones de redundancia de aplicaciones, incluidos los clústeres de conmutación por error, la creación de reflejo de la base de datos y varios tipos diferentes de replicación SQL Server.  
  
La tecnología de la información de Microsoft \(Departamento de ti\) usa SQL Server la creación de reflejo de la base de datos en alta\-seguridad \(modo sincrónico\) y clúster de conmutación por error para proporcionar compatibilidad con alta\-disponibilidad para la instancia de SQL Server. El equipo del producto\) de Microsoft no ha probado SQL Server \(de\-del mismo nivel transaccionales a\-de AD FS del mismo nivel y replicación de mezcla. Para obtener más información acerca de SQL Server, consulte [información general sobre soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853) o [selección del tipo de replicación adecuado](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versiones de SQL Server compatibles  
Las siguientes versiones de SQL Server son compatibles con AD FS instaladas con Windows Server 2012:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de ubicación de servidor y diseño de red  
De forma similar a la granja de servidores de Federación con la topología WID, todos los servidores de Federación de la granja están configurados para usar un sistema de nombres de dominio de clúster \(nombre de\) DNS \(que representa el nombre de Servicio de federación\) y una dirección IP de clúster como parte de la configuración de clúster de \(NLB\) de equilibrio de carga de red. Esto ayuda al host de NLB a asignar las solicitudes de cliente a los servidores de Federación individuales. Los proxies de servidor de Federación se pueden usar para el proxy de las solicitudes de cliente a la granja de servidores de Federación.  
  
En la siguiente ilustración se muestra cómo la empresa ficticia contoso Pharmaceuticals implementó su granja de servidores de Federación con SQL Server topología en la red corporativa. También muestra cómo la empresa configuró la red perimetral con acceso a un servidor DNS, un host de NLB adicional que usa el mismo nombre DNS del clúster \(fs.contoso.com\) que se usa en el clúster NLB de la red corporativa y con dos servidores proxy de Federación \(fsp1 y fsp2\).  
  
![granja de servidores con SQL](media/FarmSQLProxies.gif)  
  
Para obtener más información acerca de cómo configurar el entorno de red para su uso con servidores de Federación o proxies de servidor de Federación, consulte [requisitos de resolución de nombres para servidores de Federación](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisitos de resolución de nombres para los proxies de servidor de Federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
