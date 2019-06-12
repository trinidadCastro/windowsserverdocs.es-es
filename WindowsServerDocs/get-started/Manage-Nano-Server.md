---
title: Administración de Nano Server
description: actualizaciones, paquetes de servicio, seguimiento de redes, supervisión de rendimiento
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 599d6438-a506-4d57-a0ea-1eb7ec19f46e
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: cc535934705878c7f2b7fdc4e655ab5c853e4f96
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443533"
---
# <a name="manage-nano-server"></a>Administración de Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como una [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Echa un vistazo a [Cambios en Nano Server](nano-in-semi-annual-channel.md) para obtener más información.   

Nano Server se administra de forma remota. No hay ninguna funcionalidad de inicio de sesión local, ni es compatible con Terminal Services. Sin embargo, tiene una gran variedad de opciones para administrar Nano Server de forma remota, entre otras, Windows PowerShell, Instrumental de administración de Windows (WMI), Administración remota de Windows y Servicios de administración de emergencia (EMS).  

Para utilizar cualquier herramienta de administración remota, probablemente necesitará saber la dirección IP de Nano Server. Algunas formas de averiguar la dirección IP son:  
  
-   Utilice la Consola de recuperación de Nano (vea la sección Uso de la Consola de recuperación de Nano Server de este tema para obtener detalles).  
  
-   Conecte un cable serie al equipo y utilice EMS.  
  
-   Con el nombre de equipo asignado a Nano Server durante la configuración, puede obtener la dirección IP haciendo ping. Por ejemplo: `ping NanoServer-PC /4`.  
  
## <a name="using-windows-powershell-remoting"></a>Uso de la comunicación remota a Windows PowerShell  
Para administrar Nano Server con la comunicación remota a Windows PowerShell, necesita agregar la dirección IP de Nano Server a la lista de hosts de confianza del equipo de administración, agregar la cuenta que utiliza para los administradores de Nano Server y habilitar CredSSP si piensa usar esta característica.  

> [!NOTE]
> Si el destino de Nano Server y el equipo de administración están en el mismo bosque de AD DS (o en bosques con una relación de confianza), no debe agregar Nano Server a la lista de hosts de confianza, puede conectarse al servidor Nano mediante su nombre de dominio completo , por ejemplo: PS C:\> Enter-PSSession - ComputerName nanoserver.contoso.com-Credential (Get-Credential)
  
  
Para agregar Nano Server a la lista de hosts de confianza, ejecute este comando en un símbolo del sistema de Windows PowerShell con privilegios elevados:  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
Para iniciar la sesión remota de Windows PowerShell, inicie una sesión local de Windows PowerShell con privilegios elevados y, después, ejecute estos comandos:  
  
  
```  
$ip = "<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
Ahora puede ejecutar comandos de Windows PowerShell en Nano Server de la forma habitual.  
  
> [!NOTE]  
> No todos los comandos de Windows PowerShell están disponibles en esta versión de Nano Server. Para ver cuáles están disponibles, ejecute `Get-Command -CommandType Cmdlet`  
  
Detener la sesión remota con el comando `Exit-PSSession`  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>Uso de sesiones CIM en Windows PowerShell a través de WinRM  
Puede usar sesiones e instancias CIM en Windows PowerShell para ejecutar comandos WMI mediante la Administración remota de Windows (WinRM).  
  
Inicie la sesión CIM ejecutando estos comandos en un símbolo del sistema de Windows PowerShell:  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$user = $ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
Con la sesión establecida, puede ejecutar varios comandos WMI, por ejemplo:  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  
## <a name="windows-remote-management"></a>Administración remota de Windows  
Puede ejecutar programas de forma remota en Nano Server con la Administración remota de Windows (WinRM). Para usar WinRM, primero configure el servicio y establezca la página de códigos con estos comandos en un símbolo del sistema con privilegios elevados:  
  
```
winrm quickconfig
winrm set winrm/config/client @{TrustedHosts="<ip address of Nano Server>"}
chcp 65001
```
  
Ahora puede ejecutar comandos de forma remota en Nano Server. Por ejemplo:  

```
winrs -r:<IP address of Nano Server> -u:Administrator -p:<Nano Server administrator password> ipconfig
```
  
Para obtener más información sobre la Administración remota de Windows, vea [Información general sobre la Administración remota de Windows (WinRM)](https://technet.microsoft.com/library/dn265971.aspx).  
   
   
  
## <a name="running-a-network-trace-on-nano-server"></a>Ejecución un seguimiento de red en Nano Server  
 Netsh trace, Tracelog.exe y Logman.exe no están disponibles en Nano Server. Para capturar paquetes de red, puede usar estos cmdlets de Windows PowerShell:  
   
   
```  
New-NetEventSession [-Name]  
Add-NetEventPacketCaptureProvider -SessionName  
Start-NetEventSession [-Name]  
Stop-NetEventSession [-Name]  
```  
Estos cmdlets se documentan detalladamente en [Network Event Packet Capture Cmdlets](https://technet.microsoft.com/library/dn268520(v=wps.630).aspx) (Cmdlets de captura de paquetes de eventos de red) en Windows PowerShell.  

## <a name="installing-servicing-packages"></a>Instalación de paquetes de mantenimiento  
Si desea instalar paquetes de mantenimiento, utilice el parámetro -ServicingPackagePath (puede pasar una matriz de rutas de acceso a los archivos .cab):  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath \\path\to\kb123456.cab`  
  
