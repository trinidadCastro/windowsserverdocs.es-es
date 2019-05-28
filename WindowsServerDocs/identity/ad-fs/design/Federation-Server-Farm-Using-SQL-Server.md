---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Granja de servidores de federación con SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 585d0195b096056ba769f4e9a08d5c4d2156b96a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191449"
---
# <a name="federation-server-farm-using-sql-server"></a>Granja de servidores de federación con SQL Server

Esta topología de Active Directory Federation Services \(AD FS\) difiere de la granja de servidores de federación usa Windows Internal Database \(WID\) topología de implementación en el que no se replica los datos a cada servidor de federación en la granja de servidores. En su lugar, todos los servidores de federación en la granja de servidores pueden leer y escribir datos en una base de datos común que se almacena en un servidor que ejecuta Microsoft SQL Server que se encuentra en la red corporativa.  
  
> [!IMPORTANT]  
> Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012 y SQL Server 2014.  
  
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
Las siguientes versiones SQL server son compatibles con AD FS en Windows Server 2012 R2:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de selección de ubicación y la red de servidor  
Al igual que la granja de servidores de federación con topología WID, todos los servidores de federación en la granja de servidores están configurados para usar un clúster de sistema de nombres de dominio \(DNS\) nombre \(que representa el nombre de servicio de federación\)y la dirección IP de un clúster como parte del equilibrio de carga de red \(NLB\) configuración del clúster. Esto ayuda a que el host de NLB asignar las solicitudes de cliente a los servidores de federación individuales. Servidores proxy de federación pueden utilizarse para las solicitudes de cliente de proxy a la granja de servidores de federación.  
  
La siguiente ilustración muestra cómo la empresa ficticia de Contoso Pharmaceuticals implementa su granja de servidores de federación con topología de SQL Server en la red corporativa. También se muestra cómo esa empresa ha configurado la red perimetral con acceso a un servidor DNS, un host de NLB adicional que usa el mismo nombre DNS del clúster \(fs.contoso.com\) que se usa en el clúster NLB de la red corporativa y con dos web servidores proxy de aplicación \(fsp1 y fsp2\).  
  
