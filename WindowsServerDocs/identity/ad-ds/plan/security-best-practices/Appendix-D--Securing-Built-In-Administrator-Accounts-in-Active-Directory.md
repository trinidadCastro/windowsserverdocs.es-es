---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: 'Apéndice D: protección de cuentas de administrador integradas en Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cc91ccd00c951863f4f9802f3d669e36ff3f3d9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367839"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apéndice A: Protección de cuentas de administrador integradas en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apéndice A: Protección de cuentas de administrador integradas en Active Directory  
En cada dominio de Active Directory, se crea una cuenta de administrador como parte de la creación del dominio. De forma predeterminada, esta cuenta es miembro de los grupos Admins. del dominio y administradores del dominio, y si el dominio es el dominio raíz del bosque, la cuenta también es miembro del grupo administradores de empresas.

El uso de la cuenta de administrador de un dominio solo se debe reservar para las actividades de compilación iniciales y, posiblemente, en escenarios de recuperación ante desastres. Para asegurarse de que se puede usar una cuenta de administrador para realizar reparaciones en caso de que no se puedan usar otras cuentas, no debe cambiar la pertenencia predeterminada de la cuenta de administrador en ningún dominio del bosque. En su lugar, debe proteger la cuenta de administrador en cada dominio del bosque, tal y como se describe en la sección siguiente, y se detalla en las instrucciones paso a paso que se indican a continuación. 

> [!NOTE]
> Esta guía se usa para recomendar la deshabilitación de la cuenta. Se quitó cuando las notas del producto de recuperación del bosque usan la cuenta de administrador predeterminada. La razón es que esta es la única cuenta que permite el inicio de sesión sin un servidor de catálogo global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para las cuentas predefinidas de administrador  
En el caso de la cuenta de administrador integrada en cada dominio del bosque, debe configurar las siguientes opciones:  

-   Habilite la marca la **cuenta es importante y no se puede delegar** en la cuenta.  

-   La habilitación de la **tarjeta inteligente es necesaria para** la marca de inicio de sesión interactivo en la cuenta.  

-   Configure los GPO para restringir el uso de la cuenta de administrador en sistemas Unidos a un dominio:  

    -   En uno o más GPO creados y vinculados a las unidades organizativas de estaciones de trabajo y servidores miembro de cada dominio, agregue la cuenta de administrador de cada dominio a los siguientes derechos de usuario en el **equipo \ configuración de seguridad\Directivas \ directivas de seguridad\Directivas \ Asignaciones de derechos de usuario**:  

        -   Denegar el acceso desde la red a este equipo  

        -   Denegar el inicio de sesión como trabajo por lotes  

        -   Denegar el inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Al agregar cuentas a esta configuración, debe especificar si va a configurar cuentas de administrador local o cuentas de administrador de dominio. Por ejemplo, para agregar la cuenta de administrador del dominio NWTRADERS a estos derechos de denegación, debe escribir la cuenta como Nwtraders\administrador o buscar la cuenta de administrador del dominio NWTRADERS. Si escribe "administrador" en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, se restringirá la cuenta de administrador local en cada equipo al que se aplique el GPO.
>   
> Se recomienda restringir las cuentas de administrador local en los servidores miembro y las estaciones de trabajo de la misma manera que las cuentas de administrador basadas en dominio. Por lo tanto, normalmente debe agregar la cuenta de administrador para cada dominio del bosque y la cuenta de administrador de los equipos locales a esta configuración de derechos de usuario. En la captura de pantalla siguiente se muestra un ejemplo de configuración de estos derechos de usuario para bloquear cuentas de administrador local y la cuenta de administrador de un dominio para realizar inicios de sesión que no deben ser necesarios para estas cuentas.  


