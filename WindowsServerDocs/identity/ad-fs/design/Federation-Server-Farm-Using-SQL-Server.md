---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Granja de servidores de federación con SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4527b6787531b3a349534092e3597a91dbebf78f
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87864139"
---
# <a name="legacy-ad-fs-federation-server-farm-using-sql-server"></a>Granja de servidores de Federación de AD FS heredada mediante SQL Server

Esta topología para Servicios de federación de Active Directory (AD FS) \( AD FS \) difiere de la granja de servidores de Federación con la topología de implementación WID de Windows Internal Database \( \) en que no replica los datos en cada servidor de Federación de la granja. En su lugar, todos los servidores de Federación de la granja pueden leer y escribir datos en una base de datos común que se almacena en un servidor que ejecuta Microsoft SQL Server que se encuentra en la red corporativa.

> [!IMPORTANT]
> Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones más recientes, incluidas SQL Server 2012 y SQL Server 2014.

## <a name="deployment-considerations"></a>Consideraciones de la implementación
En esta sección se describen varias consideraciones sobre la audiencia, las ventajas y las limitaciones que están asociadas con esta topología de implementación.

### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?

- Organizaciones de gran tamaño con más de 100 relaciones de confianza que necesitan proporcionar a los usuarios internos y a los usuarios externos \- acceso de inicio de sesión único \( a los \) servicios o aplicaciones federados.

- Organizaciones que ya usan SQL Server y quieren aprovechar sus herramientas y conocimientos existentes

### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?

- Compatibilidad con un mayor número de relaciones de confianza \( más de 100\)

- Compatibilidad con la detección de reproducción de tokens \( \) \( parte de la característica de seguridad y la resolución de artefactos del \( Protocolo lenguaje de marcado de aserción de seguridad SAML \) 2,0\)

- Compatibilidad con las ventajas completas de SQL Server, como la creación de reflejo de la base de datos, los clústeres de conmutación por error, la creación de informes y las herramientas de administración

### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?

- Esta topología no proporciona redundancia de base de datos de forma predeterminada. Aunque una granja de servidores de Federación con topología WID replica automáticamente la base de datos WID en cada servidor de Federación de la granja, la granja de servidores de Federación con SQL Server topología solo contiene una copia de la base de datos.

    > [!NOTE]
    > SQL Server admite muchos datos diferentes y opciones de redundancia de aplicaciones, incluidos los clústeres de conmutación por error, la creación de reflejo de la base de datos y varios tipos diferentes de replicación SQL Server.

El Departamento de TI de tecnología de la información \( \) de Microsoft usa SQL Server la creación de reflejo de la base de datos en \- modo sincrónico de alta seguridad \( \) y clústeres de conmutación por error para proporcionar \- compatibilidad con alta disponibilidad para la instancia de SQL Server. \( \- \- \) El equipo del producto de AD FS de Microsoft no ha probado SQL Server del mismo nivel transaccional a par y la replicación de mezcla. Para obtener más información acerca de SQL Server, consulte [información general sobre soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853) o [selección del tipo de replicación adecuado](https://go.microsoft.com/fwlink/?LinkId=214648).

### <a name="supported-sql-server-versions"></a>Versiones de SQL Server compatibles
Las siguientes versiones de SQL Server son compatibles con AD FS en Windows Server 2012 R2:

- SQL Server 2008 \/ R2

- SQL Server 2012

- SQL Server 2014

## <a name="server-placement-and-network-layout-recommendations"></a>Recomendaciones de ubicación de servidor y diseño de red
De forma similar a la granja de servidores de Federación con la topología WID, todos los servidores de Federación de la granja están configurados para usar un nombre DNS del sistema de nombres de dominio del clúster \( \) \( que representa el nombre del servicio de Federación \) y una dirección IP del clúster como parte de la configuración del clúster NLB de equilibrio de carga de red \( \) . Esto ayuda al host de NLB a asignar las solicitudes de cliente a los servidores de Federación individuales. Los proxies de servidor de Federación se pueden usar para el proxy de las solicitudes de cliente a la granja de servidores de Federación.

En la siguiente ilustración se muestra cómo la empresa ficticia contoso Pharmaceuticals implementó su granja de servidores de Federación con SQL Server topología en la red corporativa. También muestra cómo la empresa configuró la red perimetral con acceso a un servidor DNS, un host de NLB adicional que usa el mismo nombre DNS del clúster \( FS.contoso.com \) que se usa en el clúster NLB de la red corporativa, y con dos servidores proxy de aplicación Web \( wap1 y WAP2 \) .

![granja de servidores con SQL](media/SQLFarmADFSBlue.gif)

Para obtener más información acerca de cómo configurar el entorno de red para su uso con servidores de Federación o proxy de aplicación Web, consulte la sección "requisitos de resolución de nombres" en [AD FS requisitos](AD-FS-Requirements.md) y [planear la infraestructura del proxy de aplicación web (WAP)](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)).

