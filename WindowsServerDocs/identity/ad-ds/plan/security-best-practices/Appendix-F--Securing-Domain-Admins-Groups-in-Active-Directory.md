---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: Apéndice F - protección de grupos de administradores de dominio de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1f35503d1f02d616255c067fbc1750a0cab974cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880146"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apéndice F: Protección de grupos de administradores de dominio de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apéndice F: Protección de grupos de administradores de dominio de Active Directory  
Como sucede con el grupo de administradores de Enterprise (EA), deben pertenecer al grupo de administradores de dominio (DA) solo en escenarios de recuperación ante desastres o de compilación. No debe haber ninguna cuenta de usuario diarias en el grupo DA con la excepción de la cuenta predefinida de administrador para el dominio, si se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Administradores de dominio son, de forma predeterminada, los miembros de los grupos de administradores locales en todos los servidores miembro y estaciones de trabajo en sus dominios respectivos. Este anidamiento predeterminado no debe modificarse para compatibilidad y recuperación ante desastres. Si se quitaron los grupos de administradores locales en los servidores miembro de Admins. del dominio, el grupo debe agregarse al grupo de administradores en cada servidor miembro y la estación de trabajo en el dominio. Grupo de administradores de dominio de cada dominio debe protegerse como se describe en las instrucciones paso a paso siguientes.  

Para el grupo de administradores de dominio en cada dominio del bosque:  

1.  Quite todos los miembros del grupo, con la excepción posible de la cuenta de administrador integrada para el dominio, siempre que se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  En los GPO vinculados a unidades organizativas que contiene los servidores miembro y estaciones de trabajo en cada dominio, se debe agregar el grupo DA a los siguientes derechos de usuario en **equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas locales\Asignación derechos Las asignaciones de**:  

    -   Denegar el acceso desde la red a este equipo  

    -   Denegar el inicio de sesión como trabajo por lotes  

    -   Denegar el inicio de sesión como servicio  

    -   Denegar el inicio de sesión localmente  

    -   Denegar inicio de sesión a través de los derechos de usuario de servicios de escritorio remoto  

3.  La auditoría debe configurarse para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia del grupo Admins. del dominio.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Instrucciones paso a paso para quitar a todos los miembros del grupo de administradores de dominio  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **equipos y usuarios de Active Directory**.  

2.  Para quitar a todos los miembros del grupo DA, realice los pasos siguientes:  

    1.  Haga doble clic en el **Admins. del dominio** de grupo y haga clic en el **miembros** ficha.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Seleccione un miembro del grupo, haga clic en **quitar**, haga clic en **Sí**y haga clic en **Aceptar**.  

3.  Repita el paso 2 hasta que se han quitado todos los miembros del grupo de DA.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instrucciones paso a paso para proteger los administradores de dominio de Active Directory  

1.  En **administrador del servidor**, haga clic en **herramientas**y haga clic en **Group Policy Management**.  

2.  En el árbol de consola, expanda \<bosque\>\\dominios\\\<dominio\>y, a continuación, **Group Policy Objects** (donde \<bosque\> es el nombre del bosque y \<dominio\> es el nombre del dominio donde desea establecer la directiva de grupo).  

3.  En el árbol de consola, haga clic en **Group Policy Objects**y haga clic en **New**.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  En el **nuevo GPO** cuadro de diálogo, escriba \<nombre del GPO\>y haga clic en **Aceptar** (donde \<nombre del GPO\> es el nombre de este GPO).  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  En el panel de detalles, haga clic en \<nombre del GPO\>y haga clic en **editar**.  

6.  Vaya a **configuración del equipo\Directivas\Configuración de Windows\Configuración de Seguridad\directivas**y haga clic en **asignación de derechos de usuario**.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configurar los derechos de usuario para impedir que los miembros del grupo Administradores del dominio tengan acceso a los servidores miembros y estaciones de trabajo a través de la red haciendo lo siguiente:  

    1.  Haga doble clic en **denegar el acceso a este equipo desde la red** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **Admins. del dominio**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

8.  Configurar los derechos de usuario para impedir que los miembros del grupo DA inician sesión como trabajo por lotes mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como trabajo por lotes** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **Admins. del dominio**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

9. Configurar los derechos de usuario para impedir que los miembros del grupo DA inician sesión como un servicio mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión como servicio** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **Admins. del dominio**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

10. Configurar los derechos de usuario para impedir que los miembros del grupo Admins. del dominio iniciar sesión localmente en los servidores miembro y estaciones de trabajo mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión localmente** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **Admins. del dominio**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

11. Configurar los derechos de usuario para impedir que los miembros del grupo Administradores del dominio tengan acceso a los servidores miembro y estaciones de trabajo a través de servicios de escritorio remoto mediante los pasos siguientes:  

    1.  Haga doble clic en **Denegar inicio de sesión a través de servicios de escritorio remoto** y seleccione **definir esta configuración de directiva**.  

    2.  Haga clic en **Agregar usuario o grupo** y haga clic en **examinar**.  

    3.  Tipo **Admins. del dominio**, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Haga clic en **Aceptar**, y **Aceptar** nuevo.  

12. Para salir **Editor de administración de directivas de grupo**, haga clic en **archivo**y haga clic en **salir**.  

13. En administración de directivas de grupo, vincule el GPO para el servidor miembro y las unidades organizativas de la estación de trabajo mediante los pasos siguientes:  

    1.  Navegue hasta la \<bosque\>\Domains\\\<dominio\> (donde \<bosque\> es el nombre del bosque y \<dominio\> es el nombre de el dominio donde desea establecer la directiva de grupo).  

    2.  Haga clic en la unidad organizativa que se aplicarán a y haga clic en el GPO **vincular un GPO existente**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Seleccione el GPO que acaba de crear y haga clic en **Aceptar**.  

        ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Crear vínculos a todas las otras unidades organizativas que contengan las estaciones de trabajo.  

    5.  Crear vínculos a todas las otras unidades organizativas que contengan servidores miembro.  

        > [!IMPORTANT]  
        > Si los servidores de salto se usan para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no está vinculado el GPO.  

