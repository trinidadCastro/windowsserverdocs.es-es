---
title: Implementación de varios servidores de acceso remoto en una implementación multisitio
description: Este tema forma parte de la guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da23f3082e1d97f1bcfbee7365b863d29ba2d020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404492"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>Implementación de varios servidores de acceso remoto en una implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan la VPN de DirectAccess y el servicio de acceso remoto (RAS) en un solo rol de acceso remoto. El acceso remoto se puede implementar en diversos escenarios de empresas. Esta información general proporciona una introducción al escenario empresarial para implementar servidores de acceso remoto en una configuración multisitio.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
En una implementación multisitio, dos o más servidores de acceso remoto o clústeres de servidores se implementan y configuran como puntos de entrada diferentes en una sola ubicación o en ubicaciones geográficas dispersas. La implementación de varios puntos de entrada en una sola ubicación permite la redundancia del servidor o la alineación de los servidores de acceso remoto con la arquitectura de red existente. La implementación por ubicación geográfica garantiza un uso eficaz de los recursos, ya que los equipos cliente remotos pueden conectarse a recursos de la red interna mediante un punto de entrada más cercano a ellos. El tráfico a través de una implementación multisitio se puede distribuir y equilibrar con un equilibrador de carga global externo.  
  
Una implementación multisitio es compatible con equipos cliente que ejecutan Windows 10, Windows 8 o Windows 7. Los equipos cliente que ejecutan Windows 10 o Windows 8 identifican automáticamente un punto de entrada, o bien el usuario puede seleccionar manualmente un punto de entrada. La asignación automática se produce en el siguiente orden de prioridad:  
  
1.  Usar un punto de entrada seleccionado manualmente por el usuario.  
  
2.  Use un punto de entrada identificado por un equilibrador de carga global externo si se implementa uno.  
  
3.  Use el punto de entrada más cercano identificado por el equipo cliente mediante un mecanismo de sondeo automático.  
  
La compatibilidad con los clientes que ejecutan Windows 7 debe estar habilitada manualmente en cada punto de entrada y no se admite la selección de un punto de entrada por parte de estos clientes.  
  
## <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  
  
-   [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) debe implementarse antes de una implementación multisitio.  
  
-   Los clientes de Windows 7 siempre se conectarán a un sitio específico. No podrán conectarse al sitio más cercano en función de la ubicación del cliente (a diferencia de los clientes de Windows 10, 8 o 8,1).  
  
-   No se admite la opción de cambiar directivas fuera de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.  
  
