---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: "Federación granja de servidores con SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2333f79c733415833b1d54afc8c385700ac5581e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Federación granja de servidores con SQL Server

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Esta topología para los servicios de federación de Active Directory \(AD FS\) difiere de la granja de servidores de federación mediante la topología de implementación de Windows Internal Database \(WID\) en que no se replica los datos para cada servidor de federación de la batería. En su lugar, todos los servidores de federación de la batería pueden leer y escribir datos en una base de datos que se almacena en un servidor que ejecuta Microsoft SQL Server que se encuentra en la red corporativa.  
  
> [!IMPORTANT]  
> Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluidos SQL Server 2012 y SQL Server 2014.  
  
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
Las siguientes versiones SQL server son compatibles con AD FS en Windows Server 2012 R2:  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de diseño de red y la ubicación de servidor  
De forma similar a la granja de servidores de federación con topología WID, todos los servidores de federación de la batería están configurados para usar un nombre de sistema de nombres de dominio \(DNS\) de clúster \ (que representa el equipo\ de servicios de federación) y la dirección IP de un clúster como parte de la configuración del clúster \(NLB\) equilibrio de carga de red. Esto ayuda a que el host NLB asignas solicitudes de cliente a los servidores de federación individuales. Pueden usarse proxies de servidor de federación a las solicitudes de cliente de proxy de la granja de servidores de federación.  
  
La siguiente ilustración muestra la manera en que la empresa ficticia de Contoso farmacéuticas implementa su federación de servidores con la topología de SQL Server en la red corporativa. También se muestra cómo esa compañía configurado la red perimetral con acceso a un servidor DNS, un host NLB adicional que usa el mismo clúster DNS nombre \(fs.contoso.com\) que se usa en el clúster NLB de red corporativa, y con dos proxy de aplicación web \(wap1 and wap2\).  
  
![Granja de servidores con SQL](media/SQLFarmADFSBlue.gif)  
  
