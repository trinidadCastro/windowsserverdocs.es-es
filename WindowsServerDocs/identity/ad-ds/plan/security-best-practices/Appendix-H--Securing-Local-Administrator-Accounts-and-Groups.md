---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: Apéndice H - protección de cuentas de administrador Local y grupos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 71eea3f623968172076708dbea34d5bbf4a07684
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858696"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apéndice H: Protección de cuentas de administrador Local y grupos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apéndice H: Protección de cuentas de administrador Local y grupos  
En todas las versiones de Windows actualmente en soporte estándar, la cuenta de administrador local está deshabilitada de forma predeterminada, lo que hace que la cuenta de que quede inutilizable para pass-the-hash y otros ataques de robo de credenciales. Sin embargo, en entornos que contienen sistemas operativos heredados o en el que se han habilitado las cuentas de administrador locales, estas cuentas pueden usarse como se describió anteriormente para propagar compromiso entre los servidores miembro y estaciones de trabajo. Cada grupo o cuenta de administrador local deben protegerse como se describe en las instrucciones paso a paso siguientes.  

Para obtener información detallada acerca de las consideraciones de protección de grupos de administrador integrados (BA), consulte [implementar modelos administrativos de menor privilegio](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para las cuentas de administrador Local  
Para la cuenta de administrador local en cada dominio del bosque, debe establecer la siguiente configuración:  

-   Configurar GPO para restringir el uso de la cuenta de administrador del dominio en sistemas unidos a dominio  
    -   En uno o varios GPO que crear y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agregue la cuenta de administrador a los siguientes derechos de usuario en **Policies\ de seguridad\Directivas de Windows\Configuración de configuración del equipo\Directivas\Configuración de equipo Las asignaciones de derechos de usuario**:  

        -   Denegar el acceso desde la red a este equipo  

        -   Denegar el inicio de sesión como trabajo por lotes  

        -   Denegar el inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de Servicios de Escritorio remoto  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Instrucciones paso a paso para proteger grupos de administradores locales  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configuración de GPO para restringir la cuenta de administrador en los sistemas unidos a dominio  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **Group Policy Management**.  

2.  En el árbol de consola, expanda <Forest>\Domains\\<Domain>y, a continuación, **Group Policy Objects** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee establecer la directiva de grupo).  

3.  En el árbol de consola, haga clic en **Group Policy Objects**y haga clic en **New**.  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  En el **nuevo GPO** cuadro de diálogo, escriba **<GPO Name>** y haga clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  En el panel de detalles, haga clic en **<GPO Name>** y haga clic en **editar**.  

6.  Vaya a **configuración del equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haga clic en **asignación de derechos de usuario**.  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configurar los derechos de usuario para impedir que la cuenta de administrador local tenga acceso a los servidores miembros y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Haga clic en Aceptar.  

        > [!IMPORTANT]  
        > Al agregar la cuenta de administrador para esta configuración, especifique si está configurando una cuenta de administrador local o una cuenta de administrador de dominio cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos denegar derechos, deberá navegar a la cuenta de administrador para el dominio TAILSPINTOYS, lo que aparecería como TAILSPINTOYS\Administrator. Si escribe **administrador** en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

8.  Configurar los derechos de usuario para evitar que la cuenta de administrador local pueda iniciar sesión como trabajo por lotes mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Haga clic en **Aceptar**.  

        > [!IMPORTANT]  
        > Al agregar la cuenta de administrador para esta configuración, especifique si va a configurar la cuenta de administrador local o cuenta de administrador de dominio por cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos denegar derechos, deberá navegar a la cuenta de administrador para el dominio TAILSPINTOYS, lo que aparecería como TAILSPINTOYS\Administrator. Si escribe **administrador** en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

9. Configurar los derechos de usuario para evitar que la cuenta de administrador local pueda iniciar sesión como servicio mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Haga clic en **Aceptar**.  

        > [!IMPORTANT]  
        > Al agregar la cuenta de administrador para esta configuración, especifique si va a configurar la cuenta de administrador local o cuenta de administrador de dominio por cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos denegar derechos, deberá navegar a la cuenta de administrador para el dominio TAILSPINTOYS, lo que aparecería como TAILSPINTOYS\Administrator. Si escribe **administrador** en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

