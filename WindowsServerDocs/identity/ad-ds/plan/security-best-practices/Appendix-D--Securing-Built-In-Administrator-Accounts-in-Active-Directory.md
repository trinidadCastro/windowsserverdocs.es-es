---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: Apéndice D - protección de cuentas de administrador integrada en Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 51e503f55ee0fca1f1a53339de555fd213c69296
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870686"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apéndice A: Protección de cuentas de administrador integrada en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apéndice A: Protección de cuentas de administrador integrada en Active Directory  
En cada dominio de Active Directory, se crea una cuenta de administrador como parte de la creación del dominio. Esta cuenta es de forma predeterminada, un miembro de los grupos Admins. del dominio y administradores del dominio y, si el dominio es el dominio raíz del bosque, la cuenta también es un miembro del grupo Administradores de empresas.

Uso de la cuenta de administrador de un dominio se debe reservar solo para las actividades de compilación inicial y, posiblemente, escenarios de recuperación ante desastres. Para asegurarse de que se puede usar una cuenta de administrador para llevar a cabo reparaciones en caso de que no haya otras cuentas que se pueden usar, no debe cambiar la pertenencia predeterminada de la cuenta de administrador en cualquier dominio del bosque. En su lugar, debe proteger la cuenta de administrador en cada dominio del bosque, como se describe en la siguiente sección y detallados en las instrucciones paso a paso siguientes. 

> [!NOTE]
> Esta guía utilizada para recomendar la deshabilitación de la cuenta. Esto se ha quitado como las notas de la recuperación de bosque hace uso de la cuenta de administrador predeterminada. El motivo es que esta es la única cuenta que permite el inicio de sesión sin un servidor de catálogo Global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para las cuentas de administrador integrada  
Para la cuenta predefinida de administrador en cada dominio del bosque, debe establecer la siguiente configuración:  

-   Habilitar la **cuenta es importante y no se puede delegar** marca en la cuenta.  

-   Habilitar la **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marca en la cuenta.  

-   Configurar los GPO para restringir el uso de la cuenta de administrador en los sistemas unidos a dominio:  

    -   En uno o varios GPO que crear y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agregar cuenta de administrador de cada dominio a los siguientes derechos de usuario en **seguridad\Directivas de Windows\Configuración de configuración del equipo\Directivas\Configuración de equipo Locales\Asignación de derechos de asignaciones**:  

        -   Denegar el acceso desde la red a este equipo  

        -   Denegar el inicio de sesión como trabajo por lotes  

        -   Denegar el inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Al agregar cuentas para esta configuración, debe especificar si va a configurar las cuentas de administrador locales o las cuentas de administrador de dominio. Por ejemplo, para agregar la cuenta de administrador del dominio NWTRADERS a estos denegar derechos, debe escribir la cuenta como Nwtraders\Administrador o vaya a la cuenta de administrador para el dominio NWTRADERS. Si escribe "Administrador" en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO.
>   
> Se recomienda limitar las cuentas de administrador locales en los servidores miembro y estaciones de trabajo en la misma manera que las cuentas de administrador basado en dominio. Por lo tanto, generalmente debe agregar la cuenta de administrador para cada dominio del bosque y la cuenta de administrador para los equipos locales para esta configuración de derechos de usuario. Captura de pantalla siguiente muestra un ejemplo de cómo configurar estos derechos de usuario para bloquear las cuentas de administrador locales y la cuenta de administrador de un dominio realizar inicios de sesión que no deberían ser necesario para estas cuentas.  


