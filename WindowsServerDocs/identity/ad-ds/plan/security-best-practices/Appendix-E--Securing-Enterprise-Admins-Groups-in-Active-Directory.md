---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: 'Apéndice E: protección de grupos de administradores de empresa en Active Directory'
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4eca54063af57c9e261abf8cb8b9787ebda6cb64
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071377"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Anexo E: protección de grupos de administradores de empresa en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Anexo E: protección de grupos de administradores de empresa en Active Directory
El grupo administradores de empresas (EA), que está hospedado en el dominio raíz del bosque, no debe contener ningún usuario en el día, con la posible excepción de la cuenta de administrador del dominio raíz, siempre que esté protegido como se describe en el [Apéndice D: protección de cuentas de administrador de Built-In en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).

De forma predeterminada, los administradores de empresas son miembros del grupo administradores de cada dominio del bosque. No debe quitar el grupo EA de los grupos administradores de cada dominio porque, en caso de que se produzca un escenario de recuperación ante desastres de bosque, es probable que se necesiten derechos de EA. El grupo administradores de organización del bosque debe protegerse tal y como se detalla en las instrucciones paso a paso que se indican a continuación.

Para el grupo administradores de empresas del bosque:

1.  En los GPO vinculados a las unidades organizativas que contienen servidores miembro y estaciones de trabajo en cada dominio, el grupo administradores de organización debe agregarse a los siguientes derechos de usuario en el **equipo \ configuración** de seguridad\Directivas de Seguridad\directivas locales \ asignaciones de derechos:

    -   Denegar el acceso desde la red a este equipo

    -   Denegar el inicio de sesión como trabajo por lotes

    -   Denegar el inicio de sesión como servicio

    -   Denegar el inicio de sesión localmente

    -   Denegar inicio de sesión a través de Servicios de Escritorio remoto

2.  Configure la auditoría para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia al grupo administradores de empresas.

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Instrucciones paso a paso para quitar todos los miembros del grupo administradores de empresas

1.  En **Administrador del servidor** , haga clic en **herramientas** y en **Active Directory usuarios y equipos** .

2.  Si no está administrando el dominio raíz del bosque, en el árbol de consola, haga clic con el botón secundario <Domain> y, a continuación, haga clic en **cambiar dominio** (donde <Domain> es el nombre del dominio que está administrando actualmente).

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)

3.  En el cuadro de diálogo **cambiar dominio** , haga clic en **examinar** , seleccione el dominio raíz del bosque y haga clic en **Aceptar** .

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)

4.  Para quitar todos los miembros del grupo EA:

    1.  Haga doble clic en el grupo **administradores de empresas** y, a continuación, haga clic en la pestaña **miembros** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)

    2.  Seleccione un miembro del grupo, haga clic en **quitar** , haga clic en **sí** y, a continuación, haga clic en **Aceptar** .

5.  Repita el paso 2 hasta que se hayan quitado todos los miembros del grupo EA.

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Instrucciones paso a paso para proteger los administradores de empresa en Active Directory

1.  En **Administrador del servidor** , haga clic en **herramientas** y en **Administración de directiva de grupo** .

2.  En el árbol de consola, expanda <Forest> \Domains y \\ <Domain> , a continuación, **Directiva de grupo objetos** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea establecer el Directiva de grupo).

    > [!NOTE]
    > En un bosque que contenga varios dominios, debe crearse un GPO similar en cada dominio que requiera la protección del grupo administradores de empresas.

3.  En el árbol de consola, haga clic con el botón secundario en **Directiva de grupo objetos** y haga clic en **nuevo** .

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)

4.  En el cuadro de diálogo **nuevo GPO** , escriba <GPO Name> y haga clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)

5.  En el panel de detalles, haga clic con el botón secundario <GPO Name> y haga clic en **Editar** .

6.  Vaya a **equipo \ configuración de Seguridad\directivas \ directivas de seguridad\Directivas** y haga clic en **asignación de derechos de usuario** .

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)

7.  Configure los derechos de usuario para evitar que los miembros del grupo administradores de empresa tengan acceso a los servidores y estaciones de trabajo miembros a través de la red haciendo lo siguiente:

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva** .

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar** .

    3.  Escriba **administradores de empresas** , haga clic en **Comprobar nombres** y haga clic en **Aceptar** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

