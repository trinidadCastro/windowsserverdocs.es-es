---
title: Configuración de una implementación de AD FS con Grupos de disponibilidad AlwaysOn
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/20/2020
ms.topic: article
ms.openlocfilehash: fac086cdcc08cb8941eba1dda9f4c41058d1b5d2
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865074"
---
# <a name="setting-up-an-ad-fs-deployment-with-alwayson-availability-groups"></a>Configuración de una implementación de AD FS con Grupos de disponibilidad AlwaysOn
Una topología de distribución geográfica de alta disponibilidad proporciona:
* Eliminación de un único punto de error: con las capacidades de conmutación por error, puede lograr una infraestructura ADFS de alta disponibilidad incluso si uno de los centros de datos de una parte de un mundo deja de funcionar.
* Rendimiento mejorado: puede usar la implementación sugerida para proporcionar una infraestructura de ADFS de alto rendimiento.

AD FS se pueden configurar para un escenario de distribución geográfica de alta disponibilidad.
En la guía siguiente se le guiará a través de una visión general de AD FS con grupos de disponibilidad AlwaysOn de SQL y proporcionar instrucciones y consideraciones de implementación.

## <a name="overview---alwayson-availability-groups"></a>Información general: Grupos de disponibilidad AlwaysOn

Para obtener más información sobre los grupos de disponibilidad AlwaysOn, consulte [información general de grupos de disponibilidad AlwaysOn (SQL Server)](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) .

Desde la perspectiva de los nodos de una granja de servidores de AD FS SQL Server, el grupo de disponibilidad AlwaysOn reemplaza la instancia de SQL Server única como base de datos de directivas/artefactos.El agente de escucha del grupo de disponibilidad es lo que el cliente (el servicio de token de seguridad de AD FS) utiliza para conectarse a SQL.
En el diagrama siguiente se muestra un AD FS granja de SQL Server con el grupo de disponibilidad AlwaysOn.

![granja de servidores con SQL](media/ad-fs-always-on/SQLoverview.png)

Un grupo de disponibilidad de Always On (AG) es una o varias bases de datos de usuario que conmutan por error juntas. Un grupo de disponibilidad consta de una réplica de disponibilidad principal y de una a cuatro réplicas secundarias que se mantienen a través de SQL Server movimiento de datos basado en registro para la protección de datos sin necesidad de almacenamiento compartido. Cada réplica está hospedada en una instancia de SQL Server en un nodo diferente del WSFC. El grupo de disponibilidad y un nombre de red virtual correspondiente se registran como recursos del clúster de WSFC.

Un agente de escucha del grupo de disponibilidad en el nodo de la réplica principal responde a las solicitudes de cliente entrantes para conectarse al nombre de red virtual y, en función de los atributos de la cadena de conexión, redirige cada solicitud a la instancia de SQL Server adecuada.
En el caso de una conmutación por error, en lugar de transferir la propiedad de los recursos físicos compartidos a otro nodo, WSFC se aprovecha para volver a configurar una réplica secundaria en otra instancia de SQL Server para que se convierta en la réplica principal del grupo de disponibilidad. El recurso de nombre de red virtual del grupo de disponibilidad se transfiere después a esa instancia.
En un momento dado, solo una única instancia de SQL Server puede hospedar la réplica principal de las bases de datos de un grupo de disponibilidad, todas las réplicas secundarias asociadas deben residir en una instancia independiente, y cada instancia debe residir en nodos físicos independientes.

> [!NOTE]
> Si las máquinas se ejecutan en Azure, configure las máquinas virtuales de Azure para permitir que la configuración del agente de escucha se comunique con los grupos de disponibilidad AlwaysOn. Para obtener más información, [Virtual Machines: agente de escucha de SQL Always on](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener).

Para obtener información general adicional sobre Grupos de disponibilidad AlwaysOn, consulte [información general de los grupos de disponibilidad de Always On (SQL Server)](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server).

> [!NOTE]
> Si la organización requiere conmutación por error en varios centros de datos, se recomienda crear una base de datos de artefactos en cada centro de datos, así como habilitar una caché en segundo plano, lo que reduce la latencia durante el procesamiento de la solicitud. Siga las instrucciones para hacerlo en [ajustar SQL y reducir la latencia](./adfs-sql-latency.md).

