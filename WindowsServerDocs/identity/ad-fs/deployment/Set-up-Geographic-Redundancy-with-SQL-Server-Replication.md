---
title: "Redundancia geográfica del programa de instalación con la duplicación de SQL Server"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: a9f29c1eb19a8241eac5afb5c87581e6c988c3c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Redundancia geográfica del programa de instalación con la duplicación de SQL Server

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

> [!IMPORTANT]  
> Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 o posterior.
  
Si estás usando SQL Server como la base de datos de configuración de AD FS, puede configurar geo\ redundancia para el conjunto de AD FS utiliza la duplicación de SQL Server. Geo\ redundancia replica los datos entre dos distantes sitios para que las aplicaciones pueden cambiar de un sitio a otro. De esta forma, en caso de error de un sitio, puedes seguir teniendo todos los datos de configuración disponibles en el sitio de la segundo. Para obtener más información, consulta la sección"SQL Server redundancia geográfica" en [granja de servidor de federación usar SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Requisitos previos  
Instalar y configurar una granja de servidores SQL. Para obtener más información, consulta [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). En el servidor SQL inicial, asegúrate de que se está ejecutando el servicio Agente de SQL Server y establecer para el inicio automático.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Crear la segunda \(replica\) SQL Server para geo\ redundancia  
  
1.  Instalar SQL Server \ (para obtener más información, consulta [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copia los archivos de script CreateDB.sql y SetPermissions.sql resultantes en SQL server que réplica.  
  
2.  Asegúrate de que está ejecutando el servicio Agente SQL Server y la establece en el inicio automático  
  
3.  Ejecutar **Export\ AdfsDeploymentSQLScript** en el nodo primario de AD FS para crear archivos CreateDB.sql y SetPermissions.sql.  Por ejemplo:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  Copia los scripts en el servidor secundario.  Abre el script CreateDB.sql en **SQL Management Studio** y haz clic en **Execute**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. Abre el script SetPermissions.sql en **SQL Management Studio** y haz clic en **Execute**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>También puedes usar lo siguiente desde la línea de comandos. 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Crear configuraciones de editor en el servidor SQL inicial  
  
1.  En SQL Server Management studio, en **replicación**, haz **publicaciones locales** y elige **nueva publicación... **
![ Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  En la pantalla del Asistente para nueva publicación, haz clic en **siguiente**.</br>
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  En **distribuidor** página, elige el servidor local como distribuidor y haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  En la **instantánea** carpeta, escriba \\\SQL1\repldata en lugar de la carpeta predeterminada. \ (Nota: puede que tengas que crear este yourself\ compartir).  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  Elige **AdfsConfigurationV3** como la base de datos de publicación y haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  En **tipo de publicación**, elige **combinar publicación** y haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  En **tipos de suscriptor**, elige **SQL Server 2008 o posterior** y haz clic en **siguiente**.  
 ![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  En la **artículos** página selecciona **tablas** nodo para seleccionar todas las tablas, a continuación, **un\ verificación SyncProperties** tabla \ (este no debe ser replicated\)</br>
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  En la **artículos** página, seleccione **funciones de definidas por el usuario** nodo para seleccionar todas las funciones de definidas por el usuario y haz clic en **siguiente**..  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. En la **problemas del artículo** página, haz clic **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. En la **filas de la tabla de filtro** página, haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. En la **agente de instantáneas** página, elige los valores predeterminados de inmediato y 14 días, haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
Debes crear una cuenta de dominio para el Agente SQL. Usa los pasos en [inicio de sesión de configurar SQL para la cuenta de dominio CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) para crear el inicio de sesión SQL para este nuevo usuario anuncios y asignar permisos específicos.  
  
13. En la **agente seguridad** página, haz clic en **la configuración de seguridad** y escribe el username\ y la contraseña de una cuenta de dominio \ (no una GMSA\) crea para el Agente SQL y haz clic en **Aceptar**.  Haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. En la **acciones del asistente** página, haz clic en **siguiente**.   
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. En la **completa el asistente** página, escribe un nombre para la publicación y haz clic en **finalizar**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. Una vez creada la publicación deberías ver el estado de éxito.  Haz clic en **cerrar **.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. En SQL Server Management Studio, haz clic en la publicación nuevo hacia atrás y haz clic en **iniciar el Monitor de replicación**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Crear la configuración de la suscripción en la réplica de SQL Server  
Asegúrate de que has creado la configuración de editor en el servidor SQL inicial como se describió anteriormente y, a continuación, realiza el procedimiento siguiente:  
  
1.  En la réplica de SQL Server, en SQL Server Management studio, en **replicación**, haga clic **suscripciones locales** y elige **nueva suscripción... **. ![Configurar redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  En la **Asistente para nueva suscripción** página, haz clic en **siguiente**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  En la **publicación** página, seleccione el publicador de la lista desplegable.  Expande **AdfsConfigurationV3** y selecciona el nombre de la publicación creado anteriormente y haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  En la **combinar la ubicación del agente** página, seleccione **ejecutar cada agente en su \(pull subscriptions\) suscriptor** \(the default\) y haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> Esto, junto con el tipo de suscripción a continuación, determina la lógica de resolución de conflictos. \ (Para obtener más información, consulta [detectar y resolver conflictos de replicación combinar](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5.  En la **suscriptores** página, seleccione **AdfsConfigurationV3** como la base de datos de suscriptor y haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  En la **combinar agente seguridad** página, haz clic en **... ** y escribe el nombre de usuario y contraseña de una cuenta de dominio \ (no una GMSA\) crea para el Agente SQL mediante el cuadro de puntos suspensivos y y haz clic en **siguiente**.
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  En **programación de sincronización**, elige **ejecutar continuamente** y haz clic en **siguiente**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  En **inicializar suscripciones**, haz clic en **siguiente**.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  En **tipo de suscripción**, elige **cliente** y haz clic en **siguiente**.  
  
    Implicaciones que esto se documentan [aquí](https://technet.microsoft.com/library/ms151191.aspx) y [aquí](https://technet.microsoft.com/library/ms151170.aspx).  Básicamente, tomamos la resolución de conflictos de "primero en wins publisher" simple y no es necesario volver a publicar en otros suscriptores.  
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. En la **acciones del asistente** , asegúrese de **crear la suscripción** está activado y haz clic en **siguiente**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. En la **completa el asistente** página, haz clic en **finalizar**. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. Una vez que la suscripción ha finalizado el proceso de creación, deberías ver éxito. Haz clic en **cerrar **. 
![Configurar la redundancia geográfica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Comprueba el proceso de inicialización y la replicación  
  
1.  En el servidor SQL principal, right\ y haga clic en el **replicación** nodo y haz clic en **iniciar el Monitor de replicación**.  
  
2.  En **Monitor de replicación**, haz clic en la publicación.  
  
3.  En la **todas las suscripciones** ficha, haga clic y **ver detalles**.  
  
    Deberías poder ver muchas entradas en **acciones** para la replicación inicial.  
  
4.  Además, puedes buscar en la **SQL Server Agent\\Jobs** nodo para ver la job\(s\) programada para ejecutar las operaciones de la suscripción de publication\.  Solo se muestran los trabajos locales, así que asegúrate de comprobar el publicador y el suscriptor para solucionar problemas.  Right\ y haga clic en un trabajo y selecciona **ver el historial de** para ver el historial de ejecución y los resultados.  
  
## <a name="sqlagent"></a>Configurar el inicio de sesión SQL para la cuenta de dominio CONTOSO\\sqlagent  
  
1.  Crear un nuevo inicio de sesión en los principales y réplica SQL Server denominada CONTOSO\\sqlagent \ (el nombre del nuevo usuario de dominio se crean y configuran en la **agente seguridad** página en los procedimientos anteriores. \)  
  
2.  En SQL Server, right\ y haga clic en el inicio de sesión que has creado y selecciona Propiedades, a continuación, en la **User Mapping** pestaña, asignar este inicio de sesión a **AdfsConfiguration** y **AdfsArtifact** bases de datos con el público y db\_genevaservice roles. También asignar este inicio de sesión a la base de datos de distribución y agrega el rol db\_owner para distribución y adfsconfiguration tablas.  Para hacerlo tal como se aplica en principal y réplica SQL server. Para obtener más información, consulta [modelo de seguridad del agente de replicación](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Dar la lectura de cuenta de dominio correspondiente y permisos de escritura en el recurso compartido configurado como distribuidor.  Asegúrate de que establecer leer y escribir permisos tanto en los permisos de recurso compartido y los permisos de archivos local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurar AD FS node\(s\) para señalar el conjunto de réplica de SQL Server  
Ahora que has configurado redundancia geográfico, los nodos de la granja de servidores de AD FS pueden configurarse para que apunte al conjunto de SQL Server réplica usando las funcionalidades de granja de unión FS "a" AD estándar, ya sea desde la interfaz de usuario de Asistente de configuración de AD FS o mediante Windows PowerShell.  
  
Si usas la interfaz de usuario de AD FS configuración asistente, selecciona **agregar un servidor de federación a una granja de servidores de federación**. **No** selecciona **crea el primer servidor de federación en una granja de servidores de federación**.  
  
Si usas Windows PowerShell, ejecuta **Add\ AdfsFarmNode**. **No** ejecutar **Install\ AdfsFarm**.  
  
Cuando se te solicite, proporcionar el nombre de host y la instancia de la réplica de SQL Server, **no** inicial SQL server.  
