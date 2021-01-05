---
title: Administrar cuentas de usuario en Windows Server Essentials
description: Obtenga información sobre la página usuarios del panel de Windows Server Essentials y cómo centraliza la información y las tareas que le ayudarán a administrar las cuentas de usuario.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: da6a85d2337af18464c1ce709d5a2c3cf3209e06
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811232"
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Administrar cuentas de usuario en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

La página de usuarios de la consola de Windows Server Essentials centraliza la información y las tareas que le ayudan a administrar las cuentas de usuario en la red de pequeña empresa. Para obtener información general sobre el panel de usuarios, consulte [información general del panel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).


##  <a name="managing-user-accounts"></a><a name="BKMK_ManageAccounts"></a> Administración de cuentas de usuario
 Los siguientes temas contienen información sobre cómo usar el escritorio de Windows Server Essentials para administrar las cuentas de usuario en el servidor:

-   [Agregar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)

-   [Quitar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)

-   [Ver cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)

-   [Cambiar el nombre para mostrar de la cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)

-   [Activar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)

-   [Desactivar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)

-   [Comprender las cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)

-   [Administrar cuentas de usuario mediante el panel](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)

###  <a name="add-a-user-account"></a><a name="BKMK_Manage1"></a> Agregar una cuenta de usuario
 Al agregar una cuenta de usuario, el usuario asignado puede iniciar sesión en la red, y se le puede conceder al usuario permiso de acceso a los recursos de red como las carpetas compartidas y el sitio de acceso web remoto. Windows Server Essentials incluye el Asistente para agregar cuentas de usuario, que le ayudará a hacer lo siguiente:

-   Dar un nombre y una contraseña para la cuenta de usuario.

-   Definir la cuenta como cuenta de administrador o de usuario estándar.

-   Seleccionar a qué carpetas compartidas puede acceder la cuenta de usuario.

-   Especificar si la cuenta de usuario tiene acceso remoto a la red.

-   Si procede, seleccionar las opciones de correo electrónico.

-   Asigne una cuenta de Microsoft Online Services (denominada Microsoft 365 cuenta en Windows Server Essentials), si procede.

-   Asignar grupos de usuarios (solo en Windows Server Essentials).

> [!NOTE]
> - No se admiten caracteres que no sean ASCII en Microsoft Azure Active Directory (Azure AD). No use caracteres que no sean ASCII en la contraseña si el servidor se integra con Azure AD.
>   -   Las opciones de correo electrónico solo están disponibles si instala un complemento que ofrece el servicio de correo electrónico.

##### <a name="to-add-a-user-account"></a>Para agregar una cuenta de usuario

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Usuarios**.

3.  En el panel  **Tareas de usuarios** , haga clic en **Agregar una cuenta de usuario**. Se abrirá el asistente para agregar una cuenta de usuario.

4.  Siga las instrucciones para completar el asistente.

###  <a name="remove-a-user-account"></a><a name="BKMK_Remove"></a> Quitar una cuenta de usuario
 Si decide quitar una cuenta de usuario del servidor, un asistente elimina la cuenta seleccionada. Por este motivo, ya no se puede usar la cuenta para iniciar sesión en la red o para tener acceso a los recursos de red. Como opción, también puede eliminar los archivos de la cuenta de usuario a la vez que la quita. Si no quiere quitar la cuenta de usuario de forma permanente, en vez de ello puede desactivarla para suspender el acceso a recursos de red.

> [!IMPORTANT]
>  Si una cuenta de usuario tiene asignada una cuenta en línea de Microsoft, al quitar la cuenta de usuario, también se quita la cuenta en línea de Microsoft Online Services, y los datos del usuario, incluido el correo electrónico, están sujetos a las directivas de retención de datos de Microsoft Online Services. Si quiere conservar los datos de usuario para la cuenta en línea, desactive la cuenta de usuario en lugar de quitarla. Para obtener más información, consulte [administrar cuentas en línea para usuarios](Manage-Online-Accounts-for-Users.md).

##### <a name="to-remove-a-user-account"></a>Para quitar una cuenta de usuario

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Usuarios**.

3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiere quitar.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **quitar la cuenta de usuario**. Aparece el Asistente para eliminar cuentas de usuario.

5.  En la página **¿desea conservar los archivos?** del asistente, puede optar por eliminar los archivos del usuario, incluidas las copias de seguridad del historial de archivos y la carpeta redirigida de la cuenta de usuario. Para conservar los archivos del usuario, deje la casilla de verificación vacía. Después de hacer su selección, haga clic en **Siguiente**.

6.  Haga clic en **Eliminar cuenta**.

> [!NOTE]
>  Después de quitar una cuenta de usuario, la cuenta ya no aparece en la lista de cuentas de usuario. Si opta por eliminar los archivos, el servidor elimina de forma permanente la carpeta del usuario de la carpeta del servidor de **usuarios** y de la carpeta del servidor **copias de seguridad del historial de archivos** .
>
>  Si tiene un proveedor de correo electrónico integrado, también se quitará la cuenta de correo electrónico asignada a la cuenta de usuario.

###  <a name="view-user-accounts"></a><a name="BKMK_Manage3"></a> Ver cuentas de usuario
 La sección **Usuarios** del panel de Windows Server Essentials muestra una lista de cuentas de usuario de red. La lista también da información adicional acerca de cada cuenta.