A menudo, un paquete de servicio o revisión se descarga como un artículo de KB que contiene un archivo .cab. Siga estos pasos para extraer el archivo .cab, que puede instalar con el parámetro -ServicingPackagePath:  
  
1.  Descargue el paquete de mantenimiento desde el artículo de Knowledge Base asociado o desde el [Catálogo de Microsoft Update](https://catalog.update.microsoft.com/v7/site/home.aspx). Guárdelo en un directorio o red recurso compartido local, por ejemplo: C:\ServicingPackages  
2.  Cree una carpeta en la que guardará el paquete de mantenimiento extraído.  Ejemplo: c:\KB3157663_expanded  
3.  Abra una consola de Windows PowerShell y use el comando `Expand` especificando la ruta de acceso al archivo .msu del paquete de mantenimiento, incluidos el parámetro `-f:*` y la ruta de acceso donde desea extraer el paquete de mantenimiento.  Por ejemplo:  `Expand "C:\ServicingPackages\Windows10.0-KB3157663-x64.msu" -f:* "C:\KB3157663_expanded"`  
  
    Los archivos expandidos deben tener un aspecto similar al siguiente:  
C:>dir C:\KB3157663_expanded   
El volumen de la unidad C es el sistema operativo  
El número de serie del volumen es B05B-CC3D  
   
      Directorio de C:\KB3157663_expanded  
   
      04/19/2016  01:17 PM    \<DIR>          .  
      04/19/2016  01:17 PM    \<DIR&gt;          .  
        04/17/2016  12:31 AM               517 Windows10.0-KB3157663-x64-pkgProperties.txt  
04/17/2016  12:30 AM        93,886,347 Windows10.0-KB3157663-x64.cab  
04/17/2016  12:31 AM               454 Windows10.0-KB3157663-x64.xml  
04/17/2016  12:36 AM           185,818 WSUSSCAN.cab  
               4 File(s)     94,073,136 bytes  
               2 Dir(s)  328,559,427,584 bytes free  
4.  Ejecute `New-NanoServerImage` con el parámetro - ServicingPackagePath que apunta al archivo .cab en este directorio, por ejemplo: `New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath C:\KB3157663_expanded\Windows10.0-KB3157663-x64.cab`  

## <a name="managing-updates-in-nano-server"></a>Administración de actualizaciones en Nano Server

Actualmente se puede utilizar el proveedor de Windows Update para que el Instrumental de administración de Windows (WMI) busque la lista de actualizaciones aplicables y luego las instale todas o solo un subconjunto. Si usa Windows Server Update Services (WSUS), también puede configurar Nano Server para ponerse en contacto con el servidor WSUS para obtener actualizaciones.  

En todos los casos, primero hay que establecer una sesión remota de Windows PowerShell en el equipo de Nano Server. Estos ejemplos utilizan *$sess* para la sesión; si utiliza otro elemento, reemplácelo según sea necesario.  


### <a name="view-all-available-updates"></a>Visualización de todas las actualizaciones  
---  
Obtenga la lista completa de las actualizaciones aplicables con estos comandos:  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}  
```  
**Nota:**  
Si no hay actualizaciones disponibles, este comando devolverá el error siguiente:  
```  
Invoke-CimMethod : A general error occurred that is not covered by a more specific error code.  