## <a name="deployment-guidance"></a>Guía de implementación

1. <b> Tenga en cuenta la base de datos correcta para los objetivos de la implementación de AD FS. </b> AD FS utiliza una base de datos para almacenar la configuración y, en algunos casos, los datos transaccionales relacionados con la Servicio de federación. Puede usar AD FS software para seleccionar la versión integrada de Windows Internal Database (WID) o Microsoft SQL Server 2008 o una más reciente para almacenar los datos en el servicio de Federación.
En la tabla siguiente se describen las diferencias en las características admitidas entre WID y SQL Database.


| Category      | Característica       | Compatible con WID  | Compatible con SQL |
| ------------------ |:-------------:| :---:|:---: |
| AD FS características     | Implementación de una granja de servidores de federación | Sí  | Sí |
| AD FS características     | Resolución de artefactos SAML. Nota: esto no es común para las aplicaciones SAML     |   No | Sí  |
| AD FS características | Detección de reproducción de tokens SAML/WS-Federation. Nota: solo se requiere cuando AD FS recibe tokens de IDP externos. Esto no es necesario si AD FS no actúa como asociado de Federación.      |    No  | Sí |
| Características de la base de datos     |   Redundancia básica de bases de datos mediante la replicación de extracción, donde uno o más servidores que hospedan una copia de solo lectura de la solicitud de base de datos los cambios realizados en un servidor de origen hospedan una copia de lectura/escritura de la base de datos    |   No | No  |
| Características de la base de datos | Redundancia de bases de datos mediante soluciones de alta disponibilidad, como la agrupación en clústeres o la creación de reflejo (en el nivel de base de datos)      |    No  | Sí |
| Características adicionales | Escenario fases de OAuth     |   Sí  | Sí |

Si es una organización grande con más de 100 relaciones de confianza que necesitan proporcionar a los usuarios internos y a los usuarios externos el acceso de inicio de sesión único a servicios o aplicaciones de Federación, SQL es la opción recomendada.

Si es una organización con 100 o menos relaciones de confianza configuradas, WID proporciona redundancia de datos y servicios de Federación (donde cada servidor de Federación replica los cambios en otros servidores de Federación de la misma granja). WID no admite la detección de reproducción de tokens ni la resolución de artefactos y tiene un límite de 30 servidores de Federación.
Para obtener más información sobre cómo planear la implementación, visite [aquí](../design/planning-your-deployment.md).

## <a name="sql-server-high-availability-solutions"></a>SQL Server soluciones de alta disponibilidad
Si usa SQL Server como su AD FS base de datos de configuración, puede configurar la redundancia geográfica para la granja de AD FS mediante la replicación de SQL Server. La redundancia geográfica replica los datos entre dos sitios alejados geográficamente para que las aplicaciones puedan cambiar de un sitio a otro. De este modo, en caso de que se produzca un error en un sitio, todavía puede tener todos los datos de configuración disponibles en el segundo sitio. 
Si SQL es la base de datos adecuada para los objetivos de implementación, continúe con esta guía de implementación.

Esta guía le guiará a través de lo siguiente
* Implementar AD FS
* Configurar AD FS para usar una Grupos de disponibilidad AlwaysOn
* Instalar el rol de clúster de conmutación por error
* Ejecutar pruebas de validación de clústeres
* Habilitar grupos de disponibilidad de Always On
* Copia de seguridad de bases de datos de AD FS
* Crear grupos de disponibilidad AlwaysOn
* Agregar bases de datos en el segundo nodo
* Unir una réplica de disponibilidad a un grupo de disponibilidad
* Actualización de la cadena de conexión SQL

## <a name="deploy-ad-fs"></a>Implementar AD FS

> [!NOTE]
> Si las máquinas se ejecutan en Azure, el Virtual Machines se debe configurar de una manera específica para permitir que el agente de escucha se comunique con el grupo de disponibilidad Always On. Para más información sobre la configuración, consulte Configuración de [un equilibrador de carga para un grupo de disponibilidad en máquinas virtuales de Azure SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)


