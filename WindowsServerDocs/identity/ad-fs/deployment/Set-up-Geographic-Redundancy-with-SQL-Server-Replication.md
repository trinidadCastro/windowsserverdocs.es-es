---
title: El programa de instalación de redundancia geográfica con replicación de SQL Server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: cf3d7513dd02bdb578bffd2f3ef8bdb29d8f983d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192171"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>El programa de instalación de redundancia geográfica con replicación de SQL Server


> [!IMPORTANT]  
> Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 o posterior.
  
Si usas SQL Server como la base de datos de configuración de AD FS, puede configurar geo\-redundancia para la granja de AD FS mediante la replicación de SQL Server. Geográfica\-redundancia replica datos entre dos sitios geográficamente distantes, para que las aplicaciones se pueden cambiar de un sitio a otro. De este modo, en caso de error de un sitio, puede todavía tiene todos los datos de configuración disponibles en el segundo sitio. Para obtener más información, consulte la sección"SQL Server redundancia geográfica" en [granja de servidores de federación Server utilizando SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Requisitos previos  
Instalar y configurar una granja de servidores SQL server. Para obtener más información, consulte [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). En el servidor SQL inicial, asegúrese de que se está ejecutando el servicio Agente SQL Server y configurados para el inicio automático.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Cree el segundo \(réplica\) SQL Server para geo\-redundancia  
  
1.  Instalar SQL Server \(para obtener más información, consulte [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copie los archivos de script CreateDB.sql y SetPermissions.sql resultantes en el servidor de réplica de SQL.  
  
2.  Asegúrese de que se está ejecutando el servicio Agente SQL Server y configurados para el inicio automático  
  
3.  Ejecute **exportar\-AdfsDeploymentSQLScript** en el nodo principal de AD FS para crear archivos CreateDB.sql y SetPermissions.sql.  Por ejemplo:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  Copie los scripts en el servidor secundario.  Abra el script CreateDB.sql en **SQL Management Studio** y haga clic en **Execute**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. Abra el script SetPermissions.sql en **SQL Management Studio** y haga clic en **Execute**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>También puede usar lo siguiente desde la línea de comandos. 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Crear la configuración del publicador en el servidor de SQL inicial  
  
1.  Desde SQL Server Management studio, en **replicación**, haga clic en **publicaciones locales** y elija **nueva publicación... ** 
 ![Configurar redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  En la pantalla del Asistente para nueva publicación, haga clic en **siguiente**.</br>
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  En **distribuidor** página, elija el servidor local como distribuidor y haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  En el **instantánea** carpeta, escriba \\\SQL1\repldata en lugar de la carpeta predeterminada. \(NOTA: Es posible que deba crear este recurso compartido por sí mismo\).  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  Elija **AdfsConfigurationV3** como la base de datos de publicación y haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  En **tipo de publicación**, elija **publicación de combinación** y haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  En **tipos de suscriptor**, elija **SQL Server 2008 o posterior** y haga clic en **siguiente**.  
 ![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  En el **artículos** página, seleccione **tablas** nodo para seleccionar todas las tablas, a continuación, **anular\-comprobar SyncProperties** tabla \(ésta no debe ser replicado\)</br>
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  En el **artículos** página, seleccione **funciones definidas por el usuario** nodo para seleccionar todas las funciones definidas por usuario y haga clic en **siguiente**...  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. En el **problemas de los artículos** página haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. En el **filtrar filas de tabla** página, haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. En el **agente de instantáneas** página, elija los valores predeterminados de 14 días e inmediato, haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
Es posible que deba crear una cuenta de dominio para el Agente SQL. Siga los pasos de [inicio de sesión de configuración de SQL para la cuenta de dominio CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) para crear el inicio de sesión SQL para este nuevo usuario de AD y asignar permisos específicos.  
  
13. En el **seguridad del agente** página, haga clic en **configuración de seguridad** y escriba el nombre de usuario\/contraseña de una cuenta de dominio \(no una GMSA\) creado para el Agente SQL y Haga clic en **Aceptar**.  Haz clic en **Siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. En el **acciones del asistente** página, haga clic en **siguiente**.   
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. En el **Complete el asistente** página, escriba un nombre para la publicación y haga clic en **finalizar**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. Una vez creada la publicación debería ver el estado de correcto.  Haga clic en **Cerrar**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. Nuevo en SQL Server Management Studio, haga clic en la nueva publicación y haga clic en **iniciar Monitor de replicación**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Crear configuración de la suscripción en la réplica de SQL Server  
Asegúrese de que ha creado la configuración del publicador en el servidor SQL inicial como se describió anteriormente y, a continuación, complete el procedimiento siguiente:  
  
1.  En la réplica de SQL Server, desde SQL Server Management studio, en **replicación**, haga clic en **suscripciones locales** y elija **nueva suscripción...** . ![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  En el **Asistente para nueva suscripción** página, haga clic en **siguiente**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  En el **publicación** página, seleccione el publicador de la lista desplegable.  Expanda **AdfsConfigurationV3** y seleccione el nombre de la publicación creada anteriormente y haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  En el **ubicación del agente de mezcla** página, seleccione **ejecutar cada agente en su suscriptor \(suscripciones de extracción\)**  \(el valor predeterminado\) y haga clic en  **Siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> Esto, junto con el tipo de suscripción a continuación, determina la lógica de resolución de conflictos. \(Para obtener más información, consulte [detectar y resolver conflictos de replicación de mezcla](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5.  En el **suscriptores** página, seleccione **AdfsConfigurationV3** como la base de datos de suscriptor y haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  En el **seguridad del agente de mezcla** página, haga clic en **...**  y escriba el nombre de usuario y la contraseña de una cuenta de dominio \(no GMSA\) creadas para el Agente SQL con el botón de puntos suspensivos y haga clic en **siguiente**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  En **programación de sincronización**, elija **ejecutar continuamente** y haga clic en **siguiente**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  En **inicializar suscripciones**, haga clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  En **tipo de suscripción**, elija **cliente** y haga clic en **siguiente**.  
  
    Implicaciones de esto se documentan [aquí](https://technet.microsoft.com/library/ms151191.aspx) y [aquí](https://technet.microsoft.com/library/ms151170.aspx).  En esencia, creamos la resolución de conflictos "de la primera para el publicador gana" simple y no es necesario volver a publicar en otros suscriptores.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. En el **acciones del asistente** , asegúrese de **crear la suscripción** está activada y haga clic en **siguiente**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. En el **Complete el asistente** página, haga clic en **finalizar**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. Una vez finalizado el proceso de creación de la suscripción, debería ver el éxito. Haga clic en **Cerrar**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Comprobar el proceso de inicialización y replicación  
  
1.  En el servidor SQL principal, secundario\-haga clic en el **replicación** nodo y haga clic en **iniciar Monitor de replicación**.  
  
2.  En **Monitor de replicación**, haga clic en la publicación.  
  
3.  En el **todas las suscripciones** pestaña, haga clic y **ver detalles**.  
  
    Debe ser capaz de ver el número de entradas en **acciones** para la replicación inicial.  
  
4.  Además, puede buscar en el **del Agente SQL Server\\trabajos** nodo para ver el trabajo\(s\) programado para ejecutar las operaciones de la publicación\/suscripción.  Solo se muestran los trabajos locales, así que asegúrese de comprobar en el publicador y el suscriptor para solucionar problemas.  Derecha\-haga clic en un trabajo y seleccione **Ver historial** para ver el historial de ejecución y los resultados.  
  
## <a name="sqlagent"></a>Configurar el inicio de sesión SQL para la cuenta de dominio CONTOSO\\sqlagent  
  
1.  Crear un nuevo inicio de sesión en el servidor primario y la réplica de SQL Server denominada CONTOSO\\sqlagent \(el nombre del nuevo usuario de dominio creado y configurado en el **seguridad del agente** página en los procedimientos anteriores.\)  
  
2.  En SQL Server, a la derecha\-haga clic en el inicio de sesión que creó, seleccione Propiedades y, a continuación, en el **User Mapping** ficha, asigne a este inicio de sesión **AdfsConfiguration** y **AdfsArtifact**  bases de datos con el público y db\_genevaservice roles. También asignar este inicio de sesión de base de datos de distribución y agregue db\_rol de propietario para las tablas de distribución y adfsconfiguration.  Ello según corresponda en principal y réplica de SQL server. Para obtener más información, consulte [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Asigne a la cuenta de dominio correspondiente leer y permisos de escritura en el recurso compartido configurado como distribuidor.  Asegúrese de establecer lectura y escritura tanto en los permisos de recurso compartido y los permisos de archivo local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurar AD FS nodo\(s\) para que apunte a la granja de servidores de réplica de SQL Server  
Ahora que ha configurado la redundancia geográfica, los nodos de la granja de servidores de AD FS pueden configurarse para que apunte a la granja de SQL Server de réplica mediante las capacidades de granja de servidores de FS "join" AD estándar, ya sea desde la interfaz de usuario de Asistente de configuración de AD FS o mediante Windows PowerShell.  
  
Si usa la interfaz de usuario de Asistente de configuración de AD FS, seleccione **agregar un servidor de federación a una granja de servidores de federación**. **No** seleccione **crear el primer servidor de federación en una granja de servidores de federación**.  
  
Si usa Windows PowerShell, ejecute **agregar\-AdfsFarmNode**. **No** ejecutar **instalar\-AdfsFarm**.  
  
Cuando se le solicite, proporcione el nombre de host y una instancia de la réplica de SQL Server, **no** inicial SQL server.  