## <a name="high-availability-options-for-sql-server-farms"></a>Opciones de alta disponibilidad para granjas de SQL Server
En Windows Server 2012 R2, AD FS hay dos nuevas opciones para admitir la alta disponibilidad en las granjas de AD FS mediante SQL Server.

- Compatibilidad con SQL Server Grupos de disponibilidad AlwaysOn

- Compatibilidad con alta disponibilidad distribuida geográficamente mediante la replicación de mezcla de SQL Server

En esta sección se describe cada una de estas opciones, qué problemas resuelven respectivamente y algunas consideraciones clave para decidir qué opciones implementar.

> [!NOTE]
> AD FS granjas que usan Windows Internal Database \( WID \) proporcionan redundancia de datos básica con \/ acceso de lectura y escritura en el nodo del servidor de Federación principal y \- acceso de solo lectura en nodos secundarios.Se puede usar en una topología geográficamente local o distribuida geográficamente.
>
> Al usar WID, tenga en cuenta las siguientes limitaciones:
>
> - Una granja WID tiene un límite de 30 servidores de Federación si tiene 100 o menos relaciones de confianza para usuario autenticado.
> - Una granja WID no admite la detección de reproducción de tokens ni \( la resolución de artefactos del \( protocolo SAML lenguaje de marcado de aserción de seguridad \) \) .

En la tabla siguiente se proporciona un resumen del uso de una granja de servidores WID:

| 1-100 relaciones de confianza para usuario autenticado | Más de 100 relaciones de confianza para usuario autenticado |
|--|--|
| **1-30 nodos de AD FS:** Compatible con WID | **1-30 nodos de AD FS:** No compatible con WID, se requiere SQL |
| **Más de 30 nodos de AD FS:** No compatible con WID, se requiere SQL | **Más de 30 nodos de AD FS:** No compatible con WID, se requiere SQL |

### <a name="alwayson-availability-groups"></a>Grupos de disponibilidad AlwaysOn
**Información general**

Los grupos de disponibilidad AlwaysOn se introdujeron en SQL Server 2012 y proporcionan una nueva forma de crear una instancia de SQL Server de alta disponibilidad.Los grupos de disponibilidad AlwaysOn combinan elementos de agrupación en clústeres y creación de reflejo de base de datos para redundancia y conmutación por error en el nivel de instancia SQL y en el nivel de base de datos.A diferencia de las opciones de alta disponibilidad anteriores, los grupos de disponibilidad AlwaysOn no requieren una red de área de almacenamiento o almacenamiento común \( \) en el nivel de base de datos.

Un grupo de disponibilidad consta de una réplica principal \( un conjunto de bases de \- datos principales de lectura \) y de una a cuatro \( conjuntos de réplicas de disponibilidad de las bases de datos secundarias correspondientes \) .El grupo de disponibilidad admite una sola \- escritura \( de lectura y copia la réplica principal \) y de una a cuatro \- réplicas de disponibilidad de solo lectura.Cada réplica de disponibilidad debe residir en otro nodo de un único clúster de WSFC de clústeres de conmutación por error de Windows Server \( \) .Para obtener más información sobre los grupos de disponibilidad AlwaysOn, consulte [información general de Grupos de disponibilidad AlwaysOn \( SQL Server \) ](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-ver15).

Desde la perspectiva de los nodos de una AD FS SQL Server granja, el grupo de disponibilidad AlwaysOn reemplaza la instancia de SQL Server única como la \/ base de datos de artefactos de directiva.El agente de escucha del grupo de disponibilidad es el cliente que \( el servicio de token de seguridad de AD FS \) usa para conectarse a SQL.

En el diagrama siguiente se muestra un AD FS granja de SQL Server con el grupo de disponibilidad AlwaysOn.

![granja de servidores con SQL](media/alwaysonavailabilitygroups.jpg)

> [!NOTE]
> Los grupos de disponibilidad AlwaysOn requieren que las instancias de SQL Server residan en los nodos WSFC de clústeres de conmutación por error de Windows Server \( \) .

> [!NOTE]
> Solo una réplica de disponibilidad puede actuar como un destino de conmutación por error automática, las otras tres se basarán en las conmutaciones por error manuales.

**Consideraciones de implementación clave**

Si tiene previsto usar grupos de disponibilidad AlwaysOn en combinación con SQL Server la replicación de mezcla, tome nota de los problemas descritos en la sección "consideraciones de implementación clave para usar AD FS con la replicación de mezcla de SQL Server".En concreto, cuando un grupo de disponibilidad AlwaysOn que contiene una base de datos que es un suscriptor de replicación conmuta por error, se produce un error en la suscripción de replicación. Para reanudar la replicación, un administrador de replicación debe configurar manualmente el suscriptor.Vea la descripción SQL Server de un problema específico en los [suscriptores de replicación y Grupos de disponibilidad AlwaysOn \( \) SQL Server](/sql/database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server?view=sql-server-ver15) y las instrucciones de soporte general para los grupos de disponibilidad AlwaysOn con opciones de replicación en [replicación, Change Tracking, captura de datos modificados y \( \) ](/sql/database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability?view=sql-server-ver15)grupos de disponibilidad AlwaysOn SQL Server.