![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurar GPO para restringir las cuentas de administrador en los controladores de dominio  
    -   En cada dominio del bosque, el GPO de controladores de dominio predeterminado o una directiva vinculado a la unidad organizativa debe modificarse para agregar la cuenta de administrador de cada dominio a los siguientes derechos de usuario en los controladores de dominio **equipo\Directivas\Configuración de equipo Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de asignaciones**:   
        -   Denegar el acceso desde la red a este equipo  

        -   Denegar el inicio de sesión como trabajo por lotes  

        -   Denegar el inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Esta configuración se asegurará de que cuenta predefinida de administrador del dominio no puede utilizarse para conectarse a un controlador de dominio, aunque la cuenta, si está habilitada, puede iniciar sesión localmente en los controladores de dominio. Dado que esta cuenta solo se debería habilitar y usar en escenarios de recuperación ante desastres, se prevé que el acceso físico al menos un controlador de dominio esté disponible o que pueden ser otras cuentas con permisos de acceso remoto a los controladores de dominio usar.  

-   Configurar la auditoría de las cuentas de administrador  

    Cuando haya protegido de la cuenta de administrador de cada dominio y deshabilitado, debe configurar la auditoría para supervisar los cambios en la cuenta. Si la cuenta está habilitada, se restablece su contraseña o cualquier otra modificación se realiza en la cuenta, las alertas se enviarán a los usuarios o los equipos responsables de la administración de Active Directory, además de los equipos de respuesta a incidentes de su organización.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Instrucciones paso a paso para proteger las cuentas de administrador integrada en Active Directory  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **equipos y usuarios de Active Directory**.  

2.  Para evitar ataques que aprovechan la delegación para usar las credenciales de cuenta en otros sistemas, realice los pasos siguientes:  

    1.  Haga clic en el **administrador** cuenta y haga clic en **propiedades**.  

    2.  Haga clic en el **cuenta** ficha.  

    3.  En **opciones de cuenta**, seleccione **cuenta es importante y no se puede delegar** marca como se indica en la siguiente captura de pantalla y haga clic en **Aceptar**.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Para habilitar el **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marca en la cuenta, realice los pasos siguientes:  

    1.  Haga clic en el **administrador** cuenta y seleccione **propiedades**.  

    2.  Haga clic en el **cuenta** ficha.  

    3.  En **cuenta** opciones, seleccionadas el **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marca como se indica en la siguiente captura de pantalla y haga clic en **Aceptar**.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configuración de GPO para restringir las cuentas de administrador en el nivel de dominio  

> [!WARNING]  
> Este GPO nunca debe estar vinculada en el nivel de dominio, ya que puede que la cuenta Administrador integrado no utilizable, incluso en escenarios de recuperación ante desastres.  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **Group Policy Management**.  

2.  En el árbol de consola, expanda <Forest>\Domains\\<Domain>y, a continuación, **Group Policy Objects** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee crear la directiva de grupo).  

3.  En el árbol de consola, haga clic en **Group Policy Objects**y haga clic en **New**.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  En el **nuevo GPO** cuadro de diálogo, escriba <GPO Name>y haga clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO) como se indica en la captura de pantalla siguiente.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  En el panel de detalles, haga clic en <GPO Name>y haga clic en **editar**.  

6.  Vaya a **configuración del equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haga clic en **asignación de derechos de usuario**.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configurar los derechos de usuario para evitar que la cuenta de administrador de acceso a los servidores miembros y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administrador**, haga clic en **comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en <DomainName>\Username formato como se indica en la captura de pantalla siguiente.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

8.  Configurar los derechos de usuario para evitar que la cuenta de administrador de sesión como trabajo por lotes mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administrador**, haga clic en **comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en <DomainName>\Username formato como se indica en la captura de pantalla siguiente.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

9. Configurar los derechos de usuario para evitar que la cuenta de administrador de sesión como un servicio mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administrador**, haga clic en **comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en <DomainName>\Username formato como se indica en la captura de pantalla siguiente.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

10. Configurar los derechos de usuario para evitar que la cuenta de BA accedan a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administrador**, haga clic en **comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en <DomainName>\Username formato como se indica en la captura de pantalla siguiente.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

11. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y haga clic en **salir**.  

12. En **Group Policy Management**, vincule el GPO a unidades organizativas de la estación de trabajo y del servidor miembro haciendo lo siguiente:  

    1.  Navegue hasta la <Forest>\Domains\\ <Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desea establecer la directiva de grupo).  

    2.  Haga clic en la unidad organizativa que se aplicarán a y haga clic en el GPO **vincular un GPO existente**.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Seleccione el GPO que ha creado y haga clic en **Aceptar**.  

        ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Crear vínculos a todas las otras unidades organizativas que contengan las estaciones de trabajo.  

    5.  Crear vínculos a todas las otras unidades organizativas que contengan servidores miembro.  