At line:1 char:16  

+ ... anResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUp ...  

+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  

    + CategoryInfo          : NotSpecified: (MSFT_WUOperatio...-5b842a3dd45d")  

   :CimInstance) [Invoke-CimMethod], CimException  

    + FullyQualifiedErrorId : MI RESULT 1,Microsoft.Management.Infrastructure.  

   CimCmdlets.InvokeCimMethodCommand  
```  

### <a name="install-all-available-updates"></a>Instalar todas las actualizaciones disponibles  
---  
Puede detectar, descargar e instalar **todas** las actualizaciones disponibles a la vez mediante los siguientes comandos:  

```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ApplyApplicableUpdates  

Restart-Computer  
```  
**Nota:**  
Windows Defender impide que se instalen actualizaciones. Para solucionar este problema, desinstale Windows Defender, instale las actualizaciones y después vuelva a instalar Windows Defender. Como alternativa, puede descargar las actualizaciones en otro equipo, copiarlas en Nano Server y luego aplicarlas con DISM.exe.  


### <a name="verify-installation-of-updates"></a>Verificación de la instalación de actualizaciones  
---  
Utilice estos comandos para obtener una lista de las actualizaciones instaladas actualmente:  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}  
```  

**Nota:**  
Estos comandos muestran lo que está instalado, pero no aparece específicamente la indicación “instalada” en la salida. Si necesita que se indique en la salida, como en un informe, puede ejecutar  
```  
Get-WindowsPackage--Online  
```  

### <a name="using-wsus"></a>Uso de WSUS  
---  
Los comandos enumerados anteriormente consultarán el servicio de Windows Update y Microsoft Update en Internet para buscar y descargar actualizaciones. Si utiliza WSUS, puede establecer las claves del Registro en Nano Server para utilizar el servidor WSUS en su lugar.  
  
