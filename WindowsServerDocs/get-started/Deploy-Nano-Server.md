---
title: Implementación de Nano Server
description: Explica la creación e implementación de imágenes personalizadas, paquetes, controladores, dominios, funciones y características.
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9f109c91-7c2e-4065-856c-ce9e2e9ce558
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: e61844cfb04f95723fe9d08b9bd2e8b481714eea
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66442229"
---
# <a name="deploy-nano-server"></a>Implementación de Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Echa un vistazo a [Cambios en Nano Server](nano-in-semi-annual-channel.md) para más información. 

En este tema se incluye información necesaria para implementar imágenes de Nano Server más personalizadas según sus necesidades en comparación con los sencillos ejemplos que figuran en el tema Inicio rápido de Nano Server. Encontrará información sobre la realización de una imagen personalizada de Nano Server exactamente con las características que desee, la instalación de imágenes de Nano Server desde VHD o WIM, la edición de archivos, el trabajo con dominios, la administración de paquetes mediante varios métodos y el trabajo con roles de servidor.

## <a name="nano-server-image-builder"></a>Nano Server Image Builder

Nano Server Image Builder es una herramienta que ayuda a crear una imagen personalizada de Nano Server y medios USB de arranque con la ayuda de una interfaz gráfica. Según las entradas que proporcione, genera scripts de PowerShell reutilizables que permiten automatizar fácilmente instalaciones coherentes de Nano Server que ejecutan las ediciones Windows Server 2016 Datacenter o Standard.

