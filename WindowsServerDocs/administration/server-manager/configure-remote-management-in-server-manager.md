---
title: Configurar la administración remota en Administrador del servidor
description: Administrador de servidores
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a39eaaff5497ee85cb823907cd8b57f1888dd08
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991874"
---
# <a name="configure-remote-management-in-server-manager"></a>Configurar la administración remota en Administrador del servidor

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, puede usar Administrador del servidor para realizar tareas de administración en servidores remotos. la administración remota está habilitada de forma predeterminada en los servidores que ejecutan Windows Server 2016. Para administrar un servidor de forma remota mediante Administrador del servidor, agregue el servidor al grupo de servidores Administrador del servidor.

Puede usar Administrador del servidor para administrar servidores remotos que ejecutan versiones anteriores de Windows Server, pero se necesitan las siguientes actualizaciones para administrar por completo estos sistemas operativos anteriores.

Para administrar servidores que ejecutan versiones de Windows Server anteriores a Windows Server 2016, instale el software y las actualizaciones siguientes para que las versiones anteriores de Windows Server se pueda administrar mediante Administrador del servidor en Windows Server 2016.

|Sistema operativo|Requisitos de software|Facilidad de uso|
|----------|-----------|---------|
| Windows Server 2012 R2 o Windows Server 2012 |-   [.NET Framework 4,6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5,0](https://go.microsoft.com/fwlink/?LinkID=395058). La descarga de Windows Management Framework 5,0 proveedores de actualizaciones de paquetes Instrumental de administración de Windows (WMI) en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2. Los proveedores de WMI actualizados permiten a Administrador del servidor recopilar información acerca de los roles y las características que están instalados en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 tienen un estado de capacidad de administración de **no accesible**.<br />-La actualización de rendimiento asociada con el [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) ya no es necesaria en servidores que ejecutan windows Server 2012 R2 o windows Server 2012.||
| Windows Server 2008 R2 |-   [.NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881). La descarga de Windows Management Framework 4,0 proveedores de actualizaciones de paquetes Instrumental de administración de Windows (WMI) en Windows Server 2008 R2. Los proveedores de WMI actualizados permiten a Administrador del servidor recopilar información acerca de los roles y las características que están instalados en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2008 R2 tienen un estado de capacidad de administración de **no accesible**.<br />-La actualización de rendimiento asociada con el [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite a administrador del servidor recopilar datos de rendimiento de Windows Server 2008 R2.||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) La descarga de Windows Management Framework 3,0 proveedores de actualizaciones de paquetes Instrumental de administración de Windows (WMI) en Windows Server 2008. Los proveedores de WMI actualizados permiten a Administrador del servidor recopilar información acerca de los roles y las características que están instalados en los servidores administrados. Hasta que se aplique la actualización, los servidores que ejecutan Windows Server 2008 tienen un estado de capacidad de administración de **no accesible: Compruebe que las versiones anteriores ejecutan Windows Management Framework 3,0**.<br />-La actualización de rendimiento asociada con el [artículo 2682011 de Knowledge Base](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite a administrador del servidor recopilar datos de rendimiento de Windows Server 2008.||

para obtener información detallada sobre cómo agregar servidores que están en grupos de trabajo para administrar o administrar servidores remotos desde un equipo de grupo de trabajo que ejecuta Administrador del servidor, consulte [agregar servidores a administrador del servidor](add-servers-to-server-manager.md).

## <a name="enabling-or-disabling-remote-management"></a>Habilitar o deshabilitar la administración remota
En Windows Server 2016, la administración remota está habilitada de forma predeterminada. Para poder conectarse a un equipo que ejecuta Windows Server 2016 de forma remota mediante Administrador del servidor, Administrador del servidor la administración remota de debe estar habilitada en el equipo de destino si se ha deshabilitado. En los procedimientos de esta sección se describe cómo deshabilitar la administración remota y cómo volver a habilitarla si ha sido deshabilitada. En la consola de Administrador del servidor, el estado de administración remota del servidor local se muestra en el área **propiedades** de la página **servidor local** .

Es posible que las cuentas de administrador local distintas de la cuenta predefinida de administrador no tengan derechos para administrar un servidor de forma remota, aunque la administración remota esté habilitada. El valor del registro **LocalAccountTokenFilterPolicy** del control de cuentas de usuario (UAC) remoto debe estar configurado para permitir que las cuentas locales del grupo administradores que no sea la cuenta predefinida Administrador puedan administrar el servidor de forma remota.

En Windows Server 2016, Administrador del servidor se basa en la administración remota de Windows (WinRM) y en el modelo de objetos componentes distribuido (DCOM) para las comunicaciones remotas. Los valores que se controlan mediante el cuadro de diálogo **Configurar administración remota** solo afectan a partes de administrador del servidor y Windows PowerShell que usan WinRM para las comunicaciones remotas. No afectan a las partes de Administrador del servidor que usan DCOM para las comunicaciones remotas. Por ejemplo, Administrador del servidor usa WinRM para comunicarse con los servidores remotos que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, pero usa DCOM para comunicarse con los servidores que ejecutan Windows Server 2008 y Windows Server 2008 R2, pero no tienen aplicada la actualización de [Windows Management framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881) o [Windows Management Framework](https://go.microsoft.com/fwlink/p/?LinkID=229019) 3,0. Microsoft Management Console (MMC) y otras herramientas de administración heredadas usan DCOM. Para obtener más información acerca de cómo cambiar esta configuración, vea [para configurar MMC u otra herramienta de administración remota a través de DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom) en este tema.

> [!NOTE]
> Los procedimientos que se describen en esta sección pueden realizarse solo en equipos que ejecuten Windows Server. No se puede habilitar o deshabilitar la administración remota en un equipo que ejecuta Windows 10 mediante estos procedimientos, porque el sistema operativo cliente no se puede administrar mediante Administrador del servidor.

-   Para habilitar la administración remota de WinRM, selecciona uno de los siguientes procedimientos

    -   [Para configurar la administración remota del Administrador del servidor con la interfaz de Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [Para habilitar la administración remota del Administrador del servidor con Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [Para habilitar la administración remota del Administrador del servidor con la línea de comandos](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [Para habilitar la administración remota de Windows PowerShell y el Administrador del servidor en versiones anteriores de Windows Server](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   Para deshabilitar WinRM y Administrador del servidor la administración remota, seleccione uno de los procedimientos siguientes.

    -   [Para deshabilitar la administración remota con la directiva de grupo](#to-disable-remote-management-by-using-group-policy)

    -   [Para deshabilitar la administración remota con un archivo de respuesta durante una instalación desatendida](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   Para configurar la administración remota con DCOM, vea [Para configurar MMC u otra herramienta de administración remota con DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom).

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>Para configurar la administración remota del Administrador del servidor con la interfaz de Windows

1.  > [!NOTE]
    > La configuración que se controla mediante el cuadro de diálogo **Configurar administración remota** no afecta a las partes de administrador del servidor que usan DCOM para las comunicaciones remotas.

    En el equipo que desea administrar de forma remota, abra Administrador del servidor, si aún no está abierto. En la barra de tareas de Windows, haga clic en **Administrador del servidor**. En la pantalla **Inicio** , haga clic en el icono de **Administrador del servidor** .

2.  En el área **propiedades** de la página **servidores locales** , haga clic en el valor con hipervínculo de la propiedad **administración remota** .

3.  Realice una de las siguientes acciones y haga clic en **Aceptar**.

    -   Para evitar que este equipo se administre de forma remota mediante Administrador del servidor (o Windows PowerShell si está instalado), desactive la casilla **Habilitar la administración remota de este servidor desde otros equipos** .

    -   Para permitir que este equipo se administre de forma remota mediante Administrador del servidor o Windows PowerShell, seleccione **Habilitar administración remota de este servidor desde otros equipos**.

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>Para habilitar la administración remota del Administrador del servidor con Windows PowerShell

1.  En el equipo que desea administrar de forma remota, realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.

    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en **Windows PowerShell**y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.

2.  Escriba lo siguiente y, a continuación, presione **entrar** para habilitar todas las excepciones de reglas de Firewall necesarias.

    **Configure-SMremoting.exe: habilitar**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>Para habilitar la administración remota del Administrador del servidor con la línea de comandos

1.  En el equipo que desee administrar de forma remota, abra una sesión de símbolo del sistema con permisos de usuario elevados. Para ello, en la pantalla **Inicio** , escriba **cmd**, haga clic con el botón derecho en el icono **símbolo del sistema** cuando aparezca en los resultados de **aplicaciones** y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.

2.  Ejecutar el siguiente archivo ejecutable.

    **% WINDIR% \system32\Configure-SMremoting.exe**

3.  Lleve a cabo una de las siguientes acciones:

    -   Para deshabilitar la administración remota, escriba **Configure-SMremoting.exe-Disable**y, a continuación, presione **entrar**.

    -   Para habilitar la administración remota, escriba **Configure-SMremoting.exe-enable**y, a continuación, presione **entrar**.

    -   Para ver la configuración de administración remota actual, escriba **Configure-SMremoting.exe-Get**y, a continuación, presione Entrar.

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>Para habilitar la administración remota de Windows PowerShell y el Administrador del servidor en versiones anteriores de Windows Server

-   Lleve a cabo una de las siguientes acciones:

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2012, consulte [para habilitar la administración remota de administrador del servidor mediante la interfaz de Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) en este tema.

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008 R2, consulte [administración remota con administrador del servidor](https://go.microsoft.com/fwlink/?LinkID=137378) en la ayuda de windows Server 2008 R2.

    -   Para habilitar la administración remota en servidores que ejecutan Windows Server 2008, vea [habilitar y usar comandos remotos en Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>Para configurar MMC u otra herramienta de administración remota a través de DCOM

1.  Realice una de las siguientes acciones para abrir el complemento Firewall de Windows con seguridad avanzada.

    -   En el área **propiedades** de la página **servidor local** de administrador del servidor, haga clic en el valor de hipertexto de la propiedad Firewall de **Windows** y, a continuación, haga clic en **Configuración avanzada**.

    -   En la pantalla **Inicio** , escriba **WF. msc**y, a continuación, haga clic en el icono del complemento cuando aparezca en los resultados de **aplicaciones** .

2.  En el panel de árbol, seleccione **Reglas de entrada**.

3.  Compruebe que las excepciones a las siguientes reglas de Firewall están habilitadas y que no se han deshabilitado por directiva de grupo configuración. Si alguno no está habilitado, vaya al paso siguiente.

    -   COM+ Network Access (DCOM-In)

    -   Administración remota de registro de eventos (NP-in)

    -   Administración remota de registro de eventos (RPC)

    -   Administración remota de registro de eventos (RPC-EPMAP)

4.  Haga clic con el botón secundario en las reglas que no están habilitadas y, a continuación, haga clic en **Habilitar regla** en el menú contextual.

5.  Cierre el complemento Firewall de Windows con seguridad avanzada.

### <a name="to-disable-remote-management-by-using-group-policy"></a>Para deshabilitar la administración remota con la directiva de grupo

1.  Realice una de las siguientes acciones para abrir el editor de directiva de grupo local.

    -   En un servidor que ejecute Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, en la pantalla **Inicio** , escriba **gpedit. msc**y, a continuación, haga clic en el icono **gpedit** cuando se muestre.

    -   En un servidor que ejecute Windows Server 2008 R2 o Windows Server 2008, en el cuadro de diálogo **Ejecutar** , escriba **gpedit. msc**y, a continuación, presione **entrar**.

2.  Abra **equipo Equipo\plantillas administrativas\Componentes de Windows\Windows Remote Management (WinRM) \Servicio WinRM Service**.

3.  En el panel de contenido, haga doble clic en **Permitir la administración remota de servidores a través de WinRM**.

4.  En el cuadro de diálogo de la configuración de directiva **Permitir la administración remota de servidores a través de WinRM**, seleccione **Deshabilitado** para deshabilitar la administración remota. Haga clic en **Aceptar** para guardar los cambios y cerrar el cuadro de diálogo de la configuración de directiva.

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>Para deshabilitar la administración remota con un archivo de respuesta durante una instalación desatendida

1.  Cree un archivo de respuesta de instalación desatendida para las instalaciones de Windows Server 2016 mediante el administrador de imágenes de sistema (Windows SIM). Para obtener más información sobre cómo crear un archivo de respuesta y usar Windows SIM, consulte [¿Qué es el Administrador de imágenes de sistema?](/previous-versions/windows/it-pro/windows-vista/cc766347(v=ws.10)) y [Paso a paso: implementación básica de Windows para profesionales de TI](/previous-versions/windows/it-pro/windows-7/dd349348(v=ws.10)).

2.  En el archivo de respuesta, busque la configuración **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**.

3.  Para deshabilitar Administrador del servidor la administración remota de forma predeterminada en todos los servidores a los que desea aplicar el archivo de respuesta, establezca **Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement** en **false**.

    > [!NOTE]
    > Esta configuración deshabilita la administración remota como parte del proceso de instalación del sistema operativo. La configuración de esta opción no impide que un administrador habilite Administrador del servidor la administración remota en un servidor una vez finalizada la instalación del sistema operativo. Los administradores pueden volver a habilitar Administrador del servidor la administración remota mediante los pasos de [para configurar la administración remota de administrador del servidor mediante la interfaz de Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) o [para habilitar la administración remota de administrador del servidor con Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell) en este tema.
    >
    > Si deshabilita la administración remota de forma predeterminada como parte de una instalación desatendida y no vuelve a habilitar la administración remota en el servidor después de la instalación, los servidores a los que se aplica este archivo de respuesta no se pueden administrar completamente mediante Administrador del servidor. Los servidores que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (y que tienen la administración remota deshabilitada de forma predeterminada) generan errores de estado de capacidad de administración en la consola de Administrador del servidor una vez agregados al grupo de servidores Administrador del servidor.

## <a name="windows-remote-management-winrm-listener-settings"></a>Configuración del agente de escucha de administración remota de Windows (WinRM)
Administrador del servidor se basa en la configuración predeterminada del agente de escucha de WinRM en los servidores remotos que desea administrar. Si el mecanismo de autenticación predeterminado o el número de puerto del agente de escucha de WinRM en un servidor remoto ha cambiado con respecto a la configuración predeterminada, Administrador del servidor no puede comunicarse con el servidor remoto.

En la siguiente lista se muestra la configuración predeterminada del agente de escucha de WinRM para la administración mediante Administrador del servidor.

-   El servicio WinRM se está ejecutando.

-   Se crea un proceso de escucha de WinRM para aceptar las solicitudes HTTP a través del número de puerto 5985.

-   El número de puerto 5985 está habilitado en la configuración del Firewall de Windows para permitir las solicitudes a través de WinRM.

-   Los tipos de autenticación **Kerberos** y **Negotiate** están habilitados.

El número de puerto predeterminado es 5985 para WinRM para comunicarse con un equipo remoto.

para obtener más información sobre cómo configurar las opciones del agente de escucha de WinRM, escriba **WinRM Help config**en un símbolo del sistema y, a continuación, presione Entrar.

## <a name="see-also"></a>Consulte también
[Agregar servidores a administrador del servidor](add-servers-to-server-manager.md) 
 [Windows PowerShell: about_remote_Troubleshooting en Windows Server TechCenter](/previous-versions/dd347642(v=technet.10)) 
 [Descripción del control de cuentas de usuario](https://support.microsoft.com/kb/951016)