---
title: Administrar las cuentas de usuario de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91175836e4453860b17d2655e6a5a831645de410
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Administrar las cuentas de usuario de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

La página de usuarios del escritorio de Windows Server Essentials centraliza información y tareas que te ayudarán a administran las cuentas de usuario de la red de pequeñas empresas. Para obtener información general del panel a los usuarios, consulta [información general del panel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
  
##  <a name="BKMK_ManageAccounts"></a>Administración de cuentas de usuario  
 Los siguientes temas proporcionan información sobre cómo usar el escritorio de Windows Server Essentials para administrar las cuentas de usuario en el servidor:  
  
-   [Agregar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [Quitar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [Cuentas de usuario de vista](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [Cambiar el nombre para mostrar de la cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [Activar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [Desactivar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [Comprender las cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [Administrar cuentas de usuario mediante el panel](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>Agregar una cuenta de usuario  
 Cuando agregas una cuenta de usuario, el usuario asignado puede iniciar sesión en la red y proporcionarle al usuario permiso para acceder a los recursos de red, como las carpetas compartidas y el sitio de acceso Web remoto. Windows Server Essentials incluye el Asistente de cuenta de usuario que te ayuda a agregar:  
  
-   Proporcionar un nombre y contraseña de la cuenta de usuario.  
  
-   Definir la cuenta como administrador o como un usuario estándar.  
  
-   Selecciona qué puede acceder la cuenta de usuario de carpetas compartidas.  
  
-   Especifica si la cuenta de usuario tiene acceso remoto a la red.  
  
-   Selecciona las opciones de correo electrónico si corresponde.  
  
-   Asignar una cuenta de Microsoft Online Services (denominada una cuenta de Office 365 en Windows Server Essentials) si corresponde.  
  
-   Asignar a grupos de usuarios (solo Windows Server Essentials).  
  
> [!NOTE]
>  -   No se admiten caracteres distintos de ASCII en Microsoft Azure Active Directory (AD Azure). No utilice los caracteres que no sean ASCII en tu contraseña, si el servidor está integrado en Azure AD.  
> -   Las opciones de correo electrónico solo están disponibles si instalas un complemento que proporciona el servicio de correo electrónico.  
  
##### <a name="to-add-a-user-account"></a>Para agregar una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la **tareas de los usuarios** panel, haz clic en **agregar una cuenta de usuario**. La adición de que aparece un asistente de cuenta de usuario.  
  
4.  Sigue las instrucciones para completar al asistente.  
  
###  <a name="BKMK_Remove"></a>Quitar una cuenta de usuario  
 Si decides quitar una cuenta de usuario desde el servidor, un asistente elimina la cuenta seleccionada. Por este motivo, ya no se puede usar la cuenta para iniciar sesión en la red o tener acceso a cualquiera de los recursos de red. Como una opción, también puedes eliminar los archivos de la cuenta de usuario al mismo tiempo que quitar la cuenta. Si no quieres quitar la cuenta de usuario de forma permanente, puede desactivar la cuenta de usuario en su lugar para suspender el acceso a recursos de red.  
  
> [!IMPORTANT]
>  Si una cuenta de usuario tiene un Microsoft online cuenta asignado, al quitar la cuenta de usuario, también, se quita la cuenta en línea de Microsoft Online Services y los datos de s de usuario, incluido el correo electrónico, están sujeto a los datos de las directivas de retención de Microsoft Online Services. Si quieres conservar los datos de usuario de la cuenta en línea, desactivar la cuenta de usuario en lugar de quitarla. Para obtener más información, consulta [administrar cuentas en línea para los usuarios](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account"></a>Para quitar una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieras quitar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **quitar la cuenta de usuario**. Aparecerá un asistente de cuenta de usuario de la eliminación.  
  
5.  En la **¿desea mantener los archivos? ** página del asistente, puedes elegir eliminar los archivos de usuario s, incluidas las copias de seguridad del historial de archivos y la carpeta redirigida para la cuenta de usuario. Para mantener al usuario archivos s, dejar vacía la casilla de verificación. Después de realizar la selección, haz clic en **siguiente**.  
  
6.  Haz clic en **eliminar cuenta**.  
  
> [!NOTE]
>  Después de quitar una cuenta de usuario, la cuenta ya no aparece en la lista de cuentas de usuario. Si eliges eliminar los archivos, el servidor elimina permanentemente la carpeta de usuario s desde el **usuarios** carpeta del servidor y desde el **copias de seguridad de historial de archivos** carpeta del servidor.  
>   
>  Si tienes un proveedor de correo electrónico integrada, también se quitará la cuenta de correo electrónico asignada a la cuenta de usuario.  
  
###  <a name="BKMK_Manage3"></a>Cuentas de usuario de vista  
 La **usuarios** sección del panel de Essentials de servidor de Windows muestra una lista de cuentas de usuario de la red. La lista también proporciona información adicional sobre cada cuenta.  
  
##### <a name="to-view-a-list-of-user-accounts"></a>Para ver una lista de cuentas de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  El panel muestra una lista de cuentas de usuario actual.  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>Para ver o cambiar las propiedades de una cuenta de usuario  
  
1.  En la lista de cuentas de usuario, selecciona la cuenta que quieres ver o cambiar las propiedades.  
  
2.  En la **< usuario Account\ > tareas** panel, haz clic en **ver las propiedades de la cuenta**. La **propiedades** para aparece de la cuenta de usuario de la página.  
  
3.  Haz clic en una pestaña para mostrar las propiedades para esa característica de cuenta.  
  
4.  Para guardar los cambios que realices en las propiedades de la cuenta de usuario, haz clic en **aplicar**.  
  
###  <a name="BKMK_Manage4"></a>Cambiar el nombre para mostrar de la cuenta de usuario  
 El nombre para mostrar es el nombre que aparece en la **nombre** columna en la **usuarios** página del panel. Cambiar el nombre para mostrar no se cambia el nombre de inicio de sesión o inicio de sesión para una cuenta de usuario.  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>Para cambiar el nombre para mostrar de una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieres cambiar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **ver las propiedades de la cuenta**. La **propiedades** para aparece de la cuenta de usuario de la página.  
  
5.  En la **General** pestaña, escribe un nuevo **nombre** y **apellido** para la cuenta de usuario y, a continuación, haz clic en **Aceptar**.  
  
     El nombre para mostrar aparece en la lista de cuentas de usuario.  
  
###  <a name="BKMK_Manage5"></a>Activar una cuenta de usuario  
 Cuando activas una cuenta de usuario, el usuario asignado puede iniciar una sesión a los recursos de red de red y acceso a los que la cuenta tiene permiso, como las carpetas compartidas y el sitio de acceso Web remoto.  
  
> [!NOTE]
>  Solo se puede activar una cuenta de usuario que se desactiva. No se puede activar una cuenta de usuario después de quitar el servidor.  
  
##### <a name="to-activate-a-user-account"></a>Para activar una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la vista de lista, selecciona la cuenta de usuario que quieras activar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **activar la cuenta de usuario**.  
  
5.  En la ventana de confirmación, haz clic en **Sí** para confirmar la acción.  
  
> [!NOTE]
>  Después de activar una cuenta de usuario, muestra el estado de la cuenta **Active**. La cuenta de usuario lo recupera los mismos derechos de acceso que se asignaron antes de la desactivación de la cuenta.  
>   
>  Si tienes un proveedor de correo electrónico integrada, también se activará la cuenta de correo electrónico asignada a la cuenta de usuario.  
  
###  <a name="BKMK_Manage6"></a>Desactivar una cuenta de usuario  
 Al desactivar una cuenta de usuario, acceso a la cuenta en el servidor está suspendido temporalmente. Por este motivo, el usuario asignado no puede usar la cuenta para acceder a recursos de red como las carpetas compartidas o el sitio de acceso Web remoto hasta que activar la cuenta.  
  
 Si la cuenta de usuario tiene una cuenta de Microsoft en línea asignada, también se desactiva la cuenta en línea. El usuario no puede usar recursos de Office 365 y otros servicios en línea que se suscribe a, pero se conservan los datos de s de usuario, incluido el correo electrónico, en Microsoft Online Services.  
  
> [!NOTE]
>  Sólo se puede desactivar una cuenta de usuario que está activo.  
  
##### <a name="to-deactivate-a-user-account"></a>Para desactivar una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la vista de lista, selecciona la cuenta de usuario que desee desactivar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **desactivar la cuenta de usuario**.  
  
5.  En la ventana de confirmación, haz clic en **Sí** para confirmar la acción.  
  
> [!NOTE]
>  Después de desactivar una cuenta de usuario, muestra el estado de la cuenta **inactivo**.  
>   
>  Si tienes un proveedor de correo electrónico integrada, también se desactivan la cuenta de correo electrónico asignada a la cuenta de usuario.  
  
###  <a name="BKMK_Manage7"></a>Comprender las cuentas de usuario  
 Una cuenta de usuario proporciona información importante para Windows Server Essentials, que permite a los usuarios acceder a la información que se almacena en el servidor y lo hace posible que los usuarios individuales crear y administrar sus archivos y configuraciones. Si tienen una cuenta de usuario de Windows Server Essentials y que tienen permiso para acceder a un equipo, los usuarios pueden iniciar sesión en cualquier equipo de la red. Los usuarios acceder a sus cuentas de usuario con su nombre de usuario y contraseña.  
  
 Hay dos tipos principales de cuentas de usuario. Cada tipo proporciona al usuario un nivel diferente de control sobre el equipo:  
  
-   **Estándar** cuentas se usan para tareas habituales. La cuenta estándar ayuda a proteger la red al impedir que los usuarios realicen cambios que afecten a otros usuarios, como eliminar archivos o cambiar la configuración de red.  
  
-   **Administrador** cuentas proporcionan el mayor control sobre una red del equipo. Debes asignar el administrador solo cuando sea necesario de tipo de cuenta.  
  
###  <a name="BKMK_Manage8"></a>Administrar cuentas de usuario mediante el panel  
 Windows Server Essentials hace posible realizar tareas de administración comunes mediante el panel de Windows Server Essentials. De manera predeterminada, la **usuarios** página del panel incluye dos pestañas **usuarios** y **grupos de usuarios**.  
  
> [!NOTE]
>  -   Si integrar el servidor que ejecuta Windows Server Essentials con Office 365, llama a una nueva pestaña **grupos de distribución** también se agrega en la **usuarios** página del panel.  
> -   En Windows Server Essentials, la **usuarios** página del panel incluye una sola pestaña - **usuarios**.  
  
 La **usuarios** ficha incluye lo siguiente:  
  
-   Una lista de cuentas de usuario, que muestra:  
  
    -   El nombre del usuario.  
  
    -   El nombre de inicio de sesión para la cuenta de usuario.  
  
    -   Si la cuenta de usuario tiene permiso de acceso desde cualquier lugar. En cualquier lugar permiso de acceso para una cuenta de usuario sea **permitido** o **no permite**.  
  
    -   Si el historial de archivos para esta cuenta de usuario está administrado por el servidor que ejecuta Windows Server Essentials. El estado del historial de archivos para una cuenta de usuario es **administrado** o **no administrado**.  
  
    -   El nivel de acceso que se asigna a la cuenta de usuario. Puedes asignar cualquiera **usuario estándar** acceso o **administrador** acceso para una cuenta de usuario.  
  
    -   El estado de la cuenta de usuario. Puede ser una cuenta de usuario **Active**, **inactivo**, o **incompleto**.  
  
    -   En Windows Server Essentials, si el servidor está integrado con Office 365 o Windows Intune, se muestra la cuenta de Microsoft en línea.  
  
    -   En Windows Server Essentials, si el servidor se integra con Microsoft Office 365, se muestra el estado de la cuenta de Office 365 (conocida en Windows Server Essentials como la cuenta de Microsoft en línea) para la cuenta de usuario.  
  
-   Un panel de detalles con información adicional sobre una cuenta de usuario seleccionado.  
  
-   Un panel de tareas que incluye:  
  
    -   Un conjunto de tareas administrativas de cuenta de usuario, como la visualización y eliminación de cuentas de usuario y cambiar las contraseñas.  
  
    -   Tareas que te permiten establecer o cambiar la configuración de todas las cuentas de usuario de la red globalmente.  
  
 La siguiente tabla describen las diferentes tareas de cuenta de usuario que están disponibles desde el **usuarios** pestaña. Algunas de las tareas son específicas de las cuentas de usuario, y solo están visibles cuando se selecciona una cuenta de usuario en la lista.  
  
> [!NOTE]
>  Si Office 365 se integra con Windows Server Essentials, tareas adicionales estarán disponibles. Para obtener más información, consulta [administrar cuentas en línea para los usuarios](Manage-Online-Accounts-for-Users.md).  
  
### <a name="user-account-tasks-in-the-dashboard"></a>Tareas de la cuenta de usuario en el panel  
  
|Nombre de la tarea|Descripción|  
|---------------|-----------------|  
|Ver las propiedades de la cuenta|Te permite ver y cambiar las propiedades de la cuenta de usuario seleccionado y para especificar los permisos de acceso de carpeta para la cuenta.|  
|Desactivar la cuenta de usuario|Una cuenta de usuario que se desactiva no puede iniciar sesión en la red o acceder a recursos de red como carpetas o impresoras compartidas.|  
|Activar la cuenta de usuario|Una cuenta de usuario que se activa puede iniciar sesión en la red y puede tener acceso a recursos de red como se define en los permisos de cuenta.|  
|Quitar la cuenta de usuario|Permite quitar la cuenta de usuario seleccionado.|  
|Cambiar la contraseña de cuenta de usuario|Te permite restablecer la contraseña de red para la cuenta de usuario seleccionado.|  
|Agregar una cuenta de usuario|Inicia el agregar un asistente de cuenta de usuario, que te permite crear una nueva cuenta de usuario único que tiene acceso de usuario estándar o de acceso de administrador.|  
|Asignar una cuenta de Microsoft en línea|Agrega una cuenta de Microsoft en línea a la cuenta de usuario de la red local que está seleccionada.<br /><br /> Esta tarea se muestra cuando el servidor se integra con Microsoft online services, como Office 365.|  
|Agregar cuentas de Microsoft en línea|Agrega cuentas en línea de Microsoft y los asocia a cuentas de usuario de la red local.<br /><br /> Esta tarea se muestra cuando el servidor se integra con Microsoft online services, como Office 365.|  
|Establece la directiva de contraseñas|Permite cambiar los valores de la contraseña de directivas de la red.|  
|Importar cuentas en línea de Microsoft|Realiza una importación masiva de cuentas desde servicios en línea de Microsoft en la red local.<br /><br /> Esta tarea se muestra cuando el servidor se integra con Microsoft online services, como Office 365.|  
|Actualizar|Actualiza la pestaña de los usuarios.<br /><br /> Esta tarea es aplicable a Windows Server Essentials.|  
|Cambiar la configuración del historial de archivos|Te permite cambiar la configuración del historial de archivos, como la frecuencia de copia de seguridad o la duración de copia de seguridad.<br /><br /> Esta tarea es aplicable a Windows Server Essentials.|  
|Exportar todas las conexiones remotas|Crea una. Archivo de formato CSV de todas las conexiones remotas en el servidor que se hayan producido en los últimos 30 días.|  
  
##  <a name="BKMK_ManageAccess"></a>Acceso y administración de contraseñas  
 Los siguientes temas proporcionan información sobre cómo usar el escritorio de Windows Server Essentials para administrar las contraseñas de cuentas de usuario y el acceso de usuario a las carpetas compartidas en el servidor:  
  
-   [Cambiar o restablecer la contraseña de una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [Lo que debes saber acerca de las directivas de contraseña](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [Cambiar la directiva de contraseñas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [Nivel de acceso a las carpetas compartidas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [Conservar y administrar el acceso a archivos para cuentas de usuario quitado](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [Sincronizar la contraseña DSRM con la contraseña de administrador de red](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [Dar permiso de escritorio remoto de cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [Permitir que los usuarios acceder a recursos en el servidor](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [Cambiar los permisos de acceso remoto para una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [Cambiar los permisos de red privada virtual para una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [Cambiar el acceso a las carpetas compartidas internas para una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [Permitir que las cuentas de usuario establecer una sesión de escritorio remoto en el equipo](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>Cambiar o restablecer la contraseña de una cuenta de usuario  
 Para cambiar o restablecer la contraseña de una cuenta de usuario, sigue estos pasos.  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>Para restablecer la contraseña de una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que desea restablecer.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **cambiar la contraseña de cuenta de usuario**. Aparece el Asistente para cambiar de contraseña de cuenta de usuario.  
  
5.  Escribe una contraseña nueva para la cuenta de usuario y, a continuación, escribe la contraseña otra vez para confirmarla.  
  
6.  Haz clic en **cambiar contraseña**.  
  
7.  Proporcionar la nueva contraseña al usuario.  
  
    > [!IMPORTANT]
    >  -   Es podrán que no puedas cambiar la contraseña si la directiva de contraseña de tu cuenta se ha establecido en **las contraseñas nunca expiren**.  
    > -   No se admiten caracteres distintos de ASCII en Azure AD. Por lo tanto, si el servidor está integrado en Azure AD, no uses los caracteres que no sean ASCII en tu contraseña.  
    > -   Si una cuenta de Microsoft en línea (conocida en Windows Server Essentials como una cuenta de Office 365) se asigna al usuario, la contraseña se sincroniza con la contraseña de cuenta en línea. El usuario usará la nueva contraseña para iniciar sesión en el servidor o iniciar sesión en Office 365. Para obtener más información, consulta [administrar cuentas en línea para los usuarios](Manage-Online-Accounts-for-Users.md).  
  
###  <a name="BKMK_Access3"></a>Lo que debes saber acerca de las directivas de contraseña  
 La directiva de contraseña es un conjunto de reglas que definen cómo los usuarios crear y usan las contraseñas. La directiva ayuda a impedir el acceso no autorizado a datos de usuario y otra información que se almacena en el servidor. La directiva de contraseña se aplica a todas las cuentas de usuario que acceda a la red.  
  
 La directiva de contraseñas de Windows Server Essentials consta de tres elementos principales de la siguiente manera:  
  
-   **Longitud de contraseña**.  Es más de una contraseña, más seguro. Las contraseñas en blanco no son seguras.  
  
-   **Complejidad de contraseña**.  Contraseñas complejas contienen una combinación de letras mayúsculas y minúsculas (a – z, a la Z), base (0-9) de números y símbolos no alfabéticos (como;!, @, #, _,-). Las contraseñas complejas son mucho menos susceptibles de sufrir un acceso no autorizado. Las contraseñas que contienen los nombres de usuario, el cumpleaños u otra información personal no proporcionan el nivel de seguridad adecuado.  
  
-   **Vigencia de la contraseña**.  Windows Server Essentials requiere que los usuarios cambiar su contraseña al menos una vez cada 180 días. Como una opción, puede que las contraseñas nunca expiran.  
  
 Para que sea más fácil de implementar una directiva de contraseñas en la red del equipo, Windows Server Essentials proporciona una herramienta simple que permite establecer o cambiar la directiva de contraseña a cualquiera de los siguientes cuatro perfiles de directiva predefinidas:  
  
-   **Débiles**.  Los usuarios pueden especificar que cualquier contraseña que no está en blanco.  
  
-   **Mediano**.  Estas contraseñas deben contener al menos 5 caracteres. No se requiere una contraseña compleja.  
  
-   **Mediano seguro**.  Estas contraseñas deben contener al menos 5 caracteres y deben incluir letras, números y símbolos.  
  
-   **Seguro**.  Estas contraseñas deben contener al menos de 7 caracteres y deben incluir letras, números y símbolos. Estas contraseñas son más seguras, pero pueden ser más difíciles de recordar para los usuarios.  
  
    > [!NOTE]
    >  Las contraseñas no pueden contener la dirección de correo electrónico o nombre de usuario.  
    >   
    >  Si se integra con Office 365, aplica la integración del **seguro** directiva de contraseñas y actualiza la directiva para incluir los siguientes requisitos:  
    >   
    >  -   Las contraseñas deben contener 8 16 caracteres.  
    > -   Las contraseñas no pueden contener un espacio o el nombre de correo electrónico de Office 365.  
  
 De manera predeterminada, servidor de instalación establece la directiva de contraseñas predeterminada la **seguro** opción.  
  
###  <a name="BKMK_Access4"></a>Cambiar la directiva de contraseñas  
 Usa el siguiente procedimiento para establecer o cambiar la directiva de contraseñas en cualquiera de los cuatro perfiles de directiva predefinidas.  
  
##### <a name="to-change-the-password-policy"></a>Para cambiar la directiva de contraseñas  
  
1.  Abre el panel de Windows Server Essentials y, a continuación, haz clic en **usuarios**.  
  
2.  En la **tareas de los usuarios** panel, haz clic en **establecer la directiva de contraseña**.  
  
3.  En la **cambiar la directiva de contraseñas** de pantalla, Establece el nivel de seguridad de la contraseña, mueve el control deslizante.  
  
     Microsoft recomienda que establezca la intensidad de la contraseña **seguro**.  
  
    > [!NOTE]
    >  Como una opción, también puedes seleccionar **las contraseñas nunca expiren**. Esta configuración es menos segura y, por lo tanto, no se recomienda.  
  
4.  Haz clic en **cambiar directiva**.  
  
###  <a name="BKMK_Access5"></a>Nivel de acceso a las carpetas compartidas  
 Como procedimiento recomendado, debes asignar los permisos más restrictivos que permitan a los usuarios realizar las tareas necesarias.  
  
 Tiene tres opciones de configuración de acceso disponibles para las carpetas compartidas en el servidor:  
  
-   **Lectura y escritura**.  Elige esta opción si desea permitir que la cuenta de usuario permiso Crear, modificar y eliminar los archivos en la carpeta compartida.  
  
-   **Solo lectura**.  Elige esta opción si desea permitir que la cuenta de usuario permiso Leer solo los archivos en la carpeta compartida. Cuentas de usuario con acceso de solo lectura no se pueden crear, cambiar o eliminar los archivos en la carpeta compartida.  
  
-   **Sin acceso**.  Elige esta opción si no desea que la cuenta de usuario para acceder a los archivos en la carpeta compartida.  
  
###  <a name="BKMK_Access6"></a>Conservar y administrar el acceso a archivos para cuentas de usuario quitado  
 El Administrador de red puede quitar una cuenta de usuario y elegir mantener al usuario archivos s para uso futuro. En este escenario, la cuenta de usuario quitado ya no puede usarse para iniciar sesión en la red. Sin embargo, los archivos de este usuario se guardarán en una carpeta compartida, que puede compartirse con otro usuario.  
  
> [!IMPORTANT]
>  Ten en cuenta que, si quitas una cuenta de usuario que tiene asignada una cuenta en línea de Microsoft, también se quita la cuenta en línea y los datos de usuario, incluido el correo electrónico, están sujeto a las directivas de retención de datos de Microsoft Online Services. Para conservar los datos de usuario de la cuenta en línea, desactivar la cuenta de usuario en lugar de quitarla. Para obtener más información, consulta [administrar cuentas en línea para los usuarios](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-user-s-files"></a>Para quitar una cuenta de usuario pero conservar el acceso a los archivos de usuario s  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieras quitar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **quitar la cuenta de usuario**. Aparecerá un asistente de cuenta de usuario de la eliminación.  
  
5.  En la **¿desea mantener los archivos? ** página, debes asegurarte de que la **eliminar los archivos incluidos en las copias de seguridad del historial de archivos y la carpeta redirigida para esta cuenta de usuario** activa la casilla está desactivada y, a continuación, haz clic en **siguiente**.  
  
     Se abrirá una página de confirmación de advertencia que se va a eliminar la cuenta, pero al mantener los archivos.  
  
6.  Haz clic en **eliminar cuenta** para quitar la cuenta de usuario.  
  
 Después de quita la cuenta de usuario, el administrador puede darte acceso a otra cuenta de usuario a la carpeta compartida.  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Para dar una cuenta de usuario permiso para acceder a una carpeta compartida  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **almacenamiento**y, a continuación, haz clic en el **carpetas del servidor** pestaña.  
  
3.  En la lista de carpetas, selecciona el **usuarios** carpeta.  
  
4.  En la **tareas de los usuarios** panel, haz clic en **abre la carpeta**. El Explorador de Windows se abre y muestra el contenido de la **usuarios** carpeta.  
  
5.  Haz clic en la carpeta para la cuenta de usuario que quieras compartir y, a continuación, haz clic en **propiedades**.  
  
6.  En **< usuario Account\ > propiedades**, haz clic en el **compartir** pestaña y, a continuación, haz clic en **compartir**.  
  
7.  En la **uso compartido de archivos** ventana, escribe o selecciona el nombre de cuenta de usuario con los que desee compartir la carpeta y, a continuación, haz clic en **agregar**.  
  
8.  Elige el **nivel de permiso** que quieres tener y, a continuación, haz clic en la cuenta de usuario **compartir**.  
  
###  <a name="BKMK_Access7"></a>Sincronizar la contraseña DSRM con la contraseña de administrador de red  
 Modo de restauración de servicios de directorio (DSRM) es un modo de inicio especial para reparar o la recuperación de Active Directory. El sistema operativo usa DSRM para iniciar sesión en el equipo si se produce un error de Active Directory o necesita restaurar. Si la contraseña de administrador de red y la contraseña DSRM son diferentes, no se cargará DSRM.  
  
 Durante una instalación limpia, la primera vez de Windows Server Essentials, el sistema establece la contraseña DSRM a la contraseña de cuenta de administrador de red que especifiques durante la instalación o en el archivo de respuesta de la migración. Cuando cambias la contraseña de administrador de red (como se recomienda normalmente cada 60 días por motivos de seguridad del servidor mayor), el cambio de contraseña no se reenvía al DSRM. Esto da como resultado un error de coincidencia de contraseña. Si esto ocurre, puedes usar las siguientes soluciones para manualmente o sincronizar automáticamente la contraseña de s del Administrador de red con la contraseña DSRM.  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Sincronizar manualmente la contraseña DSRM una cuenta de administrador de red  
  
1.  En un símbolo del sistema, ejecuta `ntdsutil.exe` para abrir la herramienta ntdsutil.  
  
2.  Para restablecer la contraseña de DSRM, escriba **establecer dsrm contraseña**.  
  
3.  Para sincronizar la contraseña de DSRM en un controlador de dominio con la cuenta de s de administrador de red actual, escribe:  
  
     **sincronización de cuenta de dominio** *< current_network_administrator_account >*, y, a continuación, presione ENTRAR.  
  
 Debido a que periódicamente se cambia la contraseña de la cuenta de administrador de red, para garantizar que la contraseña DSRM es siempre el mismo que la contraseña actual del Administrador de red, te recomendamos que crees una tarea de programación para automáticamente sincronizar la contraseña DSRM a la contraseña de administrador de red diariamente.  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Para sincronizar automáticamente la contraseña DSRM una cuenta de administrador de red  
  
1.  Desde el servidor, abra **herramientas administrativas**y, a continuación, haz doble clic en **programador de tareas**.  
  
2.  En el programador de tareas **acciones** panel, haz clic en **crear tarea**.  
  
3.  En la **nombre** cuadro de texto, escriba un nombre para la tarea como **sincronización automática DSRM contraseña**y, a continuación, selecciona el **ejecutar con privilegios más altos** opción.  
  
4.  Define cuándo debe ejecutarse la tarea:  
  
    1.  En la **crear tarea** cuadro de diálogo, haz clic en el **desencadenadores** pestaña y, a continuación, haz clic en **nueva**.  
  
    2.  En la **nuevo desencadenador** cuadro de diálogo, selecciona la opción de periodicidad, especificar el intervalo de periodicidad y elige una hora de inicio.  
  
        > [!NOTE]
        >  Como procedimiento recomendado, debes establecer la tarea se ejecute en todos los días durante las horas de inactividad.  
  
    3.  Haz clic en **Aceptar** para guardar los cambios y volver a la **crear tarea** cuadro de diálogo.  
  
5.  Definir las acciones de la tarea:  
  
    1.  Haz clic en el **acciones** pestaña y, a continuación, haz clic en **nueva**. La **nueva acción** aparece el cuadro de diálogo.  
  
    2.  En la **acción** la lista, haz clic en **iniciar un programa**y, a continuación, busca **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.  
  
    3.  En la **agregar argumentos**texto (opcional), escriba lo siguiente (debe incluir las comillas): **establecer dsrm sincronización de contraseña de cuenta de dominio SBS_network_administrator_account q q** donde *SBS_network_administrator_account* es el nombre de cuenta de administrador s de red actual.  
  
6.  Haz clic en **Aceptar** dos veces para guardar la tarea y cerrar la **crear tarea** cuadro de diálogo. La nueva tarea aparece en la **tareas activas** sección de **planificación de la tarea**.  
  
###  <a name="BKMK_Access8"></a>Dar permiso de escritorio remoto de cuentas de usuario  
 En la instalación predeterminada de Windows Server Essentials, los usuarios de red no tienen permiso para establecer una conexión remota de equipos u otros recursos de la red.  
  
 Antes de que los usuarios de la red pueden establecer una conexión remota a recursos de red, debes configurar acceso desde cualquier lugar. Después de configurar acceso desde cualquier lugar, los usuarios pueden acceder a archivos, aplicaciones y equipos de la red de oficina de un dispositivo en cualquier lugar con una conexión a Internet.  
  
 Configurar el Asistente de acceso desde cualquier lugar permite habilitar los dos métodos de acceso remoto:  
  
-   Red privada virtual (VPN)  
  
-   Acceso Web remoto  
  
 Cuando se ejecuta el asistente, también puedes permitir el acceso desde cualquier lugar para todas las cuentas de usuario actual y recién agregado.  
  
 Para configurar el acceso desde cualquier lugar, abra el panel **Home** página, haz clic en **instalación**y, a continuación, haz clic en **configurar acceso desde cualquier lugar**.  
  
 Para obtener más información sobre el acceso desde cualquier lugar, consulta [administrar acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_Access9"></a>Permitir que los usuarios acceder a recursos en el servidor  
  En esta sección se aplica a un servidor que ejecuta Windows Server Essentials o Windows Server Essentials, o a un servidor que ejecuta Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol de Windows Server Essentials Experience instalado.  
  
 Si quieres que los usuarios a usar acceso remoto o tener cuentas de usuario individual, cuando termines de conectar un equipo con el servidor, puedes crear nueva red cuentas de usuario para los usuarios del equipo en red en el servidor mediante el panel. Para obtener más información sobre cómo crear una cuenta de usuario, consulta [agregar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Después de crear las cuentas de usuario, debes proporcionar la información de nombre y la contraseña de usuario de red a los usuarios del equipo cliente para que pueden acceder a recursos en el servidor mediante el uso del Launchpad.  
  
 Para cada cuenta de usuario que crees puede establecer propiedades de la cuenta de acceso para lo siguiente a través del usuario:  
  
-   **Las carpetas compartidas**.  De manera predeterminada, los administradores de red tienen **lectura y escritura** permiso a todas las carpetas compartidas y cuentas de usuario estándar tiene **de sólo lectura** permisos a la carpeta de la empresa. Si se habilita la transmisión por secuencias de multimedia, puedes asignar permisos de acceso de carpetas para cuentas de usuario estándar individuales para carpetas comparten de los siguientes: **música**, **imágenes**, **TV grabada**, y **vídeos**. Puedes establecer permisos para las cuentas de usuario acceder a las carpetas compartidas en el **las carpetas compartidas** ficha de las propiedades de la cuenta de usuario.  
  
-   **Acceso desde cualquier lugar**.  De manera predeterminada, los administradores de red pueden usar VPN o acceso Web remoto a los recursos del servidor de acceso. Para las cuentas de usuario estándar, debe establecer permisos de la cuenta de usuario en el **acceso desde cualquier lugar** pestaña.  
  
-   **Acceso al equipo**.  De manera predeterminada, los administradores de red pueden acceder a todos los equipos de la red. Sin embargo, para las cuentas de usuario estándar puede establecer permisos de la cuenta de usuario individual para acceder a los equipos de la red en la **acceso al equipo** ficha de las propiedades de la cuenta de usuario.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Para editar las propiedades de la cuenta de usuario en Windows Server Essentials 2012 R2  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieres editar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **ver las propiedades de la cuenta**.  
  
5.  En la **< usuario Account\ > propiedades**, haz lo siguiente:  
  
    1.  En la **las carpetas compartidas** pestaña, establecer los permisos adecuados para cada carpeta compartida según sea necesario.  
  
    2.  En la **acceso desde cualquier lugar** pestaña:  
  
        1.  Para permitir que un usuario que se conecte al servidor mediante VPN, selecciona el **permitir red privada Virtual (VPN)** casilla de verificación.  
  
        2.  Para permitir que un usuario que se conecte al servidor mediante el uso de acceso Web remoto, selecciona el **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web** casilla de verificación.  
  
    3.  En la **acceso al equipo** pestaña, selecciona los equipos de red que quieres que el usuario tenga acceso a.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Para editar las propiedades de la cuenta de usuario en Windows Server Essentials 2012  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieres editar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **propiedades**.  
  
5.  En la **< usuario Account\ > propiedades**, haz lo siguiente:  
  
    1.  En la **General** , selecciona **usuario puede ver alertas de estado de red** si la cuenta de usuario necesita acceder a informes de estado de red.  
  
    2.  En la **las carpetas compartidas** pestaña, establecer los permisos adecuados para cada carpeta compartida según sea necesario.  
  
    3.  En la **acceso desde cualquier lugar** pestaña:  
  
        1.  Para permitir que un usuario que se conecte al servidor mediante VPN, selecciona el **permitir red privada Virtual (VPN)** casilla de verificación.  
  
        2.  Para permitir que un usuario que se conecte al servidor mediante el uso de acceso Web remoto, selecciona el **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web** casilla de verificación.  
  
    4.  En la **acceso al equipo** pestaña, selecciona los equipos de red que quieres que el usuario tenga acceso a.  
  
###  <a name="BKMK_Access10"></a>Cambiar los permisos de acceso remoto para una cuenta de usuario  
 Un usuario puede acceder a recursos que se encuentra en el servidor desde una ubicación remota mediante una red privada virtual (VPN), de acceso Web remoto o de otras aplicaciones de servicios web. De manera predeterminada, los permisos de acceso remoto están activados para los usuarios de red cuando configure acceso desde cualquier lugar en Windows Server Essentials mediante el panel.  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Para cambiar los permisos de acceso remoto para una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieres cambiar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **ver las propiedades de la cuenta**. La **propiedades** para aparece de la cuenta de usuario de la página.  
  
5.  En la **acceso desde cualquier lugar** pestaña, haz lo siguiente:  
  
    -   Selecciona el **permitir red privada Virtual (VPN)** casilla de verificación para permitir que un usuario conecta al servidor mediante la VPN.  
  
    -   Selecciona el **permitir el acceso Web remoto y el acceso a aplicaciones de servicios web** casilla de verificación para permitir que un usuario conecta al servidor mediante el acceso Web remoto.  
  
6.  Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
###  <a name="BKMK_Access11"></a>Cambiar los permisos de red privada virtual para una cuenta de usuario  
 Puedes usar una red privada virtual (VPN) para conectarse a Windows Server Essentials y obtener acceso a todos los recursos que se almacenan en el servidor. Esto es especialmente útil si tienes un equipo cliente que está configurado con cuentas de red que pueden usarse para conectarse a un servidor de Windows Server Essentials hospedado a través de una conexión VPN. Todas las cuentas de usuario recién creado en el servidor de Windows Server Essentials hospedado deben usar VPN para iniciar sesión en el equipo cliente por primera vez.  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>Para cambiar los permisos de VPN para los usuarios de red  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario al que quieres conceder permisos para acceder al escritorio de forma remota.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **propiedades**.  
  
5.  En la **< usuario Account\ > propiedades**, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
6.  En la **acceso desde cualquier lugar** ficha, para permitir que un usuario que se conecte al servidor mediante VPN, selecciona el **permitir red privada Virtual (VPN)** casilla de verificación.  
  
7.  Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
###  <a name="BKMK_Access12"></a>Cambiar el acceso a las carpetas compartidas internas para una cuenta de usuario  
 Puedes administrar el acceso a las carpetas compartidas en el servidor mediante el uso de las tareas en la **carpetas del servidor** ficha del panel. De manera predeterminada, las siguientes carpetas de servidor se crean al instalar Windows Server Essentials:  
  
-   **Las copias de seguridad del equipo de cliente**.  Se usa para almacenar las copias de seguridad de un equipo cliente creados por copia de seguridad de Windows Server. No se comparte esta carpeta del servidor.  
  
-   **Empresa**.  Se usa para almacenar y acceder a los documentos relacionados a la organización los usuarios de red.  
  
-   **Las copias de seguridad del historial de archivos**.  De forma predeterminada, Windows Server Essentials almacena copias de seguridad de archivos creados mediante el historial de archivos. No se comparte esta carpeta del servidor.  
  
-   **Redirección de carpetas**.  Se usa para almacenar y acceder a carpetas configuradas para la redirección de carpetas de los usuarios de red. No se comparte esta carpeta del servidor.  
  
-   **Música**.  Se usa para almacenar y acceder a archivos de música con usuarios de la red. Esta carpeta se crea al activar el uso compartido de multimedia.  
  
-   **Imágenes**.  Usada para almacenar y acceder a las imágenes que los usuarios de red. Esta carpeta se crea al activar el uso compartido de multimedia.  
  
-   **Programas de TV grabados**.  Se usa para almacenar y acceder a los programas de TV grabados los usuarios de red. Esta carpeta se crea al activar el uso compartido de multimedia.  
  
-   **Vídeos**.  Se usa para almacenar y acceder a los vídeos por los usuarios de red. Esta carpeta se crea al activar el uso compartido de multimedia.  
  
-   **Los usuarios**.  Usada para almacenar y acceder a los archivos que los usuarios de red. Una carpeta específica del usuario se genera automáticamente en el **usuarios** carpeta del servidor para cada cuenta de usuario de la red que crees.  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Para cambiar el acceso a una carpeta compartida para una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  Haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  Navega hasta y selecciona la carpeta del servidor para el que quieres modificar permisos.  
  
4.  En el panel de tareas, haz clic en **ver las propiedades de carpeta**.  
  
5.  En **< FolderName\ > propiedades**, haz clic en **compartir**, selecciona el nivel de acceso de usuario adecuados para las cuentas de usuario de la lista y, a continuación, haz clic en **aplicar**.  
  
    > [!NOTE]
    >  No se pueden modificar los permisos de uso compartidos de **copias de seguridad de historial de archivos**, **redirección de carpetas**, y **usuarios** carpetas del servidor. Por lo tanto, las propiedades de carpeta de estas carpetas del servidor no incluyen un **compartir** pestaña.  
  
###  <a name="BKMK_Access13"></a>Permitir que las cuentas de usuario establecer una sesión de escritorio remoto en el equipo  
  En esta sección se aplica a un servidor que ejecuta Windows Server Essentials o Windows Server Essentials, o a un servidor que ejecuta Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol de Windows Server Essentials Experience instalado.  
  
 El Administrador de red puede conceder permisos a los usuarios de red que puedan acceder a sus equipos de red desde una ubicación remota.  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Para permitir que los usuarios accedan a sus equipos de red desde una ubicación remota  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que desea conceder permisos para obtener acceso al escritorio de forma remota.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **propiedades**.  
  
5.  En la **< usuario Account\ > propiedades**, haz clic en el **acceso al equipo** pestaña.  
  
6.  Selecciona los equipos que quieres que esta cuenta de usuario para poder acceder de forma remota y, a continuación, haz clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar cuentas en línea para los usuarios](Manage-Online-Accounts-for-Users.md)  
  
-   [Mantente conectado](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
