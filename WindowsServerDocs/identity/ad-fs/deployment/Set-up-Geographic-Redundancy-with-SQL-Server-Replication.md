---
title: Configuración de la redundancia geográfica con Replicación de SQL Server
description: Más información acerca de cómo configurar la redundancia geográfica con Replicación de SQL Server
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: dd666023e7b53af72f63edf1750321317e08f527
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049243"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Configuración de la redundancia geográfica con Replicación de SQL Server


> [!IMPORTANT]
> Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 o superior.

Si usa SQL Server como su AD FS base de datos de configuración, puede configurar \- la redundancia geográfica para la granja de AD FS mediante la replicación de SQL Server. La \- redundancia geográfica replica los datos entre dos sitios alejados geográficamente para que las aplicaciones puedan cambiar de un sitio a otro. De este modo, en caso de que se produzca un error en un sitio, todavía puede tener todos los datos de configuración disponibles en el segundo sitio. Para obtener más información, consulte la sección "SQL Server redundancia geográfica" en [granja de servidores de Federación con SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).

## <a name="prerequisites"></a>Prerrequisitos
Instalar y configurar una granja de servidores de SQL Server. Para obtener más información, vea [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://www.microsoft.com/en-us/evalcenter/). En el SQL Server inicial, asegúrese de que el servicio de Agente SQL Server está en ejecución y establecido en Inicio automático.

## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Crear la segunda \( réplica \) SQL Server para la \- redundancia geográfica

1. Instalar SQL Server \( para obtener más información, vea [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://www.microsoft.com/en-us/evalcenter/) . Copie los archivos de script CreateDB. SQL y SetPermissions. SQL resultantes en el servidor SQL Server de réplica.

2. Asegúrese de que Agente SQL Server servicio se está ejecutando y establecido en Inicio automático.

3. Ejecute **Export \- AdfsDeploymentSQLScript** en el nodo principal AD FS para crear archivos CreateDB. SQL y SetPermissions. SQL.  Por ejemplo: `PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql2.png)

4. Copie los scripts en el servidor secundario.  Abra el script CreateDB. SQL en **sql Management Studio** y haga clic en **Ejecutar**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql4.png)

5. Abra el script SetPermissions. SQL en **sql Management Studio** y haga clic en **Ejecutar**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql6.png)



> [!NOTE]
> También puede utilizar lo siguiente desde la línea de comandos.
>
>    `c:\>sqlcmd –i CreateDB.sql`
>
>    `c:\>sqlcmd –i SetPermissions.sql`
>
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Crear la configuración del publicador en el SQL Server inicial

1. En el SQL Server Management Studio, en **replicación**, haga clic con el botón derecho en **publicaciones locales** y elija **nueva publicación...** 
    ![ Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql7.png) </br>

2. En la pantalla del Asistente para nueva publicación, haga clic en **siguiente**.</br>
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql8.png) </br>

3. En la página **distribuidor** , elija servidor local como distribuidor y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql9.png) </br>

4. En la página carpeta de **instantáneas** , escriba \\ \SQL1\repldata en lugar de la carpeta predeterminada. \(Nota: es posible que tenga que crear este recurso compartido \) .
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql10.png) </br>

5. Elija **AdfsConfigurationV3** como base de datos de publicación y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql11.png) </br>

6. En **tipo de publicación**, seleccione **publicación de combinación** y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql12.png) </br>

7. En **tipos de suscriptor**, elija **SQL Server 2008 o posterior** y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql13.png) </br>

8. En la página **artículos** , seleccione el nodo **tablas** para seleccionar todas las tablas y, después, **\- desactive** la tabla SyncProperties. \( esta no se debe replicar.\)</br>
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql14.png) </br>

9. En la página **artículos** , seleccione el nodo **funciones definidas por el usuario** para seleccionar todas las funciones definidas por el usuario y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql15.png) </br>

10. En la página **problemas de artículo** , haga clic en **siguiente**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql16.png) </br>

11. En la página **Filtrar filas de tabla**, haga clic en **Siguiente**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql17.png) </br>
12. En la página **agente de instantáneas** , elija valores predeterminados inmediato y 14 días, haga clic en **siguiente**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql18.png) </br>
    Es posible que tenga que crear una cuenta de dominio para el Agente SQL. Siga los pasos descritos en [configurar el inicio de sesión de SQL para la cuenta de dominio contoso \\ SQLAgent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) para crear el inicio de sesión de SQL para este nuevo usuario de AD y asignar permisos específicos.

13. En la página **seguridad del agente** , haga clic en configuración de **seguridad** y escriba la \/ contraseña del nombre de usuario de una cuenta de dominio que \( no sea un GMSA \) creado para el Agente SQL y haga clic en **Aceptar**.  Haga clic en **Next**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql19.png) </br>

14. En la página **acciones del asistente** , haga clic en **siguiente**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql20.png) </br>

15. En la página **finalización del asistente** , escriba un nombre para la publicación y haga clic en **Finalizar**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql21.png) </br>

16. Una vez creada la publicación, debería ver el estado correcto.  Haga clic en **Cerrar**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql22.png) </br>

17. De nuevo en SQL Server Management Studio, haga clic con el botón secundario en la nueva publicación y haga clic en **iniciar monitor de replicación**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql23.png) </br>

## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Crear configuración de suscripción en la réplica SQL Server
Asegúrese de que ha creado la configuración del publicador en el SQL Server inicial como se describió anteriormente y, a continuación, complete el procedimiento siguiente:

1. En el SQL Server de réplica, en SQL Server Management Studio, en **replicación**, haga clic con el botón secundario en **suscripciones locales** y elija **nueva suscripción.**... ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql24.png) </br>

2. En la página del **Asistente para nuevas suscripciones** , haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql25.png) </br>

3. En la página **publicación** , seleccione el publicador en la lista desplegable.  Expanda **AdfsConfigurationV3** y seleccione el nombre de la publicación creada anteriormente y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql26.png) </br>

4. En la página **Ubicación de agente de mezcla** , seleccione **ejecutar cada agente en sus \( suscripciones \) de extracción del suscriptor** \( el valor predeterminado \) y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql27.png) </br> Esto, junto con el tipo de suscripción siguiente, determina la lógica de resolución de conflictos. \(Para obtener más información, vea [detectar y resolver conflictos de replicación de mezcla](/sql/relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution). </br>

5. En la página **suscriptores** , seleccione **AdfsConfigurationV3** como la base de datos del suscriptor y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql28.png) </br>

6. En la página **seguridad de agente de mezcla** , haga clic en **...** y escriba el nombre de usuario y la contraseña de una cuenta de dominio, \( no un GMSA \) creado para el Agente SQL mediante el cuadro puntos suspensivos y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql30.png) </br>

7. En **programación de sincronización**, elija **Ejecutar continuamente** y haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql31.png) </br>

8. En **inicializar suscripciones**, haga clic en **siguiente**.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql32.png) </br>

9. En **tipo de suscripción**, elija **cliente** y haga clic en **siguiente**.

   Las implicaciones de esto se documentan [aquí](/sql/relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution) y [aquí](/sql/relational-databases/replication/subscribe-to-publications).  En esencia, se realiza la resolución de conflictos "primero en el publicador gana" y no es necesario volver a publicar en otros suscriptores.
   ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql33.png) </br>

10. En la página **acciones del asistente** , asegúrese de **que la opción crear la suscripción** está activada y haga clic en **siguiente**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql34.png) </br>

11. En la página **finalización del asistente** , haga clic en **Finalizar**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql35.png) </br>

12. Una vez que la suscripción ha finalizado el proceso de creación, debería ver que se ha realizado correctamente. Haga clic en **Cerrar**.
    ![Configuración de redundancia geográfica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql36.png) </br>

## <a name="verify-the-process-of-initialization-and-replication"></a>Comprobar el proceso de inicialización y replicación

1.  En el servidor SQL Server principal, haga clic con el botón secundario \- en el nodo **replicación** y haga clic en **iniciar monitor de replicación**.

2.  En el **monitor de replicación**, haga clic en la publicación.

3.  En la pestaña **todas las suscripciones** , haga clic con el botón derecho y **vea los detalles**.

    Debe poder ver muchas entradas en **acciones** para la replicación inicial.

4.  Además, puede buscar en el nodo **Agente SQL Server \\ trabajos** para ver el trabajo \( \) programado para ejecutar las operaciones de la suscripción de publicación \/ .  Solo se muestran los trabajos locales, por lo que debe asegurarse de comprobar en el publicador y en el suscriptor para solucionar el problema.  Haga clic con el botón derecho \- en un trabajo y seleccione **Ver historial** para ver el historial de ejecución y los resultados.

## <a name="configure-sql-login-for-the-domain-account-contososqlagent"></a><a name="sqlagent"></a>Configurar el inicio de sesión de SQL para la cuenta de dominio CONTOSO \\ SQLAgent

1.  Cree un nuevo inicio de sesión en el SQL Server principal y de réplica denominado CONTOSO \\ SQLAgent \( nombre del nuevo usuario de dominio creado y configurado en la página **seguridad del agente** en los procedimientos anteriores.\)

2.  En SQL Server, haga clic con el botón secundario \- en el inicio de sesión que ha creado y seleccione Propiedades y, en la pestaña **asignación de usuarios** , asigne este inicio de sesión a las bases de datos **AdfsConfiguration** y **AdfsArtifact** con roles públicos y de base de datos \_ genevaservice. Asigne también este inicio de sesión a la base de datos de distribución y agregue el \_ rol de propietario de base de datos para las tablas de distribución y adfsconfiguration.  Haga esto como corresponda en el servidor SQL Server principal y en el de réplica. Para más información, consulte [Modelo de seguridad del agente de replicación](/sql/relational-databases/replication/security/replication-agent-security-model).

3.  Conceda a la cuenta de dominio correspondiente permisos de lectura y escritura en el recurso compartido configurado como distribuidor.  Asegúrese de establecer los permisos de lectura y escritura en los permisos de recurso compartido y en los permisos de archivo local.

## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configuración de AD FS nodo \( s \) para que apunte a la granja de SQL Server réplica
Ahora que ha configurado la redundancia geográfica, los nodos de la granja de servidores de AD FS se pueden configurar para que apunten a su réplica SQL Server granja de servidores mediante las capacidades de la granja de servidores estándar AD FS "unirse", ya sea desde la interfaz de usuario del Asistente para configuración de AD FS o mediante Windows PowerShell.

Si usa la interfaz de usuario del Asistente para configuración de AD FS, seleccione **Agregar un servidor de Federación a una granja de servidores de Federación**. **No** seleccione **crear el primer servidor de Federación en una granja de servidores de Federación**.

Si usa Windows PowerShell, ejecute **agregar \- AdfsFarmNode**. **No** ejecute **install \- AdfsFarm**.

Cuando se le solicite, proporcione el nombre de host y de instancia de la réplica SQL Server, **no** el servidor SQL Server inicial.
