---
title: Instalación o desinstalación de roles, servicios de rol o características
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cca2e4c7ba2658c4d85b14ef61ef5f79fbc96345
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383191"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Instalación o desinstalación de roles, servicios de rol o características

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, la consola de Administrador del servidor y los cmdlets de Windows PowerShell para Administrador del servidor permiten la instalación de roles y características en servidores remotos o locales, o en discos duros virtuales (VHD) sin conexión. Puede instalar varios roles y características en un solo servidor remoto o VHD sin conexión en un único asistente para agregar roles y características o en una sesión de Windows PowerShell.  
  
> [!IMPORTANT]  
> El Administrador de servidores no se puede usar para administrar una versión más reciente del sistema operativo Windows Server. Administrador del servidor que se ejecutan en Windows Server 2012 R2 o Windows 8.1 no se pueden usar para instalar roles, servicios de rol y características en servidores que ejecutan Windows Server 2016.  
  
Debe haber iniciado sesión en un servidor como administrador para instalar o desinstalar roles, servicios de función y características. Si ha iniciado sesión en el equipo local con una cuenta que no tiene derechos de administrador en el servidor de destino, haga clic con el botón secundario en el servidor de destino, en el icono **Servidores** y haga clic en **Administrar como** para proporcionar una cuenta con derechos de administrador. El servidor en el que desea montar un VHD sin conexión debe agregarse al Administrador del servidor y el usuario debe tener derechos de administrador en ese servidor.  
  