10. Configurar los derechos de usuario para impedir que la cuenta de administrador local tenga acceso a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo**, escriba el nombre de usuario de la cuenta de administrador local y haga clic en **Aceptar**. Este nombre de usuario será **administrador**, el valor predeterminado cuando se instala Windows.  

        ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Haga clic en **Aceptar**.  

        > [!IMPORTANT]  
        > Al agregar la cuenta de administrador para esta configuración, especifique si va a configurar la cuenta de administrador local o cuenta de administrador de dominio por cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos denegar derechos, deberá navegar a la cuenta de administrador para el dominio TAILSPINTOYS, lo que aparecería como TAILSPINTOYS\Administrator. Si escribe **administrador** en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

11. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y haga clic en **salir**.  

12. En **Group Policy Management**, vincule el GPO a unidades organizativas de la estación de trabajo y del servidor miembro haciendo lo siguiente:  

    1.  Navegue hasta la <Forest>\Domains\\ <Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desea establecer la directiva de grupo).  

    2.  Haga clic en la unidad organizativa que se aplicarán a y haga clic en el GPO **vincular un GPO existente**.  

        ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Seleccione el GPO que ha creado y haga clic en **Aceptar**.  

        ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Crear vínculos a todas las otras unidades organizativas que contengan las estaciones de trabajo.  

    5.  Crear vínculos a todas las otras unidades organizativas que contengan servidores miembro.  

#### <a name="verification-steps"></a>Pasos de comprobación  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Compruebe la configuración de GPO de "Denegar el acceso a este equipo desde la red"  

Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (por ejemplo, un servidor de salto), intente acceder a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el uso de la **NET USE** comando.  

1.  Inicie sesión localmente en cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **símbolo**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador** para abrir un elevado símbolo del sistema.  

4.  Cuando se le solicite que aprobar la elevación, haga clic en **Sí**.  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  En el **símbolo** ventana, escriba **net use \\ \\ <Server Name>\c$ /user:<Server Name>\Administrator**, donde <Server Name> es el nombre del miembro servidor o estación de trabajo que está intentando tener acceso a través de la red.  

    > [!NOTE]  
    > Las credenciales de administrador locales deben ser del mismo sistema que está intentando tener acceso a través de la red.  

6.  Captura de pantalla siguiente muestra el mensaje de error que debe aparecer.  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  
Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

###### <a name="create-a-batch-file"></a>Cree un archivo por lotes  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **el Bloc de notas**y haga clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haga clic en **archivo**y haga clic en **Guardar como**.  

5.  En el **nombre de archivo** , escriba  **<Filename>.bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** cuadro, escriba el programador de tareas y haga clic en **programador de tareas**.  

    > [!NOTE]  
    > En los equipos que ejecutan Windows 8, en el **búsqueda** , escriba **programar tareas**y haga clic en **programar tareas**.  

3.  Haga clic en **acción**y haga clic en **crear tarea**.  

4.  En el **crear tarea** cuadro de diálogo, escriba **<Task Name>** (donde <Task Name> es el nombre de la nueva tarea).  

5.  Haga clic en el **acciones** ficha y haga clic en **New**.  

6.  En el **acción** , a continuación, haga clic en **iniciar un programa**.  

7.  En el **programa o script** , a continuación, haga clic en **examinar**, busque y seleccione el archivo por lotes creado en el **crear un archivo por lotes** sección y haga clic en **abrir**.  

8.  Haga clic en **Aceptar**.  

9. Haga clic en la pestaña **General**.  

10. En el **las opciones de seguridad** , a continuación, haga clic en **Cambiar usuario o grupo**.  

11. Escriba el nombre de cuenta de administrador local del sistema, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

12. Seleccione **ejecutar si el usuario inició sesión como no** y **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haga clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo credenciales de cuenta de usuario solicitante para ejecutar la tarea.  

15. Después de escribir las credenciales, haga clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como** , a continuación, haga clic en **esta cuenta**.  

7.  Haga clic en **examinar**, escriba la cuenta de administrador local del sistema, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

8.  En el **contraseña** y **Confirmar contraseña** campos, escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar**.  

9. Haga clic en **Aceptar** tres veces más.  

10. Haga clic en **impresión** y haga clic en **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios realizados en el servicio cola de impresión  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En el **iniciar sesión como**: campo, seleccione **Systemaccount Local**y haga clic en **Aceptar**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **conexión a Escritorio remoto**y haga clic en **conexión a Escritorio remoto**.  

3.  En el **equipo** , escriba el nombre del equipo que desea conectarse y haga clic en **Connect**. (También puede escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se le solicite, proporcione credenciales para el sistema local **administrador** cuenta.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos y cuentas de administrador local segura](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