-   Hay que implementar una infraestructura de clave pública.  
  
    Para obtener más información, consulta: @no__t: módulo de la guía de laboratorio de 0Test: PKI básica para Windows Server 2012. ](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   La red corporativa debe estar habilitada para IPv6. Si utilizas ISATAP, debes eliminarlo y usar IPv6 nativo.  
  
## <a name="in-this-scenario"></a>En este escenario  
El escenario de implementación multisitio incluye una serie de pasos:  
  
1. [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Antes de configurar una implementación multisitio, se debe implementar un solo servidor de acceso remoto con configuración avanzada.  
  
2. [Planear una implementación multisitio](plan/Plan-a-Multisite-Deployment.md). Para crear una implementación multisitio desde un solo servidor, se requieren varios pasos de planeación adicionales, incluido el cumplimiento de los requisitos previos de multisitio y la planificación de Active Directory grupos de seguridad, objetos de directiva de grupo (GPO), DNS y configuración de cliente.  
  
3. [Configurar una implementación multisitio](configure/Configure-a-Multisite-Deployment.md). Consta de varios pasos de configuración, como la preparación de la infraestructura de Active Directory, la configuración del servidor de acceso remoto existente y la adición de varios servidores de acceso remoto como puntos de entrada a la implementación multisitio.  
  
4. [Solucionar problemas de una implementación multisitio](troubleshoot/Troubleshoot-a-Multisite-Deployment.md). Esta sección de solución de problemas describe una serie de los errores más comunes que se pueden producir al implementar el acceso remoto en una implementación multisitio.
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
Una implementación multisitio proporciona lo siguiente:  
  
-   Rendimiento mejorado: una implementación multisitio permite que los equipos cliente tengan acceso a recursos internos mediante el acceso remoto para conectarse con el punto de entrada más cercano y adecuado. El acceso del cliente a los recursos internos de forma eficaz y la velocidad de las solicitudes de Internet de cliente enrutadas a través de DirectAccess se ha mejorado. Se puede equilibrar el tráfico entre los puntos de entrada mediante un equilibrador de carga global externo.  
  
-   Facilidad de administración: multisitio permite a los administradores alinear la implementación de acceso remoto con una implementación de sitios Active Directory, lo que proporciona una arquitectura simplificada. La configuración compartida se puede establecer fácilmente en los servidores de punto de entrada o en los clústeres. La configuración de acceso remoto se puede administrar desde cualquiera de los servidores de la implementación, o de forma remota mediante Herramientas de administración remota del servidor (RSAT). Además, se puede supervisar toda la implementación multisitio desde una sola consola de administración de acceso remoto.  
  
## <a name="BKMK_NEW"></a>Roles y características incluidos en este escenario  
En la tabla siguiente se enumeran los roles y las características que se usan en este escenario.  
  
|Rol/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol de acceso remoto|El rol se instala y desinstala mediante la consola del Administrador del servidor. Incluye tanto DirectAccess, que antes era una característica de Windows Server 2008 R2, como los servicios de enrutamiento y acceso remoto (RRAS), que antes eran un servicio de roles de los Servicios de acceso y directivas de redes (NPAS). El rol de acceso remoto consta de dos componentes:<br /><br />-DirectAccess y VPN de servicios de enrutamiento y acceso remoto (RRAS): DirectAccess y VPN se administran juntos en la consola de administración de acceso remoto.<br />-Enrutamiento RRAS: las características de enrutamiento RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />Las dependencias son las siguientes:<br /><br />-Servidor web-Internet Information Services (IIS): esta característica es necesaria para configurar el servidor de ubicación de red y el sondeo Web predeterminado.<br />-Windows Internal Database: se usa para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Se puede instalar opcionalmente en un servidor que no ejecute el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-Herramientas de línea de comandos y GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />-Consola de administración de directivas de grupo<br />-Kit de administración del administrador de conexiones RAS (CMAK)<br />-Windows PowerShell 3,0<br />-Infraestructura y herramientas de administración de gráficos|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
-   Al menos dos equipos de acceso remoto que se van a recopilar en una implementación multisitio.   
  
-   Para probar el escenario, se requiere al menos un equipo que ejecute Windows 8 y que esté configurado como cliente de DirectAccess. Para probar el escenario para clientes que ejecutan Windows 7, se requiere al menos un equipo que ejecute Windows 7.  
  
-   Para equilibrar la carga del tráfico entre servidores de punto de entrada, se requiere un equilibrador de carga global externo de terceros.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Los requisitos de software para este escenario son los siguientes:  
  
-   Requisitos de software para la implementación de un solo servidor.  
  
-   Además de los requisitos de software para un solo servidor, hay varios requisitos específicos de multisitio:  
  
    -   Requisitos de autenticación de IPsec: en una implementación multisitio, DirectAccess debe implementarse mediante la autenticación de certificado de equipo IPsec. No se admite la opción de realizar la autenticación IPsec mediante el servidor de acceso remoto como un proxy Kerberos. Se requiere una entidad de certificación interna para implementar los certificados IPsec.  
  
    -   Requisitos de IP-HTTPS y del servidor de ubicación de red: los certificados necesarios para IP-HTTPS y el servidor de ubicación de red deben ser emitidos por una CA. No se admite la opción de usar certificados que el servidor de acceso remoto emite automáticamente y que están autofirmados. Los certificados pueden ser emitidos por una entidad de certificación interna o por una entidad de certificación externa de terceros.  
  
    -   Requisitos de Active Directory: se requiere al menos un sitio Active Directory. El servidor de acceso remoto debe estar ubicado en el sitio. Para tiempos de actualización más rápidos, se recomienda que cada sitio tenga un controlador de dominio grabable, aunque esto no es obligatorio.  
  
    -   Requisitos de grupo de seguridad: los requisitos son los siguientes:  
  
        -   Se requiere un solo grupo de seguridad para todos los equipos cliente de Windows 8 de todos los dominios. Se recomienda crear un grupo de seguridad único de estos clientes para cada dominio.  
  
        -   Se requiere un grupo de seguridad único que contenga equipos con Windows 7 para cada punto de entrada configurado para admitir clientes de Windows 7. Se recomienda tener un grupo de seguridad único para cada punto de entrada de cada dominio.  
  
        -   Los equipos no deben estar incluidos en más de un grupo de seguridad que incluya clientes de DirectAccess. Si los clientes están incluidos en varios grupos, la resolución de nombres para las solicitudes de los clientes no funcionará según lo esperado.  
  
    -   Requisitos de GPO: los GPO se pueden crear manualmente antes de configurar el acceso remoto o crearse automáticamente durante la implementación de acceso remoto. Los requisitos son los siguientes:  
  
        -   Se requiere un GPO de cliente único para cada dominio.  
  
        -   Se requiere un GPO de servidor para cada punto de entrada, en el dominio en el que se encuentra el punto de entrada. Por tanto, si varios puntos de entrada se encuentran en el mismo dominio, habrá varios GPO de servidor (uno para cada punto de entrada) en el dominio.  
  
        -   Se requiere un GPO de cliente de Windows 7 único para cada punto de entrada habilitado para la compatibilidad con clientes de Windows 7 para cada dominio.  
  
## <a name="KnownIssues"></a>Problemas conocidos  
A continuación se indican problemas conocidos al configurar un escenario de multisitio:  
  
-   **Varios puntos de entrada en la misma subred IPv4**. La adición de varios puntos de entrada en la misma subred IPv4 producirá un mensaje de conflicto de direcciones IP y la dirección DNS64 para el punto de entrada no se configurará según lo esperado. Este problema se produce cuando IPv6 no se ha implementado en las interfaces internas de los servidores de la red corporativa. Para evitar este problema, ejecute el siguiente comando de Windows PowerShell en todos los servidores de acceso remoto actuales y futuros:  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   Si la dirección pública especificada para que los clientes de DirectAccess se conecten al servidor de acceso remoto tiene un sufijo incluido en NRPT, es posible que DirectAccess no funcione según lo esperado. Asegúrese de que la NRPT tiene una exención para el nombre público. En una implementación multisitio, se deben agregar exenciones para los nombres públicos de todos los puntos de entrada. Tenga en cuenta que si está habilitado el túnel forzado, estas exenciones se agregan automáticamente. Se quitan si la tunelización forzada está deshabilitada.  
  
-   Al usar el cmdlet **Disable-DAMultiSite**de Windows PowerShell, los parámetros Whatif y CONFIRM no tienen ningún efecto, y se deshabilitará multisitio y se quitarán los GPO de Windows 7.  
  
-   Cuando los clientes de Windows 7 que usan DCA en una implementación multisitio se actualizan a Windows 8, el Asistente para la conectividad de red no funcionará. Este problema se puede resolver antes de la actualización del cliente mediante la modificación de los GPO de Windows 7 mediante los siguientes cmdlets de Windows PowerShell:  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    En caso de que el cliente ya se haya actualizado, mueva el equipo cliente al grupo de seguridad de Windows 8.  
  
-   Al modificar la configuración del controlador de dominio con el cmdlet **de Windows PowerShell Set-DAEntryPointDC**, si el parámetro COMPUTERNAME especificado es un servidor de acceso remoto en un punto de entrada distinto del último que se agregó a la implementación multisitio, se trata de una advertencia. se mostrará que indica que el servidor especificado no se actualizará hasta la siguiente actualización de directiva. Los servidores reales que no se actualizaron pueden verse con el **Estado de configuración** en el **Panel** de la consola de **Administración de acceso remoto**. No obstante, esto no causará ningún problema funcional; sin embargo, puede ejecutar **gpupdate/force** en los servidores que no se actualizaron para obtener el estado de configuración actualizado inmediatamente.  
  
-   Cuando se implementa multisitio en una red corporativa solo IPv4, al cambiar el prefijo IPv6 de la red interna también se cambia la dirección DNS64, pero no se actualiza la dirección en las reglas de firewall que permiten consultas DNS en el servicio DNS64. Para resolver este problema, ejecute los siguientes comandos de Windows PowerShell después de cambiar el prefijo IPv6 de la red interna:  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   Si DirectAccess se implementó cuando una infraestructura ISATAP existente estaba presente, al quitar un punto de entrada que era un host ISATAP, la dirección IPv6 del servicio DNS64 se quitará de las direcciones de servidor DNS de todos los sufijos DNS de la NRPT.  
  
    Para resolver este problema, en el Asistente para la **instalación del servidor de infraestructura** , en la página **DNS** , quite los sufijos DNS que se modificaron y agréguelos de nuevo con las direcciones de servidor DNS correctas; para ello, haga clic en **detectar** en las **direcciones del servidor DNS.** cuadro de diálogo.  
  