##### <a name="to-view-a-list-of-user-accounts"></a>Para ver una lista de cuentas de usuario

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación principal, haga clic en **Usuarios**.

3.  El panel muestra una lista de cuentas de usuario actuales.

##### <a name="to-view-or-change-properties-for-a-user-account"></a>Para ver o cambiar las propiedades de una cuenta de usuario

1.  En la lista de cuentas de usuario, seleccione la cuenta para la que quiere ver o cambiar las propiedades.

2.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **ver las propiedades de la cuenta**. Aparece la página **Propiedades** de la cuenta de usuario.

3.  Haga clic en una pestaña para visualizar las propiedades de esa cuenta.

4.  Para guardar los cambios que haga en las propiedades de la cuenta de usuario, haga clic en **Aplicar**.

###  <a name="change-the-display-name-for-the-user-account"></a><a name="BKMK_Manage4"></a> Cambiar el nombre para mostrar de la cuenta de usuario
 El nombre para mostrar es el nombre que aparece en la columna **Nombre** de la página **Usuarios** del panel. Al cambiar el nombre para mostrar no cambia el nombre de inicio de sesión de una cuenta de usuario.

##### <a name="to-change-the-display-name-for-a-user-account"></a>Para cambiar el nombre para mostrar de una cuenta de usuario

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Usuarios**.

3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiera cambiar.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **ver las propiedades de la cuenta**. Aparece la página **Propiedades** de la cuenta de usuario.

5.  En la pestaña **General**, escriba un nuevo **Nombre** y **Apellidos** para la cuenta de usuario y después haga clic en **Aceptar**.

     El nuevo nombre para mostrar aparece en la lista de cuentas de usuario.

###  <a name="activate-a-user-account"></a><a name="BKMK_Manage5"></a> Activar una cuenta de usuario
 Cuando se activa una cuenta de usuario, el usuario asignado puede iniciar sesión en la red y acceder a los recursos de red para los que tiene permiso la cuenta, como las carpetas compartidas y el sitio de acceso web remoto.

> [!NOTE]
>  Solo puede activar una cuenta de usuario que está desactivada. No puede activar una cuenta de usuario después de quitarla del servidor.

##### <a name="to-activate-a-user-account"></a>Para activar una cuenta de usuario

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Usuarios**.

3.  En la vista de lista, seleccione la cuenta de usuario que quiere activar.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **activar la cuenta de usuario**.

5.  En la ventana de confirmación, haga clic en **Sí** para confirmar la acción.

> [!NOTE]
>  Después de activar una cuenta de usuario, el estado de la cuenta se muestra como **Activo**. La cuenta de usuario recupera los mismos derechos de acceso que se le asignaron antes de desactivarla.
>
>  Si tiene un proveedor de correo electrónico integrado, también se activará la cuenta de correo electrónico asignada a la cuenta de usuario.

###  <a name="deactivate-a-user-account"></a><a name="BKMK_Manage6"></a> Desactivar una cuenta de usuario
 Cuando desactiva una cuenta de usuario, se suspende temporalmente el acceso de la cuenta al servidor. Por este motivo, el usuario asignado no puede usar la cuenta para acceder a recursos de red como las carpetas compartidas o el sitio de acceso web remoto hasta que se active la cuenta.

 Si la cuenta de usuario tiene asignada una cuenta en línea de Microsoft, también se desactiva la cuenta en línea. El usuario no puede usar los recursos de Microsoft 365 y otros servicios en línea a los que se suscriba, pero los datos del usuario, incluido el correo electrónico, se conservan en Microsoft Online Services.

> [!NOTE]
>  Solo puede desactivar una cuenta de usuario que está activada actualmente.

##### <a name="to-deactivate-a-user-account"></a>Para desactivar una cuenta de usuario

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Usuarios**.

3.  En la vista de lista, seleccione la cuenta de usuario que quiere desactivar.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **desactivar la cuenta de usuario**.

5.  En la ventana de confirmación, haga clic en **Sí** para confirmar la acción.

> [!NOTE]
>  Después de desactivar una cuenta de usuario, el estado de la cuenta se muestra como **Inactivo**.
>
>  Si tiene un proveedor de correo electrónico integrado, también se desactivará la cuenta de correo electrónico asignada a la cuenta de usuario.

###  <a name="understand-user-accounts"></a><a name="BKMK_Manage7"></a> Descripción de las cuentas de usuario
 Una cuenta de usuario da información importante de Windows Server Essentials que permite a los usuarios acceder a la información que se almacena en el servidor y permite que los usuarios individuales creen y administren sus archivos y su configuración. Los usuarios pueden iniciar sesión en cualquier equipo de la red si tienen una cuenta de usuario de Windows Server Essentials y tienen permisos de acceso a un equipo. Los usuarios acceden a sus cuentas de usuario con su nombre de usuario y contraseña.

 Hay dos tipos principales de cuentas de usuario. Cada tipo ofrece a los usuarios un nivel diferente de control sobre el equipo:

-   Las cuentas **estándar** son para tareas habituales. La cuenta estándar le ayuda a proteger su red al evitar que los usuarios hagan cambios que afecten a otros usuarios, como eliminar archivos o cambiar la configuración de red.

-   Las cuentas de **administrador** dan el máximo control sobre una red de equipos. Solo debe asignar el tipo de cuenta de administrador cuando sea necesario.

