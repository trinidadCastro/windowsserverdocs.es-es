---
title: Administrar varios servidores remotos con Administrador del servidor
description: Administrador de servidores
ms.topic: article
ms.assetid: 3a17e686-e7f2-47e2-b7af-733777c38b5f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c85e9bd4525cc40ddc7e5c77aacb9fd2ab9cf124
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627837"
---
# <a name="manage-multiple-remote-servers-with-server-manager"></a>Administrar varios servidores remotos con Administrador del servidor

>Se aplica a: Windows Server 2016

Administrador del servidor es una consola de administración de Windows Server 2012 R2 y Windows Server 2012 que ayuda a los profesionales de ti a aprovisionar y administrar servidores basados en Windows, tanto locales como remotos, desde sus escritorios, sin necesidad de tener acceso físico a los servidores ni de habilitar conexiones de protocolo de Escritorio remoto (rdP) a cada servidor. Aunque Administrador del servidor está disponible en Windows Server 2008 R2 y Windows Server 2008, Administrador del servidor se actualizó en Windows Server 2012, para admitir la administración remota de varios servidores y ayudar a aumentar el número de servidores que puede administrar un administrador.

En nuestras pruebas, Administrador del servidor en Windows Server 2012 R2 y Windows Server 2012 se pueden usar para administrar hasta 100 servidores que estén configurados con una carga de trabajo típica. El número de servidores que puede administrar mediante una única consola de Administrador del servidor puede variar en función de la cantidad de datos que solicite a los servidores administrados y de los recursos de hardware y de red disponibles para el equipo que ejecuta Administrador del servidor. A medida que la cantidad de datos que desea mostrar se aproxima a la capacidad de recursos del equipo, puede experimentar respuestas lentas de Administrador del servidor y retrasos en la finalización de las actualizaciones. Para ayudar a aumentar el número de servidores que puede administrar mediante Administrador del servidor, se recomienda limitar los datos de evento que Administrador del servidor obtiene de los servidores administrados, mediante el uso de la configuración del cuadro de diálogo **configurar datos de eventos** . Configurar datos de eventos se puede abrir desde el menú **Tareas** del icono **Eventos**. Si necesita administrar un número de servidores de nivel empresarial en su organización, se recomienda evaluar los productos del conjunto de [Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).

En este tema y sus temas secundarios se proporciona información sobre cómo usar las características de en la consola de Administrador del servidor. En este tema se incluyen las siguientes secciones.

