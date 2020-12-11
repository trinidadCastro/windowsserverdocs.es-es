---
description: 'Más información acerca de: Apéndice F: protección de grupos de Admins. del dominio en Active Directory'
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: 'Apéndice F: protección de grupos de administradores de dominio en Active Directory'
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 591cab5cdba949b7f1828a6719904a2beae071e5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041643"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Anexo F: protección de grupos de administradores de dominio en Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Anexo F: protección de grupos de administradores de dominio en Active Directory
Como es el caso del grupo administradores de empresas (EA), la pertenencia al grupo Admins. del dominio (DA) solo debe ser necesaria en escenarios de recuperación ante desastres o de compilación. No debe haber cuentas de usuario cotidianas en el grupo de DA a excepción de la cuenta de administrador integrada del dominio, si se ha protegido como se describe en el [Apéndice D: protección de cuentas de administrador de Built-In en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).

De forma predeterminada, los administradores de dominio son miembros de los grupos de administradores locales en todos los servidores miembro y estaciones de trabajo de sus respectivos dominios. Este anidamiento predeterminado no debe modificarse para fines de compatibilidad y recuperación ante desastres. Si los administradores de dominio se han quitado de los grupos de administradores locales en los servidores miembro, el grupo debe agregarse al grupo de administradores en cada servidor miembro y estación de trabajo del dominio. El grupo Admins. del dominio de cada dominio debe protegerse tal y como se describe en las instrucciones paso a paso que se indican a continuación.

Para el grupo Admins. del dominio en cada dominio del bosque:

1.  Quite todos los miembros del grupo, con la posible excepción de la cuenta de administrador integrada para el dominio, siempre que se haya protegido como se describe en el [Apéndice D: protección de cuentas de administrador de Built-In en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).

2.  En los GPO vinculados a las unidades organizativas que contienen servidores miembro y estaciones de trabajo en cada dominio, el grupo de DA debe agregarse a los siguientes derechos de usuario en el **equipo \ configuración** de seguridad\Directivas de Seguridad\directivas locales \ asignaciones de derechos:

    -   Denegar el acceso desde la red a este equipo

    -   Denegar el inicio de sesión como trabajo por lotes

    -   Denegar el inicio de sesión como servicio

    -   Denegar el inicio de sesión localmente

    -   Denegar el inicio de sesión a través de Servicios de Escritorio remoto derechos de usuario

3.  La auditoría debe estar configurada para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia al grupo Admins. del dominio.

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Instrucciones paso a paso para quitar todos los miembros del grupo Admins. del dominio

1.  En **Administrador del servidor**, haga clic en **herramientas** y en **Active Directory usuarios y equipos**.

2.  Para quitar todos los miembros del grupo de DA, realice los pasos siguientes:

    1.  Haga doble clic en el grupo **Admins** . del dominio y haga clic en la pestaña **miembros** .

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)

    2.  Seleccione un miembro del grupo, haga clic en **quitar**, haga clic en **sí** y, a continuación, haga clic en **Aceptar**.

3.  Repita el paso 2 hasta que se hayan quitado todos los miembros del grupo de DA.

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instrucciones paso a paso para proteger los administradores de dominio en Active Directory

1.  En **Administrador del servidor**, haga clic en **herramientas** y en **Administración de directiva de grupo**.

2.  En el árbol de consola, expanda \<Forest\> \\ dominios \\ \<Domain\> y, a continuación, **Directiva de grupo objetos** (donde \<Forest\> es el nombre del bosque y \<Domain\> es el nombre del dominio en el que desea establecer el Directiva de grupo).

3.  En el árbol de consola, haga clic con el botón secundario en **Directiva de grupo objetos** y haga clic en **nuevo**.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)

4.  En el cuadro de diálogo **nuevo GPO** , escriba \<GPO Name\> y haga clic en **Aceptar** (donde \<GPO Name\> es el nombre de este GPO).

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)

5.  En el panel de detalles, haga clic con el botón secundario \<GPO Name\> y haga clic en **Editar**.

