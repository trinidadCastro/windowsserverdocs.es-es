---
title: Instalación o desinstalación de roles, servicios de rol o características
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32d356b3ae70b7b15f23a40247e73b4b8f61c3db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822376"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Instalación o desinstalación de roles, servicios de rol o características

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, la consola de administrador del servidor y los cmdlets de Windows PowerShell para el administrador del servidor permitir la instalación de roles y características en servidores locales o remotos, o discos duros virtuales (VHD) sin conexión. Puede instalar varios roles y características en un servidor remoto único o VHD sin conexión en una sola agregar Roles y características Asistente o sesión de Windows PowerShell.  
  
> [!IMPORTANT]  
> El Administrador de servidores no se puede usar para administrar una versión más reciente del sistema operativo Windows Server. Administrador del servidor que se ejecutan en Windows Server 2012 R2 o Windows 8.1 no se puede usar para instalar roles, servicios de rol y características en servidores que ejecutan Windows Server 2016.  
  
Debe ser iniciado sesión en un servidor como administrador para instalar o desinstalar roles, servicios de rol y características. Si ha iniciado sesión en el equipo local con una cuenta que no tiene derechos de administrador en el servidor de destino, haga clic con el botón secundario en el servidor de destino, en el icono **Servidores** y haga clic en **Administrar como** para proporcionar una cuenta con derechos de administrador. El servidor en el que desea montar un VHD sin conexión debe agregarse al Administrador del servidor y el usuario debe tener derechos de administrador en ese servidor.  
  