Obtenga la herramienta en el [Centro de descarga](https://www.microsoft.com/en-us/download/details.aspx?id=54065). 

La herramienta también requiere [Windows Assessment and Deployment Kit (ADK)](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit).


Nano Server Image Builder crea imágenes de Nano Server personalizadas en los formatos VHD, VHDX o ISO y puede crear medios USB de arranque para implementar Nano Server o detectar la configuración de hardware de un servidor. También puede hacer lo siguiente:

- Aceptar los términos de licencia 
- Crear formatos VHD, VHDX o ISO
- Agregar roles de servidor
- Agregar controladores de dispositivos
- Establecer nombre de equipo, contraseña de administrador, ruta de acceso del archivo de registro y zona horaria
- Unirse a un dominio con una cuenta de Active Directory existente o un blob obtenido de la unión a un dominio
- Habilitar WinRM para la comunicación fuera de la subred local
- Habilitar identificadores de LAN virtual y configurar direcciones IP estáticas
- Insertar nuevos paquetes de mantenimiento sobre la marcha
- Agregar un script setupcomplete.cmd u otros scripts de cliente para ejecutarlos tras el procesamiento de unattend.xml
- Habilitar Servicios de administración de emergencia (EMS) para el acceso a la consola de puerto serie
- Habilitar servicios de desarrollo para permitir probar controladores firmados y aplicaciones sin firmar, el shell predeterminado de PowerShell
- Habilitar depuración a través de protocolos de serie, USB, TCP/IP o IEEE 1394
- Crear un medio USB con WinPE que particionará el servidor e instalará la imagen Nano
- Crear un medio USB con WinPE que detectará la configuración de hardware de Nano Server existente e informará de todos los detalles en un registro y en pantalla. Esto incluye los adaptadores de red, las direcciones MAC y el tipo de firmware (BIOS o UEFI). El proceso de detección también mostrará todos los volúmenes del sistema y los dispositivos que no tienen un controlador incluido en el paquete de controladores de Server Core.

Si no está familiarizado con alguno de estos puntos, revise el resto de este tema y los otros temas de Nano Server para estar preparado para ofrecer a la herramienta la información que necesitará.

## <a name="BKMK_CreateImage"></a>Creación de una imagen de Nano Server personalizada  
Para Windows Server 2016, Nano Server se distribuye en los medios físicos, donde encontrará una carpeta **NanoServer**; contiene una imagen .wim y una subcarpeta denominada **Packages**. Estos archivos de paquete son los que se utilizan para agregar roles de servidor y características a la imagen VHD, en la que luego arranca.  

También puedes buscar e instalar estos paquetes con el proveedor NanoServerPackage del módulo de PowerShell PackageManagement (OneGet). Vea la sección "Instalación de roles y características en línea" de este tema.  

Esta tabla muestra los roles y las características que están disponibles en esta versión de Nano Server, junto con las opciones de Windows PowerShell que instalarán los paquetes para ellos. Algunos de los paquetes se instalan directamente con sus propios modificadores de Windows PowerShell (por ejemplo, -Compute); otros se instalan pasando los nombres del paquete al parámetro -Package, que se pueden combinar en una lista separada por comas. Puede enumerar de forma dinámica los paquetes disponibles mediante el cmdlet Get-NanoServerPackage.  


|                                                                             Rol o característica                                                                             |                                                                                                                                                                                                          Opción                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                     Rol de Hyper-V (incluido NetQoS)                                                                     |                                                                                                                                                                                                         -Compute                                                                                                                                                                                                          |
|                                                   Clústeres de conmutación por error y otros componentes, detalladas después de esta tabla.                                                   |                                                                                                                                                                                                        -Clustering                                                                                                                                                                                                        |
| Controladores básicos para una variedad de adaptadores de red y controladores de almacenamiento. Este es el mismo conjunto de controladores incluidos en una instalación Server Core de Windows Server 2016. |                                                                                                                                                                                                        -OEMDrivers                                                                                                                                                                                                        |
|                                                Podrás ver una información detallada sobre el rol de servidor de archivos y otros componentes de almacenamiento después de esta tabla.                                                 |                                                                                                                                                                                                         -Storage                                                                                                                                                                                                          |
|                                                          Windows Defender, incluido un archivo de firma predeterminado                                                           |                                                                                                                                                                                                         -Defender                                                                                                                                                                                                         |
|                         Invertir reenviadores para compatibilidad de aplicaciones, por ejemplo marcos de aplicaciones comunes como Ruby, Node.js, etc.                         |                                                                                                                                                                                                  Se incluye de manera predeterminada                                                                                                                                                                                                  |
|                                                                             Rol Servidor DNS                                                                             |                                                                                                                                                                                         -Paquete Microsoft-NanoServer-DNS-Package                                                                                                                                                                                         |
|                                                              Configuración de estado deseado (DSC) de PowerShell                                                               |                                                                                                                               -Paquete Microsoft-NanoServer-DSC-Package<br />**Nota:** Para obtener todos los detalles, vea [Uso de DSC en Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).                                                                                                                               |
|                                                                    Internet Information Server (IIS)                                                                    |                                                                                                                                       -Package Microsoft-NanoServer-IIS-Package<br />**Nota:** Consulta [IIS en Nano Server](IIS-on-Nano-Server.md) para más información sobre cómo trabajar con IIS.                                                                                                                                        |
|                                                                   Compatibilidad de host para los contenedores de Windows                                                                   |                                                                                                                                                                                                        -Containers                                                                                                                                                                                                        |
|                                                               Agente System Center Virtual Machine Manager                                                               | -Package Microsoft-NanoServer-SCVMM-Package<br />-Package Microsoft-NanoServer-SCVMM-Compute-Package<br />**Nota:** Usa el paquete de proceso SCVMM solo si supervisas Hyper-V. Para implementaciones hiperconvergidas en VMM, conviene especificar también el parámetro -Storage. Para más información, consulta la [documentación sobre VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-compute-add-nano-hyper-v). |
|                                                                 Agente System Center Operations Manager                                                                  |                                                                                                                 Instalado por separado. Consulta la documentación de System Center Operations Manager para más detalles, en https://technet.microsoft.com/system-center-docs/om/manage/install-agent-on-nano-server.                                                                                                                 |
|                                                                 Protocolo de puente del centro de datos (incluido DCBQoS)                                                                 |                                                                                                                                                                                         -Paquete Microsoft-NanoServer-DCB-Package                                                                                                                                                                                         |
|                                                                     Implementación en una máquina virtual                                                                      |                                                                                                                                                                                        -Package Microsoft-NanoServer-Guest-Package                                                                                                                                                                                        |
|                                                                     Implementación en una máquina física                                                                     |                                                                                                                                                                                        - Paquete Microsoft-NanoServer-SCVMM-Package                                                                                                                                                                                        |
|     BitLocker, el módulo de plataforma segura (TPM), el cifrado de volumen, identificación de la plataforma, los proveedores de criptografía y otras funciones relacionados con inicio seguro.     |                                                                                                                                                                                    -Package Microsoft-NanoServer-SecureStartup-Package                                                                                                                                                                                    |
|                                                                    Compatibilidad de Hyper-V con máquinas virtuales blindadas                                                                     |                                                                                                                                         -Paquete Microsoft-NanoServer-ShieldedVM-Package<br />**Nota:** Este paquete solo está disponible para la edición Datacenter de Nano Server.                                                                                                                                         |
|                                                             Agente del Protocolo simple de administración de redes (SNMP)                                                             |                                   -Package Microsoft-NanoServer-SNMP-Agent-Package.cab<br />**Nota:** No se incluye con los medios de instalación de Windows Server 2016. Disponible solo en línea. Para más información, consulta [Instalación de roles y características en línea](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online).                                    |
|               Servicio de IPHelper que proporciona conectividad de túnel con tecnologías de transición IPv6 (6to4, ISATAP, Proxy de puerto y Teredo) e IP-HTTPS               |                                -Package Microsoft-NanoServer-IPHelper-Service-Package.cab<br />**Nota:** No se incluye con los medios de instalación de Windows Server 2016. Disponible solo en línea. Para más información, consulta [Instalación de roles y características en línea](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online).                                 |

> [!NOTE]  
> Al instalar paquetes con estas opciones, un paquete de idioma correspondiente también se instala en función de la configuración regional del medio de servidor seleccionada. Puede encontrar los paquetes de idioma disponibles y sus abreviaturas de configuración regional en el medio de instalación en subcarpetas con el nombre de la configuración regional de la imagen.  

> [!NOTE]  
> Cuando se usa el parámetro -Storage para instalar Servicios de archivo, estos no están habilitados realmente. Habilita esta característica desde un equipo remoto con el Administrador de servidores. 

### <a name="failover-clustering-items-installed-by-the--clustering-parameter"></a>Elementos de clústeres de conmutación por error instalados por el parámetro de clústeres

- Rol de clústeres de conmutación por error
- Clústeres de conmutación por error de VM
- Espacios de almacenamiento directo (S2D)
- Calidad de servicio de almacenamiento
- Replicación de volúmenes de clústeres
- Servicio Testigo SMB


### <a name="file-and-storage-items-installed-by-the--storage-parameter"></a>Los elementos de almacenamiento y archivos instalados por el parámetro -Storage.

- Rol de Servidor de archivos
- Desduplicación de datos
- E/S de múltiples rutas, incluido un controlador para el Módulo específico de dispositivo de Microsoft (MSDSM)
- ReFS (v1 y v2)
- Iniciador iSCSI (pero no el destino iSCSI)
- Réplica de almacenamiento
- Servicio de administración del almacenamiento con soporte técnico de SMI-S
- Servicio Testigo SMB
- Volúmenes dinámicos
- Proveedores de almacenamiento básicos de Windows (para la administración de almacenamiento de Windows)




### <a name="installing-a-nano-server-vhd"></a>Instalación de un VHD de Nano Server  
Este ejemplo crea una imagen de VHDX basada en GPT con el nombre de un equipo determinado e incluidos los controladores invitados de Hyper-V, a partir de los medios de instalación de Nano Server en un recurso compartido de red. En un símbolo de Windows PowerShell con privilegios elevados, empiece con este cmdlet:  

`Import-Module <Server media location>\NanoServer\NanoServerImageGenerator; New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\server_en-us -BasePath .\Base -TargetPath .\FirstStepsNano.vhdx -ComputerName FirstStepsNano`  

El cmdlet llevará a cabo todas estas tareas:  

1.  Seleccionar Standard como una edición básica  

2.  Solicitar la contraseña de administrador  

3.  Copiar los medios de instalación de \\\Path\To\Media\server_en-us en .\Base  

4.  Convertir la imagen WIM a un VHD. (La extensión de archivo del argumento de ruta de acceso de destino determina si crea un VHD basado en MBR para máquinas virtuales de generación 1 en comparación con un VHDX basado en GPT para máquinas virtuales de generación 2)  

5.  Copiar el VHD resultante en \FirstStepsNano.vhdx  

6.  Establecer la contraseña de administrador para la imagen tal como se especifica  

7.  Establecer el nombre del equipo de la imagen en FirstStepsNano  

8.  Instalar los controladores invitados de Hyper-V  

Todo esto tiene como resultado una imagen de .\FirstStepsNano.vhdx.  

El cmdlet genera un registro según se ejecuta y permite saber dónde se encuentra este registro cuando termine. La conversión de WIM a VHD realizada por el script complementario genera su propio registro en %TEMP%\Convert-WindowsImage\\\<GUID> (donde \<GUID> es un identificador único por cada sesión de conversión).  

Siempre que use la misma ruta de acceso base, puede omitir el parámetro de ruta de acceso de medios cada vez que ejecute este cmdlet, puesto que va a utilizar los archivos almacenados en caché desde la ruta de acceso base. Si no se especifica una ruta de acceso base, el cmdlet generará una predeterminada en la carpeta TEMP. Si desea utilizar medios de origen diferentes, pero la misma ruta de acceso base, debe especificar el parámetro de ruta de acceso de medios.  

>[!NOTE]  
>Ahora tiene la opción de especificar la edición de Nano Server para compilar la edición Standard o Datacenter. Use el parámetro -Edition para especificar las ediciones *Standard* o *Datacenter*.  

Si tiene una imagen existente, puede modificarla según sea necesario con el cmdlet *Edit-NanoServerImage*.  

Si no especifica un nombre de equipo, se generará un nombre aleatorio.  

### <a name="installing-a-nano-server-wim"></a>Instalación de un WIM de Nano Server  

1. Copie la carpeta *NanoServerImageGenerator* desde la carpeta \NanoServer de la imagen ISO de Windows Server 2016 en una carpeta local del equipo.  
2. Inicie Windows PowerShell como administrador, cambie el directorio a la carpeta donde ha colocado la carpeta NanoServerImageGenerator y, a continuación, importe el módulo con `Import-Module .\NanoServerImageGenerator -Verbose`.  

   >[!NOTE]  
   >Puede que tenga que ajustar la directiva de ejecución de Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` debería funcionar correctamente.  

Para crear una imagen de Nano Server para actuar como un host de Hyper-V, ejecute lo siguiente:  

`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.wim -ComputerName <computer name> -OEMDrivers -Compute -Clustering`  

Donde  
-   -MediaPath es la raíz de los soportes de DVD o la imagen ISO que contienen Windows Server 2016.  
-   -BasePath contendrá una copia de los archivos binarios de Nano Server, para que pueda usar New-NanoServerImage -BasePath sin tener que especificar -MediaPath en futuras ejecuciones.  
-   -TargetPath contendrá el archivo .wim resultante que contiene los roles y las características que ha seleccionado. Asegúrese de especificar la extensión .wim.  
-   -Compute agrega el rol de Hyper-V.  
-   -OemDrivers agrega un número de controladores comunes.  

Se le pedirá que proporcione una contraseña de administrador.  

Para más información, vea `Get-Help New-NanoServerImage -Full`.  

Arranque en WinPE y asegúrese que se puede acceder al archivo .wim que acaba de crear desde WinPE. (Podría, por ejemplo, copiar el archivo .wim a una imagen de WinPE de arranque en una unidad flash USB).  

Una vez que se inicia WinPE, utilice Diskpart.exe para preparar la unidad de disco duro del equipo de destino. Ejecute los siguientes comandos de Diskpart (modificar en consecuencia, si no utiliza UEFI y GPT):  

> [!WARNING]  
> Estos comandos eliminarán todos los datos del disco duro.  

**Diskpart.exe Select disk 0 Clean Convert GPT Create partition efi size=100 Format quick FS=FAT32 label="System" Assign letter="s" Create partition msr size=128 Create partition primary Format quick FS=NTFS label="NanoServer" Assign letter="n" List volume Exit**  

Aplique la imagen de Nano Server (ajuste la ruta de acceso del archivo .wim):  

**Dism.exe /apply-image /imagefile:.\NanoServer.wim /index:1 /applydir:n:\ Bcdboot.exe n:\Windows /s s:**  

Extrae el soporte de DVD o la unidad USB y reinicia el sistema con **Wpeutil.exe Reboot**  


### <a name="editing-files-on-nano-server-locally-and-remotely"></a>Edición local y remota de archivos en Nano Server  
 En cualquier caso, conéctese al servidor Nano Server, como con la comunicación remota de Windows PowerShell.  

 Una vez que se conecta a Nano Server, puede editar un archivo que reside en el equipo local, pasando la ruta de acceso absoluta o relativa del archivo al comando psEdit, por ejemplo:   
`psEdit C:\Windows\Logs\DISM\dism.log` o `psEdit .\myScript.ps1`  

Edite un archivo que reside en un servidor remoto de Nano Server iniciando una sesión remota con `Enter-PSSession -ComputerName "192.168.0.100" -Credential ~\Administrator` y luego pase la ruta absoluta o relativa del archivo al comando psEdit, de una forma similar a esta:   
`psEdit C:\Windows\Logs\DISM\dism.log`  

## <a name="BKMK_online"></a>Instalación de roles y características en línea  
> [!NOTE]
> Si instalas un paquete opcional del servidor Nano desde un repositorio en línea o multimedia, este no incluirá las recientes revisiones de seguridad. Para evitar errores de coincidencia entre los paquetes opcionales y el sistema operativo base, te recomendamos que instales la [última actualización acumulativa](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) inmediatamente después de instalar los paquetes opcionales y **antes** de reiniciar el servidor.

### <a name="installing-roles-and-features-from-a-package-repository"></a>Instalación de roles y características desde un repositorio de paquetes  
Puedes buscar e instalar paquetes de Nano Server desde el repositorio de paquetes en línea mediante el proveedor del NanoServerPackage del módulo de PackageManagement de PowerShell. Para instalar este proveedor, usa estos cmdlets:

```powershell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
```

>[!NOTE]
>Si se producen errores cuando ejecutes Install-PackageProvider, comprueba que tienes instalada la [última actualización acumulativa](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) ([KB3206632](https://support.microsoft.com/kb/3206632) o posterior) o usa Save-Module tal como se indica: 

```powershell
Save-Module -Path "$Env:ProgramFiles\WindowsPowerShell\Modules\" -Name NanoServerPackage -MinimumVersion 1.0.1.0
Import-PackageProvider NanoServerPackage
```

Una vez que el proveedor esté instalado e importado, puedes buscar, descargar e instalar los paquetes de Nano Server mediante los cmdlets diseñados específicamente para trabajar con paquetes Nano Server:

```powershell
Find-NanoServerPackage  
Save-NanoServerPackage  
Install-NanoServerPackage
```  

También puedes usar los cmdlets genéricos de PackageManagement y especificar el proveedor de NanoServerPackage:

```powershell
Find-Package -ProviderName NanoServerPackage
Save-Package -ProviderName NanoServerPackage
Install-Package -ProviderName NanoServerPackage
Get-Package -ProviderName NanoServerPackage
```

Para utilizar cualquiera de estos cmdlets con los paquetes de Nano Server en Nano Server, agrega `-ProviderName NanoServerPackage`. Si no agregas el parámetro -ProviderName, PackageManagement creará una iteración de todos los proveedores. Para más información sobre estos cmdlets, ejecuta `Get-Help <cmdlet>`. Estos son algunos ejemplos de uso común:

### <a name="searching-for-nano-server-packages"></a>Buscar paquetes de Nano Server  
Puedes utilizar `Find-NanoServerPackage` o `Find-Package -ProviderName NanoServerPackage` para buscar y devolver una lista de paquetes de Nano Server que están disponibles en el repositorio en línea. Por ejemplo, puedes obtener una lista de todos los paquetes más recientes:

```powershell
Find-NanoServerPackage
```

Ejecutando `Find-Package -ProviderName NanoServerPackage -DisplayCulture` se muestran todas las referencias culturales disponibles.

Si necesitas una versión de configuración regional específica, como Inglés (Estados Unidos), podrías utilizar `Find-NanoServerPackage -Culture en-us` o  
`Find-Package -ProviderName NanoServerPackage -Culture en-us` o `Find-Package -Culture en-us -DisplayCulture`.

Para buscar un paquete específico por nombre de paquete, usa el parámetro -Name. Este parámetro también acepta caracteres comodín. Por ejemplo, para buscar todos los paquetes con VMM en el nombre, usa `Find-NanoServerPackage -Name *VMM*` o `Find-Package -ProviderName NanoServerPackage -Name *VMM*`.

Puedes encontrar una versión determinada con los parámetros -RequiredVersion, -MinimumVersion, o -MaximumVersion. Para buscar todas las versiones disponibles, usa -AllVersions. De lo contrario, se devuelve solo la última versión. Por ejemplo: `Find-NanoServerPackage -Name *VMM* -RequiredVersion 10.0.14393.0`. O para todas las versiones: `Find-Package -ProviderName NanoServerPackage -Name *VMM* -AllVersions`

### <a name="installing-nano-server-packages"></a>Instalar paquetes de Nano Server  
Puedes instalar un paquete de Nano Server (incluidos sus paquetes de dependencia, si procede) en Nano Server, ya sea localmente o en una imagen sin conexión con `Install-NanoServerPackage` o `Install-Package -ProviderName NanoServerPackage`. Ambas aceptan la entrada de la canalización.

Para instalar la última versión de un paquete de Nano Server en un servidor en línea de Nano Server, usa `Install-NanoServerPackage -Name Microsoft-NanoServer-Containers-Package` o `Install-Package -Name Microsoft-NanoServer-Containers-Package`. PackageManagement utilizará la referencia cultural de Nano Server.

Puedes instalar un paquete de Nano Server en una imagen sin conexión, especificando una versión concreta y la referencia cultural, de forma similar a esta:

`Install-NanoServerPackage -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

o:

`Install-Package -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

Estos son algunos ejemplos de canalización de resultados de búsqueda de paquetes para el cmdlet de instalación:  

`Find-NanoServerPackage *dcb* | Install-NanoServerPackage` busca todos los paquetes con "dcb" en el nombre y luego los instala.

`Find-Package *nanoserver-compute-* | Install-Package` busca paquetes con "nanoserver-compute-" en el nombre y los instala.

`Find-NanoServerPackage -Name *nanoserver-compute* | Install-NanoServerPackage -ToVhd C:\MyNanoVhd.vhd` busca paquetes con "compute" en el nombre y los instala en una imagen sin conexión.

`Find-Package -ProviderName NanoserverPackage *nanoserver-compute-* | Install-Package -ToVhd C:\MyNanoVhd.vhd` hace lo mismo con cualquier paquete que tenga "nanoserver-compute-" en el nombre.

### <a name="downloading-nano-server-packages"></a>Descargar paquetes de Nano Server  

`Save-NanoServerPackage` o `Save-Package` te permite descargar paquetes y guardarlos sin instalarlos. Ambos cmdlets aceptan la entrada de la canalización.

Por ejemplo, para descargar y guardar un paquete de Nano Server en un directorio que coincida con la ruta de acceso comodín, usa `Save-NanoServerPackage -Name Microsoft-NanoServer-DNS-Package -Path C:\` En este ejemplo, -Culture no se ha especificado, así que usará la referencia cultural del equipo local. No se ha especificado ninguna versión, por lo que se guardará la última.

`Save-Package -ProviderName NanoServerPackage -Name Microsoft-NanoServer-IIS-Package -Path C:\ -Culture it-IT -MinimumVersion 10.0.14393.0` guarda una versión determinada y para el idioma y la configuración regional Italiano.

Puedes enviar los resultados de la búsqueda a través de la canalización como se muestra en estos ejemplos:

`Find-NanoServerPackage -Name *containers* -MaximumVersion 10.2 -MinimumVersion 1.0 -Culture es-ES | Save-NanoServerPackage -Path C:\`

o bien

`Find-Package -ProviderName NanoServerPackage -Name *shield* -Culture es-ES | Save-Package -Path`

### <a name="inventory-installed-packages"></a>Inventario de paquetes instalados
Puedes detectar qué paquetes de Nano Server están instalados con `Get-Package`. Por ejemplo, ve qué paquetes están en Nano Server con `Get-Package -ProviderName NanoserverPackage`.

Para comprobar los paquetes de Nano Server que estén instalados en una imagen sin conexión, ejecuta `Get-Package -ProviderName NanoserverPackage -FromVhd C:\MyNanoVhd.vhd`.


### <a name="installing-roles-and-features-from-local-source"></a>Instalación de roles y características desde un origen local  
Aunque se recomienda la instalación sin conexión de los roles de servidor y de otros paquetes, debe realizar la instalación en línea (con Nano Server activado) en escenarios de contenedor. Para ello, realice los pasos siguientes:  

1.  Copie la carpeta Packages desde el medio de instalación localmente al servidor Nano Server activado (por ejemplo, en C:\packages).  

2.  Cree un nuevo archivo Unattend.xml en otro equipo y luego cópielo en Nano Server. Puede copiar y pegar este contenido XML en el archivo XML que ha creado (en este ejemplo se muestra la instalación del paquete de IIS):  



```  
<?xml version="1.0" encoding="utf-8"?>
    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  




3. En el archivo XML nuevo creado (o copiado), edite C:\packages en el directorio en que ha copiado el contenido de Packages.  

4. Cambie al directorio con el archivo XML recién creado y ejecute  

   **dism /online /apply-unattend:.\unattend.xml**  



5. Confirme que el paquete y su paquete de idioma asociado está instalado correctamente; para ello, ejecute:  

   **dism /online /get-packages**  

   Deberías ver que "Package Identity : Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~en-US~10.0.10586.0" aparezca dos veces, una para Release Type : Language Pack y una para Release Type : Feature Pack.  

## <a name="customizing-an-existing-nano-server-vhd"></a>Personalización de un VHD existente de Nano Server  
Puede cambiar los detalles de un VHD existente mediante el cmdlet Edit-NanoServerImage, como en este ejemplo:  

`Edit-NanoServerImage   -BasePath .\Base   -TargetPath .\BYOVHD.vhd`  

Este cmdlet hace lo mismo que NanoServerImage, pero cambia la imagen existente en lugar de convertir un archivo WIM a un VHD. Admite los mismos parámetros que New-NanoServerImage con la excepción de -MediaPath y -MaxSize, por lo que el VHD inicial se debe haber creado con esos parámetros antes de poder hacer cambios con Edit-NanoServerImage.  

## <a name="additional-tasks-you-can-accomplish-with-new-nanoserverimage-and-edit-nanoserverimage"></a>Tareas adicionales que puede realizar con New-NanoServerImage y Edit-NanoServerImage  

### <a name="joining-domains"></a>Unión de dominios  
New-NanoServerImage ofrece dos métodos para unirse a un dominio; ambos se basan en el aprovisionamiento de dominios sin conexión, pero uno utiliza un blob para llevar a cabo la unión. En este ejemplo, el cmdlet utiliza un blob de dominio para el dominio Contoso desde el equipo local (que por supuesto debe ser parte del dominio Contoso); a continuación, realiza el aprovisionamiento sin conexión de la imagen con el blob:  

`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomHarvest.vhdx -ComputerName JoinDomHarvest -DomainName Contoso`  

Cuando se completa este cmdlet, debe buscar un equipo denominado "JoinDomHarvest" en la lista de equipos de Active Directory.  

También puede usar este cmdlet en un equipo que no está unido a un dominio. Para ello, utilice un blob de cualquier equipo que se ha unido al dominio y después proporcione el blob al cmdlet. Tenga en cuenta que, al utilizar el blob de otro equipo, el blob ya incluye el nombre de ese equipo; por tanto, si trata de agregar el parámetro *-ComputerName*, se producirá un error.  

Puede utilizar el blob con este comando:  

**djoin**  
 **/Provision**  
 **/Domain Contoso**  
 **/Machine JoiningDomainsNoHarvest**  
 **/SaveFile JoiningDomainsNoHarvest.djoin**  

Ejecute New-NanoServerImage con el blob utilizado:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomNoHrvest.vhd -DomainBlobPath .\Path\To\Domain\Blob\JoinDomNoHrvestContoso.djoin`  

En caso de que ya tenga un nodo en el dominio con el mismo nombre de equipo que el futuro servidor de Nano Server, podría reutilizar el nombre de equipo agregando el parámetro `-ReuseDomainNode`.  

### <a name="adding-additional-drivers"></a>Incorporación de controladores adicionales
Nano Server ofrece un paquete que incluye un conjunto de controladores básicos para una variedad de adaptadores de red y controladores de almacenamiento; es posible que no se incluyan los controladores de sus adaptadores de red. Puede utilizar estos pasos para buscar controladores en un sistema de trabajo, extraerlos y agregarlos a la imagen de Nano Server.

1. Instale Windows Server 2016 en el equipo físico donde se ejecutará Nano Server.
2. Abra el Administrador de dispositivos e identifique los dispositivos en las siguientes categorías:
3. Adaptadores de red
4. Controladores de almacenamiento
5. Unidades de disco
6. Para cada dispositivo de estas categorías, haga clic con el botón derecho en el nombre del dispositivo y haga clic en **Propiedades**. En el cuadro de diálogo que se abre, haga clic en la pestaña **Controlador** y luego haga clic en **Detalles del controlador**.
7. Anote el nombre de archivo y ruta de acceso del archivo del controlador que aparece. Por ejemplo, supongamos que el archivo del controlador es e1i63x64.sys, que se encuentra en C:\Windows\System32\Drivers.
8. En un símbolo del sistema, busque el archivo de controlador y busque todas las instancias con dir e1i*.sys /s /b. En este ejemplo, el archivo de controlador también está presente en la ruta de acceso C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd\e1i63x64.sys.
9. En un símbolo del sistema con privilegios elevados, navega hasta el directorio donde está el VHD de Nano Server y ejecuta los siguientes comandos: **md mountdir**

    **dism\dism /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**

    **dism\dism /Add-Driver /image:.\mountdir /driver: C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd**

    **dism\dism /Unmount-Image /MountDir:.\MountDir /Commit**
10. Repita estos pasos para cada archivo de controlador que necesita.

> [!NOTE]  
> En la carpeta donde se guardan los controladores, los archivos SYS y los archivos INF correspondientes deben estar presentes. Además, Nano Server solo admite controladores firmados de 64 bits. 

### <a name="injecting-drivers"></a>Inserción de controladores  
Nano Server ofrece un paquete que incluye un conjunto de controladores básicos para una variedad de adaptadores de red y controladores de almacenamiento; es posible que no se incluyan los controladores de sus adaptadores de red. Puede utilizar esta sintaxis para que New-NanoServerImage busque los controladores disponibles en el directorio y los inserte en la imagen de Nano Server:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers`  

> [!NOTE]
> En la carpeta donde se guardan los controladores, los archivos SYS y los archivos INF correspondientes deben estar presentes. Además, Nano Server solo admite controladores firmados de 64 bits.

Con el parámetro -DriverPath, también puedes pasar una matriz de rutas de acceso a archivos .inf de controlador:

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers\netcard64.inf`

### <a name="connecting-with-winrm"></a>Conexión con WinRM  
Para poder conectarse a un equipo de Nano Server mediante Administración remota de Windows (WinRM) (desde otro equipo que no está en la misma subred), abra el puerto 5985 para el tráfico TCP entrante en la imagen de Nano Server. Use este cmdlet:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\ConnectingOverWinRM.vhd -EnableRemoteManagementPort`  

### <a name="setting-static-ip-addresses"></a>Configuración de direcciones IP estáticas  
Para configurar una imagen de Nano Server para usar direcciones IP estáticas, primero busque el nombre o índice de la interfaz que se va a modificar mediante Get-NetAdapter, netsh o la Consola de recuperación de Nano Server. Use los parámetros -Ipv6Address, -Ipv6Dns, -Ipv4Address, -Ipv4SubnetMask, -Ipv4Gateway y -Ipv4Dns para especificar la configuración, como en este ejemplo:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\StaticIpv4.vhd -InterfaceNameOrIndex Ethernet -Ipv4Address 192.168.1.2 -Ipv4SubnetMask 255.255.255.0 -Ipv4Gateway 192.168.1.1 -Ipv4Dns 192.168.1.1`  

### <a name="custom-image-size"></a>Personalización del tamaño de la imagen  
Puede configurar la imagen de Nano Server para que sea un VHD o VHDX de expansión dinámica con el parámetro - MaxSize, como en este ejemplo:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -MaxSize 100GB`  

### <a name="embedding-custom-data"></a>Inserción de datos personalizados  
Para insertar scripts o archivos binarios propios en la imagen de Nano Server, utilice el parámetro -CopyPath para pasar una matriz de archivos y directorios para copiarlos. El parámetro -CopyPath también puedes aceptar una tabla hash para especificar la ruta de acceso de destino de archivos y directorios.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -CopyPath .\tools`

### <a name="running-custom-commands-after-the-first-boot"></a>Ejecución de comandos personalizados después del primer arranque
Para ejecutar comandos personalizados como parte de setupcomplete.cmd, use el parámetro -SetupCompleteCommand para pasar una matriz de comandos:

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -SetupCompleteCommand @("echo foo", "echo bar")`


### <a name="running-custom-powershell-scripts-as-part-of-image-creation"></a>Ejecutar los scripts personalizados de PowerShell como parte de la creación de imágenes
Para ejecutar scripts personalizados de PowerShell como parte del proceso de creación de imagen, usa el parámetro -OfflineScriptPath para pasar una matriz de rutas de acceso a scripts. ps1. Si estos scripts toman argumentos, usa el parámetro -OfflineScriptArgument para pasar de una tabla hash de argumentos adicionales a los scripts.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -OfflineScriptPath C:\MyScripts\custom.ps1 -OfflineScriptArgument @{Param1="Value1"; Param2="Value2"}`


### <a name="support-for-development-scenarios"></a>Compatibilidad con escenarios de desarrollo
Si desea desarrollar y probar en Nano Server, puede usar el parámetro -Development. Esto habilitará a PowerShell como el shell local predeterminado, habilitará la instalación de controladores no firmados, copiará los archivos binarios de depurador, abrirá un puerto para la depuración, habilitará la firma de prueba y habilitará la instalación de paquetes AppX sin una licencia de desarrollador.

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`


### <a name="custom-unattend-file"></a>Personalización de archivo de instalación desatendida  
Si desea utilizar su propio archivo de instalación desatendida, use el parámetro -UnattendPath:  

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -UnattendPath \\path\to\unattend.xml`  

Especificar un nombre de equipo o la contraseña de administrador en este archivo de instalación desatendida invalidará los valores establecidos por -AdministratorPassword y -ComputerName. 

> [!NOTE]
> Nano Server no admite configurar las opciones de TCP/IP a través de los archivos de instalación desatendida. Puedes usar Setupcomplete.cmd para configurar las opciones de TCP/IP.

### <a name="collecting-log-files"></a>Recopilar archivos de registro
Si quieres recopilar los archivos de registro durante la creación de imágenes, usa el parámetro -LogPath para especificar un directorio donde se copiarán todos los archivos de registro.

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -LogPath C:\Logs`


> [!NOTE]
> Algunos parámetros en New-NanoServerImage y Edit-NanoServerImage son solo para uso interno y se pueden omitir con toda tranquilidad. Estos incluyen los parámetros -SetupUI e -Internal.


## <a name="installing-apps-and-drivers"></a>Instalación de aplicaciones y controladores
[comment]: # (De Xumin Sun, error n.º 68620.)  

### <a name="windows-server-app-installer"></a>Instalador de la aplicación de Windows Server
El instalador de la aplicación de Windows Server (WSA) proporciona una opción de instalación confiable para Nano Server. Puesto que Windows Installer (MSI) no se admite en Nano Server, WSA también es la única tecnología de instalación disponible para productos que no son de Microsoft. WSA aprovecha la tecnología del paquete de la aplicación de Windows diseñada para instalar y mantener aplicaciones de forma segura y confiable, mediante un manifiesto declarativo. Extiende el instalador del paquete de la aplicación de Windows para admitir las extensiones específicas de Windows Server, con la única limitación de que WSA no admite la instalación de controladores.

Crear e instalar un paquete de WSA en Nano Server implica realizar los pasos del publicador y del consumidor del paquete.

El publicador de paquetes debe hacer lo siguiente:

1. Instalar el [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk), que incluye las herramientas necesarias para crear un paquete de WSA: MakeAppx, MakeCert, Pvk2Pfx, SignTool.
2. Declarar un manifiesto: Sigue el [esquema de extensión del manifiesto de WSA](https://msdn.microsoft.com/library/windows/apps/mt670653.aspx) para crear el archivo de manifiesto, AppxManifest.xml.
3. Utilice la herramienta **MakeAppx** para crear un paquete de WSA.
4. Utilice las herramientas **MakeCert** y **Pvk2Pfx** para crear el certificado y después utilice **Signtool** para firmar el paquete.

A continuación, el consumidor del paquete debe seguir estos pasos:

1. Ejecuta el cmdlet de PowerShell [*Import-Certificate*](https://technet.microsoft.com/library/hh848630) para importar el certificado del publicador del paso 4 anterior a Nano Server con certStoreLocation en “Cert:\LocalMachine\TrustedPeople”. Por ejemplo: `Import-Certificate -FilePath ".\xyz.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedPeople"`
2. Instalar la aplicación en Nano Server ejecutando el cmdlet [**Add-AppxPackage**](https://technet.microsoft.com/library/mt575516(v=wps.620).aspx) de PowerShell para instalar un paquete de WSA en Nano Server. Por ejemplo: `Add-AppxPackage wsaSample.appx`

#### <a name="additional-resources-for-creating-apps"></a>Recursos adicionales para crear aplicaciones
WSA es la extensión de servidor de la tecnología de paquete de la aplicación de Windows (aunque no se hospeda en Microsoft Store). Si desea publicar aplicaciones con WSA, estos temas le ayudarán a familiarizarse con la canalización de paquetes de la aplicación:

- [Cómo crear un manifiesto de paquete básico](https://msdn.microsoft.com/library/windows/desktop/br211475.aspx)
- [App packager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) (Empaquetador de aplicaciones [MakeAppx.exe])
- [How to create an app package signing certificate](https://msdn.microsoft.com/library/windows/desktop/jj835832(v=vs.85).aspx) (Cómo crear un certificado de firma del paquete de la aplicación)
- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)

### <a name="installing-drivers-on-nano-server"></a>Instalación de controladores en Nano Server
Puede instalar controladores de terceros en Nano Server mediante el uso de paquetes de controladores INF. Comprenden paquetes de controladores Plug and Play (PnP) y paquetes de controladores del filtro del sistema de archivos. Los controladores de filtro de red no se admiten actualmente en Nano Server.

Tanto los paquetes de controladores PnP y del filtro del sistema de archivos deben cumplir los requisitos de controlador universal y el proceso de instalación, así como las instrucciones del paquete de controladores general, como la firma. Se documentan en las siguientes ubicaciones:

- [Driver Signing](https://msdn.microsoft.com/windows/hardware/drivers/install/driver-signing) (Firma de controladores)
- [Using a Universal INF File](https://msdn.microsoft.com/windows/hardware/drivers/install/using-a-configurable-inf-file) (Uso de un archivo INF universal)

#### <a name="installing-driver-packages-offline"></a>Instalación de paquetes de controladores sin conexión

Se pueden instalar paquetes de controladores compatibles en Nano Server sin conexión a través de los cmdlets [DISM.exe](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/dism-driver-servicing-command-line-options-s14) o de [PowerShell de DISM](https://technet.microsoft.com/library/dn376497.aspx).

#### <a name="installing-driver-packages-online"></a>Instalación de paquetes de controladores en línea
Los paquetes de controladores PnP se pueden instalar en Nano Server en línea mediante [PnpUtil](https://msdn.microsoft.com/library/windows/hardware/ff550419(v=vs.85).aspx). La instalación de controladores en línea de paquetes de controladores que no son PnP no se admite actualmente en Nano Server.






--------------------------------------------------  


## <a name="BKMK_JoinDomain"></a>Unión de Nano Server a un dominio  

### <a name="to-add-nano-server-to-a-domain-online"></a>Para agregar Nano Server a un dominio en línea  

1.  Utilice un blob de datos desde un equipo en el dominio que ya ejecuta Windows Threshold Server con este comando:  

    `djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  

    Guarda el blob de datos en un archivo denominado "odjblob".  

2.  Copie el archivo "odjblob" en el equipo de Nano Server con estos comandos:  

    **net use z: \\\\\<dirección IP de Nano Server>\c$**  

    > [!NOTE]  
    > Si se produce un error del comando net use, probablemente necesita ajustar las reglas de Firewall de Windows. Para ello, primero abra un símbolo del sistema con privilegios elevados, inicie Windows PowerShell y conéctese al equipo de Nano Server mediante la Comunicación remota de Windows PowerShell con estos comandos:  
    >   
    > `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
    >   
    > `$ip = "<ip address of Nano Server>"`  
    >   
    > `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
    >   
    > Cuando se le solicite, proporcione la contraseña de administrador y luego ejecute este comando para establecer la regla de firewall:  
    >   
    > **netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes**  
    >   
    > Salga de Windows PowerShell con `Exit-PSSession` y luego vuelva a intentar el comando net use. Si se realiza correctamente, siga copiando el contenido del archivo "odjblob" en Nano Server.  

    **md z:\Temp**  

    **copy odjblob z:\Temp**  

3.  Compruebe el dominio al que desea unir Nano Server y asegúrese de que DNS está configurado. Además, compruebe que la resolución de nombre del dominio o un controlador de dominio funciona según lo previsto. Para ello, abra un símbolo del sistema con privilegios elevados, inicie Windows PowerShell y conéctese al equipo de Nano Server mediante la Comunicación remota de Windows PowerShell con estos comandos:  

    `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  

    `$ip = "<ip address of Nano Server>"`  

    `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  

    Cuando se le solicite, proporcione la contraseña de administrador. Nslookup no está disponible en Nano Server, así que puede comprobar la resolución de nombres con Resolve-DNSName.

4. Si la resolución de nombres se realiza correctamente, en la misma sesión de Windows PowerShell, ejecute este comando para unirse al dominio:  

    `djoin /requestodj /loadfile c:\Temp\odjblob /windowspath c:\windows /localos`  

5.  Reinicie el equipo de Nano Server y después salga de la sesión de Windows PowerShell:  

    `shutdown /r /t 5`  

    `Exit-PSSession`  

6.  Una vez Nano Server se ha unido a un dominio, agregue la cuenta de usuario de dominio al grupo Administradores en Nano Server.

7. Por seguridad, quita el servidor de Nano Server de la lista de hosts de confianza con este comando: `Set-Item WSMan:\localhost\client\TrustedHosts ""` 

**Método alternativo para unirse a un dominio en un solo paso**  

En primer lugar, utilice el blob de datos desde otro equipo que ejecuta Windows Threshold Server que ya está en el dominio con este comando:  

`djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  

Abra el archivo "odjblob" (quizás en el Bloc de notas), copie su contenido y, a continuación, pegue el contenido en la sección \<AccountData> del archivo Unattend.xml a continuación.  

Coloque este archivo Unattend.xml en la carpeta C:\NanoServer y después utilice los siguientes comandos para montar el VHD y aplicar la configuración de la sección `offlineServicing`:  

**dism\dism /Mount-ImagemediaFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**  

**dism\dismmedia:.\mountdir /Apply-Unattend:.\unattend.xml**  

Cree una carpeta "Panther" (utilizada por los sistemas Windows para almacenar archivos durante la instalación; vea [Ubicaciones de archivos de registro de instalación Windows 7, Windows Server 2008 R2 y Windows Vista](https://support.microsoft.com/en-us/kb/927521) si siente curiosidad), copie el archivo Unattend.xml ahí y después desmonte el VHD con estos comandos:  

**md .\mountdir\windows\panther**  

**copy .\unattend.xml .\mountdir\windows\panther**  

**dism\dism /Unmount-Image /MountDir:.\mountdir /Commit**  

La primera vez que arranque Nano Server desde este VHD, se aplicará la otra configuración.  

Una vez Nano Server se ha unido a un dominio, agregue la cuenta de usuario de dominio al grupo Administradores en Nano Server.  

## <a name="working-with-server-roles-on-nano-server"></a>Trabajo con roles de servidor en Nano Server

### <a name="using-hyper-v-on-nano-server"></a>Uso de Hyper-V en Nano Server  
Hyper-V funciona igual en Nano Server que en Windows Server en modo Server Core, con dos excepciones:  

-   Debe realizar toda la administración de forma remota, y el equipo de administración debe ejecutar la misma versión de Windows Server que Nano Server. Las versiones anteriores de los cmdlets de Windows PowerShell de Hyper-V o del Administrador de Hyper-V no funcionarán.  

-   RemoteFX no está disponible.  

En esta versión, estas características de Hyper-V se han comprobado:  

-   Habilitar Hyper-V  

-   Creación de máquinas virtuales de generación 1 y de generación 2  

-   Creación de conmutadores virtuales  

-   Inicio de máquinas virtuales y ejecución de sistemas operativos invitados de Windows  
- Réplica de Hyper-V  



Si desea realizar una migración en vivo de máquinas virtuales, crear una máquina virtual en un recurso compartido SMB o conectar recursos de un recurso compartido SMB existente a una máquina virtual existente, es fundamental configurar correctamente la autenticación. Tiene dos opciones para hacerlo:  

**Delegación restringida**  

La delegación restringida funciona exactamente igual que en versiones anteriores. Consulte estos artículos para obtener más información:  

-   [Enabling Hyper-V Remote Management - Configuring Constrained Delegation For SMB and Highly Available SMB](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-smb-and-highly-available-smb.aspx) (Habilitar la administración remota de Hyper-V: configuración de la delegación restringida para SMB y SMB de alta disponibilidad)  

-   [Enabling Hyper-V Remote Management - Configuring Constrained Delegation For Non-Clustered Live Migration](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-non-clustered-live-migration.aspx) (Habilitar la administración remota de Hyper-V: configuración de la delegación restringida para migración en vivo no agrupada)  

**CredSSP**  

En primer lugar, consulte la sección "Uso de la comunicación remota a Windows PowerShell" de este tema para habilitar y probar CredSSP. A continuación, en el equipo de administración, puede usar el Administrador de Hyper-V y seleccionar la opción "Conectar como otro usuario". El Administrador de Hyper-V utilizará CredSSP. Debe hacerlo incluso si se usa la cuenta actual.  

Los cmdlets de Windows PowerShell para Hyper-V pueden usar parámetros CimSession o Credential, porque ambos funcionan con CredSSP.  

### <a name="BKMK_Failover"></a>Uso de clústeres de conmutación por error en Nano Server  
Los clústeres de conmutación por error funcionan igual en Nano Server que en Windows Server en modo Server Core, pero tenga presente estas advertencias:  

-   Los clústeres deben administrarse de manera remota con el Administrador de clústeres de conmutación por error o con Windows PowerShell.  

-   Todos los nodos del clúster de Nano Server deben estar unidos al mismo dominio, de manera similar a los nodos de clúster de Windows Server.  

-   La cuenta de dominio debe tener privilegios de administrador en todos los nodos de Nano Server, al igual que con los nodos de clúster de Windows Server.  

-   Todos los comandos deben ejecutarse en un símbolo del sistema con privilegios elevados.  

> [!NOTE]  
> Además, algunas características no se admiten en esta versión:  
>   
> -   No puede ejecutar cmdlets de clústeres de conmutación por error en un servidor local de Nano Server mediante Windows PowerShell local.  
> -   Agrupación de clústeres que no son de Hyper-V ni del servidor de archivos.  

Estos cmdlets de Windows PowerShell le resultarán útiles para administrar clústeres de conmutación por error:  

Puedes crear un nuevo clúster con `New-Cluster -Name <clustername> -Node <comma-separated cluster node list>`  

Una vez que haya establecido un nuevo clúster, debe ejecutar `Set-StorageSetting -NewDiskPolicy OfflineShared` en todos los nodos.  

Agrega un nodo adicional al clúster con `Add-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  

Elimina un nodo del clúster con `Remove-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`.  

Crea un servidor de archivos de escalabilidad horizontal con `Add-ClusterScaleoutFileServerRole -name <sofsname> -cluster <clustername>`.  

Puede encontrar otros cmdlets para clústeres de conmutación por error en [Microsoft.FailoverClusters.PowerShell](https://technet.microsoft.com/library/ee461009.aspx).  

### <a name="BKMK_DNS"></a>Uso del servidor DNS en Nano Server  
Para proporcionar a Nano Server el rol de servidor DNS, agregue Microsoft-NanoServer-DNS-Package a la imagen (vea la sección "Creación de una imagen de Nano Server personalizada" de este tema). Una vez que se ejecuta Nano Server, conéctese a dicho servidor y ejecute este comando desde una consola de Windows PowerShell con privilegios elevados para habilitar la característica:  

`Enable-WindowsOptionalFeature -Online -FeatureName DNS-Server-Full-Role`  

### <a name="BKMK_IIS"></a>Uso de IIS en Nano Server  
Para conocer los pasos para utilizar el rol de Internet Information Services (IIS), vea [IIS en Nano Server](IIS-on-Nano-Server.md). 

### <a name="using-mpio-on-nano-server"></a>Uso de E/S de múltiples rutas en Nano Server
Para saber el procedimiento para usar MPIO, vea [E/S de múltiples rutas en Nano Server](MPIO-on-Nano-Server.md) 

### <a name="BKMK_SSH"></a>Uso de SSH en Nano Server
Para obtener instrucciones sobre cómo instalar y utilizar SSH en Nano Server con el proyecto OpenSSH, vea la [wiki Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/wiki).

## <a name="appendix-sample-unattendxml-file-that-joins-nano-server-to-a-domain"></a>Apéndice: Archivo de ejemplo Unattend.xml que une Nano Server a un dominio  

> [!NOTE]  
> Asegúrese de eliminar el espacio final del contenido de "odjblob" una vez que lo pega en el archivo de instalación desatendida.  

```  
<?xml version='1.0' encoding='utf-8'?>  
<unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  

  <settings pass="offlineServicing">  
    <component name="Microsoft-Windows-UnattendedJoin" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
        <OfflineIdentification>                
           <Provisioning>    
             <AccountData> AAAAAAARUABLEABLEABAoAAAAAAAMABSUABLEABLEABAwAAAAAAAAABbMAAdYABc8ABYkABLAABbMAAEAAAAMAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABAsAAAAAAAQAAZoABNUABOYABZYAANQABMoAAOEAAMIAAOkAANoAAMAAAXwAAJAAAAYAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABLEAALMABLQABU0AATMABXAAAAAAAKdf/mhfXoAAUAAAQAAAAb8ABLQABbMABcMABb4ABc8ABAIAAAAAAb8ABLQABbMABcMABb4ABc8ABLQABb0ABZIAAGAAAAsAAR4ABTQABUAAAAAAACAAAQwABZMAAZcAAUgABVcAAegAARcABKkABVIAASwAAY4ABbcABW8ABQoAAT0ABN8AAO8ABekAAJMAAVkAAZUABckABXEABJUAAQ8AAJ4AAIsABZMABdoAAOsABIsABKkABQEABUEABIwABKoAAaAABXgABNwAAegAAAkAAAAABAMABLIABdIABc8ABY4AADAAAA4AAZ4ABbQABcAAAAAAACAAkKBW0ID8nJDWYAHnBAXE77j7BAEWEkl+lKB98XC2G0/9+Wd1DJQW4IYAkKBAADhAnKBWEwhiDAAAM2zzDCEAM6IAAAgAAAAAAAQAAAAAAAAAAAABwzzAAA  
</AccountData>  
           </Provisioning>    
         </OfflineIdentification>    
    </component>  
  </settings>  

  <settings pass="oobeSystem">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <UserAccounts>  
        <AdministratorPassword>  
           <Value>Tuva</Value>  
           <PlainText>true</PlainText>  
        </AdministratorPassword>  
      </UserAccounts>  
      <TimeZone>Pacific Standard Time</TimeZone>  
    </component>  
  </settings>  

  <settings pass="specialize">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <RegisteredOwner>My Team</RegisteredOwner>  
      <RegisteredOrganization>My Corporation</RegisteredOrganization>  
    </component>  
  </settings>  
</unattend>  
```  

