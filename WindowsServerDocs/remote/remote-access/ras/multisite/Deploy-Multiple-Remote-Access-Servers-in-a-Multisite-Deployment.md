---
title: Implementación de varios servidores de acceso remoto en una implementación multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3091c770fb9b207d82deaa571970bfeb44d17ee3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875886"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>Implementación de varios servidores de acceso remoto en una implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan DirectAccess y el servicio de acceso remoto (RAS) VPN en un solo rol de acceso remoto. El acceso remoto se puede implementar en diversos escenarios de empresas. Esta información general proporciona una introducción al escenario empresarial para implementar servidores de acceso remoto en una configuración multisitio.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
En una implementación multisitio dos o más servidores de acceso remoto o clústeres de servidores han implementado y configurado como puntos de entrada diferentes en una sola ubicación o en ubicaciones geográficamente dispersas. Implementación de varios puntos de entrada en una sola ubicación permite la redundancia de servidores, o para la alineación de los servidores de acceso remoto con la arquitectura de red existente. Implementación por ubicación geográfica garantiza un uso eficaz de recursos, como los equipos cliente remotos pueden conectarse a recursos de red interna mediante un punto de entrada más cercano a ellos. Puede distribuir el tráfico a través de una implementación multisitio y equilibrado con el equilibrador de carga global externo.  
  
Una implementación multisitio es compatible con equipos cliente que ejecutan Windows 10, Windows 8 o Windows 7. Identifican los equipos cliente que ejecutan Windows 10 o Windows 8 automáticamente un punto de entrada o el usuario puede seleccionar manualmente un punto de entrada. Asignación automática se produce en el orden de prioridad siguiente:  
  
1.  Use un punto de entrada seleccionado manualmente por el usuario.  
  
2.  Use un punto de entrada que se identifica mediante un equilibrador de carga global externo si uno se implementa.  
  
3.  Utilice el punto de entrada más próximo identificado por el equipo cliente utilizando un mecanismo de sondeo automático.  
  
Soporte técnico para clientes que ejecutan Windows 7 debe habilitarse manualmente en cada punto de entrada y no se admite la selección de un punto de entrada por estos clientes.  
  
## <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  
  
-   [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) deben implementar antes de una implementación multisitio.  
  
-   Los clientes de Windows 7 siempre se conectarán a un sitio específico. No podrá conectarse al sitio más cercano según la ubicación del cliente (a diferencia de los clientes de Windows 10, 8 u 8.1).  
  
-   No se admite la opción de cambiar directivas fuera de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.  
  