8.  Configure los derechos de usuario para evitar que los miembros del grupo administradores de empresas inicien sesión como un trabajo por lotes haciendo lo siguiente:

    1.  Haga doble clic en **denegar el inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva** .

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar** .

        > [!NOTE]
        > En un bosque que contenga varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.

    3.  Escriba **administradores de empresas** , haga clic en **Comprobar nombres** y haga clic en **Aceptar** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

9. Configure los derechos de usuario para evitar que los miembros del grupo EA inicien sesión como servicio de la siguiente manera:

    1.  Haga doble clic en **denegar registro como servicio** y seleccione **definir esta configuración de directiva** .

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, haga clic en **examinar** .

        > [!NOTE]
        > En un bosque que contenga varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.

    3.  Escriba **administradores de empresas** , haga clic en **Comprobar nombres** y haga clic en **Aceptar** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

10. Configure los derechos de usuario para evitar que los miembros del grupo administradores de empresas inicien sesión localmente en los servidores miembro y en las estaciones de trabajo haciendo lo siguiente:

    1.  Haga doble clic en **denegar el inicio de sesión localmente** y seleccione **definir esta configuración de directiva** .

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, haga clic en **examinar** .

        > [!NOTE]
        > En un bosque que contenga varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.

    3.  Escriba **administradores de empresas** , haga clic en **Comprobar nombres** y haga clic en **Aceptar** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

11. Configure los derechos de usuario para evitar que los miembros del grupo administradores de empresa tengan acceso a los servidores y estaciones de trabajo miembro a través de Servicios de Escritorio remoto de la siguiente manera:

    1.  Haga doble clic en **denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva** .

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, haga clic en **examinar** .

        > [!NOTE]
        > En un bosque que contenga varios dominios, haga clic en **ubicaciones** y seleccione el dominio raíz del bosque.

    3.  Escriba **administradores de empresas** , haga clic en **Comprobar nombres** y haga clic en **Aceptar** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

12. Para salir **Editor de administración de directivas de grupo** , haga clic en **archivo** y, a continuación, en **salir** .

13. En **Administración de directiva de grupo** , VINCULE el GPO al servidor miembro y las unidades organizativas de la estación de trabajo haciendo lo siguiente:

    1.  Navegue hasta <Forest> \Domains \\ <Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea establecer el Directiva de grupo).

    2.  Haga clic con el botón secundario en la unidad organizativa a la que se aplicará el GPO y haga clic en **vincular un GPO existente** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar** .

        ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)

    4.  Cree vínculos a todas las demás unidades organizativas que contengan estaciones de trabajo.

    5.  Cree vínculos a todas las demás unidades organizativas que contengan servidores miembro.

    6.  En un bosque que contenga varios dominios, debe crearse un GPO similar en cada dominio que requiera la protección del grupo administradores de empresas.

> [!IMPORTANT]
> Si se usan servidores de salto para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no están vinculados los GPO.

### <a name="verification-steps"></a>Pasos de comprobación

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "denegar el acceso a este equipo desde la red"
Desde cualquier servidor miembro o estación de trabajo que no se vea afectado por los cambios de GPO (como un "servidor de salto"), intente tener acceso a un servidor miembro o una estación de trabajo a través de la red que afecte a los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el comando **net use** mediante los pasos siguientes:

1.  Inicie sesión localmente con una cuenta que sea miembro del grupo EA.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar** .

3.  En el cuadro de **búsqueda** , escriba **símbolo del sistema** , haga clic con el botón secundario en símbolo del **sistema** y, a continuación, haga clic en **Ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.

4.  Cuando se le pida que apruebe la elevación, haga clic en **sí** .

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)

5.  En la ventana del **símbolo del sistema** , escriba **net use \\ \\ \<Server Name\> \c $** , donde \<Server Name\> es el nombre del servidor miembro o de la estación de trabajo a la que está intentando obtener acceso a través de la red.

6.  En la captura de pantalla siguiente se muestra el mensaje de error que debe aparecer.

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como trabajo por lotes"

Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

