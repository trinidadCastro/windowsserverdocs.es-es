---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: "Actualizar clústeres requisitos y recomendaciones"
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 4/28/2017
description: "Requisitos para usar una actualización compatible con clústeres para instalar las actualizaciones en los clústeres que ejecutan Windows Server."
ms.openlocfilehash: 8517466d58345077af446c1b2c1e2aeb3b17aaa1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Actualizar clústeres requisitos y recomendaciones

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta sección describe los requisitos y dependencias que son necesarios para usar [actualizar clústeres](cluster-aware-updating.md) (PRU) para aplicar actualizaciones a un clúster de conmutación por error que ejecutan Windows Server.
  
> [!NOTE]  
> Es posible que debas independientemente validar que el entorno de clúster está listo para aplicar actualizaciones si usas un plug\ de distinto de **Microsoft.WindowsUpdatePlugin**. Si estás usando un Microsoft non\ en plug\, ponte en contacto con el Editor para obtener más información. Para obtener más información sobre los complementos de plug\, consulta [cómo funcionan los complementos Plug\](cluster-aware-updating-plug-ins.md).   
  
## <a name="BKMK_REQ_CLUS"></a>Instalar la característica clúster de conmutación por error y las herramientas de clústeres de conmutación por error  
PRU requiere una instalación de la característica clúster de conmutación por error y las herramientas de clústeres de conmutación por error. Las herramientas de clústeres de conmutación por error incluyen la \(clusterawareupdating.dll\) herramientas PRU, los cmdlets de clústeres de conmutación por error y otros componentes necesarios para las operaciones de PRU. Para que conocer los pasos instalar la característica clúster de conmutación por error, consulta [instalar las herramientas y la característica de clúster de conmutación por error](http://go.microsoft.com/fwlink/p/?LinkId=253342).  
  
Los requisitos de instalación exacto para las herramientas de clústeres de conmutación por error dependen de si PRU coordine las actualizaciones como un rol de clúster en el clúster de conmutación por error \ (mediante el uso de modo de conexión a actualizar self\ errores\) o desde un equipo remoto. Además, el modo de actualización self\ de PRU requiere la instalación de la función clúster PRU en el clúster de conmutación por error mediante las herramientas de PRU.    
  
La siguiente tabla resume los requisitos de instalación de la característica de PRU para los dos modos de actualización de PRU.  
  
|Componente instalado|Modo de actualización Self\|Modo de actualización Remote\|  
|-----------------------|-----------------------|-------------------------|  
|Característica clúster de conmutación por error|Necesario en todos los nodos del clúster|Necesario en todos los nodos del clúster|  
|Herramientas de clúster de conmutación por error|Necesario en todos los nodos del clúster|-Necesario actualizar remote\ equipo<br />-Necesarios en todos los nodos del clúster para ejecutar el [guardar CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet|  
|Rol clúster PRU|Obligatorio|No es necesario|  
  
## <a name="obtain-an-administrator-account"></a>Obtener una cuenta de administrador  
Los siguientes requisitos de administrador son necesarios para usar características de PRU.  
  
-   Para obtener una vista previa o aplicar acciones de actualización mediante el uso de la interfaz de usuario PRU \(UI\) o los cmdlets de actualización compatible con clústeres, debes usar una cuenta de dominio que tenga derechos de administrador local y permisos en todos los nodos del clúster. Si la cuenta no tiene suficientes privilegios en todos los nodos, se le pedirá en la ventana actualizar clústeres para proporcionar las credenciales necesarias al realizar estas acciones. Para usar la [actualizar clústeres](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating) cmdlets, como un parámetro de cmdlet puede proporcionar las credenciales necesarias.  
  
-   Si usas PRU en modo de actualización remote\ al iniciar sesión con una cuenta que no tiene derechos de administrador local y permisos en los nodos del clúster, debe ejecutar las herramientas de PRU como administrador con una cuenta de administrador local en el equipo del Coordinador de actualización, o mediante una cuenta que tenga la **suplantar a un cliente tras la autenticación** derecho de usuario. 
  
-   Para ejecutar el analizador de procedimientos recomendados de PRU, debes usar una cuenta con privilegios administrativos en los nodos del clúster y privilegios de administrador local en el equipo que se usa para ejecutar el [prueba CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet o para analizar actualizar readiness mediante la ventana de la actualización de clústeres de clúster. Para obtener más información, consulta [clúster prueba actualizar readiness](#BKMK_BPA).  
  
## <a name="verify-the-cluster-configuration"></a>Comprueba la configuración del clúster  
Las siguientes son los requisitos generales para un clúster de conmutación por error admitir actualizaciones mediante PRU. Requisitos de configuración adicionales para la administración remota en los nodos que se enumeran en [configurar los nodos de administración remota](#BKMK_NODE_CONFIG) más adelante en este tema.  
  
-   Para que este tiene quórum, deben estar conectados suficientes nodos del clúster.  
  
-   Todos los nodos del clúster deben estar en el mismo dominio de Active Directory.  
  
-   El nombre del clúster debe resolverse en la red mediante DNS.  
  
-   Si PRU se usa en modo de actualización remote\, el equipo del Coordinador de actualización debe tener conectividad de red a los nodos del clúster de conmutación por error, y debe estar en el mismo dominio de Active Directory que el clúster de conmutación por error.  
  
-   El servicio de clúster debe ejecutarse en todos los nodos del clúster. De manera predeterminada, este servicio se instala en todos los nodos del clúster y está configurado para iniciarse automáticamente.  
  
-   Para usar scripts de PowerShell pre\ actualización o post\ durante un PRU actualizar ejecutar, asegúrate de que los scripts están instalados en todos los nodos del clúster o que son accesibles para todos los nodos, por ejemplo, en un recurso compartido de red de alta disponibilidad. Si los scripts se guardan en un recurso compartido de red, configurar la carpeta permiso de lectura del todos grupo.  
  
## <a name="BKMK_NODE_CONFIG"></a>Configurar los nodos de administración remota  
Para usar una actualización compatible con clústeres, todos los nodos del clúster deben estar configurados para la administración remota. De manera predeterminada, la única tarea que debe realizar para configurar los nodos de administración remota a [habilitar una regla de firewall permitir que los reinicios automáticos](#BKMK_FW). 

La siguiente tabla enumera los requisitos de administración remota completo, en caso de que el entorno difiere de los valores predeterminados.

Estos requisitos son además de los requisitos de instalación para la [instalar la característica clúster de conmutación por error y las herramientas de clústeres de conmutación por error](#BKMK_REQ_CLUS) y general clústeres requisitos que se describen en las secciones anteriores en este tema.  
  
|Requisito|Estado predeterminado|Modo de actualización Self\|Modo de actualización Remote\|  
|---------------|---|-----------------------|-------------------------|  
|[Habilitar una regla de firewall permitir que los reinicios automáticos](#BKMK_FW)|Deshabilitado|Obligatorio en todos los nodos del clúster, si un firewall está en uso|Obligatorio en todos los nodos del clúster, si un firewall está en uso|  
|[Habilitar Instrumental de administración de Windows](#BKMK_WMI)|Habilitado|Necesario en todos los nodos del clúster|Necesario en todos los nodos del clúster|  
|[Habilitar Windows PowerShell 3.0 o 4.0 y Windows PowerShell remoto](#BKMK_PS)|Habilitado|Necesario en todos los nodos del clúster|Se necesita en todos los nodos del clúster para ejecutar las siguientes acciones:<br /><br />-La [guardar CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-Secuencias de comandos de PowerShell pre\ actualización y post\ actualización durante una actualización de ejecutar<br />-Pruebas de actualización mediante la ventana de la actualización de clústeres de preparación del clúster o la [Test\ CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) cmdlet de Windows PowerShell|  
|[Instalar .NET Framework 4.6 o 4.5](#BKMK_NET)|Habilitado|Necesario en todos los nodos del clúster|Se necesita en todos los nodos del clúster para ejecutar las siguientes acciones:<br /><br />-La [guardar CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-Secuencias de comandos de PowerShell pre\ actualización y post\ actualización durante una actualización de ejecutar<br />-Pruebas de actualización mediante la ventana de la actualización de clústeres de preparación del clúster o la [Test\ CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) cmdlet de Windows PowerShell|  

### <a name="BKMK_FW"></a>Habilitar una regla de firewall permitir que los reinicios automáticos  
Para permitir que los reinicios automáticos después de que las actualizaciones se aplican \ (si la instalación de una actualización requiere un restart\), si Firewall de Windows o un firewall non\ Microsoft está en uso en los nodos del clúster, una regla de firewall debe estar habilitada en todos los nodos que permite el tráfico siguiente:  
  
-   Protocolo: TCP  
  
-   Dirección: entrante  
  
-   Programa: wininit.exe  
  
-   Puertos: Los puertos RPC dinámica  
  
-   Perfil: dominio  
  
Si Firewall de Windows se usa en los nodos del clúster, puedes hacerlo activando el **apagado remoto** grupo de regla de Firewall de Windows en cada nodo del clúster. Cuando uses la ventana actualizar clústeres para aplicar actualizaciones y configurar las opciones de actualización self\, el **apagado remoto** grupo de reglas de Firewall de Windows está habilitada automáticamente en cada nodo del clúster.  
  
> [!NOTE]  
> La **apagado remoto** no se puede habilitar el grupo de reglas de Firewall de Windows cuando entra en conflicto con la configuración de directiva de grupo que está configurados para el Firewall de Windows.    
  
La **apagado remoto** grupo de reglas de firewall también se habilita al especificar el **: EnableFirewallRules** parámetro cuando se ejecuta los siguientes cmdlets PRU: [agregar CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole), [Invoke CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan), y [SetCauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole).  
  
El siguiente ejemplo de PowerShell, muestra un método adicional para permitir que los reinicios automáticos en un nodo del clúster.  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Habilitar Instrumental de administración de Windows (WMI) 
Todos los nodos del clúster deben estar configurados para la administración remota con \(WMI\) Instrumental de administración de Windows. Esto está habilitado de manera predeterminada.  
  
Para habilitar la administración remota manualmente, haz lo siguiente:  
  
1.  En la consola de servicios, iniciar la **administración remota de Windows** de servicio y establece el tipo de inicio en **automática**.  
  
2.  Ejecutar el [conjunto WSManQuickConfig](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.wsman.management/set-wsmanquickconfig) pedir cmdlet, o ejecuta el siguiente comando desde un comando con privilegios elevados:  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
Para admitir remotos WMI, si Firewall de Windows está en uso en los nodos del clúster, el firewall de entrada de reglas para **administración remota de Windows \(HTTP\-In\)** debe estar habilitado en cada nodo.  De manera predeterminada, esta regla está habilitada.  
  
### <a name="BKMK_PS"></a>Habilitar Windows PowerShell y Windows PowerShell remoto  
Para habilitar el modo de actualización self\ y ciertas funciones de PRU en modo de actualización remote\, PowerShell debe estar instalado y habilitado para ejecutar comandos remotos en todos los nodos del clúster. De manera predeterminada, PowerShell está instalado y habilitado para la comunicación remota.  
  
Para habilitar la comunicación remota de PowerShell, usa uno de los siguientes métodos:  
  
-   Ejecutar el [habilitar PSRemoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) cmdlet.  
  
-   Establecer una configuración de directiva de grupo de nivel de domain\ para \(WinRM\) administración remota de Windows.  
  
Para obtener más información acerca de cómo habilitar PowerShell remoto, vea [about\_Remote\_Requirements](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_remote_requirements).  
  
### <a name="BKMK_NET"></a>Instalar .NET Framework 4.6 o 4.5  
Para habilitar el modo de actualización self\ y ciertas funciones de PRU en modo de actualización remote\, .NET Framework 4.6 o .NET Framework 4.5 (en Windows Server 2012 R2) debe instalarse en todos los nodos del clúster. De manera predeterminada, se instala .NET Framework.  

Para instalar .NET Framework 4.6 (o 4.5) mediante PowerShell si todavía no está instalado, usa el siguiente comando:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Procedimientos recomendados para usar la actualización compatible con clústeres 
  
### <a name="BKMK_BP_WUA"></a>Recomendaciones para aplicar actualizaciones de Microsoft

Te recomendamos que cuando empieces a utilizar PRU para aplicar actualizaciones con el valor predeterminado **Microsoft.WindowsUpdatePlugin** plug\ en un clúster, dejar de usar otros métodos para instalar las actualizaciones de software de Microsoft en los nodos del clúster.  
  
> [!CAUTION]  
> Combinar PRU con los métodos que se actualizan automáticamente los nodos individuales \ (en un schedule\ de tiempo fijo) puede provocar resultados impredecibles, incluida interrupciones en el servicio y el tiempo de inactividad imprevisto.  
  
Te recomendamos que sigas estas directrices:  
  
-   Para obtener resultados óptimos, se recomienda deshabilitar a la configuración en los nodos del clúster para las actualizaciones automáticas, por ejemplo, a través de la configuración de las actualizaciones automáticas en el Panel de Control, o en la configuración que se configura con la directiva de grupo.  
  
    > [!CAUTION]  
    > Instalación automática de actualizaciones en los nodos del clúster puede interferir con la instalación de actualizaciones mediante PRU y puede producir errores de PRU.  
  
    Si son necesarios, la siguiente configuración de las actualizaciones automáticas es compatible con PRU, ya que el administrador puede controlar el tiempo de instalación de actualización:  
  
    -   Configuración para notificar antes de descargar las actualizaciones y notificar antes de la instalación  
  
    -   Configuración para descargar actualizaciones automáticamente y notificar antes de la instalación  
  
    Sin embargo, si las actualizaciones automáticas está descargando actualizaciones al mismo tiempo como un PRU actualizar ejecutar, ejecutar la actualización puede tardar más en completarse.  
  
-   No se configura un sistema de actualizaciones, como Windows Server Update Services \(WSUS\) para aplicar actualizaciones automáticamente \ (en un schedule\ de tiempo fijo) a los nodos del clúster.  
  
-   Todos los nodos del clúster deben estar configurados de manera uniforme para usar el mismo origen de la actualización, por ejemplo, un WSUS server, Windows Update o Microsoft Update.  
  
-   Si usas un sistema de administración de configuración para aplicar actualizaciones de software en equipos de la red, excluir todas las actualizaciones automáticas o necesarias nodos del clúster. Microsoft System Center Configuration Manager 2007 y Microsoft System Center Virtual Machine Manager 2008 son ejemplos de sistemas de administración de configuración.  
  
-   Si los servidores de distribución de software internos \ (por ejemplo, WSUS servidores\) se usan para contener e implementar las actualizaciones, asegúrate de que esos servidores identifican correctamente las actualizaciones aprobadas para los nodos del clúster.  
  
#### <a name="BKMK_PROXY"></a>Aplicar las actualizaciones de Microsoft en sucursales  
Para descargar actualizaciones de Microsoft desde Microsoft Update o Windows Update en los nodos del clúster en determinados escenarios, debes configurar las opciones de proxy para la cuenta de sistema Local en cada nodo. Por ejemplo, es posible que debas hacerlo si los clústeres de office rama acceder a Microsoft Update o Windows Update para descargar las actualizaciones mediante un servidor proxy local.  
  
Si es necesario, configurar la configuración del proxy WinHTTP en cada nodo para especificar un servidor proxy local y configurar excepciones direcciones locales \ (es decir, una lista de omisión de addresses\ local). Para ello, puede ejecutar el comando siguiente en cada nodo del clúster desde un símbolo del sistema con privilegios elevados:  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
donde <*ProxyServerFQDN*> es el nombre de dominio completo del servidor proxy y <*puerto*> es el puerto en que se comunican (normalmente el puerto 443).  
  
Por ejemplo, para establecer la configuración de proxy de WinHTTP de la cuenta de sistema Local, especificar el servidor proxy *MyProxy.CONTOSO.com*, con el puerto 443 y excepciones de dirección local, escribe el siguiente comando:  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>Recomendaciones para utilizar la Microsoft.HotfixPlugin  
  
-   Te recomendamos que configures los permisos en la carpeta raíz de revisión y el archivo de configuración de revisión para restringir el acceso de escritura a los administradores locales en los equipos que se usan para almacenar estos archivos. Esto ayuda a prevenir la manipulación de estos archivos, los usuarios no autorizados que pueda poner en riesgo la funcionalidad del clúster de conmutación por error cuando se aplican a revisiones.  
  
-   Para ayudar a garantizar la integridad de los datos para las conexiones de \(SMB\) del bloque de mensajes de servidor que se usan para acceder a la carpeta raíz de revisión, debes configurar SMB cifrado en la carpeta compartida de SMB, si es posible que a configurarlo. La **Microsoft.HotfixPlugin** SMB cifrado o firma SMB debe configurarse para ayudar a garantizar la integridad de los datos para las conexiones de SMB. 
  
    Para obtener más información, consulta [restringir el acceso a la carpeta raíz de revisión y el archivo de configuración de revisión](cluster-aware-updating-plug-ins.md#BKMK_ACL).
  
### <a name="additional-recommendations"></a>Recomendaciones adicionales  
  
-   Para evitar interferencias con un PRU actualizar ejecutar que se pueden programar al mismo tiempo, no programar cambios de contraseña para objetos de nombre de clúster y equipo virtual durante las ventanas de mantenimiento programado.  
  
-   Debes establecer los permisos adecuados en los scripts pre\-update y update de post\ que están guardados en carpetas compartidas de red para evitar posibles alteraciones con estos archivos de usuarios no autorizados.  
  
-   Para configurar PRU en modo de actualización self\, un objeto de equipo virtual \(VCO\) para el rol clúster PRU debe crearse en Active Directory. PRU puede crear este objeto automáticamente en el momento en que se agrega el rol clúster PRU, si el clúster de conmutación por error tiene los permisos necesarios. Sin embargo, debido a las directivas de seguridad en determinadas organizaciones, puede ser necesario preconfigurar el objeto de Active Directory. Un procedimiento hacerlo, consulta [pasos para preconfigurar una cuenta para una función de clúster](http://go.microsoft.com/fwlink/p/?LinkId=237624).  
  
-   Para guardar y reutilizar configuración actualización ejecutar en los clústeres de conmutación por error con similar actualizar las necesidades de la organización de TI, puede crear perfiles de ejecutar la actualización. Además, en función del modo de actualización, puedes guardar y administrar los perfiles de ejecutar la actualización en un recurso compartido de archivos que sea accesible para todos los equipos remotos de coordinador de actualización o clústeres de conmutación por error. Para obtener más información, consulta [opciones avanzadas y actualización de perfiles de ejecución para PRU](cluster-aware-updating-options.md).  
  
## <a name="BKMK_BPA"></a>Preparación para la actualización de clúster de prueba  
Puedes ejecutar el modelo \(BPA\) PRU analizador de procedimientos recomendados para comprobar si un clúster de conmutación por error y el entorno de red con muchos de los requisitos que las actualizaciones de software aplicadas por PRU. Muchas de las pruebas de comprobar el entorno esté lista Aplicar actualizaciones de Microsoft mediante el valor predeterminado, plug\ en **Microsoft.WindowsUpdatePlugin**.  
  
> [!NOTE]  
> Es posible que debas independientemente validar que el entorno de clúster está listo para aplicar actualizaciones de software mediante un plug\ de distinto de **Microsoft.WindowsUpdatePlugin**. Si estás usando un Microsoft non\ plug\ de, como el proporcionado por el fabricante del hardware, ponte en contacto con el Editor para obtener más información.  
  
Puedes ejecutar el BPA en las siguientes dos maneras:  
  
1.  Selecciona **clúster analizar actualizar readiness** en la consola de PRU. Tras la BPA completa las pruebas de preparación, aparecerá un informe de pruebas. Si se detectan problemas en los nodos del clúster, los problemas específicos y los nodos de dónde aparecen los problemas se identifican para que pueda tomar las medidas correctivas. Las pruebas pueden tardar varios minutos en completarse.  
  
2.  Ejecutar el [prueba CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet. Puedes ejecutar el cmdlet en un equipo local o remoto en el que se instala la conmutación por error clústeres módulo para Windows PowerShell (parte de las herramientas de clústeres de conmutación por error). También puedes ejecutar el cmdlet en un nodo del clúster de conmutación por error.  
  
> [!NOTE]  
> -   Debes usar una cuenta con privilegios administrativos en los nodos del clúster y privilegios de administrador local en el equipo que se usa para ejecutar el **Test\ CauSetup** cmdlet o para analizar actualizar readiness mediante la ventana de la actualización de clústeres de clúster. Para ejecutar las pruebas en la ventana actualizar clústeres, debe iniciar sesión en el equipo con las credenciales necesarias.  
> -   Las pruebas se suponen que PRU ejecutan las herramientas que sirven para obtener una vista previa y aplicar las actualizaciones de software desde el mismo equipo y con las mismas credenciales de usuario que se utilizan para probar la preparación para la actualización del clúster.  
  
> [!IMPORTANT]  
> Recomendamos que pruebes el clúster para actualizar la preparación de las siguientes situaciones:  
>   
> -   Antes de usar para aplicar actualizaciones de software PRU por primera vez.  
> -   Después de agregar un nodo al clúster o realizar otros cambios de hardware en el clúster que necesite la ejecución del validar un asistente de clúster.  
> -   Después de cambiar un origen de actualización o cambiar la configuración de actualización o configuraciones \(other than CAU\) que pueden afectar a la aplicación de actualizaciones en los nodos.  
  
### <a name="tests-for-cluster-updating-readiness"></a>Pruebas para actualizar la preparación de clúster  
La siguiente tabla enumera el clúster actualizando las pruebas de preparación, algunos problemas comunes y los pasos de resolución.  
  
|Prueba|Los impactos y posibles problemas|Pasos de resolución|  
|--------|-------------------------------|--------------------|  
|El clúster de conmutación por error debe estar disponible|No se puede resolver que el nombre de clúster de conmutación por error, o uno o varios nodos del clúster no se puede acceder. El BPA no puede ejecutar las pruebas de preparación del clúster.|-La revisión ortográfica del nombre del clúster especificado durante la ejecución de BPA.<br />-Asegúrese de que todos los nodos del clúster están en línea y en ejecución.<br />-Compruebe que el validar un asistente de configuración puede ejecutar correctamente en el clúster de conmutación por error.|  
|Los nodos del clúster de conmutación por error deben estar habilitados para la administración remota a través de WMI|Uno o varios nodos del clúster de conmutación por error no están habilitados para la administración remota mediante el uso de \(WMI\) Instrumental de administración de Windows. PRU no puede actualizar los nodos del clúster si los nodos no están configurados para la administración remota.|Asegúrate de que todos los nodos del clúster de conmutación por error están habilitados para la administración remota a través de WMI. Para obtener más información, consulta [configurar los nodos de administración remota](#BKMK_NODE_CONFIG) en este tema.|  
|PowerShell remoto debe estar habilitado en cada nodo del clúster de conmutación por error|PowerShell no está instalado o no está habilitado para el control remoto en uno o varios nodos del clúster de conmutación por error. PRU no se puede configurar para el modo de actualización self\ o utilizar determinadas características en modo de actualización remote\.|Asegúrate de que PowerShell se instala en todos los nodos del clúster y está habilitada para la comunicación remota.<br /><br />Para obtener más información, consulta [configurar los nodos de administración remota](#BKMK_NODE_CONFIG) en este tema.|  
|Versión de clúster de conmutación por error|Uno o varios nodos del clúster de conmutación por error no ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. PRU no puede actualizar el clúster de conmutación por error.|Comprueba que el clúster de conmutación por error que se especifica durante la ejecución de BPA ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.<br /><br />Para obtener más información, consulta [comprueba la configuración del clúster](#BKMK_REQ_CLUS) en este tema.|  
|Las versiones necesarias de .NET Framework y Windows PowerShell deben estar instaladas en todos los nodos del clúster de conmutación por error|.NET framework 4.6, 4.5 o Windows PowerShell no está instalado en uno o varios nodos del clúster. Algunas características PRU podrían no funcionar.|Asegúrate de que .NET Framework 4.6 o 4.5 y Windows PowerShell están instalados en todos los nodos del clúster, si son necesarios.<br /><br />Para obtener más información, consulta [configurar los nodos de administración remota](#BKMK_NODE_CONFIG) en este tema.|  
|El servicio de clúster debe estar ejecutándose en todos los nodos del clúster|No se está ejecutando el servicio de clúster en uno o varios nodos. PRU no puede actualizar el clúster de conmutación por error.|-Asegúrese de que se inicia el \(clussvc\) servicio clúster en todos los nodos del clúster, y que está configurado para iniciarse automáticamente.<br />-Compruebe que el validar un asistente de configuración puede ejecutar correctamente en el clúster de conmutación por error.<br /><br />Para obtener más información, consulta [comprueba la configuración del clúster](#BKMK_REQ_CLUS) en este tema.|  
|No se deben configurar las actualizaciones automáticas para instalar automáticamente las actualizaciones en cualquier nodo del clúster de conmutación por error|En al menos un nodo del clúster de conmutación por error, las actualizaciones automáticas está configurada para instalar automáticamente las actualizaciones de Microsoft de ese nodo. Combinar PRU con otros métodos de actualización puede producirse en tiempo de inactividad imprevisto o resultados impredecibles.|Si la funcionalidad de Windows Update está configurada para las actualizaciones automáticas en uno o varios nodos del clúster, asegúrate de que las actualizaciones automáticas no está configurada para instalar las actualizaciones automáticamente.<br /><br />Para obtener más información, consulta [recomendaciones para aplicar actualizaciones de Microsoft](#BKMK_BP_WUA).|  
|Los nodos del clúster de conmutación por error deben usar el mismo origen de actualización|Uno o varios nodos del clúster de conmutación por error están configurados para usar un origen de actualización para las actualizaciones de Microsoft que sea diferente del resto de los nodos. Actualizaciones es posible que no se aplican uniformemente en los nodos del clúster por PRU.|Asegúrate de que cada nodo del clúster está configurada para usar el mismo origen de la actualización, por ejemplo, un servidor WSUS, Windows Update o Microsoft Update.<br /><br />Para obtener más información, consulta [recomendaciones para aplicar actualizaciones de Microsoft](#BKMK_BP_WUA).|  
|Una regla de firewall que permita apagado remoto debe estar habilitada en todos los nodos del clúster de conmutación por error|Uno o varios nodos del clúster de conmutación por error no tienes una regla de firewall habilitada que permita apagado remoto o una configuración de directiva de grupo impide que esta regla esté habilitada. Una actualización ejecutar que se aplica a las actualizaciones que requieren reiniciar los nodos automáticamente no puede completar correctamente.|Si Firewall de Windows o un firewall non\ Microsoft está en uso en los nodos del clúster, configurar una regla de firewall que permita apagado remoto.<br /><br />Para obtener más información, consulta [habilitar una regla de firewall permitir que los reinicios automáticos](#BKMK_FW) en este tema.|  
|La configuración del servidor proxy en cada nodo del clúster de conmutación por error debe establecerse en un servidor proxy local|Uno o varios nodos del clúster de conmutación por error tienen una configuración del servidor proxy incorrecto.<br /><br />Si un servidor proxy local está en uso, la configuración del servidor proxy en cada nodo debe configurarse correctamente para que el clúster tener acceso a Microsoft Update o Windows Update.|Asegúrate de que la configuración de proxy WinHTTP en cada nodo del clúster se establece en un servidor proxy local si es necesario. Si un servidor proxy no está en uso en el entorno, puede omitirse esta advertencia.<br /><br />Para obtener más información, consulta [aplicar actualizaciones en sucursales](#BKMK_PROXY) en este tema.|  
|El rol clúster PRU debe instalarse en el clúster de conmutación por error para habilitar el modo de actualización self\|No está instalado el rol clúster PRU en este clúster de conmutación por error. Esta función es necesaria para actualizar self\ de clúster.|Para usar PRU en modo de actualización self\, agrega el rol clúster PRU en el clúster de conmutación por error en una de las siguientes maneras:<br /><br />-Ejecuta la [agregar CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet de PowerShell.<br />: Selecciona la **configurar las opciones de actualización self\ de clúster** acción en la ventana de clústeres de actualización.|  
|El rol clúster PRU debe estar habilitado en el clúster de conmutación por error para habilitar el modo de actualización self\|El rol clúster PRU está deshabilitado. Por ejemplo, no está instalado el rol clúster PRU o se ha deshabilitado utilizando la [Disable\ CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/disable-cauclusterrole) cmdlet de PowerShell. Esta función es necesaria para actualizar self\ de clúster.|Para usar PRU en modo de actualización self\, habilitar la función clúster PRU en este clúster de conmutación por error en una de las siguientes maneras:<br /><br />-Ejecuta la [habilitar CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/enable-cauclusterrole) cmdlet de PowerShell.<br />: Selecciona la **configurar las opciones de actualización self\ de clúster** acción en la ventana de clústeres de actualización.|  
|El PRU configurado plug\ en modo de actualización self\ debe registrarse en todos los nodos del clúster de conmutación por error|El rol clúster PRU en uno o varios nodos de este clúster de conmutación por error no tiene acceso el módulo de plug\ de PRU que esté configurado de las opciones de actualización self\. Un self\ actualizar ejecutar puede producir un error.|-Asegúrese de que el PRU configurado en plug\ está instalado en todos los nodos del clúster mediante el procedimiento de instalación para el producto que proporciona el PRU plug\ en.<br />-Ejecuta la [Register\ CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet de PowerShell para registrar el plug\ de los nodos del clúster necesarios.|  
|Todos los nodos del clúster de conmutación por error deben tener el mismo conjunto de registrados PRU plug\-ins|Un self\ actualizar ejecutar puede producir un error si el plug\ de que está configurado para usarse en una actualización de ejecución se cambia a una versión que no está disponible en todos los nodos del clúster.|-Asegúrese de que el PRU configurado en plug\ está instalado en todos los nodos del clúster mediante el procedimiento de instalación para el producto que proporciona el PRU plug\ en.<br />-Ejecuta la [Register\ CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet de PowerShell para registrar el plug\ de los nodos del clúster necesarios.|  
|Las opciones configuradas actualizar ejecutar deben ser válidas|El plan de actualización self\ y opciones de actualización ejecutar que están configurados para esta clúster de conmutación por error son incompletas o no válidos. Un self\ actualizar ejecutar puede producir un error.|Configura una programación actualizar self\ válida y un conjunto de opciones de actualización de ejecución. Por ejemplo, puedes usar la [Set CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole) cmdlet de PowerShell para configurar la PRU agrupado rol.|  
|Al menos dos nodos del clúster de conmutación por error deben ser los propietarios de la función clúster PRU|Una actualización ejecutar iniciarse en modo de actualización self\ se producirá un error porque el rol clúster PRU no tiene un nodo de propietario posible mover a.|Usa las herramientas de clústeres de conmutación por error para asegurarse de que todos los nodos del clúster están configurados como posibles propietarios de la PRU habían agrupado rol. Esta es la configuración predeterminada.||  
|Todos los nodos del clúster de conmutación por error deben ser capaz de acceder a los scripts de Windows PowerShell|No todos los nodos de posible propietario de la función clúster PRU pueden acceder a los scripts de actualización de pre\ y post\ actualización de Windows PowerShell configurados. Un self\ actualizar ejecutar se producirá un error.|Asegúrate de que todos los nodos de posible propietario de la función clúster PRU tienen permisos para acceder a los scripts de pre\-update y update de post\ PowerShell configurados.|  
|Todos los nodos del clúster de conmutación por error deben usar scripts de Windows PowerShell idénticos|No todos los nodos de posible propietario de la función clúster PRU usan la misma copia de los scripts de actualización de pre\ y post\ actualización de Windows PowerShell especificados. Una ejecución actualizar self\ podría producirá un error o mostrar un comportamiento inesperado.|Asegúrate de que todos los nodos de posible propietario de la función clúster PRU usen los mismos scripts de PowerShell pre\-update y update de post\.|  
|La configuración de WarnAfter especificada para actualización de ejecución debe ser menor que el valor de StopAfter|Los valores de tiempo de espera especificados actualizar ejecutar PRU hacen que el tiempo de espera de advertencia ineficaz. Es posible que se cancele una actualización de ejecutar antes de poder generar un registro de eventos de advertencia.|En las opciones de actualización ejecuta, configurar un **WarnAfter** opción valor que sea menor que el **StopAfter** valor de opción.|  
  
## <a name="see-also"></a>Consulta también  
  
-   [Introducción a actualizar clústeres](cluster-aware-updating.md)
  