Para obtener más información acerca de cuáles son los roles, servicios de rol y características, vea [Roles, servicios de rol y características](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
En este tema se incluyen las siguientes secciones.  
  
-   [Instalar roles, servicios de rol y características mediante el agregar Roles y características Asistente](#BKMK_installarfw)  
  
-   [Instalación de roles, servicios de rol y características mediante los cmdlets de Windows PowerShell](#BKMK_installwps)  
  
-   [Quitar roles, servicios de rol y características mediante la quitar Roles y características de Asistente](#BKMK_removerrfw)  
  
-   [Eliminación de roles, servicios de rol y características mediante los cmdlets de Windows PowerShell](#BKMK_removewps)  
  
-   [Instalar roles y características en varios servidores ejecutando un script de Windows PowerShell](#BKMK_batch)  
  
-   [Instalación de .NET Framework 3.5 y otras características a petición](#BKMK_FoD)  
  
## <a name="BKMK_installarfw"></a>Instalar roles, servicios de rol y características mediante el agregar Roles y características Asistente  
En una única sesión en el Asistente de las características y agregar funciones, puede instalar roles, servicios de rol y características en el servidor local, un servidor remoto que se ha agregado al administrador del servidor o un VHD sin conexión. Para obtener más información sobre cómo agregar un servidor al administrador del servidor para administrar, consulte [agregar servidores al administrador del servidor](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Si está ejecutando el administrador del servidor en Windows Server 2016 o Windows 10, puede usar el Asistente de las características y agregar Roles para instalar roles y características únicamente en servidores y VHD sin conexión que ejecutan Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Para instalar roles y características mediante el agregar Roles y características Asistente  
  
1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.  
  
    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.  
  
    -   En el Windows **iniciar** pantalla, haga clic en el **administrador del servidor** icono.  
  
2.  En el **administrar** menú, haga clic en **agregar Roles y características**.  
  
3.  En la página **Antes de comenzar**, compruebe que el servidor de destino y el entorno de red estén preparados para el rol y la característica que desea instalar. Haz clic en **Siguiente**.  
  
4.  En la página **Seleccionar tipo de instalación** , seleccione **Instalación basada en características o en roles** para instalar todas las partes de los roles o las características en un solo servidor, o bien seleccione **Instalación de Servicios de Escritorio remoto** para instalar una infraestructura de escritorio basada en máquina virtual o una infraestructura de escritorio basada en sesión para Servicios de Escritorio remoto. La opción **Instalación de Servicios de Escritorio remoto** que distribuye las partes lógicas del rol de Servicios de Escritorio remoto en los diferentes servidores según la necesidad de los administradores. Haz clic en **Siguiente**.  
  
5.  En la página **Seleccionar servidor de destino**, seleccione un servidor del grupo de servidores o un VHD sin conexión. Para seleccionar un VHD sin conexión como servidor de destino, primero seleccione el servidor en el que montará el VHD y, a continuación, seleccione el archivo VHD. Para obtener información acerca de cómo agregar servidores al grupo de servidores, consulte [agregar servidores al administrador del servidor](add-servers-to-server-manager.md). Una vez seleccionado el servidor de destino, haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > Para instalar roles y características en VHD sin conexión, los VHD de destino deben cumplir los requisitos siguientes.  
    >   
    > -   Los VHD deben ejecutar la versión de Windows Server que coincida con la versión del administrador del servidor está ejecutando. Consulte la nota al principio de [instalar roles, servicios de rol y características mediante el agregar Roles y características Asistente](#BKMK_installarfw).  
    > -   Los VHD no deben tener más de una partición o volumen de sistema.  
    > -   La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
    >   
    >     -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
    >     -   **Control total** acceder en el **seguridad** tab, archivo o carpeta **propiedades** cuadro de diálogo.  
  
6.  Seleccione los roles, seleccione los servicios para el rol, si corresponde, y haga clic en **Siguiente** para seleccionar las características.  
  
    Cuando proceda, el agregar Roles y características Asistente le notificará automáticamente si se detectaron conflictos en el servidor de destino que puede impedir que los roles seleccionados o las características de instalación o funcionamiento normal. También se le solicitará que agregue los roles, servicios de rol o características que necesiten los roles o las características que ha seleccionado.  
  
    Además, si planea administrar el rol de forma remota, desde otro servidor o desde un equipo basado en cliente de Windows que está ejecutando las Herramientas de administración remota del servidor, puede optar por no instalar los complementos y las herramientas de administración para los roles en el servidor de destino. De forma predeterminada, en el agregar Roles y características Asistente, se seleccionan las herramientas de administración para la instalación.  
  
7.  En la página **Confirmar selecciones de instalación** , revise el rol, la característica y las selecciones de servidor. Si está listo para realizar la instalación, haga clic en **Instalar**.  
  
    También puede exportar sus selecciones a un archivo de configuración basado en XML que puede usar para instalaciones desatendidas con Windows PowerShell. Para exportar la configuración especificada en esta agregar Roles y características Asistente para la sesión, haga clic en **exportar opciones de configuración**y, a continuación, guarde el archivo XML en una ubicación adecuada.  
  
    El comando **Especifique una ruta de acceso de origen alternativa** en la página **Confirmar selecciones de instalación** permite especificar una ruta de acceso de origen para los archivos necesarios para instalar los roles y las características en el servidor seleccionado. En Windows Server 2012 y versiones posteriores de Windows Server, [características a petición](https://go.microsoft.com/fwlink/p/?LinkID=241573) permite reducir la cantidad de espacio en disco usado por el sistema operativo, eliminando los archivos de roles y características de los servidores que se administran exclusivamente de de forma remota. Si ha eliminado los archivos de rol y de características desde un servidor mediante el cmdlet `Uninstall-WindowsFeature -remove`, podrá volver a instalarlos más adelante al especificar una ruta de origen alternativa o un recurso compartido donde se almacenan los archivos de características y roles requeridos. El recurso compartido de archivo o ruta de acceso de origen debe conceder **lectura** permisos a la **todo el mundo** grupo (no recomendado por motivos de seguridad), o a la cuenta de equipo (*dominio* \\ *SERverNAME*$) del servidor de destino; conceder acceso a la cuenta de usuario no es suficiente. Para obtener más información sobre Características a petición, vea [Opciones de instalación de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    Puede especificar un archivo WIM como un origen de archivo de características alternativo cuando va a instalar roles, servicios de rol y características en un servidor físico y ejecución. La ruta de acceso de origen de un archivo WIM debe tener el formato siguiente, con **WIM** como prefijo y el índice en el que los archivos de características se encuentran como sufijo: **WIM:e:\sources\install.wim:4**. Sin embargo, no se puede usar un archivo WIM directamente como origen para la instalación de roles, servicios de rol y características en un VHD sin conexión; debe montar el VHD sin conexión y apuntar a su ruta de montaje para los archivos de origen, o debe apuntar a una carpeta que contiene una copia del contenido del archivo WIM.  
  
8.  Tras hacer clic en **instalar**, **progreso de la instalación** página muestra el progreso de la instalación, los resultados y mensajes como advertencias, errores o pasos de configuración posteriores a la instalación se requiere para los roles o características que instaló. En Windows Server 2012 y versiones posteriores de Windows Server, puede cerrar el agregar Roles y características Asistente mientras la instalación está en curso y ver los resultados de instalación u otros mensajes en el **notificaciones** área en la parte superior de la consola de administrador del servidor. Haga clic en el **notificaciones** icono de marca para ver más detalles sobre las instalaciones u otras tareas que se va a realizar en el administrador del servidor.  
  
## <a name="BKMK_installwps"></a>Instalar roles, servicios de rol y características mediante los cmdlets de Windows PowerShell  
Los cmdlets de implementación del administrador del servidor para la función de Windows PowerShell de forma similar a basado en GUI agregar Roles y características Asistente y quitar Roles y características Asistente, con una diferencia importante. En Windows PowerShell, a diferencia de en el agregar Roles y características Asistente, las herramientas de administración y complementos para un rol no se incluyen de forma predeterminada. Para incluir herramientas de administración como parte de una instalación de rol, agregue el parámetro `IncludeManagementTools` al cmdlet. Si está instalando roles y características en un servidor que se está ejecutando la opción de instalación Server Core de Windows Server 2012 o versiones posteriores, puede agregar las herramientas de administración de un rol a una instalación, pero no se puede instalar complementos y herramientas de administración basada en GUI en los servidores que ejecutan la opción de instalación Server Core de Windows Server. Solo línea de comandos y herramientas de administración de Windows PowerShell pueden instalarse en la opción de instalación Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Para instalar roles y características mediante el cmdlet Install-WindowsFeature  
  
1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    > [!NOTE]  
    > Si está instalando roles y características en un servidor remoto, no es necesario ejecutar Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En el Windows **iniciar** pantalla, haga clic en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **ejecutar como administrador**.  
  
2.  Escriba **Get-WindowsFeature** y, a continuación, presione **Entrar** para ver una lista de los roles y las características disponibles e instalados en el servidor local. Si el equipo local no es un servidor, o si desea obtener información acerca de un servidor remoto, ejecute **Get-WindowsFeature - computerName <***computer_name***>**, en el que  *computer_name* representa el nombre de un equipo remoto que ejecuta Windows Server 2016. Los resultados del cmdlet contienen los nombres de comando de roles y características que agregue al cmdlet en el paso 4.  
  
    > [!NOTE]  
    > En Windows PowerShell 3.0 y versiones posteriores de Windows PowerShell, no hay ninguna necesidad de importar el módulo de cmdlets del administrador del servidor en la sesión de Windows PowerShell antes de ejecutar los cmdlets que forman parte del módulo. El módulo se importa automáticamente la primera vez que se ejecuta un cmdlet que es parte del módulo. Además, los cmdlets de Windows PowerShell ni los nombres de características usados con los cmdlets distinguen mayúsculas de minúsculas.  
  
3.  tipo **Get-help Install-WindowsFeature**y, a continuación, presione **ENTRAR** para ver la sintaxis y los parámetros aceptados para el `Install-WindowsFeature` cmdlet.  
  
4.  Escriba lo siguiente y, a continuación, presione **ENTRAR**, donde *feature_name* representa el nombre de comando de un rol o característica que desea instalar (obtenido en el paso 2), y *computer_name* representa un equipo remoto en el que desea instalar roles y características. Separe los valores múltiples para *feature_name* con comas. El parámetro `Restart` reinicia el servidor de destino automáticamente si lo requiere la instalación del rol o la característica.  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    Para instalar roles y características en un VHD sin conexión, agregue tanto el parámetro `computerName` como el parámetro `VHD` . Si no agrega el parámetro `computerName`, el cmdlet da por hecho que el equipo local está montado para acceder al VHD. El parámetro `computerName` contiene el nombre del servidor en el que se montará el VHD y el parámetro `VHD` contiene la ruta de acceso al archivo VHD en el servidor especificado.  
  
    > [!NOTE]  
    > Debe agregar el `computerName` parámetro si está ejecutando el cmdlet desde un equipo que se está ejecutando un sistema operativo de cliente de Windows.  
    >   
    > Para instalar roles y características en VHD sin conexión, los VHD de destino deben cumplir los requisitos siguientes.  
    >   
    > -   Los VHD deben ejecutar la versión de Windows Server que coincida con la versión del administrador del servidor está ejecutando. Consulte la nota al principio de [instalar roles, servicios de rol y características mediante el agregar Roles y características Asistente](#BKMK_installarfw).  
    > -   Los VHD no deben tener más de una partición o volumen de sistema.  
    > -   La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
    >   
    >     -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
    >     -   **Control total** acceder en el **seguridad** tab, archivo o carpeta **propiedades** cuadro de diálogo.  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **Ejemplo:** El siguiente cmdlet instala el rol Servicios de dominio de Active Directory y la característica de administración de directivas de grupo en un servidor remoto, ContosoDC1. Se agregan los complementos y las herramientas de administración mediante el parámetro `IncludeManagementTools`, y el servidor de destino se reinicia automáticamente si así lo requiere la instalación.  
  
    ```  
    Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  Cuando finalice la instalación, comprobar la instalación, abra el **todos los servidores** página Administrador del servidor, seleccione un servidor en el que haya instalado roles y características y vea el **Roles y características** icono en la página del servidor seleccionado. También puede ejecutar el `Get-WindowsFeature` cmdlet destinado al servidor seleccionado (Get-WindowsFeature - computerName <*computer_name*>) para ver una lista de roles y características que están instaladas en el servidor.  
  
## <a name="BKMK_removerrfw"></a>quitar roles, servicios de rol y características mediante la quitar Roles y características de Asistente  
Debe ser iniciado sesión en un servidor como administrador para desinstalar los roles, servicios de rol y características. Si ha iniciado sesión en el equipo local con una cuenta que no tiene derechos de administrador en el servidor de destino de desinstalación, haga clic con el botón secundario en el servidor de destino, en el icono **Servidores** y haga clic en **Administrar como** para proporcionar una cuenta con derechos de administrador. El servidor en el que desea montar un VHD sin conexión debe agregarse al Administrador del servidor y el usuario debe tener derechos de administrador en ese servidor.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Para quitar roles y características mediante la quitar Roles y características de Asistente  
  
1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.  
  
    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.  
  
    -   En la pantalla **Inicio** de Windows, haga clic en el icono **Administrador del servidor** .  
  
2.  En el menú **Administrar**, haga clic en **Quitar roles y funciones**.  
  
3.  En la página **Antes de comenzar** , compruebe que ha preparado la eliminación de roles y características de un servidor. Haz clic en **Siguiente**.  
  
4.  En el **Seleccionar servidor de destino** página, seleccione un servidor del grupo de servidores o seleccione un VHD sin conexión. Para seleccionar un VHD sin conexión, primero seleccione el servidor en el que montará el VHD y, a continuación, seleccione el archivo VHD.  
  
    > [!NOTE]  
    > La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
    >   
    > -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
    > -   Acceso **Control total** en la pestaña **Seguridad** del cuadro de diálogo **Propiedades** de la carpeta o archivo.  
  
    Para obtener información acerca de cómo agregar servidores al grupo de servidores, consulte [agregar servidores al administrador del servidor](add-servers-to-server-manager.md). Una vez seleccionado el servidor de destino, haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > Puede usar la eliminación de Roles y características Asistente para quitar roles y características desde servidores que ejecutan la misma versión de Windows Server que admiten la versión del administrador del servidor que está usando. No se puede quitar roles, servicios de rol o características de los servidores que ejecutan Windows Server 2016, si está ejecutando el administrador del servidor en Windows Server 2012 R2, Windows Server 2012 o Windows 8. No se puede usar la quitar Roles y características Asistente para quitar roles y características de los servidores que ejecutan Windows Server 2008 o Windows Server 2008 R2.  
  
5.  Seleccione los roles, seleccione los servicios para el rol, si corresponde, y haga clic en **Siguiente** para seleccionar las características.  
  
    Según proceda, la eliminación de Roles y características Asistente le pedirá automáticamente que quite los roles, servicios de rol o características que no pueden ejecutarse sin los roles o las características que va a quitar.  
  
    Además, puede optar por quitar herramientas de administración y complementos para los roles del servidor de destino. De forma predeterminada, en el para quitar Roles y características Asistente, se seleccionan las herramientas de administración para su eliminación. Puede dejar los complementos y las herramientas de administración si planea usar el servidor seleccionado para administrar el rol en otros servidores remotos.  
  
6.  En la página **Confirmar selecciones de eliminación**, revise el rol, la característica y las selecciones de servidor. Si está listo para quitar los roles o características, haga clic en **quitar**.  
  
7.  Tras hacer clic en **quitar**, **progreso de la eliminación** página muestra el progreso de la eliminación, los resultados y mensajes como advertencias, errores o pasos de configuración posteriores a la eliminación necesarios, como Reiniciando el servidor de destino. En Windows Server 2012 y versiones posteriores de Windows Server, puede cerrar la quitar Roles y características Asistente mientras la eliminación permanece en progresan y ver resultados de la eliminación u otros mensajes en el **notificaciones** área en la parte superior de la Consola de administrador del servidor. Haga clic en el **notificaciones** marca para ver más detalles sobre las eliminaciones u otras tareas que se va a realizar en el administrador del servidor.  
  
## <a name="BKMK_removewps"></a>quitar roles, servicios de rol y características mediante los cmdlets de Windows PowerShell  
Los cmdlets de implementación del administrador del servidor para la función de Windows PowerShell de forma similar a basado en GUI quitar Roles y características Asistente, con una diferencia importante. En Windows PowerShell, a diferencia de en el para quitar Roles y características Asistente, las herramientas de administración y complementos para un rol no se quitan de forma predeterminada. Para eliminar herramientas de administración como parte de una eliminación de rol, agregue el parámetro `IncludeManagementTools` al cmdlet. Si está desinstalando roles y características de un servidor que ejecuta la opción de instalación Server Core de Windows Server 2012 o una versión posterior de Windows Server, este parámetro quita en línea de comandos y las herramientas de administración de Windows PowerShell para el elemento especificado roles y características.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Para eliminar roles y características mediante el cmdlet Uninstall-WindowsFeature  
  
1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    > [!NOTE]  
    > Si está desinstalando roles y características de un servidor remoto, no es necesario ejecutar Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En el Windows **iniciar** pantalla, haga clic en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **ejecutar como administrador**.  
  
2.  Escriba **Get-WindowsFeature** y, a continuación, presione **Entrar** para ver una lista de los roles y las características disponibles e instalados en el servidor local. Si el equipo local no es un servidor, o si desea obtener información acerca de un servidor remoto, ejecute **Get-WindowsFeature - computerName <***computer_name***>**, en el que  *computer_name* representa el nombre de un equipo remoto que ejecuta Windows Server 2016. Los resultados del cmdlet contienen los nombres de comando de roles y características que agregue al cmdlet en el paso 4.  
  
    > [!NOTE]  
    > En Windows PowerShell 3.0 y versiones posteriores de Windows PowerShell, no hay ninguna necesidad de importar el módulo de cmdlets del administrador del servidor en la sesión de Windows PowerShell antes de ejecutar los cmdlets que forman parte del módulo. El módulo se importa automáticamente la primera vez que se ejecuta un cmdlet que es parte del módulo. Además, los cmdlets de Windows PowerShell ni los nombres de características usados con los cmdlets distinguen mayúsculas de minúsculas.  
  
3.  tipo **Get-help Uninstall-WindowsFeature**y, a continuación, presione **ENTRAR** para ver la sintaxis y los parámetros aceptados para el `Uninstall-WindowsFeature` cmdlet.  
  
4.  Escriba lo siguiente y, a continuación, presione **Entrar**, donde *feature_name* representa el nombre de comando de un rol o característica que se desea quitar (obtenido en el paso 2) y *computer_name* representa un equipo remoto del que se desean quitar roles y características. Separe los valores múltiples para *feature_name* con comas. El parámetro `Restart` reinicia automáticamente los servidores de destino si así lo requiere la eliminación del rol o la característica.  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    Para quitar roles y características de un VHD sin conexión, agregue tanto el parámetro `computerName` como el parámetro `VHD`. Si no agrega el parámetro `computerName`, el cmdlet da por hecho que el equipo local está montado para acceder al VHD. El parámetro `computerName` contiene el nombre del servidor en el que se montará el VHD y el parámetro `VHD` contiene la ruta de acceso al archivo VHD en el servidor especificado.  
  
    > [!NOTE]  
    > Debe agregar el `computerName` parámetro si está ejecutando el cmdlet desde un equipo que se está ejecutando un sistema operativo de cliente de Windows.  
    >   
    > La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
    >   
    > -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
    > -   **Control total** acceder en el **seguridad** tab, archivo o carpeta **propiedades** cuadro de diálogo.  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **Ejemplo:** El siguiente cmdlet quita el rol Servicios de dominio de Active Directory y la característica de administración de directivas de grupo desde un servidor remoto, ContosoDC1. También se quitan los complementos y las herramientas de administración, y el servidor de destino se reinicia automáticamente si la eliminación necesita que se reinicien los servidores.  
  
    ```  
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  Eliminación una vez finalizada, compruebe que los roles y características se han quitado abriendo el **todos los servidores** página Administrador del servidor, seleccione el servidor desde el que se haya quitado roles y características y vea el **Roles y Características** icono en la página del servidor seleccionado. También puede ejecutar el `Get-WindowsFeature` cmdlet destinado al servidor seleccionado (Get-WindowsFeature - computerName <*computer_name*>) para ver una lista de roles y características que están instaladas en el servidor.  
  
## <a name="BKMK_batch"></a>Instalar roles y características en varios servidores ejecutando un script de Windows PowerShell  
Aunque no se puede usar el agregar Roles y características Asistente para instalar roles, servicios de rol y características en más de un servidor de destino de una única sesión del asistente, puede usar un script de Windows PowerShell para instalar roles, servicios de rol y características en varios destinos servidores que se están administrando mediante el administrador del servidor. El script que use para realizar la implementación por lotes, como se llama a este proceso, apunta a un archivo de configuración XML que se puede crear fácilmente con el Asistente de las características y agregar Roles y haciendo clic en **exportar opciones de configuración** después de avanzar por el Asistente para la **Confirmar selecciones de instalación** página del Asistente de las características y agregar funciones.  
  
> [!IMPORTANT]  
> Todos los servidores de destino que se especifican en el script deben ejecutar la versión de Windows Server que coincida con la versión del administrador del servidor está ejecutando en el equipo local. Por ejemplo, si está ejecutando el administrador del servidor en Windows 10, puede instalar roles, servicios de rol y características en servidores que ejecutan Windows Server 2016. Si se agregan herramientas de administración basada en GUI a la instalación, el proceso de instalación automáticamente convierte servidores de destino que ejecutan la opción de instalación Server Core de Windows Server en la opción de instalación completa (servidor con una GUI completa, también se conoce como ejecución Shell gráfico de servidor).  
>   
> El script proporcionado en esta sección es un ejemplo de cómo se puede realizar la implementación por lotes mediante la `Install-WindowsFeature` cmdlet y una secuencia de comandos de Windows PowerShell. Hay otros posibles scripts y métodos para realizar la implementación por lotes en varios servidores. Para buscar o proporcionar otros scripts para implementar roles y características, busque en el [Repositorio del Centro de scripts](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Para instalar roles y características en varios servidores  
  
1.  Si aún no lo ha hecho, cree un archivo de configuración XML que contiene los roles, servicios de rol y características que desea instalar en varios servidores. Puede crear este archivo de configuración que ejecutan el agregar Roles y características Asistente, seleccione roles, servicios de rol y características que desee y haciendo clic en **exportar opciones de configuración** después de avanzar por el Asistente para la **Confirmar selecciones de instalación** página. Guarde el archivo de configuración en una ubicación conveniente. No necesita hacer clic en **Instalar** ni completar el asistente si solo lo ejecuta para crear un archivo de configuración.  
  
2.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En el Windows **iniciar** pantalla, haga clic en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **ejecutar como administrador**.  
  
3.  Copie y pegue el siguiente script en la sesión de Windows PowerShell.  
  
    ```  
    function Invoke-WindowsFeatureBatchDeployment {  
        param (  
            [parameter(mandatory)]  
            [string[]] $computerNames,  
            [parameter(mandatory)]  
            [string] $ConfigurationFilepath  
        )  
  
        # Deploy the features on multiple computers simultaneously.  
        $jobs = @()  
        foreach($computerName in $computerNames) {  
            $jobs += start-Job -Command {  
                Install-WindowsFeature -ConfigurationFilepath $using:ConfigurationFilepath -computerName $using:computerName -Restart  
            }   
        }  
  
        Receive-Job -Job $jobs -Wait | select-Object Success, RestartNeeded, exitCode, FeatureResult  
    }  
    ```  
  
    Los servidores de destino se reinician automáticamente si así los requieren los roles y características que seleccione.  
  
4.  Ejecute la función mediante uno de los procedimientos siguientes.  
  
    1.  Cree una variable en la que almacenar los nombres de los equipos de destino, separados por coma. En el ejemplo siguiente, la variable `$ServerNames` almacena los nombres de los servidores de destino *Contoso_01* y *Contoso_02*. Presione **Entrar**.  
  
        ```  
        # Sample Invocation  
        $ServerNames = 'Contoso_01','Contoso_02'  
        Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\sampleuser\Desktop\DeploymentConfigTemplate.xml  
        ```  
  
    2.  Para ejecutar la función, escriba lo siguiente y presione **Entrar**, donde `$ServerNames` es un ejemplo de la variable que creó en el paso anterior y *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* es un ejemplo de una ruta al archivo de configuración que creó en el paso 1.  
  
        **Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Cuando finalice la instalación, comprobar la instalación, abra el **todos los servidores** página Administrador del servidor, seleccione un servidor en el que haya instalado roles y características y vea el **Roles y características** icono en la página del servidor seleccionado. También puede ejecutar el `Get-WindowsFeature` cmdlet destinado a un servidor específico (`Get-WindowsFeature -computerName` <*computer_name*>) para ver una lista de roles y características que están instaladas en el servidor.  
  
## <a name="BKMK_FoD"></a>Instalar .NET Framework 3.5 y otras características a petición  
a partir de Windows Server 2012 y Windows 8, los archivos de características para .NET Framework 3.5 (que incluye .NET Framework 2.0 y .NET Framework 3.0) no están disponibles en el equipo local de forma predeterminada. Los archivos se han eliminado. Los archivos de las características que se eliminaron en una configuración de características a petición, junto con los archivos de características para .NET Framework 3.5, se encuentran disponibles a través de Windows Update. De forma predeterminada, si los archivos de características no están disponibles en el servidor de destino que ejecuta Windows Server 2012 o versiones posteriores, el proceso de instalación busca los archivos que faltan conecta a Windows Update. Puede invalidar el comportamiento predeterminado, configure una configuración de directiva de grupo o especifique una ruta de acceso de origen alternativa durante la instalación, independientemente de si va a instalar mediante el agregar Roles y características de GUI del Asistente para o una línea de comandos.  
  
Para instalar .NET Framework 3.5, realice una de las siguientes acciones.  
  
-   Use [Para instalar .NET Framework 3.5 mediante la ejecución del cmdlet Install-WindowsFeature](#BKMK_dotnetcmdlet) para agregar el parámetro `Source` y especifique un origen a partir del cual se van a obtener los archivos de características de .NET Framework 3.5. Si no agrega el parámetro `Source`, el proceso de instalación primero determina si se ha especificado una ruta de acceso a los archivos de características en la configuración de la directiva de grupo y, si no encuentra la ruta de acceso, usa Windows Update para buscar los archivos de características que faltan.  
  
-   Use [para instalar .NET Framework 3.5 mediante el agregar Roles y características Asistente](#BKMK_arfw) para especificar una ubicación de archivo de origen alternativa en el **Confirmar opciones de instalación** página del Asistente de las características y agregar funciones.  
  
-   Use [Para instalar .NET Framework 3.5 mediante DISM](#BKMK_dism) para obtener los archivos desde Windows Update de manera predeterminada o mediante la especificación de una ruta de acceso de origen para los medios de instalación.  
  
[Configurar orígenes alternativos para los archivos de características en la directiva de grupo](#BKMK_configgp) para .NET Framework 3.5 u otras características, si no se encuentran los archivos de características en el equipo local.  
  
> [!IMPORTANT]  
> Al instalar archivos de características desde un origen remoto, la ruta de acceso de origen o el recurso compartido de archivos deben conceder permisos de **lectura** al grupo **Todos** (no se recomienda por razones de seguridad) o a la cuenta de equipo (de sistema local) del servidor de destino; no es suficiente conceder acceso a la cuenta de usuario.  
>   
> Los servidores que formen parte de grupos de trabajo no pueden obtener acceso a recursos compartidos de archivos externos, aunque la cuenta de equipo del servidor del grupo de trabajo tenga el permiso **Lectura** en el recurso compartido externo. Hay ubicaciones de origen alternativas que funcionan para servidores del grupo de trabajo, como medios de instalación, Windows Update y archivos VHD o WIM almacenados en el servidor del grupo de trabajo local.  
>   
> Puede especificar un archivo WIM como un origen de archivo de características alternativo cuando va a instalar roles, servicios de rol y características en un servidor físico y ejecución. La ruta de acceso de origen de un archivo WIM debe tener el formato siguiente, con **WIM** como prefijo y el índice en el que los archivos de características se encuentran como sufijo: **WIM:e:\sources\install.wim:4**. Sin embargo, no se puede usar un archivo WIM directamente como origen para la instalación de roles, servicios de rol y características en un VHD sin conexión; debe montar el VHD sin conexión y apuntar a su ruta de montaje para los archivos de origen, o debe apuntar a una carpeta que contiene una copia del contenido del archivo WIM.  
  
### <a name="BKMK_dotnetcmdlet"></a>Para instalar .NET Framework 3.5, ejecute el cmdlet Install-WindowsFeature  
  
1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    > [!NOTE]  
    > Si está instalando roles y características desde un servidor remoto, no es necesario ejecutar Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En el Windows **iniciar** pantalla, haga clic en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **ejecutar como administrador**.  
  
    -   En un servidor que ejecuta la opción de instalación Server Core de Windows Server 2012 R2 o Windows Server 2012, escriba **PowerShell** en un símbolo del sistema y, a continuación, presione **ENTRAR**.  
  
2.  Escriba el siguiente comando y, a continuación, presione **ENTRAR**. En el siguiente ejemplo, los archivos de origen se encuentran en un almacén colateral (abreviado como **SxS**) en los medios de instalación de la unidad D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Si quiere que la línea de comandos use Windows Update como el origen de los archivos de características que faltan, o si ya se ha configurado un origen predeterminado mediante la directiva de grupo, no es necesario agregar el parámetro `Source` , a menos que desee especificar otro origen.  
  
### <a name="BKMK_arfw"></a>Para instalar .NET Framework 3.5 mediante el agregar Roles y características Asistente  
  
1.  En el **administrar** menú Administrador del servidor, haga clic en **agregar Roles y características**.  
  
2.  Seleccione un servidor de destino que ejecuta Windows Server 2016.  
  
3.  En el **seleccionar características** página del Asistente de características, seleccione Agregar Roles y **.NET Framework 3.5**.  
  
4.  Si la configuración de la directiva de grupo lo permite en el equipo local, el proceso de instalación intentará obtener los archivos de características que faltan mediante Windows Update. Haga clic en **Instalar**; no es necesario avanzar al siguiente paso.  
  
    Si la configuración de directiva de grupo no lo permite, o desea usar otro origen para los archivos de características de .NET Framework 3.5, en el **Confirmar selecciones de instalación** página del asistente, haga clic en **especificar una ruta de acceso de origen alternativa** .  
  
5.  Proporcione una ruta de acceso a un almacén colateral (denominado **SxS**) en los medios de instalación o a un archivo WIM. En el siguiente ejemplo, los medios de instalación se encuentran en la unidad D.  
  
    **D:\Sources\SxS\\**  
  
    Para especificar un archivo WIM, agregue un prefijo **WIM:** y agregue el índice de la imagen que se usará en el archivo WIM como sufijo, como se muestra en el siguiente ejemplo.  
  
    **WIM:\\\\***server_name***\share\install.wim:3**  
  
6.  Haga clic en **Aceptar**y, a continuación, en **Instalar**.  
  
### <a name="BKMK_dism"></a>Para instalar .NET Framework 3.5 mediante DISM  
  
1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    > [!NOTE]  
    > Si está instalando roles y características desde un servidor remoto, no es necesario ejecutar Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En el Windows **iniciar** pantalla, haga clic en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **ejecutar como administrador**.  
  
    -   En un servidor que ejecuta la opción de instalación Server Core, escriba **PowerShell** en un símbolo del sistema y, a continuación, presione **ENTRAR**.  
  
2.  Ejecute uno de los siguientes comandos DISM.  
  
    -   Si el equipo tiene acceso a Windows Update o una ubicación predeterminada del archivo de origen ya se ha configurado en directiva de grupo, ejecute el siguiente comando.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Si el equipo tiene acceso a los medios de instalación, ejecute un comando similar al siguiente. En el siguiente ejemplo, los medios de instalación del sistema operativo se encuentran en la unidad D. El parámetro `LimitAccess` impide que el comando intente ponerse en contacto con Windows Update o con un servidor que ejecute WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > El comando DISM distingue mayúsculas de minúsculas.  
  
### <a name="BKMK_configgp"></a>Configurar orígenes alternativos para los archivos de características en la directiva de grupo  
La configuración de la directiva de grupo descrita en esta sección especifica las ubicaciones de origen autorizadas para los archivos de .NET Framework 3.5 y otros archivos de características que se han quitado como parte de la configuración de características a petición. La configuración de directiva **especificar la configuración de instalación de componentes opcionales y reparación de componentes** se encuentra en la **equipo Configuración del equipo\Plantillas administrativas\Sistema** carpeta en la directiva de grupo Editor de directiva de grupo Local o de consola de administración.  
  
> [!NOTE]  
> Debe pertenecer al grupo Administradores para cambiar la configuración de la directiva de grupo en el equipo local. Si la configuración de la directiva de grupo en el equipo que desea administrar se controla en el nivel del dominio, debe pertenecer al grupo de administradores de dominio para modificarla.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Para configurar una ruta de acceso de origen alternativa predeterminada en la directiva de grupo  
  
1.  En el editor de directivas de grupo Local o la consola de administración de directivas de grupo, abra la siguiente configuración de directiva.  
  
    **configuración del equipo\Plantillas administrativas\sistema\especificar configuración de equipo para la instalación de componentes opcionales y reparación de componentes**  
  
2. Seleccione **habilitado** para habilitar la configuración de directiva, si aún no está habilitado.  
  
3.  En el cuadro de texto **Ruta de acceso del archivo de origen alternativa**, en el área **Opciones**, especifique una ruta de acceso completa a una carpeta compartida o a un archivo WIM. Para especificar un archivo WIM como ubicación del archivo de origen alternativa, agregue el prefijo **WIM:** a la ruta de acceso y agregue el índice de la imagen que se usará en el archivo WIM como sufijo. Los siguientes son ejemplos de valores que puede especificar.  
  
    -   ruta de acceso a una carpeta compartida: **\\\\***nombre_servidor***\share\\*** nombreDeCarpeta*  
  
    -   ruta de acceso a un archivo WIM, donde **3** representa el índice de la imagen en el que se encuentran los archivos de características:  **WIM:\\\\***server_name***\share\install.wim:3**  
  
4.  Si no desea que los equipos controlados por esta configuración de directiva para buscar archivos de características que faltan en Windows Update, seleccione **no intentar descargar carga desde Windows Update**.  
  
5.  Si los equipos controlados por esta configuración de directiva suelen recibir actualizaciones a través de WSUS, pero prefiere usar Windows Update en lugar de WSUS para buscar los archivos de características que faltan, seleccione **Ponerse en contacto directamente con Windows Update para descargar contenido de reparaciones en lugar de usar Windows Server Update Services (WSUS)**.  
  
6.  Haga clic en **Aceptar** cuando haya terminado de modificar esta configuración de directiva y, a continuación, cierre el Editor de directivas de grupo.  
  
## <a name="see-also"></a>Vea también  
[Opciones de instalación de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Consideraciones de implementación de Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Cómo habilitar o deshabilitar características de Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


