---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: Implementación y configuración de controladores de dominio virtualizados
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: be2c919e4379cf615fe25d68446855229ace87dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390699"
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Implementación y configuración de controladores de dominio virtualizados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En esta sección se tratan los siguientes temas:  
  
-   [Consideraciones de instalación](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Incluye los requisitos de la plataforma y otras limitaciones importantes.  
  
-   [Clonación de controladores de dominio virtualizados](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Explica en detalle todo el proceso de clonación de controladores de dominio virtualizados.  
  
-   [Restauración segura de controladores de dominio virtualizados](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Explica en detalle las validaciones que se llevan a cabo durante la restauración segura de controladores de dominio virtualizados.  
  
## <a name="BKMK_InstallConsiderations"></a>Consideraciones de instalación  
No existe una instalación de características o rol en especial para los controladores de dominio (DC) virtualizados; todos los controladores de dominio contienen automáticamente capacidades de clonación y restauración segura. No puedes eliminar o deshabilitar dichas capacidades.  
  
El uso de controladores de dominio de Windows Server 2012 requiere un esquema de AD DS de Windows Server 2012 versión 56 o posterior y un nivel funcional de bosque igual a Windows Server 2003 nativo o superior.  
  
Tanto los controladores de dominio de escritura como los de solo lectura son compatibles con todos los aspectos del DC virtualizado, igual que los catálogos globales y los roles FSMO.  
  
> [!IMPORTANT]  
> El contenedor de rol FSMO del emulador de PDC debe estar en línea cuando comience la clonación.  
  
### <a name="BKMK_PlatformReqs"></a>Requisitos de la plataforma  
La clonación de controladores de dominio virtualizados requiere lo siguiente:  
  
-   Un rol FSMO del emulador de PDC hospedado en un DC de Windows Server 2012  
  
-   Un emulador de PDC disponible durante las operaciones de clonación  
  
La clonación y la restauración segura requieren lo siguiente:  
  
-   Invitados virtuales de Windows Server 2012  
  
-   Compatibilidad de la plataforma de host de virtualización con el identificador de generación de VM (VMGID)  
  
Revisa la siguiente tabla sobre los productos de virtualización para comprobar si son compatibles con controladores de dominio virtualizados e identificadores de generación de VM.  
  
|||  
|-|-|  
|**Producto de virtualización**|**Admite controladores de dominio virtualizados y VMGID**|  
|**Microsoft Windows Server 2012 Server con la característica Hyper-V**|Sí|  
|**Servidor Hyper-V de Microsoft Windows Server 2012**|Sí|  
|**Microsoft Windows 8 con la característica de cliente de Hyper-V**|Sí|  
|**Windows Server 2008 R2 y Windows Server 2008**|No|  
|**Soluciones de virtualización que no son de Microsoft**|Ponte en contacto con el proveedor|  
  
Aunque Microsoft es compatible con Windows 7 Virtual PC, Virtual PC 2007, Virtual PC 2004 y Virtual Server 2005, estos no pueden ejecutar invitados de 64 bits ni son compatibles con identificadores de generación de VM.  
  
Para obtener ayuda sobre productos de virtualización de terceros y conocer el tipo de soporte técnico que ofrecen para los controladores de dominio virtualizados, ponte en contacto directamente con el proveedor.  
  
Para obtener más información, revise la directiva de soporte técnico para el [Software de Microsoft que se ejecuta en software de virtualización de hardware que no sea de Microsoft](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Advertencias importantes  
Los controladores de dominio virtualizados *no* son compatibles con la restauración segura de lo siguiente:  
  
-   Archivos VHD y VHDX copiados manualmente sobre archivos VHD existentes  
  
-   Archivos VHD y VHDX restaurados mediante software de copia de seguridad de archivos o de discos completos  
  
> [!NOTE]  
> Archivos VHDX nuevos en Windows Server 2012 Hyper-V.  
  
Ninguna de estas operaciones está contemplada por la semántica VM-GenerationID, por lo tanto, no cambian el identificador de generación de VM. La restauración de controladores de dominio mediante estos métodos podría provocar una reversión de USN, o bien poner en cuarentena el controlador de dominio o introducir objetos persistentes y la necesidad de realizar operaciones de limpieza profunda en el bosque.  
  
> [!WARNING]  
> La restauración segura de controladores de dominio virtualizados no sustituye a las copias de seguridad del estado del sistema y la Papelera de reciclaje de AD DS.  
>   
> Después de restaurar una instantánea, se perderán de forma permanente los deltas de los cambios previamente no replicados que se originaron en ese controlador de dominio tras la instantánea. La restauración segura implementa la restauración no autoritativa automatizada para evitar *solo* la cuarentena accidental de controladores de dominio.  
  
Para obtener más información sobre las burbujas de USN y los objetos persistentes, vea [Troubleshooting Active Directory operaciones que dan error 8606: "Se asignaron atributos insuficientes para crear un objeto" ](https://support.microsoft.com/kb/2028495).  
  
## <a name="BKMK_VDCCloning"></a>Clonación de controladores de dominio virtualizados  
Existen varias fases y pasos en la clonación de un controlador de dominio virtualizado, independientemente de si se utilizan herramientas gráficas o Windows PowerShell. En un nivel alto, las tres fases son las siguientes:  
  
**Preparar el entorno**  
  
-   Paso 1: Validar que el hipervisor sea compatible con el identificador de generación de VM y, por lo tanto, la clonación  
  
-   Paso 2: Compruebe que el rol de emulador de PDC esté hospedado en un controlador de dominio que ejecute Windows Server 2012 y que esté en línea y que el controlador de dominio clonado pueda acceder a él durante la clonación.  
  
**Preparar el controlador de dominio de origen**  
  
-   Paso 3: Autorizar el controlador de dominio de origen para la clonación  
  
-   Paso 4: Eliminar los servicios o programas incompatibles o agregarlos al archivo CustomDCCloneAllowList.xml.  
  
-   Paso 5: Crear DCCloneConfig.xml  
  
-   Paso 6: Desconectar el controlador de dominio de origen  
  
**Crear el controlador de dominio clonado**  
  
-   Paso 7: Copiar o exportar la VM de origen y agregar el XML si todavía no se ha copiado  
  
-   Paso 8: Crear una máquina virtual nueva desde la copia  
  
-   Paso 9: Iniciar la máquina virtual nueva para comenzar la clonación  
  
No existe ninguna diferencia en el procedimiento, independientemente de si se utilizan herramientas gráficas como la consola de administración de Hyper-V o herramientas de línea de comandos como Windows PowerShell, por lo que estos pasos se presentan una sola vez con ambas interfaces. En este tema se aportan ejemplos de Windows PowerShell para que explores la automatización de un extremo a otro del proceso de clonación; no son necesarios para ninguno de los pasos. En Windows Server 2012 no se incluye ninguna herramienta de administración gráfica para controladores de dominio virtualizados.  
  
En varios puntos de este procedimiento podrás elegir cómo crear el equipo clonado y cómo agregar los archivos xml; estos pasos se indican en los detalles que aparecen más abajo. Por lo demás, el proceso no puede alterarse.  
  
En el diagrama siguiente se ilustra el proceso de clonación de controladores de dominio virtualizados, cuando ya existe el dominio.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Paso 1 - Validar el hipervisor  
Consulta la documentación del proveedor para asegurarte de que el controlador de dominio de origen se esté ejecutando en un hipervisor compatible. Los controladores de dominio virtualizados son independientes del hipervisor y no necesitan Hyper-V.  
  
Si el hipervisor es Microsoft Hyper-V, asegúrese de que se está ejecutando en Windows Server 2012. Puedes verificarlo utilizando la Administración de dispositivos.  
  
Abre **Devmgmt.msc** y examina en **Dispositivos del sistema** los dispositivos y controladores Microsoft Hyper-V instalados. El dispositivo de sistema específico que se requiere para controladores de dominio virtualizados es el **Contador de generación de Microsoft Hyper-V** (controlador: vmgencounter.sys).  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Paso 2 - Comprobar el rol FSMO del PDCE  
Antes de intentar clonar un DC, debes asegurarte de que el controlador de dominio que hospeda el FSMO del emulador del controlador de dominio principal ejecute Windows Server 2012. El emulador del PDC (PDCE) es necesario por varias razones:  
  
1.  El PDCE crea el grupo especial **Controladores de dominio clonables** y establece su permiso en la raíz del dominio para permitir que un controlador de dominio se clone a sí mismo.  
  
2.  El controlador de dominio que se clona se pone en contacto directamente con el PDCE a través del protocolo RPC DRSUAPI, a fin de crear objetos de equipo para el DC que se clona.  
  
    > [!NOTE]  
    > Windows Server 2012 ha ampliado el protocolo remoto del Servicio de replicación de directorios (DRS) existente (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) para que incluya un método RPC nuevo **IDL_DRSAddCloneDC** (Opnum **28**). El método **IDL_DRSAddCloneDC** crea un objeto de controlador de dominio nuevo copiando atributos desde un objeto de controlador de dominio existente.  
    >   
    > Los estados de un controlador de dominio se componen de equipo, servidor, configuración NTDS, FRS, DFSR y objetos de conexión mantenidos para cada controlador de dominio. Al duplicar un objeto, este método RPC sustituye todas las referencias al controlador de dominio original por los objetos correspondientes del controlador de dominio nuevo. El llamador debe disponer del derecho de acceso de control DS-Clone-Domain-Controller en el contexto de nomenclatura de dominio.  
    >   
    > El uso de este método nuevo siempre requiere el acceso directo al controlador de dominio del emulador de PDC desde el llamador.  
    >   
    > Dado que este método RPC es nuevo, tu software de análisis de red necesita analizadores actualizados para que incluya campos para el nuevo Opnum 28 en el UUID existente E3514235-4B06-11D1-AB04-00C04FC2DCD2. De lo contrario, no podrás analizar este tráfico.  
    >   
    > Para obtener más información, consulte [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/library/hh554213(v=prot.13).aspx).  
  
***Esto también significa que cuando se usan redes no completamente enrutadas, la clonación de controladores de dominio virtualizados requiere segmentos de red con acceso al PDCE***. Se puede mover un controlador de dominio clonado a una red diferente tras la clonación (igual que un controlador de dominio físico), siempre y cuando actualices la información de sitio lógico de AD DS.  
  
> [!IMPORTANT]  
> Al clonar un dominio que contiene un solo controlador de dominio, debes asegurarte de que el DC de origen vuelva a estar en línea antes de comenzar las copias de clonación. Un domino de producción debería contener siempre por lo menos dos controladores de dominio.  
  
#### <a name="active-directory-users-and-computers-method"></a>Método de usuarios y equipos de Active Directory  
  
1.  Con el complemento Dsa.msc, haz clic con el botón secundario en el dominio y haz clic en **Maestros de operaciones**. Toma nota del controlador de dominio indicado en la pestaña PDC y cierra el cuadro de diálogo.  
  
2.  Haz clic con el botón secundario en el objeto de equipo de ese DC y haz clic en **Propiedades**; después, valida la información del sistema operativo.  
  
#### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes combinar los siguientes cmdlets de Active Directory del módulo de Windows PowerShell para regresar a la versión del emulador de PDC:  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Si no se proporciona el dominio, estos cmdlets dan por supuesto que se trata del dominio del equipo donde se ejecutan.  
  
El siguiente comando devuelve información del PDCE y del sistema operativo:  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
El ejemplo incluido a continuación muestra cómo especificar el nombre de dominio y cómo filtrar las propiedades devueltas antes de la canalización de Windows PowerShell:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Paso 3 - Autorizar un DC de origen  
El controlador de dominio de origen debe tener el derecho de acceso de control (CAR) **Permitir a un DC crear un clon de sí mismo** en el encabezado NC del dominio. De manera predeterminada, el grupo conocido **Controladores de dominio clonables** tiene este permiso y no contiene miembros. El PDCE crea este grupo cuando ese rol FSMO se transfiere a un controlador de dominio de Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Método del Centro de administración de Active Directory  
  
1.  Inicia Dsac.exe, navega al DC de origen y abre la página de detalles.  
  
2.  En la sección **Miembro de** , agrega el grupo **Controladores de dominio clonables** de ese dominio.  
  
#### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes combinar los siguientes cmdlets de Active Directory del módulo de Windows PowerShell **get-adcomputer** y **add-adgroupmember** para agregar un controlador de dominio al grupo **Controladores de dominio clonables**:  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Por ejemplo, esto agrega el servidor DC1 al grupo, sin necesidad de especificar el nombre distintivo del miembro del grupo:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>Volver a generar los permisos predeterminados  
Si eliminas este permiso del encabezado del dominio, la clonación genera un error. Puedes recrear el permiso a través del Centro de administración de Active Directory o de Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Método del Centro de administración de Active Directory  
  
1.  Abre el **Centro de administración de Active Directory**, haz clic con el botón secundario en el encabezado del dominio y haz clic en **Propiedades**; en la pestaña **Extensiones**, haz clic en **Seguridad** y en **Opciones avanzadas**. Haz clic en **Solo este objeto**.  
  
2.  Haz clic en **Agregar** y, en **Escriba el nombre del objeto que desea seleccionar**, escribe el nombre del grupo **Controladores de dominio clonables.**  
  
3.  En Permisos, haz clic en **Permitir a un DC crear un clon de sí mismo**y, después, haz clic en **Aceptar**.  
  
> [!NOTE]  
> También puedes eliminar el permiso predeterminado y agregar controladores de dominio individuales. Sin embargo, esto podría causar problemas continuos de mantenimiento en caso de que los administradores nuevos no sepan que se ha realizado esta personalización. La modificación de la configuración predeterminada no aumenta la seguridad y está desaconsejada.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Usa los siguientes comandos en un símbolo de sistema con permisos de administrador en la consola de Windows PowerShell. Estos comandos detectan el nombre de dominio y vuelven a agregar los permisos predeterminados:  
  
```  
import-module activedirectory  
cd ad:  
$domainNC = get-addomain  
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
$sid1 = (get-adgroup $dcgroup).sid  
$acl = get-acl $domainNC  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
cd c:  
```  
  
También puedes ejecutar el ejemplo [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) en una consola de Windows PowerShell, cuando la consola se inicia como un administrador con permisos elevados en un controlador de dominio en el dominio afectado. De manera automática, establece los permisos. El ejemplo está ubicado en el apéndice de este módulo.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Paso 4 - Eliminar las aplicaciones o servicios incompatibles (si no utilizan CustomDCCloneAllowList.xml)  
Todos los programas o servicios que previamente devolvió Get-ADDCCloningExcludedApplicationList ( *y no agregados a CustomDCCloneAllowList.xml* ) deben eliminarse antes de la clonación. El método recomendado consiste en desinstalar la aplicación o el servicio.  
  
> [!WARNING]  
> Todos los programas o servicios incompatibles que no se hayan desinstalado o que se hayan agregado a CustomDCCloneAllowList.xml impiden la clonación.  
  
Utiliza el cmdlet Get-AdComputerServiceAccount para localizar todas las Cuentas de servicio administradas (MSA) independientes en el dominio y para comprobar si el equipo utiliza alguna. Si hay alguna MSA instalada, utiliza el cmdlet Uninstall-ADServiceAccount para eliminar la cuenta de servicio instalada localmente. Cuando acabes de desconectar el controlador de dominio de origen en el paso 6, puedes volver a agregar la MSA mediante Install-ADServiceAccount cuando el servidor vuelva a estar en línea. Para obtener más información, consulte [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/hh852310).  
  
> [!IMPORTANT]  
> Las MSA independientes (lanzadas por primera vez en Windows Server 2008 R2) han sido sustituidas en Windows Server 2012 por las MSA de grupo. Las MSA de grupo son compatibles con la clonación.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Paso 5 - Crear DCCloneConfig.xml  
El archivo DcCloneConfig.xml es necesario para clonar controladores de dominio. Sus contenidos permiten especificar detalles únicos, como el nuevo nombre del equipo y la dirección IP.  
  
El archivo CustomDCCloneAllowList.xml es opcional, a menos que instales aplicaciones o servicios de Windows potencialmente incompatibles en el controlador de dominio de origen. Los archivos requieren una nomenclatura, un formateo y una colocación precisos; de lo contrario, la clonación generará un error.  
  
Por esta razón, debes utilizar siempre los cmdlets de Windows PowerShell para crear los archivos XML y colocarlos en la ubicación correcta.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Generar con New-ADDCCloneConfigFile  
El módulo de Active Directory de Windows PowerShell contiene un cmdlet nuevo en Windows Server 2012:  
  
```  
New-ADDCCloneConfigFile  
```  
  
Ejecuta el cmdlet en el controlador de dominio de origen propuesto que pretendes clonar. El cmdlet es compatible con varios argumentos y, cuando se utiliza, prueba siempre el equipo y el entorno en el que se ejecuta, a menos que especifiques el argumento -offline.  
  
||||  
|-|-|-|  
|**ActiveDirectory**<br /><br />**Cmdlet**|**Argumentos**|**Sobre**|  
|**New-ADDCCloneConfigFile**|*<no argument specified>*|Cree un archivo DcCloneConfig.xml vacío en el Directorio de trabajo de DSA (valor predeterminado: %systemroot%\ntds)|  
||-CloneComputerName|Especifica el nombre del equipo del DC que se clona. Tipo de datos de cadena.|  
||-Path|Especifica la carpeta para crear el archivo DcCloneConfig.xml. Si no se especifica, escriba en el Directorio de trabajo de DSA (valor predeterminado: %systemroot%\ntds). Tipo de datos de cadena.|  
||-SiteName|Especifica el nombre del sitio lógico de AD al que unirse durante la creación de la cuenta de equipo clonada. Tipo de datos de cadena.|  
||-IPv4Address|Especifica la dirección IPv4 estática del equipo clonado. Tipo de datos de cadena.|  
||-IPv4SubnetMask|Especifica la máscara de subred de IPv4 estática del equipo clonado. Tipo de datos de cadena.|  
||-IPv4DefaultGateway|Especifica la dirección de puerta de enlace predeterminada de IPv4 estática del equipo clonado. Tipo de datos de cadena.|  
||-IPv4DNSResolver|Especifica las entradas DNS de IPv4 estática del equipo clonado en una lista separada por comas. Tipo de datos de matriz. Pueden proporcionarse hasta cuatro entradas.|  
||-PreferredWINSServer|Especifica la dirección IPv4 estática del servidor WINS principal. Tipo de datos de cadena.|  
||-AlternateWINSServer|Especifica la dirección IPv4 estática del servidor WINS secundario. Tipo de datos de cadena.|  
||-IPv6DNSResolver|Especifica las entradas DNS de IPv6 estática del equipo clonado en una lista separada por comas. No hay manera de establecer información de IPv6 estática en la clonación de controladores de dominio virtualizados. Tipo de datos de matriz.|  
||-Offline|No realiza las pruebas de validación y sobrescribe cualquier archivo dccloneconfig.xml existente. No tiene parámetros.|  
||*-Estático*|Requerido si se especifican argumentos de IP estática IPv4SubnetMask, IPv4SubnetMask o IPv4DefaultGateway. No tiene parámetros.|  
  
Pruebas realizadas si se ejecuta en modo en línea:  
  
-   El emulador de PDC es Windows Server 2012 o posterior  
  
-   El controlador de dominio de origen es miembro del grupo Controladores de dominio clonables  
  
-   El controlador de dominio de origen no incluye aplicaciones o servicios excluidos  
  
-   El controlador de dominio de origen no contiene ya un archivo DcCloneConfig.xml en la ruta de acceso especificada  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Paso 6 - Desconectar el controlador de dominio de origen  
No puedes copiar un DC de origen que se esté ejecutando; debes cerrarlo correctamente. No clones un controlador de dominio detenido debido a un error de alimentación.  
  
#### <a name="graphical-method"></a>Método gráfico  
Usa el botón de apagado dentro del DC que se está ejecutando o el botón de apagado del Administrador de Hyper-V.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes apagar una máquina virtual utilizando uno de los siguientes cmdlets:  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer es un cmdlet que permite apagar equipos independientemente de la virtualización, y es análogo a la utilidad heredada Shutdown.exe. Stop-vm es un cmdlet nuevo en el módulo de Windows PowerShell de Hyper-V de Windows Server 2012, y es equivalente a las opciones de energía del Administrador de Hyper-V. Este último resulta útil en entornos de laboratorio, donde un controlador de dominio suele funcionar en una red virtualizada privada.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Paso 7 - Copiar discos  
En la fase de copia, es necesario tomar una decisión administrativa:  
  
-   Copiar los discos manualmente, sin Hyper-V  
  
-   Exportar la VM utilizando Hyper-V  
  
-   Exportar los discos combinados utilizando Hyper-V  
  
Deben copiarse todos los discos de la máquina virtual, no solo la unidad de sistema. Si el controlador de dominio de origen usa discos de diferenciación y planeas mover tu controlador de dominio clonado a otro host de Hyper-V, debes exportar.  
  
Se recomienda copiar los discos manualmente si el controlador de dominio de origen tiene *una sola* unidad. Se recomienda exportar/importar en el caso de las VM con *más de una* unidad u otras personalizaciones complejas de hardware virtualizado, como varios NIC.  
  
Si copias los archivos manualmente, elimina todas las instantáneas anteriores a la copia. Si exportas la VM, elimina las instantáneas anteriores a la exportación o elimínalas de la nueva VM tras la importación.  
  
> [!WARNING]  
> Las instantáneas son discos de diferenciación que pueden devolver un controlador de dominio a un estado previo. Si deseabas clonar un controlador de dominio y después restaurar su instantánea previa a la clonación, acabarías con controladores de dominio duplicados en el bosque. Las instantáneas previas no tienen ningún valor en un controlador de dominio recién clonado.  
  
#### <a name="manually-copying-disks"></a>Copiar discos manualmente  
  
##### <a name="hyper-v-manager-method"></a>Método de Administrador de Hyper-V  
Usa el complemento Administrador de Hyper-V para determinar qué discos están asociados al controlador de dominio de origen. Usa la opción Inspeccionar para validar si el controlador de dominio utiliza discos de diferenciación (para lo que también debes copiar el disco principal).  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Para eliminar las instantáneas, selecciona una VM y elimina el subárbol de instantáneas.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
Después ya puedes copiar manualmente los archivos VHD o VHDX mediante el Explorador de Windows, Xcopy.exe o Robocopy.exe. No hace falta realizar ningún paso especial. Es recomendable cambiar los nombres de los archivos, incluso aunque los muevas a otra carpeta.  
  
> [!NOTE]  
> Si estás copiando entre equipos host en una LAN (1 Gbit o más), la opción **Xcopy.exe /J** copia los archivos VHD/VHDX considerablemente más rápido que cualquier otra herramienta, pero utiliza mucho más ancho de banda.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Para determinar los discos que utilizan Windows PowerShell, usa los Módulos de Hyper-V:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
Por ejemplo, puedes devolver todos los discos duros IDE desde una VM llamada **DC2** con la siguiente muestra:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Si la ruta de acceso del disco señala a un archivo AVHD o AVHDX, es una instantánea. Para eliminar las instantáneas asociadas a un disco y combinar el VHD o VHDX real, usa cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por ejemplo, para eliminar todas las instantáneas de una VM llamada DC2-SOURCECLONE:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Para copiar los archivos con Windows PowerShell, utiliza el cmdlet siguiente:  
  
```  
Copy-Item  
```  
  
Combínalo con cmdlets de VM en canalización para facilitar la automatización. La canalización es un canal que se utiliza entre varios cmdlets para pasar datos. Por ejemplo, para copiar la unidad de un controlador de dominio de origen sin conexión llamado DC2-SOURCECLONE en un nuevo disco llamado c:\temp\copy.vhd sin necesidad de saber la ruta de acceso exacta a la unidad del sistema:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> No puedes utilizar discos de paso a través con la clonación, ya que no usan un archivo de disco virtual, sino un disco duro real.  
  
> [!NOTE]  
> Para obtener más información sobre las operaciones de Windows PowerShell con canalizaciones, consulta [Canalizar y la canalización de Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Exportar la VM  
Otra alternativa a copiar los discos consiste en exportar toda la VM de Hyper-V como copia. Al exportar automáticamente, se crea una carpeta con nombre para la VM que contiene la información de configuración y todos los discos.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Método de Administrador de Hyper-V  
Para exportar una VM con el Administrador de Hyper-V:  
  
1.  Haz clic con el botón secundario en el controlador de dominio de origen y haz clic en **Exportar**.  
  
2.  Selecciona una carpeta existente como contenedor para la exportación.  
  
3.  Espera a que la columna Estado deje de mostrar el mensaje **Exportando**.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Para exportar una VM con el módulo de Windows PowerShell de Hyper-V, usa el cmdlet:  
  
```  
Export-vm  
```  
  
Por ejemplo, para exportar una VM llamada DC2-SOURCECLONE a una carpeta llamada C:\VM:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Hyper-V de Windows Server 2012 es compatible con nuevas capacidades de exportación e importación que no se incluyen en esta explicación. Revisa TechNet para obtener más información.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Exportar discos combinados utilizando Hyper-V  
La última opción es usar las opciones de combinación y conversión de discos dentro de Hyper-V. Estas opciones te permiten copiar una estructura de disco existente (también cuando se incluyen archivos de instantánea AVHD/AVHDX) en un solo disco nuevo. Al igual que en el escenario de copia de disco manual, esto está destinado principalmente a máquinas virtuales más sencillas que solo utilizan una sola unidad, como C: \\. Su única ventaja es que, a diferencia de la copia manual, no te obliga a eliminar primero las instantáneas. Esta operación es por fuerza más lenta que simplemente eliminar las instantáneas y copiar los discos.  
  
##### <a name="hyper-v-manager-method"></a>Método de Administrador de Hyper-V  
Para crear un disco combinado utilizando el Administrador de Hyper-V:  
  
1.  Haga clic en **Editar disco**.  
  
2.  Busca el disco secundario más bajo. Por ejemplo, si usas un disco de diferenciación, el disco secundario es el elemento secundario más bajo. Si la máquina virtual tiene una instantánea (o varias), la instantánea seleccionada actualmente es el disco secundario más bajo.  
  
3.  Selecciona la opción **Combinar** para crear un solo disco a partir de toda la estructura principal-secundario.  
  
4.  Selecciona un nuevo disco duro virtual y escribe una ruta de acceso. De este modo, se reconcilian los archivos VHD/VHDX existentes en una sola unidad portátil nueva que no corre el riesgo de restaurar instantáneas anteriores.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Para crear un disco combinado desde un conjunto complejo de elementos principales utilizando el módulo de Windows PowerShell de Hyper-V, usa el cmdlet:  
  
```  
Convert-vm  
```  
  
Por ejemplo, para exportar toda la cadena de instantáneas de disco de una VM (esta vez sin incluir discos de diferenciación) y un disco principal en un solo disco nuevo llamado DC4-CLONED.VHDX:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>Agregar XML al disco del sistema sin conexión  
Si copiaste Dccloneconfig.xml en el DC de origen en ejecución, ahora debes copiar el archivo dccloneconfig.xml actualizado en el disco del sistema copiado/exportado sin conexión. En función de las aplicaciones instaladas que se detectaron antes con Get-ADDCCloningExcludedApplicationList, también podrías tener que copiar el archivo CustomDCCloneAllowList.xml en el disco.  
  
Las siguientes ubicaciones pueden contener el archivo DcCloneConfig.xml:  
  
1.  Directorio de trabajo de DSA  
  
2.  %windir%\NTDS  
  
3.  Medios extraíbles de lectura/escritura según el orden de la letra de unidad, en la raíz de la unidad  
  
Estas rutas de acceso no son configurables. Una vez comenzada la clonación, esta comprueba dichas ubicaciones en ese orden específico y utiliza el primer archivo DcCloneConfig.xml que encuentra, independientemente de los demás contenidos de la carpeta.  
  
Las siguientes ubicaciones pueden contener el archivo CustomDCCloneAllowList.xml:  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  Directorio de trabajo de DSA  
  
3.  %windir%\NTDS  
  
4.  Medios extraíbles de lectura/escritura según el orden de la letra de unidad, en la raíz de la unidad  
  
Puedes ejecutar New-ADDCCloneConfigFile con el argumento **-offline** (lo que también se conoce como modo sin conexión) para crear el archivo DcCloneConfig.xml y colocarlo en una ubicación correcta. Los siguientes ejemplos muestran cómo ejecutar New-ADDCCloneConfigFile en el modo sin conexión.  
  
Para crear un controlador de dominio clonado denominado CloneDC1 en el modo sin conexión, en un sitio denominado "REDMOND" con una dirección IPv4 estática, escriba:  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Para crear un controlador de dominio clonado denominado Clone2 en el modo sin conexión con una configuración de IPv4 estática e IPv6 estática, escriba:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Para crear un controlador de dominio clonado en el modo sin conexión con una configuración de IPv4 estática e IPv6 dinámica, y especificar varios servidores DNS en la configuración de resolución DNS, escriba:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Para crear un controlador de dominio clonado denominado Clone1 en el modo sin conexión con una configuración de IPv4 dinámica e IPv6 estática, escriba:  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Para crear un controlador de dominio clonado en el modo sin conexión con una configuración de IPv4 dinámica e IPv6 dinámica, escriba:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Método del Explorador de Windows  
Actualmente, Windows Server 2012 ofrece una opción gráfica para montar archivos VHD y VHDX. Esto requiere la instalación de la característica Experiencia de escritorio en Windows Server 2012.  
  
1.  Haz clic en el archivo VHD/VHDX recién copiado que contiene la unidad de sistema del DC de origen o la carpeta de ubicación del Directorio de trabajo de DSA y, después, haz clic en **Montar**, en el menú **Herramientas de imagen de disco**.  
  
2.  En la unidad ya montada, copia los archivos XML en una ubicación válida. Es posible que se te pida algún tipo de permiso para acceder a la carpeta.  
  
3.  Haz clic en la unidad montada y, después, haz clic en **Expulsar** en el menú **Herramientas de disco**.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
También puedes montar el disco sin conexión y copiar el archivo XML utilizando los cmdlets de Windows PowerShell:  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
Esto te permite controlar el proceso por completo. Por ejemplo, es posible montar la unidad con una letra de unidad específica, copiar el archivo y desmontar la unidad.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Por ejemplo:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
También puedes usar el nuevo cmdlet **Mount-DiskImage** para montar un archivo VHD (o ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Paso 8 - Crear la nueva máquina virtual  
El paso final de la configuración, antes de iniciar el proceso de clonación, consiste en crear una nueva VM que utilice los discos desde el controlador de dominio de origen copiado. En función de lo que elegiste en la fase de copia de discos, tienes dos opciones:  
  
1.  Asociar una nueva VM al disco copiado  
  
2.  Importar la VM exportada  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Asociar una nueva VM a los discos copiados  
Si copiaste el disco del sistema manualmente, debes crear una nueva máquina utilizando el disco copiado. El hipervisor establece automáticamente el identificador de generación de VM cuando se crea una nueva VM; no hace falta cambiar la configuración en la VM o en el host de Hyper-V.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Método de Administrador de Hyper-V  
  
1.  Crea una máquina virtual nueva.  
  
2.  Especifica el nombre de la VM, la memoria y la red.  
  
3.  En la página Conectar disco duro virtual, especifica el disco del sistema copiado.  
  
4.  Completa el asistente para crear la VM.  
  
Si hubiera varios discos, adaptadores de red u otras personalizaciones, configúralos antes de iniciar el controlador de dominio. El método de "exportar-importar" para copiar discos está recomendado para VM complejas.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes usar el módulo de Windows PowerShell de Hyper-V para automatizar la creación de VM en Windows Server 2012, utilizando el siguiente cmdlet:  
  
```  
New-VM  
```  
  
Por ejemplo, en este caso se crea la VM DC4-CLONEDFROMDC2, para lo que se usa 1 GB de RAM, se arranca desde el archivo c:\vm\dc4-systemdrive-clonedfromdc2.vhd y se utiliza la red virtual 10.0:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Import VM  
Si previamente exportaste tu VM, ahora debes importarla como copia. De este modo, se utiliza el XML exportado para recrear el equipo con la configuración, los controladores, las redes y los ajustes de memoria anteriores.  
  
Si planeas crear copias adicionales desde la misma VM exportada, haz tantas copias de la VM exportada como sean necesarias. A continuación, utiliza Importar para cada copia.  
  
> [!IMPORTANT]  
> Es importante usar la opción **Copiar**, ya que si exportas se conserva toda la información de origen, mientras que si importas el servidor con **Mover** o **Sin mover** se produce una colisión de información si se hace en el mismo servidor host de Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Método de Administrador de Hyper-V  
Para importar utilizando el complemento Administrador de Hyper-V:  
  
1.  Haz clic en **Importar máquina virtual**.  
  
2.  En la página **Buscar carpeta** , selecciona el archivo de definición de la VM exportada utilizando el botón Examinar.  
  
3.  En la página **Seleccionar máquina virtual**, haz clic en el equipo de origen.  
  
4.  En la página **Elegir tipo de importación** , haz clic en **Copiar la máquina virtual (crear un identificador exclusivo nuevo)** y, después, en **Finalizar**.  
  
5.  Cambia el nombre de la VM importada si la estás importando al mismo host de Hyper-V; tendrá el mismo nombre que el controlador de dominio de origen exportado.  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
Recuerda eliminar todas las instantáneas importadas utilizando el complemento Administrador de Hyper-V:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> Es muy importante que elimines todas las instantáneas importadas; si se aplican, devolverían el controlador de dominio clonado al estado de un DC anterior (probablemente activo), lo que provocaría un error de replicación, información de IP duplicada y otras interrupciones.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes usar el módulo de Windows PowerShell de Hyper-V para automatizar la importación de VM en Windows Server 2012, utilizando los siguientes cmdlets:  
  
```  
Import-VM  
Rename-VM  
```  
  
Por ejemplo, en este caso, la VM exportada DC2-CLONED se importa utilizando el archivo XML determinado automáticamente y se le cambia de nombre de inmediato a DC5-CLONEDFROMDC2, que será su nuevo nombre de VM:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
Recuerda eliminar todas las instantáneas importadas utilizando los siguientes cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por ejemplo:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]
> Asegúrate de que, al importar el equipo, las direcciones MAC estáticas no se asignaron al controlador de dominio de origen. Si se clona un equipo de origen con una MAC estática, los equipos copiados no enviarán ni recibirán tráfico de red correctamente. Establece una nueva dirección MAC exclusiva, estática o dinámica, si este es el caso. Puedes ver si una VM utiliza direcciones MAC estáticas con el comando:  
> 
> **Get-VM-VMName**   
>  ***Test-VM* | Get-VMNetworkAdapter | FL \\** *  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Paso 9 - Clonar la nueva máquina virtual  
Si lo deseas, antes de empezar a clonar, puedes reiniciar el controlador de dominio de origen de clonación sin conexión. A pesar de todo, asegúrate de que el emulador de PDC esté en línea.  
  
Para comenzar la clonación, basta con que inicies la nueva máquina virtual. El proceso empieza de forma de forma automática, y el controlador de dominio se reinicia automáticamente una vez finalizada la clonación.  
  
> [!IMPORTANT]  
> No se recomienda mantener los controladores de dominio apagados durante mucho tiempo; además, si el clon se une al mismo sitio que su DC de origen, la topología de replicación inicial dentro de los sitios y entre los sitios podría tardar más en generarse si el controlador de dominio de origen está sin conexión.  
  
Si usas Windows PowerShell para iniciar una VM, el cmdlet nuevo del módulo de Hyper-V es:  
  
```  
Start-VM  
```  
  
Por ejemplo:  
  
![Implementación de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Cuando el equipo se reinicie una vez finalizada la clonación, será un controlador de dominio y podrás iniciar sesión de la manera habitual para confirmar que funciona normalmente. Si se produce algún error, el servidor se configura para iniciarse en el modo de restauración de servicios de directorio para investigarlo.  
  
## <a name="BKMK_VDCSafeRestore"></a>Medidas de seguridad de virtualización  
A diferencia de lo que sucede en la clonación de controladores de dominio virtualizados, no hay pasos para configurar las medidas de seguridad de virtualización de Windows Server 2012. La característica funciona sin necesidad de intervención, siempre y cuando cumplas unas simples condiciones:  
  
-   El hipervisor es compatible con el identificador de generación de VM.  
  
-   Hay un controlador de dominio asociado válido desde el que un controlador de dominio restaurado puede replicar cambios de forma no autoritativa.  
  
### <a name="validate-the-hypervisor"></a>Validar el hipervisor  
Consulta la documentación del proveedor para asegurarte de que el controlador de dominio de origen se esté ejecutando en un hipervisor compatible. Los controladores de dominio virtualizados son independientes del hipervisor y no necesitan Hyper-V.  
  
Consulta en la anterior sección [Requisitos de la plataforma](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) la compatibilidad conocida con el identificador de generación de VM.  
  
Si deseas migrar varias VM desde un hipervisor de origen a un hipervisor de destino diferente, las medidas de seguridad de virtualización podrían activarse o no, en función de si los hipervisores son compatibles con el identificador de generación de VM, como se explica en la tabla siguiente.  
  
|Hipervisor de origen|Hipervisor de destino|Resultado|  
|---------------------|---------------------|----------|  
|Compatible con el identificador de generación de VM|No compatible con el identificador de generación de VM|Las medidas de seguridad no se activan (si hay un archivo DCCloneConfigFile.xml, el DC arrancará en DSRM)|  
|No compatible con el identificador de generación de VM|Compatible con el identificador de generación de VM|Las medidas de seguridad se activan|  
|Compatible con el identificador de generación de VM|Compatible con el identificador de generación de VM|Las medidas de seguridad no se activan porque la definición de la VM no ha cambiado, lo que significa que el identificador de generación de VM sigue siendo el mismo|  
  
### <a name="validate-the-replication-topology"></a>Validar la topología de replicación  
Las medidas de seguridad de virtualización inician la replicación entrante no autoritativa para el delta de la replicación de Active Directory, así como para la resincronización no autoritativa de todos los contenidos SYSVOL. De este modo, se garantiza que el controlador de dominio regrese de una instantánea con una funcionalidad completa y que sea coherente con el resto del entorno.  
  
Esta nueva capacidad conlleva varios requisitos y limitaciones:  
  
-   Un controlador de dominio restaurado debe ser capaz de ponerse en contacto con un DC de escritura  
  
-   No debe realizarse la restauración simultánea de todos los controladores de dominio de un dominio  
  
-   Se perderán para siempre todos los cambios originados en un controlador de dominio restaurado que todavía no se hayan replicado hacia fuera desde que se tomó la instantánea  
  
Aunque la sección de solución de problemas contempla estas situaciones, los detalles mencionados a continuación te ayudan a asegurarte de que no crees una topología que pueda causar problemas.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilidad del controlador de dominio de escritura  
Si un controlador de dominio se restaura, debe tener conectividad con un controlador de dominio de escritura, ya que un controlador de dominio de solo lectura no puede enviar el delta de actualizaciones. Probablemente la topología ya sea correcta para esto, debido a que un controlador de dominio de escritura siempre necesita un asociado de escritura. Sin embargo, si todos los controladores de dominio de escritura se restauran simultáneamente, ninguno de ellos encontrará un origen válido. Lo mismo sucede si los controladores de dominio de escritura se encuentran sin conexión por tareas de mantenimiento o son inaccesibles a través de la red por cualquier otra razón.  
  
#### <a name="simultaneous-restore"></a>Restauración simultánea  
No restaures todos los controladores de dominio de un mismo dominio al mismo tiempo. Si todas las instantáneas se restauran al mismo tiempo, la replicación de Active Directory funcionará con normalidad, pero la replicación de SYSVOL se detendrá. La arquitectura de restauración de FRS y DFSR obliga a establecer su instancia de réplica en un modo de sincronización no autoritativa. Si todos los controladores de dominio se restauran al mismo tiempo, y si cada controlador de dominio se marca como no autoritativo para SYSVOL, todos intentarán sincronizar directivas de grupo y scripts desde un asociado autoritativo; sin embargo, en ese momento, todos los asociados son no autoritativos.  
  
> [!IMPORTANT]  
> Si todos los controladores de dominio se restauran al mismo tiempo, consulta los siguientes artículos para establecer como autoritativo un controlador de dominio (generalmente, el emulador de PDC), de modo que los demás controladores de dominio puedan volver a funcionar con normalidad:  
>   
> [Usar la clave del registro BurFlags para reinicializar conjuntos de réplicas del servicio de replicación de archivos](https://support.microsoft.com/kb/290762)  
>   
> [Cómo forzar una sincronización autoritaria y no autoritativa para SYSVOL replicado con DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> No ejecutes todos los controladores de dominio de un bosque o dominio en el mismo host de hipervisor. De este modo, se introduce un único punto de error que inutiliza AD DS, Exchange, SQL y otras operaciones empresariales cada vez que el hipervisor está sin conexión. Esto es igual que utilizar un solo controlador de dominio para todo un dominio o bosque. La existencia de varios controladores de dominio en varias plataformas ayuda a ofrecer redundancia y tolerancia a errores.  
  
#### <a name="post-snapshot-replication"></a>Replicación tras las instantáneas  
No restaures las instantáneas hasta que se hayan replicado hacia fuera todos los cambios que se originaron localmente desde la creación de la instantánea. Todos los cambios que se produzcan se perderán para siempre si no los recibieron mediante replicación otros controladores de dominio.  
  
Usa Repadmin.exe para ver todos los cambios salientes no replicados entre un controlador de dominio y sus asociados:  
  
1.  Devuelve los nombres del DC del asociado y los GUID del objeto DSA con:  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Devuelve la replicación entrante pendiente del controlador de dominio del asociado al controlador de dominio que se debe restaurar:  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
Otra opción, solo para ver el número de cambios sin replicar:  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Por ejemplo, (con el resultado modificado para facilitar la lectura y las entradas importantes en ***cursiva***), aquí puedes ver los asociados de replicación de DC4:  
  
```  
C:\>repadmin.exe /showrepl dc4.corp.contoso.com /repsto  
  
Default-First-Site-Name\DC4  
DSA Options: IS_GC  
Site Options: (none)  
DSA object GUID: 5d083398-4bd3-48a4-a80d-fb2ebafb984f  
DSA invocationID: 730fafec-b6d4-4911-88f2-5b64e48fc2f1  
  
==== OUTBOUND NEIGHBORS FOR CHANGE NOTIFICATIONS ============  
  
DC=corp,DC=contoso,DC=com  
    Default-First-Site-Name\DC3 via RPC  
        DSA object GUID: f62978a8-fcf7-40b5-ac00-40aa9c4f5ad3  
        Last attempt @ 2011-11-11 15:04:12 was successful.  
    Default-First-Site-Name\DC2 via RPC  
        DSA object GUID: 3019137e-d223-4b62-baaa-e241a0c46a11  
        Last attempt @ 2011-11-11 15:04:15 was successful.  
```  
  
Ahora sabes que está replicando con DC2 y DC3. A continuación, se muestra la lista de cambios que DC2 afirma todavía no haber recibido de DC4, donde verás que hay un grupo nuevo:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com  
  
==== SOURCE DSA: (null) ====  
Objects returned: 1  
(0) add CN=newgroup4,CN=Users,DC=corp,DC=contoso,DC=com  
    1> parentGUID: 55fc995a-04f4-4774-b076-d6a48ac1af99  
    1> objectGUID: 96b848a2-df1d-433c-a645-956cfbf44086  
    2> objectClass: top; group  
    1> instanceType: 0x4 = ( WRITE )  
    1> whenCreated: 11/11/2011 3:03:57 PM Eastern Standard Time  
```  
  
En este caso, probarías el otro asociado para asegurarte de que todavía no se ha replicado.  
  
Si no te importa qué objetos no se han replicado y solo deseas saber si hay objetos pendientes, también puedes utilizar la opción **/statistics**:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Prueba todos los asociados de escritura si ves algún error o replicación pendiente. Siempre que haya por lo menos uno convergente, lo más seguro suele ser restaurar la instantánea, ya que la replicación transitiva acaba reconciliando los demás servidores.  
>   
> Asegúrate de tomar nota de los errores de la replicación que muestre /showchanges y no sigas adelante antes de solucionarlos.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Cmdlets de instantáneas de Windows PowerShell  
Los siguientes cmdlets del módulo de Hyper-V de Windows PowerShell proporcionan funciones de instantánea en Windows Server 2012:  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


