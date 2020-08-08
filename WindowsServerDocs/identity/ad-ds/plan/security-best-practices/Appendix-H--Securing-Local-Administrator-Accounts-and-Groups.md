---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: 'Apéndice H: protección de grupos y cuentas de administrador local'
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c3d9e6bd810b0d0c3d3d6aac310b3ff058b3e018
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963253"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Anexo H: protección de cuentas de administrador local y grupos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Anexo H: protección de cuentas de administrador local y grupos
En todas las versiones de Windows que se encuentran actualmente en soporte estándar, la cuenta de administrador local está deshabilitada de forma predeterminada, lo que hace que la cuenta sea inutilizable para Pass-The-hash y otros ataques de robo de credenciales. Sin embargo, en entornos que contienen sistemas operativos heredados o en los que se han habilitado cuentas de administrador local, estas cuentas se pueden usar como se ha descrito anteriormente para propagar el compromiso entre los servidores miembro y las estaciones de trabajo. Cada cuenta de administrador local y grupo debe protegerse tal y como se describe en las instrucciones paso a paso que se indican a continuación.

Para obtener información detallada acerca de las consideraciones sobre la protección de grupos de administradores integrados (BA), consulte [implementación de modelos administrativos con privilegios mínimos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).

#### <a name="controls-for-local-administrator-accounts"></a>Controles para las cuentas de administrador local
Para la cuenta de administrador local en cada dominio del bosque, debe configurar las siguientes opciones:

-   Configurar GPO para restringir el uso de la cuenta de administrador del dominio en sistemas Unidos a un dominio
    -   En uno o más GPO que cree y vincule a las unidades organizativas de estaciones de trabajo y servidores miembro de cada dominio, agregue la cuenta de administrador a los siguientes derechos de usuario en el **equipo \ configuración de Seguridad\directivas \ configuración de directivas**de usuario \ asignación de derechos:

        -   Denegar el acceso desde la red a este equipo

        -   Denegar el inicio de sesión como trabajo por lotes

        -   Denegar el inicio de sesión como servicio

        -   Denegar inicio de sesión a través de Servicios de Escritorio remoto

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Instrucciones paso a paso para proteger los grupos de administradores locales

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configuración de GPO para restringir la cuenta de administrador en sistemas Unidos a un dominio

1.  En **Administrador del servidor**, haga clic en **herramientas**y en **Administración de directiva de grupo**.

2.  En el árbol de consola, expanda <Forest> \Domains y \\ <Domain> , a continuación, **Directiva de grupo objetos** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea establecer el Directiva de grupo).

3.  En el árbol de consola, haga clic con el botón secundario en **Directiva de grupo objetos**y haga clic en **nuevo**.

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)

4.  En el cuadro de diálogo **nuevo GPO** , escriba **<GPO Name>** y haga clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)

5.  En el panel de detalles, haga clic con el botón secundario **<GPO Name>** y haga clic en **Editar**.

6.  Vaya a **equipo \ configuración de Seguridad\directivas \ directivas de seguridad\Directivas**y haga clic en **asignación de derechos de usuario**.

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)

7.  Configure los derechos de usuario para impedir que la cuenta de administrador local tenga acceso a los servidores y estaciones de trabajo de los miembros a través de la red haciendo lo siguiente:

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **Administrador**, que es el valor predeterminado cuando se instala Windows.

        ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)

    3.  Haga clic en Aceptar.

        > [!IMPORTANT]
        > Al agregar la cuenta de administrador a esta configuración, se especifica si se va a configurar una cuenta de administrador local o una cuenta de administrador de dominio mediante el etiquetado de las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos derechos de denegación, debe ir a la cuenta de administrador del dominio TAILSPINTOYS, que aparecería como TAILSPINTOYS\Administrator. Si escribe **Administrador** en esta configuración de derechos de usuario en el editor de objetos de directiva de grupo, restringe la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.

