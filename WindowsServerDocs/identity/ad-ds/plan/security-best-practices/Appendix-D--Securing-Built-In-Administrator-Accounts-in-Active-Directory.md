---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: "Apéndice D: - seguridad de las cuentas de administrador integrada en Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2878e661e1bb93fcdc3161c46b73d8e4baec76d2
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2018
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apéndice D: seguridad de las cuentas de administrador integrada en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apéndice D: seguridad de las cuentas de administrador integrada en Active Directory  
En cada dominio de Active Directory, se crea una cuenta de administrador como parte de la creación del dominio. Esta cuenta es de manera predeterminada, un miembro de los grupos Administradores de dominio y administradores del dominio y, si el dominio es el dominio raíz, la cuenta también es un miembro del grupo Administradores de empresa.

Uso de la cuenta de administrador de un dominio debe reservarse únicamente para actividades de la compilación inicial y, posiblemente, los escenarios de recuperación ante desastres. Para garantizar que una cuenta de administrador puede usarse para realizar reparaciones en caso de que ninguna otra cuenta puede usarse, no debe cambiar la pertenencia predeterminada de la cuenta de administrador en cualquier dominio del bosque. En su lugar, debe proteger la cuenta Administrador en cada dominio del bosque como se describe en la sección siguiente y se detalla en las instrucciones paso a paso que siguen. 

> [!NOTE]
> Esta guía se usa para recomendar deshabilitar la cuenta. Esto se quitó como las notas de recuperación del bosque hace uso de la cuenta de administrador predeterminada. El motivo es que esta cuenta es la única que permite el inicio de sesión sin un servidor de catálogo Global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para las cuentas de administrador predefinida  
Para que la cuenta predefinida de administrador en cada dominio del bosque, debes configurar la siguiente configuración:  

-   Habilitar la **cuenta es confidencial y no se puede delegar** marca en la cuenta.  

-   Habilitar la **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marca en la cuenta.  

-   Configurar los GPO para restringir el uso de la cuenta de administrador en sistemas unidos a un dominio:  

    -   En uno o más GPO que crean y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agrega la cuenta de administrador de cada dominio a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:  

        -   Denegar el acceso a este equipo desde la red  

        -   Denegar inicio de sesión como trabajo por lotes  

        -   Denegar inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de servicios de escritorio remoto  

> [!NOTE]  
> Al agregar cuentas a esta configuración, debes especificar si va a configurar cuentas de administrador locales o cuentas de administrador de dominio. Por ejemplo agregar la cuenta de administrador del dominio NWTRADERS estos denegar derechos de propiedad, debes escribir la cuenta como Nwtraders\Administrador o ve a la cuenta de administrador para el dominio NWTRADERS. Si escribe "Administrador" en estas opciones de configuración de derechos de usuario en el Editor de objetos de directiva de grupo, se limitará la cuenta de administrador local en cada equipo al que se aplica el GPO.
>   
> Se recomienda limitar las cuentas de administrador locales en servidores miembro y estaciones de trabajo en la misma manera que las cuentas de administrador de dominio. Por lo tanto, por lo general, debes agregar la cuenta de administrador para cada dominio en el bosque y la cuenta de administrador para los equipos locales para estas opciones de configuración de derechos de usuario. Captura de pantalla siguiente muestra un ejemplo de configurar estos derechos de usuario para bloquear las cuentas de administrador locales y cuenta de administrador de un dominio de la realización de inicios de sesión que no deberían ser necesario para estas cuentas.  


