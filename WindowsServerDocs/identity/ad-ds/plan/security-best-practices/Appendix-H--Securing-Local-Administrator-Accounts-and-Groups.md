---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: "Apéndice H - proteger cuentas de administrador Local y grupos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 222e6725456bb76267240467469e97c5b64fc888
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apéndice H: proteger cuentas de administrador Local y grupos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apéndice H: proteger cuentas de administrador Local y grupos  
En todas las versiones de Windows actualmente en el soporte estándar, la cuenta de administrador local está deshabilitada de manera predeterminada, lo que hace que la cuenta que no se puede utilizar para pass-the-hash y otros ataques de robo de credenciales. Sin embargo, en entornos que contienen los sistemas operativos heredados o en el que se han habilitado cuentas de administrador locales, estas cuentas pueden usarse como se describió anteriormente para propagar compromiso en estaciones de trabajo y los servidores miembro. Cada cuenta de administrador local y el grupo se deben proteger como se describe en las instrucciones paso a paso que siguen.  

Para obtener más información acerca de las consideraciones de seguridad de los grupos de administrador integrada (BA), [implementar modelos administrativos de privilegios mínimos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para cuentas de administrador Local  
Para que la cuenta de administrador local en cada dominio del bosque, debes configurar la siguiente configuración:  

-   Configurar los GPO para restringir el uso de la cuenta de administrador de dominio en sistemas unidos a un dominio  
    -   En uno o más GPO que crean y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agrega la cuenta de administrador a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:  

        -   Denegar el acceso a este equipo desde la red  

        -   Denegar inicio de sesión como trabajo por lotes  

        -   Denegar inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de servicios de escritorio remoto  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Instrucciones paso a paso para proteger los grupos de administradores locales  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configuración de GPO para restringir la cuenta de administrador en sistemas unidos a un dominio  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **Group Policy Management**.  

2.  En el árbol de consola, expande <Forest>\Domains\\<Domain>y luego **objetos de directiva de grupo** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

3.  En el árbol de consola, haz clic en **objetos de directiva de grupo**y haz clic en **nueva**.  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  En la **nuevo GPO** cuadro de diálogo, escribe ** <GPO Name> **y haz clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  En el panel de detalles, haz clic en ** <GPO Name> **y haz clic en **editar**.  

6.  Navegar a **equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haz clic en **asignación de derechos de usuario**.  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configurar los derechos de usuario para evitar que la cuenta de administrador local accedan a los servidores miembros y estaciones de trabajo en la red haciendo lo siguiente:  

    1.  Haz doble clic en **denegar el acceso a este equipo desde la red** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo**, escribe el nombre de usuario de la cuenta de administrador local y haz clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Haz clic en Aceptar.  

        > [!IMPORTANT]  
        > Cuando agregas la cuenta de administrador para estas opciones de configuración, especifican si va a configurar una cuenta de administrador local o una cuenta de administrador de dominio mediante cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS estos denegar derechos, navegas a la cuenta de administrador para el dominio TAILSPINTOYS, que aparecieran como TAILSPINTOYS\Administrator. Si escribes **administrador** en estas opciones de derechos de usuario en el Editor de objetos de directiva de grupo, se restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

8.  Configurar los derechos de usuario para evitar que la cuenta de administrador local de iniciar sesión como trabajo por lotes haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como trabajo por lotes** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo**, escribe el nombre de usuario de la cuenta de administrador local y haz clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Haz clic en **Aceptar**.  

        > [!IMPORTANT]  
        > Cuando agregas la cuenta de administrador para estas opciones de configuración, especifican si va a configurar la cuenta de administrador local o cuenta de administrador de dominio mediante cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS estos denegar derechos, navegas a la cuenta de administrador para el dominio TAILSPINTOYS, que aparecieran como TAILSPINTOYS\Administrator. Si escribes **administrador** en estas opciones de derechos de usuario en el Editor de objetos de directiva de grupo, se restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

9. Configurar los derechos de usuario para evitar que la cuenta de administrador local de iniciar sesión como servicio haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como servicio** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo**, escribe el nombre de usuario de la cuenta de administrador local y haz clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Haz clic en **Aceptar**.  

        > [!IMPORTANT]  
        > Cuando agregas la cuenta de administrador para estas opciones de configuración, especifican si va a configurar la cuenta de administrador local o cuenta de administrador de dominio mediante cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS estos denegar derechos, navegas a la cuenta de administrador para el dominio TAILSPINTOYS, que aparecieran como TAILSPINTOYS\Administrator. Si escribes **administrador** en estas opciones de derechos de usuario en el Editor de objetos de directiva de grupo, se restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

10. Configurar los derechos de usuario para evitar que la cuenta de administrador local accedan a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo**, escribe el nombre de usuario de la cuenta de administrador local y haz clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Haz clic en **Aceptar**.  

        > [!IMPORTANT]  
        > Cuando agregas la cuenta de administrador para estas opciones de configuración, especifican si va a configurar la cuenta de administrador local o cuenta de administrador de dominio mediante cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS estos denegar derechos, navegas a la cuenta de administrador para el dominio TAILSPINTOYS, que aparecieran como TAILSPINTOYS\Administrator. Si escribes **administrador** en estas opciones de derechos de usuario en el Editor de objetos de directiva de grupo, se restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

11. Para salir **el Editor de administración de directivas de grupo**, haz clic en **archivo**y haz clic en **salir**.  

12. En **Group Policy Management**, vincular el GPO a los servidores miembros y unidades organizativas de la estación de trabajo mediante las siguientes acciones:  

    1.  Navegar a la <Forest>\Domains\\<Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

    2.  Haz clic en la unidad organizativa que el GPO se aplicarán a y haz clic en **vincular un GPO existente**.  

        ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Selecciona el GPO que creaste y haz clic en **Aceptar**.  

        ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Crea vínculos a todas las otras unidades organizativas que contienen las estaciones de trabajo.  

    5.  Crea vínculos a todas las otras unidades organizativas que contienen los servidores miembro.  

#### <a name="verification-steps"></a>Pasos de verificación  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "Denegar el acceso a este equipo desde la red"  

Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (como un servidor de accesos directos), intenta obtener acceso a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración de GPO, intenta asignar la unidad del sistema mediante el **NET USE** comando.  

1.  Iniciar sesión localmente en cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **símbolo**, haz clic en **símbolo**y, a continuación, haz clic en **ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.  

4.  Cuando se te solicite para aprobar la elevación, haz clic en **Sí**.  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  En la **símbolo** ventana, escribe **net use \\\<Server Name>/User \c$:<Server Name>\Administrator**, donde <Server Name> es el nombre del servidor miembro o estación de trabajo que estás intentando acceder a través de la red.  

    > [!NOTE]  
    > Las credenciales de administrador locales deben ser del mismo sistema de que estás intentando acceder a través de la red.  

6.  Captura de pantalla siguiente muestra el mensaje de error que debe aparecer.  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  
Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

###### <a name="create-a-batch-file"></a>Crear un archivo por lotes  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **el Bloc de notas**y haz clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haz clic en **archivo**y haz clic en **Guardar como**.  

5.  En la **nombre de archivo** , escriba ** <Filename>.bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** cuadro, escribe el programador de tareas y haz clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en la **búsqueda** , escriba **programar tareas**y haz clic en **programar tareas**.  

3.  Haz clic en **acción**y haz clic en **crear tarea**.  

4.  En la **crear tarea** cuadro de diálogo, escribe ** <Task Name> ** (donde <Task Name> es el nombre de la nueva tarea).  

5.  Haz clic en el **acciones** pestaña y haz clic en **nueva**.  

6.  En la **acción** , a continuación, haz clic en **iniciar un programa**.  

7.  En la **programa o script** , a continuación, haz clic en **examinar**, busca y selecciona el archivo por lotes que creaste en el **crear un archivo por lotes** sección y haz clic en **abrir**.  

8.  Haz clic en **Aceptar**.  

9. Haz clic en el **General** pestaña.  

10. En la **opciones de seguridad** , a continuación, haz clic en **Cambiar usuario o grupo**.  

11. Escribe el nombre de cuenta de administrador local del sistema, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

12. Selecciona **ejecutar si el usuario ha iniciado sesión o no** y **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haz clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo, cuenta de usuario solicita credenciales para ejecutar la tarea.  

15. Después de escribir las credenciales, haz clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En **iniciar sesión como** , a continuación, haz clic en **esta cuenta**.  

7.  Haz clic en **examinar**, escribe la cuenta de administrador local del sistema, haga clic en **comprobar nombres**y haz clic en **Aceptar**.  

8.  En la **contraseña** y **Confirmar contraseña** campos, escribe la contraseña de la cuenta seleccionada y haz clic en **Aceptar**.  

9. Haz clic en **Aceptar** tres veces más.  

10. Haz clic en **cola de impresión** y haz clic en **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio de la cola de impresora  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En la **iniciar sesión como**: campo, seleccione **Systemaccount Local**y haz clic en **Aceptar**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **conexión a Escritorio remoto**y haz clic en **conexión a Escritorio remoto**.  

3.  En la **equipo** , a continuación, escribe el nombre del equipo que desea conectarse y haz clic en **conectar**. (También puedes escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se te solicite, proporcionar credenciales para el sistema local **administrador** cuenta.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos y cuentas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
