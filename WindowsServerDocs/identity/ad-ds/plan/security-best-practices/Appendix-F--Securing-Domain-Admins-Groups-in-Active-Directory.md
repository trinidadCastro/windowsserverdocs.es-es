---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: "Apéndice F - protección de grupos de administradores de dominio de Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3535714e0df43cb94c7e89c503bc5f875cd49e28
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apéndice F: proteger grupos de administradores de dominio de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apéndice F: proteger grupos de administradores de dominio de Active Directory  
Como sucede con el grupo de administradores de empresa (EA), deben pertenecer al grupo de administradores de dominio (DA) solo en escenarios de recuperación ante desastres o de compilación. No debería haber ninguna cuenta de usuario diarias en el grupo DA a excepción de la cuenta predefinida de administrador para el dominio, si ha protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Administradores de dominio son, de manera predeterminada, los miembros del grupo de administradores locales en todos los servidores miembro y estaciones de trabajo en sus respectivos dominios. No se debe modificar este anidamiento predeterminado para obtener compatibilidad y recuperación ante desastres. Si se han quitado los administradores de dominio de los grupos de administradores locales en los servidores miembro, el grupo debe agregarse al grupo Administradores en cada servidor miembro y la estación de trabajo en el dominio. Grupo de administradores de dominio de cada dominio se debe proteger como se describe en las instrucciones paso a paso que siguen.  

Para el grupo de administradores de dominio en cada dominio del bosque:  

1.  Quitar todos los miembros del grupo, con la posible excepción de la cuenta predefinida de administrador para el dominio, siempre ha sido protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  En los GPO vinculados a las unidades organizativas que contienen los servidores miembro y estaciones de trabajo en cada dominio, debe agregarse el grupo DA a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:  

    -   Denegar el acceso a este equipo desde la red  

    -   Denegar inicio de sesión como trabajo por lotes  

    -   Denegar inicio de sesión como servicio  

    -   Denegar el inicio de sesión localmente  

    -   Denegar inicio de sesión a través de los derechos de usuario de servicios de escritorio remoto  

3.  Auditoría debe configurarse para enviar alertas si se realizan todas las modificaciones en las propiedades o la pertenencia al grupo de administradores de dominio.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Instrucciones paso a paso para quitar a todos los miembros del grupo de administradores de dominio  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **equipos y usuarios de Active Directory**.  

2.  Para quitar a todos los miembros del grupo DA, realiza los siguientes pasos:  

    1.  Haz doble clic en el **administradores de dominio** de grupo y haga clic en el **miembros** pestaña.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Selecciona un miembro del grupo, haz clic en **quitar**, haz clic en **Sí**y haz clic en **Aceptar**.  

3.  Repite el paso 2 hasta que se han quitado todos los miembros del grupo DA.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instrucciones paso a paso para proteger los administradores de dominio de Active Directory  

1.  En **administrador del servidor**, haz clic en **herramientas**y haz clic en **Group Policy Management**.  

2.  En el árbol de consola, expande \ < Forest\ > \\Domains\\\ < Domain\ > y luego **objetos de directiva de grupo** (donde \ < Forest\ > es el nombre del bosque y \ < Domain\ > es el nombre del dominio donde desee para establecer la directiva de grupo).  

3.  En el árbol de consola, haz clic en **objetos de directiva de grupo**y haz clic en **nueva**.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  En la **nuevo GPO** cuadro de diálogo, escribe \ < GPO equipo\ > y haz clic en **Aceptar** (donde \ < GPO equipo\ > es el nombre de este GPO).  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  En el panel de detalles, haz clic en \ < GPO equipo\ > y haz clic en **editar**.  

6.  Navegar a **equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haz clic en **asignación de derechos de usuario**.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configurar los derechos de usuario para impedir que los miembros del grupo Domain Admins acceder a los servidores miembros y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haz doble clic en **denegar el acceso a este equipo desde la red** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores de dominio**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

8.  Configurar los derechos de usuario para impedir que los miembros del grupo DA iniciar sesión como trabajo por lotes haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como trabajo por lotes** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores de dominio**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

9. Configurar los derechos de usuario para impedir que los miembros del grupo DA iniciar sesión como servicio haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión como servicio** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores de dominio**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

10. Configurar los derechos de usuario para impedir que los miembros del grupo Domain Admins iniciar sesión localmente en los servidores miembro y estaciones de trabajo haciendo lo siguiente:  

    1.  Haz doble clic en **denegar el inicio de sesión localmente** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores de dominio**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

11. Configurar los derechos de usuario para impedir que los miembros del grupo Domain Admins obtener acceso a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto haciendo lo siguiente:  

    1.  Haz doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y selecciona **definir esta configuración de directiva**.  

    2.  Haz clic en **Agregar usuario o grupo** y haz clic en **examinar**.  

    3.  Tipo **administradores de dominio**, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Haz clic en **Aceptar**, y **Aceptar** nuevamente.  

12. Para salir **el Editor de administración de directivas de grupo**, haz clic en **archivo**y haz clic en **salir**.  