![granja de servidores con SQL](media/SQLFarmADFSBlue.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o servidores proxy de aplicación web, vea "Requisitos de resolución de nombre" sección [requisitos de AD FS](AD-FS-Requirements.md) y [Plan Web Infraestructura del Proxy de aplicación (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opciones de alta disponibilidad para las granjas de servidores SQL  
En Windows Server 2012 R2, AD FS existe son dos nuevas opciones para lograr alta disponibilidad en granjas de servidores de AD FS usa SQL Server.  
  
-   Compatibilidad con grupos de disponibilidad AlwaysOn SQL Server  
  
-   Compatibilidad con distribuidas geográficamente alta disponibilidad con replicación de mezcla de SQL Server  
  
Esta sección describe cada una de estas opciones, ¿qué problemas que solucionan respectivamente y algunas consideraciones claves para decidir qué opciones de implementación.  
  
> [!NOTE]  
> Los conjuntos de servidores AD FS que utilizan Windows Internal Database \(WID\) proporcionar redundancia de datos básica con lectura\/acceso de escritura en el nodo del servidor de federación principal y leer\-acceso solo en nodos secundarios.  Esto puede usarse en un geográficamente local o en una topología distribuida geográficamente.  
>   
> Cuando se usa WID tener en cuenta las siguientes limitaciones:  
>   
> -   Una granja WID tiene un límite de 30 servidores de federación si tiene 100 o menos relaciones de confianza.  
> -   Una granja WID no admite la resolución de artefacto o de detección de reproducción de tokens \(parte del lenguaje de marcado de aserción de seguridad \(SAML\) protocolo\).  
  
En la tabla siguiente proporciona un resumen de uso de una granja WID.  
  
||||  
|-|-|-|  
||1 \- 100 de RP de confianzas|Más de 100 de RP de confianzas|  
|1 \- 30 AD FS nodos|WID admitida|No se admite con WID \- SQL necesario|  
|Nodos de más de 30 AD FS|No se admite con WID \- SQL necesario|No se admite con WID \- SQL necesario|  
  
### <a name="alwayson-availability-groups"></a>Grupos de disponibilidad AlwaysOn  
**Información general**  
  
Grupos de disponibilidad AlwaysOn se introdujeron en SQL Server 2012 y proporcionan una nueva forma de crear una instancia de SQL Server de alta disponibilidad.  Grupos de disponibilidad AlwaysOn combinan elementos de agrupación en clústeres y creación de reflejo de base de datos para obtener redundancia y conmutación por error en el nivel de instancia SQL y la capa de base de datos.  A diferencia de las opciones de alta disponibilidad anteriores, los grupos de disponibilidad AlwaysOn no requieren un almacenamiento común \(o red de área de almacenamiento\) en el nivel de base de datos.  
  
Un grupo de disponibilidad se compone de una réplica principal \(un conjunto de lectura\-escribir bases de datos principales\) y de uno a cuatro réplicas de disponibilidad \(conjuntos de bases de datos secundarias correspondientes\).  El grupo de disponibilidad admite una sola lectura\-escribir copia \(la réplica principal\), lectura y de uno a cuatro\-solo las réplicas de disponibilidad.  Cada réplica de disponibilidad debe residir en otro nodo de un único clúster de conmutación por error de Windows Server \(WSFC\) clúster.  Para obtener más información sobre la disponibilidad de AlwaysOn grupos vea [información general de los grupos de disponibilidad AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Desde la perspectiva de los nodos de una granja de servidores de SQL Server de AD FS, el grupo de disponibilidad AlwaysOn reemplaza la única instancia de SQL Server como la directiva \/ base de datos de artefacto.  El agente de escucha del grupo de disponibilidad es lo que el cliente \(el servicio de token de seguridad de AD FS\) usa para conectarse a SQL.  
  
El siguiente diagrama muestra una granja de servidores de AD FS SQL con el grupo de disponibilidad AlwaysOn.  
  
![granja de servidores con SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Grupos de disponibilidad AlwaysOn requieren que las instancias de SQL Server residen en clústeres de conmutación por error de Windows Server \(WSFC\) nodos.  
  
> [!NOTE]  
> Una única réplica de disponibilidad puede actuar como un destino de conmutación por error automática, los otros tres se basará en las conmutaciones por error manual.  
  
**Consideraciones de implementación clave**  
  
Si tiene previsto usar grupos de disponibilidad AlwaysOn en combinación con la replicación de mezcla de SQL Server, tome nota de los problemas descritos en "Consideraciones sobre la implementación de la clave para usar AD FS con la replicación de mezcla SQL Server" a continuación.  En concreto, cuando un grupo de disponibilidad AlwaysOn que contiene una base de datos es un suscriptor de replicación conmuta por error, se produce un error en la suscripción de replicación. Para reanudar la replicación, un administrador de replicación debe volver a configurar manualmente el suscriptor.  Vea la descripción de SQL Server de un problema específico en [suscriptores de replicación y grupos de disponibilidad AlwaysOn \(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx) y admite las instrucciones generales para grupos de disponibilidad AlwaysOn con Opciones de replicación en [replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configuración de AD FS para usar un grupo de disponibilidad AlwaysOn**  
  
Configurar una granja de servidores de AD FS con grupos de disponibilidad AlwaysOn requiere una pequeña modificación en el procedimiento de implementación de AD FS:  
  
1.  Las bases de datos que desea realizar copias de seguridad deben crearse antes de que se pueden configurar los grupos de disponibilidad AlwaysOn.  AD FS crea sus bases de datos como parte del programa de instalación y la configuración inicial del primer nodo de servicio de federación de una nueva granja de SQL Server de AD FS.  Como parte de la configuración de AD FS, debe especificar una cadena de conexión SQL, por lo que tendrá que configurar el primer nodo de granja de servidores de AD FS para conectarse directamente a una instancia de SQL \(esto es solo temporal\).   Para obtener instrucciones específicas sobre cómo configurar una granja de AD FS, incluida la configuración de un nodo de granja de servidores de AD FS con una cadena de conexión de SQL server, vea [configurar un servidor de federación](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Una vez que se han creado las bases de datos de AD FS, asignarlos a grupos de disponibilidad AlwaysOn y crear el agente de escucha de TCP/IP común con herramientas de SQL Server y procesar en [creación y configuración de grupos de disponibilidad \(deSQLServer\) ](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Por último, use PowerShell para editar las propiedades de AD FS para actualizar la cadena de conexión de SQL para usar la dirección DNS del agente de escucha del grupo de disponibilidad AlwaysOn.  
  
    Ejemplos de comandos PSH para actualizar la cadena de conexión de SQL para la base de datos de configuración de AD FS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Ejemplos de comandos PSH para actualizar la cadena de conexión de SQL para la base de datos de servicio de resolución de AD FS artefacto:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Replicación de mezcla SQL Server  
También ha introducido en SQL Server 2012, permite que la replicación de mezcla para la redundancia de datos de directiva de AD FS con las siguientes características:  
  
-   Leer y escribir la funcionalidad en todos los nodos \(no solo la réplica principal\)  
  
-   Pequeñas cantidades de datos que se replican de forma asincrónica para evitar la introducción de latencia en el sistema  
  
El siguiente diagrama muestra un granjas de servidores de AD FS SQL Server con redundancia geográfica con replicación de mezcla \(1 publisher, 2 suscriptores\):  
  
![granja de servidores con SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Consideraciones de implementación clave para usar AD FS con la replicación de mezcla de SQL Server \(tenga en cuenta los números en el diagrama anterior\)**  
  
-   La base de datos del distribuidor no se admite para su uso con grupos de disponibilidad AlwaysOn o creación de reflejo de base de datos.  Ver SQL Server son compatibles con las instrucciones para grupos de disponibilidad AlwaysOn con las opciones de replicación en [replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Cuando conmute por error un grupo de disponibilidad AlwaysOn que contiene una base de datos es un suscriptor de replicación, se produce un error en la suscripción de replicación. Para reanudar la replicación, un administrador de replicación debe volver a configurar manualmente el suscriptor.  Vea la descripción de SQL Server de un problema específico en [suscriptores de replicación y grupos de disponibilidad AlwaysOn \(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx) y admite las instrucciones generales para grupos de disponibilidad AlwaysOn con las opciones de replicación [replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
Para que obtener instrucciones más detalladas sobre cómo configurar AD FS usar la replicación de mezcla de SQL Server, vea [instalación redundancia geográfica con replicación de SQL Server](https://technet.microsoft.com/library/dn632406.aspx).  
  
## <a name="see-also"></a>Vea también  
[Planear la topología de la implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