###  <a name="manage-user-accounts-using-the-dashboard"></a><a name="BKMK_Manage8"></a> Administrar cuentas de usuario mediante el panel
 Windows Server Essentials permite realizar tareas administrativas comunes mediante su panel. De forma predeterminada, la página **usuarios** del panel incluye dos pestañas: **usuarios** y **grupos de usuarios**.

> [!NOTE]
> - Si integra el servidor que ejecuta Windows Server Essentials con Microsoft 365, también se agrega una nueva pestaña denominada **grupos de distribución** dentro de la página **usuarios** del panel.
>   -   En Windows Server Essentials, la página **usuarios** del panel solo incluye una pestaña- **usuarios**.

 La pestaña **Usuarios** incluye lo siguiente:

- Una lista de cuentas de usuario, que muestra:

  -   Nombre del usuario.

  -   El nombre de inicio de sesión de la cuenta de usuario.

  -   Si la cuenta de usuario tiene permiso de acceso desde cualquier lugar. El permiso de acceso desde cualquier lugar de una cuenta de usuario puede estar **Permitido** o **No permitido**.

  -   Si el servidor que ejecuta Windows Server Essentials administra el Historial de archivos para esta cuenta de usuario. El estado del Historial de archivos para una cuenta de usuario es **Administrado** o **No administrado**.

  -   El nivel de acceso que se asigna a la cuenta de usuario. Puede asignar un acceso de **usuario estándar** o de **administrador** a una cuenta de usuario.

  -   El estado de la cuenta de usuario. Una cuenta de usuario puede estar **Activa**, **Inactiva** o **Incompleta**.

  -   En Windows Server Essentials, si el servidor está integrado con Microsoft 365 o Windows Intune, se muestra la cuenta en línea de Microsoft.

  -   En Windows Server Essentials, si el servidor se integra con Microsoft 365, se muestra el estado de la cuenta (conocido en Windows Server Essentials como cuenta en línea de Microsoft) para la cuenta de usuario.

- Un panel de detalles con información adicional acerca de una cuenta de usuario seleccionada.

- Un panel de tareas que incluye:

  -   Un conjunto de tareas administrativas de cuenta de usuario como la visualización y eliminación de cuentas de usuario y el cambio de contraseñas.

  -   Tareas que le permiten establecer o cambiar la configuración de todas las cuentas de usuario en la red de forma global.

  En la tabla siguiente se describen las diversas tareas de cuenta de usuario que están disponibles en la pestaña **usuarios** . Algunas de las tareas son específicas de la cuenta de usuario y solo están visibles cuando se selecciona una cuenta de usuario en la lista.

> [!NOTE]
>  Si integra Microsoft 365 con Windows Server Essentials, las tareas adicionales estarán disponibles. Para obtener más información, consulte [administrar cuentas en línea para usuarios](Manage-Online-Accounts-for-Users.md).

### <a name="user-account-tasks-in-the-dashboard"></a>Tareas de la cuenta de usuario en el panel

|Nombre de la tarea|Description|
|---------------|-----------------|
|Ver las propiedades de la cuenta|Permite ver y cambiar las propiedades de la cuenta de usuario seleccionada y especificar los permisos de acceso a carpetas de la cuenta.|
|Desactivar la cuenta de usuario|No se puede iniciar sesión con una cuenta de usuario desactivada en la red ni acceder a recursos de red como carpetas o impresoras compartidas.|
|Activar la cuenta de usuario|Una cuenta de usuario que se activa puede iniciar sesión en la red y acceder a los recursos de red según la definición de los permisos de cuenta.|
|Quitar la cuenta de usuario|Permite quitar la cuenta de usuario seleccionada.|
|Cambiar la contraseña de la cuenta de usuario|Permite restablecer la contraseña de red para la cuenta de usuario seleccionada.|
|Agregar una cuenta de usuario|Inicia el Asistente para agregar cuentas de usuario, que le permite crear una nueva cuenta de usuario único que tenga acceso de usuario estándar o acceso de administrador.|
|Asignar una cuenta en línea de Microsoft|Agrega una cuenta en línea de Microsoft a la cuenta de usuario de red local seleccionada.<br /><br /> Esta tarea se muestra cuando el servidor se integra con Microsoft servicios en línea, como Microsoft 365.|
|Agregar cuentas en línea de Microsoft|Agrega cuentas en línea de Microsoft y las asocia a las cuentas de usuario de red local.<br /><br /> Esta tarea se muestra cuando el servidor se integra con Microsoft servicios en línea, como Microsoft 365.|
|Establecer la directiva de contraseñas|Permite cambiar los valores de la directiva de contraseñas de la red.|
|Importar cuentas en línea de Microsoft|Realiza una importación masiva de cuentas de Microsoft Online Services a la red local.<br /><br /> Esta tarea se muestra cuando el servidor se integra con Microsoft servicios en línea, como Microsoft 365.|
|Actualizar|Actualiza la pestaña Usuarios.<br /><br /> Esta tarea es aplicable a Windows Server Essentials.|
|Cambiar la configuración del Historial de archivos|Permite cambiar la configuración del Historial de archivos, como la frecuencia o la duración de la copia de seguridad.<br /><br /> Esta tarea es aplicable a Windows Server Essentials.|
|Exportar todas las conexiones remotas|Crea un archivo con formato .CSV de todas las conexiones remotas al servidor que se han producido durante los últimos 30 días.|