#### <a name="verification-steps"></a>Pasos de comprobación  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Compruebe la configuración de GPO de "Denegar el acceso a este equipo desde la red"  
Desde cualquier servidor miembro o estación de trabajo que no se ve afectado por los cambios de GPO (por ejemplo, "servidor de salto"), intente acceder a un servidor miembro o estación de trabajo a través de la red que se ve afectada por los cambios de GPO. Para comprobar la configuración del GPO, intente asignar la unidad del sistema mediante el uso de la **NET USE** comando.  

1.  Inicie sesión localmente con una cuenta que sea miembro del grupo Administradores del dominio.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **símbolo**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador** para abrir un elevado símbolo del sistema.  

4.  Cuando se le solicite que aprobar la elevación, haga clic en **Sí**.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  En el **símbolo** ventana, escriba **net use \\ \\ \<nombre del servidor\>\c$**, donde \<nombre del servidor\> es el nombre del servidor miembro o estación de trabajo que está intentando tener acceso a través de la red.  

6.  Captura de pantalla siguiente muestra el mensaje de error que debe aparecer.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como trabajo por lotes"  

Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

###### <a name="create-a-batch-file"></a>Cree un archivo por lotes  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **el Bloc de notas**y haga clic en **el Bloc de notas**.  

3.  En **el Bloc de notas**, tipo **dir c:**.  

4.  Haga clic en **archivo**y haga clic en **Guardar como**.  

5.  En el **archivo** campo nombre, escriba  **\<Filename\>.bat** (donde \<Filename\> es el nombre del nuevo archivo por lotes).  

###### <a name="schedule-a-task"></a>Programar una tarea  

1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **programador de tareas**y haga clic en **programador de tareas**.  

    > [!NOTE]  
    > En los equipos que ejecutan Windows 8, en el **búsqueda** , escriba **programar tareas**y haga clic en **programar tareas**.  

3.  En el **programador de tareas** barra de menús, haga clic en **acción**y haga clic en **crear tarea**.  

4.  En el **crear tarea** cuadro de diálogo, escriba **\<nombre de la tarea\>** (donde \<nombre de la tarea\> es el nombre de la nueva tarea).  

5.  Haga clic en el **acciones** ficha y haga clic en **New**.  

6.  En el **acción** campos, seleccione **iniciar un programa**.  

7.  En **programa o script**, haga clic en **examinar**, busque y seleccione el archivo por lotes creado en el **crear un archivo por lotes** sección y haga clic en **abrir**.  

8.  Haga clic en **Aceptar**.  

9. Haga clic en la pestaña **General**.  

10. En **seguridad** opciones, haga clic en **Cambiar usuario o grupo**.  

11. Escriba el nombre de una cuenta que sea miembro del grupo Administradores del dominio, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

12. Seleccione **ejecutar si el usuario inició sesión como no** y seleccione **no almacenar contraseña**. La tarea solo tendrá acceso a los recursos del equipo local.  

13. Haga clic en **Aceptar**.  

14. Debe aparecer un cuadro de diálogo credenciales de cuenta de usuario solicitante para ejecutar la tarea.  

15. Después de escribir las credenciales, haga clic en **Aceptar**.  

16. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión como servicio"  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como**, seleccione el **esta cuenta** opción.  

7.  Haga clic en **examinar**, escriba el nombre de una cuenta que sea miembro del grupo Administradores del dominio, haga clic en **comprobar nombres**y haga clic en **Aceptar**.  

8.  En **contraseña** y **Confirmar contraseña**, escriba la contraseña de la cuenta seleccionada y haga clic en **Aceptar**.  

9. Haga clic en **Aceptar** tres veces más.  

10. Haga clic en **impresión** y haga clic en **reiniciar**.  

11. Cuando se reinicia el servicio, debe aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Revertir los cambios realizados en el servicio cola de impresión  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, inicie sesión localmente.  

2.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

3.  En el **búsqueda** , escriba **servicios**y haga clic en **servicios**.  

4.  Busque y haga doble clic en **impresión**.  

5.  Haga clic en la pestaña **Iniciar sesión**.  

6.  En **iniciar sesión como**, seleccione el **sistema Local** cuenta y haga clic en **Aceptar**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Compruebe la configuración de GPO "Denegar iniciar sesión localmente"  

1.  Desde cualquier servidor miembro o estación de trabajo afectados por los cambios de GPO, intente iniciar sesión localmente con una cuenta que sea miembro del grupo Admins. del dominio. Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Compruebe la configuración de GPO "Denegar inicio de sesión a través de servicios de escritorio remoto"    
1.  Con el mouse, mueva el puntero en la esquina superior derecha o inferior derecha de la pantalla. Cuando el **accesos** barra aparece, haga clic en **búsqueda**.  

2.  En el **búsqueda** , escriba **conexión a Escritorio remoto**y haga clic en **conexión a Escritorio remoto**.  

3.  En el **equipo** , escriba el nombre del equipo que desea conectarse y haga clic en **Connect**. (También puede escribir la dirección IP en lugar del nombre de equipo.)  

4.  Cuando se le solicite, proporcione credenciales para una cuenta que sea miembro del grupo Administradores del dominio.  

5.  Debería aparecer un cuadro de diálogo similar al siguiente.  

    ![grupos de administradores de dominio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