-   Hay que implementar una infraestructura de clave pública.  
  
    Para obtener más información, consulta: [Minimódulo de guía de laboratorio de pruebas: PKI básica para Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   La red corporativa debe estar habilitado el protocolo IPv6. Si utilizas ISATAP, debes eliminarlo y usar IPv6 nativo.  
  
## <a name="in-this-scenario"></a>En este escenario  
El escenario de implementación multisitio incluye una serie de pasos:  
  
1. [Implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un único servidor de acceso remoto con configuración avanzada se debe implementar antes de configurar una implementación multisitio.  
  
2. [Planear una implementación multisitio](plan/Plan-a-Multisite-Deployment.md). Para crear una implementación multisitio desde un único servidor un número de una planeación adicional pasos son necesarios, incluido el cumplimiento con los requisitos previos de multisitios y planeación de grupos de seguridad de Active Directory, objetos de directiva de grupo (GPO), DNS y configuración de cliente.  
  
3. [Configurar una implementación multisitio](configure/Configure-a-Multisite-Deployment.md). Esto se compone de una serie de pasos de configuración, incluida la preparación de la infraestructura de Active Directory, la configuración del servidor de acceso remoto existente y la adición de varios servidores de acceso remoto como puntos de entrada a la implementación multisitio.  
  
4. [Solución de problemas de una implementación multisitio](troubleshoot/Troubleshoot-a-Multisite-Deployment.md). En esta sección de solución de problemas describe una serie de los errores más comunes que pueden producirse al implementar el acceso remoto en una implementación multisitio.
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
Una implementación multisitio proporciona lo siguiente:  
  
-   Implementación multisitio de rendimiento A mejorada permite al cliente equipos que acceden a recursos internos mediante el acceso remoto para conectarse con el punto de entrada más cercano y más adecuado. Cliente tener acceso a los recursos internos de forma eficaz, y se mejora la velocidad de cliente Internet solicita que se enrutan a través de DirectAccess. Puede equilibrarse tráfico a través de puntos de entrada mediante un equilibrador de carga global externo.  
  
-   Facilidad de administración multisitio permite a los administradores alinear la implementación de acceso remoto a una implementación de sitios de Active Directory, que proporciona una arquitectura simplificada. La configuración compartida fácilmente puede establecerse en servidores de punto de entrada o de clústeres. Configuración de acceso remoto puede administrarse desde cualquiera de los servidores de la implementación o de forma remota mediante las herramientas de administración remota de servidor (RSAT). Además, toda la implementación multisitio se puede supervisar desde una única consola de administración de acceso remoto.  
  
## <a name="BKMK_NEW"></a>Roles y características que se incluyen en este escenario  
En la tabla siguiente se enumera los roles y características que se usan en este escenario.  
  
|Rol/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol de acceso remoto|El rol se instala y desinstala mediante la consola del Administrador del servidor. Incluye tanto DirectAccess, que antes era una característica de Windows Server 2008 R2, como los servicios de enrutamiento y acceso remoto (RRAS), que antes eran un servicio de roles de los Servicios de acceso y directivas de redes (NPAS). El rol de acceso remoto consta de dos componentes:<br /><br />-DirectAccess y enrutamiento y servicios de acceso remoto (RRAS) VPN de DirectAccess y VPN se administran conjuntamente en la consola de administración de acceso remoto.<br />-Características de enrutamiento de enrutamiento de RRAS RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />Las dependencias son las siguientes:<br /><br />-Internet Information Services (IIS) Web Server: esta característica es necesaria para configurar el servidor de ubicación de red y el sondeo web predeterminado.<br />-Windows Database-Used interno para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Se puede instalar opcionalmente en un servidor que no ejecuta el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-Herramientas de línea de comandos y GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />: Consola de administración de directivas de grupo<br />-Kit de administración de Connection Manager (CMAK) de RAS<br />-Windows PowerShell 3.0<br />-Infraestructura y herramientas de administración gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
-   Al menos dos equipos de acceso remoto se recopilen en una implementación multisitio.   
  
-   Para probar el escenario, se requiere al menos un equipo que ejecuta Windows 8 y configurado como un cliente de DirectAccess. Para probar el escenario para los clientes que ejecutan Windows 7 se requiere al menos un equipo que ejecuta Windows 7.  
  
-   Para equilibrar la carga entre servidores de punto de entrada, es necesario un equilibrador de carga global externo de terceros.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Los requisitos de software para este escenario son los siguientes:  
  
-   Requisitos de software para la implementación de un solo servidor.  
  
-   Además de los requisitos de software para un único servidor hay una serie de requisitos multisite-específicos:  
  
    -   Autenticación de IPsec en los requisitos de que una implementación multisitio de DirectAccess debe implementarse mediante la autenticación de certificado de equipo IPsec. No se admite la opción de realizar la autenticación de IPsec mediante el servidor de acceso remoto como un proxy Kerberos. Se requiere una CA interna para implementar los certificados IPsec.  
  
    -   Red e IP-HTTPS ubicación requisitos-certificados de servidor necesarios para el servidor de ubicación de red e IP-HTTPS deben ser emitidos por una entidad de certificación. No se admite la opción para usar los certificados que se emiten automáticamente y se autofirmado por el servidor de acceso remoto. Los certificados pueden ser emitidos por una CA interna o por una CA externa de terceros.  
  
    -   Requisitos de Active Directory-se requiere al menos un sitio de Active Directory. El servidor de acceso remoto debe estar en el sitio. Para la actualización más rápido, se recomienda que cada sitio tiene un controlador de dominio grabable, aunque esto no es obligatorio.  
  
    -   Grupo de seguridad-requisitos son los siguientes:  
  
        -   Un grupo de seguridad solo se requiere para todos los equipos cliente de Windows 8 de todos los dominios. Se recomienda crear un grupo de seguridad únicos de estos clientes para cada dominio.  
  
        -   Falta un grupo de seguridad único que contiene los equipos de Windows 7 para cada punto de entrada configurado para admitir a clientes de Windows 7. Se recomienda tener un grupo de seguridad único para cada punto de entrada en cada dominio.  
  
        -   Los equipos no deben estar incluidos en más de un grupo de seguridad que incluya clientes de DirectAccess. Si los clientes están incluidos en varios grupos, la resolución de nombres para las solicitudes de los clientes no funcionará según lo esperado.  
  
    -   GPO de los requisitos de GPO pueden crearse manualmente antes de configurar el acceso remoto, o se crea automáticamente durante la implementación de acceso remoto. Los requisitos son los siguientes:  
  
        -   Falta un GPO de cliente único para cada dominio.  
  
        -   Un GPO de servidor se requiere para cada punto de entrada, en el dominio donde se encuentra el punto de entrada. Por lo que si varios puntos de entrada se encuentran en el mismo dominio, habrá varios GPO (uno para cada punto de entrada) en el dominio de servidor.  
  
        -   Un único GPO de cliente de Windows 7 es necesario para cada punto de entrada habilitado para la compatibilidad con clientes de Windows 7, para cada dominio.  
  
## <a name="KnownIssues"></a>Problemas conocidos  
Los siguientes son problemas conocidos al configurar un escenario multisitio:  
  
-   **Varios puntos de entrada en la misma subred IPv4**. Si agrega varios puntos de entrada en la misma subred IPv4 obtendrá un mensaje de conflicto de dirección IP y la dirección DNS64 del punto de entrada no se configurará según lo previsto. Este problema se produce cuando no se ha implementado IPv6 en las interfaces de los servidores de la red corporativa internas. Para evitar este problema, ejecute el siguiente comando de Windows PowerShell en todos los servidores de acceso remoto actuales y futuros:  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   Si la dirección pública especificada para que los clientes de DirectAccess para conectarse al servidor de acceso remoto tiene un sufijo que se incluyen en la tabla NRPT, DirectAccess podrían no funcionar según lo previsto. Asegúrese de que la tabla NRPT tiene una exención para el nombre público. En una implementación multisitio, se deben agregar excepciones para los nombres de todos los puntos de entrada públicos. Tenga en cuenta que if el túnel forzado está habilitado estas excepciones se agregan automáticamente. Se quitan si se deshabilita el túnel forzado.  
  
-   Cuando se usa el cmdlet de Windows PowerShell **Disable DAMultiSite**, los parámetros WhatIf y Confirm no tienen ningún efecto y se deshabilitará la funcionalidad de multisitio y se quitarán los GPO de Windows 7.  
  
-   Cuando los clientes de Windows 7 mediante el DCA en una implementación multisitio se actualizan a Windows 8, el Asistente de conectividad de red no funcionará. Este problema puede solucionarse antes de la actualización del cliente mediante la modificación de los GPO de 7 de Windows con los siguientes cmdlets de Windows PowerShell:  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    En caso de que ya se ha actualizado el cliente, a continuación, mueva el equipo cliente al grupo de seguridad de Windows 8.  
  
-   Cuando se modifica la configuración del controlador de dominio mediante el cmdlet de Windows PowerShell **Set-DAEntryPointDC**, si el parámetro ComputerName especificado es un servidor de acceso remoto en un punto de entrada distinto del último agregado a la funcionalidad de multisitio implementación, será una advertencia que indica que el servidor especificado no se actualizarán hasta la siguiente actualización de directiva de muestra. Se pueden ver los servidores reales que no se actualizaron con la **estado de configuración** en el **panel** de la **consola de administración de acceso remoto**. Esto no causará problemas de funcionamiento, sin embargo, puede ejecutar **gpupdate /force** en los servidores que no se actualizan para obtener el estado de configuración se actualiza inmediatamente.  
  
-   Cuando se implementa la funcionalidad de multisitio en una red corporativa solo IPv4, cambiar el texto interno de red prefijo IPv6 también cambios DNS64 dirección, pero no actualiza la dirección de las reglas de firewall que permiten las consultas DNS para el servicio de DNS64. Para resolver este problema, ejecute los siguientes comandos de Windows PowerShell después de cambiar el prefijo IPv6 de la red interna:  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   Si se ha implementado DirectAccess cuando una infraestructura ISATAP existente estaba presente, al quitar un punto de entrada que era un host ISATAP, la dirección IPv6 de DNS64 service se quitarán las direcciones de servidor DNS de todos los sufijos DNS en la tabla NRPT.  
  
    Para resolver este problema, en el **el programa de instalación del servidor de infraestructura** asistente la **DNS** página, quite los sufijos DNS que se han modificado y agregarlas de nuevo con las direcciones de servidor DNS correctas, haga clic en  **Detectar** en el **direcciones de servidor DNS** cuadro de diálogo.  
  