Consulta la tabla "Windows Update Agent Environment Options Registry Keys" (Claves del Registro de las opciones del entorno del Agente de Windows Update) en [Configure Automatic Updates in a Non-Active Directory Environment](https://technet.microsoft.com/library/cc708449(v=ws.10).aspx) (Configuración de Actualizaciones automáticas en un entorno que no es de Active Directory).  
  
Debes establecer al menos las claves del Registro **WUServer** y **WUStatusServer**, pero en función de cómo hayas implementado WSUS, puede que se necesiten otros valores. Siempre puede confirmar esta configuración mediante el examen de otro servidor de Windows Server en el mismo entorno.  

Una vez que estos valores se establecen para el servidor WSUS, los comandos de la sección anterior consultarán ese servidor para las actualizaciones y lo usarán como origen de descarga.  

### <a name="automatic-updates"></a>Actualizaciones automáticas  
---  
Actualmente, la manera de automatizar la instalación de la actualización es convertir los pasos anteriores en un script de Windows PowerShell local y luego crear una tarea programada para ejecutarlo y reiniciar el sistema según la programación.


## <a name="performance-and-event-monitoring-on-nano-server"></a>Supervisión de rendimiento y eventos en Nano Server
[comment]: # (desde Venkat Yalla.)
Nano Server es totalmente compatible con el marco de [Seguimiento de eventos para Windows](https://aka.ms/u2pa0i) (ETW), pero algunas herramientas familiares utilizadas para administrar el seguimiento y los contadores de rendimiento no están disponibles actualmente en Nano Server. Sin embargo, Nano Server tiene herramientas y cmdlets para ejecutar los escenarios más comunes de análisis de rendimiento.

El flujo de trabajo de alto nivel sigue siendo el mismo que en cualquier instalación de Windows Server: el seguimiento de baja sobrecarga se realiza en el equipo de destino (Nano Server), y los archivos de seguimiento resultantes y los registros se procesan posteriormente sin conexión en un equipo independiente con herramientas como [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx), [Analizador de mensajes](https://www.microsoft.com/download/details.aspx?id=44226) u otras.

> [!NOTE]
> Vea [How to copy files to and from Nano Server](https://aka.ms/nri9c8) (Copia de archivos a y desde Nano Server) para hacer un repaso sobre cómo transferir archivos mediante la comunicación remota a PowerShell.

En las secciones siguientes se enumeran las actividades de recopilación de datos de rendimiento más comunes junto con una forma compatible de llevarlas a cabo en Nano Server.

### <a name="query-available-event-providers"></a>Proveedores de eventos disponibles para consultas
[Windows Performance Recorder](https://msdn.microsoft.com/en-us/library/hh448229.aspx) es la herramienta para consultar a los proveedores de eventos disponibles como sigue:
```
wpr.exe -providers
```

Puede filtrar la salida por el tipo de eventos que le interesan. Por ejemplo:
```
PS C:\> wpr.exe -providers | select-string "Storage"

       595f33ea-d4af-4f4d-b4dd-9dacdd17fc6e                              : Microsoft-Windows-StorageManagement-WSP-Host
       595f7f52-c90a-4026-a125-8eb5e083f15e                              : Microsoft-Windows-StorageSpaces-Driver
       69c8ca7e-1adf-472b-ba4c-a0485986b9f6                              : Microsoft-Windows-StorageSpaces-SpaceManager
       7e58e69a-e361-4f06-b880-ad2f4b64c944                              : Microsoft-Windows-StorageManagement
       88c09888-118d-48fc-8863-e1c6d39ca4df                              : Microsoft-Windows-StorageManagement-WSP-Spaces
```

### <a name="record-traces-from-a-single-etw-provider"></a>Seguimientos de registros de un único proveedor de ETW
Puede utilizar los nuevos [cmdlets de administración de seguimiento de eventos](https://technet.microsoft.com/library/dn919247.aspx) para esto. A continuación se presenta un flujo de trabajo de ejemplo:

Cree e inicie el seguimiento, especificando un nombre de archivo para almacenar los eventos.
```
PS C:\> New-EtwTraceSession -Name "ExampleTrace" -LocalFilePath c:\etrace.etl
```

Agregue un GUID de proveedor para el seguimiento. Utilice ```wpr.exe -providers``` para el nombre de proveedor en la traducción de GUID. 
```
PS C:\> wpr.exe -providers | select-string "Kernel-Memory"

       d1d93ef7-e1f2-4f45-9943-03d245fe6c00                              : Microsoft-Windows-Kernel-Memory

PS C:\> Add-EtwTraceProvider -Guid "{d1d93ef7-e1f2-4f45-9943-03d245fe6c00}" -SessionName "ExampleTrace"
```

Quite el seguimiento, ya que así se detiene la sesión de seguimiento, y los eventos se vacían en el archivo de registro asociado.
```
PS C:\> Remove-EtwTraceSession -Name "ExampleTrace"

PS C:\> dir .\etrace.etl

    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/14/2016  11:17 AM       16515072 etrace.etl
```
> [!NOTE]
> En este ejemplo se muestra la incorporación de un proveedor de seguimiento único a la sesión, pero también puede utilizar el cmdlet ```Add-EtwTraceProvider``` varias veces en una sesión de seguimiento con GUID de proveedor diferentes para habilitar el seguimiento desde varios orígenes. Otra alternativa es usar los perfiles ```wpr.exe``` que se describen a continuación.

### <a name="record-traces-from-multiple-etw-providers"></a>Seguimientos de registros de un varios proveedores de ETW
La opción ```-profiles``` de [Windows Performance Recorder](https://msdn.microsoft.com/library/hh448229.aspx) permite realizar el seguimiento de varios proveedores al mismo tiempo. Hay un número de perfiles integrados como CPU, red y DiskIO entre los que se puede elegir:
```
PS C:\Users\Administrator\Documents> wpr.exe -profiles 

Microsoft Windows Performance Recorder Version 10.0.14393 (CoreSystem)
Copyright (c) 2015 Microsoft Corporation. All rights reserved.

        GeneralProfile              First level triage
        CPU                         CPU usage
        DiskIO                      Disk I/O activity
        FileIO                      File I/O activity
        Registry                    Registry I/O activity
        Network                     Networking I/O activity
        Heap                        Heap usage
        Pool                        Pool usage
        VirtualAllocation           VirtualAlloc usage
        Audio                       Audio glitches
        Video                       Video glitches
        Power                       Power usage
        InternetExplorer            Internet Explorer
        EdgeBrowser                 Edge Browser
        Minifilter                  Minifilter I/O activity
        GPU                         GPU activity
        Handle                      Handle usage
        XAMLActivity                XAML activity
        HTMLActivity                HTML activity
        DesktopComposition          Desktop composition activity
        XAMLAppResponsiveness       XAML App Responsiveness analysis
        HTMLResponsiveness          HTML Responsiveness analysis
        ReferenceSet                Reference Set analysis
        ResidentSet                 Resident Set analysis
        XAMLHTMLAppMemoryAnalysis   XAML/HTML application memory analysis
        UTC                         UTC Scenarios
        DotNET                      .NET Activity
        WdfTraceLoggingProvider     WDF Driver Activity
```

Para obtener instrucciones detalladas sobre cómo crear perfiles personalizados, vea la [documentación sobre WPR.exe](https://msdn.microsoft.com/library/windows/hardware/hh448223.aspx).

### <a name="record-etw-traces-during-operating-system-boot-time"></a>Registro de seguimientos de ETW durante el tiempo de arranque del sistema operativo
Utilice el cmdlet ```New-AutologgerConfig``` para recopilar eventos durante el arranque del sistema. Su uso es muy similar al del cmdlet ```New-EtwTraceSession```, pero los proveedores agregados a la configuración del registrador automático solo estarán disponibles en el siguiente arranque. El flujo de trabajo general tiene este aspecto:

En primer lugar, cree una nueva configuración del registrador automático.
```
PS C:\> New-AutologgerConfig -Name "BootPnpLog" -LocalFilePath c:\bootpnp.etl 
```

Agregue a él un proveedor de ETW. Este ejemplo utiliza el proveedor de kernel PnP. Invoque ```Add-EtwTraceProvider``` de nuevo, especificando el mismo nombre del registrador automático, pero un GUID diferente, para habilitar la recopilación de seguimientos de arranque desde varios orígenes.
```
Add-EtwTraceProvider -Guid "{9c205a39-1250-487d-abd7-e831c6290539}" -AutologgerName BootPnpLog
```

No se inicia una sesión de ETW inmediatamente, pero en su lugar se configura una para que se inicie en el siguiente arranque. Después de reiniciar, se inicia automáticamente una nueva sesión de ETW con el nombre de la configuración del registrador automático con los proveedores de seguimiento agregados habilitados. Una vez que arranca Nano Server, el siguiente comando detendrá la sesión de seguimiento después de vaciar los eventos registrados en el archivo de seguimiento asociado:
```
PS C:\> Remove-EtwTraceSession -Name BootPnpLog
```

Para evitar que se cree automáticamente otra sesión de seguimiento en el siguiente arranque, quite la configuración del registrador automático como sigue:
```
PS C:\> Remove-AutologgerConfig -Name BootPnpLog
```

Para recopilar seguimientos de arranque e instalación de un número de sistemas o de un sistema sin disco, considere el uso de [Configurar y arrancar la recopilación de eventos](../administration/get-started-with-setup-and-boot-event-collection.md).

### <a name="capture-performance-counter-data"></a>Captura de datos del contador de rendimiento
Normalmente, supervise los datos del contador de rendimiento con la GUI de Perfmon.exe. En Nano Server, use el equivalente de la línea de comandos ```Typeperf.exe```. Por ejemplo:

Consultar los contadores disponibles: puede filtrar el resultado para encontrar con facilidad lo que más le interesa.
```
PS C:\> typeperf.exe -q | Select-String "UDPv6"

\UDPv6\Datagrams/sec
\UDPv6\Datagrams Received/sec
\UDPv6\Datagrams No Port/sec
\UDPv6\Datagrams Received Errors
\UDPv6\Datagrams Sent/sec
```

Las opciones permiten especificar la frecuencia y el intervalo para la recopilación de los valores de los contadores. En el ejemplo siguiente, el tiempo de inactividad de procesador se recopila 5 veces cada 3 segundos.
```
PS C:\> typeperf.exe "\Processor Information(0,0)\% Idle Time" -si 3 -sc 5

"(PDH-CSV 4.0)","\\ns-g2\Processor Information(0,0)\% Idle Time"
"09/15/2016 09:20:56.002","99.982990"
"09/15/2016 09:20:59.002","99.469634"
"09/15/2016 09:21:02.003","99.990081"
"09/15/2016 09:21:05.003","99.990454"
"09/15/2016 09:21:08.003","99.998577"
Exiting, please wait...
The command completed successfully.
```

Otras opciones de línea de comandos le permiten especificar los nombres de contador de rendimiento de interés en un archivo de configuración y redirigir la salida a un archivo de registro, entre otras acciones. Vea la [documentación sobre typeperf.exe](https://technet.microsoft.com/library/bb490960.aspx) para obtener más información.

También puede utilizar la interfaz gráfica de Perfmon.exe de forma remota con destinos de Nano Server. Al agregar los contadores de rendimiento a la vista, especifique el destino de Nano Server en el nombre de equipo en lugar del valor predeterminado *<Local computer>* .

### <a name="interact-with-the-windows-event-log"></a>Interactuación con el registro de eventos de Windows

Nano Server admite el cmdlet ```Get-WinEvent```, que proporciona el registro de eventos de Windows filtrando y consultando las capacidades, tanto localmente así como en un equipo remoto. Las opciones y los ejemplos detallados están disponibles en la [página de documentación de Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx). En este sencillo ejemplo se recuperan los *errores* indicados en el registro del *sistema* durante los últimos dos días.
```
PS C:\> $StartTime = (Get-Date) - (New-TimeSpan -Day 2)
PS C:\> Get-WinEvent -FilterHashTable @{LogName='System'; Level=2; StartTime=$StartTime} | select TimeCreated, Message

TimeCreated           Message
-----------           -------
9/15/2016 11:31:19 AM Task Scheduler service failed to start Task Compatibility module. Tasks may not be able to reg...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 6 failed with status: {File...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 0 failed with status: {File...
```

Nano Server también admite ```wevtutil.exe```, que permite recuperar información sobre los registros de eventos y publicadores. Vea la [documentación de wevtutil.exe](https://aka.ms/qvod7p) para obtener más detalles. 

### <a name="graphical-interface-tools"></a>Herramientas de interfaz gráfica
Las [herramientas de administración de servidor basadas en Web](https://blogs.technet.microsoft.com/servermanagement/2016/08/17/deploy-setup-server-management-tools/) pueden usarse para administrar los destinos de Nano Server de forma remota y presentar un registro de eventos de Nano Server mediante un explorador web. Por último, el Visor de eventos como complemento de MMC (eventvwr.msc) puede utilizarse también para ver registros; simplemente ábralo en un equipo con un escritorio y remítalo a un servidor remoto de Nano Server.




## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>Uso de la configuración de estado deseado de Windows PowerShell con Nano Server  
  
Puede administrar Nano Server como nodos de destino con la configuración de estado deseado (DSC) de Windows PowerShell. Actualmente, puede administrar los nodos que ejecutan Nano Server con DSC en modo de inserción solo. No todas las características de DSC funcionan con Nano Server.  
  
Para obtener todos los detalles, vea [Uso de DSC en Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).  
