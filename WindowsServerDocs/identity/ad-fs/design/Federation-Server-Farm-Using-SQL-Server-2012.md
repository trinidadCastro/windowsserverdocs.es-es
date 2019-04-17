---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: "Federación granja de servidores con SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Federación granja de servidores con SQL Server

>Se aplica a: Windows Server 2012

Esta topología para los servicios de federación de Active Directory \(AD FS\) difiere de la granja de servidores de federación mediante la topología de implementación de Windows Internal Database \(WID\) en que no se replica los datos para cada servidor de federación de la batería. En su lugar, todos los servidores de federación de la batería pueden leer y escribir datos en una base de datos que se almacena en un servidor que ejecuta Microsoft SQL Server que se encuentra en la red corporativa.  
  
## <a name="deployment-considerations"></a>Consideraciones de implementación  
Esta sección describe diversas consideraciones acerca de la audiencia de destino, ventajas y limitaciones que están asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Grandes organizaciones con más de 100 relaciones de confianza que se deben proporcionar tanto sus usuarios internos y externos a los usuarios solo acceso sign\ en \(SSO\) aplicación federada o los servicios  
  
-   Las organizaciones que ya usan SQL Server y quieren aprovechar sus herramientas existentes y la experiencia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Soporte técnico para un mayor número de las relaciones de confianza \(more than 100\)  
  
-   Soporte técnico para la detección de reproducción token \(a security feature\) y la resolución de artefacto \ (parte de la \(SAML\) lenguaje de marcado de seguridad aserción 2.0 protocol\)  
  
-   Herramientas de soporte técnico para todas las ventajas de SQL Server, como la creación de reflejo de base de datos, clústeres de conmutación por error, informes y administración  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Esta topología no proporciona redundancia de base de datos de manera predeterminada. Aunque una granja de servidores de federación con topología WID replica automáticamente la base de datos WID cada servidor de federación de la batería, la granja de servidores de federación con topología de SQL Server contiene solamente una copia de la base de datos  
  
> [!NOTE]  
> SQL Server es compatible con muchas opciones de redundancia de aplicación como clústeres de conmutación por error, la creación de reflejo de base de datos y distintos tipos de replicación de SQL Server y de datos diferente.  
  
El departamento de tecnología de la información de Microsoft \(IT\) usa el reflejo de base de datos de SQL Server en el modo de protección de high\ \(synchronous\) y conmutación por error para proporcionar compatibilidad con high\ disponibilidad para la instancia de SQL Server. No se han probado SQL Server transaccionales \(peer\-to\-peer\) y mezcla por el equipo de AD FS de Microsoft. Para obtener más información acerca de SQL Server, consulta [alta disponibilidad Solutions Overview](https://go.microsoft.com/fwlink/?LinkId=179853) o [seleccionar el tipo adecuado de duplicación](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versiones compatibles de SQL Server  
Las siguientes versiones SQL server son compatibles con AD FS instalada con Windows Server 2012:  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de red y la ubicación de servidor  
De forma similar a la granja de servidores de federación con topología WID, todos los servidores de federación de la batería están configurados para usar un nombre de sistema de nombres de dominio \(DNS\) de clúster \ (que representa el equipo\ de servicios de federación) y la dirección IP de un clúster como parte de la configuración del clúster \(NLB\) equilibrio de carga de red. Esto ayuda a que el host NLB asignas solicitudes de cliente a los servidores de federación individuales. Pueden usarse proxies de servidor de federación a las solicitudes de cliente de proxy de la granja de servidores de federación.  
  
La siguiente ilustración muestra la manera en que la empresa ficticia de Contoso farmacéuticas implementa su federación de servidores con la topología de SQL Server en la red corporativa. También se muestra cómo esa compañía configurado la red perimetral con acceso a un servidor DNS, un host NLB adicional que usa el mismo clúster DNS nombre \(fs.contoso.com\) que se usa en el clúster NLB de red corporativa, y con dos proxies de servidor de federación \(fsp1 and fsp2\).  
  
![Granja de servidores con SQL](media/FarmSQLProxies.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o servidores proxy de servidor de federación, consulta uno [requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisitos de resolución de nombre de Proxies de servidor de federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
