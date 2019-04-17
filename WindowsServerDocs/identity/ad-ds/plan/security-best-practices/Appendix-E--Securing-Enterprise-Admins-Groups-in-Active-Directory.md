---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: "Apéndice E - proteger los grupos de administradores de empresa en Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 714bc0ab3fe15d09f4ccabb3f35d9b4519e5459c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apéndice E: proteger los grupos de administradores de empresa en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apéndice E: proteger los grupos de administradores de empresa en Active Directory  
El grupo de administradores de empresa (EA), que se almacena en el dominio raíz, no debe contener ningún usuario diariamente, con la posible excepción de la cuenta de administrador del dominio raíz, siempre está protegido según se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Administradores de empresa son, de manera predeterminada, los miembros del grupo Administradores en cada dominio en el bosque. No se debe quitar el grupo EA de los grupos Administradores en cada dominio porque en caso de un escenario de recuperación ante desastres bosque, derechos EA probablemente sea necesarios. Se debe proteger el grupo de administradores de organización del bosque sea lo más detallada en las instrucciones paso a paso que siguen.  

Para el grupo de administradores de empresa en el bosque:  

1.  En los GPO vinculados a las unidades organizativas que contienen los servidores miembro y estaciones de trabajo en cada dominio, el grupo de administradores de empresa debe agregarse a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:  

    -   Denegar el acceso a este equipo desde la red  

    -   Denegar inicio de sesión como trabajo por lotes  

    -   Denegar inicio de sesión como servicio  

    -   Denegar el inicio de sesión localmente  

    -   Denegar inicio de sesión a través de servicios de escritorio remoto  

2.  Configurar la auditoría para enviar alertas si se realizan todas las modificaciones en las propiedades o la pertenencia al grupo de administradores de empresa.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Instrucciones paso a paso para quitar a todos los miembros del grupo de administradores de empresa  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **equipos y usuarios de Active Directory**.  

2.  Si no administra el dominio raíz del bosque, en el árbol de consola, haz clic en <Domain>y, a continuación, haz clic en **cambiar dominio** (donde <Domain> es el nombre del dominio que actualmente está administrando).  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  En la **cambiar dominio** cuadro de diálogo, haz clic en **examinar**, selecciona el dominio raíz del bosque y haz clic en **Aceptar**.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Para quitar a todos los miembros del grupo de EA:  

    1.  Haz doble clic en el **administradores** de grupo y, a continuación, haz clic en el **miembros** pestaña.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Selecciona un miembro del grupo, haz clic en **quitar**, haz clic en **Sí**y haz clic en **Aceptar**.  

5.  Repite el paso 2 hasta que se han quitado todos los miembros del grupo de EA.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Instrucciones paso a paso para proteger los administradores de empresa en Active Directory  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **Group Policy Management**.  

2.  En el árbol de consola, expande <Forest>\Domains\\<Domain>y luego **objetos de directiva de grupo** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

    > [!NOTE]  
    > En un bosque que contiene varios dominios, se debe crear un GPO similar en cada dominio que requiere que el grupo de administradores de empresa están protegidos.  

3.  En el árbol de consola, haz clic en **objetos de directiva de grupo**y haz clic en **nueva**.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  En la **nuevo GPO** cuadro de diálogo, escribe <GPO Name>y haz clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  En el panel de detalles, haz clic en <GPO Name>y haz clic en **editar**.  

6.  Navegar a **equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haz clic en **asignación de derechos de usuario**.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de empresa de acceder a los servidores miembro y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haz doble clic en **denegar el acceso a este equipo desde la red** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

8.  Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de empresa iniciando sesión como trabajo por lotes haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como trabajo por lotes** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haz clic en **ubicaciones** y selecciona el dominio raíz del bosque.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

9. Configurar los derechos de usuario para impedir que los miembros del grupo de EA iniciar sesión como servicio haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como servicio** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y, a continuación, haz clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haz clic en **ubicaciones** y selecciona el dominio raíz del bosque.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

10. Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de empresa de iniciar sesión localmente en los servidores miembro y estaciones de trabajo haciendo lo siguiente:  

    1.  Haz doble clic en **denegar el inicio de sesión localmente** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y, a continuación, haz clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haz clic en **ubicaciones** y selecciona el dominio raíz del bosque.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

11. Configurar los derechos de usuario para impedir que los miembros del grupo Administradores de empresa de obtener acceso a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y, a continuación, haz clic en **examinar**.  

        > [!NOTE]  
        > En un bosque que contiene varios dominios, haz clic en **ubicaciones** y selecciona el dominio raíz del bosque.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