![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurar los GPO para restringir las cuentas de administrador en controladores de dominio  
    -   En cada dominio en el bosque, el GPO de controladores de dominio predeterminada o una directiva vinculado a los controladores de dominio, unidad organizativa debe modificarse para agregar la cuenta de administrador de cada dominio a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:   
        -   Denegar el acceso a este equipo desde la red  

        -   Denegar inicio de sesión como trabajo por lotes  

        -   Denegar inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de servicios de escritorio remoto  

> [!NOTE]  
> Estas opciones de configuración se garantizarán que cuenta predefinida de administrador de dominio no puede usarse para conectarse a un controlador de dominio, aunque la cuenta, si se habilita, puede iniciar sesión localmente en controladores de dominio. Dado que esta cuenta solo debe habilitada y usarse en escenarios de recuperación ante desastres, se prevé que esté disponible el acceso físico al menos un controlador de dominio o que se pueden utilizar otras cuentas con permisos de acceso a controladores de dominio de forma remota.  

-   Configurar la auditoría de cuentas de administrador  

    Cuando se protege la cuenta de administrador de cada dominio y se deshabilitado, debe configurar la auditoría para supervisar los cambios en la cuenta. Si se habilita la cuenta, se restablece la contraseña o cualquier otra modificación se realiza en la cuenta, se deben enviar alertas a los usuarios o equipos responsables de administración de Active Directory, además de los equipos de respuesta a incidentes de la organización.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Instrucciones paso a paso para proteger las cuentas de administrador integrada en Active Directory  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **equipos y usuarios de Active Directory**.  

2.  Para evitar ataques que aprovechan la delegación para usar las credenciales de cuenta en otros sistemas, realiza los siguientes pasos:  

    1.  Haz clic en el **administrador** cuenta y haz clic en **propiedades**.  

    2.  Haz clic en el **cuenta** pestaña.  

    3.  En **opciones de cuenta**, selecciona **cuenta es confidencial y no se puede delegar** marcar como se indica en la siguiente captura de pantalla y haz clic en **Aceptar**.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Para habilitar la **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marca en la cuenta, realiza los siguientes pasos:  

    1.  Haz clic en el **administrador** cuenta y selecciona **propiedades**.  

    2.  Haz clic en el **cuenta** pestaña.  

    3.  En **cuenta** opciones, seleccionadas la **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marcar como se indica en la siguiente captura de pantalla y haz clic en **Aceptar**.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configuración de GPO para restringir las cuentas de administrador en el nivel de dominio  

> [!WARNING]  
> Este GPO nunca debe vincularse a nivel de dominio porque hace que la cuenta Administrador predefinida no utilizable, incluso en los escenarios de recuperación ante desastres.  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **Group Policy Management**.  

2.  En el árbol de consola, expande <Forest>\Domains\\<Domain>y luego **objetos de directiva de grupo** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee crear la directiva de grupo).  

3.  En el árbol de consola, haz clic en **objetos de directiva de grupo**y haz clic en **nueva**.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  En la **nuevo GPO** cuadro de diálogo, escribe <GPO Name>y haz clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO) como se indica en la siguiente captura de pantalla.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  En el panel de detalles, haz clic en <GPO Name>y haz clic en **editar**.  

6.  Navegar a **equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haz clic en **asignación de derechos de usuario**.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configurar los derechos de usuario para evitar que la cuenta de administrador de acceder a los servidores miembros y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haz doble clic en **denegar el acceso a este equipo desde la red** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administrador**, haz clic en **comprobar nombres**y haz clic en **Aceptar**. Comprueba que la cuenta se muestra en <DomainName>\Username formato como se indica en la siguiente captura de pantalla.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

8.  Configurar los derechos de usuario para evitar que la cuenta de administrador de iniciar sesión como trabajo por lotes haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como trabajo por lotes** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administrador**, haz clic en **comprobar nombres**y haz clic en **Aceptar**. Comprueba que la cuenta se muestra en <DomainName>\Username formato como se indica en la siguiente captura de pantalla.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

9. Configurar los derechos de usuario para evitar que la cuenta de administrador de iniciar sesión como servicio haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como servicio** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administrador**, haz clic en **comprobar nombres**y haz clic en **Aceptar**. Comprueba que la cuenta se muestra en <DomainName>\Username formato como se indica en la siguiente captura de pantalla.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

10. Configurar los derechos de usuario para evitar que la cuenta BA accedan a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administrador**, haz clic en **comprobar nombres**y haz clic en **Aceptar**. Comprueba que la cuenta se muestra en <DomainName>\Username formato como se indica en la siguiente captura de pantalla.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

11. Para salir **el Editor de administración de directivas de grupo**, haz clic en **archivo**y haz clic en **salir**.  

12. En **Group Policy Management**, vincular el GPO a los servidores miembros y unidades organizativas de la estación de trabajo mediante las siguientes acciones:  

    1.  Navegar a la <Forest>\Domains\\<Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

    2.  Haz clic en la unidad organizativa que el GPO se aplicarán a y haz clic en **vincular un GPO existente**.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Selecciona el GPO que creaste y haz clic en **Aceptar**.  

        ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Crea vínculos a todas las otras unidades organizativas que contienen las estaciones de trabajo.  

    5.  Crea vínculos a todas las otras unidades organizativas que contienen los servidores miembro.  

