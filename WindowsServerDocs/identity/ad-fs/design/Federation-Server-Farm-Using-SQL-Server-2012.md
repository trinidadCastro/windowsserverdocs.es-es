---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Granja de servidores de federación con SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 66c8bae2fbccca2bf618e46ffd3ccc05cb52f911
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191497"
---
# <a name="federation-server-farm-using-sql-server"></a>Granja de servidores de federación con SQL Server

Esta topología de Active Directory Federation Services \(AD FS\) difiere de la granja de servidores de federación usa Windows Internal Database \(WID\) topología de implementación en el que no se replica los datos a cada servidor de federación en la granja de servidores. En su lugar, todos los servidores de federación en la granja de servidores pueden leer y escribir datos en una base de datos común que se almacena en un servidor que ejecuta Microsoft SQL Server que se encuentra en la red corporativa.  
  
## <a name="deployment-considerations"></a>Consideraciones acerca de la implementación  
Esta sección describen diversas consideraciones acerca de los destinatarios, ventajas y limitaciones asociadas con esta topología de implementación.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Las grandes organizaciones con más de 100 relaciones de confianza que necesita proporcionar a sus usuarios internos y externos a los usuarios con inicio de sesión único\-en \(SSO\) acceso a aplicaciones federadas o servicios  
  
-   Las organizaciones que ya usan SQL Server y desean sacar partido de sus herramientas existentes y experiencia  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Compatibilidad con un mayor número de relaciones de confianza \(más de 100\)  
  
-   Soporte técnico para la detección de reproducción de tokens \(una característica de seguridad\) y resolución de artefactos \(parte del lenguaje de marcado de aserción de seguridad \(SAML\) protocolo 2.0\)  
  
-   Herramientas de soporte técnico para todos los beneficios de SQL Server, como la creación de reflejo de base de datos, agrupación en clústeres de conmutación por error, informes y administración  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Esta topología no proporciona redundancia de base de datos de forma predeterminada. Aunque una granja de servidores de federación con topología WID replica automáticamente la base de datos WID en cada servidor de federación en la granja de servidores, la granja de servidores de federación con topología de SQL Server contiene solo una copia de la base de datos  
  
> [!NOTE]  
> SQL Server admite muchas opciones de redundancia de aplicación incluida la agrupación en clústeres de conmutación por error, la creación de reflejo de base de datos y varios tipos diferentes de replicación de SQL Server y datos diferentes.  
  
El Microsoft Information Technology \(TI\) departamento usa reflejos de base de datos de SQL Server de alta\-seguridad \(sincrónica\) modo y agrupación en clústeres de conmutación por error para proporcionar alta\- soporte de disponibilidad para la instancia de SQL Server. Transaccional de SQL Server \(punto\-a\-punto\) y la replicación de mezcla no se han probado por el equipo de producto de AD FS en Microsoft. Para obtener más información acerca de SQL Server, vea [Introducción a soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853) o [seleccionando el tipo de replicación adecuada](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versiones admitidas de SQL Server  
Las siguientes versiones SQL server son compatibles con AD FS instalado con Windows Server 2012:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de selección de ubicación y la red de servidor  
Al igual que la granja de servidores de federación con topología WID, todos los servidores de federación en la granja de servidores están configurados para usar un clúster de sistema de nombres de dominio \(DNS\) nombre \(que representa el nombre de servicio de federación\)y la dirección IP de un clúster como parte del equilibrio de carga de red \(NLB\) configuración del clúster. Esto ayuda a que el host de NLB asignar las solicitudes de cliente a los servidores de federación individuales. Servidores proxy de federación pueden utilizarse para las solicitudes de cliente de proxy a la granja de servidores de federación.  
  
La siguiente ilustración muestra cómo la empresa ficticia de Contoso Pharmaceuticals implementa su granja de servidores de federación con topología de SQL Server en la red corporativa. También se muestra cómo esa empresa ha configurado la red perimetral con acceso a un servidor DNS, un host de NLB adicional que usa el mismo nombre DNS del clúster \(fs.contoso.com\) que se usa en el clúster NLB de la red corporativa y con dos servidores proxy de federación \(fsp1 y fsp2\).  
  
![granja de servidores con SQL](media/FarmSQLProxies.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o servidores proxy de federación, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md) o [nombre Requisitos de resolución para los servidores proxy de federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