##### <a name="create-a-batch-file"></a>Crear un archivo por lotes

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar** .

2.  En el cuadro de **búsqueda** , escriba **Notepad** y haga clic en **Bloc de notas** .

3.  En **el Bloc de notas** , escriba **dir c:** .

4.  Haga clic en **archivo** y en **Guardar como** .

5.  En el cuadro Nombre de **archivo** , escriba **<Filename> . bat** (donde <Filename> es el nombre del nuevo archivo por lotes).

##### <a name="schedule-a-task"></a>Programar una tarea

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar** .

2.  En el cuadro de **búsqueda** , escriba **programador de tareas** y haga clic en **programador de tareas** .

    > [!NOTE]
    > En equipos que ejecutan Windows 8, en el cuadro de **búsqueda** , escriba **programar tareas** y haga clic en **programar tareas** .

3.  Haga clic en **acción** y en **crear tarea** .

4.  En el cuadro de diálogo **crear tarea** , escriba **<Task Name>** (donde <Task Name> es el nombre de la nueva tarea).

5.  Haga clic en la pestaña **acciones** y en **nuevo** .

6.  En el campo **acción** , seleccione **iniciar un programa** .

7.  En **programa/script** , haga clic en **examinar** , busque y seleccione el archivo por lotes creado en la sección **crear un archivo por lotes** y haga clic en **abrir** .

8.  Haga clic en **Aceptar** .

9. Haga clic en la pestaña **General** .

10. En el campo **Opciones de seguridad** , haga clic en **cambiar usuario o grupo** .

11. Escriba el nombre de una cuenta que sea miembro del grupo EAs, haga clic en **Comprobar nombres** y haga clic en **Aceptar** .

12. Seleccione **ejecutar si el usuario ha iniciado sesión o no** y seleccione no **almacenar contraseña** . La tarea solo tendrá acceso a los recursos del equipo local.

13. Haga clic en **Aceptar** .

14. Debe aparecer un cuadro de diálogo que solicite las credenciales de la cuenta de usuario para ejecutar la tarea.

15. Después de escribir las credenciales, haga clic en **Aceptar** .

16. Debería aparecer un cuadro de diálogo similar al siguiente.

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como servicio"

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar** .

3.  En el cuadro de **búsqueda** , escriba **servicios** y haga clic en **servicios** .

4.  Busque y haga doble clic en **Administrador de trabajos de impresión** .

5.  Haga clic en la ficha **Iniciar sesión** .

6.  En **iniciar sesión como** , seleccione **esta cuenta** .

7.  Haga clic en **examinar** , escriba el nombre de una cuenta que sea miembro del grupo EAS, haga clic en **Comprobar nombres** y haga clic en **Aceptar** .

8.  En **contraseña:** y **Confirmar contraseña** , escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar** .

9. Haga clic en **Aceptar** tres veces más.

10. Haga clic con el botón secundario en el servicio **Administrador de trabajos de impresión** y seleccione **reiniciar** .

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio cola de impresión

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar** .

3.  En el cuadro de **búsqueda** , escriba **servicios** y haga clic en **servicios** .

4.  Busque y haga doble clic en **Administrador de trabajos de impresión** .

5.  Haga clic en la ficha **Iniciar sesión** .

6.  En **iniciar sesión como** , seleccione la cuenta de **sistema local** y haga clic en **Aceptar** .

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Comprobar la configuración de GPO "denegar el inicio de sesión local"

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, intente iniciar sesión localmente con una cuenta que sea miembro del grupo EA. Debería aparecer un cuadro de diálogo similar al siguiente.

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión a través de Servicios de Escritorio remoto"

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar** .

2.  En el cuadro de **búsqueda** , escriba **conexión a escritorio remoto** y, a continuación, haga clic en **conexión a escritorio remoto** .

3.  En el campo **equipo** , escriba el nombre del equipo al que desea conectarse y, a continuación, haga clic en **conectar** . (También puede escribir la dirección IP en lugar del nombre del equipo).

4.  Cuando se le solicite, proporcione las credenciales de una cuenta que sea miembro del grupo EA.

5.  Debería aparecer un cuadro de diálogo similar al siguiente.

    ![proteger los grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)
