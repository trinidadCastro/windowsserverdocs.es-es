---
title: Desarrollo de cmdlets de PowerShell para Nano Server
description: 'traslado de CIM, cmdlets de.NET, C++ '
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b4267f0-1c91-4a40-9262-5daf4659f686
author: jaimeo
ms.author: jaimeo
ms.date: 09/06/2017
ms.localizationpriority: medium
ms.openlocfilehash: 4c669db414c4f12b6145a26a75b83449f43e8918
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887686"
---
# <a name="developing-powershell-cmdlets-for-nano-server"></a>Desarrollo de cmdlets de PowerShell para Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como una [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Echa un vistazo a [Cambios en Nano Server](nano-in-semi-annual-channel.md) para obtener más información. 
  
## <a name="overview"></a>Información general  
Nano Server incluye PowerShell Core de forma predeterminada en todas las instalaciones de Nano Server. PowerShell Core es una edición de superficie reducida de PowerShell que se basa en .NET Core y se ejecuta en las ediciones de superficie reducida de Windows, como Nano Server y Windows IoT Core. PowerShell Core funciona de la misma manera que otras ediciones de PowerShell, como Windows PowerShell se ejecutan en Windows Server 2016. Sin embargo, la superficie reducida de Nano Server significa que no todas las características de PowerShell de Windows Server 2016 están disponibles en PowerShell Core en Nano Server.  
  
Si dispone de los cmdlets de PowerShell existentes que desea ejecutar en Nano Server o está desarrollando nuevos para ese propósito, este tema incluye consejos y sugerencias que deben facilitar el proceso.  

## <a name="powershell-editions"></a>Ediciones de PowerShell  
  
  
A partir de la versión 5.1, PowerShell está disponible en diferentes ediciones que denotan distintos conjuntos de características y compatibilidad de la plataforma.  
  
- **Edición de escritorio:** Basado en .NET Framework y proporciona compatibilidad con scripts y módulos destinados a versiones de PowerShell que se ejecutan en las ediciones de superficie completa de Windows como Server Core y el escritorio de Windows.  
- **Core Edition:** Basado en .NET Core y proporciona compatibilidad con scripts y módulos destinados a versiones de PowerShell que se ejecutan en las ediciones de superficie reducida de Windows como Nano Server y Windows IoT.  
  
La edición de ejecución de PowerShell se muestra en la propiedad PSEdition de $PSVersionTable.  
```powershell  
$PSVersionTable  
  
Name                           Value  
----                           -----  
PSVersion                      5.1.14300.1000  
PSEdition                      Desktop  
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}  
CLRVersion                     4.0.30319.42000  
BuildVersion                   10.0.14300.1000  
WSManStackVersion              3.0  
PSRemotingProtocolVersion      2.3  
SerializationVersion           1.1.0.1  
```  
  
Los autores del módulo pueden declarar sus módulos para que sean compatibles con una o más ediciones de PowerShell mediante la clave de manifiesto de módulo CompatiblePSEditions. Esta clave solo se admite en PowerShell 5.1 o posterior.  
```powershell  
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1  
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1  
$moduleInfo.CompatiblePSEditions  
Desktop  
Core  
  
$moduleInfo | Get-Member CompatiblePSEditions  
  
   TypeName: System.Management.Automation.PSModuleInfo  
  
Name                 MemberType Definition  
----                 ---------- ----------  
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}  
  
```  
Al obtener una lista de los módulos disponibles, puede filtrar la lista de edición de PowerShell.  
```powershell  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Desktop"  
  
    Directory: C:\Program Files\WindowsPowerShell\Modules  
  
  
ModuleType Version    Name                                ExportedCommands  
---------- -------    ----                                ----------------  
Manifest   1.0        ModuleWithPSEditions  
  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Core" | % CompatiblePSEditions  
Desktop  
Core  
  
```  
Los autores de scripts pueden impedir la ejecución de un script, a menos que se ejecute en una edición compatible de PowerShell mediante el parámetro PSEdition con una instrucción #requires.  
```powershell  
Set-Content C:\script.ps1 -Value "#requires -PSEdition Core  
Get-Process -Name PowerShell"  
Get-Content C:\script.ps1  
#requires -PSEdition Core  
Get-Process -Name PowerShell  
  
C:\script.ps1  
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a "#requires" statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.  
At line:1 char:1  
+ C:\script.ps1  
+ ~~~~~~~~~~~~~  
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException  
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition  
```  
  
  
## <a name="installing-nano-server"></a>Instalación de Nano Server  
Se proporcionan pasos detallados y de inicio rápido para la instalación de Nano Server en máquinas físicas o virtuales en [Instalación de Nano Server](Getting-Started-with-Nano-Server.md), que es el tema principal de este.  
  
