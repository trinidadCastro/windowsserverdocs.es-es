---
title: Configuración de la redundancia geográfica con Replicación de SQL Server
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 54106ae635d44368542986c7c469560981f9888a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855848"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Configuración de la redundancia geográfica con Replicación de SQL Server


> [!IMPORTANT]  
> Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 o superior.
  
Si usa SQL Server como su AD FS base de datos de configuración, puede configurar la redundancia de\-geográfica para la granja de AD FS con SQL Server replicación. Geo\-redundancia replica los datos entre dos sitios alejados geográficamente para que las aplicaciones puedan cambiar de un sitio a otro. De este modo, en caso de que se produzca un error en un sitio, todavía puede tener todos los datos de configuración disponibles en el segundo sitio. Para obtener más información, consulte la sección "SQL Server redundancia geográfica" en [granja de servidores de Federación con SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Requisitos previos  
Instalar y configurar una granja de servidores de SQL Server. Para obtener más información, vea [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). En el SQL Server inicial, asegúrese de que el servicio de Agente SQL Server está en ejecución y establecido en Inicio automático.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Cree la segunda réplica de \(\) SQL Server para redundancia geográfica\-  
  
1. Instale SQL Server \(para obtener más información, vea [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copie los archivos de script CreateDB. SQL y SetPermissions. SQL resultantes en el servidor SQL Server de réplica.  
  
2. Asegúrese de que Agente SQL Server servicio se está ejecutando y establecido en Inicio automático.  
  
3. Ejecute **Export\-AdfsDeploymentSQLScript** en el nodo principal de AD FS para crear archivos CreateDB. SQL y SetPermissions. SQL.  Por ejemplo:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql2.png)
  
4. Copie los scripts en el servidor secundario.  Abra el script CreateDB. SQL en **sql Management Studio** y haga clic en **Ejecutar**.
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql4.png)

5. Abra el script SetPermissions. SQL en **sql Management Studio** y haga clic en **Ejecutar**.
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql6.png) 

   

> [!NOTE]
> También puede utilizar lo siguiente desde la línea de comandos. 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Crear la configuración del publicador en el SQL Server inicial  
  
1. En el SQL Server Management Studio, en **replicación**, haga clic con el botón derecho en **publicaciones locales** y elija **nueva publicación...** 
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql7.png) </br>  

2. En la pantalla del Asistente para nueva publicación, haga clic en **siguiente**.</br>
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql8.png) </br> 
  
3. En la página **distribuidor** , elija servidor local como distribuidor y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql9.png) </br>   

4. En la página carpeta de **instantáneas** , escriba \\\SQL1\repldata en lugar de la carpeta predeterminada. \(Nota: es posible que tenga que crear este recurso compartido usted mismo\).  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql10.png) </br>   
  
5. Elija **AdfsConfigurationV3** como base de datos de publicación y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql11.png) </br>
  
6. En **tipo de publicación**, seleccione **publicación de combinación** y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql12.png) </br>
  
7. En **tipos de suscriptor**, elija **SQL Server 2008 o posterior** y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql13.png) </br> 

8. En la página **artículos** , seleccione el nodo **tablas** para seleccionar todas las tablas y, a continuación, **un\-comprobar** la tabla SyncProperties \(no se debe replicar esta\)</br>
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql14.png) </br>    
  
9. En la página **artículos** , seleccione el nodo **funciones definidas por el usuario** para seleccionar todas las funciones definidas por el usuario y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql15.png) </br>    

10. En la página **problemas de artículo** , haga clic en **siguiente**.  
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql16.png) </br>   

11. En la página **filtrar filas de tabla** , haga clic en **siguiente**.  
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql17.png) </br>   
12. En la página **agente de instantáneas** , elija valores predeterminados inmediato y 14 días, haga clic en **siguiente**.  
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql18.png) </br>   
    Es posible que tenga que crear una cuenta de dominio para el Agente SQL. Siga los pasos descritos en [configurar el inicio de sesión de SQL para la cuenta de dominio CONTOSO\\SQLAgent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) para crear el inicio de sesión de SQL para este nuevo usuario de AD y asignar permisos específicos.  
  
13. En la página **seguridad del agente** , haga clic en configuración de **seguridad** y escriba el nombre de usuario\/contraseña de una cuenta de dominio \(no un\) de GMSA creado para el Agente SQL y haga clic en **Aceptar**.  Haga clic en **Siguiente**.  
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql19.png) </br>  

14. En la página **acciones del asistente** , haga clic en **siguiente**.   
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql20.png) </br>

15. En la página **finalización del asistente** , escriba un nombre para la publicación y haga clic en **Finalizar**. 
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql21.png) </br>  

16. Una vez creada la publicación, debería ver el estado correcto.  Haga clic en **Cerrar**.
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql22.png) </br>  

17. De nuevo en SQL Server Management Studio, haga clic con el botón secundario en la nueva publicación y haga clic en **iniciar monitor de replicación**.  
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Crear configuración de suscripción en la réplica SQL Server  
Asegúrese de que ha creado la configuración del publicador en el SQL Server inicial como se describió anteriormente y, a continuación, complete el procedimiento siguiente:  
  
1. En el SQL Server de réplica, en SQL Server Management Studio, en **replicación**, haga clic con el botón secundario en **suscripciones locales** y elija **nueva suscripción.** ... ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql24.png) </br>  

