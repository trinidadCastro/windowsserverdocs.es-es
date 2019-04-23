---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: Apéndice G - protección de grupos de administradores en Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2912dfc534d751d4aa121d238dffc36c07562d76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882716"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apéndice G: Protección de grupos de administradores en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apéndice G: Protección de grupos de administradores en Active Directory  
Como sucede con los grupos Administradores de Enterprise (EA) y los administradores de dominio (DA), pertenencia al grupo integrado Administradores (BA) deben solo en escenarios de recuperación ante desastres o de compilación. No debe haber ninguna cuenta de usuario diarias en el grupo de administradores con la excepción de la cuenta Administrador integrado para el dominio, si se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Los administradores son, de forma predeterminada, los propietarios de la mayoría de los objetos de AD DS en sus dominios respectivos. Pertenencia a este grupo puede ser necesario en escenarios de recuperación ante desastres o de compilación en el que se requiere la propiedad o la capacidad de tomar posesión de objetos. Además, DAs y EAs heredan un número de sus derechos y permisos en virtud de su pertenencia predeterminado en el grupo Administradores. No se debe modificar el grupo predeterminado de anidamiento de grupos con privilegios en Active Directory, y debe protegerse el grupo de administradores de cada dominio como se describe en las instrucciones paso a paso siguientes.  

Para el grupo de administradores en cada dominio del bosque:  

1.  Quitar todos los miembros del grupo de administradores, con la excepción posible de la cuenta de administrador integrada para el dominio, siempre que se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  En los GPO vinculados a unidades organizativas que contiene los servidores miembro y estaciones de trabajo en cada dominio, se debe agregar el grupo DA a los siguientes derechos de usuario en **derechos de usuario de equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas Policies\ Asignación**:  

    -   Denegar el acceso desde la red a este equipo  

    -   Denegar el inicio de sesión como trabajo por lotes  

    -   Denegar el inicio de sesión como servicio  

3.  En la unidad organizativa en cada dominio del bosque controladores de dominio, el grupo de administradores debe conceder los derechos de usuario siguientes:  

    -   Obtener acceso a este equipo desde la red  

    -   Permitir el inicio de sesión local  

    -   Permitir inicio de sesión a través de Servicios de Escritorio remoto  

4.  La auditoría debe configurarse para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia del grupo Administradores.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Instrucciones paso a paso para quitar a todos los miembros del grupo de administradores  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **equipos y usuarios de Active Directory**.  

2.  Para quitar a todos los miembros del grupo de administradores, realice los pasos siguientes:  

    1.  Haga doble clic en el **administradores** de grupo y haga clic en el **miembros** ficha.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Seleccione un miembro del grupo, haga clic en **quitar**, haga clic en **Sí**y haga clic en **Aceptar**.  

3.  Repita el paso 2 hasta que se han quitado todos los miembros del grupo Administradores.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Instrucciones paso a paso para proteger grupos de administradores en Active Directory  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **Group Policy Management**.  

2.  En el árbol de consola, expanda &lt;bosque&gt;\Domains\\&lt;dominio&gt;y, a continuación, **Group Policy Objects** (donde &lt;bosque&gt; es el nombre del bosque y &lt;dominio&gt; es el nombre del dominio donde desea establecer la directiva de grupo).  

3.  En el árbol de consola, haga clic en **Group Policy Objects**y haga clic en **New**.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  En el **nuevo GPO** cuadro de diálogo, escriba <GPO Name>y haga clic en **Aceptar** (donde *nombre del GPO* es el nombre de este GPO).  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  En el panel de detalles, haga clic en **<GPO Name>** y haga clic en **editar**.  

6.  Vaya a **configuración del equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haga clic en **asignación de derechos de usuario**.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configurar los derechos de usuario para impedir que los miembros del grupo Administradores tengan acceso a los servidores miembro y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administradores**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

8.  Configurar los derechos de usuario para impedir que los miembros del grupo administradores inician sesión como trabajo por lotes mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administradores**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

9. Configurar los derechos de usuario para impedir que los miembros del grupo administradores inicien sesión como un servicio mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administradores**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

10. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y haga clic en **salir**.  

11. En **Group Policy Management**, vincule el GPO a unidades organizativas de la estación de trabajo y del servidor miembro haciendo lo siguiente:  

    1.  Navegue hasta la &lt;bosque&gt;> \Domains\\&lt;dominio&gt; (donde &lt;bosque&gt; es el nombre del bosque y &lt;dominio&gt; es el nombre del dominio donde desea establecer la directiva de grupo).  

    2.  Haga clic en la unidad organizativa que se aplicarán a y haga clic en el GPO **vincular un GPO existente**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Crear vínculos a todas las otras unidades organizativas que contengan las estaciones de trabajo.  

    5.  Crear vínculos a todas las otras unidades organizativas que contengan servidores miembro.  

        > [!IMPORTANT]  
        > Si los servidores de salto se usan para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no está vinculado el GPO.  

        > [!NOTE]  
        > Cuando implementa restricciones en el grupo de administradores en los GPO, Windows aplica la configuración a los miembros del grupo de administradores local del equipo además de grupo de administradores del dominio. Por lo tanto, se debe tener cuidado al implementar restricciones en el grupo de administradores. Aunque la prohibición de red, batch y los inicios de sesión de servicio para los miembros del grupo Administradores es, recomienda siempre que lo sea factible implementar, no restrinja inicios de sesión locales o los inicios de sesión a través de servicios de escritorio remoto. El bloqueo de estos tipos de inicio de sesión puede impedir administración legítima de un equipo por los miembros del grupo Administradores local.  
        >   
        > Captura de pantalla siguiente muestra los valores de configuración que bloquean el uso indebido de integrados local y dominio de cuentas de administrador, además de uso indebido del grupo Administradores integrado del dominio o local. Tenga en cuenta que el **Denegar inicio de sesión a través de servicios de escritorio remoto** derecho de usuario no incluye el grupo de administradores, ya que incluirlo en esta configuración también bloquearía estos inicios de sesión para las cuentas que son miembros del equipo local Grupo de administradores. Si los servicios en los equipos están configurados para ejecutarse en el contexto de cualquiera de los grupos con privilegios descritos en esta sección, implementación de esta configuración puede provocar aplicaciones y servicios a un error. Por lo tanto, al igual que con todas las recomendaciones de esta sección, debe probar exhaustivamente la configuración para la aplicación en su entorno.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Instrucciones paso a paso, conceda al usuario derechos al grupo de administradores  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **Group Policy Management**.  