> [!NOTE]  
> Para el trabajo de desarrollo en Nano Server, le resultará útil instalar Nano Server con el parámetro -Development de New-NanoServerImage. Habilita la instalación de controladores no firmados, copia los archivos binarios de depurador, abre un puerto para la depuración, habilita la firma de prueba y habilita la instalación de paquetes AppX sin una licencia de desarrollador. Por ejemplo:  
>  
>`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`  
  
## <a name="determining-the-type-of-cmdlet-implementation"></a>Determinación del tipo de implementación de cmdlet  
PowerShell es compatible con varios tipos de implementación para los cmdlets, y el que ha utilizado determina el proceso y las herramientas necesarios para crearlos o migrarlos para trabajar en Nano Server. Los tipos de implementación compatibles son:  
* CIM: consta de los archivos CDXML superpuestos por proveedores CIM (WMIv2)   
* .NET: consta de ensamblados de .NET que implementan interfaces administradas de cmdlet, normalmente escritas en C#   
* Script de PowerShell: consta de módulos de script (.psm1) o scripts (.ps1) escritos en el lenguaje de PowerShell   
  
Si no está seguro de qué implementación ha usado para los cmdlets existentes que desea migrar, instale el producto o la característica y después busque la carpeta del módulo de PowerShell en una de las siguientes ubicaciones:   
  
* %windir%\system32\WindowsPowerShell\v1.0\Modules   
* %ProgramFiles%\WindowsPowerShell\Modules   
* %UserProfile%\Documents\WindowsPowerShell\Modules   
* \<la ubicación de instalación del producto >   
    
 Compruebe en estas ubicaciones estos detalles:  
 * Los cmdlets CIM tienen extensiones de archivo .cdxml.  
 * Los cmdlets de .NET tienen extensiones de archivo .dll o tienen ensamblados instalados en la GAC que aparecen en el archivo. psd1 en los campos RootModule, ModuleToProcess o NestedModules.  
* Los cmdlets de script de PowerShell tienen las extensiones de archivo .psm1 o .ps1.   
  
## <a name="porting-cim-cmdlets"></a>Migración de los cmdlets CIM  
Por lo general, estos cmdlets deben funcionar en Nano Server sin que sea necesario realizar ninguna conversión. Sin embargo, debe migrar el proveedor de WMI v2 subyacente para ejecutarlo en Nano Server si no lo ha hecho ya.  
  
### <a name="building-c-for-nano-server"></a>Compilación en C++ para Nano Server  
Para que los archivos DLL escritos en C++ funcionen en Nano Server, compílelos para Nano Server en lugar de para una edición específica.  
  