6.  Vaya a **equipo \ configuración de Seguridad\directivas \ directivas de seguridad\Directivas** y haga clic en **asignación de derechos de usuario**.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)

7.  Configure los derechos de usuario para evitar que los miembros del grupo Admins. del dominio tengan acceso a los servidores y estaciones de trabajo de los miembros a través de la red haciendo lo siguiente:

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.

    3.  Escriba **Admins**. del dominio, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

8.  Configure los derechos de usuario para evitar que los miembros del grupo de DA inicien sesión como un trabajo por lotes haciendo lo siguiente:

    1.  Haga doble clic en **denegar el inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.

    3.  Escriba **Admins**. del dominio, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

9. Configure los derechos de usuario para evitar que los miembros del grupo de DA inicien sesión como servicio de la siguiente manera:

    1.  Haga doble clic en **denegar el inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.

    3.  Escriba **Admins**. del dominio, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

10. Configure los derechos de usuario para evitar que los miembros del grupo Admins. del dominio inicien sesión localmente en los servidores miembro y en las estaciones de trabajo haciendo lo siguiente:

    1.  Haga doble clic en **denegar el inicio de sesión localmente** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.

    3.  Escriba **Admins**. del dominio, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

11. Configure los derechos de usuario para evitar que los miembros del grupo Admins. del dominio tengan acceso a los servidores y estaciones de trabajo miembro a través de Servicios de Escritorio remoto de la siguiente manera:

    1.  Haga doble clic en **denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.

    2.  Haga clic en **Agregar usuario o grupo** y, a continuación, en **examinar**.

    3.  Escriba **Admins**. del dominio, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)

    4.  Haga clic en **Aceptar** y en **Aceptar** de nuevo.

12. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo** y, a continuación, en **salir**.

13. En administración de directiva de grupo, vincule el GPO al servidor miembro y las unidades organizativas de la estación de trabajo haciendo lo siguiente:

    1.  Navegue hasta \<Forest\> \Domains \\ \<Domain\> (donde \<Forest\> es el nombre del bosque y \<Domain\> es el nombre del dominio en el que desea establecer el Directiva de grupo).

    2.  Haga clic con el botón secundario en la unidad organizativa a la que se aplicará el GPO y haga clic en **vincular un GPO existente**.

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar**.

        ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)

    4.  Cree vínculos a todas las demás unidades organizativas que contengan estaciones de trabajo.

    5.  Cree vínculos a todas las demás unidades organizativas que contengan servidores miembro.

        > [!IMPORTANT]
        > Si se usan servidores de salto para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no están vinculados los GPO.

#### <a name="verification-steps"></a>Pasos de comprobación

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Comprobar la configuración de GPO "denegar el acceso a este equipo desde la red"
Desde cualquier servidor miembro o estación de trabajo que no se vea afectado por los cambios de GPO (como un "servidor de salto"), intente tener acceso a un servidor miembro o una estación de trabajo a través de la red que afecte a los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el comando **net use** .

1.  Inicie sesión localmente con una cuenta que sea miembro del grupo Admins. del dominio.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

3.  En el cuadro de **búsqueda** , escriba **símbolo del sistema**, haga clic con el botón secundario en símbolo del **sistema** y, a continuación, haga clic en **Ejecutar como administrador** para abrir un símbolo del sistema con privilegios elevados.

4.  Cuando se le pida que apruebe la elevación, haga clic en **sí**.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)

5.  En la ventana del **símbolo del sistema** , escriba **net use \\ \\ \<Server Name\> \c $**, donde \<Server Name\> es el nombre del servidor miembro o de la estación de trabajo a la que está intentando obtener acceso a través de la red.

6.  En la captura de pantalla siguiente se muestra el mensaje de error que debe aparecer.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como trabajo por lotes"

Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

###### <a name="create-a-batch-file"></a>Crear un archivo por lotes

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

2.  En el cuadro de **búsqueda** , escriba **Notepad** y haga clic en **Bloc de notas**.

3.  En **el Bloc de notas**, escriba **dir c:**.

4.  Haga clic en **archivo** y en **Guardar como**.

