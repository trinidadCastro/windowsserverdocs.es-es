---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Requisitos y procedimientos recomendados para la actualización compatible con clústeres
ms.prod: windows-server
ms.topic: article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Requisitos para usar la actualización compatible con clústeres para instalar actualizaciones en clústeres que ejecutan Windows Server.
ms.openlocfilehash: 6062004f094dfa3b412942f44db568459ce61e17
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828028"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Requisitos y procedimientos recomendados para la actualización compatible con clústeres

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En esta sección se describen los requisitos y las dependencias necesarios para usar la [actualización compatible con clústeres](cluster-aware-updating.md) (CAU) para aplicar actualizaciones a un clúster de conmutación por error que ejecuta Windows Server.

> [!NOTE]  
> Es posible que tenga que validar de forma independiente que el entorno de clúster está listo para aplicar actualizaciones si usa un\-de conexión en lugar de **Microsoft. WindowsUpdatePlugin**. Si usa un\-de plug-\-de Microsoft en, póngase en contacto con el editor para obtener más información. Para obtener más información sobre los complementos de\-, vea [Cómo funcionan](cluster-aware-updating-plug-ins.md)los complementos de\-.   

## <a name="install-the-failover-clustering-feature-and-the-failover-clustering-tools"></a><a name="BKMK_REQ_CLUS"></a>Instalar la característica de clúster de conmutación por error y las herramientas de clúster de conmutación por error  
CAU requiere la instalación de la característica Clústeres de conmutación por error y Herramientas de clústeres de conmutación por error. Entre las herramientas de clústeres de conmutación por error se incluyen las herramientas de CAU \(clusterawareupdating. dll\), los cmdlets de clúster de conmutación por error y otros componentes necesarios para las operaciones de CAU. Para ver los pasos para instalar la característica Clústeres de conmutación por error, consulte la [Instalación de la característica y las herramientas de clústeres de conmutación por error](create-failover-cluster.md#install-the-failover-clustering-feature).  

Los requisitos de instalación exactos de las herramientas de clústeres de conmutación por error dependen de si CAU coordina las actualizaciones como rol en clúster en el clúster de conmutación por error \(mediante el modo de actualización de\-propio\) o desde un equipo remoto. Además, el modo de actualización de auto\-de CAU requiere la instalación del rol en clúster de CAU en el clúster de conmutación por error mediante las herramientas de CAU.    

En la tabla siguiente se resumen los requisitos de instalación de características de CAU para los dos modos de actualización de CAU.  

|Componente instalado|Modo de actualización de\-propio|Modo de actualización de\-remoto|  
|-----------------------|-----------------------|-------------------------|  
|Característica Clústeres de conmutación por error|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster|  
|Herramientas de clúster de conmutación por error|Obligatorio en todos los nodos del clúster|-Requerido en el equipo de actualización de\-remoto<br />-Requerido en todos los nodos del clúster para ejecutar el cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)|  
|Rol en clúster de CAU|Obligatorio|No obligatorio|  

## <a name="obtain-an-administrator-account"></a>Obtener una cuenta de administrador  
Los requisitos de administrador siguientes son necesarios para usar las características de CAU.  

-   Para obtener una vista previa de las acciones de actualización o aplicarlas mediante la interfaz de usuario de CAU \(\) de IU o los cmdlets de actualización compatible con clústeres, debe usar una cuenta de dominio que tenga derechos y permisos de administrador local en todos los nodos del clúster. Si la cuenta no tiene privilegios suficientes en cada nodo, se le solicitará en la ventana actualización compatible con clústeres para proporcionar las credenciales necesarias al realizar estas acciones. Para usar los cmdlets [de actualización compatible con clústeres](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) , puede proporcionar las credenciales necesarias como parámetro de cmdlet.  

-   Si usa CAU en modo de actualización de\-remoto cuando inicia sesión con una cuenta que no tiene derechos y permisos de administrador local en los nodos del clúster, debe ejecutar las herramientas de la CAU como administrador mediante una cuenta de administrador local en el equipo coordinador de actualizaciones, o bien mediante una cuenta que tenga el derecho de usuario **Suplantar a un cliente tras la autenticación** . 

