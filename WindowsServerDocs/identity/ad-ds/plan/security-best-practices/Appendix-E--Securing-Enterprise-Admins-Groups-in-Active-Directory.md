---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: Apéndice E - protección de grupos de administradores de empresa en Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 976eb8c7159c8349b72bee05a5248b5cc116d96b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856726"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apéndice E: Protección de grupos de administradores de empresa en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apéndice E: Protección de grupos de administradores de empresa en Active Directory  
El grupo de administradores de Enterprise (EA), que se aloja en el dominio raíz del bosque, no debe contener ningún usuario en forma diaria, con la excepción posible de la cuenta de administrador del dominio raíz, siempre que se protege como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Son administradores de empresas, de forma predeterminada, los miembros del grupo Administradores en cada dominio del bosque. No debería quitar el grupo de EA de los grupos de administradores en cada dominio ya en el caso de un escenario de recuperación ante desastres de bosque, derechos EA probablemente será necesarios. Grupo de administradores de empresas del bosque debe protegerse tal como se detalla en las instrucciones paso a paso que siguen.  

Para el grupo de administradores de empresas del bosque:  

1.  En los GPO vinculados a unidades organizativas que contiene los servidores miembro y estaciones de trabajo en cada dominio, se debe agregar el grupo de administradores de organización para los siguientes derechos de usuario en **Policies\ de seguridad\Directivas de Windows\Configuración de configuración del equipo\Directivas\Configuración de equipo Las asignaciones de derechos de usuario**:  

    -   Denegar el acceso desde la red a este equipo  

    -   Denegar el inicio de sesión como trabajo por lotes  

    -   Denegar el inicio de sesión como servicio  

    -   Denegar el inicio de sesión localmente  

    -   Denegar inicio de sesión a través de Servicios de Escritorio remoto  

2.  Configurar la auditoría para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia del grupo Administradores de empresas.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Instrucciones paso a paso para quitar a todos los miembros del grupo de administradores de empresa  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **equipos y usuarios de Active Directory**.  

2.  Si no está administrando el dominio raíz del bosque, en el árbol de consola, haga clic en <Domain>y, a continuación, haga clic en **cambiar dominio** (donde <Domain> es el nombre del dominio que está administrando actualmente).  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  En el **cambiar dominio** cuadro de diálogo, haga clic en **examinar**, seleccione el dominio raíz del bosque y haga clic en **Aceptar**.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Para quitar a todos los miembros del grupo de EA:  

    1.  Haga doble clic en el **administradores de empresas** de grupo y, a continuación, haga clic en el **miembros** ficha.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Seleccione un miembro del grupo, haga clic en **quitar**, haga clic en **Sí**y haga clic en **Aceptar**.  

5.  Repita el paso 2 hasta que se han quitado todos los miembros del grupo de EA.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Instrucciones paso a paso para proteger los administradores de empresas en Active Directory  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **Group Policy Management**.  

2.  En el árbol de consola, expanda <Forest>\Domains\\<Domain>y, a continuación, **Group Policy Objects** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee establecer la directiva de grupo).  

    > [!NOTE]  
    > En un bosque que contiene varios dominios, se debe crear un GPO similar en cada dominio que requiere que el grupo de administradores de empresa están protegidos.  

3.  En el árbol de consola, haga clic en **Group Policy Objects**y haga clic en **New**.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  En el **nuevo GPO** cuadro de diálogo, escriba <GPO Name>y haga clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  En el panel de detalles, haga clic en <GPO Name>y haga clic en **editar**.  

6.  Vaya a **configuración del equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haga clic en **asignación de derechos de usuario**.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de organización tengan acceso a los servidores miembro y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administradores de empresas**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

8.  Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de organización inician sesión como trabajo por lotes mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.  

    3.  Tipo **administradores de empresas**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

9. Configurar los derechos de usuario para impedir que los miembros del grupo de EA inician sesión como un servicio mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, haga clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.  

    3.  Tipo **administradores de empresas**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

10. Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de organización iniciar sesión localmente en los servidores miembro y estaciones de trabajo mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión localmente** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, haga clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.  

    3.  Tipo **administradores de empresas**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

11. Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de organización tengan acceso a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, haga clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.  

    3.  Tipo **administradores de empresas**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

12. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y haga clic en **salir**.  

13. En **Group Policy Management**, vincule el GPO a unidades organizativas de la estación de trabajo y del servidor miembro haciendo lo siguiente:  

    1.  Navegue hasta la <Forest>\Domains\\ <Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desea establecer la directiva de grupo).  

    2.  Haga clic en la unidad organizativa que se aplicarán a y haga clic en el GPO **vincular un GPO existente**.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar**.  

        ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Crear vínculos a todas las otras unidades organizativas que contengan las estaciones de trabajo.  

    5.  Crear vínculos a todas las otras unidades organizativas que contengan servidores miembro.  

    6.  En un bosque que contiene varios dominios, se debe crear un GPO similar en cada dominio que requiere que el grupo de administradores de empresa están protegidos.  

> [!IMPORTANT]  
> Si los servidores de salto se usan para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no está vinculado el GPO.  

### <a name="verification-steps"></a>Pasos de comprobación  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Compruebe la configuración de GPO de "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (por ejemplo, "servidor de salto"), intente acceder a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el uso de la **NET USE** comando realizando los pasos siguientes:  

1.  Inicie sesión localmente con una cuenta que sea miembro del grupo de EA.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **símbolo**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador** para abrir un elevado símbolo del sistema.  

4.  Cuando se le solicite que aprobar la elevación, haga clic en **Sí**.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  En el **símbolo** ventana, escriba **net use \\ \\ \<nombre del servidor\>\c$**, donde \<nombre del servidor\> es el nombre del servidor miembro o estación de trabajo que está intentando tener acceso a través de la red.  

6.  Captura de pantalla siguiente muestra el mensaje de error que debe aparecer.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  

Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

##### <a name="create-a-batch-file"></a>Cree un archivo por lotes  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **el Bloc de notas**y haga clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haga clic en **archivo**y haga clic en **Guardar como**.  

5.  En el **archivo** cuadro Nombre, escriba  **<Filename>.bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

##### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **programador de tareas**y haga clic en **programador de tareas**.  

    > [!NOTE]  
    > En los equipos que ejecutan Windows 8, en el **búsqueda** , escriba **programar tareas**y haga clic en **programar tareas**.  

3.  Haga clic en **acción**y haga clic en **crear tarea**.  

4.  En el **crear tarea** cuadro de diálogo, escriba **<Task Name>** (donde <Task Name> es el nombre de la nueva tarea).  

5.  Haga clic en el **acciones** ficha y haga clic en **New**.  

6.  En el **acción** campos, seleccione **iniciar un programa**.  

7.  En **programa o script**, haga clic en **examinar**, busque y seleccione el archivo por lotes creado en el **crear un archivo por lotes** sección y haga clic en **abrir**.  

8.  Haga clic en **Aceptar**.  

9. Haga clic en la pestaña **General**.  

10. En el **las opciones de seguridad** , a continuación, haga clic en **Cambiar usuario o grupo**.  

11. Escriba el nombre de una cuenta que sea miembro del grupo de atributos extendidos, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

12. Seleccione **ejecutar si el usuario inició sesión como no** y seleccione **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haga clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo credenciales de cuenta de usuario solicitante para ejecutar la tarea.  

15. Después de escribir las credenciales, haga clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como**, seleccione **esta cuenta**.  

7.  Haga clic en **examinar**, escriba el nombre de una cuenta que sea miembro del grupo de atributos extendidos, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

8.  En **contraseña:** y **Confirmar contraseña**, escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar**.  

9. Haga clic en **Aceptar** tres veces más.  

10. Haga clic en el **impresión** de servicio y seleccione **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios realizados en el servicio cola de impresión  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como**, seleccione el **sistema Local** cuenta y haga clic en **Aceptar**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Compruebe la configuración de GPO "Denegar iniciar sesión localmente"  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, intente iniciar sesión localmente con una cuenta que sea miembro del grupo de EA. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **conexión a Escritorio remoto**y, a continuación, haga clic en **conexión a Escritorio remoto**.  

3.  En el **equipo** , escriba el nombre del equipo que desea conectarse a y, a continuación, haga clic en **Connect**. (También puede escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se le solicite, proporcione credenciales para una cuenta que sea miembro del grupo de EA.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![proteger grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