##  <a name="managing-passwords-and-access"></a><a name="BKMK_ManageAccess"></a> Administración de contraseñas y acceso
 Los siguientes temas contienen información sobre cómo usar el escritorio de Windows Server Essentials para administrar las contraseñas de las cuentas de usuario y el acceso de usuario a las carpetas compartidas del servidor:

-   [Cambiar o restablecer la contraseña de una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)

-   [Lo que debe saber acerca de las directivas de contraseñas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)

-   [Cambiar la directiva de contraseñas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)

-   [Nivel de acceso a las carpetas compartidas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)

-   [Conservar y administrar el acceso a los archivos para cuentas de usuario eliminadas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)

-   [Sincronizar la contraseña de DSRM con la contraseña del administrador de red](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)

-   [Conceder permiso de escritorio remoto a las cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)

-   [Permitir a los usuarios tener acceso a los recursos del servidor](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)

-   [Cambiar los permisos de acceso remoto para una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)

-   [Cambiar los permisos de red privada virtual de una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)

-   [Cambiar el acceso a las carpetas compartidas internas de una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)

-   [Permitir que las cuentas de usuario establezcan una sesión de escritorio remoto en su equipo](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)

###  <a name="change-or-reset-the-password-for-a-user-account"></a><a name="BKMK_Access1"></a> Cambiar o restablecer la contraseña de una cuenta de usuario
 Para cambiar o restablecer la contraseña de una cuenta de usuario, siga estos pasos.

##### <a name="to-reset-the-password-for-a-user-account"></a>Para restablecer la contraseña de una cuenta de usuario

1. Abra el panel de Windows Server Essentials.

2. En la barra de navegación, haga clic en **Usuarios**.

3. En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiere restablecer.

4. En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **cambiar la contraseña de la cuenta de usuario**. Aparece el Asistente para cambiar la contraseña de la cuenta de usuario.

5. Escriba una nueva contraseña para la cuenta de usuario y después escriba la contraseña para confirmarla.

6. Haga clic en **Cambiar contraseña**.

7. Dé la nueva contraseña al usuario.

   > [!IMPORTANT]
   > - Es posible que no pueda cambiar la contraseña si se ha establecido la directiva de contraseñas para su cuenta en **Las contraseñas nunca expiran**.
   >   -   No se admiten caracteres que no sean ASCII en Azure AD. Por lo tanto, si el servidor se integra con Azure AD, no use caracteres que no sean ASCII en la contraseña.
   >   -   Si se asigna al usuario una cuenta en línea de Microsoft (conocida en Windows Server Essentials como cuenta de Microsoft 365), la contraseña se sincroniza con la contraseña de la cuenta en línea. El usuario usará la nueva contraseña para iniciar sesión en el servidor o iniciar sesión en Microsoft 365. Para obtener más información, consulte [administrar cuentas en línea para usuarios](Manage-Online-Accounts-for-Users.md).

###  <a name="what-you-should-know-about-password-policies"></a><a name="BKMK_Access3"></a> Lo que debe saber acerca de las directivas de contraseñas
 La directiva de contraseñas es un conjunto de reglas que definen cómo crean y usan las contraseñas los usuarios. La directiva ayuda a impedir el acceso no autorizado a los datos de usuario y a otra información que se almacena en el servidor. La directiva de contraseñas se aplica a todas las cuentas de usuario que tengan acceso a la red.

 La directiva de contraseñas de Windows Server Essentials consta de tres elementos principales, que se describen a continuación:

- **Longitud de la contraseña**.  Cuanto más larga sea una contraseña, más segura será. Las contraseñas en blanco no son seguras.