> [!IMPORTANT]  
> Al agregar la cuenta de administrador para esta configuración, especifique si está configurando una cuenta de administrador local o una cuenta de administrador de dominio cómo etiquetar las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos denegar derechos, deberá navegar a la cuenta de administrador para el dominio TAILSPINTOYS, lo que aparecería como TAILSPINTOYS\Administrator. Si escribe "Administrador" en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

#### <a name="verification-steps"></a>Pasos de comprobación  
Los pasos de comprobación que se describen aquí son específicos de Windows 8 y Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Compruebe que "la tarjeta inteligente es necesaria para el inicio de sesión interactivo" opción de cuenta  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, intente iniciar sesión interactivamente en el dominio mediante el uso de la cuenta predefinida de administrador del dominio. Después de intentar iniciar sesión, debería aparecer un cuadro de diálogo similar al siguiente.  

![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Compruebe "Cuenta está deshabilitada" opción de cuenta  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, intente iniciar sesión interactivamente en el dominio mediante el uso de la cuenta predefinida de administrador del dominio. Después de intentar iniciar sesión, debería aparecer un cuadro de diálogo similar al siguiente.  

![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Compruebe la configuración de GPO de "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (por ejemplo, un servidor de salto), intente acceder a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el uso de la **NET USE** comando realizando los pasos siguientes:  

1.  Inicie sesión en el dominio con la cuenta predefinida de administrador del dominio.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **símbolo**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador** para abrir un elevado símbolo del sistema.  

4.  Cuando se le solicite que aprobar la elevación, haga clic en **Sí**.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  En el **símbolo** ventana, escriba **net use \\ \\ \<nombre del servidor\>\c$**, donde \<nombre del servidor\> es el nombre del servidor miembro o estación de trabajo que está intentando obtener acceso a través de la red.  

6.  Captura de pantalla siguiente muestra el mensaje de error que debe aparecer.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  

Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

###### <a name="create-a-batch-file"></a>Cree un archivo por lotes  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **el Bloc de notas**y haga clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haga clic en **archivo** y haga clic en **Guardar como**.  

5.  En el **Filename** , escriba  **<Filename>.bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **programador de tareas**y haga clic en **programador de tareas**.  

    > [!NOTE]  
    > En los equipos que ejecutan Windows 8, en el cuadro de búsqueda, escriba **programar tareas**y haga clic en **programar tareas**.  

3.  En **programador de tareas**, haga clic en **acción**y haga clic en **crear tarea**.  

4.  En el **crear tarea** cuadro de diálogo, escriba **<Task Name>** (donde **<Task Name>** es el nombre de la nueva tarea).  

5.  Haga clic en el **acciones** ficha y haga clic en **New**.  

6.  En **acción:**, seleccione **iniciar un programa**.  

7.  En **programa o script:**, haga clic en **examinar**, busque y seleccione el archivo por lotes que creó en la sección "Crear un archivo por lotes" y haga clic en **abierto**.  

8.  Haga clic en **Aceptar**.  

9. Haga clic en la pestaña **General**.  

10. En **seguridad** opciones, haga clic en **Cambiar usuario o grupo**.  

11. Escriba el nombre de la cuenta de BA a nivel de dominio, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

12. Seleccione **ejecutar si el usuario inició sesión como no** y **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haga clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo credenciales de cuenta de usuario solicitante para ejecutar la tarea.  

15. Después de escribir las credenciales, haga clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como:**, seleccione **esta cuenta**.  

7.  Haga clic en **examinar**, escriba el nombre de la cuenta de BA en el nivel de dominio, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

8.  En **contraseña:** y **Confirmar contraseña:**, escriba la contraseña de la cuenta de administrador y haga clic en **Aceptar**.  

9. Haga clic en **Aceptar** tres veces más.  

10. Haga clic en el **servicio cola de impresión** y seleccione **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios realizados en el servicio cola de impresión  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como:**, seleccione el **sistema Local** cuenta y haga clic en **Aceptar**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **conexión a Escritorio remoto**y haga clic en **conexión a Escritorio remoto**.  

3.  En el **equipo** , escriba el nombre del equipo que desea conectarse y haga clic en **Connect**. (También puede escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se le solicite, proporcione credenciales para el nombre de la cuenta de BA en el nivel de dominio.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![protección de cuentas de administrador integrada](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
