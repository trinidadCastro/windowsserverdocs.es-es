---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 'Apéndice G: protección de grupos de administradores en Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cdea04e211b1873ff51c4bc3dc9ff24e746ead69
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408646"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Anexo G: protección de grupos de administradores en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Anexo G: protección de grupos de administradores en Active Directory  
Como es el caso de los grupos administradores de empresas (EA) y Admins. del dominio (DA), la pertenencia al grupo administradores integrados (BA) solo debe ser necesaria en escenarios de recuperación ante desastres o de compilación. No debe haber cuentas de usuario diarias en el grupo de administradores, salvo la cuenta de administrador integrada del dominio, si se ha protegido como se describe en el [Apéndice D: protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

De forma predeterminada, los administradores son los propietarios de la mayoría de los objetos de AD DS en sus respectivos dominios. La pertenencia a este grupo puede ser necesaria en escenarios de compilación o recuperación ante desastres en los que se requiere la propiedad o la capacidad de asumir la propiedad de los objetos. Además, DAs y EAs heredan una serie de sus derechos y permisos en virtud de su pertenencia predeterminada en el grupo de administradores. No se debe modificar el anidamiento de grupos predeterminado para los grupos con privilegios en Active Directory y el grupo de administradores de cada dominio debe protegerse tal y como se describe en las instrucciones paso a paso que se indican a continuación.  

Para el grupo administradores de cada dominio del bosque:  

1.  Quite todos los miembros del grupo administradores, con la posible excepción de la cuenta de administrador integrada para el dominio, siempre que se haya protegido como se describe en el [Apéndice D: protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  En los GPO vinculados a unidades organizativas que contienen servidores miembro y estaciones de trabajo en cada dominio, el grupo de BA debe agregarse a los siguientes derechos de usuario en el **equipo \ configuración de seguridad\Directivas \ directivas de seguridad\Directivas \ asignación de derechos de usuario**:  

    -   Denegar el acceso desde la red a este equipo  

    -   Denegar el inicio de sesión como trabajo por lotes  

    -   Denegar el inicio de sesión como servicio  

3.  En la unidad organizativa controladores de dominio de cada dominio del bosque, al grupo administradores se le deben conceder los siguientes derechos de usuario:  

    -   Obtener acceso a este equipo desde la red  

    -   Permitir el inicio de sesión local  

    -   Permitir inicio de sesión a través de Servicios de Escritorio remoto  

4.  La auditoría debe estar configurada para enviar alertas si se realizan modificaciones en las propiedades o en la pertenencia del grupo administradores.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Instrucciones paso a paso para quitar todos los miembros del grupo administradores  

1.  En **Administrador del servidor**, haga clic en **herramientas**y en **Active Directory usuarios y equipos**.  

2.  Para quitar todos los miembros del grupo administradores, siga estos pasos:  

    1.  Haga doble clic en el grupo **administradores** y haga clic en la pestaña **miembros** .  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Seleccione un miembro del grupo, haga clic en **quitar**, haga clic en **sí**y, a continuación, haga clic en **Aceptar**.  

3.  Repita el paso 2 hasta que se hayan quitado todos los miembros del grupo administradores.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Instrucciones paso a paso para proteger los grupos de administradores en Active Directory  

1.  En **Administrador del servidor**, haga clic en **herramientas**y en **Administración de directiva de grupo**.  

2.  En el árbol de consola, expanda &lt;bosque&gt;\Domains\\&lt;dominio&gt;y, a continuación, **Directiva de grupo objetos** (donde &lt;bosque&gt; es el nombre del bosque y &lt;dominio&gt; es el nombre del dominio en el que desea establecer la Directiva de grupo).  

3.  En el árbol de consola, haga clic con el botón secundario en **Directiva de grupo objetos**y haga clic en **nuevo**.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  En el cuadro de diálogo **nuevo GPO** , escriba <GPO Name>y haga clic en **Aceptar** (donde *nombre de GPO* es el nombre de este GPO).  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  En el panel de detalles, haga clic con el botón secundario en **<GPO Name>** y haga clic en **Editar**.  

6.  Vaya a **equipo \ configuración de Seguridad\directivas \ directivas de seguridad\Directivas**y haga clic en **asignación de derechos de usuario**.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configure los derechos de usuario para evitar que los miembros del grupo administradores tengan acceso a los servidores y estaciones de trabajo de los miembros a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **administradores**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

8.  Configure los derechos de usuario para evitar que los miembros del grupo administradores inicien sesión como un trabajo por lotes haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **administradores**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

9. Configure los derechos de usuario para evitar que los miembros del grupo administradores inicien sesión como servicio de la siguiente manera:  

    1.  Haga doble clic en **denegar el inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **administradores**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

10. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y, a continuación, en **salir**.  

11. En **Administración de directiva de grupo**, VINCULE el GPO al servidor miembro y las unidades organizativas de la estación de trabajo haciendo lo siguiente:  

    1.  Navegue hasta el &lt;bosque&gt;> \Domains\\&lt;&gt; de dominio (donde &lt;bosque&gt; es el nombre del bosque y &lt;dominio&gt; es el nombre del dominio en el que desea establecer la directiva de grupo).  

    2.  Haga clic con el botón secundario en la unidad organizativa a la que se aplicará el GPO y haga clic en **vincular un GPO existente**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Cree vínculos a todas las demás unidades organizativas que contengan estaciones de trabajo.  

    5.  Cree vínculos a todas las demás unidades organizativas que contengan servidores miembro.  

        > [!IMPORTANT]  
        > Si se usan servidores de salto para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no están vinculados los GPO.  

        > [!NOTE]  
        > Cuando se implementan restricciones en el grupo de administradores de GPO, Windows aplica la configuración a los miembros del grupo de administradores locales de un equipo además del grupo de administradores del dominio. Por lo tanto, debe tener cuidado al implementar restricciones en el grupo de administradores. Aunque se recomienda prohibir los inicios de sesión de red, de batch y de servicio para los miembros del grupo de administradores, siempre que sea factible implementarlos, no restrinja los inicios de sesión locales ni los inicios de sesión a través de Servicios de Escritorio remoto. El bloqueo de estos tipos de inicio de sesión puede bloquear la administración legítima de un equipo por parte de los miembros del grupo de administradores locales.  
        >   
        > La siguiente captura de pantalla muestra los valores de configuración que bloquean el uso indebido de las cuentas de administrador de dominio y locales integradas, además de un uso incorrecto de los grupos de administradores locales o de dominio integrados. Tenga en cuenta que el derecho de usuario **denegar el inicio de sesión a través de servicios de escritorio remoto** no incluye el grupo administradores, ya que si se incluye en esta configuración, también se bloquearán estos inicios de sesión para las cuentas que son miembros del grupo administradores del equipo local. Si los servicios de en los equipos están configurados para ejecutarse en el contexto de cualquiera de los grupos con privilegios descritos en esta sección, la implementación de esta configuración puede provocar errores en los servicios y las aplicaciones. Por lo tanto, al igual que con todas las recomendaciones de esta sección, debe probar exhaustivamente la configuración de aplicabilidad en su entorno.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Instrucciones paso a paso para conceder derechos de usuario al grupo de administradores  

1.  En **Administrador del servidor**, haga clic en **herramientas**y en **Administración de directiva de grupo**.  

2.  En el árbol de consola, expanda <Forest>\Domains\\<Domain>y, a continuación, **Directiva de grupo objetos** (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea establecer la Directiva de grupo).  

3.  En el árbol de consola, haga clic con el botón secundario en **Directiva de grupo objetos**y haga clic en **nuevo**.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  En el cuadro de diálogo **nuevo GPO** , escriba <GPO Name>y haga clic en **aceptar** (donde <GPO Name> es el nombre de este GPO).  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  En el panel de detalles, haga clic con el botón secundario en **<GPO Name>** y haga clic en **Editar**.  

6.  Vaya a **equipo \ configuración de Seguridad\directivas \ directivas de seguridad\Directivas**y haga clic en **asignación de derechos de usuario**.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configure los derechos de usuario para permitir que los miembros del grupo administradores tengan acceso a los controladores de dominio a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

8.  Configure los derechos de usuario para permitir que los miembros del grupo administradores inicien sesión localmente mediante el procedimiento siguiente:  

    1.  Haga doble clic en **permitir el inicio de sesión local** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **administradores**, haga clic en comprobar **nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

9. Configure los derechos de usuario para permitir que los miembros del grupo administradores inicien sesión a través de Servicios de Escritorio remoto haciendo lo siguiente:  

    1.  Haga doble clic en **permitir el inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.  

    3.  Escriba **administradores**, haga clic en **Comprobar nombres**y haga clic en **Aceptar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Haga clic en **Aceptar**y en **Aceptar** de nuevo.  

10. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y, a continuación, en **salir**.  

11. En **Administración de directiva de grupo**, VINCULE el GPO a la unidad organizativa controladores de dominio haciendo lo siguiente:  

    1.  Navegue hasta el <Forest>\Domains\\<Domain> (donde <Forest> es el nombre del bosque y <Domain> es el nombre del dominio en el que desea establecer la directiva de grupo).  

    2.  Haga clic con el botón secundario en la unidad organizativa controladores de dominio y haga clic en **vincular un GPO existente**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar**.  

        ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Pasos de comprobación  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se vea afectado por los cambios de GPO (como un "servidor de salto"), intente tener acceso a un servidor miembro o una estación de trabajo a través de la red que afecte a los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el comando **net use** .  

1.  Inicie sesión localmente con una cuenta que sea miembro del grupo administradores.  

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

3.  En el cuadro de **búsqueda** , escriba **símbolo del sistema**, haga clic con el botón secundario en símbolo del **sistema**y, a continuación, haga clic en **Ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.  

4.  Cuando se le pida que apruebe la elevación, haga clic en **sí**.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  En la ventana del **símbolo del sistema** , escriba **net use \\\\nombre del servidor \<\>\c $** , donde \<nombre del servidor\> es el nombre del servidor miembro o de la estación de trabajo a la que está intentando obtener acceso a través de la red.  

6.  En la captura de pantalla siguiente se muestra el mensaje de error que debe aparecer.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como trabajo por lotes"  
Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.  

###### <a name="create-a-batch-file"></a>Crear un archivo por lotes  

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

2.  En el cuadro de **búsqueda** , escriba **Notepad**y haga clic en **Bloc de notas**.  

3.  En **el Bloc de notas**, escriba **dir c:** .  

4.  Haga clic en **archivo**y en **Guardar como**.  

5.  En el campo **nombre de archivo** , escriba **<Filename>. bat** (donde <Filename> es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

2.  En el cuadro de **búsqueda** , escriba **programador de tareas**y haga clic en **programador de tareas**.  

    > [!NOTE]  
    > En equipos que ejecutan Windows 8, en el cuadro de búsqueda, escriba programar tareas y haga clic en programar tareas.  

3.  Haga clic en **acción**y en **crear tarea**.  

4.  En el cuadro de diálogo **crear tarea** , escriba **<Task Name>** (donde <Task Name> es el nombre de la nueva tarea).  

5.  Haga clic en la pestaña **acciones** y en **nuevo**.  

6.  En el campo **acción** , seleccione **iniciar un programa**.  

7.  En el campo **programa/script** , haga clic en **examinar**, busque y seleccione el archivo por lotes creado en la sección **crear un archivo por lotes** y haga clic en **abrir**.  

8.  Haz clic en **Aceptar**.  

9. Haga clic en la pestaña **General**.  

10. En el campo **Opciones de seguridad** , haga clic en **cambiar usuario o grupo**.  

11. Escriba el nombre de una cuenta que sea miembro del grupo administradores, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.  

12. Seleccione **ejecutar si el usuario ha registrado Onor** y no **almacenar la contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haz clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo que solicite las credenciales de la cuenta de usuario para ejecutar la tarea.  

15. Después de escribir la contraseña, haga clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

3.  En el cuadro de **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En el campo **iniciar sesión como** , seleccione **esta cuenta**.  

7.  Haga clic en **examinar**, escriba el nombre de una cuenta que sea miembro del grupo administradores, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.  

8.  En los campos **contraseña** y **Confirmar contraseña** , escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar**.  

9. Haga clic en **Aceptar** tres veces más.  

10. Haga clic con el botón secundario en **Administrador de trabajos de impresión** y haga clic en **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![proteger grupos de administración](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio cola de impresión  

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.  

3.  En el cuadro de **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En el campo **iniciar sesión como** , haga clic en cuenta de **sistema local** y haga clic en **Aceptar**.  