Para consultar los requisitos previos y un tutorial de desarrollo con C++ en Nano Server, vea [Developing Native Apps on Nano Server](http://blogs.technet.com/b/nanoserver/archive/2016/04/27/developing-native-apps-on-nano-server.aspx) (Desarrollo de aplicaciones nativas en Nano Server).  
  
  
## <a name="porting-net-cmdlets"></a>Migración de los cmdlets de .NET  
La mayor parte del código C# se admite en Nano Server. Puede usar [ApiPort](https://github.com/Microsoft/dotnet-apiport) para examinar las API incompatibles.  
  
### <a name="powershell-core-sdk"></a>SDK de PowerShell Core  
El módulo "Microsoft.PowerShell.NanoServer.SDK" está disponible en la [Galería de PowerShell](https://www.powershellgallery.com/packages/Microsoft.PowerShell.NanoServer.SDK/) para facilitar el desarrollo de cmdlets de .NET mediante Visual Studio 2015 Update 2, que está destinado a las versiones de CoreCLR y PowerShell Core disponibles en Nano Server. Puede instalar el módulo mediante PowerShellGet con este comando:  
  
`Find-Module Microsoft.PowerShell.NanoServer.SDK -Repository PSGallery | Install-Module -Scope <scope>`  
  
El módulo del SDK de PowerShell Core expone cmdlets para configurar los ensamblados de referencia de CoreCLR y PowerShell Core correctos, crear un proyecto de C# en Visual Studio 2015 para esos ensamblados de referencia y configurar el depurador remoto en una máquina Nano Server, para que los desarrolladores puedan depurar los cmdlets de .NET que ejecutan de forma remota para Nano Server en Visual Studio 2015.  
  
El módulo del SDK de PowerShell Core requiere Visual Studio 2015 Update 2. Si no tiene instalado Visual Studio 2015, puede instalar [Visual Studio Community 2015](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx).  
  
El módulo de SDK también depende de la característica siguiente que se va a instalar en Visual Studio 2015:  
  
- Desarrollo de Web y de Windows -> Herramientas de desarrollo de aplicaciones universales de Windows -> Herramientas (1.3.1) y SDK de Windows 10  
  
Revise la instalación de Visual Studio antes de utilizar el módulo de SDK para garantizar que se satisfacen estos requisitos previos. Asegúrese de que se selecciona instalar la característica anterior durante la instalación de Visual Studio, o modifique la instalación existente de Visual Studio 2015 para instalarla.  
  
El módulo de SDK de PowerShell Core incluye los siguientes cmdlets:  
- New-NanoCSharpProject: Crea un nuevo Visual Studio C# proyecto destinado a CoreCLR y PowerShell Core incluido en la versión de Windows Server 2016 de Nano Server.  
- Show-SdkSetupReadMe: Se abre la carpeta raíz del SDK en el Explorador de archivos y abre el archivo Léame.txt para la instalación manual.  
- Install-RemoteDebugger: Instala y configura al depurador remoto de Visual Studio en un equipo de Nano Server.  
- Start-RemoteDebugger: Inicia al depurador remoto en un equipo remoto que ejecuta Nano Server.  
- Stop-RemoteDebugger: Detiene al depurador remoto en un equipo remoto que ejecuta Nano Server.  
  
Para obtener información detallada sobre cómo usar los cmdlets, ejecute Get-Help en cada cmdlet después de instalar e importar el módulo como sigue:  
  
`Get-Command -Module Microsoft.PowerShell.NanoServer.SDK | Get-Help -Full`   
  
  
### <a name="searching-for-compatible-apis"></a>Búsqueda de API compatibles  
  
Puede buscar en el catálogo de API para .NET Core o desensamblar los ensamblados de referencia de CoreCLR. Para obtener más información sobre la portabilidad de la plataforma de API de .NET, vea [Platform Portability](https://github.com/Microsoft/dotnet-apiport/blob/master/docs/HowTo/PlatformPortability.md) (Portabilidad de la plataforma).  
  
### <a name="pinvoke"></a>PInvoke  
En CoreCLR que Nano Server utiliza, algunos archivos DLL fundamentales, como kernel32.dll y advapi32.dll, se dividieron en varios conjuntos de API, por lo que deberá asegurarse de que las llamadas a PInvoke hacen referencia a la API correcta. Cualquier incompatibilidad se manifiesta como un error en tiempo de ejecución.  
  
Para obtener una lista de las API nativas admitidas en Nano Server, vea [Nano Server APIs](https://msdn.microsoft.com/library/mt588480(v=vs.85).aspx) (API de Nano Server).  
  
### <a name="building-c-for-nano-server"></a>Compilación en C# para Nano Server  
  
Una vez que se crea un proyecto de C# en Visual Studio 2015 mediante `New-NanoCSharpProject`, simplemente puede compilarlo en Visual Studio; para ello, haga clic en el menú **Compilar** y seleccione **Generar proyecto** o **Generar solución**. Los ensamblados generados estarán destinados a CoreCLR y PowerShell Core integrados en Nano Server; puede copiar los ensamblados en un equipo que ejecuta Nano Server y utilizarlos.  
  
### <a name="building-managed-c-cppcli-for-nano-server"></a>Compilación en C++ administrado (CPP/CLI) para Nano Server  
C++ administrado no se admite para CoreCLR. Al migrar a CoreCLR, vuelva a escribir el código de C++ administrado en C# y realice todas las llamadas nativas a través de PInvoke.  
  
## <a name="porting-powershell-script-cmdlets"></a>Migración de cmdlets de script de PowerShell  
  
PowerShell Core tiene paridad completa del lenguaje de PowerShell con otras ediciones de PowerShell, incluida la versión que se ejecuta en Windows Server 2016 y Windows 10. Sin embargo, al migrar cmdlets de script de PowerShell a Nano Server, tenga estos factores en cuenta:  
* ¿Existen dependencias en otros cmdlets? Si es así, ¿están dichos cmdlets disponibles en Nano Server? Vea [PowerShell en Nano Server](PowerShell-on-Nano-Server.md) para obtener información sobre lo que no está disponible.  
* Si tiene dependencias en ensamblados que se cargan en tiempo de ejecución, ¿seguirán funcionando?   
* ¿Cómo puede depurar el script de forma remota?   
* ¿Cómo puede migrar desde WMI .Net a MI .Net?  
  
### <a name="dependency-on-built-in-cmdlets"></a>Dependencia de cmdlets integrados  
No todos los cmdlets de Windows Server 2016 están disponibles en Nano Server (vea [PowerShell en Nano Server](PowerShell-on-Nano-Server.md)). El mejor enfoque consiste en configurar una máquina virtual Nano Server y detectar si los cmdlets que necesita están disponibles. Para ello, ejecute `Enter-PSSession` para conectarse al servidor de destino de Nano Server y después ejecute `Get-Command -CommandType Cmdlet, Function` para obtener la lista de cmdlets disponibles.  
  
### <a name="consider-using-powershell-classes"></a>Consideración del uso de las clases de PowerShell  
Add-Type es compatible con Nano Server para compilar código C# en línea. Si escribe código nuevo o migra código existente, también puede considerar la utilización de clases de PowerShell para definir tipos personalizados. Puede utilizar las clases de PowerShell para escenarios de contenedor de propiedades y para enumeraciones. Si necesita hacer una llamada a PInvoke, hágala mediante C# con Add-Type o en un ensamblado compilado previamente.  
Este es un ejemplo que muestra el uso de Add-Type:  
  
```  
Add-Type -ReferencedAssemblies ([Microsoft.Management.Infrastructure.Ciminstance].Assembly.Location) -TypeDefinition @'  
public class TestNetConnectionResult  
{  
   // The compute name  
   public string ComputerName = null;  
   // The Remote IP address used for connectivity  
   public System.Net.IPAddress RemoteAddress = null;  
}  
'@  
# Create object and set properties  
$result = New-Object TestNetConnectionResult  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
 Este ejemplo muestra el uso de clases de PowerShell en Nano Server:  
   
```  
class TestNetConnectionResult    
{    
   # The compute name  
  [string] $ComputerName    
  
  #The Remote IP address used for connectivity    
  [System.Net.IPAddress] $RemoteAddress  
}  
# Create object and set properties  
$result = [TestNetConnectionResult]::new()  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
  
### <a name="remotely-debugging-scripts"></a>Depuración remota de scripts  
  
Para depurar un script de forma remota, conéctese al equipo remoto utilizando `Enter-PSsession` desde PowerShell ISE. Una vez dentro de la sesión, puede ejecutar `psedit <file_path>`, y una copia del archivo se abrirá en PowerShell ISE local. A continuación, puede depurar el script como si se estuviera realizando la ejecución localmente mediante el establecimiento de puntos de interrupción. Además, los cambios realizados en este archivo se guardarán en la versión remota.   
  
### <a name="migrating-from-wmi-net-to-mi-net"></a>Migración de WMI .NET a MI .NET  
  
[WMI .NET](https://msdn.microsoft.com/library/mt481551(v=vs.110).aspx) no es compatible, por lo que debe migrar todos los cmdlets que usan la API anterior a la API de WMI admitida: [MI. .NET](https://msdn.microsoft.com/library/dn387184(v=vs.85).aspx). Puede acceder a MI .NET directamente a través de C# o a través de los cmdlets en el módulo CimCmdlets.   
  
### <a name="cimcmdlets-module"></a>Módulo CimCmdlets  
  
Los cmdlets WMI v1 (por ejemplo, Get-WmiObject) no se admiten en Nano Server. Sin embargo, sí se admiten los cmdlets CIM (por ejemplo, Get-CimInstance) en el módulo CimCmdlets. Los cmdlets CIM se asocian de manera bastante precisa con los cmdlets WMI v1. Por ejemplo, Get-WmiObject se correlaciona con Get-CimInstance mediante la utilización de parámetros muy similares. La sintaxis de invocación de método es ligeramente diferente, pero está bien documentada mediante Invoke-CimMethod. Tenga cuidado con respecto a los tipos de parámetros. MI .NET tiene requisitos más estrictos con respecto a los tipos de parámetros de métodos.  
  
### <a name="c-api"></a>API DE C#  
  
WMI .NET contiene la interfaz de WMIv1, mientras que MI .NET contiene la interfaz de WMIv2 (CIM). Las clases expuestas pueden ser distintas, pero las operaciones subyacentes son muy similares. Enumere u obtenga instancias de objetos e invoque operaciones en ellos para realizar las tareas.   
  
  