8.  Configure los derechos de usuario para evitar que la cuenta de administrador local inicie sesión como un trabajo por lotes haciendo lo siguiente:

    1.  Haga doble clic en **denegar el inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **Administrador**, que es el valor predeterminado cuando se instala Windows.

        ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)

    3.  Haga clic en **Aceptar**.

        > [!IMPORTANT]
        > Al agregar la cuenta de administrador a esta configuración, debe especificar si va a configurar la cuenta de administrador local o la cuenta de administrador de dominio mediante el etiquetado de las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos derechos de denegación, debe ir a la cuenta de administrador del dominio TAILSPINTOYS, que aparecería como TAILSPINTOYS\Administrator. Si escribe **Administrador** en esta configuración de derechos de usuario en el editor de objetos de directiva de grupo, restringe la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.

9. Configure los derechos de usuario para evitar que la cuenta de administrador local inicie sesión como servicio haciendo lo siguiente:

    1.  Haga doble clic en **denegar el inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **Administrador**, que es el valor predeterminado cuando se instala Windows.

        ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)

    3.  Haga clic en **Aceptar**.

        > [!IMPORTANT]
        > Al agregar la cuenta de administrador a esta configuración, debe especificar si va a configurar la cuenta de administrador local o la cuenta de administrador de dominio mediante el etiquetado de las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos derechos de denegación, debe ir a la cuenta de administrador del dominio TAILSPINTOYS, que aparecería como TAILSPINTOYS\Administrator. Si escribe **Administrador** en esta configuración de derechos de usuario en el editor de objetos de directiva de grupo, restringe la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.

10. Configure los derechos de usuario para impedir que la cuenta de administrador local tenga acceso a los servidores y las estaciones de trabajo miembro a través de Servicios de Escritorio remoto de la siguiente manera:

    1.  Haga doble clic en **denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **Administrador**, que es el valor predeterminado cuando se instala Windows.

        ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)

    3.  Haga clic en **Aceptar**.

        > [!IMPORTANT]
        > Al agregar la cuenta de administrador a esta configuración, debe especificar si va a configurar la cuenta de administrador local o la cuenta de administrador de dominio mediante el etiquetado de las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos derechos de denegación, debe ir a la cuenta de administrador del dominio TAILSPINTOYS, que aparecería como TAILSPINTOYS\Administrator. Si escribe **Administrador** en esta configuración de derechos de usuario en el editor de objetos de directiva de grupo, restringe la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.

11. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y, a continuación, en **salir**.

12. En **Administración de directiva de grupo**, VINCULE el GPO al servidor miembro y las unidades organizativas de la estación de trabajo haciendo lo siguiente:

    1.  Navegue hasta <Forest> \Domains \\ <Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea establecer el Directiva de grupo).

    2.  Haga clic con el botón secundario en la unidad organizativa a la que se aplicará el GPO y haga clic en **vincular un GPO existente**.

        ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)

    3.  Seleccione el GPO que ha creado y haga clic en **Aceptar**.

        ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)

    4.  Cree vínculos a todas las demás unidades organizativas que contengan estaciones de trabajo.

    5.  Cree vínculos a todas las demás unidades organizativas que contengan servidores miembro.

#### <a name="verification-steps"></a>Pasos de comprobación

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "denegar el acceso a este equipo desde la red"

Desde cualquier servidor miembro o estación de trabajo que no se vea afectado por los cambios de GPO (como un servidor de salto), intente tener acceso a un servidor miembro o una estación de trabajo a través de la red afectada por los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el comando **net use** .

1.  Iniciar sesión localmente en cualquier servidor miembro o estación de trabajo que no se vea afectado por los cambios de GPO.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

3.  En el cuadro de **búsqueda** , escriba **símbolo del sistema**, haga clic con el botón secundario en símbolo del **sistema**y, a continuación, haga clic en **Ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.

4.  Cuando se le pida que apruebe la elevación, haga clic en **sí**.

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)