12. Para salir **el Editor de administración de directivas de grupo**, haz clic en **archivo**y haz clic en **salir**.  

13. En **Group Policy Management**, vincular el GPO a los servidores miembros y unidades organizativas de la estación de trabajo mediante las siguientes acciones:  

    1.  Navegar a la <Forest>\Domains\\<Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

    2.  Haz clic en la unidad organizativa que el GPO se aplicarán a y haz clic en **vincular un GPO existente**.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Selecciona el GPO que acabas de crear y haz clic en **Aceptar**.  

        ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Crea vínculos a todas las otras unidades organizativas que contienen las estaciones de trabajo.  

    5.  Crea vínculos a todas las otras unidades organizativas que contienen los servidores miembro.  

    6.  En un bosque que contiene varios dominios, se debe crear un GPO similar en cada dominio que requiere que el grupo de administradores de empresa están protegidos.  

> [!IMPORTANT]  
> Si los servidores de accesos directos se usan para administrar controladores de dominio y Active Directory, asegúrate de que los servidores de accesos directos se encuentran en una unidad organizativa para que este no está vinculado el GPO.  

### <a name="verification-steps"></a>Pasos de verificación  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (como un "salto servidor"), intenta obtener acceso a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración de GPO, intenta asignar la unidad del sistema mediante el **NET USE** comando, efectuando los pasos siguientes:  

1.  Inicia sesión localmente con una cuenta que es un miembro del grupo de EA.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **símbolo**, haz clic en **símbolo**y, a continuación, haz clic en **ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.  

4.  Cuando se te solicite para aprobar la elevación, haz clic en **Sí**.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  En la **símbolo** ventana, escribe **net use \\\<Server Name>\c$**, donde <Server Name> es el nombre del servidor miembro o estación de trabajo que estás intentando acceder a través de la red.  

6.  La siguiente captura de pantalla muestra el mensaje de error que debe aparecer.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  

Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

##### <a name="create-a-batch-file"></a>Crear un archivo por lotes  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **el Bloc de notas**y haz clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haz clic en **archivo**y haz clic en **Guardar como**.  

5.  En la **archivo** nombre, escriba ** <Filename>.bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

##### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **programador de tareas**y haz clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en la **búsqueda** , escriba **programar tareas**y haz clic en **programar tareas**.  

3.  Haz clic en **acción**y haz clic en **crear tarea**.  

4.  En la **crear tarea** cuadro de diálogo, escribe ** <Task Name> ** (donde <Task Name> es el nombre de la nueva tarea).  

5.  Haz clic en el **acciones** pestaña y haz clic en **nueva**.  

6.  En la **acción** campo, seleccione **iniciar un programa**.  

7.  En **programa o script**, haz clic en **examinar**, busca y selecciona el archivo por lotes que creaste en el **crear un archivo por lotes** sección y haz clic en **abrir**.  

8.  Haz clic en **Aceptar**.  

9. Haz clic en el **General** pestaña.  

10. En la **opciones de seguridad** , a continuación, haz clic en **Cambiar usuario o grupo**.  

11. Escribe el nombre de una cuenta que es un miembro del grupo EAs, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

12. Selecciona **ejecutar si el usuario ha iniciado sesión o no** y selecciona **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haz clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo, cuenta de usuario solicita credenciales para ejecutar la tarea.  

15. Después de escribir las credenciales, haz clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En **iniciar sesión como**, selecciona **esta cuenta**.  

7.  Haz clic en **examinar**, escribe el nombre de una cuenta que es un miembro del grupo EAs, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

8.  En **contraseña:** y **Confirmar contraseña**, escribe la contraseña de la cuenta seleccionada y haz clic en **Aceptar**.  

9. Haz clic en **Aceptar** tres veces más.  

10. Haz clic en el **cola de impresión** de servicio y selecciona **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio de la cola de impresora  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En **iniciar sesión como**, selecciona el **sistema Local** cuenta y haz clic en **Aceptar**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión local"  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, intenta iniciar sesión localmente con una cuenta que sea miembro del grupo de EA. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **conexión a Escritorio remoto**y, a continuación, haz clic en **conexión a Escritorio remoto**.  

3.  En la **equipo** , a continuación, escribe el nombre del equipo que desea conectarse y, a continuación, haz clic en **conectar**. (También puedes escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se te solicite, proporcionar credenciales para una cuenta que es un miembro del grupo de EA.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![seguro grupos de administradores de empresa](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