> [!IMPORTANT]  
> Cuando agregas la cuenta de administrador para estas opciones de configuración, especifican si va a configurar una cuenta de administrador local o una cuenta de administrador de dominio mediante cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS estos denegar derechos, navegas a la cuenta de administrador para el dominio TAILSPINTOYS, que aparecieran como TAILSPINTOYS\Administrator. Si escribe "Administrador" en estas opciones de configuración de derechos de usuario en el Editor de objetos de directiva de grupo, se limitará la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

#### <a name="verification-steps"></a>Pasos de verificación  
Los pasos de verificación descritos aquí son específicos para Windows 8 y Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Comprobar "tarjeta inteligente es necesaria para el inicio de sesión interactivo" opción de cuenta  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, intenta iniciar sesión interactiva en el dominio mediante el uso de la cuenta predefinida de administrador del dominio. Después de intentar iniciar sesión, debe aparecer un cuadro de diálogo similar al siguiente.  

![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Comprobar "La cuenta está deshabilitada" opción de cuenta  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, intenta iniciar sesión interactiva en el dominio mediante el uso de la cuenta predefinida de administrador del dominio. Después de intentar iniciar sesión, debe aparecer un cuadro de diálogo similar al siguiente.  

![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (como un servidor de accesos directos), intenta obtener acceso a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración de GPO, intenta asignar la unidad del sistema mediante el **NET USE** comando, efectuando los pasos siguientes:  

1.  Iniciar sesión en el dominio mediante la cuenta predefinida de administrador del dominio.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **símbolo**, haz clic en **símbolo**y, a continuación, haz clic en **ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.  

4.  Cuando se te solicite para aprobar la elevación, haz clic en **Sí**.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  En la **símbolo** ventana, escribe **net use \\\<Server Name>\c$**, donde <Server Name> es el nombre del servidor miembro o estación de trabajo que estás intentando acceder a través de la red.  

6.  Captura de pantalla siguiente muestra el mensaje de error que debe aparecer.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  

Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

###### <a name="create-a-batch-file"></a>Crear un archivo por lotes  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **el Bloc de notas**y haz clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haz clic en **archivo** y haz clic en **Guardar como**.  

5.  En la **nombre de archivo** , escriba ** <Filename>.bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **programador de tareas**y haz clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en el cuadro de búsqueda, escriba **programar tareas**y haz clic en **programar tareas**.  

3.  En **programador de tareas**, haz clic en **acción**y haz clic en **crear tarea**.  

4.  En la **crear tarea** cuadro de diálogo, escribe ** <Task Name> ** (donde ** <Task Name> ** es el nombre de la nueva tarea).  

5.  Haz clic en el **acciones** pestaña y haz clic en **nueva**.  

6.  En **acción:**, selecciona **iniciar un programa**.  

7.  En **programa o script:**, haz clic en **examinar**, busca y selecciona el archivo por lotes que creaste en la sección "Crear un archivo por lotes" y haz clic en **abrir**.  

8.  Haz clic en **Aceptar**.  

9. Haz clic en el **General** pestaña.  

10. En **seguridad** opciones, haga clic en **Cambiar usuario o grupo**.  

11. Escribe el nombre de la cuenta BA en el nivel de dominio, haga clic en **comprobar nombres**y haz clic en **Aceptar**.  

12. Selecciona **ejecutar si el usuario ha iniciado sesión o no** y **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haz clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo, cuenta de usuario solicita credenciales para ejecutar la tarea.  

15. Después de escribir las credenciales, haz clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En **iniciar sesión como:**, selecciona **esta cuenta**.  

7.  Haz clic en **examinar**, escribe el nombre de la cuenta BA en el nivel de dominio, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

8.  En **contraseña:** y **Confirmar contraseña:**, escribe la contraseña de la cuenta de administrador y haz clic en **Aceptar**.  

9. Haz clic en **Aceptar** tres veces más.  

10. Haz clic en el **servicio cola de impresión** y selecciona **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio de la cola de impresora  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En **iniciar sesión como:**, selecciona el **sistema Local** cuenta y haz clic en **Aceptar**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **conexión a Escritorio remoto**y haz clic en **conexión a Escritorio remoto**.  

3.  En la **equipo** , a continuación, escribe el nombre del equipo que desea conectarse y haz clic en **conectar**. (También puedes escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se te solicite, proporcionar credenciales para el nombre de la cuenta BA en el nivel de dominio.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![seguridad de las cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