- **Complejidad de la contraseña**.  Las contraseñas complejas contienen una combinación de letras en mayúsculas y minúsculas (a-z, A-z), números base (0-9) y símbolos no alfabéticos (como ;, !, @, #, _ y -). Las contraseñas complejas son mucho menos susceptibles a que se produzcan un acceso no autorizado. Las contraseñas que contienen nombres de usuario, cumpleaños u otra información personal no dan la seguridad adecuada.

- **Duración de la contraseña**.  Windows Server Essentials requiere que los usuarios cambien su contraseña al menos una vez cada 180 días. Como opción, puede elegir que las contraseñas no expiren nunca.

  Para que sea más fácil implementar una directiva de contraseñas en la red, Windows Server Essentials ofrece una sencilla herramienta que le permite establecer o modificar la directiva de contraseñas para cualquiera de los cuatro siguientes perfiles de directivas predefinidos:

- **Débil**.  Los usuarios pueden especificar cualquier contraseña que no esté en blanco.

- **Media**.  Estas contraseñas deben contener al menos 5 caracteres. No se requiere una contraseña compleja.

- **Medianamente segura**.  Estas contraseñas deben contener al menos 5 caracteres y deben incluir letras, números y símbolos.

- **Segura**.  Estas contraseñas deben contener al menos 7 caracteres y deben incluir letras, números y símbolos. Estas contraseñas son más seguras, pero pueden ser más difíciles de recordar para los usuarios.

  > [!NOTE]
  >  Las contraseñas no pueden contener la dirección de correo electrónico o el nombre de usuario.
  >
  >  Si integra con Microsoft 365, la integración aplica la Directiva de contraseñas **seguras** y actualiza la Directiva para que incluya los siguientes requisitos:
  >
  > - Las contraseñas deben contener 8 16 caracteres.
  >   -   Las contraseñas no pueden contener un espacio ni el nombre de correo electrónico del Microsoft 365.

  De forma predeterminada, la instalación del servidor establece la directiva de contraseñas predeterminada en la opción **Segura**.

###  <a name="change-the-password-policy"></a><a name="BKMK_Access4"></a> Cambiar la Directiva de contraseñas
 Use el siguiente procedimiento para establecer o modificar la directiva de contraseñas en cualquiera de los cuatro perfiles de directiva predefinidos.

##### <a name="to-change-the-password-policy"></a>Para cambiar la directiva de contraseñas

1.  Abra el panel de Windows Server Essentials y después haga clic en **Usuarios**.

2.  En el panel **Tareas de usuarios**, haga clic en **Configurar la directiva de contraseñas**.

3.  En la pantalla **Cambiar la directiva de contraseñas**, establezca el nivel de seguridad de la contraseña moviendo el control deslizante.

     Microsoft recomienda que configure la seguridad de la contraseña en **Segura**.

    > [!NOTE]
    >  Como opción, puede seleccionar también **Las contraseñas nunca expiran**. Esta configuración es menos segura, por eso no la recomendamos.

4.  Haga clic en **Cambiar directiva**.

###  <a name="level-of-access-to-shared-folders"></a><a name="BKMK_Access5"></a> Nivel de acceso a las carpetas compartidas
 Como práctica recomendada, debe asignar los permisos más restrictivos que haya y que aun así permitan a los usuarios realizar las tareas necesarias.

 Tiene tres opciones de configuración de acceso disponibles para las carpetas compartidas en el servidor:

-   **Lectura y escritura**.  Elija esta opción si quiere dar permiso a la cuenta de usuario para crear, modificar y eliminar los archivos de la carpeta compartida.

-   **Solo lectura**.  Elija esta opción si solo quiere dar permiso a la cuenta de usuario para leer los archivos de la carpeta compartida. Las cuentas de usuario con acceso de solo lectura no pueden crear, modificar ni eliminar archivos de la carpeta compartida.

-   **Sin acceso**.  Elija esta opción si no quiere que la cuenta de usuario tenga acceso a los archivos de la carpeta compartida.

###  <a name="retain-and-manage-access-to-files-for-removed-user-accounts"></a><a name="BKMK_Access6"></a> Conservar y administrar el acceso a los archivos de las cuentas de usuario eliminadas
 El administrador de red puede quitar una cuenta de usuario y elegir mantener los archivos del usuario para su uso futuro. En este escenario, la cuenta de usuario que se ha quitado ya no puede usarse para iniciar sesión en la red, pero se guardarán los archivos de este usuario en una carpeta compartida que puede compartirse con otro usuario.

> [!IMPORTANT]
>  Tenga en cuenta que si quita una cuenta de usuario que tenga asignada una cuenta en línea de Microsoft, también se quita la cuenta en línea, y los datos de usuario, incluida la dirección de correo electrónico, están sujetos a las directivas de retención de datos de Microsoft Online Services. Para conservar los datos de usuario para la cuenta en línea, desactive la cuenta de usuario en lugar de quitarla. Para obtener más información, consulte [administrar cuentas en línea para usuarios](Manage-Online-Accounts-for-Users.md).

##### <a name="to-remove-a-user-account-but-retain-access-to-the-users-files"></a>Para quitar una cuenta de usuario, pero conservar el acceso a los archivos del usuario

1. Abra el panel de Windows Server Essentials.

2. En la barra de navegación, haga clic en **Usuarios**.

3. En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiere quitar.

4. En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **quitar la cuenta de usuario**. Aparece el Asistente para eliminar cuentas de usuario.

5. En la página **¿Quiere mantener los archivos?**, asegúrese de que la casilla **Eliminar los archivos que incluyan copias de seguridad del Historial de archivos de esta cuenta de usuario** esté desactivada y después haga clic en **Siguiente**.

    Aparece una página de confirmación que le advierte de que se va a quitar la cuenta, pero de que los archivos se van a mantener.

6. Haga clic en **Eliminar cuenta** para quitar la cuenta de usuario.

   Después de quitar la cuenta de usuario, el administrador puede conceder acceso a la carpeta compartida a otra cuenta de usuario.

##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Para permitir a una cuenta de usuario tener acceso a una carpeta compartida

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Almacenamiento** y después haga clic en la pestaña **Carpetas del servidor**.

3.  En la lista de carpetas, seleccione la carpeta **Usuarios**.

4.  En el panel **Tareas de usuarios**, haga clic en **Abrir la carpeta**. El Explorador de Windows abre y muestra el contenido de la carpeta **Usuarios**.

5.  Haga clic con el botón secundario en la carpeta de la cuenta de usuario que quiere compartir y después haga clic en **Propiedades**.

6.  En **<\> propiedades** de la cuenta de usuario, haga clic en la pestaña **compartir** y, a continuación, haga clic en **compartir**.

7.  En la ventana **Uso compartido de archivos**, escriba o seleccione el nombre de la cuenta de usuario con la que quiere compartir la carpeta y después haga clic en **Agregar**.

8.  Elija el **nivel de permisos** que quiere que tenga la cuenta de usuario y después haga clic en **Compartir**.

###  <a name="synchronize-the-dsrm-password-with-the-network-administrator-password"></a><a name="BKMK_Access7"></a> Sincronizar la contraseña de DSRM con la contraseña de administrador de red
 El modo de restauración de servicios de directorio (DSRM) es un modo especial para reparar o recuperar Active Directory. El sistema operativo usa DSRM para iniciar sesión en el equipo si se produce un error en Active Directory o hay que restaurarlo. Si la contraseña del administrador de red y la contraseña de DSRM son diferentes, no se cargará DSRM.

 Cuando se hace una instalación limpia de Windows Server Essentials por primera vez, el programa establece la contraseña de DSRM en la contraseña de la cuenta de administrador de red que se especifique durante la instalación o en el archivo de respuesta de la migración. Al cambiar la contraseña del administrador de red (normalmente se recomienda hacerlo cada 60 días para aumentar la seguridad del servidor), el cambio de contraseña no se reenvía a DSRM. Esto produce un error de coincidencia de contraseña. Si esto ocurre, puede usar las siguientes soluciones para sincronizar de forma manual o automática la contraseña del administrador de la red con la contraseña de DSRM.

##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Para sincronizar de forma manual la contraseña de DSRM con una cuenta de administrador de red

1. En un símbolo del sistema, ejecute `ntdsutil.exe` para abrir la herramienta ntdsutil.

2. Para restablecer la contraseña de DSRM, escriba **set dsrm password**.

3. Para sincronizar la contraseña de DSRM en un controlador de dominio con la cuenta del administrador de red actual, escriba:

    **sync from domain account** *<cuenta_de_administrador_de_red_actual>* y, a continuación, presione ENTRAR.

   Como va a cambiar la contraseña de la cuenta del administrador de red de forma periódica, para asegurarse de que la contraseña de DSRM sea siempre la misma que la contraseña actual del administrador de red, le recomendamos que cree una tarea de programación para sincronizar a diario y de forma automática la contraseña de DSRM con la contraseña del administrador de red.

##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Para sincronizar de forma automática la contraseña de DSRM con una cuenta de administrador de red

1.  Desde el servidor, abra **Herramientas administrativas** y después haga doble clic en **Programador de tareas**.

2.  En el panel **Acciones** del Programador de tareas, haga clic en **Crear tarea**.

3.  En el cuadro de texto **Nombre**, escriba un nombre para la tarea, como **Contraseña de DSRM sincronización automática**, y después seleccione la opción **Ejecutar con los privilegios más altos**.

4.  Defina cuándo debe ejecutarse la tarea:

    1.  En el cuadro de diálogo **Crear tarea**, haga clic en la pestaña **Desencadenadores** y después haga clic en **Nuevo**.

    2.  En el cuadro de diálogo **Nuevo desencadenador**, seleccione la opción de periodicidad, especifique el intervalo de periodicidad y elija una hora de inicio.

        > [!NOTE]
        >  Como práctica recomendada, le aconsejamos que configure la tarea para que se ejecute diariamente durante el horario no comercial.

    3.  Haga clic en **Aceptar** para guardar los cambios y volver al cuadro de diálogo **Crear tarea**.

5.  Defina las acciones de la tarea:

    1.  Haga clic en la pestaña **Acciones** y después, en **Nueva**. Aparece el cuadro de diálogo **Nueva acción**.

    2.  En la lista **Acción**, haga clic en **Iniciar un programa** y después vaya a **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.

    3.  En el cuadro de texto **Agregar argumentos**(opcional), escriba lo siguiente (debe incluir las comillas): **establezca la sincronización de contraseñas de DSRM desde la cuenta de dominio SBS_network_administrator_account q q** , donde *SBS_network_administrator_account* es el nombre de cuenta del administrador de red actual.

6.  Haga clic en **Aceptar** dos veces para guardar la tarea y cerrar el cuadro de diálogo **Crear tarea**. La nueva tarea aparece en la sección **Tareas activas** de **Programación de tareas**.

###  <a name="give-user-accounts-remote-desktop-permission"></a><a name="BKMK_Access8"></a> Conceder permiso de escritorio remoto a las cuentas de usuario
 En la instalación predeterminada de Windows Server Essentials, los usuarios de red no tienen permiso para establecer una conexión remota a equipos u otros recursos de la red.

 Antes de que los usuarios de red puedan establecer una conexión remota a los recursos de red, debe configurar primero el Acceso desde cualquier lugar. Después de configurar el Acceso desde cualquier lugar, los usuarios pueden tener acceso a archivos, aplicaciones y equipos de la red de la oficina desde un dispositivo situado en cualquier ubicación y que tenga conexión a Internet.

 El Asistente para configurar el Acceso desde cualquier lugar permite habilitar dos métodos de acceso remoto:

- Red privada virtual (VPN)

- Acceso web remoto

  Al ejecutar el asistente, también puede permitir el Acceso desde cualquier lugar a todas las cuentas de usuario actuales y recién agregadas.

  Para configurar el Acceso desde cualquier lugar, abra la página **Inicio** del panel, haga clic en **CONFIGURAR** y después haga clic en **Configurar Acceso desde cualquier lugar**.

  Para obtener más información sobre el acceso desde cualquier lugar, consulte [Administración del acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).

###  <a name="enable-users-to-access-resources-on-the-server"></a><a name="BKMK_Access9"></a> Permitir a los usuarios tener acceso a los recursos del servidor
  Esta sección se aplica a un servidor que ejecuta Windows Server Essentials o Windows Server Essentials, o a un servidor que ejecuta Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol de experiencia con Windows Server Essentials instalado.

 Si quiere que los usuarios usen el acceso remoto o que tengan cuentas de usuario individuales, al acabar de conectar un equipo al servidor, puede crear nuevas cuentas de usuario de red en el servidor para los usuarios del equipo en red usando el panel. Para más información sobre cómo crear una cuenta de usuario, vea [Agregar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Después de crear las cuentas de usuario, debe dar la información de nombre y contraseña de usuario de red a los usuarios del equipo cliente para que puedan tener acceso a recursos en el servidor mediante Launchpad.

 Mediante las propiedades de cuenta de usuario, puede establecer acceso a lo siguiente para cada cuenta de usuario que cree:

-   **Carpetas compartidas**.  De forma predeterminada, los administradores de red tienen permisos de **lectura y escritura** para todas las carpetas compartidas, y las cuentas de usuario estándar tienen permisos de **solo lectura** para la carpeta de la empresa. Si se habilita la transmisión por secuencias de multimedia, puede asignar permisos de acceso a carpetas a las cuentas de usuario estándar individuales para las siguientes carpetas compartidas: **Música**, **Imágenes**, **TV grabada** y **Vídeos**. Puede establecer permisos para que las cuentas de usuario tengan acceso a las carpetas compartidas en la pestaña **Carpetas compartidas** de las propiedades de la cuenta de usuario.

-   **Acceso desde cualquier lugar**.  De forma predeterminada, los administradores de red pueden usar VPN o acceso web remoto a los recursos del servidor de acceso. Para las cuentas de usuario estándar, debe establecer los permisos de cuenta de usuario en la pestaña **Acceso desde cualquier lugar**.

-   **Acceso al equipo**.  De forma predeterminada, los administradores de red pueden acceder a todos los equipos de la red. Sin embargo, para las cuentas de usuario estándar, puede establecer permisos de cuenta de usuario individual para acceder a los equipos de la red en la pestaña **Acceso** de las propiedades de cuenta de usuario.

##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Para editar las propiedades de cuenta de usuario de Windows Server Essentials 2012 R2

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **USUARIOS**.

3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiere editar.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **ver las propiedades de la cuenta**.

5.  En el **<\> propiedades** de la cuenta de usuario, haga lo siguiente:

    1.  En la pestaña **Carpetas compartidas**, configure los permisos de carpeta adecuados para cada carpeta compartida según sea necesario.

    2.  En la pestaña **Acceso desde cualquier lugar**:

        1.  Para permitir que un usuario se conecte al servidor mediante VPN, active la casilla **Permitir red privada virtual (VPN)**.

        2.  Para permitir que un usuario se conecte al servidor mediante acceso Web remoto, active la casilla **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.

    3.  En la pestaña **Acceso al equipo**, seleccione los equipos de red a los que quiere que el usuario tenga acceso.

##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Para editar las propiedades de cuenta de usuario de Windows Server Essentials 2012

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **USUARIOS**.

3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiere editar.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **propiedades**.

5.  En el **<\> propiedades** de la cuenta de usuario, haga lo siguiente:

    1.  En la pestaña **General**, seleccione **El usuario puede ver las alertas de estado de la red** si la cuenta de usuario debe tener acceso a los informes de mantenimiento de la red.

    2.  En la pestaña **Carpetas compartidas**, configure los permisos de carpeta adecuados para cada carpeta compartida según sea necesario.

    3.  En la pestaña **Acceso desde cualquier lugar**:

        1.  Para permitir que un usuario se conecte al servidor mediante VPN, active la casilla **Permitir red privada virtual (VPN)**.

        2.  Para permitir que un usuario se conecte al servidor mediante acceso Web remoto, active la casilla **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.

    4.  En la pestaña **Acceso al equipo**, seleccione los equipos de red a los que quiere que el usuario tenga acceso.

###  <a name="change-remote-access-permissions-for-a-user-account"></a><a name="BKMK_Access10"></a> Cambiar los permisos de acceso remoto para una cuenta de usuario
 Un usuario puede tener acceso a recursos ubicados en el servidor desde una ubicación remota mediante una red privada virtual (VPN), acceso web remoto u otras aplicaciones de servicios web. De forma predeterminada, los permisos de acceso remoto están activados para usuarios de red al configurar Acceso desde cualquier lugar en Windows Server Essentials mediante el panel.

##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Para cambiar los permisos de acceso remoto de una cuenta de usuario

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **Usuarios**.

3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiera cambiar.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **ver las propiedades de la cuenta**. Aparece la página **Propiedades** de la cuenta de usuario.

5.  En la pestaña **Acceso desde cualquier lugar**, haga lo siguiente:

    -   Seleccione la casilla de verificación **Permitir red privada virtual (VPN)** para permitir que un usuario se conecte al servidor mediante VPN.

    -   Seleccione la casilla de verificación **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web** para permitir que un usuario se conecte al servidor mediante el acceso web remoto.

6.  Haga clic en **Aplicar** y en **Aceptar**.

###  <a name="change-virtual-private-network-permissions-for-a-user-account"></a><a name="BKMK_Access11"></a> Cambiar los permisos de red privada virtual de una cuenta de usuario
 Puede usar una red privada virtual (VPN) para conectarse a Windows Server Essentials y acceder a todos los recursos que se almacenan en el servidor. Esto resulta especialmente útil si un equipo cliente está configurado con las cuentas de red que pueden usarse para conectar con un servidor de Windows Server Essentials hospedado, a través de una conexión VPN. Todas las cuentas de usuario nuevas que se creen en el servidor de Windows Server Essentials hospedado deben usar VPN cuando inicien sesión en el equipo cliente por primera vez.

##### <a name="to-change-vpn-permissions-for-network-users"></a>Para cambiar los permisos de VPN de los usuarios de red

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **USUARIOS**.

3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario a la que quiere conceder permisos de acceso remoto al escritorio.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **propiedades**.

5.  En las **\> propiedades** de la cuenta de usuario de<, haga clic en la pestaña **acceso desde cualquier lugar** .

6.  En la pestaña **Acceso desde cualquier lugar**, seleccione la casilla de verificación **Permitir red privada virtual (VPN)** para permitir a un usuario conectarse al servidor mediante VPN.

7.  Haga clic en **Aplicar** y en **Aceptar**.

###  <a name="change-access-to-internal-shared-folders-for-a-user-account"></a><a name="BKMK_Access12"></a> Cambiar el acceso a las carpetas compartidas internas de una cuenta de usuario
 Puede administrar el acceso a las carpetas compartidas del servidor mediante las tareas de la pestaña **Carpetas del servidor** del panel. De forma predeterminada, se crean las siguientes carpetas de servidor al instalar Windows Server Essentials:

-   **Copias de seguridad de equipos cliente**.  Se usa para almacenar las copias de seguridad de un equipo cliente que crea la copia de seguridad de Windows Server. Esta carpeta del servidor no se comparte.

-   **Empresa**.  Se usa para que los usuarios de red almacenen y accedan a documentos relacionados con la organización.

-   **Copias de seguridad del Historial de archivos**.  De forma predeterminada, Windows Server Essentials almacena las copias de seguridad de archivos creadas mediante el Historial de archivos. Esta carpeta del servidor no se comparte.

-   **Redirección de carpetas**.  Se usa para que los usuarios de red almacenen y accedan a carpetas configuradas para la redirección de carpetas. Esta carpeta del servidor no se comparte.

-   **Música**.  Se usa para que los usuarios de red almacenen y accedan a archivos de música. Esta carpeta se crea al activar el uso compartido de multimedia.

-   **Imágenes**.  Se usa para que los usuarios de red almacenen y accedan a imágenes. Esta carpeta se crea al activar el uso compartido de multimedia.

-   **TV grabada**.  Se usa para que los usuarios de red almacenen y accedan a programas de TV grabados. Esta carpeta se crea al activar el uso compartido de multimedia.

-   **Vídeos**.  Se usa para que los usuarios de red almacenen y accedan a vídeos. Esta carpeta se crea al activar el uso compartido de multimedia.

-   **Usuarios**.  Se usa para que los usuarios de red almacenen y accedan a archivos. Se genera automáticamente una carpeta específica del usuario en la carpeta de servidor **Usuarios** para cada cuenta de usuario de red que se cree.

##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Para cambiar el acceso a una carpeta compartida de una cuenta de usuario

1.  Abra el panel de Windows Server Essentials.

2.  Haga clic en **ALMACENAMIENTO** y después, en **Carpetas del servidor**.

3.  Navegue a la carpeta del servidor para la que quiere modificar los permisos y selecciónela.

4.  En el panel de tareas, haga clic en **Ver las propiedades de la carpeta**.

5.  En **<\> propiedades de nombreDeCarpeta**, haga clic en **compartir** y seleccione el nivel de acceso de usuario adecuado para las cuentas de usuario de la lista y, a continuación, haga clic en **aplicar**.

    > [!NOTE]
    >  No puede modificar los permisos de uso compartido de las carpetas del servidor **Copias de seguridad del Historial de archivos**, **Redirección de carpetas** y **Usuarios**. Por tanto, las propiedades de carpeta de estas carpetas del servidor no incluyen la pestaña **Uso compartido**.

###  <a name="allow-user-accounts-to-establish-a-remote-desktop-session-to-their-computer"></a><a name="BKMK_Access13"></a> Permitir que las cuentas de usuario establezcan una sesión de escritorio remoto en su equipo
  Esta sección se aplica a un servidor que ejecuta Windows Server Essentials o Windows Server Essentials, o a un servidor que ejecuta Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol de experiencia con Windows Server Essentials instalado.

 El administrador de red puede conceder permisos a los usuarios de red que les permitan acceder a sus equipos de red desde una ubicación remota.

##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Para permitir a los usuarios acceder a sus equipos de red desde una ubicación remota

1.  Abra el panel de Windows Server Essentials.

2.  En la barra de navegación, haga clic en **USUARIOS**.

3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario a la que quiere conceder permisos de acceso remoto al escritorio.

4.  En el panel **\> tareas** de la cuenta de usuario de<, haga clic en **propiedades**.

5.  En la **<\> propiedades** de la cuenta de usuario, haga clic en la pestaña **acceso al equipo** .

6.  Seleccione los equipos a los que quiere que acceda esta cuenta de usuario de forma remota y después haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar cuentas en línea para usuarios](Manage-Online-Accounts-for-Users.md)

-   [Conéctese](../use/Get-Connected-in-Windows-Server-Essentials.md)

-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
