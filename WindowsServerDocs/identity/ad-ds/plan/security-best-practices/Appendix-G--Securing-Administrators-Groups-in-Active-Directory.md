---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: "Apéndice G - proteger el grupo de administradores en Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cf89f7bf31ce848de384778b0213d0ddc822158
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apéndice G: proteger el grupo de administradores en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apéndice G: proteger el grupo de administradores en Active Directory  
Como es el caso de los grupos Administradores de empresa (EA) y administradores de dominio (DA), deben pertenencia al grupo integrado Administradores (BA) solo en escenarios de recuperación ante desastres o de compilación. No debería haber ninguna cuenta de usuario diarias en el grupo de administradores a excepción de la cuenta predefinida de administrador para el dominio, si ha protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Los administradores son, de manera predeterminada, los propietarios de la mayoría de los objetos de AD DS en sus respectivos dominios. Pertenencia a este grupo puede ser necesario en escenarios de recuperación ante desastres o de compilación en el que se requiere la propiedad o la capacidad de tomar posesión de objetos. Además, DAs y EAs heredan un número de sus derechos y permisos en virtud de su pertenencia predeterminado en el grupo de administradores. No se debe modificar grupo predeterminado anidamiento de grupos con privilegios en Active Directory y grupo de administradores de cada dominio se debe proteger como se describe en las instrucciones paso a paso que siguen.  

Para el grupo de administradores en cada dominio del bosque:  

1.  Quitar todos los miembros del grupo de administradores, con la posible excepción de la cuenta predefinida de administrador para el dominio, siempre ha sido protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  En los GPO vinculados a las unidades organizativas que contienen los servidores miembro y estaciones de trabajo en cada dominio, debe agregarse el grupo DA a los siguientes derechos de usuario en **asignación de derechos de usuario de equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas Policies\**:  

    -   Denegar el acceso a este equipo desde la red  

    -   Denegar inicio de sesión como trabajo por lotes  

    -   Denegar inicio de sesión como servicio  

3.  En la unidad organizativa en cada dominio en el bosque de controladores de dominio, el grupo de administradores debe conceder los siguientes derechos de usuario:  

    -   Acceso a este equipo desde la red  

    -   Permitir el registro local  

    -   Permitir el inicio de sesión en a través de servicios de escritorio remoto  

4.  Auditoría debe configurarse para enviar alertas si se realizan todas las modificaciones en las propiedades o la pertenencia al grupo de administradores.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Instrucciones paso a paso para quitar a todos los miembros del grupo Administradores  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **equipos y usuarios de Active Directory**.  

2.  Para quitar a todos los miembros del grupo Administradores, realiza los siguientes pasos:  

    1.  Haz doble clic en el **administradores** de grupo y haga clic en el **miembros** pestaña.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Selecciona un miembro del grupo, haz clic en **quitar**, haz clic en **Sí**y haz clic en **Aceptar**.  

3.  Repite el paso 2 hasta que se han quitado todos los miembros del grupo de administradores.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Instrucciones paso a paso para proteger el grupo de administradores en Active Directory  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **Group Policy Management**.  

2.  En el árbol de consola, expande <Forest>\Domains\\<Domain>y luego **objetos de directiva de grupo** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

3.  En el árbol de consola, haz clic en **objetos de directiva de grupo**y haz clic en **nueva**.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  En la **nuevo GPO** cuadro de diálogo, escribe <GPO Name>y haz clic en **Aceptar** (donde *nombre del GPO* es el nombre de este GPO).  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  En el panel de detalles, haz clic en ** <GPO Name> **y haz clic en **editar**.  

6.  Navegar a **equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haz clic en **asignación de derechos de usuario**.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configurar los derechos de usuario para evitar que a los miembros del grupo Administradores de acceder a los servidores miembro y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haz doble clic en **denegar el acceso a este equipo desde la red** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

8.  Configurar los derechos de usuario para evitar que a los miembros del grupo Administradores de iniciar sesión como trabajo por lotes haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como trabajo por lotes** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

9. Configurar los derechos de usuario para evitar que a los miembros del grupo Administradores de iniciar sesión como servicio haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como servicio** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

10. Para salir **el Editor de administración de directivas de grupo**, haz clic en **archivo**y haz clic en **salir**.  

11. En **Group Policy Management**, vincular el GPO a los servidores miembros y unidades organizativas de la estación de trabajo mediante las siguientes acciones:  

    1.  Navegar a la <Forest>\Domains\\<Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

    2.  Haz clic en la unidad organizativa que el GPO se aplicarán a y haz clic en **vincular un GPO existente**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Selecciona el GPO que acabas de crear y haz clic en **Aceptar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Crea vínculos a todas las otras unidades organizativas que contienen las estaciones de trabajo.  

    5.  Crea vínculos a todas las otras unidades organizativas que contienen los servidores miembro.  

        > [!IMPORTANT]  
        > Si los servidores de accesos directos se usan para administrar controladores de dominio y Active Directory, asegúrate de que los servidores de accesos directos se encuentran en una unidad organizativa para que este no está vinculado el GPO.  

        > [!NOTE]  
        > Cuando se implementan restricciones en el grupo de administradores en los GPO, Windows aplica la configuración a los miembros del grupo de administradores local de un equipo además de grupo de administradores del dominio. Por lo tanto, debes tener cuidado al implementar restricciones en el grupo de administradores. Aunque prohibición de red, por lotes y los inicios de sesión de servicio para los miembros del grupo Administradores es recomendable donde es factible implementar, no restringir los inicios de sesión local o los inicios de sesión a través de servicios de escritorio remoto. Bloqueo de estos tipos de inicio de sesión, puede impedir legítima administración de un equipo por miembros del grupo de administradores local.  
        >   
        > La siguiente captura de pantalla muestra las opciones de configuración que bloquear el uso incorrecto de integrados locales y cuentas Administrador de dominio, además de uso inadecuado de los grupos de administradores de dominio o local integrados. Ten en cuenta que la **Denegar inicio de sesión a través de servicios de escritorio remoto** derecho de usuario no incluye el grupo de administradores, porque se incluye en esta configuración también bloquearía estos inicios de sesión para las cuentas que son miembros del grupo Administradores del equipo local. Si los servicios en los equipos se configuran para ejecutarse en el contexto de cualquiera de los grupos con privilegios, que se describe en esta sección, implementar estas opciones de configuración puede ocasionar servicios y aplicaciones a un error. Por lo tanto, al igual que con todas las recomendaciones indicadas en esta sección, debe probar la configuración de aplicación en su entorno.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Instrucciones paso a paso a los derechos de usuario de concesión al grupo de administradores  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **Group Policy Management**.  