![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurar GPO para restringir cuentas de administrador en controladores de dominio  
    -   En cada dominio del bosque, el GPO de controladores de dominio predeterminados o una directiva vinculada a la unidad organizativa de controladores de dominio se deben modificar para agregar la cuenta de administrador de cada dominio a los siguientes derechos de usuario en el **equipo \ configuración \ Seguridad de Seguridad\directivas**locales \ asignaciones de derechos:   
        -   Denegar el acceso desde la red a este equipo  

        -   Denegar el inicio de sesión como trabajo por lotes  

        -   Denegar el inicio de sesión como servicio  

        -   Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Esta configuración garantizará que la cuenta de administrador integrada del dominio no se puede usar para conectarse a un controlador de dominio, aunque la cuenta, si está habilitada, puede iniciar sesión localmente en los controladores de dominio. Dado que esta cuenta solo se debe habilitar y usar en escenarios de recuperación ante desastres, se prevé que el acceso físico a un controlador de dominio como mínimo estará disponible o que otras cuentas con permisos para tener acceso a los controladores de dominio de forma remota puedan usa.  

-   Configurar la auditoría de cuentas de administrador  

    Una vez que haya protegido la cuenta de administrador de cada dominio y lo haya deshabilitado, debe configurar la auditoría para supervisar los cambios de la cuenta. Si la cuenta está habilitada, se restablece su contraseña o se realizan otras modificaciones en la cuenta, se deben enviar alertas a los usuarios o equipos responsables de la administración de Active Directory, además de los equipos de respuesta a los incidentes de la organización.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Instrucciones paso a paso para proteger las cuentas de administrador integradas en Active Directory  

1.  En **Administrador del servidor**, haga clic en **herramientas**y en **Active Directory usuarios y equipos**.  

2.  Para evitar que los ataques que aprovechan la delegación usen las credenciales de la cuenta en otros sistemas, realice los pasos siguientes:  

    1.  Haga clic con el botón secundario en la cuenta de **Administrador** y haga clic en **propiedades**.  

    2.  Haga clic en la pestaña **cuenta** .  

    3.  En **Opciones de cuenta**, seleccione la opción la **cuenta es importante y no se puede delegar** , como se indica en la siguiente captura de pantalla, y haga clic en **Aceptar**.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Para habilitar la **tarjeta inteligente es necesaria para** la marca de inicio de sesión interactivo en la cuenta, realice los pasos siguientes:  

    1.  Haga clic con el botón secundario en la cuenta de **Administrador** y seleccione **propiedades**.  

    2.  Haga clic en la pestaña **cuenta** .  

    3.  En opciones de **cuenta** , seleccione la **tarjeta inteligente es necesaria para la marca de inicio de sesión interactivo** , como se indica en la siguiente captura de pantalla, y haga clic en **Aceptar**.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configuración de GPO para restringir las cuentas de administrador en el nivel de dominio  

> [!WARNING]  
> Este GPO nunca debe estar vinculado en el nivel de dominio, ya que puede hacer que la cuenta de administrador integrada sea inutilizable, incluso en escenarios de recuperación ante desastres.  

1.  En **Administrador del servidor**, haga clic en **herramientas**y en **Administración de directiva de grupo**.  

2.  En el árbol de consola, expanda <Forest> \ Domains @ no__t-1 @ no__t-2 y, a continuación, **Directiva de grupo objetos** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea crear el Directiva de grupo).  

3.  En el árbol de consola, haga clic con el botón secundario en **Directiva de grupo objetos**y haga clic en **nuevo**.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  En el cuadro de diálogo **nuevo GPO** , escriba <GPO Name> y haga clic en **aceptar** (donde <GPO Name> es el nombre de este GPO) como se indica en la siguiente captura de pantalla.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  En el panel de detalles, haga clic con el botón secundario en <GPO Name> y haga clic en **Editar**.  

6.  Vaya a **equipo \ configuración de Seguridad\directivas \ directivas de seguridad\Directivas**y haga clic en **asignación de derechos de usuario**.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configure los derechos de usuario para impedir que la cuenta de administrador tenga acceso a los servidores y estaciones de trabajo de los miembros a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **Administrador**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en el formato <DomainName> \ nombredeusuario como se indica en la siguiente captura de pantalla.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

8.  Configure los derechos de usuario para impedir que la cuenta de administrador inicie sesión como un trabajo por lotes haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **Administrador**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en el formato <DomainName> \ nombredeusuario como se indica en la siguiente captura de pantalla.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

9. Configure los derechos de usuario para impedir que la cuenta de administrador inicie sesión como servicio haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **Administrador**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en el formato <DomainName> \ nombredeusuario como se indica en la siguiente captura de pantalla.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

10. Configure los derechos de usuario para impedir que la cuenta de BA tenga acceso a los servidores y las estaciones de trabajo miembro a través de Servicios de Escritorio remoto haciendo lo siguiente:  

    1.  Haga doble clic en **denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **Administrador**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**. Compruebe que la cuenta se muestra en el formato <DomainName> \ nombredeusuario como se indica en la siguiente captura de pantalla.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

11. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y, a continuación, en **salir**.  

12. En **Administración de directiva de grupo**, VINCULE el GPO al servidor miembro y las unidades organizativas de la estación de trabajo haciendo lo siguiente:  

    1.  Desplácese hasta el <Forest> \ Domains @ no__t-1 @ no__t-2 (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea establecer el directiva de grupo).  

    2.  Haga clic con el botón secundario en la unidad organizativa a la que se aplicará el GPO y haga clic en **vincular un GPO existente**.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Seleccione el GPO que ha creado y haga clic en **Aceptar**.  

        ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Cree vínculos a todas las demás unidades organizativas que contengan estaciones de trabajo.  

    5.  Cree vínculos a todas las demás unidades organizativas que contengan servidores miembro.  

> [!IMPORTANT]  
> Al agregar la cuenta de administrador a esta configuración, se especifica si se va a configurar una cuenta de administrador local o una cuenta de administrador de dominio mediante el etiquetado de las cuentas. Por ejemplo, para agregar la cuenta de administrador del dominio TAILSPINTOYS a estos derechos de denegación, debe ir a la cuenta de administrador del dominio TAILSPINTOYS, que aparecería como TAILSPINTOYS\Administrator. Si escribe "administrador" en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringe la cuenta de administrador local en cada equipo al que se aplica el GPO, como se describió anteriormente.  