13. En la administración de directivas de grupo, vincular el GPO a la estación de trabajo unidades organizativas y del servidor miembro haciendo lo siguiente:  

    1.  Navegar a la \ < Forest\ > \Domains\\\ < Domain\ > (donde \ < Forest\ > es el nombre del bosque y \ < Domain\ > es el nombre del dominio donde desee para establecer la directiva de grupo).  

    2.  Haz clic en la unidad organizativa que el GPO se aplicarán a y haz clic en **vincular un GPO existente**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Selecciona el GPO que acabas de crear y haz clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Crea vínculos a todas las otras unidades organizativas que contienen las estaciones de trabajo.  

    5.  Crea vínculos a todas las otras unidades organizativas que contienen los servidores miembro.  

        > [!IMPORTANT]  
        > Si los servidores de accesos directos se usan para administrar controladores de dominio y Active Directory, asegúrate de que los servidores de accesos directos se encuentran en una unidad organizativa para que este no está vinculado el GPO.  

#### <a name="verification-steps"></a>Pasos de verificación  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (como un "salto servidor"), intenta obtener acceso a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración de GPO, intenta asignar la unidad del sistema mediante el **NET USE** comando.  

1.  Inicia sesión localmente con una cuenta que sea miembro del grupo Domain Admins.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **símbolo**, haz clic en **símbolo**y, a continuación, haz clic en **ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.  

4.  Cuando se te solicite para aprobar la elevación, haz clic en **Sí**.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  En la **símbolo** ventana, escribe **net use \\\ < servidor equipo\ > \c$**, donde \ < servidor equipo\ > es el nombre del servidor miembro o estación de trabajo que estás intentando acceder a través de la red.  

6.  La siguiente captura de pantalla muestra el mensaje de error que debe aparecer.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  

Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

###### <a name="create-a-batch-file"></a>Crear un archivo por lotes  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **el Bloc de notas**y haz clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haz clic en **archivo**y haz clic en **Guardar como**.  

5.  En la **archivo** campo de nombre, escribe **\ < Filename\ > .bat** (donde \ < Filename\ > es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **programador de tareas**y haz clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en la **búsqueda** , escriba **programar tareas**y haz clic en **programar tareas**.  

3.  En la **programador de tareas** barra de menús, haz clic en **acción**y haz clic en **crear tarea**.  

4.  En la **crear tarea** cuadro de diálogo, escribe **\ < tarea equipo\ >** (donde \ < tarea equipo\ > es el nombre de la nueva tarea).  

5.  Haz clic en el **acciones** pestaña y haz clic en **nueva**.  

6.  En la **acción** campo, seleccione **iniciar un programa**.  

7.  En **programa o script**, haz clic en **examinar**, busca y selecciona el archivo por lotes que creaste en el **crear un archivo por lotes** sección y haz clic en **abrir**.  

8.  Haz clic en **Aceptar**.  

9. Haz clic en el **General** pestaña.  

10. En **seguridad** opciones, haga clic en **Cambiar usuario o grupo**.  

11. Escribe el nombre de una cuenta que es un miembro del grupo Administradores de dominio, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

12. Selecciona **ejecutar si el usuario ha iniciado sesión o no** y selecciona **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haz clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo, cuenta de usuario solicita credenciales para ejecutar la tarea.  

15. Después de escribir las credenciales, haz clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En **iniciar sesión como**, selecciona el **esta cuenta** opción.  

7.  Haz clic en **examinar**, escribe el nombre de una cuenta que es un miembro del grupo Administradores de dominio, haz clic en **comprobar nombres**y haz clic en **Aceptar**.  

8.  En **contraseña** y **Confirmar contraseña**, escribe la contraseña de la cuenta seleccionada y haz clic en **Aceptar**.  

9. Haz clic en **Aceptar** tres veces más.  

10. Haz clic en **cola de impresión** y haz clic en **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio de la cola de impresora  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, iniciar sesión localmente.  

2.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

3.  En la **búsqueda** , escriba **servicios**y haz clic en **servicios**.  

4.  Busque y haga doble clic en **cola de impresión**.  

5.  Haz clic en el **iniciar sesión** pestaña.  

6.  En **iniciar sesión como**, selecciona el **sistema Local** cuenta y haz clic en **Aceptar**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión local"  

1.  Desde cualquier servidor miembro o estación de trabajo que se ve afectado por los cambios de GPO, intenta iniciar sesión localmente con una cuenta que sea miembro del grupo Administradores de dominio. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"    
1.  Con el mouse, mueve el puntero hacia la esquina superior derecha o inferior derecha de la pantalla. Cuando la **accesos** barra aparece, haz clic en **búsqueda**.  

2.  En la **búsqueda** , escriba **conexión a Escritorio remoto**y haz clic en **conexión a Escritorio remoto**.  

3.  En la **equipo** , a continuación, escribe el nombre del equipo que desea conectarse y haz clic en **conectar**. (También puedes escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se te solicite, proporcionar credenciales para una cuenta que sea miembro del grupo Domain Admins.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
