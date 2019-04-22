---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Requisitos para la actualización compatible con clústeres y los procedimientos recomendados
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Requisitos para usar la actualización de clústeres para instalar actualizaciones en los clústeres que ejecutan Windows Server.
ms.openlocfilehash: 379c3caa39b09e8a912150f2423190e143991c05
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819736"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Requisitos para la actualización compatible con clústeres y los procedimientos recomendados

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta sección describen los requisitos y las dependencias que son necesarios para usar [Cluster-Aware Updating](cluster-aware-updating.md) (CAU) para aplicar actualizaciones a un clúster de conmutación por error que ejecuta Windows Server.
  
> [!NOTE]  
> Es posible que deba validar de manera independiente que el entorno de clúster está preparado para aplicar actualizaciones si usas un enchufe\-en distinto **Microsoft.WindowsUpdatePlugin**. Si está utilizando sin\-plug Microsoft\-, póngase en contacto con el publicador para obtener más información. Para obtener más información acerca de plug\-ins, consulte [cómo conectar\-funcionan los complementos](cluster-aware-updating-plug-ins.md).   
  
## <a name="BKMK_REQ_CLUS"></a>Instalar la característica de agrupación en clústeres de conmutación por error y las herramientas de clústeres de conmutación por error  
CAU requiere la instalación de la característica Clústeres de conmutación por error y Herramientas de clústeres de conmutación por error. Las herramientas de clústeres de conmutación por error incluyen las herramientas de CAU \(clusterawareupdating.dll\), los cmdlets de agrupación en clústeres de conmutación por error y otros componentes necesarios para las operaciones de CAU. Para ver los pasos para instalar la característica Clústeres de conmutación por error, consulte la [Instalación de la característica y las herramientas de clústeres de conmutación por error](create-failover-cluster.md#install-the-failover-clustering-feature).  
  
Los requisitos de instalación exactos de las herramientas de clústeres de conmutación por error dependen de si CAU coordina las actualizaciones como un rol en clúster en el clúster de conmutación por error \(por automático\-modo de actualización\) o desde un equipo remoto. El autoservicio\-actualizando además el modo de la CAU requiere la instalación del rol en clúster de CAU en el clúster de conmutación por error mediante el uso de las herramientas de CAU.    
  
En la tabla siguiente se resumen los requisitos de instalación de características de CAU para los dos modos de actualización de CAU.  
  
|Componente instalado|Self\-modo de actualización|Remoto\-modo de actualización|  
|-----------------------|-----------------------|-------------------------|  
|Característica Clústeres de conmutación por error|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster|  
|Herramientas de clústeres de conmutación por error|Obligatorio en todos los nodos del clúster|-Required remoto\-Actualizar equipo<br />-Obligatorio en todos los nodos del clúster para ejecutar el [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet|  
|Rol en clúster de CAU|Requerido|No obligatorio|  
  
## <a name="obtain-an-administrator-account"></a>Obtener una cuenta de administrador  
Los requisitos de administrador siguientes son necesarios para usar las características de CAU.  
  
-   Para obtener una vista previa o aplicar las acciones de actualización mediante la interfaz de usuario CAU \(UI\) o los cmdlets de actualización de clústeres, debe usar una cuenta de dominio que tenga permisos y derechos de administrador local en todos los nodos del clúster. Si la cuenta no tiene privilegios suficientes en cada nodo, se le pida en la ventana de actualización de clústeres para proporcionar las credenciales necesarias al realizar estas acciones. Para usar el [Cluster-Aware Updating](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) cmdlets, puede proporcionar las credenciales necesarias como parámetro del cmdlet.  
  
-   Si usas CAU en remoto\-modo de actualización cuando haya iniciado sesión con una cuenta que no tiene los permisos y derechos de administrador local en los nodos del clúster, debe ejecutar las herramientas de CAU como administrador mediante el uso de una cuenta de administrador local en el Actualizar el equipo coordinador o usando una cuenta que tenga el **suplantar un cliente tras la autenticación** derecho de usuario. 
  
-   Para ejecutar la herramienta Best Practices Analyzer de CAU, debe usar una cuenta que tenga privilegios administrativos en los nodos del clúster y privilegios de administrador local en el equipo que se usa para ejecutar el [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet o para analizar preparación para la actualización mediante la ventana de actualización de clústeres del clúster. Para obtener más información, consulta [Probar la preparación del clúster para la actualización](#BKMK_BPA).  
  
## <a name="verify-the-cluster-configuration"></a>Comprobar la configuración del clúster  
A continuación se incluyen los requisitos generales para que un clúster de conmutación por error admita actualizaciones por medio de CAU. Los requisitos de configuración adicionales para la administración remota en los nodos se incluyen en [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) más adelante en este tema.  
  
-   Debe haber suficientes nodos del clúster en línea para que el clúster tenga quórum.  
  
-   Todos los nodos del clúster deben estar en el mismo dominio de Active Directory.  
  
-   El nombre del clúster se debe resolver en la red por medio de DNS.  
  
-   Si se usa CAU en remoto\-modo de actualización, el equipo Coordinador de actualizaciones debe tener conectividad de red a los nodos del clúster de conmutación por error y debe ser en el mismo dominio de Active Directory que el clúster de conmutación por error.  
  
-   El servicio de clúster debe ejecutarse en todos los nodos del clúster. De manera predeterminada, este servicio se instala en todos los nodos del clúster y se configura para iniciarse automáticamente.  
  
-   Usar PowerShell pre\-actualizar o registrar\-actualización durante una ejecución de actualización de CAU, asegúrese de que los scripts estén instalados en todos los nodos del clúster o que son accesibles para todos los nodos, por ejemplo, en un recurso compartido de red de alta disponibilidad. Si los scripts se dejan en un recurso compartido de archivos de red, configura la carpeta para que el grupo Todos tenga permisos de lectura.  
  
## <a name="BKMK_NODE_CONFIG"></a>Configurar los nodos para la administración remota  
Para usar la actualización compatible con clúster, todos los nodos del clúster deben configurarse para la administración remota. De forma predeterminada, la única tarea que debe realizar para configurar los nodos de administración remota consiste en [habilitar una regla de firewall para permitir los reinicios automáticos](#BKMK_FW). 

En la tabla siguiente se enumera los requisitos de administración remota completa, en caso de que su entorno difiere de los valores predeterminados.

Estos requisitos se suman a los requisitos de instalación de [Instalar la característica Clústeres de conmutación por error y Herramientas de clústeres de conmutación por error](#BKMK_REQ_CLUS) y a los requisitos de clústeres generales que se describen en las secciones anteriores de este tema.  
  
|Requisitos|Estado predeterminado|Self\-modo de actualización|Remoto\-modo de actualización|  
|---------------|---|-----------------------|-------------------------|  
|[Habilitar una regla de firewall para permitir los reinicios automáticos](#BKMK_FW)|Deshabilitada|Obligatorio en todos los nodos del clúster si se usa un firewall|Obligatorio en todos los nodos del clúster si se usa un firewall|  
|[Habilitar Instrumental de administración de Windows](#BKMK_WMI)|Enabled|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster|  
|[Habilitar Windows PowerShell 3.0 o 4.0 y la comunicación remota de Windows PowerShell](#BKMK_PS)|Enabled|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster para ejecutar lo siguiente:<br /><br />-El [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-Pre PowerShell\-actualizar y registrar\-actualización durante una ejecución de actualización<br />-Las pruebas de disponibilidad mediante la ventana de actualización de clústeres de actualización del clúster o el [prueba\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet de Windows PowerShell|  
|[Instale .NET Framework 4.6 o 4.5](#BKMK_NET)|Enabled|Obligatorio en todos los nodos del clúster|Obligatorio en todos los nodos del clúster para ejecutar lo siguiente:<br /><br />-El [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-Pre PowerShell\-actualizar y registrar\-actualización durante una ejecución de actualización<br />-Las pruebas de disponibilidad mediante la ventana de actualización de clústeres de actualización del clúster o el [prueba\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet de Windows PowerShell|  

### <a name="BKMK_FW"></a>Habilitar una regla de firewall para permitir los reinicios automáticos  
Para permitir los reinicios automáticos después de aplican las actualizaciones \(si la instalación de una actualización requiere un reinicio\), si el Firewall de Windows o sin\-firewall de Microsoft está en uso en los nodos del clúster, una regla de firewall debe estar habilitada en cada nodo que permita el tráfico siguiente:  
  
-   Protocolo: TCP  
  
-   Dirección: entrada  
  
-   Programa: wininit.exe  
  
-   Puertos: Puertos dinámicos RPC  
  
-   Perfil: Dominio  
  
Si se usa Firewall de Windows en los nodos del clúster, puedes hacerlo si habilitas el grupo de reglas de Firewall de Windows **Cierre remoto** en cada nodo del clúster. Cuando la ventana de actualización de clústeres se usa para aplicar actualizaciones y configurar self\-actualizar las opciones, el **apagado remoto** grupo de reglas de Firewall de Windows se habilita automáticamente en cada nodo del clúster.  
  
> [!NOTE]  
> El grupo de reglas de Windows Firewall **Remote Shutdown** no se puede habilitar si entra en conflicto con opciones de configuración de directiva de grupo establecidas para Firewall de Windows.    
  
El **apagado remoto** grupo de reglas de firewall también se habilita especificando el **– EnableFirewallRules** parámetro al ejecutar los cmdlets siguientes: [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps), y [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  
  
El siguiente ejemplo de PowerShell muestra un método adicional para permitir reinicios automáticos en un nodo del clúster.  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Habilitar Instrumental de administración de Windows (WMI) 
Todos los nodos del clúster deben configurarse para la administración remota mediante Instrumental de administración de Windows \(WMI\). Está habilitado de manera predeterminada.  
  
Para habilitar la administración remota manualmente, haz lo siguiente:  
  
1.  En la consola de servicios, inicia el servicio **Administración remota de Windows** y establece el tipo de inicio en **Automático**.  
  
2.  Ejecute el [Set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) cmdlet o ejecute el siguiente comando con privilegios elevados desde un símbolo del sistema:  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
Para admitir la comunicación remota de WMI, si el Firewall de Windows está en uso en los nodos del clúster, el firewall de entrada de regla para **administración remota de Windows \(HTTP\-en\)**  debe estar habilitado en cada nodo.  De manera predeterminada, esta regla está habilitada.  
  
### <a name="BKMK_PS"></a>Habilitar Windows PowerShell y la comunicación remota de Windows PowerShell  
Para habilitar el autoservicio\-actualizar modo y ciertas características CAU en remoto\-modo de actualización, PowerShell debe estar instalado y habilitado para ejecutar comandos remotos en todos los nodos del clúster. De forma predeterminada, PowerShell está instalado y habilitado para la comunicación remota.  
  
Para habilitar la comunicación remota de PowerShell, use uno de los métodos siguientes:  
  
-   Ejecute el [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) cmdlet.  
  
-   Configurar un dominio\-configuración de directiva de grupo para la administración remota de Windows de nivel \(WinRM\).  
  
Para obtener más información acerca de cómo habilitar la comunicación remota de PowerShell, consulte [requisitos de remoto](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  
  
### <a name="BKMK_NET"></a>Instale .NET Framework 4.6 o 4.5  
Para habilitar el autoservicio\-actualizar modo y ciertas características CAU en remoto\-actualización de modo, .NET Framework 4.6 o .NET Framework 4.5 (en Windows Server 2012 R2) debe estar instalada en todos los nodos del clúster. De forma predeterminada, se instala .NET Framework.  

Para instalar .NET Framework 4.6 (o 4.5) mediante PowerShell si todavía no está instalado, use el siguiente comando:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Procedimientos recomendados para usar la actualización compatible con clústeres 
  
### <a name="BKMK_BP_WUA"></a>Recomendaciones para aplicar actualizaciones de Microsoft

Se recomienda que al comenzar a usar CAU para aplicar actualizaciones con el valor predeterminado **Microsoft.WindowsUpdatePlugin** enchufe\-en un clúster, deja de usar otros métodos para instalar las actualizaciones de software de Microsoft en el clúster nodos.  
  
> [!CAUTION]  
> La combinación de CAU con otros métodos que actualizan nodos individuales automáticamente \(según una programación de tiempo fijo\) puede causar resultados imprevisibles, incluidas interrupciones del servicio y el tiempo de inactividad imprevisto.  
  
Se recomienda seguir estas instrucciones:  
  
-   Para lograr resultados óptimos, te recomendamos deshabilitar la configuración en los nodos del clúster para actualizaciones automáticas, por ejemplo, por medio de la configuración de Actualizaciones automáticas del Panel de control o en las opciones configuradas por medio de la directiva de grupo.  
  
    > [!CAUTION]  
    > La instalación automática de actualizaciones en los nodos del clúster puede interferir con la instalación de actualizaciones por parte de CAU y puede causar errores de CAU.  
  
    Si son necesarias, las siguientes opciones de configuración de Actualizaciones automáticas son compatibles con CAU, porque el administrador puede controlar cuándo se instalan las actualizaciones:  
  
    -   Configuración para notificar antes de descargar actualizaciones y antes de la instalación  
  
    -   Configuración para descargar actualizaciones automáticamente y notificar antes de la instalación  
  
    No obstante, si Actualizaciones automáticas descarga actualizaciones al mismo tiempo que una ejecución de actualización de CAU, la ejecución de actualización podría tardar más en completarse.  
  
-   No configure un sistema de actualizaciones como Windows Server Update Services \(WSUS\) para aplicar actualizaciones automáticamente \(según una programación de tiempo fijo\) a nodos del clúster.  
  
-   Todos los nodos del clúster se deben configurar de manera uniforme para usar el mismo origen de actualización, por ejemplo, un servidor WSUS, Windows Update o Microsoft Update.  
  
-   Si usa un sistema de administración de configuración para aplicar actualizaciones de software a los equipos de la red, excluya los nodos de clúster de todas las actualizaciones requeridas o automáticas. Algunos ejemplos de sistemas de administración de configuración incluyen Microsoft System Center Configuration Manager 2007 y Microsoft System Center Virtual Machine Manager 2008.  
  
-   Si los servidores de distribución de software interno \(por ejemplo, los servidores WSUS\) se utilizan para contener e implementar las actualizaciones, asegúrese de que estos identifiquen correctamente las actualizaciones aprobadas para los nodos del clúster.  
  
#### <a name="BKMK_PROXY"></a>Aplicar actualizaciones de Microsoft en escenarios de sucursales  
Para descargar actualizaciones de Microsoft desde Microsoft Update o Windows Update a nodos del clúster en ciertos escenarios de sucursales, es posible que tengas que configurar el proxy para la cuenta de sistema local en cada nodo. Por ejemplo, es posible que lo tengas que hacer si los clústeres de la sucursal tienen acceso a Microsoft Update o a Windows Update para descargar actualizaciones por medio de un servidor proxy local.  
  
Si es necesario, configure la configuración del proxy WinHTTP en cada nodo para especificar un servidor proxy local y configurar excepciones de direcciones locales \(es decir, una lista de omisión para direcciones locales\). Para ello, puedes ejecutar el siguiente comando en cada nodo del clúster desde un símbolo del sistema con privilegios elevados:  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
donde <*ProxyServerFQDN*> es el nombre de dominio completo del servidor proxy y <*puerto*> es el puerto en el que se comunican (normalmente, el puerto 443).  
  
Por ejemplo, para configurar la configuración del proxy WinHTTP para la cuenta de sistema Local especificando el servidor proxy *MyProxy.CONTOSO.com*, con el puerto 443 y excepciones de direcciones local, escriba el siguiente comando:  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>Recomendaciones para usar Microsoft.HotfixPlugin  
  
-   Te recomendamos que configures los permisos de la carpeta raíz de revisiones y el archivo de configuración de revisiones para restringir el acceso de escritura solo a los administradores locales de los equipos que se usan para almacenar estos archivos. Esto ayuda a impedir que usuarios no autorizados manipulen estos archivos, lo cual podría obstaculizar la funcionalidad del clúster de conmutación por error al aplicar revisiones.  
  
-   Para ayudar a garantizar la integridad de los datos para el bloque de mensajes del servidor \(SMB\) las conexiones que se usan para tener acceso a la carpeta raíz de revisiones, debe configurar el cifrado SMB en la carpeta compartida SMB, si es posible configurarlo. El complemento **Microsoft.HotfixPlugin** requiere que la firma SMB o el cifrado SMB estén configurados para garantizar la integridad de los datos para las conexiones SMB. 
  
    Para obtener más información, consulte [restringir el acceso a la carpeta raíz de revisiones y el archivo de configuración de revisiones](cluster-aware-updating-plug-ins.md#BKMK_ACL).
  
### <a name="additional-recommendations"></a>Recomendaciones adicionales  
  
-   Para evitar interferir con una ejecución de actualización de CAU que pueda estar programada a la misma hora, no programes cambios de contraseña para los objetos de nombre de clúster y los objetos de equipo virtual durante los períodos de mantenimiento programados.  
  
-   Debe establecer los permisos adecuados en pre\-actualizar y registrar\-los scripts de actualización que se guardan en la red comparten carpetas para impedir posibles manipulaciones de estos archivos, los usuarios no autorizados.  
  
-   Para configurar CAU en self\-actualizar el modo de un objeto de equipo virtual \(VCO\) para la CAU se debe crear el rol en clúster en Active Directory. CAU puede crear este objeto automáticamente en el momento en que se agrega el rol en clúster de CAU si el clúster de conmutación por error tiene permisos suficientes. No obstante, debido a las directivas de seguridad de ciertas organizaciones, es posible que sea necesario preconfigurar el objeto en Active Directory. Para ver un procedimiento sobre cómo hacerlo, consulte los [Pasos para preconfigurar una cuenta para un rol en clúster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  
  
-   Para guardar y reutilizar la configuración de la ejecución de actualización entre clústeres de conmutación por error con necesidades de actualización parecidas en la organización de TI, puedes crear perfiles de ejecución de actualización. Además, en función del modo de actualización, puedes guardar y administrar los perfiles de ejecución de actualización en un recurso compartido de archivos al que puedan tener acceso todos los equipos coordinadores de actualizaciones remotas o clústeres de conmutación por error. Para obtener más información, consulte [opciones avanzadas y perfiles de ejecución actualización para CAU](cluster-aware-updating-options.md).  
  
## <a name="BKMK_BPA"></a>Preparación de la actualización de clúster de prueba  
Puede ejecutar la herramienta Best Practices Analyzer de CAU \(BPA\) modelo para comprobar si un clúster de conmutación por error y el entorno de red satisfacen muchos de los requisitos para las actualizaciones de software que se aplican por parte de CAU. Muchas de las pruebas comprueban la preparación del entorno para aplicar las actualizaciones de Microsoft utilizando el enchufe predeterminada\-en, **Microsoft.WindowsUpdatePlugin**.  
  
> [!NOTE]  
> Es posible que deba validar de manera independiente que el entorno de clúster está preparado para aplicar las actualizaciones de software mediante el uso de un enchufe\-en distinto **Microsoft.WindowsUpdatePlugin**. Si está utilizando sin\-plug Microsoft\-, por ejemplo, uno proporcionado por el fabricante del hardware, póngase en contacto con el publicador para obtener más información.  
  
Puedes ejecutar el BPA de las dos maneras siguientes:  
  
1.  Selecciona **Analizar preparación de la actualización del clúster** en la consola de CAU. Después de que el BPA complete las pruebas de preparación, aparece un informe de prueba. Si se detectan problemas en los nodos del clúster, se identifican los problemas específicos y los nodos en los que aparecen para que puedas tomar medidas correctivas. Las pruebas pueden tardar varios minutos en completarse.  
  
2.  Ejecute el [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) cmdlet. Puede ejecutar el cmdlet en un equipo local o remoto en el que se instala la conmutación por error de clústeres de módulo para Windows PowerShell (parte de las herramientas de clústeres de conmutación por error). También puedes ejecutar el cmdlet en un nodo del clúster de conmutación por error.  
  
> [!NOTE]  
> -   Debe usar una cuenta que tenga privilegios administrativos en los nodos del clúster y privilegios de administrador local en el equipo que se usa para ejecutar el **prueba\-CauSetup** cmdlet o para analizar la preparación de la actualización del clúster uso de la ventana de actualización de clústeres. Para ejecutar las pruebas mediante la ventana de actualización de clústeres, debe haber iniciado en el equipo con las credenciales necesarias.  
> -   En las pruebas se da por supuesto que las herramientas de CAU que se usan para obtener una vista previa de las actualizaciones de software y aplicarlas se ejecutan desde el mismo equipo y con las mismas credenciales de usuario que se usan para probar la preparación del clúster para la actualización.  
  
> [!IMPORTANT]  
> Te recomendamos que realices las pruebas para comprobar si el clúster está preparado para la actualización en los siguientes casos:  
>   
> -   Antes de usar CAU por primera vez para aplicar actualizaciones de software.  
> -   Después de añadir un nodo al clúster o de realizar otros cambios de hardware en el clúster que requieran la ejecución del Asistente para validar un clúster.  
> -   Después de cambiar un origen de actualización, o cambiar la configuración de actualización o configuraciones \(aparte de CAU\) que pueden afectar a la aplicación de actualizaciones en los nodos.  
  
### <a name="tests-for-cluster-updating-readiness"></a>Pruebas de la preparación del clúster para la actualización  
En la tabla siguiente se incluyen las pruebas de la preparación del clúster para la actualización, algunos problemas habituales y pasos para solucionarlos.  
  
|Prueba|Posibles problemas y consecuencias|Pasos de resolución|  
|--------|-------------------------------|--------------------|  
|El clúster de conmutación por error debe estar disponible|No se puede resolver el nombre del clúster de conmutación por error o no se puede tener acceso a uno o más nodos del clúster. El BPA no puede ejecutar las pruebas de preparación del clúster.|-Compruebe la ortografía del nombre del clúster especificado durante la ejecución del BPA.<br />-Asegúrese de que todos los nodos del clúster están en línea y en ejecución.<br />-Compruebe que el validar un asistente de configuración puede ejecutar correctamente en el clúster de conmutación por error.|  
|Los nodos de clúster de conmutación por error deben estar habilitados para la administración remota mediante WMI|Uno o varios nodos de clúster de conmutación por error no están habilitados para la administración remota mediante el uso de Instrumental de administración de Windows \(WMI\). CAU no puede actualizar los nodos del clúster si no están configurados para la administración remota.|Asegúrate de que todos los nodos del clúster de conmutación por error habilitados para la administración remota por medio de WMI. Para obtener más información, consulta [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) en este tema.|  
|Comunicación remota de PowerShell debe estar habilitado en cada nodo del clúster de conmutación por error|PowerShell no está instalado o no está habilitada para la comunicación remota en uno o más nodos del clúster de conmutación por error. No se puede configurar CAU para sí mismo\-modo de actualización o usar ciertas características de oficinas remotas\-modo de actualización.|Asegúrese de que PowerShell está instalado en todos los nodos del clúster y está habilitada para la comunicación remota.<br /><br />Para obtener más información, consulta [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) en este tema.|  
|Versión del clúster de conmutación por error|No ejecutan uno o varios nodos del clúster de conmutación por error de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. CAU no puede actualizar el clúster de conmutación por error.|Compruebe que el clúster de conmutación por error que se especifica durante la ejecución del BPA se está ejecutando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.<br /><br />Para obtener más información, consulte [Comprobar la configuración del clúster](#BKMK_REQ_CLUS) en este tema.|  
|Las versiones de .NET Framework y Windows PowerShell requeridas deben estar instaladas en todos los nodos de clúster de conmutación por error|.NET framework 4.6, 4.5 o Windows PowerShell no está instalado en uno o más nodos del clúster. Es posible que algunas características de CAU no funcionen.|Asegúrese de que .NET Framework 4.6 o 4.5 y Windows PowerShell están instalados en todos los nodos del clúster, si son necesarios.<br /><br />Para obtener más información, consulta [Configurar los nodos para la administración remota](#BKMK_NODE_CONFIG) en este tema.|  
|El servicio de clúster debe ejecutarse en todos los nodos de clúster|El servicio de clúster no se ejecuta en uno o más nodos. CAU no puede actualizar el clúster de conmutación por error.|-Asegúrese de que el servicio de clúster \(clussvc\) se inicia en todos los nodos del clúster, y está configurado para iniciarse automáticamente.<br />-Compruebe que el validar un asistente de configuración puede ejecutar correctamente en el clúster de conmutación por error.<br /><br />Para obtener más información, consulte [Comprobar la configuración del clúster](#BKMK_REQ_CLUS) en este tema.|  
|Las actualizaciones automáticas no deben estar configuradas para la instalación automática de actualizaciones en ningún nodo de clúster de conmutación por error|En un nodo del clúster del conmutación por error como mínimo, Actualizaciones automáticas se ha configurado para instalar automáticamente actualizaciones de Microsoft en ese nodo. La combinación de CAU con otros métodos de actualización puede tener como consecuencia tiempo de inactividad no planeado o resultados impredecibles.|Si la funcionalidad de Windows Update se ha configurado para Actualizaciones automáticas en uno o más nodos del clúster, asegúrate de que Actualizaciones automáticas no se haya configurado para instalar las actualizaciones automáticamente.<br /><br />Para obtener más información, consulta [Recomendaciones para aplicar actualizaciones de Microsoft](#BKMK_BP_WUA).|  
|Los nodos de clúster de conmutación por error deben usar el mismo origen de actualización|Uno o más nodos del clúster de conmutación por error están configurados para usar un origen de actualización para las actualizaciones de Microsoft distinto del resto de nodos. Es posible que CAU no aplique de manera uniforme las actualizaciones en los nodos del clúster.|Asegúrate de que cada nodo del clúster esté configurado para usar el mismo origen de actualización, por ejemplo, un servidor WSUS, Windows Update o Microsoft Update.<br /><br />Para obtener más información, consulta [Recomendaciones para aplicar actualizaciones de Microsoft](#BKMK_BP_WUA).|  
|Debe habilitarse una regla de firewall que permita el apagado remoto en cada nodo del clúster de conmutación por error|Uno o más nodos del clúster de conmutación por error no tienen una regla de firewall habilitada que permita el apagado remoto, o bien, una configuración de directiva de grupo impide habilitar esta regla. Es posible que una ejecución de actualización que aplique actualizaciones que requieran reiniciar los nodos automáticamente no se complete correctamente.|Si Firewall de Windows o un no\-firewall de Microsoft está en uso en los nodos del clúster, configure una regla de firewall que permita el apagado remoto.<br /><br />Para obtener más información, consulte [Habilitar una regla de firewall para permitir los reinicios automáticos](#BKMK_FW) en este tema.|  
|La configuración de servidor proxy de cada nodo de clúster de conmutación por error debe estar establecido en un servidor proxy local|Uno o más nodos del clúster de conmutación por error tienen una configuración incorrecta de servidor proxy.<br /><br />Si se usa un servidor proxy local, la configuración de servidor proxy en cada nodo debe estar definida correctamente para que el clúster pueda tener acceso a Microsoft Update o Windows Update.|Asegúrate de que la configuración de proxy WinHTTP en cada nodo del clúster esté establecida en un servidor proxy local si es necesario. Si no se usa un servidor proxy en el entorno, esta advertencia se puede ignorar.<br /><br />Para obtener más información, consulta [Aplicar actualizaciones de Microsoft en escenarios de sucursales](#BKMK_PROXY) en este tema.|  
|El rol en clúster de CAU debe instalarse en el clúster de conmutación por error para habilitar el autoservicio\-modo de actualización|El rol en clúster de CAU no está instalado en este clúster de conmutación por error. Es necesario para este rol de clúster self\-actualizando.|Para usar CAU en self\-actualizando modo, agregar el rol en clúster de CAU en el clúster de conmutación por error en una de las maneras siguientes:<br /><br />-Ejecute el [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) cmdlet de PowerShell.<br />-Seleccione la **configurar clúster self\-opciones de actualización** acción en la ventana de actualización de clústeres.|  
|El rol en clúster de CAU debe habilitarse en el clúster de conmutación por error para habilitar el autoservicio\-modo de actualización|El rol en clúster de CAU está deshabilitado. Por ejemplo, el rol en clúster de CAU no está instalado o se ha deshabilitado mediante el uso de la [deshabilitar\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) cmdlet de PowerShell. Es necesario para este rol de clúster self\-actualizando.|Para usar CAU en self\-actualizando modo, habilite el rol en clúster de CAU en este clúster de conmutación por error en una de las maneras siguientes:<br /><br />-Ejecute el [Enable-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) cmdlet de PowerShell.<br />-Seleccione la **configurar clúster self\-opciones de actualización** acción en la ventana de actualización de clústeres.|  
|El enchufe CAU configurado\-en para sí mismo\-modo de actualización debe estar registrado en todos los nodos de clúster de conmutación por error|El rol en clúster de CAU en uno o más nodos de este clúster de conmutación por error no puede tener acceso el enchufe CAU\-en el módulo que está configurado en el autoservicio\-opciones de actualización. Un autoservicio\-podría producir un error al ejecutar la actualización.|-Asegúrese de que el enchufe CAU configurado\-en se instala en todos los nodos del clúster siguiendo el procedimiento de instalación para el producto que proporciona el enchufe CAU\-en.<br />-Ejecute el [registrar\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) cmdlet de PowerShell para registrar el enchufe\-en los nodos de clúster necesarios.|  
|Todos los nodos de clúster de conmutación por error deben tener el mismo conjunto de complemento CAU registrado\-ins|Un autoservicio\-ejecutar la actualización puede producir un error si el enchufe\-en el que está configurado para usarse en una ejecución de actualización cambia a uno que no está disponible en todos los nodos del clúster.|-Asegúrese de que el enchufe CAU configurado\-en se instala en todos los nodos del clúster siguiendo el procedimiento de instalación para el producto que proporciona el enchufe CAU\-en.<br />-Ejecute el [registrar\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) cmdlet de PowerShell para registrar el enchufe\-en los nodos de clúster necesarios.|  
|Las opciones de ejecución de actualización configuradas deben ser válidas|El autoservicio\-actualización de programación y ejecución de actualización de las opciones que están configuradas para este clúster de conmutación por error están incompletas o no son válidas. Un autoservicio\-podría producir un error al ejecutar la actualización.|Configurar un válido\-actualizando la programación y el conjunto de opciones de ejecución de actualización. Por ejemplo, puede usar el [establecer\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) rol en clúster el cmdlet de PowerShell para configurar la CAU.|  
|Al menos dos nodos de clúster de conmutación por error deben ser propietarios del rol en clúster de CAU|Una ejecución de actualización iniciada en el propio\-modo de actualización se producirá un error porque el rol en clúster de CAU no tiene un nodo propietario posible para mover a.|Usa Herramientas de clústeres de conmutación por error para asegurarte de que todos los nodos del clúster estén configurados como posibles propietarios del rol en clúster de CAU. Es la configuración predeterminada.||  
|Todos los nodos de clúster de conmutación por error deben poder obtener acceso a scripts de Windows PowerShell|No todos los posibles nodos propietarios del rol en clúster CAU pueden tener acceso a la anterior a Windows PowerShell configurado\-actualizar y registrar\-actualizar las secuencias de comandos. Un autoservicio\-se producirá un error al ejecutar la actualización.|Asegúrese de que todos los posibles nodos propietarios del rol en clúster de CAU tengan permisos para acceder a la anterior a PowerShell configurado\-actualizar y registrar\-actualizar las secuencias de comandos.|  
|Todos los nodos de clúster de conmutación por error deben usar scripts de Windows PowerShell idénticos|No todos los posibles nodos propietarios del rol en clúster de CAU usan la misma copia de la anterior a Windows PowerShell especificado\-actualizar y registrar\-actualizar las secuencias de comandos. Un autoservicio\-actualizando ejecutar puedan generar errores o mostrar un comportamiento inesperado.|Asegúrese de que todos los posibles nodos propietarios del rol en clúster de CAU usan la misma pre PowerShell\-actualizar y registrar\-actualizar las secuencias de comandos.|  
|El valor de WarnAfter debe ser menor que el valor de StopAfter para la ejecución de actualización|Los valores de tiempo de espera de la ejecución de actualización de CAU especificada dejan sin efecto al tiempo de espera de advertencia. Es posible que una ejecución de actualización se cancele antes de que se pueda generar un registro de eventos de advertencia.|En las opciones de la ejecución de advertencia, configura un valor de la opción **WarnAfter** que sea menor que el valor de la opción **StopAfter**.|  
  
## <a name="see-also"></a>Vea también  
  
-   [Introducción a la actualización compatible con clústeres](cluster-aware-updating.md)