En esta guía de implementación se muestra una granja de dos nodos con dos servidores SQL Server como ejemplo.
Para implementar AD FS siga los vínculos iniciales siguientes para instalar el servicio de rol AD FS. Para configurar un grupo de AoA, habrá pasos adicionales para el rol.
-   [Unir un equipo a un dominio](../deployment/join-a-computer-to-a-domain.md)
-   [Inscribir un certificado SSL de AD FS](../deployment/enroll-an-ssl-certificate-for-ad-fs.md)
-   [Instalar el servicio de rol de AD FS](../deployment/install-the-ad-fs-role-service.md)


## <a name="configuring-ad-fs-to-use-an-alwayson-availability-group"></a>Configuración de AD FS para usar un grupo de disponibilidad AlwaysOn

La configuración de una granja de AD FS con grupos de disponibilidad AlwaysOn requiere una ligera modificación en el procedimiento de implementación de AD FS. Asegúrese de que cada instancia de servidor ejecuta la misma versión de SQL. Para ver la lista completa de requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad Always On, lea [aquí](/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-2017#PrerequisitesForDbs).

1.  Las bases de datos de las que desea realizar una copia de seguridad deben crearse antes de que se puedan configurar los grupos de disponibilidad AlwaysOn.  AD FS crea sus bases de datos como parte de la configuración y la configuración inicial del primer nodo de servicio de Federación de una nueva granja de servidores de SQL Server de AD FS.  Especifique el nombre de host de base de datos de la granja existente mediante SQL Server. Como parte de la configuración de AD FS, debe especificar una cadena de conexión SQL, por lo que tendrá que configurar la primera granja de AD FS para conectarse directamente a una instancia de SQL (esto es solo temporal). Para obtener instrucciones específicas sobre la configuración de una granja de AD FS, incluida la configuración de un nodo de la granja de AD FS con una cadena de conexión de SQL Server, vea [configurar un servidor de Federación](../deployment/configure-a-federation-server.md).

![especificar granja de servidores](media/ad-fs-always-on/deploymentSpecifyFarm.png)

2.  Compruebe la conectividad con la base de datos mediante SSMS y conéctese al nombre de host de la base de datos de destino. Si agrega otro nodo a la granja de servidores de Federación, conéctese a la base de datos de destino.
3.  Especifique el certificado SSL para la granja de AD FS.

![especificar certificado SSL](media/ad-fs-always-on/deploymentSpecifySSL.png)

4.  Conecte la granja de servidores a una cuenta de servicio o gMSA.

![especificar cuenta de servicio](media/ad-fs-always-on/deploymentSpecifyServiceAccount.png)

5.  Complete la instalación y configuración de la granja de AD FS.

> [!NOTE]
> SQL Server debe ejecutarse en una cuenta de dominio para la instalación de Always On grupos de disponibilidad. De forma predeterminada, se ejecuta como sistema local.

## <a name="install-the-failover-clustering-role"></a>Instalar el rol de clúster de conmutación por error
El rol de clúster de conmutación por error de Windows Server proporciona el para obtener más información sobre los clústeres de conmutación por error de Windows Server.
1.  Inicie el Administrador del servidor.
2.  En el menú administrar, seleccione Agregar roles y características.
3.  En la página antes de comenzar, seleccione siguiente.
4.  En la página Seleccionar tipo de instalación, seleccione Instalación basada en características o en roles y, a continuación, seleccione siguiente.
5.  En la página Seleccionar servidor de destino, seleccione el servidor SQL Server en el que desea instalar la característica y, a continuación, seleccione siguiente.

![servidores de destino](media/ad-fs-always-on/clusteringDestinationServer.png)

6.  En la pantalla Seleccionar roles de servidor, selecciona Siguiente.
7.  En la página Seleccionar características, activa la casilla Clúster de conmutación por error.

![selección de la característica de agrupación en clústeres](media/ad-fs-always-on/clusteringFeature.png)

8.  En la página confirmar selecciones de instalación, seleccione instalar.
La característica de clústeres de conmutación por error no requiere reiniciar el servidor.
9.  Una vez completada la instalación, seleccione cerrar.
10. Repite este procedimiento en todos los servidores que desees incluir como nodo de clúster de conmutación por error.

## <a name="run-cluster-validation-tests"></a>Ejecutar pruebas de validación de clústeres
1.  En un equipo que tenga las Herramientas de administración de clústeres de conmutación por error instaladas desde las Herramientas de administración remota del servidor, o en un servidor donde hayas instalado la característica de clústeres de conmutación por error, inicia el Administrador de clústeres de conmutación por error. Para hacer esto en un servidor, inicie Administrador del servidor y, a continuación, en el menú herramientas, seleccione Administrador de clústeres de conmutación por error.
2.  En el panel Administrador de clústeres de conmutación por error, en administración, seleccione validar configuración.
3.  En la página Antes de empezar, seleccione Siguiente.
4.  En la página seleccionar servidores o un clúster, en el cuadro escribir nombre, escriba el nombre NetBIOS o el nombre de dominio completo de un servidor que vaya a agregar como nodo de clúster de conmutación por error y, a continuación, seleccione Agregar. Repite este paso para todos los servidores que quieras agregar. Para agregar varios servidores al mismo tiempo, separa los nombres con una coma o un punto y coma. Por ejemplo, escribe los nombres con el formato server1.contoso.com, server2.contoso.com. Cuando haya terminado, seleccione Siguiente.

![seleccionar la imagen de los servidores](media/ad-fs-always-on/clusterValidationServers.png)

5. En la página Opciones de prueba, seleccione ejecutar todas las pruebas (recomendado) y, a continuación, seleccione siguiente.
6. En la página Confirmación, seleccione siguiente.
La página Validación muestra el estado de las pruebas en ejecución.
7. En la página Resumen, realiza una de las acciones siguientes:
- Si los resultados indican que las pruebas se completaron correctamente y la configuración es adecuada para la agrupación en clústeres y desea crear el clúster inmediatamente, asegúrese de que la casilla crear el clúster ahora con los nodos validados esté activada y, a continuación, seleccione Finalizar. Después, continúe con el paso 4 del [procedimiento crear el clúster de conmutación por error](../../../failover-clustering/create-failover-cluster.md#create-the-failover-cluster).

![imagen de configuración de validación](media/ad-fs-always-on/clusterValidationResults.png)

-   Si los resultados indican que había advertencias o errores, seleccione Ver informe para ver los detalles y determinar qué problemas deben corregirse. Ten en cuenta que una advertencia sobre una prueba de validación en particular indica que ese aspecto del clúster de conmutación por error puede ser compatible, pero podría no cumplir los procedimientos recomendados.

> [!NOTE]
> Si recibes una advertencia sobre la prueba Validar reserva persistente de espacios de almacenamiento, consulta la entrada de blog [La advertencia sobre la validación de un clúster de conmutación por error de Windows indica que los discos no son compatibles con las reservas persistentes de espacios de almacenamiento](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) para obtener más información.
> Para obtener más información acerca de las pruebas de validación de hardware, consulta [Validación de hardware para un clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)).

## <a name="create-the-failover-cluster"></a>Creación del clúster de conmutación por error

Para completar este paso, asegúrate de que la cuenta de usuario con la que inicias sesión cumpla los requisitos indicados en la sección [Comprobar los requisitos previos](../../../failover-clustering/create-failover-cluster.md#verify-the-prerequisites) de este tema.
1.  Inicie el Administrador del servidor.
2.  En el menú herramientas, seleccione Administrador de clústeres de conmutación por error.
3.  En el panel Administrador de clústeres de conmutación por error, en administración, seleccione crear clúster.
Se abre el Asistente para crear clúster.
4.  En la página Antes de empezar, seleccione Siguiente.
5.  Si aparece la página seleccionar servidores, en el cuadro escribir nombre, escriba el nombre NetBIOS o el nombre de dominio completo de un servidor que vaya a agregar como nodo de clúster de conmutación por error y, a continuación, seleccione Agregar. Repite este paso para todos los servidores que quieras agregar. Para agregar varios servidores al mismo tiempo, separa los nombres con una coma o un punto y coma. Por ejemplo, escribe los nombres con el formato server1.contoso.com; server2.contoso.com. Cuando haya terminado, seleccione Siguiente.

![crear clúster y seleccionar servidores](media/ad-fs-always-on/createClusterServers.png)

> [!NOTE]
> Si decide crear el clúster inmediatamente después de ejecutar la validación en el [procedimiento de validación](../../../failover-clustering/create-failover-cluster.md#validate-the-configuration)de la configuración, no verá la página seleccionar servidores. Los nodos que se validaron se agregan automáticamente al Asistente para crear clúster, de modo que no tengas que introducirlos de nuevo.

6.  Si antes omitiste la validación, aparecerá la página Advertencia de validación. Recomendamos encarecidamente ejecutar la validación de clústeres. Solo los clústeres que superan todas las pruebas de validación son compatibles con Microsoft. Para ejecutar las pruebas de validación, seleccione sí y, a continuación, seleccione siguiente. Complete el Asistente para validar una configuración tal y como se describe en [validación de la configuración](../../../failover-clustering/create-failover-cluster.md#validate-the-configuration).
7.  En la página Punto de acceso para administrar el clúster, haz lo siguiente:
-   En el cuadro Nombre de clúster, escribe el nombre que deseas utilizar para administrar el clúster. Antes de hacerlo, revisa la siguiente información:
 -  Durante la creación del clúster, este nombre se registra como el objeto de equipo de clúster (también llamado objeto de nombre de clúster o CNO) en AD DS. Si especificas un nombre NetBIOS para el clúster, el CNO se crea en la misma ubicación en la que residen los objetos de equipo para los nodos de clúster. Puede ser el contenedor Equipos predeterminado o una OU.
 -  Para especificar una ubicación diferente para el CNO, escribe el nombre distintivo de una OU en el cuadro Nombre de clúster. Por ejemplo: CN=ClusterName, OU=Clusters, DC=Contoso, DC=com.
 -  Si un administrador de dominio ha preconfigurado el CNO en una OU diferente de donde residen los nodos de clúster, especifica el nombre distintivo que te proporcione el administrador de dominio.
- Si el servidor no tiene ningún adaptador de red configurado para usar DHCP, debes configurar una o varias direcciones IP estáticas para el clúster de conmutación por error. Selecciona la casilla situada junto a cada red que desees utilizar para la administración de clústeres. Seleccione el campo Dirección situado junto a una red seleccionada y, a continuación, escriba la dirección IP que desea asignar al clúster. Esta dirección o direcciones IP se asociarán al nombre del clúster en el Sistema de nombres de dominio (DNS).
- Cuando haya terminado, seleccione Siguiente.

8.  En la página Confirmación, revisa la configuración. De manera predeterminada, la casilla Agregar todo el almacenamiento apto al clúster está seleccionada. Desactiva esta casilla si deseas realizar una de las siguientes acciones:
-   Deseas configurar el almacenamiento más adelante.
-   Tienes previsto crear espacios de almacenamiento en clúster mediante el Administrador de clústeres de conmutación por error o mediante los cmdlets de Windows PowerShell de clústeres de conmutación por error, y todavía no has creado espacios de almacenamiento en los Servicios de archivos y almacenamiento. Para obtener más información, consulta [Implementar espacios de almacenamiento en clúster](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)).
9.  Seleccione siguiente para crear el clúster de conmutación por error.
10. En la página Resumen, confirma que el clúster de conmutación por error se ha creado correctamente. Si hay advertencias o errores, vea la salida de resumen o seleccione Ver informe para ver el informe completo. Seleccione Finalizar.
11. Para confirmar que se ha creado el clúster, comprueba que su nombre aparece en el Administrador de clústeres de conmutación por error, en el árbol de navegación. Puede expandir el nombre del clúster y seleccionar elementos en nodos, almacenamiento o redes para ver los recursos asociados.
Ten en cuenta que el nombre del clúster podría tardar un poco en replicarse correctamente en DNS. Después de que el registro y la replicación de DNS se hayan realizado correctamente, si selecciona todos los servidores en Administrador del servidor, el nombre del clúster debe aparecer como servidor con el estado de manejabilidad de en línea.

![creación del clúster completa](media/ad-fs-always-on/createClusterComplete.png)

## <a name="enable-always-on-availability-groups-with-sql-server-configuration-manager"></a>Habilitar grupos de disponibilidad AlwaysOn con Administrador de configuración de SQL Server

1.  Conéctese al nodo de clúster de conmutación por error de Windows Server (WSFC) que hospeda la instancia de SQL Server en la que desea habilitar Always On grupos de disponibilidad.
2.  En el menú Inicio, seleccione todos los programas, Microsoft SQL Server, Herramientas de configuración y haga clic en Administrador de configuración de SQL Server.
3.  En Administrador de configuración de SQL Server, haga clic en servicios de SQL Server, haga clic con el botón secundario en SQL Server ( <instance name> ), donde <instance name> es el nombre de una instancia del servidor local para la que desea habilitar los grupos de disponibilidad Always on y haga clic en propiedades.
4.  Seleccione la pestaña Alta disponibilidad de AlwaysOn.
5.  Compruebe que el campo Nombre del clúster de conmutación por error de Windows contiene el nombre del clúster de conmutación por error local. Si este campo está en blanco, esta instancia del servidor no admite actualmente Grupos de disponibilidad Always On. Es posible que el equipo local no sea un nodo de clúster, que se haya cerrado el clúster de WSFC o que esta edición de SQL Server no admita Grupos de disponibilidad Always On.
6.  Active la casilla Habilitar los grupos de disponibilidad AlwaysOn y haga clic en Aceptar.
El Administrador de configuración de SQL Server guarda el cambio. A continuación, debe reiniciar manualmente el servicio SQL Server. Esto le permite elegir una hora de reinicio que sea la mejor para sus requisitos empresariales. Cuando se reinicie el servicio SQL Server, se habilitará Always On y la propiedad del servidor IsHadrEnabled se establecerá en 1.

![habilitar AoA](media/ad-fs-always-on/enableAoAGroup.png)

## <a name="back-up-ad-fs-databases"></a>Copia de seguridad de bases de datos de AD FS
Realice una copia de seguridad de la configuración de AD FS y de las bases de datos de artefacto con los registros de transacciones completos. Coloque la copia de seguridad en el destino elegido.
Realice una copia de seguridad del artefacto de ADFS y de las bases de datos de configuración.
- Tareas > copia de seguridad > completa > agregar a un archivo de copia de seguridad > aceptar para crear

![copia de seguridad del servidor](media/ad-fs-always-on/backUpADFS.png)

## <a name="create-new-availability-group"></a>Crear nuevo grupo de disponibilidad

1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal.
2.  Expanda los nodos Alta disponibilidad de AlwaysOn y Grupos de disponibilidad .
3.  Para iniciar el Asistente para nuevo grupo de disponibilidad, seleccione el comando Asistente para nuevo grupo de disponibilidad.
4.  La primera vez que se ejecuta este asistente, aparece una página Introducción. Para omitir esta página en el futuro, puede hacer clic en No volver a mostrar esta página. Después de leer esta página, haga clic en Siguiente.
5.  En la página Especificar opciones de grupo de disponibilidad, escriba el nombre del nuevo grupo de disponibilidad en el campo Nombre de grupo de disponibilidad. Este nombre debe ser un identificador de SQL Server válido que sea único en el clúster y en el dominio en su totalidad. La longitud máxima del nombre de un grupo de disponibilidad es 128 caracteres. e
6.  Después, especifique el tipo de clúster. Los tipos de clúster posibles dependen de la versión de SQL Server y del sistema operativo. Elija WSFC, EXTERNAL o NONE. Para obtener información detallada, consulte [especificar el nombre del grupo de disponibilidad](/sql/database-engine/availability-groups/windows/specify-availability-group-name-page) .

![nombre del grupo y el clúster de AoA](media/ad-fs-always-on/createAoAName.png)

7.  En la página Seleccionar bases de datos, la cuadrícula enumera las bases de datos de la instancia del servidor conectado que se pueden convertir en bases de datos de disponibilidad. Seleccione una o varias de las bases de datos enumeradas para participar como bases de datos de disponibilidad en el nuevo grupo de disponibilidad. Estas bases de datos serán inicialmente las bases de datos principales iniciales.
Para cada base de datos de la lista, la columna Tamaño muestra el tamaño de la base de datos, si se conoce. La columna Estado indica si una base de datos determinada cumple los [requisitos previos](/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability) para las bases de datos de disponibilidad. Si los requisitos previos no se cumplen, una breve descripción de estado indica el motivo por el que la base de datos no es apta; por ejemplo, si no utiliza el modelo de recuperación completa. Para obtener más información, haga clic en la descripción del estado.
Si cambia una base de datos para que pueda ser apta, haga clic en Actualizar para actualizar la cuadrícula de bases de datos.
Si la base de datos contiene una clave maestra de base de datos, escriba la contraseña para dicha clave en la columna Contraseña.

![seleccionar bases de datos para AoA](media/ad-fs-always-on/createAoASelectDb.png)

8. en la página especificar réplicas, especifique y configure una o varias réplicas para el nuevo grupo de disponibilidad. Esta página contiene cuatro pestañas: En la siguiente tabla se presentan estas pestañas. Para obtener más información, consulte la [página especificar réplicas (Asistente para nuevo grupo de disponibilidad: Asistente para agregar réplica)](/sql/database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard) .

| Pestaña      | Breve descripción       |
| ------------------ |:-------------:|
| Réplicas     | Use esta pestaña para especificar cada instancia de SQL Server que hospedará una réplica secundaria. Tenga en cuenta que la instancia de servidor a la que está conectado actualmente debe hospedar la réplica principal. |
| Puntos de conexión     | Utilice esta pestaña para comprobar los extremos de creación de reflejo de base de datos existentes y también, si este extremo falta en una instancia de servidor cuyas cuentas de servicio utilizan la autenticación de Windows, para crear el extremo automáticamente.|
| Preferencias de copia de seguridad | Utilice esta pestaña para especificar sus preferencias de copias de seguridad para el grupo de disponibilidad en conjunto y las prioridades de copias de seguridad para las réplicas de disponibilidad individuales.      |
| Agente de escucha     | Utilice esta pestaña para crear un agente de escucha del grupo de disponibilidad. De forma predeterminada, el asistente no crea un agente de escucha.      |

![especificar detalles de la réplica](media/ad-fs-always-on/createAoAchooseReplica.png)

9. En la página Seleccionar sincronización de datos iniciales, elija cómo desea que las nuevas bases de datos secundarias se creen y se unan al grupo de disponibilidad. Elija una de las siguientes opciones:
-   Propagación automática
 - SQL Server crea automáticamente las réplicas secundarias de cada base de datos del grupo. La propagación automática requiere que la ruta de acceso del archivo de datos y de registro sea la misma en cada instancia de SQL Server que participe en el grupo. Disponible en SQL Server 2016 (13. x) y versiones posteriores. Consulte [inicializar automáticamente Always on grupos de disponibilidad](/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group).
- Copia de seguridad completa de registros y bases de datos
 - Seleccione esta opción si su entorno cumple los requisitos para iniciar automáticamente la sincronización de datos iniciales (para obtener más información, vea [requisitos previos, restricciones y recomendaciones, anteriormente en este tema)](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio#Prerequisites).
Si selecciona Completa, después de crear el grupo de disponibilidad, el asistente realizará una copia de seguridad de cada base de datos principal y su registro de transacciones a un recurso compartido de red y restaurará las copias de seguridad en cada instancia de servidor que hospeda una réplica secundaria. El asistente unirá entonces cada base de datos secundaria al grupo de disponibilidad.
En el campo Especificar una ubicación de red compartida accesible por todas las réplicas:, indique un recurso compartido de copia de seguridad al que todas las instancias del servidor que hospedan réplicas tienen acceso de lectura y escritura. Para obtener más información, vea Requisitos previos, anteriormente en este tema. En el paso de validación, el asistente realizará una prueba para asegurarse de que la ubicación de red proporcionada es válida, la prueba creará una base de datos en la réplica principal, denominada "BackupLocDb_" seguido de un GUID, realizará una copia de seguridad en la ubicación de red proporcionada y después la restaurará en las réplicas secundarias. Se puede eliminar esta base de datos junto con su historial de copias de seguridad y el archivo de copia de seguridad en caso de que el asistente no pueda eliminarlos.
- Solo unión
 - Si ha preparado manualmente las bases de datos secundarias de las instancias de servidor que hospedarán las réplicas secundarias, puede seleccionar esta opción. El asistente unirá las bases de datos secundarias existentes al grupo de disponibilidad.
- Omitir la sincronización de datos iniciales
 - Seleccione esta opción si desea usar sus propias bases de datos y copias de seguridad de registros de sus bases de datos principales. Para obtener más información, vea [iniciar el movimiento de datos en una base de datos secundaria Always On (SQL Server)](/sql/database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server).

![elegir la opción de sincronización de datos](media/ad-fs-always-on/createAoADataSync.png)

9.  La página Validación comprueba si los valores especificados en este asistente cumplen los requisitos del Asistente para nuevo grupo de disponibilidad. Para realizar un cambio, haga clic en Anterior para volver a una página anterior del asistente con el fin de cambiar uno o varios valores. Haga clic en Siguiente para volver a la página Validación y haga clic en Volver a ejecutar la validación.

10. En la página Resumen, revise las opciones para el nuevo grupo de disponibilidad. Para realizar un cambio, haga clic en Anterior para volver a la página correspondiente. Después de realizar el cambio, haga clic en Siguiente para volver a la página Resumen.

> [!NOTE]
> Cuando la cuenta de servicio de SQL Server de una instancia de servidor que hospedará una nueva réplica de disponibilidad no existe ya como inicio de sesión, el Asistente para nuevo grupo de disponibilidad debe crear el inicio de sesión. En la página Resumen, el asistente muestra la información del inicio de sesión que se va a crear. Si hace clic en Finalizar, el asistente creará este inicio de sesión para la cuenta de servicio de SQL Server y le concederá el permiso CONNECT.
> Si está satisfecho con las selecciones, puede hacer si lo desea en Script para crear un script de los pasos que ejecutará el asistente. A continuación, para crear y configurar el nuevo grupo de disponibilidad, haga clic en Finalizar.

11. La página Progreso muestra el progreso de los pasos necesarios para crear el grupo de disponibilidad (configuración de extremos, creación del grupo de disponibilidad y unión de la réplica secundaria al grupo).
12. Según se completen estos pasos, la página Resultados mostrará el resultado de cada paso. Si todos estos pasos se realizan correctamente, el nuevo grupo de disponibilidad se configura por completo. Si cualquiera de los pasos produce un error, quizás necesite completar manualmente la configuración o usar un asistente para el paso con error. Para obtener información sobre la causa de un error determinado, haga clic en el vínculo “Error” asociado de la columna Resultado.
Una vez completado el asistente, haga clic en Cerrar para salir.

![validación completada](media/ad-fs-always-on/createAoAValidation.png)

## <a name="add-databases-on-secondary-node"></a>Agregar bases de datos en el nodo secundario

1.  Restaure la base de datos de artefactos a través de la interfaz de usuario del nodo secundario mediante los archivos de copia de seguridad creados.
![restaurar a través de la interfaz de usuario](media/ad-fs-always-on/restoreDB.png)

2. Restaure la base de datos en un estado de no recuperación.
![restauración con no recuperación](media/ad-fs-always-on/restoreNonRecovery.png)

3. Repita el proceso para restaurar la base de datos de configuración.

## <a name="join-availability-replica-to-an-availability-group"></a>Unir una réplica de disponibilidad a un grupo de disponibilidad

1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica secundaria y haga clic en el nombre del servidor para expandir el árbol de servidores.
2.  Expanda los nodos Alta disponibilidad de AlwaysOn y Grupos de disponibilidad .
3.  Seleccione el grupo de disponibilidad de la réplica secundaria a la que está conectado.
4.  Haga clic con el botón secundario en la réplica secundaria y haga clic en Combinar con grupo de disponibilidad.
5.  Se abrirá el cuadro de diálogo Combinar réplica con grupo de disponibilidad.
6.  Para combinar la réplica secundaria con el grupo de disponibilidad, haga clic en Aceptar.

![unir réplica secundaria](media/ad-fs-always-on/jointoAoA.png)

## <a name="update-the-sql-connection-string"></a>Actualización de la cadena de conexión SQL
Por último, use PowerShell para editar las propiedades de AD FS para actualizar la cadena de conexión de SQL para usar la dirección DNS del agente de escucha del grupo de disponibilidad AlwaysOn.
Ejecute el cambio de la base de datos de configuración en cada nodo y reinicie el servicio ADFS en todos los nodos de ADFS. El valor del catálogo inicial cambia en función de la versión de la granja.

```
PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService
PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”
PS:\>$temp.put()
PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”
```