-   [Revisar consideraciones iniciales y requisitos del sistema](#BKMK_1.1)

-   [Tareas que puede realizar con el Administrador del servidor](#BKMK_tasks)

-   [Inicio Administrador del servidor](#BKMK_start)

-   [Reiniciar servidores remotos](#BKMK_restart)

-   [Exportar la configuración del Administrador del servidor a otros equipos](#BKMK_export)

## <a name="review-initial-considerations-and-system-requirements"></a><a name=BKMK_1.1></a>Revisar consideraciones iniciales y requisitos del sistema
En las secciones siguientes se enumeran algunas consideraciones iniciales que debe revisar, así como los requisitos de hardware y software para Administrador del servidor.

### <a name="hardware-requirements"></a>Requisitos de hardware
Administrador del servidor se instala de manera predeterminada con todas las ediciones de Windows Server 2012 R2 y Windows Server 2012. No existen requisitos de hardware adicionales para Administrador del servidor.

### <a name="software-and-configuration-requirements"></a><a name=BKMK_softconfig></a>Requisitos de software y configuración
Administrador del servidor se instala de manera predeterminada con todas las ediciones de Windows Server 2012. Aunque puede usar Administrador del servidor para administrar [las opciones de instalación de Server Core](https://go.microsoft.com/fwlink/p/?LinkID=241573) de windows Server 2012 y windows Server 2008 R2 que se ejecutan en equipos remotos, administrador del servidor no se ejecuta directamente en las opciones de instalación de Server Core.

Para administrar completamente los servidores remotos que ejecutan Windows Server 2008 o Windows Server 2008 R2, instale las siguientes actualizaciones en los servidores que desea administrar, en el orden mostrado.

Para administrar servidores que ejecutan Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 mediante Administrador del servidor en Windows Server 2012 R2, aplique las siguientes actualizaciones a los sistemas operativos anteriores.

-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)

-   [Windows Management Framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881). Windows Management Framework 4,0 descargar los proveedores de actualizaciones de paquetes Instrumental de administración de Windows (WMI) en Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008. Los proveedores de WMI actualizados permiten a Administrador del servidor recopilar información acerca de los roles y las características que están instalados en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 tienen un estado de capacidad de administración de **no accesible**.

-   La actualización de rendimiento asociada con el [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite a administrador del servidor recopilar datos de rendimiento de windows Server 2008 y windows Server 2008 R2. Esta actualización de rendimiento no es necesaria en los servidores que ejecutan Windows Server 2012.

Para administrar los servidores que ejecutan Windows Server 2008 R2 o Windows Server 2008, aplique las siguientes actualizaciones a los sistemas operativos anteriores.

-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)

-   [Windows Management Framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) La descarga de Windows Management Framework 3,0 proveedores de actualizaciones de paquetes Instrumental de administración de Windows (WMI) en Windows Server 2008 y Windows Server 2008 R2. Los proveedores de WMI actualizados permiten a Administrador del servidor recopilar información acerca de los roles y las características que están instalados en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2008 o Windows Server 2008 R2 tienen un estado de capacidad de administración de **no accesible: Compruebe que las versiones anteriores ejecutan Windows Management Framework 3,0**.

-   La actualización de rendimiento asociada con el [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite a administrador del servidor recopilar datos de rendimiento de windows Server 2008 y windows Server 2008 R2.

Administrador del servidor se ejecuta en la interfaz gráfica de servidor mínima; es decir, cuando se ha desinstalado la característica Shell gráfico de servidor. La característica Shell gráfico de servidor se instala de forma predeterminada en Windows Server 2012 R2 y Windows Server 2012. Si desinstala el shell gráfico de servidor, la consola de Administrador del servidor se ejecuta, pero algunas aplicaciones o herramientas disponibles desde la consola no están disponibles. Los exploradores de Internet no se pueden ejecutar sin el shell gráfico de servidor, por lo que no se pueden abrir páginas web y aplicaciones como la ayuda HTML (la ayuda F1 de MMC, por ejemplo). No puede abrir cuadros de diálogo para configurar las actualizaciones automáticas de Windows y los comentarios cuando el shell gráfico de servidor no está instalado. los comandos que abren estos cuadros de diálogo en la consola de Administrador del servidor se redirigen para ejecutar **sconfig. cmd**.

#### <a name="manage-remote-computers-from-a-client-computer"></a>Administrar equipos remotos desde un equipo cliente
La consola de Administrador del servidor se incluye con [herramientas de administración remota del servidor](https://go.microsoft.com/fwlink/?LinkID=304145) para Windows 8.1 y [herramientas de administración remota del servidor](https://go.microsoft.com/fwlink/p/?LinkID=238560) para Windows 8. Tenga en cuenta que cuando Herramientas de administración remota del servidor está instalado en un equipo cliente, no puede administrar el equipo local mediante Administrador del servidor; Administrador del servidor no se pueden usar para administrar equipos o dispositivos que ejecutan un sistema operativo de cliente de Windows. Puede usar Administrador del servidor para administrar solo servidores basados en Windows.

|Administrador del servidor sistema operativo de origen|Destinado a Windows Server 2012 R2 |Destinado a Windows Server 2012 |Destinado a Windows Server 2008 R2 o Windows Server 2008 |Dirigido a Windows Server 2003|
|-------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 8 o Windows Server 2012 |No compatible|Compatibilidad completa|Tras cumplir los [requisitos de software y configuración](#BKMK_softconfig), puede realizar la mayoría de las tareas de administración, pero no puede instalar ni desinstalar roles o características.|Compatibilidad limitada; solo estados con y sin conexión|
|Windows 8.1 o Windows Server 2012 R2 |Compatibilidad completa|Compatibilidad completa|Tras cumplir los [requisitos de software y configuración](#BKMK_softconfig), puede realizar la mayoría de las tareas de administración, pero no puede instalar ni desinstalar roles o características.|Compatibilidad limitada; solo estados con y sin conexión|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar el Administrador del servidor en un equipo cliente

1.  Siga las instrucciones de [implementar herramientas de administración remota del servidor](https://go.microsoft.com/fwlink/?LinkID=238562) para instalar Herramientas de administración remota del servidor para Windows 8.1 o herramientas de administración remota del servidor para Windows 8.

2.  En la pantalla **Inicio** , haga clic en **Administrador del servidor**. El icono de **Administrador del servidor** está disponible después de instalar las Herramientas de administración remota del servidor.

3.  Si no se muestran ni los iconos **herramientas administrativas** ni **Administrador del servidor** en la pantalla **Inicio** después de instalar herramientas de administración remota del servidor y la búsqueda de administrador del servidor en la pantalla **Inicio** no muestra resultados, compruebe que la opción **Mostrar herramientas administrativas** está activada. Para ver esta configuración, mantenga el cursor del mouse sobre la esquina superior derecha de la pantalla **Inicio** y, a continuación, haga clic en **configuración**. Si la opción **Mostrar herramientas administrativas** está desactivada, actívala para mostrar las herramientas que instalaste como parte de las Herramientas de administración remota del servidor.

para obtener más información acerca de cómo ejecutar Herramientas de administración remota del servidor para Windows 8 con el fin de administrar servidores remotos, consulte [herramientas de administración remota del servidor](https://go.microsoft.com/fwlink/?LinkID=221055) en el sitio wiki de TechNet.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurar la administración remota en los servidores que desea administrar

> [!IMPORTANT]
> De forma predeterminada, Administrador del servidor y la administración remota de Windows PowerShell está habilitada en Windows Server 2012 R2 y Windows Server 2012.

Para realizar tareas de administración en servidores remotos mediante Administrador del servidor, los servidores remotos que desee administrar deben estar configurados para permitir la administración remota mediante Administrador del servidor y Windows PowerShell. Si la administración remota se ha deshabilitado en Windows Server 2012 R2 o Windows Server 2012 y desea habilitarla de nuevo, realice los pasos siguientes.

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a><a name=BKMK_windows></a>Para configurar la administración remota de Administrador del servidor en Windows Server 2012 R2 o Windows Server 2012 mediante la interfaz de Windows

1.  > [!NOTE]
    > La configuración que se controla mediante el cuadro de diálogo **Configurar administración remota** no afecta a las partes de administrador del servidor que usan DCOM para las comunicaciones remotas.

    Realice una de las siguientes acciones para abrir Administrador del servidor si aún no está abierto.

    -   En la barra de tareas de Windows, haga clic en el botón Administrador del servidor.

    -   En la pantalla **Inicio** , haga clic en **Administrador del servidor**.

2.  En el área **propiedades** de la página **servidores locales** , haga clic en el valor con hipervínculo de la propiedad **administración remota** .

3.  Realice una de las siguientes acciones y haga clic en **Aceptar**.

    -   Para evitar que este equipo se administre de forma remota mediante Administrador del servidor (o Windows PowerShell si está instalado), desactive la casilla **Habilitar la administración remota de este servidor desde otros equipos** .

    -   Para permitir que este equipo se administre de forma remota mediante Administrador del servidor o Windows PowerShell, seleccione **Habilitar administración remota de este servidor desde otros equipos**.

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a><a name=BKMK_ps></a>Para habilitar Administrador del servidor la administración remota en Windows Server 2012 R2 o Windows Server 2012 mediante Windows PowerShell

1.  Realice una de las acciones siguientes.

    -   Para ejecutar Windows PowerShell como administrador desde la pantalla **Inicio** , haga clic con el botón derecho en el icono de **Windows PowerShell** y, a continuación, haga clic en **Ejecutar como administrador**.

    -   Para ejecutar Windows PowerShell como administrador desde el escritorio, haga clic con el botón secundario en el acceso directo de **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.

2.  Escriba lo siguiente y, a continuación, presione **entrar** para habilitar todas las excepciones de reglas de Firewall necesarias.

    **Configure-SMremoting.exe: habilitar**

    > [!NOTE]
    > Este comando también funciona en un símbolo del sistema que se ha abierto con permisos de usuario elevados (Ejecutar como administrador)

    Si se produce un error al habilitar la administración remota, consulte [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) en Microsoft TechNet para obtener sugerencias de solución de problemas y procedimientos recomendados.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Para habilitar la administración remota de Windows PowerShell y el Administrador del servidor en versiones anteriores de los sistemas operativos

-   Realice una de las acciones siguientes.

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008 R2, consulte [administración remota con administrador del servidor](https://go.microsoft.com/fwlink/?LinkID=137378) en la ayuda de windows Server 2008 R2.

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008, vea [habilitar y usar comandos remotos en Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2003, habilite las excepciones DCOM de WMI en Firewall de Windows. Para obtener más información sobre cómo hacer esto en servidores que ejecutan Windows Server 2003, consulta [Conexión a través de Firewall de Windows](/windows/win32/wmisdk/connecting-to-wmi-remotely-with-vbscript) en MSDN.

## <a name="tasks-that-you-can-perform-in-server-manager"></a><a name=BKMK_tasks></a>Tareas que puede realizar con el Administrador del servidor
Administrador del servidor hace que la administración del servidor sea más eficaz, ya que permite a los administradores realizar tareas en la tabla siguiente mediante una sola herramienta. En Windows Server 2012 R2 y Windows Server 2012, tanto los usuarios estándar de un servidor como los miembros del grupo administradores pueden realizar tareas de administración en Administrador del servidor, pero, de forma predeterminada, los usuarios estándar no pueden realizar algunas tareas, como se muestra en la tabla siguiente.

Los administradores pueden usar dos cmdlets de Windows PowerShell en el módulo de cmdlet de Administrador del servidor, [enable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) y [Disable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx), para controlar aún más el acceso de usuarios estándar a algunos datos adicionales. El cmdlet **enable-ServerManagerStandardUserremoting** puede proporcionar a uno o varios usuarios estándar y no administradores acceso a los datos de inventario de eventos, servicios, contadores de rendimiento y roles y características.

> [!IMPORTANT]
> Administrador del servidor no se puede usar para administrar una versión más reciente del sistema operativo Windows Server. Administrador del servidor que se ejecutan en Windows Server 2012 o Windows 8 no se puede usar para administrar servidores que ejecutan Windows Server 2012 R2.

|Descripción de la tarea|Administradores (incluida la cuenta predefinida de administrador)|Usuarios estándar del servidor|
|----------|----------------------------------|-------------|
|agregar servidores remotos a un grupo de servidores que Administrador del servidor puede usar para administrar.|Sí|No|
|crear y editar grupos de servidores personalizados, como los servidores que se encuentran en una ubicación geográfica específica o que sirven para un fin específico.|Sí|Sí|
|Instalar o desinstalar roles, servicios de rol y características en los servidores locales o remotos que ejecutan Windows Server 2012 R2 o Windows Server 2012. Para ver las definiciones de roles, servicios de rol y características, vea [roles, servicios de rol y características](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Sí|No|
|Ver y modificar los roles y características de servidor instalados en los servidores locales o remotos. **Nota:** En Administrador del servidor, los datos de roles y características se muestran en el idioma base del sistema, también denominado idioma predeterminado de la GUI del sistema, o el idioma seleccionado durante la instalación del sistema operativo.|Sí|Los usuarios estándar pueden ver y administrar roles y características y realizar tareas como ver eventos de rol, pero no pueden agregar ni quitar servicios de rol.|
|Inicie herramientas de administración como Windows PowerShell o los complementos MMC. Puede iniciar una sesión de Windows PowerShell destinada a un servidor remoto si hace clic con el botón derecho en el servidor en el icono **servidores** y luego hace clic en **Windows PowerShell**. Puede iniciar los complementos MMC desde el menú **herramientas** de la consola de administrador del servidor y, a continuación, dirigir la MMC hacia un equipo remoto después de abrir el complemento.|Sí|Sí|
|Administrar servidores remotos con distintas credenciales; para ello, debe hacer clic con el botón secundario en el icono **Servidores** y, a continuación, hacer clic en **Administrar como**. Puede usar **Administrar como** para tareas de administración generales del servidor y de Servicios de archivos y almacenamiento.|Sí|No|
|Realizar tareas de administración asociadas al ciclo de vida operativo de los servidores, como iniciar o detener servicios; e inician otras herramientas que le permiten configurar las opciones de red, los usuarios y los grupos de un servidor y las conexiones Escritorio remoto.|Sí|Los usuarios estándar no pueden iniciar ni detener servicios. Pueden cambiar el nombre del servidor local, el grupo de trabajo o la pertenencia al dominio y la configuración de Escritorio remoto, pero el control de cuentas de usuario les solicita que proporcionen credenciales de administrador para poder completar estas tareas. No pueden cambiar la configuración de administración remota.|
|Realizar tareas de administración asociadas al ciclo de vida operativo de los roles que están instalados en los servidores, incluido el examen de roles para garantizar el cumplimiento de los procedimientos recomendados.|Sí|Los usuarios estándar no pueden ejecutar exámenes de Analizador de procedimientos recomendados.|
|Determinar el estado del servidor, identificar eventos críticos, y analizar y solucionar problemas o errores de configuración.|Sí|Sí|
|Personalizar los eventos, datos de rendimiento, servicios y Analizador de procedimientos recomendados resultados sobre los que desea recibir alertas en el panel de Administrador del servidor.|Sí|Sí|
|Reiniciar los servidores.|Sí|No|
|Actualice los datos que se muestran en la consola de Administrador del servidor acerca de los servidores administrados.|Sí|No|

> [!NOTE]
> El Administrador del servidor solo puede recibir el estado con o sin conexión de los servidores que ejecutan Windows Server 2003. Administrador del servidor no se puede usar para agregar roles y características a servidores que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="start-server-manager"></a><a name=BKMK_start></a>Inicio Administrador del servidor
Administrador del servidor se inicia automáticamente de forma predeterminada en los servidores que ejecutan Windows Server 2012 cuando un miembro del grupo administradores inicia sesión en un servidor. Si cierra Administrador del servidor, reinícielo de una de las siguientes maneras. Esta sección también contiene pasos para cambiar el comportamiento predeterminado e impedir que Administrador del servidor se inicie automáticamente.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Para iniciar Administrador del servidor desde la pantalla Inicio

-   En la pantalla **Inicio** de Windows, haga clic en el icono de **Administrador del servidor** .

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Para iniciar el Administrador del servidor desde el escritorio de Windows

-   En la barra de tareas de Windows, haga clic en **Administrador del servidor**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Para impedir que el Administrador del servidor se inicie automáticamente

1.  En la consola de Administrador del servidor, en el menú **administrar** , haga clic en **propiedades de administrador del servidor**.

2.  En el cuadro de diálogo **Propiedades del Administrador del servidor**, active la casilla de **No iniciar el Administrador del servidor automáticamente al iniciar sesión**. Haga clic en **OK**.

3.  Como alternativa, puede impedir que Administrador del servidor se inicie automáticamente si habilita la configuración de directiva de grupo, **no inicia administrador del servidor automáticamente al iniciar sesión**. La ruta de acceso a esta configuración de Directiva, en la consola del editor de directiva de grupo local, es configuración del Equipo\plantillas Administrativas\sistema\administrador del servidor Manager.

## <a name="restart-remote-servers"></a><a name=BKMK_restart></a>Reiniciar servidores remotos
Puede reiniciar un servidor remoto desde el icono **servidores** de una página de rol o grupo de administrador del servidor.

> [!IMPORTANT]
> El reinicio de un servidor remoto fuerza el reinicio del servidor, aunque todavía haya usuarios con sesión iniciada en el servidor remoto y programas abiertos con datos no guardados. Este comportamiento se diferencia del apagado o reinicio del equipo local, en el que se le solicita que guarde los datos de programas que todavía no haya guardado y que compruebe que desea forzar el cierre de sesión de los usuarios que tienen una sesión iniciada. Asegúrese de que puede forzar a los otros usuarios a cerrar sesión en los servidores remotos y descartar los datos no guardados en los programas que se ejecutan en los servidores remotos.
>
> Si se produce una actualización automática en Administrador del servidor mientras se está cerrando y reiniciando un servidor administrado, pueden producirse errores de actualización y de estado de administración en el servidor administrado, ya que Administrador del servidor no se puede conectar al servidor remoto hasta que haya terminado de reiniciarse.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Para reiniciar los servidores remotos en el Administrador del servidor

1.  Abra la Página principal de un rol o grupo de servidores en Administrador del servidor.

2.  Seleccione uno o varios servidores remotos que haya agregado a Administrador del servidor. Mantenga presionado **Ctrl** mientras hace clic para para seleccionar varios servidores al mismo tiempo. Para obtener más información acerca de cómo agregar servidores al grupo de servidores de Administrador del servidor, consulte [agregar servidores a administrador del servidor](add-servers-to-server-manager.md).

3.  Haga clic con el botón secundario en los servidores seleccionados y, a continuación, haga clic en **Reiniciar servidor**.

## <a name="export-server-manager-settings-to-other-computers"></a><a name=BKMK_export></a>Exportar la configuración del Administrador del servidor a otros equipos
En Administrador del servidor, la lista de servidores administrados, los cambios en la configuración de la consola de Administrador del servidor y los grupos personalizados que ha creado se almacenan en los dos archivos siguientes. Puede volver a usar esta configuración en otros equipos que ejecuten la misma versión de Administrador del servidor (no en equipos que ejecutan la opción de instalación Server Core) o Windows 8. Herramientas de administración remota del servidor debe ejecutarse en equipos basados en cliente de Windows para exportar la configuración de Administrador del servidor a esos equipos.

-   %*AppData*% \Microsoft\Windows\ServerManager\Serverlist.xml

-   %*AppData*% \Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   Las credenciales de Administrar como (o alternativas) de los servidores del grupo de servidores no se almacenan en el perfil móvil. Administrador del servidor los usuarios deben agregarlas en cada equipo desde el que deseen ser administrados.
> -   El perfil móvil del recurso compartido de red no se crea hasta que un usuario inicia sesión en la red y luego la cierra por primera vez. El archivo **Serverlist.xml** se crea en ese momento.

Puede exportar la configuración de Administrador del servidor, hacer que Administrador del servidor configuración sea portable o usarla en otros equipos de una de las dos maneras siguientes.

-   Para exportar la configuración a otro equipo unido a un dominio, configure el usuario Administrador del servidor para que tenga un perfil móvil en usuarios y equipos de Active Directory. Debe ser administrador de dominio para cambiar las propiedades de usuario en usuarios y equipos de Active Directory.

-   Para exportar la configuración a otro equipo de un grupo de trabajo, copie los dos archivos anteriores en la misma ubicación del equipo desde el que desea administrar mediante Administrador del servidor.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Para exportar la configuración del Administrador del servidor a otros equipos unidos al dominio

1.  En usuarios y equipos de Active Directory, abra el cuadro de diálogo **propiedades** de un usuario administrador del servidor.

2.  En la pestaña **perfil** , agregue una ruta de acceso a un recurso compartido de red para almacenar el perfil del usuario.

3.  Realice una de las acciones siguientes.

    -   En las compilaciones en Inglés de Estados Unidos (en-US), los cambios realizados en el archivo **Serverlist.xml** se guardan automáticamente en el perfil. Vaya al paso siguiente.

    -   En otras compilaciones, copie los dos archivos siguientes desde el equipo que ejecuta Administrador del servidor al recurso compartido de red que forma parte del perfil móvil del usuario.

        -   %*AppData*% \Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*LocalAppData*% \Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  Haga clic en **Aceptar** para guardar los cambios y cerrar el cuadro de diálogo **Propiedades**.

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Para exportar la configuración del Administrador del servidor a equipos de grupos de trabajo

-   En un equipo desde el que desee administrar servidores remotos, sobrescriba los dos archivos siguientes con los mismos archivos de otro equipo que ejecute Administrador del servidor y que tenga la configuración que desee.

    -   %*AppData*% \Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*LocalAppData*% \Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config