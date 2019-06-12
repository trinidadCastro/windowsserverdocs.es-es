---
title: Administrador de servidores
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d996ef40-8bcc-42b0-b6ae-806b828223f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3f3abeec3d4ecbe5e80d08a99a00b43a408c4ac
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811287"
---
# <a name="server-manager"></a>Administrador de servidores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El administrador del servidor es una consola de administración en Windows Server que ayuda a disposición de los profesionales de TI y administrar los servidores locales y remotos basados en Windows desde sus escritorios, sin requerir acceso físico a los servidores o la necesidad de habilitar Escritorio remoto conexiones de protocolo (rdP) para cada servidor. Aunque el administrador del servidor está disponible en Windows Server 2008 R2 y Windows Server 2008, el administrador del servidor se actualizó en Windows Server 2012 para admitir la administración remota de varios servidor y ayudan a aumentar el número de servidores que puede administrar un administrador.

En nuestras pruebas, el administrador del servidor en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 puede utilizarse para administrar hasta 100 servidores, dependiendo de las cargas de trabajo que se ejecutan los servidores. El número de servidores que se pueden administrar con una sola consola del Administrador de servidores puede variar según la cantidad de datos que solicite de los servidores administrados así como los recursos de hardware y de red disponibles para el equipo que ejecuta el Administrador de servidores. Como la cantidad de datos que desea mostrar se acerca a la capacidad de los recursos del equipo, puede experimentar respuestas lentas del administrador de servidores y retrasos en la finalización de las actualizaciones. Para ayudar a aumentar el número de servidores que se pueden administrar mediante el Administrador de servidores, se recomienda limitar los datos de eventos que el Administrador de servidores obtiene de los servidores administrados utilizando la configuración del cuadro de diálogo **Configurar datos de eventos** . Configurar datos de eventos se puede abrir desde el menú **Tareas** del icono **Eventos** . Si tienes que administrar un número de nivel de empresa de servidores de su organización, se recomienda evaluar productos en la [suite Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).

En este tema y sus temas secundarios proporcionan información sobre cómo usar las características en la consola de administrador del servidor. En este tema se incluyen las siguientes secciones.