**Configuración de AD FS para usar un grupo de disponibilidad AlwaysOn**

La configuración de una granja de AD FS con grupos de disponibilidad AlwaysOn requiere una ligera modificación en el procedimiento de implementación de AD FS:

1. Las bases de datos de las que desea realizar una copia de seguridad deben crearse antes de que se puedan configurar los grupos de disponibilidad AlwaysOn.AD FS crea sus bases de datos como parte de la configuración y la configuración inicial del primer nodo de servicio de Federación de una nueva granja de servidores de SQL Server de AD FS.Como parte de la configuración de AD FS, debe especificar una cadena de conexión SQL, por lo que tendrá que configurar el primer nodo de la granja de AD FS para conectarse a una instancia de SQL directamente, \( solo es temporal \) . Para obtener instrucciones específicas sobre la configuración de una granja de AD FS, incluida la configuración de un nodo de la granja de AD FS con una cadena de conexión de SQL Server, vea [configurar un servidor de Federación](../../ad-fs/deployment/Configure-a-Federation-Server.md).

2. Una vez que se han creado las bases de datos de AD FS, asígnelas a grupos de disponibilidad AlwaysOn y cree el agente de escucha de TCPIP común con SQL Server herramientas y proceso en la [creación y configuración de grupos de disponibilidad \( SQL Server \) ](/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-ver15).

3. Por último, use PowerShell para editar las propiedades de AD FS para actualizar la cadena de conexión de SQL para usar la dirección DNS del agente de escucha del grupo de disponibilidad AlwaysOn.

    Ejemplos de comandos de PSH para actualizar la cadena de conexión de SQL para la base de datos de configuración de AD FS:

    ```
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService
    PS:\>$temp.ConfigurationdatabaseConnectionstring="data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true"
    PS:\>$temp.put()

    ```

4. Ejemplos de comandos de PSH para actualizar la cadena de conexión de SQL para la base de datos del servicio de resolución de artefactos de AD FS:

    ```
    PS:\> Set-AdfsProperties –artifactdbconnection "Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True"
    ```

### <a name="sql-server-merge-replication"></a>SQL Server la replicación de mezcla
También se presentó en SQL Server 2012, la replicación de mezcla permite la redundancia de datos de la Directiva de AD FS con las siguientes características:

- Capacidad de lectura y escritura en todos los nodos, \( no solo en el principal\)

- Cantidades más pequeñas de datos replicados de forma asincrónica para evitar la introducción de latencia en el sistema

En el diagrama siguiente se muestra una AD FS redundante geográficamente SQL Server granjas con la replicación de mezcla \( 1 publicador, 2 suscriptores \) :

![granja de servidores con SQL](media/ADFSSQLGeoRedundancy3.png)

**Consideraciones de implementación clave para usar AD FS con SQL Server \( números de nota de replicación de mezcla en el diagrama anterior\)**

- La base de datos del distribuidor no se admite para su uso con Grupos de disponibilidad AlwaysOn o la creación de reflejo de la base de datos.Consulte SQL Server instrucciones de compatibilidad para grupos de disponibilidad AlwaysOn con opciones de replicación en [replicación, Change Tracking, captura de datos modificados y Grupos de disponibilidad AlwaysOn \( SQL Server \) ](/sql/database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability?view=sql-server-ver15).

- Cuando el grupo de disponibilidad AlwaysOn que contiene la base de datos que es un suscriptor de replicación realiza una conmutación por error, se produce un error en la suscripción de replicación. Para reanudar la replicación, un administrador de replicación debe configurar manualmente el suscriptor.Vea el SQL Server Descripción de un problema específico en los [suscriptores de replicación y Grupos de disponibilidad AlwaysOn \( \) SQL Server](/sql/database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server?view=sql-server-ver15) y las instrucciones de soporte técnico generales para los grupos de disponibilidad AlwaysOn con las opciones de replicación [replicación, Change Tracking, captura de datos modificados y \( \) ](/sql/database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability?view=sql-server-ver15)grupos de disponibilidad AlwaysOn SQL Server.

Para obtener instrucciones más detalladas sobre cómo configurar AD FS para usar una replicación de mezcla de SQL Server, consulte Configuración de la [redundancia geográfica con replicación de SQL Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn632406(v=ws.11)).

## <a name="see-also"></a>Consulte también
[Planeación de la topología](Plan-Your-AD-FS-Deployment-Topology.md) 
 de implementación de AD FS [Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)