5.  En la ventana del **símbolo del sistema** , escriba **net use \\ \\ <Server Name> \c $/User: <Server Name> \Administrador**, donde <Server Name> es el nombre del servidor miembro o de la estación de trabajo a la que está intentando obtener acceso a través de la red.

    > [!NOTE]
    > Las credenciales de administrador local deben estar en el mismo sistema al que está intentando obtener acceso a través de la red.

6.  En la captura de pantalla siguiente se muestra el mensaje de error que debe aparecer.

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como trabajo por lotes"
Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

###### <a name="create-a-batch-file"></a>Crear un archivo por lotes

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

2.  En el cuadro de **búsqueda** , escriba **Notepad**y haga clic en **Bloc de notas**.

3.  En **el Bloc de notas**, escriba **dir c:**.

4.  Haga clic en **archivo**y en **Guardar como**.

5.  En el cuadro **nombre de archivo** , escriba ** <Filename> . bat** (donde <Filename> es el nombre del nuevo archivo por lotes).

###### <a name="schedule-a-task"></a>Programar una tarea

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

2.  En el cuadro de **búsqueda** , escriba programador de tareas y haga clic en **programador de tareas**.

    > [!NOTE]
    > En equipos que ejecutan Windows 8, en el cuadro de **búsqueda** , escriba **programar tareas**y haga clic en **programar tareas**.

3.  Haga clic en **acción**y en **crear tarea**.

4.  En el cuadro de diálogo **crear tarea** , escriba **<Task Name>** (donde <Task Name> es el nombre de la nueva tarea).

5.  Haga clic en la pestaña **acciones** y en **nuevo**.

6.  En el campo **acción** , haga clic en **iniciar un programa**.

7.  En el campo **programa/script** , haga clic en **examinar**, busque y seleccione el archivo por lotes creado en la sección **crear un archivo por lotes** y haga clic en **abrir**.

8.  Haga clic en **Aceptar**.

9. Haga clic en la pestaña **General**.

10. En el campo **Opciones de seguridad** , haga clic en **cambiar usuario o grupo**.

11. Escriba el nombre de la cuenta de administrador local del sistema, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.

12. Seleccione **ejecutar si el usuario ha iniciado sesión o no** y no **almacena la contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.

13. Haga clic en **Aceptar**.

14. Debe aparecer un cuadro de diálogo que solicite las credenciales de la cuenta de usuario para ejecutar la tarea.

15. Después de escribir las credenciales, haga clic en **Aceptar**.

16. Debería aparecer un cuadro de diálogo similar al siguiente.

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como servicio"

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

3.  En el cuadro de **búsqueda** , escriba **servicios**y haga clic en **servicios**.

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.

5.  Haga clic en la ficha **Iniciar sesión**.

6.  En el campo **iniciar sesión como** , haga clic en **esta cuenta**.

7.  Haga clic en **examinar**, escriba la cuenta de administrador local del sistema, haga clic en **Comprobar nombres**y haga clic en **Aceptar**.

8.  En los campos **contraseña** y **Confirmar contraseña** , escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar**.

9. Haga clic en **Aceptar** tres veces más.

10. Haga clic con el botón secundario en **Administrador de trabajos de impresión** y haga clic en **reiniciar**.

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio cola de impresión

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

3.  En el cuadro de **búsqueda** , escriba **servicios**y haga clic en **servicios**.

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.

5.  Haga clic en la ficha **Iniciar sesión**.

6.  En el campo **iniciar sesión como**:, seleccione **local SYSTEMACCOUNT**y haga clic en **Aceptar**.

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión a través de Servicios de Escritorio remoto"

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

2.  En el cuadro de **búsqueda** , escriba **conexión a escritorio remoto**y haga clic en **conexión a escritorio remoto**.

3.  En el campo **equipo** , escriba el nombre del equipo al que desea conectarse y haga clic en **conectar**. (También puede escribir la dirección IP en lugar del nombre del equipo).

4.  Cuando se le solicite, proporcione las credenciales de la cuenta de **Administrador** local del sistema.

5.  Debería aparecer un cuadro de diálogo similar al siguiente.

    ![protección de grupos y cuentas de administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)