-   [Revisar Consideraciones iniciales y requisitos del sistema](#review-initial-considerations-and-system-requirements)

-   [Tareas que puede realizar en el administrador del servidor](#tasks-that-you-can-perform-in-server-manager)

-   [iniciar el administrador del servidor](#start-server-manager)

-   [Reiniciar servidores remotos](#restart-remote-servers)

-   [Exportar la configuración de administrador del servidor a otros equipos](#export-server-manager-settings-to-other-computers)

## <a name="review-initial-considerations-and-system-requirements"></a>Revisar consideraciones iniciales y requisitos del sistema
Las secciones siguientes enumeran algunas consideraciones iniciales que debe revisar, así como los requisitos de hardware y software para el administrador del servidor.

### <a name="hardware-requirements"></a>Requisitos de hardware
El administrador del servidor se instala de forma predeterminada con todas las ediciones de Windows Server 2016. No hay requisitos de hardware adicional existen para el administrador del servidor.

### <a name="software-and-configuration-requirements"></a>Requisitos de software y configuración
El administrador del servidor se instala de forma predeterminada con todas las ediciones de Windows Server 2016. Puede usar el administrador del servidor en Windows Server 2016 para administrar [opciones de instalación de Server Core](https://go.microsoft.com/fwlink/p/?LinkID=241573) de Windows Server 2016, Windows Server 2012 y Windows Server 2008 R2 que se ejecutan en equipos remotos. Ejecute el administrador del servidor en la opción de instalación Server Core de Windows Server 2016.

El administrador del servidor se ejecuta en la interfaz gráfica de servidor mínima; es decir, cuando la característica Server Graphical Shell no está instalada. La característica Server Graphical Shell no está instalada de forma predeterminada en Windows Server 2016. Si no está ejecutando Server Graphical Shell, el administrador del servidor se ejecuta la consola, pero algunas aplicaciones o herramientas disponibles en la consola no están disponibles. Los exploradores de Internet no pueden ejecutarse sin Server Graphical Shell, las páginas de Web es así y aplicaciones como ayuda HTML (por ejemplo, la Ayuda de mmc F1) no se puede abrir. No se puede abrir cuadros de diálogo para configurar actualizaciones automáticas de Windows y los comentarios cuando Server Graphical Shell no está instalado; los comandos que abren estos cuadros de diálogo en la consola de administrador del servidor se redirigen para ejecutar **sconfig.cmd**.

Para administrar servidores que ejecutan versiones de Windows Server anterior a Windows Server 2016, instale el siguiente software y actualizaciones para hacer que las versiones anteriores de Windows Server puedan administrar con el administrador del servidor en Windows Server 2016.

|Sistema operativo|Software necesario|
|----------|-----------|
| Windows Server 2012 R2 o Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058). El paquete de descarga de Windows Management Framework 5.0 actualiza los proveedores de Instrumental de administración de Windows (WMI) en Windows Server 2012 R2 y Windows Server 2012. Los proveedores de WMI actualizados permiten que el administrador del servidor recopilar información sobre los roles y características que están instaladas en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2012 R2 o Windows Server 2012 tiene un estado de manejabilidad **no está accesible**.<br />-La actualización de rendimiento asociado con [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) ya no es necesario en servidores que ejecutan Windows Server 2012 R2 o Windows Server 2012.|
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). El paquete de descarga de Windows Management Framework 4.0 actualiza los proveedores de Instrumental de administración de Windows (WMI) en Windows Server 2008 R2. Los proveedores de WMI actualizados permiten que el administrador del servidor recopilar información sobre los roles y características que están instaladas en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2008 R2 tienen un estado de manejabilidad **no está accesible**.<br />-La actualización de rendimiento asociado con [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite recopilar datos de rendimiento de Windows Server 2008 R2 que el administrador del servidor.|
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) paquete de descarga de The Windows Management Framework 3.0 actualiza los proveedores de Instrumental de administración de Windows (WMI) en Windows Server 2008. Los proveedores de WMI actualizados permiten que el administrador del servidor recopilar información sobre los roles y características que están instaladas en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2008 tienen un estado de manejabilidad **no accesible: Compruebe que las versiones anteriores ejecutan Windows Management Framework 3.0**.<br />-La actualización de rendimiento asociado con [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite recopilar datos de rendimiento de Windows Server 2008 que el administrador del servidor.|

#### <a name="manage-remote-computers-from-a-client-computer"></a>Administrar equipos remotos desde un equipo cliente
Se incluye con la consola de administrador del servidor [herramientas de administración remota del servidor](https://go.microsoft.com/fwlink/?LinkID=404281) para Windows 10. Tenga en cuenta que, cuando se instala herramientas de administración remota en un equipo cliente, no puede administrar el equipo local mediante el administrador del servidor; El administrador del servidor no se puede usar para administrar equipos o dispositivos que ejecutan un sistema operativo de cliente de Windows. Sólo se puede utilizar el administrador del servidor para administrar servidores basados en Windows.

|Sistema operativo del administrador del servidor de origen|Dirigido a Windows Server 2016|Dirigido a Windows Server 2012 R2 |Dirigido a Windows Server 2012 |Dirigido a Windows Server 2008 R2 o Windows Server 2008 |Dirigido a Windows Server 2003|
|-------------------------------|--------------------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 10 o Windows Server 2016|Compatibilidad completa|Compatibilidad completa|Compatibilidad completa|Tras cumplir los [Requisitos de software y configuración](#software-and-configuration-requirements) , puedes realizar la mayoría de las tareas de administración, pero no puedes instalar ni desinstalar roles o características.|No se admite|
|Windows 8.1 o Windows Server 2012 R2 |No se admite|Compatibilidad completa|Compatibilidad completa|Tras cumplir los [Requisitos de software y configuración](#software-and-configuration-requirements) , puedes realizar la mayoría de las tareas de administración, pero no puedes instalar ni desinstalar roles o características.|Compatibilidad limitada; solo estados con y sin conexión|
|Windows 8 o Windows Server 2012 |No se admite|No se admite|Compatibilidad completa|Tras cumplir los [Requisitos de software y configuración](#software-and-configuration-requirements) , puedes realizar la mayoría de las tareas de administración, pero no puedes instalar ni desinstalar roles o características.|Compatibilidad limitada; solo estados con y sin conexión|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar el Administrador del servidor en un equipo cliente

1.  Siga las instrucciones de [herramientas de administración remota del servidor](../../remote/remote-server-administration-tools.md) instalar remoto Server Administration Tools for Windows 10.

2.  En el **iniciar** pantalla, haga clic en **administrador del servidor**. El icono de **Administrador del servidor** está disponible después de instalar las Herramientas de administración remota del servidor.

3.  Si no la **herramientas administrativas** ni **administrador del servidor** los iconos se muestran en el **iniciar** pantalla después de instalar herramientas de administración remota, y Buscar para el administrador del servidor en el **iniciar** pantalla no muestra resultados, compruebe que la **Mostrar herramientas administrativas** esté activada. Para ver esta configuración, mantenga el cursor del mouse en la esquina superior derecha de la **iniciar** pantalla y, a continuación, haga clic en **configuración**. Si la opción **Mostrar herramientas administrativas** está desactivada, actívala para mostrar las herramientas que instalaste como parte de las Herramientas de administración remota del servidor.

Para obtener más información acerca de la ejecución remota Server Administration Tools para Windows 10 para administrar servidores remotos, vea [herramientas de administración remota del servidor](https://go.microsoft.com/fwlink/?LinkID=221055) en TechNet Wiki.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurar la administración remota en los servidores que desea administrar

> [!IMPORTANT]
> De forma predeterminada, el administrador del servidor y Windows PowerShell de administración remota está habilitada en Windows Server 2016.

Para llevar a cabo tareas de administración en servidores remotos mediante el administrador del servidor, los servidores remotos que desea administrar deben configurarse para permitir la administración remota mediante el administrador del servidor y Windows PowerShell. Si se ha deshabilitado la administración remota en Windows Server 2012 R2 o Windows Server 2012, y desea volver a habilitarla, realice los pasos siguientes.

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a>Para configurar la administración remota del administrador del servidor en Windows Server 2012 R2 o Windows Server 2012 mediante el uso de la interfaz de Windows

1.  > [!NOTE]
    > La configuración que se controla mediante el **configurar la administración remota** cuadro de diálogo no afectan a las partes del administrador del servidor que usan DCOM para las comunicaciones remotas.

    Realice una de las siguientes acciones para abrir el administrador del servidor si no está ya abierto.

    -   En la barra de tareas de Windows, haga clic en el botón Administrador del servidor.

    -   En el **iniciar** pantalla, haga clic en **administrador del servidor**.

2.  En el **propiedades** área de la **servidores locales** página, haga clic en el valor con hipervínculo para el **administración remota** propiedad.

3.  Realice una de las siguientes acciones y haga clic en **Aceptar**.

    -   Para evitar que este equipo se administre de forma remota mediante el administrador del servidor (o Windows PowerShell si está instalado), desactive la **habilitar la administración remota de este servidor desde otros equipos** casilla de verificación.

    -   Para permitir que este equipo a administrarse de forma remota mediante el administrador del servidor o Windows PowerShell, seleccione **habilitar la administración remota de este servidor desde otros equipos**.

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a>Para habilitar la administración remota del administrador del servidor en Windows Server 2012 R2 o Windows Server 2012 mediante Windows PowerShell

1.  Realice una de las siguientes acciones:

    -   Para ejecutar Windows PowerShell como administrador desde el **iniciar** pantalla, haga clic en el **Windows PowerShell** icono y, a continuación, haga clic en **ejecutar como administrador**.

    -   Para ejecutar Windows PowerShell como administrador desde el escritorio, haga clic en el **Windows PowerShell** acceso directo en la barra de tareas y, a continuación, haga clic en **ejecutar como administrador**.

2.  Escriba lo siguiente y, a continuación, presione **ENTRAR** habilitar todas las excepciones de reglas de firewall necesarias.

    **Configure-SMremoting.exe -Enable**

    > [!NOTE]
    > Este comando también funciona en un símbolo del sistema que se ha abierto con permisos de usuario elevados (Ejecutar como administrador)

    Si no se puede habilitar la administración remota, consulte [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) en Microsoft TechNet para solucionar problemas de sugerencias y prácticas recomendadas.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Para habilitar la administración remota de Windows PowerShell y el Administrador del servidor en versiones anteriores de los sistemas operativos

-   Realice una de las siguientes acciones:

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008 R2, consulte [con el administrador del servidor de administración remota](https://go.microsoft.com/fwlink/?LinkID=137378) en la Ayuda de Windows Server 2008 R2.

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008, consulte [habilitar y usar comandos remotos en Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

## <a name="tasks-that-you-can-perform-in-server-manager"></a>Tareas que puede realizar con el Administrador del servidor
Administrador del servidor aporta la administración de servidor más eficaz, ya que permite a los administradores realizar tareas en la siguiente tabla con una única herramienta. En Windows Server 2012 R2 y Windows Server 2012, los usuarios estándares de un servidor y los miembros del grupo administradores pueden realizar tareas de administración en el administrador del servidor, pero de forma predeterminada, los usuarios estándar no pueden realizar algunas tareas, como se muestra en el tabla siguiente.

Los administradores pueden usar dos cmdlets de Windows PowerShell en el módulo de cmdlets de administrador del servidor, [Enable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) y [Disable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx)a controlar aún más el acceso de usuario estándar a algunos datos adicionales. El **Enable-ServerManagerStandardUserremoting** cmdlet puede proporcionar uno o más acceso de los usuarios estándar sin privilegios de administrador a eventos, servicios, el contador de rendimiento y datos de inventario de roles y características.

> [!IMPORTANT]
> El Administrador de servidores no se puede usar para administrar una versión más reciente del sistema operativo Windows Server. Administrador del servidor que se ejecutan en Windows Server 2012 o Windows 8 no se puede usar para administrar servidores que ejecutan Windows Server 2012 R2.

|Descripción de la tarea|Administradores (incluida la cuenta predefinida de administrador)|Usuarios estándar del servidor|
|----------|----------------------------------|-------------|
|Agregar servidores remotos a un grupo de servidores que el administrador del servidor se pueden usar para administrar.|Sí|No|
|crear y editar grupos personalizados de servidores, como los servidores que están en una ubicación geográfica específica o para un propósito específico.|Sí|Sí|
|Instalar o desinstalar roles, servicios de rol y características en el equipo local o en servidores remotos que ejecutan Windows Server 2012 R2 o Windows Server 2012. Para obtener definiciones de roles, servicios de rol y características, vea [Roles, servicios de rol y características](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Sí|No|
|Ver y modificar los roles y características de servidor instalados en los servidores locales o remotos. **Nota:** En el administrador del servidor, roles y características de datos se muestran en el idioma base del sistema, también denominado el idioma predeterminado de la interfaz gráfica de usuario o el idioma seleccionado durante la instalación del sistema operativo.|Sí|Los usuarios estándar pueden ver y administrar roles y características y realizar tareas como ver eventos de rol, pero no pueden agregar ni quitar servicios de rol.|
|iniciar las herramientas de administración, como Windows PowerShell o complementos mmc. Puede iniciar una sesión de Windows PowerShell dirigida a un servidor remoto haciendo clic con el servidor en el **servidores** icono y, a continuación, haga clic en **Windows PowerShell**. Puede iniciar los complementos mmc desde el **herramientas** menú de la consola de administrador del servidor y luego dirigir la mmc hacia un equipo remoto después el complemento está abierto.|Sí|Sí|
|Administrar servidores remotos con distintas credenciales; para ello, debe hacer clic con el botón secundario en el icono **Servidores** y, a continuación, hacer clic en **Administrar como**. Puede usar **Administrar como** para tareas de administración generales del servidor y de Servicios de archivos y almacenamiento.|Sí|No|
|Realizar tareas de administración asociadas con el ciclo de vida operativo de servidores, como iniciar o detener servicios; e iniciar otras herramientas que le permiten configurar un servidor de la configuración de red, usuarios y grupos y conexiones de escritorio remoto.|Sí|Los usuarios estándar no pueden iniciar ni detener servicios. Se pueden cambiar el nombre del servidor local, workgroup, o pertenencia al dominio y la configuración de escritorio remoto, pero se le solicite en el Control de cuentas de usuario para proporcionar las credenciales de administrador antes de que puede completar estas tareas. No pueden cambiar la configuración de administración remota.|
|Realizar tareas de administración asociadas al ciclo de vida operativo de los roles que están instalados en los servidores, incluido el examen de roles para garantizar el cumplimiento de los procedimientos recomendados.|Sí|Los usuarios estándar no pueden ejecutar exámenes del analizador de procedimientos recomendados.|
|Determinar el estado del servidor, identificar eventos críticos, y analizar y solucionar problemas o errores de configuración.|Sí|Sí|
|Personalizar los eventos, datos de rendimiento, servicios y los resultados de Best Practices Analyzer sobre la que desea recibir alertas en el panel Administrador del servidor.|Sí|Sí|
|Reiniciar los servidores.|Sí|No|
|Actualizar los datos que se muestran en la consola de administrador del servidor acerca de los servidores administrados.|Sí|No|

> [!NOTE]
> No se puede usar el administrador del servidor para agregar roles y características a servidores que ejecutan Windows Server 2008 R2 o Windows Server 2008.

## <a name="start-server-manager"></a>iniciar el administrador del servidor
El administrador del servidor se inicia automáticamente de forma predeterminada en los servidores que ejecutan Windows Server 2016, cuando un miembro del grupo Administradores inicia sesión en un servidor. Si cierra el administrador del servidor, reinícielo de una de las maneras siguientes. En esta sección también contiene pasos para cambiar el comportamiento predeterminado e impide que el administrador del servidor se inicie automáticamente.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Para iniciar el administrador del servidor desde la pantalla Inicio

-   En el Windows **iniciar** pantalla, haga clic en el **administrador del servidor** icono.

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Para iniciar el Administrador del servidor desde el escritorio de Windows

-   En la barra de tareas de Windows, haga clic en **Administrador del servidor**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Para impedir que el Administrador del servidor se inicie automáticamente

1.  En la consola de administrador del servidor, en el **administrar** menú, haga clic en **propiedades del administrador del servidor**.

2.  En el cuadro de diálogo **Propiedades del Administrador del servidor** , active la casilla de **No iniciar el Administrador del servidor automáticamente al iniciar sesión**. Haga clic en **Aceptar**.

3.  Como alternativa, puede impedir que el administrador del servidor inicie automáticamente si habilita la configuración de directiva de grupo **no iniciar el administrador del servidor automáticamente al inicio de sesión**. La ruta de acceso a esta configuración de directiva, en la consola de editor de directiva de grupo Local es configuración del equipo\Plantillas Administrativas\sistema\administrador del equipo.

## <a name="restart-remote-servers"></a>Reiniciar servidores remotos
Puede reiniciar un servidor remoto desde el **servidores** mosaico de una página de rol o grupo en Administrador del servidor.

> [!IMPORTANT]
> El reinicio de un servidor remoto fuerza el reinicio del servidor, aunque todavía haya usuarios con sesión iniciada en el servidor remoto y programas abiertos con datos no guardados. Este comportamiento se diferencia del apagado o reinicio del equipo local, en el que se le solicita que guarde los datos de programas que todavía no haya guardado y que compruebe que desea forzar el cierre de sesión de los usuarios que tienen una sesión iniciada. Asegúrese de que puede forzar a los otros usuarios a cerrar sesión en los servidores remotos y descartar los datos no guardados en los programas que se ejecutan en los servidores remotos.
> 
> Si se realiza una actualización automática en el administrador del servidor mientras que un servidor administrado es apagar y reiniciar, pueden que se produzcan errores del servidor administrado, porque el administrador del servidor no se puede conectar al servidor remoto hasta que haya terminado de estado de actualización y administración reiniciando.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Para reiniciar los servidores remotos en el Administrador del servidor

1.  Abra una página principal de grupo rol o el servidor en el administrador del servidor.

2.  Seleccione uno o varios servidores remotos que se han agregado al administrador del servidor. Mantenga presionado **Ctrl** mientras hace clic para para seleccionar varios servidores al mismo tiempo. Para obtener más información acerca de cómo agregar servidores al grupo de servidores de administrador del servidor, consulte [agregar servidores al administrador del servidor](add-servers-to-server-manager.md).

3.  Haga clic con el botón secundario en los servidores seleccionados y, a continuación, haga clic en **Reiniciar servidor**.

## <a name="export-server-manager-settings-to-other-computers"></a>Exportar la configuración del Administrador del servidor a otros equipos
En el administrador del servidor, la lista de servidores administrados, se cambia a la configuración de la consola de administrador del servidor y los grupos personalizados que haya creado se almacenan en los dos archivos siguientes. Puede reutilizar esta configuración en otros equipos que ejecutan la misma versión del administrador del servidor (o Windows 10 con herramientas de administración de servidor remoto instalado). Herramientas de administración remota del servidor debe ejecutarse en equipos basados en cliente de Windows para exportar la configuración de administrador del servidor en dichos equipos.

-   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

-   %*appdata*%\Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   Las credenciales de Administrar como (o alternativas) de los servidores del grupo de servidores no se almacenan en el perfil móvil. Los usuarios del Administrador de servidores deben agregarlas en cada equipo desde el que desean administrar.
> -   El perfil móvil del recurso compartido de red no se crea hasta que un usuario inicia sesión en la red y luego la cierra por primera vez. El archivo **Serverlist.xml** se crea en ese momento.

Puede exportar la configuración del administrador del servidor, que la configuración del administrador del servidor sea portable o usarlos en otros equipos en una de las dos maneras siguientes.

-   Para exportar la configuración a otro equipo unido al dominio, configure el usuario de administrador del servidor para que tenga un perfil móvil en active directory a los usuarios y equipos. Debe ser un administrador de dominio para cambiar las propiedades de usuario en active directory a los usuarios y equipos.

-   Para exportar la configuración a otro equipo en un grupo de trabajo, copie los dos archivos anteriores a la misma ubicación en el equipo desde el que desea administrar mediante el administrador del servidor.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Para exportar la configuración del Administrador del servidor a otros equipos unidos al dominio

1.  En usuarios de active directory y los equipos, abra el **propiedades** cuadro de diálogo para un usuario de administrador del servidor.

2.  En el **perfil** pestaña, agregue una ruta de acceso a un recurso compartido de red para almacenar el perfil del usuario.

3.  Realice una de las siguientes acciones:

    -   En Estados Unidos Inglés (en-us) compilaciones, cambia a la **Serverlist.xml** archivo se guardan automáticamente en el perfil. Vaya al paso siguiente.

    -   En otras compilaciones, copie los dos archivos siguientes desde el equipo que ejecuta el administrador del servidor para el recurso compartido de red que forma parte del perfil móvil del usuario.

        -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  Haga clic en **Aceptar** para guardar los cambios y cerrar el cuadro de diálogo **Propiedades** .

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Para exportar la configuración del Administrador del servidor a equipos de grupos de trabajo

-   En un equipo desde el que desea administrar servidores remotos, sobrescriba los dos archivos siguientes con los mismos archivos desde otro equipo que ejecuta el administrador del servidor, y que tenga la configuración que desee.

    -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config