Para obtener más información sobre cómo configurar el entorno de red para su uso con los servidores de federación o proxy de aplicación web, consulta "Requisitos de resolución de nombre de" sección [AD FS requisitos](AD-FS-Requirements.md) y [planear la infraestructura de Proxy de aplicación Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opciones de alta disponibilidad para conjuntos SQL Server  
En Windows Server 2012 R2, AD FS allí son dos nuevas opciones para admitir la alta disponibilidad en conjuntos de AD FS con SQL Server.  
  
-   Compatibilidad con grupos de disponibilidad SQL Server AlwaysOn  
  
-   Soporte para geográficamente alta disponibilidad mediante duplicación de mezcla de SQL Server  
  
En esta sección se describe cada una de estas opciones, los problemas que se resuelven los respectivamente y algunos aspectos clave para decidir qué opciones puedes implementar.  
  
> [!NOTE]  
> Granjas FS de anuncios que usan Windows Internal Database \(WID\) proporcionan redundancia de datos básico con acceso de read\ y escritura en el nodo del servidor de federación principal y solo read\ en nodos secundarios.  Esto puede usarse en una topología geográficamente o un geográficamente local.  
>   
> Al usar WID Ten en cuenta las siguientes limitaciones:  
>   
> -   Una granja de servidores WID tiene un límite de 30 servidores de federación si tienes 100 confianzas de terceros de confianza.  
> -   Una granja de servidores WID no es compatible con una resolución de detección o artefacto reproducción token \ (que forma parte de la \(SAML\) protocol\ lenguaje de marcado de aserción de seguridad).  
  
La siguiente tabla proporciona un resumen de uso de una granja de servidores WID.  
  
||||  
|-|-|-|  
||1 \-100 confianzas de punto de reunión|Más de 100 confianzas de punto de reunión|  
|1 \-30 AD FS nodos|WID compatible|No se admite con WID \-SQL necesario|  
|Más de 30 AD FS nodos|No se admite con WID \-SQL necesario|No se admite con WID \-SQL necesario|  
  
### <a name="alwayson-availability-groups"></a>Grupos de disponibilidad AlwaysOn  
**Introducción**  
  
Grupos de disponibilidad AlwaysOn se introdujeron en SQL Server 2012 y proporcionan una nueva forma de crear una instancia de SQL Server de alta disponibilidad.  Grupos de disponibilidad AlwaysOn combinan los elementos de los clústeres y creación de reflejo de base de datos para conmutación por error en la capa de la instancia SQL y la capa de base de datos y la redundancia.  A diferencia de las opciones anteriores de alta disponibilidad, grupos de disponibilidad AlwaysOn no requieren un almacenamiento común \ (o red\ del área de almacenamiento) en el nivel de base de datos.  
  
Un grupo de disponibilidad se compone de una réplica principal \ (un conjunto de escritura de read\ principal databases\) y hasta cuatro réplicas de disponibilidad \ (conjuntos de correspondiente databases\ secundario).  El grupo de disponibilidad admite una copia de solo escritura read\ \ (replica\ principal), y hasta cuatro réplicas de disponibilidad solo read\.  Cada réplica de disponibilidad debe residir en un nodo diferente de un único clúster \(WSFC\) clústeres de conmutación por error de Windows Server.  Para obtener más información sobre la disponibilidad de AlwaysOn consulta grupos [información general de los grupos AlwaysOn disponibilidad \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Desde la perspectiva de los nodos de un conjunto de AD FS SQL Server, el grupo de disponibilidad AlwaysOn reemplaza la única instancia de SQL Server que la directiva \ o base de datos de artefacto.  El agente de escucha de grupo de disponibilidad es lo que el cliente \ (la AD FS seguridad token service\) se usa para conectarse a SQL.  
  
El siguiente diagrama muestra una granja de servidores de AD FS SQL con grupo AlwaysOn disponibilidad.  
  
![Granja de servidores con SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Grupos de disponibilidad AlwaysOn requieren que las instancias de SQL Server se encuentran en nodos de clústeres de conmutación por error de Windows Server \(WSFC\).  
  
> [!NOTE]  
> Solo una réplica de disponibilidad puede actuar como un destino de conmutación automática por error, los otros tres recurrirán a manual migraciones tras error.  
  
**Consideraciones de implementación de la clave**  
  
Si tienes previsto usar grupos AlwaysOn disponibilidad en combinación con la duplicación de mezcla de SQL Server, ten en cuenta los problemas descritos en "Consideraciones de implementación de la clave para el uso de AD FS con la duplicación de mezcla SQL Server".  En particular, cuando un grupo de disponibilidad de AlwaysOn que contenga una base de datos es un suscriptor de replicación conmuta por error, se produce un error en la suscripción de replicación. Para reanudar la replicación, un administrador de replicación debe volver a configurar manualmente el suscriptor.  Consulta la descripción de SQL Server del problema específico en [\(SQL Server\) suscriptores de replicación y grupos de disponibilidad AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) y general admite las instrucciones para grupos de disponibilidad AlwaysOn con opciones de replicación de [\(SQL Server\) replicación, seguimiento de cambios, cambiar la captura de datos y grupos de disponibilidad AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configurar AD FS para usar un grupo de disponibilidad AlwaysOn**  
  
Configurar un conjunto de AD FS con grupos de disponibilidad AlwaysOn requiere una ligera modificación en el procedimiento de implementación de AD FS:  
  
1.  Las bases de datos que quieres hacer una copia deben crearse antes de que los grupos de disponibilidad AlwaysOn pueden configurarse.  AD FS crea sus bases de datos como parte de la instalación y configuración inicial del primer nodo de servicios de federación de un nuevo conjunto de AD FS SQL Server.  Como parte de la configuración de AD FS, debes especificar una cadena de conexión de SQL, por lo que tendrás que configurar el primer nodo de granja de servidores de AD FS para conectarse directamente a una instancia de SQL \ (es decir, solo temporary\).   Para obtener instrucciones específicas sobre cómo configurar una granja de servidores de AD FS, incluida la configuración de un nodo de granja de servidores de AD FS con una cadena de conexión de SQL server, consulta [configurar un servidor de federación](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Una vez que se han creado las bases de datos de AD FS, asignarlos a grupos de disponibilidad AlwaysOn y crea la escucha TCPIP comunes con las herramientas de SQL Server y procesar en [\(SQL Server\) creación y configuración de los grupos de disponibilidad](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Por último, usa PowerShell para modificar las propiedades de AD FS para actualizar la cadena de conexión de SQL para usar la dirección del agente de escucha del grupo de disponibilidad AlwaysOn DNS.  
  
    Ejemplos de comandos Psh: actualizar la cadena de conexión de SQL para la base de datos de la directiva AD FS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Ejemplos de comandos Psh: actualizar la cadena de conexión de SQL para la base de datos de la directiva AD FS:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Duplicación de mezcla SQL Server  
También se introdujeron en SQL Server 2012, duplicación de mezcla permite AD FS directiva redundancia de datos con las siguientes características:  
  
-   Leer y escribir funcionalidad en todos los nodos \ (no solo el primary\)  
  
-   Pequeñas cantidades de datos replicados asincrónicamente para evitar la presentación de latencia al sistema  
  
El siguiente diagrama muestra un granjas de AD FS SQL Server geográficamente redundante con la duplicación de mezcla \ (1 Editor, 2 subscribers\):  
  
![Granja de servidores con SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Consideraciones de implementación de la clave para usar AD FS con la duplicación de mezcla de SQL Server \ (Ten en cuenta los números en el diagrama de above\)**  
  
-   No se admite la base de datos de distribuidor para su uso con grupos de disponibilidad AlwaysOn o la creación de reflejo de base de datos.  Consulta SQL Server admite las instrucciones para grupos de disponibilidad AlwaysOn con opciones de replicación de [\(SQL Server\) replicación, seguimiento de cambios, cambiar la captura de datos y grupos de disponibilidad AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Cuando un grupo de disponibilidad de AlwaysOn que contenga una base de datos es un suscriptor de replicación conmuta por error, se produce un error en la suscripción de replicación. Para reanudar la replicación, un administrador de replicación debe volver a configurar manualmente el suscriptor.  Consulta la descripción de SQL Server del problema específico en [\(SQL Server\) suscriptores de replicación y grupos de disponibilidad AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) y general admite las instrucciones para grupos de disponibilidad AlwaysOn con opciones de replicación [\(SQL Server\) replicación, seguimiento de cambios, cambiar la captura de datos y grupos de disponibilidad AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
Para que obtener instrucciones más detalladas sobre cómo configurar AD FS usar una réplica de mezcla de SQL Server, consulta [redundancia geográfica del programa de instalación con la duplicación de SQL Server](https://technet.microsoft.com/en-us/library/dn632406.aspx).  
  
## <a name="see-also"></a>Consulta también  
[Planear la topología de implementación de AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