Para obtener más información sobre los roles, servicios de rol y características, vea [roles, servicios de rol y características](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
En este tema se incluyen las siguientes secciones.  
  
-   [Instalar roles, servicios de rol y características con el Asistente para agregar roles y características](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)  
  
-   [Instalación de roles, servicios de rol y características mediante los cmdlets de Windows PowerShell](#install-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Quitar roles, servicios de rol y características con el Asistente para quitar roles y características](#remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard)  
  
-   [Eliminación de roles, servicios de rol y características mediante los cmdlets de Windows PowerShell](#remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Instalar roles y características en varios servidores ejecutando un script de Windows PowerShell](#install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script)  
  
-   [Instalación de .NET Framework 3.5 y otras características a petición](#install-net-framework-35-and-other-features-on-demand)  
  
## <a name="install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard"></a>Instalar roles, servicios de rol y características con el Asistente para agregar roles y características  
En una sola sesión del Asistente para agregar roles y características, puede instalar roles, servicios de rol y características en el servidor local, un servidor remoto que se ha agregado a Administrador del servidor o un VHD sin conexión. Para obtener más información acerca de cómo agregar un servidor a Administrador del servidor administrar, consulte [agregar servidores a administrador del servidor](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Si ejecuta Administrador del servidor en Windows Server 2016 o Windows 10, puede usar el Asistente para agregar roles y características para instalar roles y características únicamente en servidores y VHD sin conexión que ejecutan Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Para instalar roles y características mediante el Asistente para agregar roles y características  
  
1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.  
  
    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.  
  
    -   En la pantalla **Inicio** de Windows, haga clic en el icono de **Administrador del servidor** .  
  
2.  En el menú **administrar** , haga clic en **Agregar roles y características**.  
  
3.  En la página **Antes de comenzar**, compruebe que el servidor de destino y el entorno de red estén preparados para el rol y la característica que desea instalar. Haz clic en **Siguiente**.  
  
4.  En la página **Seleccionar tipo de instalación** , seleccione **Instalación basada en características o en roles** para instalar todas las partes de los roles o las características en un solo servidor, o bien seleccione **Instalación de Servicios de Escritorio remoto** para instalar una infraestructura de escritorio basada en máquina virtual o una infraestructura de escritorio basada en sesión para Servicios de Escritorio remoto. La opción **Instalación de Servicios de Escritorio remoto** que distribuye las partes lógicas del rol de Servicios de Escritorio remoto en los diferentes servidores según la necesidad de los administradores. Haz clic en **Siguiente**.  
  
5.  En la página **Seleccionar servidor de destino**, seleccione un servidor del grupo de servidores o un VHD sin conexión. Para seleccionar un VHD sin conexión como servidor de destino, primero seleccione el servidor en el que montará el VHD y, a continuación, seleccione el archivo VHD. Para obtener información acerca de cómo agregar servidores al grupo de servidores, consulte [agregar servidores a administrador del servidor](add-servers-to-server-manager.md). Una vez seleccionado el servidor de destino, haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > Para instalar roles y características en VHD sin conexión, los VHD de destino deben cumplir los requisitos siguientes.  
    >   
    > -   Los VHD deben ejecutar la versión de Windows Server que coincida con la versión de Administrador del servidor que se está ejecutando. Vea la nota al principio de [instalar roles, servicios de rol y características con el Asistente para agregar roles y características](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
    > -   Los VHD no deben tener más de una partición o volumen de sistema.  
    > -   La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
    >   
    >     -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
    >     -   Acceso de **control total** en la pestaña **seguridad** , en el cuadro de diálogo **propiedades** de archivo o carpeta.  
  
6.  Seleccione los roles, seleccione los servicios para el rol, si corresponde, y haga clic en **Siguiente** para seleccionar las características.  
  
    A medida que avance, el Asistente para agregar roles y características le informará automáticamente si se encuentran conflictos en el servidor de destino que pueden impedir la instalación o el funcionamiento normal de los roles o características seleccionados. También se le solicitará que agregue los roles, servicios de rol o características que necesiten los roles o las características que ha seleccionado.  
  
    Además, si planea administrar el rol de forma remota, desde otro servidor o desde un equipo basado en cliente de Windows que está ejecutando las Herramientas de administración remota del servidor, puede optar por no instalar los complementos y las herramientas de administración para los roles en el servidor de destino. De forma predeterminada, en el Asistente para agregar roles y características, se seleccionan las herramientas de administración para la instalación.  
  
7.  En la página **Confirmar selecciones de instalación** , revise el rol, la característica y las selecciones de servidor. Si está listo para realizar la instalación, haga clic en **Instalar**.  
  
    También puede exportar las selecciones a un archivo de configuración basado en XML que puede usar para instalaciones desatendidas con Windows PowerShell. Para exportar la configuración especificada en esta sesión del Asistente para agregar roles y características, haga clic en **exportar opciones de configuración**y guarde el archivo XML en una ubicación adecuada.  
  
    El comando **Especifique una ruta de acceso de origen alternativa** en la página **Confirmar selecciones de instalación** permite especificar una ruta de acceso de origen para los archivos necesarios para instalar los roles y las características en el servidor seleccionado. En Windows Server 2012 y versiones posteriores de Windows Server, [características a petición](https://go.microsoft.com/fwlink/p/?LinkID=241573) permite reducir la cantidad de espacio en disco que usa el sistema operativo, quitando los archivos de roles y características de los servidores que se administran exclusivamente de forma remota. Si ha eliminado los archivos de rol y de características desde un servidor mediante el cmdlet `Uninstall-WindowsFeature -remove`, podrá volver a instalarlos más adelante al especificar una ruta de origen alternativa o un recurso compartido donde se almacenan los archivos de características y roles requeridos. La ruta de acceso de origen o el recurso compartido de archivos deben conceder permisos de **lectura** al grupo **todos** (no se recomienda por motivos de seguridad) o a la cuenta de equipo (*dominio*\\*SERverNAME*$) del servidor de destino. no es suficiente conceder acceso a la cuenta de usuario. Para obtener más información sobre Características a petición, vea [Opciones de instalación de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    Puede especificar un archivo WIM como un origen de archivo de características alternativo al instalar roles, servicios de rol y características en un servidor físico en ejecución. La ruta de acceso de origen de un archivo WIM debe tener el formato siguiente, con **WIM** como prefijo, y el índice donde se encuentran los archivos de características como sufijo: **WIM:e:\sources\install.wim:4**. Sin embargo, no puede usar un archivo WIM directamente como origen para instalar roles, servicios de rol y características en un VHD sin conexión. debe montar el VHD sin conexión y apuntar a su ruta de acceso de montaje para los archivos de origen, o debe apuntar a una carpeta que contenga una copia del contenido del archivo WIM.  
  
8.  Después de hacer clic en **instalar**, la página progreso de la **instalación** muestra el progreso de la instalación, los resultados y los mensajes como advertencias, errores o pasos de configuración posteriores a la instalación necesarios para los roles o las características que ha instalado. En Windows Server 2012 y versiones posteriores de Windows Server, puede cerrar el Asistente para agregar roles y características mientras la instalación todavía está en curso y ver los resultados de la instalación u otros mensajes en el área **notificaciones** en la parte superior de la consola de administrador del servidor. Haga clic en el icono de la marca **notificaciones** para ver más detalles sobre las instalaciones u otras tareas que está llevando a cabo en Administrador del servidor.  
  
## <a name="install-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Instalación de roles, servicios de rol y características mediante los cmdlets de Windows PowerShell  
Los cmdlets de implementación de Administrador del servidor para Windows PowerShell funcionan de manera similar al Asistente para agregar roles y características basado en GUI y al Asistente para quitar roles y características, con una diferencia importante. En Windows PowerShell, a diferencia del Asistente para agregar roles y características, las herramientas y los complementos de administración para un rol no se incluyen de forma predeterminada. Para incluir herramientas de administración como parte de una instalación de rol, agregue el parámetro `IncludeManagementTools` al cmdlet. Si va a instalar roles y características en un servidor que ejecuta la opción de instalación Server Core de Windows Server 2012 o versiones posteriores, puede Agregar las herramientas de administración de un rol a una instalación, pero no se pueden instalar los complementos y las herramientas de administración basados en GUI. en los servidores que ejecutan la opción de instalación Server Core de Windows Server. Solo las herramientas de administración de Windows PowerShell y la línea de comandos se pueden instalar en la opción de instalación Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Para instalar roles y características mediante el cmdlet Install-WindowsFeature  
  
1. Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
   > [!NOTE]  
   > Si va a instalar roles y características en un servidor remoto, no necesita ejecutar Windows PowerShell con derechos de usuario elevados.  
  
   -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
   -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.  
  
2. Escriba **Get-WindowsFeature** y, a continuación, presione **Entrar** para ver una lista de los roles y las características disponibles e instalados en el servidor local. Si el equipo local no es un servidor, o si desea información acerca de un servidor remoto, ejecute **Get-WindowsFeature-computerName <** <em>computer_name</em> **>** , donde *computer_name* representa el nombre de un equipo remoto que ejecuta Windows Server 2016. Los resultados del cmdlet contienen los nombres de comando de los roles y las características que se agregan al cmdlet en el paso 4.  
  
   > [!NOTE]  
   > En Windows PowerShell 3,0 y versiones posteriores de Windows PowerShell, no es necesario importar el módulo de cmdlet de Administrador del servidor en la sesión de Windows PowerShell antes de ejecutar los cmdlets que forman parte del módulo. El módulo se importa automáticamente la primera vez que se ejecuta un cmdlet que es parte del módulo. Además, ni los cmdlets de Windows PowerShell ni los nombres de las características que se usan con los cmdlets distinguen mayúsculas de minúsculas.  
  
3. Escriba **Get-Help install-WindowsFeature**y, a continuación, presione **entrar** para ver la sintaxis y los parámetros aceptados para el cmdlet `Install-WindowsFeature`.  
  
4. Escriba lo siguiente y, a continuación, presione **entrar**, donde *feature_name* representa el nombre de comando de un rol o característica que desea instalar (obtenido en el paso 2) y *computer_name* representa un equipo remoto en el que desea instalar roles y características. Separe los valores múltiples para *feature_name* con comas. El parámetro `Restart` reinicia el servidor de destino automáticamente si lo requiere la instalación del rol o la característica.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Para instalar roles y características en un VHD sin conexión, agregue tanto el parámetro `computerName` como el parámetro `VHD` . Si no agrega el parámetro `computerName`, el cmdlet da por hecho que el equipo local está montado para acceder al VHD. El parámetro `computerName` contiene el nombre del servidor en el que se montará el VHD y el parámetro `VHD` contiene la ruta de acceso al archivo VHD en el servidor especificado.  
  
   > [!NOTE]  
   > Debe agregar el parámetro `computerName` si está ejecutando el cmdlet desde un equipo que ejecuta un sistema operativo de cliente de Windows.  
   >   
   > Para instalar roles y características en VHD sin conexión, los VHD de destino deben cumplir los requisitos siguientes.  
   >   
   > -   Los VHD deben ejecutar la versión de Windows Server que coincida con la versión de Administrador del servidor que se está ejecutando. Vea la nota al principio de [instalar roles, servicios de rol y características con el Asistente para agregar roles y características](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
   > -   Los VHD no deben tener más de una partición o volumen de sistema.  
   > -   La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
   >   
   >     -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
   >     -   Acceso de **control total** en la pestaña **seguridad** , en el cuadro de diálogo **propiedades** de archivo o carpeta.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Ejemplo:** El siguiente cmdlet instala el rol de servicios de dominio de Active Directory y la característica de administración de directiva de grupo en un servidor remoto, ContosoDC1. Se agregan los complementos y las herramientas de administración mediante el parámetro `IncludeManagementTools`, y el servidor de destino se reinicia automáticamente si así lo requiere la instalación.  
  
   ```  
   Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Una vez finalizada la instalación, para comprobar la instalación, abra la página **todos los servidores** en Administrador del servidor, seleccione un servidor en el que haya instalado roles y características, y vea el icono **roles y características** en la página del servidor seleccionado. También puede ejecutar el cmdlet `Get-WindowsFeature` destinado al servidor seleccionado (get-WindowsFeature-computerName <*computer_name*>) para ver una lista de los roles y las características que están instalados en el servidor.  
  
## <a name="remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard"></a>Quitar roles, servicios de rol y características con el Asistente para quitar roles y características  
Debe haber iniciado sesión en un servidor como administrador para desinstalar roles, servicios de función y características. Si ha iniciado sesión en el equipo local con una cuenta que no tiene derechos de administrador en el servidor de destino de desinstalación, haga clic con el botón secundario en el servidor de destino, en el icono **Servidores** y haga clic en **Administrar como** para proporcionar una cuenta con derechos de administrador. El servidor en el que desea montar un VHD sin conexión debe agregarse al Administrador del servidor y el usuario debe tener derechos de administrador en ese servidor.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Para quitar roles y características mediante el Asistente para quitar roles y características  
  
1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.  
  
    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.  
  
    -   En la pantalla **Inicio** de Windows, haga clic en el icono **Administrador del servidor** .  
  
2.  En el menú **Administrar**, haga clic en **Quitar roles y funciones**.  
  
3.  En la página **Antes de comenzar** , compruebe que ha preparado la eliminación de roles y características de un servidor. Haz clic en **Siguiente**.  
  
4.  En la página **Seleccionar servidor de destino** , seleccione un servidor del grupo de servidores o seleccione un VHD sin conexión. Para seleccionar un VHD sin conexión, primero seleccione el servidor en el que montará el VHD y, a continuación, seleccione el archivo VHD.  
  
    > [!NOTE]  
    > La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
    >   
    > -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
    > -   Acceso **Control total** en la pestaña **Seguridad** del cuadro de diálogo **Propiedades** de la carpeta o archivo.  
  
    Para obtener información acerca de cómo agregar servidores al grupo de servidores, consulte [agregar servidores a administrador del servidor](add-servers-to-server-manager.md). Una vez seleccionado el servidor de destino, haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > Puede usar el Asistente para quitar roles y características para quitar roles y características de los servidores que ejecutan la misma versión de Windows Server que admite la versión de Administrador del servidor que esté usando. No se pueden quitar roles, servicios de rol o características de los servidores que ejecutan Windows Server 2016, si se ejecuta Administrador del servidor en Windows Server 2012 R2, Windows Server 2012 o Windows 8. No puede usar el Asistente para quitar roles y características para quitar roles y características de los servidores que ejecutan Windows Server 2008 o Windows Server 2008 R2.  
  
5.  Seleccione los roles, seleccione los servicios para el rol, si corresponde, y haga clic en **Siguiente** para seleccionar las características.  
  
    A medida que avance, el Asistente para quitar roles y características le pedirá automáticamente que quite los roles, servicios de rol o características que no se puedan ejecutar sin los roles o las características que está quitando.  
  
    Además, puede optar por quitar los complementos y las herramientas de administración para los roles en el servidor de destino. De forma predeterminada, en el Asistente para quitar roles y características, se seleccionan las herramientas de administración para su eliminación. Puede dejar los complementos y las herramientas de administración si planea usar el servidor seleccionado para administrar el rol en otros servidores remotos.  
  
6.  En la página **Confirmar selecciones de eliminación**, revise el rol, la característica y las selecciones de servidor. Si está listo para quitar los roles o las características, haga clic en **quitar**.  
  
7.  Después de hacer clic en **quitar**, la página progreso de la **eliminación** muestra el progreso de la eliminación, los resultados y los mensajes como advertencias, errores o pasos de configuración posteriores a la eliminación necesarios, como reiniciar el servidor de destino. En Windows Server 2012 y versiones posteriores de Windows Server, puede cerrar el Asistente para quitar roles y características mientras la eliminación todavía está en curso y ver los resultados de la eliminación u otros mensajes en el área **notificaciones** en la parte superior de la consola de administrador del servidor. Haga clic en la marca **notificaciones** para ver más detalles sobre las eliminaciones u otras tareas que está llevando a cabo en Administrador del servidor.  
  
## <a name="remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Eliminación de roles, servicios de rol y características mediante los cmdlets de Windows PowerShell  
Los cmdlets de implementación de Administrador del servidor para Windows PowerShell funcionan de manera similar al Asistente para quitar roles y características basado en GUI, con una diferencia importante. En Windows PowerShell, a diferencia del Asistente para quitar roles y características, los complementos y las herramientas de administración para un rol no se quitan de forma predeterminada. Para eliminar herramientas de administración como parte de una eliminación de rol, agregue el parámetro `IncludeManagementTools` al cmdlet. Si está desinstalando roles y características de un servidor que ejecuta la opción de instalación Server Core de Windows Server 2012 o una versión posterior de Windows Server, este parámetro quita la línea de comandos y las herramientas de administración de Windows PowerShell para el especificado. roles y características.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Para eliminar roles y características mediante el cmdlet Uninstall-WindowsFeature  
  
1. Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
   > [!NOTE]  
   > Si está desinstalando roles y características de un servidor remoto, no necesita ejecutar Windows PowerShell con derechos de usuario elevados.  
  
   -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
   -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.  
  
2. Escriba **Get-WindowsFeature** y, a continuación, presione **Entrar** para ver una lista de los roles y las características disponibles e instalados en el servidor local. Si el equipo local no es un servidor, o si desea información acerca de un servidor remoto, ejecute **Get-WindowsFeature-computerName <** <em>computer_name</em> **>** , donde *computer_name* representa el nombre de un equipo remoto que ejecuta Windows Server 2016. Los resultados del cmdlet contienen los nombres de comando de los roles y las características que se agregan al cmdlet en el paso 4.  
  
   > [!NOTE]  
   > En Windows PowerShell 3,0 y versiones posteriores de Windows PowerShell, no es necesario importar el módulo de cmdlet de Administrador del servidor en la sesión de Windows PowerShell antes de ejecutar los cmdlets que forman parte del módulo. El módulo se importa automáticamente la primera vez que se ejecuta un cmdlet que es parte del módulo. Además, ni los cmdlets de Windows PowerShell ni los nombres de las características que se usan con los cmdlets distinguen mayúsculas de minúsculas.  
  
3. Escriba **Get-Help Uninstall-WindowsFeature**y, a continuación, presione **entrar** para ver la sintaxis y los parámetros aceptados para el cmdlet `Uninstall-WindowsFeature`.  
  
4. Escriba lo siguiente y, a continuación, presione **Entrar**, donde *feature_name* representa el nombre de comando de un rol o característica que se desea quitar (obtenido en el paso 2) y *computer_name* representa un equipo remoto del que se desean quitar roles y características. Separe los valores múltiples para *feature_name* con comas. El parámetro `Restart` reinicia automáticamente los servidores de destino si así lo requiere la eliminación del rol o la característica.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Para quitar roles y características de un VHD sin conexión, agregue tanto el parámetro `computerName` como el parámetro `VHD`. Si no agrega el parámetro `computerName`, el cmdlet da por hecho que el equipo local está montado para acceder al VHD. El parámetro `computerName` contiene el nombre del servidor en el que se montará el VHD y el parámetro `VHD` contiene la ruta de acceso al archivo VHD en el servidor especificado.  
  
   > [!NOTE]  
   > Debe agregar el parámetro `computerName` si está ejecutando el cmdlet desde un equipo que ejecuta un sistema operativo de cliente de Windows.  
   >   
   > La carpeta compartida de red donde está almacenado el archivo de VHD debe conceder los siguientes derechos de acceso a la cuenta de equipo (o sistema local) del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.  
   >   
   > -   Acceso de **Lectura y escritura** en el cuadro de diálogo **Uso compartido de archivos**.  
   > -   Acceso de **control total** en la pestaña **seguridad** , en el cuadro de diálogo **propiedades** de archivo o carpeta.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Ejemplo:** El siguiente cmdlet quita el rol de Active Directory Domain Services y la característica de administración de directiva de grupo de un servidor remoto, ContosoDC1. También se quitan los complementos y las herramientas de administración, y el servidor de destino se reinicia automáticamente si la eliminación necesita que se reinicien los servidores.  
  
   ```  
   Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Una vez finalizada la eliminación, compruebe que se han quitado los roles y las características; para ello, abra la página **todos los servidores** en Administrador del servidor, seleccione el servidor del que quitó roles y características y vea el icono **roles y características** en la página del servidor seleccionado. También puede ejecutar el cmdlet `Get-WindowsFeature` destinado al servidor seleccionado (get-WindowsFeature-computerName <*computer_name*>) para ver una lista de los roles y las características que están instalados en el servidor.  
  
## <a name="install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script"></a>Instalar roles y características en varios servidores ejecutando un script de Windows PowerShell  
Aunque no puede usar el Asistente para agregar roles y características para instalar roles, servicios de rol y características en más de un servidor de destino en una única sesión del asistente, puede usar un script de Windows PowerShell para instalar roles, servicios de rol y características en varios destinos. servidores que se administran mediante Administrador del servidor. El script que se usa para realizar la implementación por lotes, a medida que se llama a este proceso, apunta a un archivo de configuración XML que puede crear fácilmente con el Asistente para agregar roles y características y hacer clic en **exportar opciones de configuración** después de avanzar por el asistente hasta la página **confirmar selecciones de instalación** del Asistente para agregar roles y características.  
  
> [!IMPORTANT]  
> Todos los servidores de destino que se especifican en el script deben ejecutar la versión de Windows Server que coincida con la versión de Administrador del servidor que se ejecuta en el equipo local. Por ejemplo, si ejecuta Administrador del servidor en Windows 10, puede instalar roles, servicios de rol y características en servidores que ejecutan Windows Server 2016. Si se agregan herramientas de administración basadas en GUI a la instalación, el proceso de instalación convierte automáticamente los servidores de destino que ejecutan la opción de instalación Server Core de Windows Server en la opción de instalación completa (servidor con una GUI completa, también conocida como como el shell gráfico de servidor en ejecución).  
>   
> El script que se proporciona en esta sección es un ejemplo de cómo se puede realizar la implementación de Batch mediante el cmdlet `Install-WindowsFeature` y un script de Windows PowerShell. Hay otros posibles scripts y métodos para realizar la implementación por lotes en varios servidores. Para buscar o proporcionar otros scripts para implementar roles y características, busque en el [Repositorio del Centro de scripts](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Para instalar roles y características en varios servidores  
  
1.  Si todavía no lo ha hecho, cree un archivo de configuración XML que contenga los roles, servicios de función y características que desea instalar en varios servidores. Puede crear este archivo de configuración mediante la ejecución del Asistente para agregar roles y características, la selección de roles, servicios de rol y características que desee, y haciendo clic en **exportar opciones de configuración** después de avanzar por el asistente hasta la página **confirmar selecciones de instalación** . Guarde el archivo de configuración en una ubicación conveniente. No necesita hacer clic en **Instalar** ni completar el asistente si solo lo ejecuta para crear un archivo de configuración.  
  
2.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.  
  
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
  
        **Invoke-WindowsFeatureBatchDeployment-computerNames $ServerNames-ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Una vez finalizada la instalación, para comprobar la instalación, abra la página **todos los servidores** en Administrador del servidor, seleccione un servidor en el que haya instalado roles y características, y vea el icono **roles y características** en la página del servidor seleccionado. También puede ejecutar el cmdlet `Get-WindowsFeature` destinado a un servidor específico (`Get-WindowsFeature -computerName` <*computer_name*>) para ver una lista de los roles y las características que están instalados en el servidor.  
  
## <a name="install-net-framework-35-and-other-features-on-demand"></a>Instalación de .NET Framework 3.5 y otras características a petición  
a partir de Windows Server 2012 y Windows 8, los archivos de características de .NET Framework 3,5 (que incluye .NET Framework 2,0 y .NET Framework 3,0) no están disponibles en el equipo local de forma predeterminada. Los archivos se han eliminado. Los archivos de las características que se eliminaron en una configuración de características a petición, junto con los archivos de características para .NET Framework 3.5, se encuentran disponibles a través de Windows Update. De forma predeterminada, si los archivos de características no están disponibles en el servidor de destino que ejecuta Windows Server 2012 o versiones posteriores, el proceso de instalación busca los archivos que faltan conectándose a Windows Update. Puede invalidar el comportamiento predeterminado configurando un directiva de grupo valor o especificando una ruta de acceso de origen alternativa durante la instalación, independientemente de que esté instalando mediante la GUI del Asistente para agregar roles y características o una línea de comandos.  
  
Para instalar .NET Framework 3.5, realice una de las siguientes acciones.  
  
-   Use [Para instalar .NET Framework 3.5 mediante la ejecución del cmdlet Install-WindowsFeature](#to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet) para agregar el parámetro `Source` y especifique un origen a partir del cual se van a obtener los archivos de características de .NET Framework 3.5. Si no agrega el parámetro `Source`, el proceso de instalación primero determina si se ha especificado una ruta de acceso a los archivos de características en la configuración de la directiva de grupo y, si no encuentra la ruta de acceso, usa Windows Update para buscar los archivos de características que faltan.  
  
-   Use [para instalar .NET Framework 3,5 mediante el Asistente para agregar roles y características](#to-install-net-framework-35-by-using-the-add-roles-and-features-wizard) para especificar una ubicación de archivo de origen alternativa en la página **confirmar opciones de instalación** del Asistente para agregar roles y características.  
  
-   Use [Para instalar .NET Framework 3.5 mediante DISM](#to-install-net-framework-35-by-using-dism) para obtener los archivos desde Windows Update de manera predeterminada o mediante la especificación de una ruta de acceso de origen para los medios de instalación.  
  
[Configurar orígenes alternativos para los archivos de características en la directiva de grupo](#configure-alternate-sources-for-feature-files-in-group-policy) para .NET Framework 3.5 u otras características, si no se encuentran los archivos de características en el equipo local.  
  
> [!IMPORTANT]  
> Al instalar archivos de características desde un origen remoto, la ruta de acceso de origen o el recurso compartido de archivos deben conceder permisos de **lectura** al grupo **Todos** (no se recomienda por razones de seguridad) o a la cuenta de equipo (de sistema local) del servidor de destino; no es suficiente conceder acceso a la cuenta de usuario.  
>   
> Los servidores que formen parte de grupos de trabajo no pueden obtener acceso a recursos compartidos de archivos externos, aunque la cuenta de equipo del servidor del grupo de trabajo tenga el permiso **Lectura** en el recurso compartido externo. Hay ubicaciones de origen alternativas que funcionan para servidores del grupo de trabajo, como medios de instalación, Windows Update y archivos VHD o WIM almacenados en el servidor del grupo de trabajo local.  
>   
> Puede especificar un archivo WIM como un origen de archivo de características alternativo al instalar roles, servicios de rol y características en un servidor físico en ejecución. La ruta de acceso de origen de un archivo WIM debe tener el formato siguiente, con **WIM** como prefijo, y el índice donde se encuentran los archivos de características como sufijo: **WIM:e:\sources\install.wim:4**. Sin embargo, no puede usar un archivo WIM directamente como origen para instalar roles, servicios de rol y características en un VHD sin conexión. debe montar el VHD sin conexión y apuntar a su ruta de acceso de montaje para los archivos de origen, o debe apuntar a una carpeta que contenga una copia del contenido del archivo WIM.  
  
### <a name="to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet"></a>Para instalar .NET Framework 3.5 mediante la ejecución del cmdlet Install-WindowsFeature  
  
1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    > [!NOTE]  
    > Si va a instalar roles y características desde un servidor remoto, no necesita ejecutar Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.  
  
    -   En un servidor que ejecute la opción de instalación Server Core de Windows Server 2012 R2 o Windows Server 2012, escriba **PowerShell** en un símbolo del sistema y, a continuación, presione **entrar**.  
  
2.  Escriba el siguiente comando y, a continuación, presione **entrar**. En el siguiente ejemplo, los archivos de origen se encuentran en un almacén colateral (abreviado como **SxS**) en los medios de instalación de la unidad D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Si quiere que la línea de comandos use Windows Update como el origen de los archivos de características que faltan, o si ya se ha configurado un origen predeterminado mediante la directiva de grupo, no es necesario agregar el parámetro `Source` , a menos que desee especificar otro origen.  
  
### <a name="to-install-net-framework-35-by-using-the-add-roles-and-features-wizard"></a>Para instalar .NET Framework 3,5 mediante el Asistente para agregar roles y características  
  
1. En el menú **administrar** de administrador del servidor, haga clic en **Agregar roles y características**.  
  
2. Seleccione un servidor de destino que ejecute Windows Server 2016.  
  
3. En la página **seleccionar características** del Asistente para agregar roles y características, seleccione **.NET Framework 3,5**.  
  
4. Si la configuración de la directiva de grupo lo permite en el equipo local, el proceso de instalación intentará obtener los archivos de características que faltan mediante Windows Update. Haga clic en **Instalar**; no es necesario avanzar al siguiente paso.  
  
   Si directiva de grupo configuración no lo permite, o si desea usar otro origen para los archivos de características de .NET Framework 3,5, en la página **confirmar selecciones de instalación** del asistente, haga clic en **especifique una ruta de acceso de origen alternativa**.  
  
5. Proporcione una ruta de acceso a un almacén colateral (denominado **SxS**) en los medios de instalación o a un archivo WIM. En el siguiente ejemplo, los medios de instalación se encuentran en la unidad D.  
  
   **\\ D:\Sources\SxS**  
  
   Para especificar un archivo WIM, agregue un prefijo **WIM:** y agregue el índice de la imagen que se usará en el archivo WIM como sufijo, como se muestra en el siguiente ejemplo.  
  
   **Wim:\\\\** <em>SERVER_NAME</em> **\share\install.Wim: 3**  
  
6. Haga clic en **Aceptar**y, a continuación, en **Instalar**.  
  
### <a name="to-install-net-framework-35-by-using-dism"></a>Para instalar .NET Framework 3.5 mediante DISM  
  
1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.  
  
    > [!NOTE]  
    > Si va a instalar roles y características desde un servidor remoto, no necesita ejecutar Windows PowerShell con derechos de usuario elevados.  
  
    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.  
  
    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.  
  
    -   En un servidor que ejecute la opción de instalación Server Core, escriba **PowerShell** en un símbolo del sistema y, a continuación, presione **entrar**.  
  
2.  Ejecute uno de los siguientes comandos DISM.  
  
    -   Si el equipo tiene acceso a Windows Update, o si ya se ha configurado una ubicación de archivo de origen predeterminada en directiva de grupo, ejecute el siguiente comando.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Si el equipo tiene acceso a los medios de instalación, ejecute un comando similar al siguiente. En el siguiente ejemplo, los medios de instalación del sistema operativo se encuentran en la unidad D. El parámetro `LimitAccess` impide que el comando intente ponerse en contacto con Windows Update o con un servidor que ejecute WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > El comando DISM distingue mayúsculas de minúsculas.  
  
### <a name="configure-alternate-sources-for-feature-files-in-group-policy"></a>Configurar orígenes alternativos para los archivos de características en la directiva de grupo  
La configuración de la directiva de grupo descrita en esta sección especifica las ubicaciones de origen autorizadas para los archivos de .NET Framework 3.5 y otros archivos de características que se han quitado como parte de la configuración de características a petición. La configuración **de directiva especificar la configuración de instalación de componentes opcionales y reparación de componentes** se encuentra en la carpeta configuración del **equipo\Plantillas administrativas\sistema** en el editor de directiva de grupo de consola de administración de directivas de grupo o local.  
  
> [!NOTE]  
> Debe pertenecer al grupo Administradores para cambiar la configuración de la directiva de grupo en el equipo local. Si la configuración de la directiva de grupo en el equipo que desea administrar se controla en el nivel del dominio, debe pertenecer al grupo de administradores de dominio para modificarla.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Para configurar una ruta de acceso de origen alternativa predeterminada en la directiva de grupo  
  
1. En el editor de directiva de grupo local o Consola de administración de directivas de grupo, abra la siguiente configuración de directiva.  
  
   **configuración del Equipo\plantillas Administrativas\sistema\especificar configuración de instalación de componentes opcionales y reparación de componentes**  
  
2. Sselect **habilitada** para habilitar la configuración de Directiva, si aún no está habilitada.  
  
3. En el cuadro de texto **Ruta de acceso del archivo de origen alternativa**, en el área **Opciones**, especifique una ruta de acceso completa a una carpeta compartida o a un archivo WIM. Para especificar un archivo WIM como ubicación del archivo de origen alternativa, agregue el prefijo **WIM:** a la ruta de acceso y agregue el índice de la imagen que se usará en el archivo WIM como sufijo. Los siguientes son ejemplos de valores que puede especificar.  
  
   - Ruta de acceso a una carpeta compartida: **\\\\** <em>server_name</em> **\share\\** <em>folder_name</em>  
  
   - Ruta de acceso a un archivo WIM, donde **3** representa el índice de la imagen en la que se encuentran los archivos de características: **WIM:\\\\** <em>SERVER_NAME</em> **\share\install.Wim: 3**  
  
4. Si no desea que los equipos controlados por esta configuración de directiva busquen los archivos de características que faltan en Windows Update, seleccione no **intentar nunca descargar la carga desde Windows Update**.  
  
5. Si los equipos controlados por esta configuración de directiva suelen recibir actualizaciones a través de WSUS, pero prefiere usar Windows Update en lugar de WSUS para buscar los archivos de características que faltan, seleccione **Ponerse en contacto directamente con Windows Update para descargar contenido de reparaciones en lugar de usar Windows Server Update Services (WSUS)** .  
  
6. Haga clic en **Aceptar** cuando haya terminado de modificar esta configuración de directiva y, a continuación, cierre el Editor de directivas de grupo.  
  
## <a name="see-also"></a>Consulta también  
[Opciones de instalación de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Consideraciones de implementación de Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Cómo habilitar o deshabilitar características de Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