2. En la página del **Asistente para nuevas suscripciones** , haga clic en **siguiente**.
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql25.png) </br>   
  
3. En la página **publicación** , seleccione el publicador en la lista desplegable.  Expanda **AdfsConfigurationV3** y seleccione el nombre de la publicación creada anteriormente y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql26.png) </br> 
  
4. En la página **Ubicación de agente de mezcla** , seleccione **ejecutar cada agente en su suscriptor \(suscripciones de extracción\)** \(la\) predeterminada y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql27.png) </br> Esto, junto con el tipo de suscripción siguiente, determina la lógica de resolución de conflictos. \(para obtener más información, vea [detectar y resolver conflictos de replicación de mezcla](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5. En la página **suscriptores** , seleccione **AdfsConfigurationV3** como la base de datos del suscriptor y haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql28.png) </br> 
  
6. En la página **seguridad de agente de mezcla** , haga clic en **...** y escriba el nombre de usuario y la contraseña de una cuenta de dominio \(no un\) de GMSA creado para el Agente SQL mediante el cuadro puntos suspensivos y haga clic en **siguiente**.
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql30.png) </br> 
  
7. En **programación de sincronización**, elija **Ejecutar continuamente** y haga clic en **siguiente**. 
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql31.png) </br> 
 
8. En **inicializar suscripciones**, haga clic en **siguiente**.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql32.png) </br> 
  
9. En **tipo de suscripción**, elija **cliente** y haga clic en **siguiente**.  
  
   Las implicaciones de esto se documentan [aquí](https://technet.microsoft.com/library/ms151191.aspx) y [aquí](https://technet.microsoft.com/library/ms151170.aspx).  En esencia, se realiza la resolución de conflictos "primero en el publicador gana" y no es necesario volver a publicar en otros suscriptores.  
   ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql33.png) </br>
   
10. En la página **acciones del asistente** , asegúrese de **que la opción crear la suscripción** está activada y haga clic en **siguiente**. 
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql34.png) </br>

11. En la página **finalización del asistente** , haga clic en **Finalizar**. 
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql35.png) </br>

12. Una vez que la suscripción ha finalizado el proceso de creación, debería ver que se ha realizado correctamente. Haga clic en **Cerrar**. 
    ![configurar la redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Comprobar el proceso de inicialización y replicación  
  
1.  En el servidor SQL Server principal, haga clic con el botón secundario\-en el nodo **replicación** y haga clic en **iniciar monitor de replicación**.  
  
2.  En el **monitor de replicación**, haga clic en la publicación.  
  
3.  En la pestaña **todas las suscripciones** , haga clic con el botón derecho y **vea los detalles**.  
  
    Debe poder ver muchas entradas en **acciones** para la replicación inicial.  
  
4.  Además, puede buscar en el nodo **Agente SQL Server\\trabajos** para ver el trabajo\(s\) programado para ejecutar las operaciones de la suscripción\/de publicación.  Solo se muestran los trabajos locales, por lo que debe asegurarse de comprobar en el publicador y en el suscriptor para solucionar el problema.  \-haga clic con el botón derecho en un trabajo y seleccione **Ver historial** para ver el historial de ejecución y los resultados.  
  
## <a name="configure-sql-login-for-the-domain-account-contososqlagent"></a><a name="sqlagent"></a>Configurar el inicio de sesión de SQL para la cuenta de dominio CONTOSO\\SQLAgent  
  
1.  Cree un nuevo inicio de sesión en el SQL Server principal y de réplica denominado CONTOSO\\SQLAgent \(el nombre del nuevo usuario de dominio que creó y configuró en la página **seguridad del agente** en los procedimientos anteriores.\)  
  
2.  En SQL Server, haga clic con el botón secundario\-en el inicio de sesión que ha creado y seleccione Propiedades y, en la pestaña **asignación de usuarios** , asigne este inicio de sesión a las bases de datos **AdfsConfiguration** y **AdfsArtifact** con roles Public y DB\_genevaservice. Asigne este inicio de sesión a la base de datos de distribución y agregue el rol de propietario de DB\_para las tablas de distribución y de adfsconfiguration.  Haga esto como corresponda en el servidor SQL Server principal y en el de réplica. Para obtener más información, consulte [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Conceda a la cuenta de dominio correspondiente permisos de lectura y escritura en el recurso compartido configurado como distribuidor.  Asegúrese de establecer los permisos de lectura y escritura en los permisos de recurso compartido y en los permisos de archivo local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurar AD FS nodo\(s\) para que apunte a la granja de servidores de réplica SQL Server  
Ahora que ha configurado la redundancia geográfica, los nodos de la granja de servidores de AD FS se pueden configurar para que apunten a su réplica SQL Server granja de servidores mediante las capacidades de la granja de servidores estándar AD FS "unirse", ya sea desde la interfaz de usuario del Asistente para configuración de AD FS o mediante Windows PowerShell.  
  
Si usa la interfaz de usuario del Asistente para configuración de AD FS, seleccione **Agregar un servidor de Federación a una granja de servidores de Federación**. **No** seleccione **crear el primer servidor de Federación en una granja de servidores de Federación**.  
  
Si usa Windows PowerShell, ejecute **Add\-AdfsFarmNode**. **No** ejecute **install\-AdfsFarm**.  
  
Cuando se le solicite, proporcione el nombre de host y de instancia de la réplica SQL Server, **no** el servidor SQL Server inicial.  