2.  En el árbol de consola, expanda <Forest>\Domains\\<Domain>y, a continuación, **Group Policy Objects** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desee establecer la directiva de grupo).  

3.  En el árbol de consola, haga clic en **Group Policy Objects**y haga clic en **New**.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  En el **nuevo GPO** cuadro de diálogo, escriba <GPO Name>y haga clic en **Aceptar** (donde <GPO Name> es el nombre de este GPO).  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  En el panel de detalles, haga clic en **<GPO Name>** y haga clic en **editar**.  

6.  Vaya a **configuración del equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haga clic en **asignación de derechos de usuario**.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configurar los derechos de usuario para permitir a los miembros del grupo Administradores en controladores de dominio de acceso a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

8.  Configurar los derechos de usuario para permitir a los miembros del grupo Administradores iniciar sesión localmente mediante los pasos siguientes:  

    1.  Haga doble clic en **Permitir inicio de sesión localmente** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administradores**, haga clic en comprobar **nombres**y haga clic en **Aceptar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

9. Configurar los derechos de usuario para permitir a los miembros del grupo Administradores iniciar sesión a través de servicios de escritorio remoto mediante los pasos siguientes:  

    1.  Haga doble clic en **Permitir inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **administradores**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

10. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y haga clic en **salir**.  

11. En **Group Policy Management**, vincule el GPO a la unidad organizativa controladores de dominio haciendo lo siguiente:  

    1.  Navegue hasta la <Forest>\Domains\\ <Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio donde desea establecer la directiva de grupo).  

    2.  Haga clic en la unidad organizativa y haga clic en los controladores de dominio **vincular un GPO existente**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar**.  

        ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Pasos de comprobación  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Compruebe la configuración de GPO de "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (por ejemplo, "servidor de salto"), intente acceder a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el uso de la **NET USE** comando.  

1.  Inicie sesión localmente con una cuenta que sea miembro del grupo Administradores.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **símbolo**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador** para abrir un elevado símbolo del sistema.  

4.  Cuando se le solicite que aprobar la elevación, haga clic en **Sí**.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  En el **símbolo** ventana, escriba **net use \\ \\ \<nombre del servidor\>\c$**, donde \<nombre del servidor\> es el nombre del servidor miembro o estación de trabajo que está intentando tener acceso a través de la red.  

6.  Captura de pantalla siguiente muestra el mensaje de error que debe aparecer.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

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

2.  En el **búsqueda** , escriba **programador de tareas**y haga clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en el cuadro de búsqueda, escriba las tareas de programación y haga clic en programar tareas.  

3.  Haga clic en **acción**y haga clic en **crear tarea**.  

4.  En el **crear tarea** cuadro de diálogo, escriba **<Task Name>** (donde <Task Name> es el nombre de la nueva tarea).  

5.  Haga clic en el **acciones** ficha y haga clic en **New**.  

6.  En el **acción** campos, seleccione **iniciar un programa**.  

7.  En el **programa o script** , a continuación, haga clic en **examinar**, busque y seleccione el archivo por lotes creado en el **crear un archivo por lotes** sección y haga clic en **abrir**.  

8.  Haga clic en **Aceptar**.  

9. Haga clic en la pestaña **General**.  

10. En el **las opciones de seguridad** , a continuación, haga clic en **Cambiar usuario o grupo**.  

11. Escriba el nombre de una cuenta que sea miembro del grupo Administradores, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

12. Seleccione **ejecutar si el usuario no está registrada onor** y **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haga clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo credenciales de cuenta de usuario solicitante para ejecutar la tarea.  

15. Después de escribir la contraseña, haga clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En el **iniciar sesión como** campos, seleccione **esta cuenta**.  

7.  Haga clic en **examinar**, escriba el nombre de una cuenta que sea miembro del grupo Administradores, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

8.  En el **contraseña** y **Confirmar contraseña** campos, escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar**.  

9. Haga clic en **Aceptar** tres veces más.  

10. Haga clic en **impresión** y haga clic en **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administración seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios realizados en el servicio cola de impresión  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En el **iniciar sesión como** , a continuación, haga clic en **sistema Local** cuenta y haga clic en **Aceptar**.  