-   Para ejecutar el Analizador de procedimientos recomendados de CAU, debe usar una cuenta que tenga privilegios administrativos en los nodos del clúster y privilegios de administrador local en el equipo que se usa para ejecutar el cmdlet [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) o para analizar la preparación de la actualización del clúster mediante la ventana de actualización compatible con clústeres. Para obtener más información, consulta [Probar la preparación del clúster para la actualización](#BKMK_BPA).  

## <a name="verify-the-cluster-configuration"></a>Comprobar la configuración del clúster  
A continuación se incluyen los requisitos generales para que un clúster de conmutación por error admita actualizaciones por medio de CAU. Los requisitos de configuración adicionales para la administración remota en los nodos se incluyen en [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) más adelante en este tema.  

-   Debe haber suficientes nodos del clúster en línea para que el clúster tenga quórum.  

-   Todos los nodos del clúster deben estar en el mismo dominio de Active Directory.  

-   El nombre del clúster se debe resolver en la red por medio de DNS.  

-   Si se usa la CAU en el modo de actualización de\-remoto, el equipo coordinador de actualizaciones debe tener conectividad de red a los nodos de clúster de conmutación por error y debe estar en el mismo dominio Active Directory que el clúster de conmutación por error.  

-   El servicio de clúster debe ejecutarse en todos los nodos del clúster. De manera predeterminada, este servicio se instala en todos los nodos del clúster y se configura para iniciarse automáticamente.  

-   Para usar PowerShell pre\-Update o publicar scripts de actualización\-durante una ejecución de actualización de CAU, asegúrese de que los scripts estén instalados en todos los nodos del clúster o que estén accesibles para todos los nodos, por ejemplo, en un recurso compartido de archivos de red de alta disponibilidad. Si los scripts se dejan en un recurso compartido de archivos de red, configura la carpeta para que el grupo Todos tenga permisos de lectura.  

## <a name="configure-the-nodes-for-remote-management"></a><a name="BKMK_NODE_CONFIG"></a>Configurar los nodos para la administración remota  
Para usar la actualización compatible con clústeres, todos los nodos del clúster deben estar configurados para la administración remota. De forma predeterminada, la única tarea que debe realizar para configurar los nodos para la administración remota es [habilitar una regla de Firewall para permitir los reinicios automáticos](#BKMK_FW). 

En la tabla siguiente se enumeran los requisitos completos de administración remota, en caso de que el entorno difiera de los predeterminados.

Estos requisitos se suman a los requisitos de instalación de [Instalar la característica Clústeres de conmutación por error y Herramientas de clústeres de conmutación por error](#BKMK_REQ_CLUS) y a los requisitos de clústeres generales que se describen en las secciones anteriores de este tema.  

|Requisito|Estado predeterminado|Modo de actualización de\-propio|Modo de actualización de\-remoto|  
|---------------|---|-----------------------|-------------------------|  
|[Habilitar una regla de Firewall para permitir los reinicios automáticos](#BKMK_FW)|Deshabilitado|Obligatorio en todos los nodos del clúster si se usa un firewall|Obligatorio en todos los nodos del clúster si se usa un firewall|  
|[Habilitar Instrumental de administración de Windows](#BKMK_WMI)|Habilitado|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster|  
|[Habilitar Windows PowerShell 3,0 o 4,0 y la comunicación remota de Windows PowerShell](#BKMK_PS)|Habilitado|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster para ejecutar lo siguiente:<p>-El cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-\-PowerShell y los scripts posteriores a la actualización de\-durante una ejecución de actualización<br />-Pruebas de preparación de la actualización de clústeres mediante la ventana de actualización compatible con clústeres o el cmdlet de Windows PowerShell [\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps)|  
|[Instalación de .NET Framework 4,6 o 4,5](#BKMK_NET)|Habilitado|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster para ejecutar lo siguiente:<p>-El cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-\-PowerShell y los scripts posteriores a la actualización de\-durante una ejecución de actualización<br />-Pruebas de preparación de la actualización de clústeres mediante la ventana de actualización compatible con clústeres o el cmdlet de Windows PowerShell [\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps)|  

### <a name="enable-a-firewall-rule-to-allow-automatic-restarts"></a><a name="BKMK_FW"></a>Habilitar una regla de Firewall para permitir los reinicios automáticos  
Para permitir los reinicios automáticos después de aplicar las actualizaciones \(si la instalación de una actualización requiere un reinicio\), si firewall de Windows o un firewall de Microsoft no\-está en uso en los nodos del clúster, se debe habilitar una regla de firewall en cada nodo que permita el tráfico siguiente:  

-   Protocolo: TCP  

-   Dirección: entrada  

-   Programa: wininit.exe  

-   Puertos: puertos dinámicos RPC  

-   Perfil: dominio  

Si se usa Firewall de Windows en los nodos del clúster, puedes hacerlo si habilitas el grupo de reglas de Firewall de Windows **Cierre remoto** en cada nodo del clúster. Cuando se usa la ventana de actualización compatible con clústeres para aplicar actualizaciones y configurar las opciones de actualización de\-, el grupo de reglas de Firewall de Windows de **apagado remoto** se habilita automáticamente en cada nodo del clúster.  

> [!NOTE]  
> El grupo de reglas de Windows Firewall **Remote Shutdown** no se puede habilitar si entra en conflicto con opciones de configuración de directiva de grupo establecidas para Firewall de Windows.    

El grupo de reglas de Firewall de **cierre remoto** también se habilita especificando el parámetro **– EnableFirewallRules** al ejecutar los siguientes cmdlets [de Cau: Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps)y [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  

En el siguiente ejemplo de PowerShell se muestra un método adicional para habilitar los reinicios automáticos en un nodo de clúster.  

```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="enable-windows-management-instrumentation-wmi"></a><a name="BKMK_WMI"></a>Habilitar Instrumental de administración de Windows (WMI) 
Todos los nodos de clúster deben estar configurados para la administración remota mediante Instrumental de administración de Windows \(WMI\). Este valor está habilitado de forma predeterminada.  

Para habilitar la administración remota manualmente, haz lo siguiente:  

1.  En la consola de servicios, inicia el servicio **Administración remota de Windows** y establece el tipo de inicio en **Automático**.  

2.  Ejecute el cmdlet [set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) o ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados:  

    ```PowerShell  
    winrm quickconfig -q  
    ```  

Para admitir la comunicación remota de WMI, si el Firewall de Windows está en uso en los nodos del clúster, la regla de Firewall de entrada para **Administración remota de Windows \(HTTP\-en\)** debe estar habilitada en cada nodo.  De manera predeterminada, esta regla está habilitada.  

### <a name="enable-windows-powershell-and-windows-powershell-remoting"></a><a name="BKMK_PS"></a>Habilitar la comunicación remota de Windows PowerShell y Windows PowerShell  
Para habilitar el modo de actualización de\-self y ciertas características de CAU en modo de actualización remota de\-, PowerShell debe estar instalado y habilitado para ejecutar comandos remotos en todos los nodos del clúster. De forma predeterminada, PowerShell está instalado y habilitado para la comunicación remota.  

Para habilitar la comunicación remota de PowerShell, use uno de los métodos siguientes:  

-   Ejecute el cmdlet [enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) .  

-   Configure un nivel de\-de dominio directiva de grupo configuración para Administración remota de Windows \(WinRM\).  

Para obtener más información sobre cómo habilitar la comunicación remota de PowerShell, consulte [acerca de los requisitos remotos](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  

### <a name="install-net-framework-46-or-45"></a><a name="BKMK_NET"></a>Instalación de .NET Framework 4,6 o 4,5  
Para habilitar el modo de actualización de\-self y ciertas características de CAU en modo de actualización remota de\-, se debe instalar .NET Framework 4,6 o .NET Framework 4,5 (en Windows Server 2012 R2) en todos los nodos del clúster. De forma predeterminada, se instala .NET Framework.  

Para instalar .NET Framework 4,6 (o 4,5) con PowerShell si aún no está instalado, use el siguiente comando:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="best-practices-recommendations-for-using-cluster-aware-updating"></a><a name="BKMK_BEST_PRAC"></a>Prácticas recomendadas para usar la actualización compatible con clústeres 

### <a name="recommendations-for-applying-microsoft-updates"></a><a name="BKMK_BP_WUA"></a>Recomendaciones para aplicar actualizaciones de Microsoft

Se recomienda que, cuando empiece a usar la CAU para aplicar actualizaciones con el\-complemento **Microsoft. WindowsUpdatePlugin** predeterminado en un clúster, deje de usar otros métodos para instalar las actualizaciones de software de Microsoft en los nodos del clúster.  

> [!CAUTION]  
> La combinación de CAU con métodos que actualizan nodos individuales automáticamente \(en una programación de tiempo fija\) puede producir resultados imprevisibles, incluidas interrupciones del servicio y tiempo de inactividad no planeado.  

Recomendamos que siga las pautas siguientes:  

-   Para lograr resultados óptimos, te recomendamos deshabilitar la configuración en los nodos del clúster para actualizaciones automáticas, por ejemplo, por medio de la configuración de Actualizaciones automáticas del Panel de control o en las opciones configuradas por medio de la directiva de grupo.  

    > [!CAUTION]  
    > La instalación automática de actualizaciones en los nodos del clúster puede interferir con la instalación de actualizaciones por parte de CAU y puede causar errores de CAU.  

    Si son necesarias, las siguientes opciones de configuración de Actualizaciones automáticas son compatibles con CAU, porque el administrador puede controlar cuándo se instalan las actualizaciones:  

    -   Configuración para notificar antes de descargar actualizaciones y antes de la instalación  

    -   Configuración para descargar actualizaciones automáticamente y notificar antes de la instalación  

    No obstante, si Actualizaciones automáticas descarga actualizaciones al mismo tiempo que una ejecución de actualización de CAU, la ejecución de actualización podría tardar más en completarse.  

-   No configure un sistema de actualización, como Windows Server Update Services \(\) WSUS para aplicar actualizaciones automáticamente \(en una programación de tiempo fija\) a los nodos del clúster.  

-   Todos los nodos del clúster se deben configurar de manera uniforme para usar el mismo origen de actualización, por ejemplo, un servidor WSUS, Windows Update o Microsoft Update.  

-   Si usa un sistema de administración de configuración para aplicar actualizaciones de software a los equipos de la red, excluya los nodos de clúster de todas las actualizaciones requeridas o automáticas. Entre los ejemplos de sistemas de administración de configuración se incluyen Microsoft Endpoint Configuration Manager y Microsoft System Center Virtual Machine Manager 2008.  

-   Si los servidores de distribución de software internos \(por ejemplo, los servidores WSUS\) se usan para contener e implementar las actualizaciones, asegúrese de que esos servidores identifiquen correctamente las actualizaciones aprobadas para los nodos de clúster.  

#### <a name="apply-microsoft-updates-in-branch-office-scenarios"></a><a name="BKMK_PROXY"></a>Aplicar actualizaciones de Microsoft en escenarios de sucursales  
Para descargar actualizaciones de Microsoft desde Microsoft Update o Windows Update a nodos del clúster en ciertos escenarios de sucursales, es posible que tengas que configurar el proxy para la cuenta de sistema local en cada nodo. Por ejemplo, es posible que lo tengas que hacer si los clústeres de la sucursal tienen acceso a Microsoft Update o a Windows Update para descargar actualizaciones por medio de un servidor proxy local.  

Si es necesario, configure las opciones del proxy WinHTTP en cada nodo para especificar un servidor proxy local y configurar las excepciones de direcciones locales \(es decir, una lista de omisión para las direcciones locales\). Para ello, puedes ejecutar el siguiente comando en cada nodo del clúster desde un símbolo del sistema con privilegios elevados:  

```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  

donde <*ProxyServerFQDN*> es el nombre de dominio completo del servidor proxy y <*Puerto*> es el puerto en el que se va a comunicar (normalmente el puerto 443).  

Por ejemplo, para configurar el proxy WinHTTP para la cuenta de sistema local especificando el servidor proxy *MyProxy.contoso.com*, con el puerto 443 y las excepciones de direcciones locales, escriba el siguiente comando:  

```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  

### <a name="recommendations-for-using-the-microsofthotfixplugin"></a><a name="BKMK_BP_HF"></a>Recomendaciones para usar Microsoft. HotfixPlugin  

-   Te recomendamos que configures los permisos de la carpeta raíz de revisiones y el archivo de configuración de revisiones para restringir el acceso de escritura solo a los administradores locales de los equipos que se usan para almacenar estos archivos. Esto ayuda a impedir que usuarios no autorizados manipulen estos archivos, lo cual podría obstaculizar la funcionalidad del clúster de conmutación por error al aplicar revisiones.  

-   Para garantizar la integridad de los datos del bloque de mensajes del servidor \(SMB\) conexiones que se usan para tener acceso a la carpeta raíz de revisiones, debe configurar el cifrado SMB en la carpeta compartida SMB, si es posible configurarla. El complemento **Microsoft.HotfixPlugin** requiere que la firma SMB o el cifrado SMB estén configurados para garantizar la integridad de los datos para las conexiones SMB. 

    Para obtener más información, vea [restringir el acceso a la carpeta raíz de revisiones y el archivo de configuración de revisiones](cluster-aware-updating-plug-ins.md#BKMK_ACL).

### <a name="additional-recommendations"></a>Recomendaciones adicionales  

-   Para evitar interferir con una ejecución de actualización de CAU que pueda estar programada a la misma hora, no programes cambios de contraseña para los objetos de nombre de clúster y los objetos de equipo virtual durante los períodos de mantenimiento programados.  

-   Debe establecer los permisos adecuados en la actualización previa a la\-y en los scripts de actualización\-que se guardan en carpetas compartidas de red para evitar posibles manipulaciones de estos archivos por parte de usuarios no autorizados.  

-   Para configurar la CAU en el modo de actualización de\-, se debe crear un objeto de equipo virtual \(\) VCO para el rol en clúster de CAU en Active Directory. CAU puede crear este objeto automáticamente en el momento en que se agrega el rol en clúster de CAU si el clúster de conmutación por error tiene permisos suficientes. No obstante, debido a las directivas de seguridad de ciertas organizaciones, es posible que sea necesario preconfigurar el objeto en Active Directory. Para ver un procedimiento sobre cómo hacerlo, consulte los [Pasos para preconfigurar una cuenta para un rol en clúster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  

-   Para guardar y reutilizar la configuración de la ejecución de actualización entre clústeres de conmutación por error con necesidades de actualización parecidas en la organización de TI, puedes crear perfiles de ejecución de actualización. Además, en función del modo de actualización, puedes guardar y administrar los perfiles de ejecución de actualización en un recurso compartido de archivos al que puedan tener acceso todos los equipos coordinadores de actualizaciones remotas o clústeres de conmutación por error. Para obtener más información, consulte [Opciones avanzadas y perfiles de ejecución de actualización para Cau](cluster-aware-updating-options.md).  

## <a name="test-cluster-updating-readiness"></a><a name="BKMK_BPA"></a>Probar la preparación del clúster de actualización  
Puede ejecutar el Analizador de procedimientos recomendados CAU \(modelo de\) BPA para probar si un clúster de conmutación por error y el entorno de red cumplen muchos de los requisitos para que la CAU aplique las actualizaciones de software. Muchas de las pruebas comprueban la preparación del entorno para aplicar actualizaciones de Microsoft mediante el\-de complementos predeterminado en, **Microsoft. WindowsUpdatePlugin**.  

> [!NOTE]  
> Es posible que tenga que validar de forma independiente que el entorno de clúster está listo para aplicar actualizaciones de software mediante un\-de conexión en lugar de **Microsoft. WindowsUpdatePlugin**. Si usa un\-de plug-\-Microsoft en, como el que proporciona el fabricante del hardware, póngase en contacto con el editor para obtener más información.  

Puedes ejecutar el BPA de las dos maneras siguientes:  

1.  Selecciona **Analizar preparación de la actualización del clúster** en la consola de CAU. Una vez que el BPA complete las pruebas de preparación, aparecerá un informe de prueba. Si se detectan problemas en los nodos del clúster, se identifican los problemas específicos y los nodos en los que aparecen para que puedas tomar medidas correctivas. Las pruebas pueden tardar varios minutos en completarse.  

2.  Ejecute el cmdlet [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) . Puede ejecutar el cmdlet en un equipo local o remoto en el que esté instalado el módulo de clústeres de conmutación por error para Windows PowerShell (parte de las herramientas de clúster de conmutación por error). También puedes ejecutar el cmdlet en un nodo del clúster de conmutación por error.  

> [!NOTE]  
> -   Debe usar una cuenta que tenga privilegios administrativos en los nodos del clúster y privilegios de administrador local en el equipo que se usa para ejecutar el cmdlet **Test\-CauSetup** o para analizar la preparación de la actualización del clúster mediante la ventana de actualización compatible con clústeres. Para ejecutar las pruebas mediante la ventana de actualización compatible con clústeres, debe haber iniciado sesión en el equipo con las credenciales necesarias.  
> -   En las pruebas se da por supuesto que las herramientas de CAU que se usan para obtener una vista previa de las actualizaciones de software y aplicarlas se ejecutan desde el mismo equipo y con las mismas credenciales de usuario que se usan para probar la preparación del clúster para la actualización.  

> [!IMPORTANT]  
> Te recomendamos que realices las pruebas para comprobar si el clúster está preparado para la actualización en los siguientes casos:  
>   
> -   Antes de usar CAU por primera vez para aplicar actualizaciones de software.  
> -   Después de añadir un nodo al clúster o de realizar otros cambios de hardware en el clúster que requieran la ejecución del Asistente para validar un clúster.  
> -   Después de cambiar un origen de actualización o cambiar la configuración o configuraciones de actualización \(que no sean de CAU\) que puedan afectar a la aplicación de actualizaciones en los nodos.  

### <a name="tests-for-cluster-updating-readiness"></a>Pruebas de la preparación del clúster para la actualización  
En la tabla siguiente se incluyen las pruebas de la preparación del clúster para la actualización, algunos problemas habituales y pasos para solucionarlos.  


|                                                      Probar                                                      |                                                                                                                                               Posibles problemas y consecuencias                                                                                                                                               |                                                                                                                                                                                         Pasos de resolución                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     El clúster de conmutación por error debe estar disponible                                     |                                                                                       No se puede resolver el nombre del clúster de conmutación por error o no se puede tener acceso a uno o más nodos del clúster. El BPA no puede ejecutar las pruebas de preparación del clúster.                                                                                        |                                                                   -Compruebe la ortografía del nombre del clúster especificado durante la ejecución del BPA.<br />-Asegúrese de que todos los nodos del clúster estén en línea y en ejecución.<br />-Compruebe que el Asistente para validar una configuración puede ejecutarse correctamente en el clúster de conmutación por error.                                                                    |
|                    Los nodos de clúster de conmutación por error deben estar habilitados para la administración remota mediante WMI                    |                                                Uno o varios nodos de clúster de conmutación por error no están habilitados para la administración remota mediante Instrumental de administración de Windows \(WMI\). CAU no puede actualizar los nodos del clúster si no están configurados para la administración remota.                                                 |                                                                                                  Asegúrate de que todos los nodos del clúster de conmutación por error habilitados para la administración remota por medio de WMI. Para obtener más información, consulta [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) en este tema.                                                                                                   |
|                      La comunicación remota de PowerShell debe estar habilitada en cada nodo de clúster de conmutación por error                       |                                                           PowerShell no está instalado o no está habilitado para la comunicación remota en uno o varios nodos de clúster de conmutación por error. La CAU no se puede configurar para el modo de actualización\-o usar determinadas características en el modo de actualización de\-remoto.                                                            |                                                                                             Asegúrese de que PowerShell está instalado en todos los nodos del clúster y está habilitado para la comunicación remota.<p>Para obtener más información, consulta [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) en este tema.                                                                                             |
|                                            Versión del clúster de conmutación por error                                            |                                                                            Uno o más nodos del clúster de conmutación por error no ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. CAU no puede actualizar el clúster de conmutación por error.                                                                             |                                                                   Compruebe que el clúster de conmutación por error que se especifica durante la ejecución del BPA ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.<p>Para obtener más información, consulta [Comprobar la configuración del clúster](#BKMK_REQ_CLUS) en este tema.                                                                   |
| Las versiones de .NET Framework y Windows PowerShell requeridas deben estar instaladas en todos los nodos de clúster de conmutación por error |                                                                                              .NET Framework 4,6, 4,5 o Windows PowerShell no está instalado en uno o varios nodos del clúster. Es posible que algunas características de CAU no funcionen.                                                                                              |                                                                            Asegúrese de que .NET Framework 4,6 o 4,5 y Windows PowerShell están instalados en todos los nodos del clúster, si son necesarios.<p>Para obtener más información, consulta [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) en este tema.                                                                             |
|                           El servicio de clúster debe ejecutarse en todos los nodos de clúster                           |                                                                                                            El servicio de clúster no se ejecuta en uno o más nodos. CAU no puede actualizar el clúster de conmutación por error.                                                                                                             |                        -Asegúrese de que se haya iniciado el Servicio de clúster \(ClusSvc\) en todos los nodos del clúster y que esté configurado para iniciarse automáticamente.<br />-Compruebe que el Asistente para validar una configuración puede ejecutarse correctamente en el clúster de conmutación por error.<p>Para obtener más información, consulta [Comprobar la configuración del clúster](#BKMK_REQ_CLUS) en este tema.                         |
|     Las actualizaciones automáticas no deben estar configuradas para la instalación automática de actualizaciones en ningún nodo de clúster de conmutación por error     |                                           En un nodo del clúster del conmutación por error como mínimo, Actualizaciones automáticas se ha configurado para instalar automáticamente actualizaciones de Microsoft en ese nodo. La combinación de CAU con otros métodos de actualización puede tener como consecuencia tiempo de inactividad no planeado o resultados impredecibles.                                            |                                                     Si la funcionalidad de Windows Update se ha configurado para Actualizaciones automáticas en uno o más nodos del clúster, asegúrate de que Actualizaciones automáticas no se haya configurado para instalar las actualizaciones automáticamente.<p>Para obtener más información, consulta [Recomendaciones para aplicar actualizaciones de Microsoft](#BKMK_BP_WUA).                                                     |
|                          Los nodos de clúster de conmutación por error deben usar el mismo origen de actualización                          |                                                    Uno o más nodos del clúster de conmutación por error están configurados para usar un origen de actualización para las actualizaciones de Microsoft distinto del resto de nodos. Es posible que CAU no aplique de manera uniforme las actualizaciones en los nodos del clúster.                                                    |                                                                        Asegúrate de que cada nodo del clúster esté configurado para usar el mismo origen de actualización, por ejemplo, un servidor WSUS, Windows Update o Microsoft Update.<p>Para obtener más información, consulta [Recomendaciones para aplicar actualizaciones de Microsoft](#BKMK_BP_WUA).                                                                         |
|       Debe habilitarse una regla de firewall que permita el apagado remoto en cada nodo del clúster de conmutación por error       |                 Uno o más nodos del clúster de conmutación por error no tienen una regla de firewall habilitada que permita el apagado remoto, o bien, una configuración de directiva de grupo impide habilitar esta regla. Es posible que una ejecución de actualización que aplique actualizaciones que requieran reiniciar los nodos automáticamente no se complete correctamente.                  |                                                                    Si el Firewall de Windows o un firewall de Microsoft no\-está en uso en los nodos del clúster, configure una regla de firewall que permita el apagado remoto.<p>Para obtener más información, consulta [Habilitar una regla de firewall para permitir los reinicios automáticos](#BKMK_FW) en este tema.                                                                    |
|          La configuración de servidor proxy de cada nodo de clúster de conmutación por error debe estar establecido en un servidor proxy local          |                             Uno o más nodos del clúster de conmutación por error tienen una configuración incorrecta de servidor proxy.<p>Si se usa un servidor proxy local, la configuración de servidor proxy en cada nodo debe estar definida correctamente para que el clúster pueda tener acceso a Microsoft Update o Windows Update.                              |                                            Asegúrate de que la configuración de proxy WinHTTP en cada nodo del clúster esté establecida en un servidor proxy local si es necesario. Si no se usa un servidor proxy en el entorno, esta advertencia se puede ignorar.<p>Para obtener más información, consulta [Aplicar actualizaciones de Microsoft en escenarios de sucursales](#BKMK_PROXY) en este tema.                                            |
|        El rol en clúster de CAU debe instalarse en el clúster de conmutación por error para habilitar el modo de actualización de\-propio        |                                                                                                   El rol en clúster de CAU no está instalado en este clúster de conmutación por error. Este rol es necesario para la actualización del\-del clúster.                                                                                                   |      Para usar la CAU en modo de actualización de\-, agregue el rol en clúster de CAU en el clúster de conmutación por error de una de las siguientes maneras:<p>-Ejecute el cmdlet [Add-CauClusterRole de](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) PowerShell.<br />-Seleccione la acción **configurar opciones de actualización de\-de clúster** en la ventana de actualización compatible con clústeres.      |
|         El rol en clúster de CAU debe estar habilitado en el clúster de conmutación por error para habilitar el modo de actualización de\-propio         | El rol en clúster de CAU está deshabilitado. Por ejemplo, el rol en clúster de CAU no está instalado o se ha deshabilitado mediante el cmdlet de PowerShell [Disable\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) . Este rol es necesario para la actualización del\-del clúster. | Para usar la CAU en el modo de actualización de\-, habilite el rol en clúster de CAU en este clúster de conmutación por error de una de las siguientes maneras:<p>-Ejecute el cmdlet [enable-CauClusterRole de](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) PowerShell.<br />-Seleccione la acción **configurar opciones de actualización de\-de clúster** en la ventana de actualización compatible con clústeres. |
|      El\-de complemento de CAU configurado para el modo de actualización de auto\-debe estar registrado en todos los nodos de clúster de conmutación por error      |                                                              El rol en clúster de CAU en uno o más nodos de este clúster de conmutación por error no puede tener acceso a la\-de la extracción de CAU en el módulo que está configurado en las opciones de actualización de auto\-. Podría producirse un error en una ejecución de actualización de\-.                                                              |           -Asegúrese de que el\-de complemento de la CAU configurado en está instalado en todos los nodos del clúster mediante el procedimiento de instalación para el producto que proporciona el\-de complemento de la CAU en.<br />-Ejecute el cmdlet [Register\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) de PowerShell para registrar el\-de plug-in en los nodos de clúster necesarios.           |
|                Todos los nodos de clúster de conmutación por error deben tener el mismo conjunto de complementos de\-CAU registrados                 |                                                                             Podría producirse un error en una ejecución de actualización\-da si el\-de conexión de que está configurado para usarse en una ejecución de actualización se cambia a uno que no está disponible en todos los nodos del clúster.                                                                              |           -Asegúrese de que el\-de complemento de la CAU configurado en está instalado en todos los nodos del clúster mediante el procedimiento de instalación para el producto que proporciona el\-de complemento de la CAU en.<br />-Ejecute el cmdlet [Register\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) de PowerShell para registrar el\-de plug-in en los nodos de clúster necesarios.           |
|                               Las opciones de ejecución de actualización configuradas deben ser válidas                                |                                                                          La programación de actualización de auto\-y las opciones de ejecución de actualización configuradas para este clúster de conmutación por error están incompletas o no son válidas. Podría producirse un error en una ejecución de actualización de\-.                                                                           |                                                            Configure una programación de actualización de auto\-válida y un conjunto de opciones de ejecución de actualización. Por ejemplo, puede usar el cmdlet [Set\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) de PowerShell para configurar el rol en clúster de Cau.                                                            |
|                  Al menos dos nodos de clúster de conmutación por error deben ser propietarios del rol en clúster de CAU                  |                                                                                        Se producirá un error en una ejecución de actualización iniciada en el modo de actualización de\-, porque el rol en clúster de CAU no tiene un nodo propietario posible al que moverse.                                                                                         |                                                                                                                Usa Herramientas de clústeres de conmutación por error para asegurarte de que todos los nodos del clúster estén configurados como posibles propietarios del rol en clúster de CAU. Es la configuración predeterminada.                                                                                                                |
|                  Todos los nodos de clúster de conmutación por error deben poder obtener acceso a scripts de Windows PowerShell                  |                                                                        No todos los posibles nodos propietarios del rol en clúster de CAU pueden tener acceso a los scripts de actualización de Windows PowerShell anteriores\-y post\-. Se producirá un error en una ejecución de actualización de\-.                                                                        |                                                                                                                    Asegúrese de que todos los posibles nodos propietarios del rol en clúster de la CAU tengan permisos para acceder a los scripts de actualización\-de PowerShell configurados y post\-.                                                                                                                     |
|                   Todos los nodos de clúster de conmutación por error deben usar scripts de Windows PowerShell idénticos                   |                                                     No todos los posibles nodos propietarios del rol en clúster de CAU usan la misma copia de los scripts de Windows PowerShell anteriores\-Update y post\-Update. Una ejecución de actualización de\-Self podría producir un error o mostrar un comportamiento inesperado.                                                     |                                                                                                                                   Asegúrese de que todos los posibles nodos propietarios del rol en clúster de CAU usan los mismos scripts de PowerShell anteriores\-y posteriores a la actualización de\-.                                                                                                                                   |
|         El valor de WarnAfter debe ser menor que el valor de StopAfter para la ejecución de actualización         |                                                                           Los valores de tiempo de espera de la ejecución de actualización de CAU especificada dejan sin efecto al tiempo de espera de advertencia. Es posible que una ejecución de actualización se cancele antes de que se pueda generar un registro de eventos de advertencia.                                                                            |                                                                                                                                      En las opciones de la ejecución de advertencia, configura un valor de la opción **WarnAfter** que sea menor que el valor de la opción **StopAfter**.                                                                                                                                       |

## <a name="see-also"></a>Vea también  

-   [Información general sobre la actualización compatible con clústeres](cluster-aware-updating.md)