5.  En el campo Nombre de **archivo** , escriba **\<Filename\> . bat** (donde \<Filename\> es el nombre del nuevo archivo por lotes).

###### <a name="schedule-a-task"></a>Programar una tarea

1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

2.  En el cuadro de **búsqueda** , escriba **programador de tareas** y haga clic en **programador de tareas**.

    > [!NOTE]
    > En equipos que ejecutan Windows 8, en el cuadro de **búsqueda** , escriba **programar tareas** y haga clic en **programar tareas**.

3.  En la barra de menús **programador de tareas** , haga clic en **acción** y, a continuación, en **crear tarea**.

4.  En el cuadro de diálogo **crear tarea** , escriba **\<Task Name\>** (donde \<Task Name\> es el nombre de la nueva tarea).

5.  Haga clic en la pestaña **acciones** y en **nuevo**.

6.  En el campo **acción** , seleccione **iniciar un programa**.

7.  En **programa/script**, haga clic en **examinar**, busque y seleccione el archivo por lotes creado en la sección **crear un archivo por lotes** y haga clic en **abrir**.

8.  Haga clic en **OK**.

9. Haga clic en la pestaña **General**.

10. En opciones de **seguridad** , haga clic en **cambiar usuario o grupo**.

11. Escriba el nombre de una cuenta que sea miembro del grupo Admins. del dominio, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

12. Seleccione **ejecutar si el usuario ha iniciado sesión o no** y seleccione no **almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.

13. Haga clic en **OK**.

14. Debe aparecer un cuadro de diálogo que solicite las credenciales de la cuenta de usuario para ejecutar la tarea.

15. Después de escribir las credenciales, haga clic en **Aceptar**.

16. Debería aparecer un cuadro de diálogo similar al siguiente.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión como servicio"

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

3.  En el cuadro de **búsqueda** , escriba **servicios** y haga clic en **servicios**.

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.

5.  Haga clic en la ficha **Iniciar sesión**.

6.  En **iniciar sesión como**, seleccione la opción **esta cuenta** .

7.  Haga clic en **examinar**, escriba el nombre de una cuenta que sea miembro del grupo Admins. del dominio, haga clic en **Comprobar nombres** y haga clic en **Aceptar**.

8.  En **contraseña** y **Confirmar contraseña**, escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar**.

9. Haga clic en **Aceptar** tres veces más.

10. Haga clic con el botón secundario en **Administrador de trabajos de impresión** y haga clic en **reiniciar**.

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios en el servicio cola de impresión

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, inicie sesión localmente.

2.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

3.  En el cuadro de **búsqueda** , escriba **servicios** y haga clic en **servicios**.

4.  Busque y haga doble clic en **Administrador de trabajos de impresión**.

5.  Haga clic en la ficha **Iniciar sesión**.

6.  En **iniciar sesión como**, seleccione la cuenta de **sistema local** y haga clic en **Aceptar**.

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Comprobar la configuración de GPO "denegar el inicio de sesión local"

1.  Desde cualquier servidor miembro o estación de trabajo afectada por los cambios de GPO, intente iniciar sesión localmente con una cuenta que sea miembro del grupo Admins. del dominio. Debería aparecer un cuadro de diálogo similar al siguiente.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Comprobar la configuración del GPO "denegar el inicio de sesión a través de Servicios de Escritorio remoto"
1.  Con el mouse, mueva el puntero a la esquina superior derecha o inferior derecha de la pantalla. Cuando aparezca la barra de **accesos** , haga clic en **Buscar**.

2.  En el cuadro de **búsqueda** , escriba **conexión a escritorio remoto** y haga clic en **conexión a escritorio remoto**.

3.  En el campo **equipo** , escriba el nombre del equipo al que desea conectarse y haga clic en **conectar**. (También puede escribir la dirección IP en lugar del nombre del equipo).

4.  Cuando se le solicite, proporcione las credenciales de una cuenta que sea miembro del grupo Admins. del dominio.

5.  Debería aparecer un cuadro de diálogo similar al siguiente.

    ![grupos de administradores de dominio seguros](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)