2.  En el árbol de consola, expande <Forest>\Domains\\<Domain>y luego **objetos de directiva de grupo** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

3.  En el árbol de consola, haz clic en **objetos de directiva de grupo**y haz clic en **nueva**.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  En la **nuevo GPO** cuadro de diálogo, escribe <GPO Name>y haz clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  En el panel de detalles, haz clic en ** <GPO Name> **y haz clic en **editar**.  

6.  Navegar a **equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haz clic en **asignación de derechos de usuario**.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configurar los derechos de usuario para permitir que los miembros del grupo Administradores en controladores de dominio de acceso a través de la red haciendo lo siguiente:  

    1.  Haz doble clic en **acceso a este equipo desde la red** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

8.  Configurar los derechos de usuario para permitir que los miembros del grupo Administradores iniciar sesión localmente haciendo lo siguiente:  

    1.  Haz doble clic en **permitir el registro local** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores**, haz clic en comprobar **nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

9. Configurar los derechos de usuario para permitir que los miembros del grupo Administradores iniciar sesión a través de servicios de escritorio remoto haciendo lo siguiente:  

    1.  Haz doble clic en **Permitir inicio de sesión en a través de servicios de escritorio remoto** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

10. Para salir **el Editor de administración de directivas de grupo**, haz clic en **archivo**y haz clic en **salir**.  

11. En **Group Policy Management**, vincular el GPO a la unidad organizativa de controladores de dominio mediante las siguientes acciones:  

    1.  Navegar a la <Forest>\Domains\\<Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee para establecer la directiva de grupo).  

    2.  Haz clic en los controladores de dominio, unidad organizativa y haz clic en **vincular un GPO existente**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Selecciona el GPO que acabas de crear y haz clic en **Aceptar**.  

        ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Pasos de verificación  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (como un "salto servidor"), intenta obtener acceso a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración de GPO, intenta asignar la unidad del sistema mediante el **NET USE** comando.  

1.  Inicia sesión localmente con una cuenta que es un miembro del grupo de administradores.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **símbolo**, haz clic en **símbolo**y, a continuación, haz clic en **ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.  

4.  Cuando se te solicite para aprobar la elevación, haz clic en **Sí**.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  En la **símbolo** ventana, escribe **net use \\\<Server Name>\c$**, donde <Server Name> es el nombre del servidor miembro o estación de trabajo que estás intentando acceder a través de la red.  

6.  La siguiente captura de pantalla muestra el mensaje de error que debe aparecer.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

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

2.  En la **búsqueda** , escriba **programador de tareas**y haz clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en el cuadro de búsqueda, escriba programar tareas y haz clic en programar tareas.  

3.  Haz clic en **acción**y haz clic en **crear tarea**.  

4.  En la **crear tarea** cuadro de diálogo, escribe ** <Task Name> ** (donde <Task Name> es el nombre de la nueva tarea).  

5.  Haz clic en el **acciones** pestaña y haz clic en **nueva**.  

6.  En la **acción** campo, seleccione **iniciar un programa**.  

7.  En la **programa o script** , a continuación, haz clic en **examinar**, busca y selecciona el archivo por lotes que creaste en el **crear un archivo por lotes** sección y haz clic en **abrir**.  

8.  Haz clic en **Aceptar**.  

9. Haz clic en el **General** pestaña.  

10. En la **opciones de seguridad** , a continuación, haz clic en **Cambiar usuario o grupo**.  

11. Escribe el nombre de una cuenta que es un miembro del grupo de administradores, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

12. Selecciona **ejecutar si el usuario no está registrada onor** y **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haz clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo, cuenta de usuario solicita credenciales para ejecutar la tarea.  

15. Después de escribir la contraseña, haz clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En la **iniciar sesión como** campo, seleccione **esta cuenta**.  

7.  Haz clic en **examinar**, escribe el nombre de una cuenta que es un miembro del grupo de administradores, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

8.  En la **contraseña** y **Confirmar contraseña** campos, escribe la contraseña de la cuenta seleccionada y haz clic en **Aceptar**.  

9. Haz clic en **Aceptar** tres veces más.  

10. Haz clic en **cola de impresión** y haz clic en **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores seguro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio de la cola de impresora  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En la **iniciar sesión como** , a continuación, haz clic en **sistema Local** cuenta y haz clic en **Aceptar**.  
