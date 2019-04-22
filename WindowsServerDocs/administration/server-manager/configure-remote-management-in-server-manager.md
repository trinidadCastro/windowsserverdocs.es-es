---
title: Configurar la administración remota en el administrador del servidor
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a66fe7a274756de9bed9f6b14f5b9e491e5b623
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819586"
---
# <a name="configure-remote-management-in-server-manager"></a>Configurar la administración remota en el administrador del servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, puede usar el administrador del servidor para realizar tareas de administración en servidores remotos. administración remota está habilitada de forma predeterminada en los servidores que ejecutan Windows Server 2016. Para administrar un servidor de forma remota mediante el administrador del servidor, agregue el servidor al grupo de servidores de administrador del servidor.

Puede usar el administrador del servidor para administrar servidores remotos que ejecutan versiones anteriores de Windows Server, pero las siguientes actualizaciones son necesarias para administrar completamente estos sistemas operativos anteriores.

Para administrar servidores que ejecutan versiones de Windows Server anterior a Windows Server 2016, instale el siguiente software y actualizaciones para hacer que las versiones anteriores de Windows Server puedan administrar con el administrador del servidor en Windows Server 2016.

|Sistema operativo|Software necesario|Manageability|
|----------|-----------|---------|
| Windows Server 2012 R2 o Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058). El paquete de descarga de Windows Management Framework 5.0 actualiza los proveedores de Instrumental de administración de Windows (WMI) en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2. Los proveedores de WMI actualizados permiten que el administrador del servidor recopilar información sobre los roles y características que están instaladas en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 tienen un estado de manejabilidad **no está accesible**.<br />-La actualización de rendimiento asociado con [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) ya no es necesario en servidores que ejecutan Windows Server 2012 R2 o Windows Server 2012.||
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). El paquete de descarga de Windows Management Framework 4.0 actualiza los proveedores de Instrumental de administración de Windows (WMI) en Windows Server 2008 R2. Los proveedores de WMI actualizados permiten que el administrador del servidor recopilar información sobre los roles y características que están instaladas en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2008 R2 tienen un estado de manejabilidad **no está accesible**.<br />-La actualización de rendimiento asociado con [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite recopilar datos de rendimiento de Windows Server 2008 R2 que el administrador del servidor.||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) paquete de descarga de The Windows Management Framework 3.0 actualiza los proveedores de Instrumental de administración de Windows (WMI) en Windows Server 2008. Los proveedores de WMI actualizados permiten que el administrador del servidor recopilar información sobre los roles y características que están instaladas en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2008 tienen un estado de manejabilidad **no accesible: Compruebe que las versiones anteriores ejecutan Windows Management Framework 3.0**.<br />-La actualización de rendimiento asociado con [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite recopilar datos de rendimiento de Windows Server 2008 que el administrador del servidor.||

Para obtener información detallada sobre cómo agregar servidores que están en grupos de trabajo para administrar o cómo administrar servidores remotos desde un equipo de grupo de trabajo que se está ejecutando el administrador del servidor, consulte [agregar servidores al administrador del servidor](add-servers-to-server-manager.md).

## <a name="BKMK_remote"></a>Habilitar o deshabilitar la administración remota
En Windows Server 2016, la administración remota está habilitada de forma predeterminada. Antes de poder conectarse a un equipo que se ejecuta Windows Server 2016 de forma remota mediante el administrador del servidor, el administrador del servidor de administración remota debe habilitarse en el equipo de destino si se ha deshabilitado. En los procedimientos de esta sección se describe cómo deshabilitar la administración remota y cómo volver a habilitarla si ha sido deshabilitada. En la consola de administrador del servidor, se muestra el estado de administración remota del servidor local en el **propiedades** área de la **servidor Local** página.

Es posible que las cuentas de administrador local distintas de la cuenta predefinida de administrador no tengan derechos para administrar un servidor de forma remota, aunque la administración remota esté habilitada. El Control de cuentas de usuario (UAC) remoto **LocalAccountTokenFilterPolicy** configuración del registro debe configurarse para permitir que las cuentas locales del grupo administradores diferentes a la cuenta Administrador integrado para administrar de forma remota el servidor.

En Windows Server 2016, el administrador del servidor se basa en la administración remota de Windows (WinRM) y el modelo de objetos de componentes distribuidos (DCOM) para las comunicaciones remotas. La configuración que se controla mediante el **configurar la administración remota** cuadro de diálogo solo afectan a partes del administrador del servidor y Windows PowerShell que usan WinRM para las comunicaciones remotas. No afectan a las partes del administrador del servidor que usan DCOM para las comunicaciones remotas. Por ejemplo, el administrador del servidor usa WinRM para comunicarse con servidores remotos que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, pero usa DCOM para comunicarse con servidores que ejecutan Windows Server 2008 y Windows Server 2008 R2, pero no tienen la [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881) o [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) actualizaciones aplicadas. Microsoft Management Console (mmc) y otras herramientas de administración heredadas usan DCOM. Para obtener más información acerca de cómo cambiar esta configuración, consulte [para configurar mmc u otra herramienta de administración remota con DCOM](#BKMK_dcom) en este tema.

> [!NOTE]
> Los procedimientos que se describen en esta sección pueden realizarse solo en equipos que ejecuten Windows Server. No se puede habilitar o deshabilitar la administración remota en un equipo que ejecuta Windows 10 mediante el uso de estos procedimientos, porque el sistema operativo cliente no puede administrarse mediante el administrador del servidor.

-   Para habilitar la administración remota de WinRM, selecciona uno de los siguientes procedimientos

    -   [Para habilitar la administración remota del administrador del servidor mediante la interfaz de Windows](#BKMK_windows)

    -   [Para habilitar la administración remota del administrador del servidor mediante Windows PowerShell](#BKMK_ps)

    -   [Para habilitar la administración remota del administrador del servidor mediante la línea de comandos](#BKMK_cmdline)

    -   [Para habilitar la administración Server Manager y Windows PowerShell remoto en versiones anteriores de Windows Server](#BKMK_old)

-   Para deshabilitar la administración remota de WinRM y el administrador del servidor, seleccione uno de los procedimientos siguientes.

    -   [Para deshabilitar la administración remota mediante la directiva de grupo](#BKMK_disableGP)

    -   [Para deshabilitar la administración remota mediante el uso de un archivo de respuesta durante la instalación desatendida](#BKMK_unattend)

-   Para configurar la administración remota de DCOM, consulta [To configure DCOM remote management](#BKMK_dcom).

### <a name="BKMK_windows"></a>Para habilitar la administración remota del administrador del servidor mediante la interfaz de Windows

1.  > [!NOTE]
    > La configuración que se controla mediante el **configurar la administración remota** cuadro de diálogo no afectan a las partes del administrador del servidor que usan DCOM para las comunicaciones remotas.

    En el equipo que desea administrar de forma remota, abra Administrador del servidor, si no está ya abierto. En la barra de tareas de Windows, haga clic en **Administrador del servidor**. En el **iniciar** pantalla, haga clic en el **administrador del servidor** icono.

2.  En el **propiedades** área de la **servidores locales** página, haga clic en el valor con hipervínculo para el **administración remota** propiedad.

3.  Realice una de las siguientes acciones y haga clic en **Aceptar**.

    -   Para evitar que este equipo se administre de forma remota mediante el administrador del servidor (o Windows PowerShell si está instalado), desactive la **habilitar la administración remota de este servidor desde otros equipos** casilla de verificación.

    -   Para permitir que este equipo a administrarse de forma remota mediante el administrador del servidor o Windows PowerShell, seleccione **habilitar la administración remota de este servidor desde otros equipos**.

### <a name="BKMK_ps"></a>Para habilitar la administración remota del administrador del servidor mediante Windows PowerShell

1.  En el equipo que desea administrar de forma remota, realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.

    -   En el Windows **iniciar** pantalla, haga clic en **Windows PowerShell**y, a continuación, en la barra de la aplicación, haga clic en **ejecutar como administrador**.

2.  Escriba lo siguiente y, a continuación, presione **ENTRAR** habilitar todas las excepciones de reglas de firewall necesarias.

    **Configure-SMremoting.exe -enable**

### <a name="BKMK_cmdline"></a>Para habilitar la administración remota del administrador del servidor mediante la línea de comandos

1.  En el equipo que desee administrar de forma remota, abra una sesión de símbolo del sistema con permisos de usuario elevados. Para ello, en el **iniciar** , escriba **cmd**, haga clic en el **símbolo** icono cuando se muestra en el **aplicaciones** resultados, y Haga clic en la barra de la aplicación, **ejecutar como administrador**.

2.  Ejecutar el siguiente archivo ejecutable.

    **%windir%\system32\Configure-SMremoting.exe**

3.  Realiza una de las siguientes acciones:

    -   Para deshabilitar la administración remota, escriba **Configure-SMremoting.exe-deshabilitar**y, a continuación, presione **ENTRAR**.

    -   Para habilitar la administración remota, escriba **Configure-SMremoting.exe-habilitar**y, a continuación, presione **ENTRAR**.

    -   Para ver la configuración actual de la administración remota, escriba **Configure-SMremoting.exe-get**, y, a continuación, presione ENTRAR.

### <a name="BKMK_old"></a>Para habilitar la administración Server Manager y Windows PowerShell remoto en versiones anteriores de Windows Server

-   Realiza una de las siguientes acciones:

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2012, consulte [para habilitar la administración remota del administrador del servidor mediante la interfaz de Windows](#BKMK_windows) en este tema.

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008 R2, consulte [con el administrador del servidor de administración remota](https://go.microsoft.com/fwlink/?LinkID=137378) en la Ayuda de Windows Server 2008 R2.

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008, consulte [habilitar y usar comandos remotos en Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

### <a name="BKMK_dcom"></a>Para configurar mmc u otra herramienta de administración remota con DCOM

1.  Realice una de las siguientes acciones para abrir el complemento Firewall de Windows con seguridad avanzada.

    -   En el **propiedades** área de la **servidor Local** página en el administrador del servidor, haga clic en el valor del hipertexto para la **Windows Firewall** propiedad y, a continuación, haga clic en  **Configuración avanzada**.

    -   En el **iniciar** , escriba **WF.msc**y, a continuación, haga clic en el icono de inicio cuando se muestra en el **aplicaciones** resultados.

2.  En el panel de árbol, seleccione **Reglas de entrada**.

3.  Compruebe que las excepciones a las reglas de firewall siguientes están habilitadas y no se han deshabilitado mediante la configuración de directiva de grupo. Si alguno no está habilitado, vaya al paso siguiente.

    -   COM+ Network Access (DCOM-In)

    -   Administración remota de registro de eventos (NP-In)

    -   Administración de registro de eventos remoto (RPC)

    -   Administración remota de registro de eventos (RPC-EPMAP)

4.  Haga clic con el botón secundario en las reglas que no están habilitadas y, a continuación, haga clic en **Habilitar regla** en el menú contextual.

5.  Cierre el complemento Firewall de Windows con seguridad avanzada.

### <a name="BKMK_disableGP"></a>Para deshabilitar la administración remota mediante la directiva de grupo

1.  Realice una de las siguientes acciones para abrir el editor de directivas de grupo Local.

    -   En un servidor que ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, en el **iniciar** , escriba **gpedit.msc**y, a continuación, haga clic en el **gpedit** icono Cuando se muestra.

    -   En un servidor que ejecuta Windows Server 2008 R2 o Windows Server 2008, en la **ejecutar** cuadro de diálogo, escriba **gpedit.msc**y, a continuación, presione **ENTRAR**.

2.  Abra **equipo Configuración de usuario\Plantillas administrativas\Componentes Windows\Windows remoto Management (WinRM) \WinRM servicio**.

3.  En el panel de contenido, haga doble clic en **Permitir la administración remota de servidores a través de WinRM**.

4.  En el cuadro de diálogo de la configuración de directiva **Permitir la administración remota de servidores a través de WinRM** , seleccione **Deshabilitado** para deshabilitar la administración remota. Haga clic en **Aceptar** para guardar los cambios y cerrar el cuadro de diálogo de la configuración de directiva.

### <a name="BKMK_unattend"></a>Para deshabilitar la administración remota mediante el uso de un archivo de respuesta durante la instalación desatendida

1.  crear un archivo de respuesta de instalación desatendida para las instalaciones de Windows Server 2016 mediante el Administrador de imágenes de sistema de Windows (Windows SIM). Para obtener más información acerca de cómo crear un archivo de respuesta y usar Windows SIM, consulte [¿qué es el Administrador de imágenes de sistema de Windows?](https://technet.microsoft.com/library/cc766347.aspx) y [paso a paso: Implementación básica de Windows para profesionales de TI](https://technet.microsoft.com/library/dd349348.aspx).

2.  En el archivo de respuesta, busque la configuración **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**.

3.  Para deshabilitar la administración remota del administrador del servidor de forma predeterminada en todos los servidores a la que desea aplicar el archivo de respuesta, establezca **Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement** a **False** .

    > [!NOTE]
    > Esta configuración deshabilita la administración remota como parte del proceso de instalación del sistema operativo. Esta configuración impedirá que un administrador habilite la administración remota del administrador del servidor en un servidor una vez completada la instalación del sistema operativo. Los administradores pueden habilitar la administración remota de nuevo mediante los pasos en el administrador del servidor [para configurar la administración remota del administrador del servidor mediante la interfaz de Windows](#BKMK_windows) o [para habilitar la administración remota del administrador del servidor mediante Windows PowerShell](#BKMK_ps) en este tema.
    > 
    > Si deshabilitar la administración remota de forma predeterminada como parte de una instalación desatendida y no habilite la administración remota en el servidor nuevo tras la instalación, los servidores a la que se aplique este archivo de respuesta no pueden ser totalmente administrados mediante el administrador del servidor. Los servidores que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (y que tienen la administración remota habilitada de forma predeterminada) generan errores de estado de manejabilidad en la consola de administrador del servidor después de agregarlos al servidor de administrador del servidor grupo.

## <a name="windows-remote-management-winrm-listener-settings"></a>Configuración de agente de escucha de (WinRM) de la administración remota de Windows
El administrador del servidor se basa en la configuración predeterminada del agente de escucha de WinRM en los servidores remotos que desea administrar. Si el mecanismo de autenticación predeterminado o el número de puerto del agente de escucha de WinRM en un servidor remoto ha cambiado desde la configuración predeterminada, el administrador del servidor no puede comunicarse con el servidor remoto.

La lista siguiente muestra predeterminada configuración de agente de escucha de WinRM para la administración mediante el administrador del servidor.

-   El servicio WinRM se está ejecutando.

-   Se crea un proceso de escucha de WinRM para aceptar las solicitudes HTTP a través del número de puerto 5985.

-   El número de puerto 5985 está habilitado en la configuración del Firewall de Windows para permitir las solicitudes a través de WinRM.

-   Los tipos de autenticación **Kerberos** y **Negotiate** están habilitados.

El número de puerto predeterminado es 5985 para WinRM para comunicarse con un equipo remoto.

Para obtener más información acerca de cómo configurar las opciones del agente de escucha de WinRM, en un símbolo del sistema, escriba **winrm help config**, y, a continuación, presione ENTRAR.

## <a name="see-also"></a>Vea también
[Agregar servidores al administrador del servidor](add-servers-to-server-manager.md)
[Windows PowerShell: about_remote_Troubleshooting en Windows Server TechCenter](https://technet.microsoft.com/library/dd347642.aspx)
[descripción del Control de cuentas de usuario](https://support.microsoft.com/kb/951016)



