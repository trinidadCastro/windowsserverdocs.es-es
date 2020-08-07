---
title: Problemas conocidos del centro de administración de Windows
description: Problemas conocidos del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: bb416a45e18ea34628994b589e452f25d2d7744e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937674"
---
# <a name="windows-admin-center-known-issues"></a>Problemas conocidos del centro de administración de Windows

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Si encuentra un problema que no se describe en esta página, [háganoslo saber](https://aka.ms/WACfeedback).

## <a name="installer"></a>Instalador

- Al instalar el centro de administración de Windows con su propio certificado, tenga en cuenta que si copia la huella digital desde la herramienta MMC del administrador de certificados, contendrá [un carácter no válido al principio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Como solución alternativa, escriba el primer carácter de la huella digital y copie y pegue el resto.

- No se admite el uso de un puerto inferior a 1024. En el modo de servicio, puede configurar opcionalmente el puerto 80 para redirigir al puerto especificado.

## <a name="general"></a>General

- En la versión 1910,2 del centro de administración de Windows, es posible que no pueda conectarse a los servidores de Hyper-V en hardware específico. Si está bloqueado en este problema, [Descargue la compilación anterior](https://aka.ms/wacprevious).

- Si tiene el centro de administración de Windows instalado como puerta de enlace en **Windows Server 2016** bajo un uso intensivo, el servicio puede bloquearse con un error en el registro de eventos que contiene ```Faulting application name: sme.exe``` y ```Faulting module name: WsmSvc.dll``` . Esto se debe a un error que se ha corregido en Windows Server 2019. La revisión para Windows Server 2016 se incluyó la actualización acumulativa de febrero de 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si el centro de administración de Windows está instalado como una puerta de enlace y la lista de conexiones parece estar dañada, siga estos pasos:

   > [!WARNING]
   >Esto eliminará la lista de conexiones y la configuración de todos los usuarios del centro de administración de Windows en la puerta de enlace.

  1. Desinstalar el centro de administración de Windows
  2. Elimine la carpeta **experiencia de administración del servidor** en **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstalar el centro de administración de Windows

- Si deja abierta la herramienta y está inactiva durante un largo período de tiempo, puede obtener varios **errores: el estado del espacio de ejecución no es válido para estos errores de operación** . Si esto ocurre, actualice el explorador. Si encuentra esto, [envíenos sus comentarios](https://aka.ms/WACfeedback).

- Puede haber una desviación mínima entre el número de versión de los sistemas operativos que se ejecutan en los módulos del centro de administración de Windows y lo que se incluye en el aviso de software de terceros.

### <a name="extension-manager"></a>Administrador de extensiones

- Al actualizar el centro de administración de Windows, debe volver a instalar las extensiones.
- Si agrega una fuente de extensión a la que no se puede acceder, no hay ninguna advertencia. [14412861]

## <a name="browser-specific-issues"></a>Problemas específicos del explorador

### <a name="microsoft-edge"></a>Microsoft Edge

- Si tiene el centro de administración de Windows implementado como un servicio y usa Microsoft Edge como su explorador, es posible que se produzca un error al conectar la puerta de enlace a Azure después de generar una nueva ventana del explorador. Intente solucionar este problema agregando https://login.microsoftonline.com , https://login.live.com y la dirección URL de la puerta de enlace como sitios de confianza y sitios permitidos para la configuración del bloqueador de elementos emergentes en el explorador del lado cliente. Para obtener más información sobre cómo solucionar este [problema](troubleshooting.md#azure-features-dont-work-properly-in-edge)en la guía de solución de problemas. [17990376]

### <a name="google-chrome"></a>Google Chrome

- Antes de la versión 70 (publicada a finales de octubre de 2018), Chrome tenía un [error](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) relacionado con el protocolo WebSockets y la autenticación NTLM. Esto afecta a las siguientes herramientas: eventos, PowerShell Escritorio remoto.

- Chrome puede mostrar varias solicitudes de credenciales, especialmente durante la adición de una experiencia de conexión en un entorno de **grupo de trabajo** (no de dominio).

- Si el centro de administración de Windows se implementa como un servicio, es necesario habilitar los elementos emergentes de la dirección URL de la puerta de enlace para que funcione la funcionalidad de integración de Azure.

### <a name="mozilla-firefox"></a>Mozilla Firefox

El centro de administración de Windows no se ha probado con Mozilla Firefox, pero la mayoría de las funcionalidades deberían funcionar.

- Instalación de Windows 10: Mozilla Firefox tiene su propio almacén de certificados, por lo que debe importar el ```Windows Admin Center Client``` certificado en Firefox para usar el centro de administración de Windows en Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilidad de WebSocket al usar un servicio de proxy

Los módulos de Escritorio remoto, PowerShell y eventos del centro de administración de Windows utilizan el protocolo WebSocket, que a menudo no se admite cuando se usa un servicio de proxy.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Compatibilidad con versiones de Windows Server anteriores a 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> El centro de administración de Windows requiere características de PowerShell que no están incluidas en Windows Server 2012 R2, 2012 o 2008 R2. Si va a administrar Windows Server con el centro de administración de Windows, tendrá que instalar la versión 5,1 o superior de WMF en esos servidores.

Escribe `$PSVersiontable` en PowerShell para verificar que esté instalado WMF y que la versión sea 5.1 o posterior.

Si no está instalado, puedes [descargar e instalar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Control de acceso basado en roles (RBAC)

- La implementación de RBAC no se realizará correctamente en los equipos configurados para usar el control de aplicaciones de Windows Defender (WDAC, anteriormente conocido como integridad de código). [16568455]

- Para usar RBAC en un clúster, debe implementar la configuración en cada nodo miembro individualmente.

- Cuando se implementa RBAC, pueden producirse errores no autorizados que se atribuyen incorrectamente a la configuración de RBAC. [16369238]

## <a name="server-manager-solution"></a>Administrador del servidor solución

### <a name="certificates"></a>Certificados

- No se puede importar. Certificado cifrado PFX en el almacén del usuario actual. [11818622]

### <a name="events"></a>Events

- Los eventos se aplican a [la compatibilidad de WebSocket cuando se usa un servicio de proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Es posible que reciba un error que haga referencia a "tamaño de paquete" al exportar archivos de registro de gran tamaño.

  - Para resolver este error, use el siguiente comando en un símbolo del sistema con privilegios elevados en el equipo de puerta de enlace:```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Archivos

- Cargar o descargar archivos grandes que todavía no se admiten. ( \~ límite de 100 MB) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell se ve afectado por [la compatibilidad de WebSocket cuando se usa un servicio de proxy](#websocket-compatibility-when-using-a-proxy-service)

- El pegado con un solo clic con el botón secundario en la consola de PowerShell de escritorio no funciona. En su lugar, obtendrá el menú contextual del explorador, donde puede seleccionar pegar. Ctrl-V también funciona.

- Ctrl + C para copiar no funciona, siempre enviará el comando de interrupción Ctrl + C a la consola. Copiar en el menú contextual de clic con el botón secundario funciona.

- Al reducir el tamaño de la ventana del centro de administración de Windows, el contenido del terminal se redistribuirá, pero cuando vuelva a ser más grande, es posible que el contenido no vuelva a su estado anterior. Si las cosas se mezclan, puede probar Clear-host o desconectarse y volver a conectarse con el botón situado encima del terminal.

### <a name="registry-editor"></a>Editor del Registro

- Funcionalidad de búsqueda no implementada. [13820009]

### <a name="remote-desktop"></a>Escritorio remoto

- Cuando el centro de administración de Windows se implementa como un servicio, la herramienta de Escritorio remoto puede no cargarse después de actualizar el servicio del centro de administración de Windows a una nueva versión. Para solucionar este problema, borre la memoria caché del explorador.   [23824194]

- La herramienta de Escritorio remoto puede no conectarse al administrar Windows Server 2012. [20258278]

- Al usar el Escritorio remoto para conectarse a un equipo que no está unido a un dominio, debe especificar su cuenta en el ```MACHINENAME\USERNAME``` formato.

- Algunas configuraciones pueden bloquear el cliente de escritorio remoto del centro de administración de Windows con la Directiva de grupo. Si encuentra esto, habilítelo ```Allow users to connect remotely by using Remote Desktop Services``` en```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Escritorio remoto se ve afectado por la [compatibilidad de WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- Actualmente, la herramienta Escritorio remoto no admite la copia y pegado de texto, imagen o archivo entre el escritorio local y la sesión remota.

- Para realizar cualquier copia y pegado dentro de la sesión remota, puede copiar como normal (clic con el botón derecho + copiar o Ctrl + C), pero pegar requiere clic con el botón derecho + pegar (Ctrl + V no funciona)

- No se pueden enviar los siguientes comandos de clave a la sesión remota
  - Alt+Tabulador
  - Teclas de función
  - Tecla Windows
  - Impr Pant

### <a name="roles-and-features"></a>Roles y características

- Al seleccionar roles o características con orígenes no disponibles para la instalación, se omiten. [12946914]

- Si decide no reiniciar automáticamente después de la instalación del rol, no volveremos a preguntar. [13098852]

- Si elige reiniciar automáticamente, el reinicio se realizará antes de que el estado se actualice al 100%. [13098852]

### <a name="storage"></a>Storage

- Nivel inferior: las unidades de DVD/CD/disquete no aparecen como volúmenes en el nivel inferior.

- Nivel inferior: algunas propiedades de volúmenes y discos no están disponibles de nivel inferior, de modo que aparecen desconocidas o en blanco en el panel de detalles.

- Nivel inferior: cuando se crea un nuevo volumen, ReFS solo admite un tamaño de unidad de asignación de 64 k en máquinas con Windows 2012 y 2012 R2. Si un volumen ReFS se crea con un tamaño de unidad de asignación menor en los destinos de nivel inferior, se producirá un error en el formato del sistema de archivos. No se podrá usar el nuevo volumen. La solución consiste en eliminar el volumen y usar el tamaño de la unidad de asignación de 64 k.

### <a name="updates"></a>Actualizaciones

- Después de instalar las actualizaciones, el estado de la instalación puede almacenarse en caché y requerir una actualización del explorador.

- Es posible que se produzca el error: "el conjunto de claves no existe" al intentar configurar la administración de actualizaciones de Azure. En este caso, pruebe los siguientes pasos de corrección en el nodo administrado:
    1. Detenga el servicio "servicios criptográficos".
    2. Cambie las opciones de carpeta para mostrar los archivos ocultos (si es necesario).
    3. Vaya a la carpeta "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" y elimine todo su contenido.
    4. Reinicie el servicio "servicios criptográficos".
    5. Repetir la configuración de Update Management con el centro de administración de Windows

### <a name="virtual-machines"></a>Virtual Machines

- Al administrar las máquinas virtuales en un host de Windows Server 2012, la herramienta de conexión de máquina virtual en el explorador no podrá conectarse a la máquina virtual. La descarga del archivo. RDP para conectarse a la máquina virtual debería seguir funcionando. [20258278]

- Azure Site Recovery: si ASR está configurado en el host fuera de WAC, no podrá proteger una máquina virtual desde WAC [18972276]

- Actualmente no se admiten las características avanzadas disponibles en el administrador de Hyper-V como el administrador de SAN virtual, la replicación de la máquina virtual, la exportación de máquinas virtuales.

### <a name="virtual-switches"></a>Conmutadores virtuales

- Cambiar la formación de equipos incrustada (SET): al agregar NIC a un equipo, deben estar en la misma subred.

## <a name="computer-management-solution"></a>Solución de administración de equipos

La solución Administración de equipos contiene un subconjunto de las herramientas de la solución de Administrador del servidor, por lo que se aplican los mismos problemas conocidos, así como los siguientes problemas específicos de la solución de administración de equipos:

- Si usa una cuenta Microsoft ([MSA](https://account.microsoft.com/account/)) o si usa Azure Active Directory (AAD) para iniciar sesión en su equipo con Windows 10, debe usar "administrar como" para proporcionar las credenciales para una cuenta de administrador local [16568455]

- Al intentar administrar el host local, se le pedirá que eleve el proceso de puerta de enlace. Si hace clic en **no** en el menú emergente control de cuentas de usuario que aparece a continuación, debe cancelar el intento de conexión y empezar de nuevo.

- Windows 10 no tiene WinRM/PowerShell Remoting activada de forma predeterminada.

  - Para habilitar la administración del cliente de Windows 10, debe emitir el comando ```Enable-PSRemoting``` desde un símbolo del sistema de PowerShell con privilegios elevados.

  - También es posible que tenga que actualizar el firewall para permitir conexiones desde fuera de la subred local con ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any``` . Para escenarios de redes más restrictivas, consulte [esta documentación](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="cluster-deployment"></a>Implementación de clúster

### <a name="step-12"></a>Paso 1,2
Actualmente no se admiten equipos de grupos de trabajo mixtos al agregar servidores. Todas las máquinas que se usan para la agrupación en clústeres deben pertenecer al mismo grupo de trabajo. Si no es así, el botón siguiente se deshabilitará y aparecerá el siguiente error: "no se puede crear un clúster con servidores en diferentes dominios de Active Directory. Compruebe que los nombres de servidor son correctos. Mueva todos los servidores al mismo dominio e inténtelo de nuevo ".

### <a name="step-14"></a>Paso 1,4
Hyper-V debe instalarse en máquinas virtuales que ejecuten el sistema operativo HCI Azure Stack. Al intentar habilitar la característica Hyper-V para estas máquinas virtuales, se producirá el siguiente error:

![Captura de pantalla del error de habilitación de Hyper-V](../media/cluster-create-install-hyperv.png)

Para instalar Hyper-V en máquinas virtuales que ejecutan el sistema operativo de Azure Stack HCI, ejecute el siguiente comando:

```PowerShell
Enable-windowsoptionalfeature -online -featurename Microsoft-hyper-v
```

### <a name="step-17"></a>Paso 1,7
A veces, los servidores tardan más de lo esperado en reiniciarse después de instalar las actualizaciones. El Asistente para la implementación de clúster del centro de administración de Windows comprobará periódicamente el estado de reinicio del servidor para saber si el servidor se ha reiniciado correctamente. Sin embargo, si el usuario reinicia el servidor fuera del asistente manualmente, el asistente no tiene una manera de capturar el estado del servidor de una manera adecuada.

Si desea reiniciar el servidor manualmente, salga de la sesión del asistente actual. Una vez reiniciado el servidor, puede reiniciar el asistente.

### <a name="stretch-cluster-creation"></a>Creación de clústeres de stretch
Se recomienda usar servidores Unidos al dominio al crear un clúster extendido. Hay un problema de segmentación de la red al intentar usar equipos de grupo de trabajo para la implementación de clústeres extendidos debido a las limitaciones de WinRM.

### <a name="undo-and-start-over"></a>Deshacer y empezar de nuevo
Cuando se usan las mismas máquinas repetidamente para la implementación de clústeres, la limpieza de entidades de clúster anteriores es importante para obtener una implementación correcta del clúster en el mismo conjunto de máquinas. Consulte la página sobre la implementación de la [infraestructura hiperconvergida](https://docs.microsoft.com/windows-server/manage/windows-admin-center/use/deploy-hyperconverged-infrastructure#undo-and-start-over) para obtener instrucciones sobre cómo limpiar el clúster.

### <a name="credssp"></a>CredSSP
El Asistente para la implementación de clúster del centro de administración de Windows usa CredSSP en varios lugares. Este mensaje de error se ejecuta durante el asistente (esto ocurre con más frecuencia en el paso de validación del clúster):

![Captura de pantalla del error de creación del clúster de CredSSP](../media/cluster-create-credssp-error.jpg)

Puede usar los siguientes pasos para solucionar problemas:

1. Deshabilite la configuración de CredSSP en todos los nodos y en el equipo de puerta de enlace del centro de administración de Windows. Ejecute el primer comando en el equipo de la puerta de enlace y el segundo comando en todos los nodos del clúster:

```PowerShell
Disable-WsmanCredSSP -Role Client
```
```PowerShell
Disable-WsmanCredSSP -Role Server
```
2. Repare la confianza en todos los nodos. Ejecute el siguiente comando en todos los nodos:
```PowerShell
Test-ComputerSecureChannel -Verbose -Repair -Credential <account name>
```

3. Restablecimiento de datos de propagado de directiva de grupo mediante el comando
```Command Line
gpupdate /force
```

4. Reinicie los nodos. Después del reinicio, pruebe la conectividad entre la máquina de la puerta de enlace y los nodos de destino, así como la conectividad entre los nodos, mediante el comando siguiente:
```PowerShell
Enter-PSSession -computername <node fqdn>
```

### <a name="nested-virtualization"></a>Virtualización anidada
Al validar Azure Stack implementación del clúster de sistema operativo HCI en máquinas virtuales, es necesario activar la virtualización anidada antes de habilitar roles o características con el siguiente comando de PowerShell:

```PowerShell
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true
```

  > [!Note]
  > Para que la formación de equipos de conmutador virtual se realice correctamente en un entorno de máquina virtual, el siguiente comando debe ejecutarse en PowerShell en el host poco después de que se creen las máquinas virtuales: get-VM | % {Set-VMNetworkAdapter-VMName $ _. Nombre-MacAddressSpoofing on-AllowTeaming}

Si va a implementar un clúster mediante el sistema operativo de Azure Stack HCI, hay un requisito adicional. La unidad de disco duro virtual de arranque de la máquina virtual debe preinstalarse con las características de Hyper-V. Para ello, ejecute el siguiente comando antes de crear las máquinas virtuales:

```PowerShell
Install-WindowsFeature –VHD <Path to the VHD> -Name Hyper-V, RSAT-Hyper-V-Tools, Hyper-V-PowerShell
```

### <a name="support-for-rdma"></a>Compatibilidad con RDMA
El Asistente para la implementación de clústeres de la versión 2007 del centro de administración de Windows no admite la configuración de RDMA.

## <a name="failover-cluster-manager-solution"></a>Administrador de clústeres de conmutación por error solución

- Al administrar un clúster (hiperconvergido o tradicional), es posible que se produzca un error en el **Shell** . Si esto ocurre, vuelva a cargar el explorador o desplácese a otra herramienta y hacia atrás. [13882442]

- Puede producirse un problema al administrar un clúster de nivel inferior (Windows Server 2012 o 2012 R2) que no se ha configurado completamente. La solución para este problema es asegurarse de que se ha instalado y habilitado las características de Windows **rsat-clustering-PowerShell** en **cada nodo de miembro** del clúster. Para hacer esto con PowerShell, escriba el comando `Install-WindowsFeature -Name RSAT-Clustering-PowerShell` en todos los nodos del clúster. [12524664]

- Es posible que sea necesario agregar el clúster con el FQDN completo para que se detecte correctamente.

- Al conectarse a un clúster mediante el centro de administración de Windows instalado como puerta de enlace y proporcionar un nombre de usuario o contraseña explícitos para autenticarse, debe seleccionar **usar estas credenciales para todas las conexiones** para que las credenciales estén disponibles para consultar los nodos de miembro.

## <a name="hyper-converged-cluster-manager-solution"></a>Solución de administrador de clústeres hiperconvergida

- Algunos comandos, como **unidades-actualizar firmware**, **servidores-quitar** y **volúmenes abiertos,** están deshabilitados y no se admiten actualmente.

## <a name="azure-services"></a>Servicios de Azure

### <a name="azure-file-sync-permissions"></a>Azure File Sync permisos

Azure File Sync requiere permisos en Azure que el centro de administración de Windows no proporcionó antes de la versión 1910. Si ha registrado la puerta de enlace del centro de administración de Windows con Azure con una versión anterior a la versión 1910 del centro de administración de Windows, tendrá que actualizar la aplicación Azure Active Directory para obtener los permisos correctos para usar Azure File Sync en la versión más reciente del centro de administración de Windows. El permiso adicional permite a Azure File Sync realizar la configuración automática del acceso a la cuenta de almacenamiento como se describe en este artículo: Asegúrese de que [Azure File Sync tenga acceso a la cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal).

Para actualizar la aplicación Azure Active Directory, puede realizar una de estas dos acciones
1. Vaya a **configuración**  >  **Azure**  >  **anular registro**y, a continuación, vuelva a registrar el centro de administración de Windows con Azure, asegurándose de que elige crear una nueva aplicación Azure Active Directory.
2. Vaya a la aplicación de Azure Active Directory y agregue manualmente el permiso necesario a la aplicación de Azure Active Directory existente registrada en el centro de administración de Windows. Para ello, vaya a **configuración**  >  **Azure**  >  **vista de Azure en Azure**. Desde la hoja de **registro de aplicaciones** de Azure, vaya a **API permisos**y seleccione **Agregar un permiso**. Desplácese hacia abajo para seleccionar **Azure Active Directory gráfico**, seleccione **permisos delegados**, expanda el **directorio**y seleccione **Directory. AccessAsUser. All**. Haga clic en **Agregar permisos** para guardar las actualizaciones en la aplicación.

### <a name="options-for-setting-up-azure-management-services"></a>Opciones para configurar los servicios de administración de Azure

Los servicios de administración de Azure, incluidos Azure Monitor, Update Management de Azure y Azure Security Center, usan el mismo agente para un servidor local: el Microsoft Monitoring Agent. Azure Update Management tiene un conjunto más limitado de regiones admitidas y requiere que el área de trabajo Log Analytics esté vinculada a una cuenta de Azure Automation. Debido a esta limitación, si desea configurar varios servicios en el centro de administración de Windows, debe configurar primero Azure Update Management y, a continuación, Azure Security Center o Azure Monitor. Si ha configurado los servicios de administración de Azure que usan el Microsoft Monitoring Agent y, a continuación, intenta configurar Azure Update Management mediante el centro de administración de Windows, el centro de administración de Windows solo le permitirá configurar Azure Update Management si los recursos existentes vinculados a la Microsoft Monitoring Agent admiten Azure Update Management. Si no es así, tiene dos opciones:

1. Vaya al panel de control > Microsoft Monitoring Agent para [desconectar el servidor de las soluciones de administración de Azure existentes](https://docs.microsoft.com/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics) (como Azure Monitor o Azure Security Center). Después, configure Azure Update Management en el centro de administración de Windows. Después, puede volver a configurar las otras soluciones de administración de Azure a través del centro de administración de Windows sin problemas.
2. Puede [configurar manualmente los recursos de Azure necesarios para azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management) y, a continuación, [actualizar manualmente el Microsoft Monitoring Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) (fuera del centro de administración de Windows) para agregar el nuevo área de trabajo correspondiente a la solución de Update Management que quiere usar.