#### <a name="verification-steps"></a>Pasos de comprobación  
Los pasos de comprobación que se describen aquí son específicos de Windows 8 y Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Opción de cuenta "se requiere tarjeta inteligente para el inicio de sesión interactivo"  

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, intente iniciar sesión interactivamente en el dominio mediante la cuenta de administrador integrada del dominio. Después de intentar iniciar sesión, debe aparecer un cuadro de diálogo similar al siguiente.  

![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Opción de cuenta comprobar que la cuenta está deshabilitada  

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, intente iniciar sesión interactivamente en el dominio mediante la cuenta de administrador integrada del dominio. Después de intentar iniciar sesión, debe aparecer un cuadro de diálogo similar al siguiente.  

![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se vea afectado por los cambios de GPO (como un servidor de salto), intente tener acceso a un servidor miembro o una estación de trabajo a través de la red afectada por los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el comando **net use** mediante los pasos siguientes:  

1.  Inicie sesión en el dominio con la cuenta de administrador integrada del dominio.  

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

3.  En el cuadro de **búsqueda** , escriba **símbolo del sistema**, haga clic con el botón secundario en símbolo del **sistema**y, a continuación, haga clic en **Ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.  

4.  Cuando se le pida que apruebe la elevación, haga clic en **sí**.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  En la ventana del **símbolo del sistema** , escriba **net use \\ @ no__t-3 @ No__t-4Server Name @ no__t-5\c $** , donde \<Server Name @ no__t-7 es el nombre del servidor miembro o de la estación de trabajo a la que está intentando obtener acceso a través de la red.  

6.  En la captura de pantalla siguiente se muestra el mensaje de error que debe aparecer.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como trabajo por lotes"  

Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.  

###### <a name="create-a-batch-file"></a>Crear un archivo por lotes  

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

2.  En el cuadro de **búsqueda** , escriba **Notepad**y haga clic en **Bloc de notas**.  

3.  En **el Bloc de notas**, escriba **dir c:** .  

4.  Haga clic en **archivo** y en **Guardar como**.  

5.  En el campo **nombre de archivo** , escriba **@no__t -2. bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

2.  En el cuadro de **búsqueda** , escriba **programador de tareas**y haga clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en el cuadro de búsqueda, escriba **programar tareas**y haga clic en **programar tareas**.  

3.  En **programador de tareas**, haga clic en **acción**y en **crear tarea**.  

4.  En el cuadro de diálogo **crear tarea** , escriba **<Task Name>** (donde **<Task Name>** es el nombre de la nueva tarea).  

5.  Haga clic en la pestaña **acciones** y en **nuevo**.  

6.  En **acción:** , seleccione **iniciar un programa**.  

7.  En **programa/script:** , haga clic en **examinar**, busque y seleccione el archivo por lotes creado en la sección "crear un archivo por lotes" y haga clic en **abrir**.  

8.  Haga clic en **Aceptar**.  

9. Haga clic en la pestaña **General**.  

10. En opciones de **seguridad** , haga clic en **cambiar usuario o grupo**.  

11. Escriba el nombre de la cuenta de BA en el nivel de dominio, haga clic en **Comprobar nombres**y haga clic en **Aceptar**.  

12. Seleccione **ejecutar si el usuario ha iniciado sesión o no** y no **almacena la contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haga clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo que solicite las credenciales de la cuenta de usuario para ejecutar la tarea.  

15. Después de escribir las credenciales, haga clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

3.  En el cuadro de **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como:** , seleccione **esta cuenta**.  

7.  Haga clic en **examinar**, escriba el nombre de la cuenta de BA en el nivel de dominio, haga clic en **Comprobar nombres**y haga clic en **Aceptar**.  

8.  En **contraseña:** y **Confirmar contraseña:** , escriba la contraseña de la cuenta de administrador y haga clic en **Aceptar**.  

9. Haga clic en **Aceptar** tres veces más.  

10. Haga clic con el botón secundario en el **servicio Administrador de trabajos de impresión** y seleccione **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio cola de impresión  

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

3.  En el cuadro de **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como:** , seleccione la cuenta de **sistema local** y haga clic en **Aceptar**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión a través de Servicios de Escritorio remoto"

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

2.  En el cuadro de **búsqueda** , escriba **conexión a escritorio remoto**y haga clic en **conexión a escritorio remoto**.  

3.  En el campo **equipo** , escriba el nombre del equipo al que desea conectarse y haga clic en **conectar**. (También puede escribir la dirección IP en lugar del nombre del equipo).  

4.  Cuando se le solicite, proporcione las credenciales del nombre de la cuenta de BA en el nivel de dominio.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![protección de cuentas de administrador integradas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
