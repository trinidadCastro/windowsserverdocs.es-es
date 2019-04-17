---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: "Configuración e implementación de controlador de dominio virtualizada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dcb377cef003458bcdf2e4b3564167cee4310020
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Configuración e implementación de controlador de dominio virtualizada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema trata:  
  
-   [Consideraciones sobre la instalación](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Esto incluye los requisitos de la plataforma y otras restricciones importantes.  
  
-   [Virtualizados dominio clonación del controlador](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Esto explica en detalle el controlador de dominio virtualizada todo clonación proceso.  
  
-   [Restauración segura de controlador de dominio virtualizada](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Esto explica en detalle las validaciones que se realizan durante la restauración segura del controlador de dominio virtualizada.  
  
## <a name="BKMK_InstallConsiderations"></a>Consideraciones sobre la instalación  
No hay ninguna función especial o característica virtualizada para controladores de dominio; todos los controladores de dominio automáticamente con capacidades de restauración de clonación y seguro. No pueden quitar ni deshabilitar estas funcionalidades.  
  
Uso de los controladores de dominio de Windows Server 2012 requiere un AD DS esquema versión 56 o posterior de Windows Server 2012 y el nivel funcional del bosque igual, nativo de Windows Server 2003 o superior.  
  
Ambos controladores de dominio de escritura y lectura admiten todos los aspectos de DC virtualizado, así como catálogos globales y FSMO roles.  
  
> [!IMPORTANT]  
> El titular de la función FSMO de emulador PDC debe estar conectado cuando comienza la clonación.  
  
### <a name="BKMK_PlatformReqs"></a>Requisitos de plataforma  
Se requiere la clonación virtualizada de controlador de dominio:  
  
-   Función de FSMO de emulador PDC hospedado en un controlador de dominio de Windows Server 2012  
  
-   Emulador PDC disponible durante las operaciones de clonación  
  
Restauración de clonación y seguro requieren:  
  
-   Windows Server 2012 virtualizados invitados  
  
-   Plataforma de host de virtualización es compatible con el Id. de generación de la máquina virtual (VMGID)  
  
Revisa la tabla siguiente para productos de virtualización y si admiten virtualizan los controladores de dominio y el identificador de generación de la máquina virtual.  
  
|||  
|-|-|  
|**Producto de virtualización**|**Admite controladores de dominio virtualizada y VMGID**|  
|**Servidor de Microsoft Windows Server 2012 con la característica de Hyper-V**|Sí|  
|**Microsoft Windows Server 2012 Hyper-V Server**|Sí|  
|**Características de Microsoft Windows 8 con cliente Hyper-V**|Sí|  
|**Windows Server 2008 R2 y Windows Server 2008**|No|  
|**Soluciones de virtualización no son de Microsoft**|Ponte en contacto con el proveedor|  
  
Aunque Microsoft admite Virtual PC con Windows 7, Virtual PC 2007, Virtual PC 2004 y Virtual Server 2005, no se pueden ejecutar a invitados de 64 bits, así como tampoco son compatibles con GenerationID de máquina virtual.  
  
Para obtener ayuda con los productos de virtualización de terceros y su postura compatibilidad con controladores de dominio virtualizada, ponte en contacto con dicho proveedor directamente.  
  
Para obtener más información, revisa la directiva de soporte técnico para [software de Microsoft que se ejecuta en el software de virtualización de hardware no son de Microsoft](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Advertencias críticas  
Controladores de dominio virtualizada hacen *no* admite restauración seguro de las siguientes acciones:  
  
-   Archivos VHD y VHDX copiados manualmente los archivos VHD existentes  
  
-   Archivos VHD y VHDX restaurados mediante el software de copia de seguridad de disco completo o copia de seguridad de archivos  
  
> [!NOTE]  
> Archivos VHDX son nuevos en Windows Server 2012 Hyper-V.  
  
Ninguna de estas operaciones se explica en la máquina virtual GenerationID semántica y por lo tanto no cambian el identificador de generación de la máquina virtual. Restauración de dominio, ya sea podrían dar como resultado de los controladores de estos métodos en una reversión de USN y poner en cuarentena el controlador de dominio o introducir objetos persistentes y la necesidad de operaciones de limpieza ancho de bosque.  
  
> [!WARNING]  
> Restauración segura de controlador de dominio virtualizados no es una sustitución de copias de seguridad de estado de sistema y la Papelera de reciclaje de AD DS.  
>   
> Después de restaurar una instantánea, se han perdido definitivamente los deltas de cambios replicados anteriormente no originados por ese controlador de dominio después de la instantánea. Restauración segura implementa automatizada restauración no autorizado para evitar que la cuarentena del controlador de dominio accidental *solo*.  
  
Para obtener más información acerca de burbujas de USN y los objetos persistentes, consulta [operaciones de solución de problemas de Active Directory que con el error 8606: "se han proporcionado atributos insuficientes para crear un objeto"](https://support.microsoft.com/kb/2028495).  
  
## <a name="BKMK_VDCCloning"></a>Virtualizados dominio clonación del controlador  
Hay un número de fases y pasos para clonación un controlador de dominio virtualizado, independientemente de con herramientas gráficas ni Windows PowerShell. En un nivel alto, son las tres fases:  
  
**Preparar el entorno**  
  
-   Paso 1: Validar que el hipervisor admite Id. de generación de la máquina virtual y por lo tanto, clonación  
  
-   Paso 2: Comprueba que el rol de emulador PDC está alojado en un controlador de dominio que ejecuta Windows Server 2012 y que está en línea y accesible mediante el controlador de dominio clonados durante clonación.  
  
**Preparar el controlador de dominio de origen**  
  
-   Paso 3: Autorizar el controlador de dominio de origen para clonar  
  
-   Paso 4: Quitar programas o servicios incompatibles o agregarlos al archivo CustomDCCloneAllowList.xml.  
  
-   Paso 5: Crear DCCloneConfig.xml  
  
-   Paso 6: Desconectar el controlador de dominio de origen  
  
**Crear el controlador de dominio clonados**  
  
-   Paso 7: Copiar o exportar el origen de la máquina virtual y agrega el código XML si no has copiado  
  
-   Paso 8: Crear una nueva máquina virtual desde la copia  
  
-   Paso 9: Iniciar la nueva máquina virtual para comenzar la clonación  
  
No hay ningún procedimiento diferencias en la operación cuando con herramientas gráficas como la consola de administración de Hyper-V o herramientas de línea de comandos, como Windows PowerShell, de modo que los pasos se presentan una sola vez con dos interfaces. Este tema proporciona muestras de Windows PowerShell para explorar de principio a fin automatización del proceso de clonación; no son necesarios para todos los pasos. No hay ninguna herramienta de administración gráfica virtualizada para controladores de dominio incluidos en Windows Server 2012.  
  
Hay varios puntos en el procedimiento que dispone de opciones para crear el equipo clonado y cómo agregar los archivos xml; estos pasos se indican en los siguientes detalles. De lo contrario, el proceso es inalterable.  
  
El siguiente diagrama ilustra la virtualizada proceso controlador de dominio clonación, donde el dominio ya existe.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Paso 1: validar el hipervisor  
Asegúrate de que el controlador de dominio de origen se ejecuta en un hipervisor admitido por revisar la documentación del proveedor. Controladores de dominio virtualizada son independientes de hipervisor y no necesitan Hyper-V.  
  
Si el hipervisor está en Microsoft Hyper-V, asegúrate de que se está ejecutando en Windows Server 2012. Se puede validar con administración de dispositivos  
  
Abre **Devmgmt.msc** y examinar **dispositivos del sistema** para instalar Microsoft Hyper-V dispositivos y controladores. El dispositivo específica del sistema necesario para un controlador de dominio virtualizado es el **contador de generación de Microsoft Hyper-V** (controladores: vmgencounter.sys).  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Paso 2: comprobar la función FSMO PDCE  
Antes de intentar clonar un controlador de dominio, deberás validar que el controlador de dominio que hospeda el emulador de controlador de dominio principal FSMO ejecuta Windows Server 2012. El emulador PDC (PDCE) se requiere por varias razones:  
  
1.  El PDCE crea especial **clonación controladores de dominio** agrupar y establece su permiso en la raíz del dominio para permitir que un controlador de dominio clonar sí.  
  
2.  El controlador de dominio clonación pone en contacto con el PDCE directamente mediante el protocolo de RPC DRSUAPI, con el fin de crear objetos de equipo para el controlador de dominio duplicado.  
  
    > [!NOTE]  
    > Windows Server 2012 extiende el protocolo remoto del servicio de replicación de directorio (DRS) existente (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) para incluir un nuevo método RPC **IDL_DRSAddCloneDC** (Opnum **28**). La **IDL_DRSAddCloneDC** método crea un nuevo objeto de controlador de dominio mediante la copia de atributos de un objeto de controlador de dominio existente.  
    >   
    > Los Estados de un controlador de dominio se componen de objetos de equipo, servidor, configuración NTDS, FRS, DFSR y conexión mantenidos para cada controlador de dominio. Cuando duplicar un objeto, este método RPC reemplaza todas las referencias al controlador de dominio original con los objetos correspondientes del nuevo controlador de dominio. El llamador debe tener el control de acceso a la derecha DS-Clone-controlador de dominio en el contexto de nomenclatura de dominio.  
    >   
    > Uso de este nuevo método requiere siempre un acceso directo al controlador de dominio de emulador PDC desde el llamador.  
    >   
    > Dado que este método RPC es nuevo, el software de análisis de la red requiere analizadores actualizados para incluir los campos para el nuevo 28 Opnum en la existente UUID E3514235-4B06 - 11D 1-AB04-00C04FC2DCD2. De lo contrario, no se puede analizar este tráfico.  
    >   
    > Para obtener más información, consulta [4.1.29 IDL_DRSAddCloneDC (28 Opnum)](https://msdn.microsoft.com/en-us/library/hh554213(v=prot.13).aspx).  
  
***Esto también significa que cuando mediante redes que no esté totalmente enrutadas, clonación del controlador de dominio virtualizada requiere segmentos de red con acceso a la PDCE***. Es aceptable para mover un controlador de dominio clonados a otra red después de la clonación - igual que un controlador de dominio físico - como eres cuidadoso actualizar la información del sitio lógico de AD DS.  
  
> [!IMPORTANT]  
> Cuando clonación un dominio que contiene solo un controlador de dominio, debes asegurarte del que DC de origen es volver a conectarse antes de iniciar las copias clone. Un dominio de producción siempre debe contener al menos dos controladores de dominio.  
  
#### <a name="active-directory-users-and-computers-method"></a>Los usuarios de Active Directory y método de equipos  
  
1.  Usar el complemento Dsa.msc, haga clic con el botón secundario del mouse en el dominio y haz clic en **maestros de operaciones**. Ten en cuenta el controlador de dominio con nombre en la pestaña PDC y cierra el cuadro de diálogo.  
  
2.  Haz clic en el objeto del ese controlador de dominio equipo y haz clic en **propiedades**y, a continuación, validar la información del sistema operativo.  
  
#### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes combinar los siguientes cmdlets de módulo de PowerShell de Windows de Active Directory para devolver la versión del emulador PDC:  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Si no se proporciona el dominio, estos cmdlets suponer el dominio del equipo donde se ejecuta.  
  
El siguiente comando devuelve información PDCE y del sistema operativo:  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
Este siguiente ejemplo muestra cómo especificar el nombre de dominio y filtrado de las propiedades devueltas antes de la canalización de Windows PowerShell:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Paso 3: autorizar a un origen de controlador de dominio  
El controlador de dominio de origen debe tener el control de acceso correcta (coche) **permitir que un controlador de dominio crear una copia de sí mismo** en el encabezado NC del dominio. De manera predeterminada, el grupo conocido **clonación controladores de dominio** tiene este permiso y no contiene miembros. El PDCE crea este grupo cuando ese rol FSMO transfiere a un controlador de dominio de Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Método de centro de administración de Active Directory  
  
1.  Inicio Dsac.exe y navega hasta el origen de controlador de dominio y después abre su página de detalles.  
  
2.  En la **miembro de** sección, agrega el **clonación controladores de dominio** grupo para ese dominio.  
  
#### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes combinar los siguientes cmdlets de PowerShell módulo de Active Directory Windows **get adcomputer** y **adgroupmember agregar** para agregar un controlador de dominio para el **clonación controladores de dominio** grupo:  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Por ejemplo, esto agrega DC1 del servidor al grupo, sin necesidad de especificar el nombre completo del miembro del grupo:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>Reconstrucción de permisos predeterminados  
Si quitas este permiso de la cabeza de dominio, clonación se produce un error. Puedes volver a crear el permiso con el centro de administración de Active Directory o Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Método de centro de administración de Active Directory  
  
1.  Abre **centro de administración de Active Directory**, haz clic en el encabezado de dominio, haz clic en **propiedades**, haz clic en el **extensiones** , haga clic **seguridad**y, a continuación, haz clic en **avanzadas**. Haz clic en **solo este objeto**.  
  
2.  Haz clic en **agregar**, en **escribe el nombre de objeto para seleccionar**, escribe el nombre del grupo **clonación controladores de dominio.**  
  
3.  En permisos, haz clic en **permitir que un controlador de dominio crear una copia de sí mismo**y, a continuación, haz clic en **Aceptar**.  
  
> [!NOTE]  
> También puedes quitar el permiso de forma predeterminada y agregar controladores de dominio individuales. Si lo haces, es probable que cause problemas de mantenimiento en curso no obstante, donde los administradores nuevos son conscientes de esta personalización. Cambiar la configuración predeterminada no se aumenta la seguridad y se recomienda que no.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Usa los siguientes comandos en un símbolo del sistema consola de administrador privilegios elevados de Windows PowerShell. Estos comandos detectan el nombre de dominio y se agregan en los permisos predeterminados:  
  
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
  
Como alternativa, ejecutar la muestra [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) en una consola de Windows PowerShell, donde se inicia la consola como un administrador con privilegios elevados en un controlador de dominio en el dominio afectado. Establecen automáticamente los permisos. La muestra se encuentra en el apéndice de este módulo.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Paso 4: quitar las aplicaciones incompatibles o servicios (si no usas CustomDCCloneAllowList.xml)  
Todos los programas o servicios previamente devueltos por Get-ADDCCloningExcludedApplicationList - *y no agrega a la CustomDCCloneAllowList.xml* -debe quitarse antes de clonación. Desinstalar la aplicación o servicio es el método recomendado.  
  
> [!WARNING]  
> Cualquier programa incompatible o servicio no desinstalar o agregado a la CustomDCCloneAllowList.xml impide la clonación.  
  
Usa el cmdlet Get-AdComputerServiceAccount para encontrar las cuentas de servicio administradas independiente (MSA) en el dominio y si este equipo está usando cualquiera de ellas. Si está instalado cualquier MSA, usa el cmdlet de desinstalar ADServiceAccount para quitar la cuenta de servicio instalada localmente. Una vez que termines con teniendo el controlador de dominio de origen sin conexión en el paso 6, puedes volver a agregar la MSA con instalación ADServiceAccount cuando el servidor se vuelva a estar conectado. Para obtener más información, consulta [desinstalar ADServiceAccount](https://technet.microsoft.com/en-us/library/hh852310).  
  
> [!IMPORTANT]  
> Se reemplazaron MSA independiente - publicó por primera vez en Windows Server 2008 R2 - en Windows Server 2012 con MSA de grupo. Grupo MSA admite la clonación.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Paso 5: crear DCCloneConfig.xml  
El archivo DcCloneConfig.xml es necesario para clonar controladores de dominio. Su contenido te permite especificar únicos detalles como el nuevo nombre de equipo y la dirección IP.  
  
El archivo CustomDCCloneAllowList.xml es opcional, a menos que instales aplicaciones o servicios de Windows potencialmente incompatibles en el controlador de dominio de origen. Los archivos que requieren precisa de nomenclatura, formato y la colocación; clonación de lo contrario, se produce un error.  
  
Por ese motivo, siempre debe usar los cmdlets de Windows PowerShell para crear los archivos XML y colocarlos en la ubicación correcta.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Generación de nuevo ADDCCloneConfigFile  
El módulo de Active Directory de Windows PowerShell contiene un cmdlet de nuevo en Windows Server 2012:  
  
```  
New-ADDCCloneConfigFile  
```  
  
Ejecutar el cmdlet en el controlador de dominio de origen propuesta que vayas a clonar. El cmdlet admite varios argumentos y cuando se usa, siempre comprueba el equipo y el entorno donde se ejecuta a menos que especifiques el - argumento sin conexión.  
  
||||  
|-|-|-|  
|**Active Directory**<br /><br />**Cmdlet**|**Argumentos**|**Explicación**|  
|**Nueva ADDCCloneConfigFile**|*<no argument specified>*|Crea un archivo de DcCloneConfig.xml en blanco en el directorio de trabajo de DSA (valor predeterminado: systemroot%\ntds %)|  
||-CloneComputerName|Especifica el nombre del equipo de DC clone. Tipo de datos de cadena.|  
||-Path|Especifica la carpeta para crear el DcCloneConfig.xml. Si no se especifica, se escribe en el directorio de trabajo de DSA (valor predeterminado: systemroot%\ntds %). Tipo de datos de cadena.|  
||-El nombre de sitio|Especifica el nombre de sitio lógico de anuncios para unirte a durante la creación de cuentas de equipo clonados. Tipo de datos de cadena.|  
||-IPv4Address|Especifica la dirección IPv4 estática del equipo clonado. Tipo de datos de cadena.|  
||-IPv4SubnetMask|Especifica la máscara de subred IPv4 estática del equipo clonado. Tipo de datos de cadena.|  
||-IPv4DefaultGateway|Especifica la dirección de puerta de enlace predeterminada de estática IPv4 del equipo clonado. Tipo de datos de cadena.|  
||-IPv4DNSResolver|Especifica las entradas de DNS IPv4 estáticas del equipo clonado en una lista separada por comas. Tipo de datos de matriz. Pueden proporcionarse hasta cuatro entradas.|  
||-PreferredWINSServer|Especifica la dirección IPv4 estática del servidor WINS principal. Tipo de datos de cadena.|  
||-AlternateWINSServer|Especifica la dirección IPv4 estática del servidor WINS secundario. Tipo de datos de cadena.|  
||-IPv6DNSResolver|Especifica las entradas de DNS IPv6 estáticas del equipo clonado en una lista separada por comas. No hay ninguna manera de establecer la información estática Ipv6 clonación del controlador de dominio virtualizada. Tipo de datos de matriz.|  
||-Sin conexión|No realizar las pruebas de validación y sobrescribe cualquier dccloneconfig.xml existente. No tiene parámetros. Para obtener más información, consulta [ADDCCloneConfigFile de nuevo ejecutando en modo sin conexión](../../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).|  
||*-Estático*|Obligatorio si se especifica los argumentos IP estáticas IPv4SubnetMask, IPv4SubnetMask o IPv4DefaultGateway. No tiene parámetros.|  
  
Se realizan pruebas cuando se ejecuta en modo en línea:  
  
-   Emulador PDC es Windows Server 2012 o posterior  
  
-   Controlador de dominio de origen es un miembro del grupo de controladores de dominio clonación  
  
-   Controlador de dominio de origen no incluye servicios ni aplicaciones excluidas  
  
-   Controlador de dominio de origen no contiene un DcCloneConfig.xml en la ruta especificada  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Paso 6: desconectar el controlador de dominio de origen  
No se puede copiar un origen de ejecución DC; debe ser apagado correctamente. No se clonar dejado de pérdida de energía graceless un controlador de dominio.  
  
#### <a name="graphical-method"></a>Método gráfico  
Usa el botón de apagado en el DC de ejecución o en el botón de apagado del Administrador de Hyper-V.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Se pueden apagar una máquina virtual con cualquiera de los siguientes cmdlets:  
  
```  
Stop-computer  
Stop-vm  
```  
  
Equipo de parada es un cmdlet que admita apagar equipos con independencia de virtualización y es similar a la utilidad Shutdown.exe heredada. Vm de parada es un cmdlet de nuevo en el módulo de PowerShell de Windows de Windows Server 2012 Hyper-V y es equivalente a las opciones de energía en el Administrador de Hyper-V. La segunda opción es útil en entornos de laboratorio donde el controlador de dominio a menudo funciona en una red privada virtualizada.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Paso 7: copia discos  
Una elección administrativa es necesario en la fase de copiar:  
  
-   Copiar los discos manualmente, sin Hyper-V  
  
-   Exportar la máquina virtual, uso de Hyper-V  
  
-   Exportar los discos combinados, uso de Hyper-V  
  
Deben copiar todos los discos de una máquina virtual, no solo la unidad del sistema. Si el controlador de dominio de origen usa discos de diferenciación y vas a mover el controlador de dominio clonados a otro host de Hyper-V, debe exportar.  
  
Copiar manualmente los discos se recomienda si el controlador de dominio de origen tiene solo *una* unidad. Exportar o importar es la recomendada para las máquinas virtuales con *más de un* unidad u otras personalizaciones de hardware virtualizado complejos, como varias NIC.  
  
Si copiando archivos manualmente, elimina las instantáneas antes de copiar. Si vas a exportar la máquina virtual, eliminar instantáneas antes de exportar o eliminarlos de la nueva máquina virtual después de importar.  
  
> [!WARNING]  
> Las instantáneas son discos que pueden devolver un controlador de dominio al estado anterior de diferenciación. Si estuvieras clonar un controlador de dominio y, a continuación, restaurar su instantánea clonación previamente, terminaría con controladores de dominio duplicadas en el bosque. No hay ningún valor en instantáneas anteriores en un controlador de dominio recién clonados.  
  
#### <a name="manually-copying-disks"></a>Copiar manualmente los discos  
  
##### <a name="hyper-v-manager-method"></a>Método de administrador de Hyper-V  
Usar el complemento Administrador de Hyper-V para determinar qué discos están asociados con el controlador de dominio de origen. Usa la opción de inspeccionar para validar si el controlador de dominio usa discos de diferenciación (lo que requiere que también copia el disco principal)  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Para eliminar las instantáneas, selecciona una máquina virtual y eliminar el subárbol de la instantánea.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
A continuación, puedes copiar manualmente los archivos VHD o VHDX con el Explorador de Windows, Xcopy.exe o Robocopy.exe. No hay pasos especiales son necesarios. Es un procedimiento recomendado para cambiar los nombres de archivo, incluso si se mueve a otra carpeta.  
  
> [!NOTE]  
> Si copiar entre equipos host de una LAN (1 GB o superior), el **Xcopy.exe /J** opción copia los archivos VHD/VHDX considerablemente más rápido que cualquier otra herramienta a costa de mucho mayor uso de ancho de banda.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Para determinar los discos mediante Windows PowerShell, usa los módulos de Hyper-V:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
Por ejemplo, puedes devolver todas las unidades de disco duro IDE desde una máquina virtual denominada **DC2** con el siguiente ejemplo:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Si la ruta de acceso de disco apunta a un archivo AVHD o AVHDX, es una instantánea. Para eliminar las instantáneas asociadas con un disco y los combina en el disco duro virtual o VHDX real, usar cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por ejemplo, para eliminar todas las instantáneas de una máquina virtual denominado DC2 SOURCECLONE:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Para copiar los archivos con Windows PowerShell, usa el cmdlet siguiente:  
  
```  
Copy-Item  
```  
  
Combinar con los cmdlets de máquina virtual en las canalizaciones para facilitar la automatización. La canalización es un canal utilizado entre varios cmdlets para pasar datos. Por ejemplo, para copiar la unidad de un controlador de dominio sin conexión origen denominado DC2 SOURCECLONE en un nuevo disco llamado c:\temp\copy.vhd sin necesidad de conocer la ruta de acceso exacta a su unidad del sistema:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> No puedes usar el paso discos con la clonación, como no usan un archivo de disco virtual sino en un disco duro real.  
  
> [!NOTE]  
> Para obtener más información acerca de las operaciones de Windows PowerShell más con canalizaciones, consulta [diseño y la canalización de Windows PowerShell](https://technet.microsoft.com/en-us/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Exportar la máquina virtual  
Como alternativa a la copia de los discos, puedes exportar toda la VM de Hyper-V como una copia. Exportar automáticamente, crea una carpeta para la máquina virtual que contiene todos los discos y la información de configuración.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Método de administrador de Hyper-V  
Para exportar una máquina virtual con el Administrador de Hyper-V:  
  
1.  Haz clic en el controlador de dominio de origen y haz clic en **exportar**.  
  
2.  Selecciona una carpeta existente como el contenedor de exportación.  
  
3.  Espera a que la columna Estado dejar de mostrar **exportar**.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Para exportar una máquina virtual con el módulo de PowerShell de Windows Hyper-V, usa el cmdlet:  
  
```  
Export-vm  
```  
  
Por ejemplo, para exportar una máquina virtual denominado DC2 SOURCECLONE en una carpeta denominada C:\VM:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Admite Windows Server 2012 Hyper-V nuevo exporta e importa las funcionalidades que están fuera del ámbito de este curso. Revisa TechNet para obtener más información.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Exportación de discos combinados, con Hyper-V  
La última opción es usar las opciones de mezcla y la conversión de disco de Hyper-V. Estos te permiten hacer una copia de una estructura existente del disco - incluso al incluir archivos AVHD/AVHDX instantánea; en un único disco nuevo. Al igual que el escenario de copia de disco manual, esto está pensado principalmente para las máquinas virtuales más simples que solo usan una sola unidad, por ejemplo, C:\\. Su Solitario ventaja es que, a diferencia de copiar manualmente, no requiere eliminar primero las instantáneas. Esta operación es necesariamente más lenta que simplemente eliminar las instantáneas y copiar discos.  
  
##### <a name="hyper-v-manager-method"></a>Método de administrador de Hyper-V  
Para crear un disco combinado con el Administrador de Hyper-V:  
  
1.  Haz clic en **editar disco**.  
  
2.  Busca el disco secundarios más baja. Por ejemplo, si usas un disco de diferenciación, el disco secundarios es el objeto secundario inferior. Si la máquina virtual tiene una instantánea (o varios), la fotografía seleccionada es el disco de hijo menor.  
  
3.  Selecciona el **combinar** opción para crear un único disco fuera de la estructura de elementos primarios y secundarios todo.  
  
4.  Selecciona un nuevo disco duro virtual y proporciona una ruta de acceso. Esto resuelve los archivos VHD/VHDX existentes en una sola unidad nueva portátil que no está en peligro de restauración de instantáneas anteriores.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Para crear un disco combinado de un conjunto de elementos principales de uso del módulo de PowerShell de Windows Hyper-V complejo, usa el cmdlet:  
  
```  
Convert-vm  
```  
  
Por ejemplo, para exportar toda la cadena de instantáneas de disco de una máquina virtual (en este momento no incluidos los discos de diferencias) y principales de disco en un único disco nuevo llamado CLONAN DC4. VHDX:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>Agregar código XML para el disco del sistema sin conexión  
Si copia la Dccloneconfig.xml en el DC de origen está ejecutando, debe copiar el archivo de dccloneconfig.xml actualizada en el disco del sistema sin conexión copiado y exportar ahora. Dependiendo de las aplicaciones instaladas detectadas con Get-ADDCCloningExcludedApplicationList anteriormente, también necesitarás copiar el archivo CustomDCCloneAllowList.xml en el disco.  
  
Las siguientes ubicaciones pueden contener el archivo DcCloneConfig.xml:  
  
1.  Directorio de trabajo de DSA  
  
2.  %windir%\Ntds  
  
3.  Medios extraíbles de lectura y escritura, por orden de la letra de unidad, en la raíz de la unidad  
  
Estas rutas de acceso no son configurables. Una vez que comienza la clonación, las comprobaciones de clonación estas ubicaciones en ese orden específico y usa la primera DcCloneConfig.xml de archivos encontrados, independientemente del contenido de otra carpeta.  
  
Las siguientes ubicaciones pueden contener el archivo CustomDCCloneAllowList.xml:  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  Directorio de trabajo de DSA  
  
3.  %windir%\Ntds  
  
4.  Medios extraíbles de lectura y escritura, por orden de la letra de unidad, en la raíz de la unidad  
  
Puedes ejecutar ADDCCloneConfigFile de nuevo con el **-sin conexión** argumento (también conocida como desconectado) para crear el archivo DcCloneConfig.xml y colocarlo en una ubicación adecuada. Los siguientes ejemplos muestran cómo ejecutar ADDCCloneConfigFile de nuevo en el modo sin conexión.  
  
Para crear un controlador de dominio clone denominado CloneDC1 en el modo sin conexión en un sitio denominado a "REDMOND" con dirección IPv4 estática, escribe:  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Para crear un controlador de dominio clone denominado Clone2 en modo sin conexión con estática IPv4 y IPv6 una configuración estática, escribe:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Para crear un controlador de dominio clone en modo sin conexión con estática IPv4 y IPv6 dinámico y especificar varios servidores DNS para la configuración de resolución DNS, escribe:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Para crear un controlador de dominio clone denominado Clone1 en modo sin conexión con dinámico IPv4 e IPv6 una configuración estática, escribe:  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Para crear un controlador de dominio clone en modo sin conexión con dinámico IPv4 y IPv6 dinámico, escribe:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Método de explorador de Windows  
Windows Server 2012, ahora se ofrece una opción de gráficos para el montaje de archivos VHD y VHDX. Esto requiere la instalación de la característica experiencia de escritorio en Windows Server 2012.  
  
1.  Haz clic en el archivo VHD/VHDX copiado que contiene la unidad del sistema o una carpeta de la ubicación de directorio de trabajo de DSA el origen del controlador de dominio y, a continuación, haz clic en **montar** desde el **herramientas de imágenes de disco** menú.  
  
2.  En la unidad montada ahora, copia los archivos XML en una ubicación válida. Se te pedirá permiso para la carpeta.  
  
3.  Haz clic en la unidad montada y haz clic en **Eject** desde el **disco herramientas** menú.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Como alternativa, puedes montar el disco sin conexión y copia el archivo XML mediante los cmdlets de Windows PowerShell:  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
Esto te permite un control completo sobre el proceso. Por ejemplo, puede ser la unidad montada con una letra de unidad específica, copia el archivo y desmonta la unidad.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Por ejemplo:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
Como alternativa, puedes usar la nueva **montaje DiskImage** cmdlet para montar un archivo de disco duro virtual (o ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Paso 8: crear la nueva máquina Virtual  
El paso de configuración final antes de iniciar el proceso de clonación es crear una nueva máquina virtual que usa los discos desde el controlador de dominio de origen copiado. Según la selección realizada en la fase de discos copiar, tienes dos opciones:  
  
1.  Asociar una nueva máquina virtual con el disco copiado  
  
2.  Importar la máquina virtual exportada  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Asociar una nueva máquina virtual con los discos copiados  
Si has copiado el disco del sistema manualmente, debes crear una nueva máquina virtual con el disco copiado. El hipervisor establece automáticamente el identificador de generación de la máquina virtual cuando se crea una nueva máquina virtual; No hay cambios de configuración son necesarias en el host de máquina virtual o Hyper-V.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Método de administrador de Hyper-V  
  
1.  Crear una nueva máquina virtual.  
  
2.  Especificar el nombre de máquina virtual, la memoria y la red.  
  
3.  En la página de disco duro Virtual conectarse, especifica el disco del sistema copiado.  
  
4.  Completa el Asistente para crear la máquina virtual.  
  
Si hay varios discos, adaptadores de red u otras personalizaciones, configurar antes de iniciar el controlador de dominio. El método "Exportación e importación" copiar discos se recomienda para máquinas virtuales complejas.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes usar el módulo de Hyper-V Windows PowerShell para automatizar la creación de la máquina virtual en Windows Server 2012, mediante el cmdlet siguiente:  
  
```  
New-VM  
```  
  
Por ejemplo, aquí la VM DC4 CLONEDFROMDC2 se crea, con 1GB de RAM, arranque desde el archivo c:\vm\dc4-systemdrive-clonedfromdc2.vhd y el uso de la red virtual 10.0:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Importar máquina virtual  
Si anteriormente exportado la VM, ahora debes importarla como una copia de nuevo. Usa el XML exportado para volver a crear el equipo con todas las opciones de configuración anteriores, unidades, redes y configuración de la memoria.  
  
Si tienes previsto crear copias adicionales de la misma máquina virtual de exportados, hacer tantas copias de la máquina virtual exportada según sea necesario. A continuación, usar importar para cada copia.  
  
> [!IMPORTANT]  
> Es importante usar el **copia** opción como exportación conserva toda la información de la fuente; importar el servidor con **mover** o **In situ** hace colisión de información si lleva a cabo en el mismo servidor host de Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Método de administrador de Hyper-V  
Para importar mediante el complemento Administrador de Hyper-V:  
  
1.  Haz clic en **Importar máquina Virtual**  
  
2.  En la **Buscar carpeta** página, selecciona el archivo de definición de máquina virtual exportado con el botón Examinar  
  
3.  En la **seleccionar Máquina Virtual** página, haz clic en el equipo de origen.  
  
4.  En la **elegir un tipo de importación** página, haz clic en **copiar la máquina virtual (crear un nuevo identificador único)**, a continuación, haz clic en **finalizar**.  
  
5.  Cambiar el nombre de la máquina virtual importada si importar en el mismo host de Hyper-V; tendrá el mismo nombre que el controlador de dominio de origen exportado.  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
Recuerda que tienes que quitar cualquier instantáneas importadas, mediante el complemento de administración de Hyper-V:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> Eliminar cualquier instantáneas importadas es de vital importancia; Si se aplica, primero volverá el controlador de dominio clonados al estado de un controlador de dominio anterior (y posiblemente live), lo que el error de replicación, información de IP duplicada y otras interrupciones.  
  
##### <a name="windows-powershell-method"></a>Método de Windows PowerShell  
Puedes usar el módulo de Hyper-V Windows PowerShell para automatizar la importación de máquina virtual en Windows Server 2012, con los siguientes cmdlets:  
  
```  
Import-VM  
Rename-VM  
```  
  
Por ejemplo, la exportación aquí se CLONAN VM DC2 importa mediante su archivo XML determina automáticamente, a continuación, cambia el nombre inmediatamente para su nuevo nombre de la máquina virtual DC5 CLONEDFROMDC2:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
Recuerda que tienes que quitar cualquier instantáneas importadas, con los siguientes cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por ejemplo:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]  
> Asegúrate de que, al importar el equipo, no se asignaron direcciones MAC estáticas al controlador de dominio de origen. Si un equipo de origen con un MAC estático se clona, dichos equipos copiados correctamente no enviará o recibir tráfico de red. Establecer una nueva único estática o dinámica dirección MAC si este es el caso. Puedes ver si una máquina virtual usa direcciones MAC con el comando:  
>   
> **VM de Get - VMName**   
>  ***prueba la vm* | Get-VMNetworkAdapter | Florida \ ***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Paso 9: clonar la nueva máquina Virtual  
Opcionalmente, antes de comenzar la clonación, reinicia el controlador de dominio de origen clone sin conexión. Asegúrate de que el emulador PDC está en línea, sin tener en cuenta.  
  
Para comenzar la clonación, simplemente inicia la nueva máquina virtual. El proceso se inicia automáticamente y el controlador de dominio se reinicia automáticamente una vez completada la clonación.  
  
> [!IMPORTANT]  
> No se recomienda mantener los controladores de dominio desactivados durante un largo período de tiempo y si la copia une el mismo sitio como origen de controlador de dominio, el intra inicial y la topología de replicación entre sitios pueden tardar más en generar si el controlador de dominio de origen está sin conexión.  
  
Si usando Windows PowerShell para iniciar una máquina virtual, la nuevo cmdlet de módulo de Hyper-V es:  
  
```  
Start-VM  
```  
  
Por ejemplo:  
  
![Implementación virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Una vez reiniciado el equipo una vez completada la clonación, es un controlador de dominio y puede iniciar sesión en normalmente para confirmar el funcionamiento normal. Si hay errores, el servidor se establece para iniciar en modo de restauración de servicios de directorio para una investigación.  
  
## <a name="BKMK_VDCSafeRestore"></a>Medidas de seguridad de virtualización  
A diferencia de clonación de controlador de dominio virtualizado, medidas de seguridad de virtualización de Windows Server 2012 no tienen ningún paso de configuración. La característica funciona sin intervención siempre que se cumplen algunas condiciones simple:  
  
-   El hipervisor admite Id. de generación de máquina virtual  
  
-   Hay un controlador de dominio de asociado válido que un controlador de dominio restaurado replicar cambios de forma no autorizada.  
  
### <a name="validate-the-hypervisor"></a>Validar el hipervisor  
Asegúrate de que el controlador de dominio de origen se ejecuta en un hipervisor admitido por revisar la documentación del proveedor. Controladores de dominio virtualizada son independientes de hipervisor y no necesitan Hyper-V.  
  
Revisa la anterior [requisitos de la plataforma](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) sección de soporte de Id. de generación de VM conocidas.  
  
Si vas a migrar máquinas virtuales de un hipervisor de origen en un hipervisor de destino diferente, medidas de seguridad de virtualización pueden o no se desencadene dependiendo de si la hipervisores soporte Id. de generación de la máquina virtual, como se explicó en la siguiente tabla.  
  
|Hipervisor de origen|Hipervisor de destino|Resultado|  
|---------------------|---------------------|----------|  
|Id. de generación de VM admite|No es compatible con el identificador de generación de máquina virtual|Medidas de seguridad no desencadenadas (si está presente una DCCloneConfigFile.xml, DC arrancará en DSRM)|  
|No es compatible con el identificador de generación de máquina virtual|Id. de generación de VM admite|Medidas de seguridad que se desencadena|  
|Id. de generación de VM admite|Id. de generación de VM admite|Medidas de seguridad que no se desencadene porque la definición de la máquina virtual no ha cambiado, lo que significa que por lo que conserva el identificador de generación de máquina virtual|  
  
### <a name="validate-the-replication-topology"></a>Validar la topología de replicación  
Medidas de seguridad de virtualización inician no autorizado replicación entrante para el delta de réplica de Active Directory, así como la sincronización no autorizado de todo el contenido SYSVOL. Esto garantiza que el controlador de dominio devuelve desde una instantánea con funcionalidad completa y finalmente coherente con el resto del entorno.  
  
Con esta nueva funcionalidad vienen varios requisitos y limitaciones:  
  
-   Un controlador de dominio restaurado debe ser capaz de ponerse en contacto con un controlador de dominio grabable  
  
-   Todos los controladores de dominio en un dominio no se deben restaurar al mismo tiempo  
  
-   Siempre se perderán los cambios procedentes de un controlador de dominio restaurado que ya no aún replicados saliente desde que se tomó la instantánea  
  
Aunque la sección de solución de problemas trata sobre estos escenarios, los detalles de asegurarse de que no crear una topología que podría causar problemas.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilidad del controlador de dominio de escritura  
Si restaurando, un controlador de dominio debe tener conectividad a un controlador de dominio grabable; un controlador de dominio de solo lectura no puede enviar el delta de actualizaciones. La topología es probable correcto para que esto ya, como un controlador de dominio grabable siempre es necesario a un partner de escritura. Sin embargo, si todos los controladores de dominio grabable restaurar al mismo tiempo, ninguno de ellos puede encontrar un origen válido. Lo mismo ocurre si los controladores de dominio grabable están sin conexión para mantenimiento o de lo contrario no accesible a través de la red.  
  
#### <a name="simultaneous-restore"></a>Restauración simultánea  
No restaurar todos los controladores de dominio en un solo dominio al mismo tiempo. Si todas las instantáneas normalmente al mismo tiempo, restauración funciona la duplicación de Active Directory, pero se detiene la replicación de SYSVOL. La arquitectura de restauración de FRS y DFSR requiere que se configure su instancia de réplica al modo de sincronización no autorizado. Si todos los controladores de dominio restauración a la vez, y cada controlador de dominio se marca no autorizado para SYSVOL, todos ellos, a continuación, tratará de sincronizar las directivas de grupo y scripts de un asociado autorizado; en ese momento, sin embargo, todos los partners también son no autorizado.  
  
> [!IMPORTANT]  
> Si todos los controladores de dominio se restauran a la vez, usa los siguientes artículos para configurar un controlador de dominio - normalmente en el emulador PDC - como autoridad, para que otros controladores de dominio pueden devolver el funcionamiento normal:  
>   
> [Con la clave del registro de BurFlags para reiniciar el servicio de replicación establece réplica](https://support.microsoft.com/kb/290762)  
>   
> [Cómo forzar una sincronización autorizada y no autorizados para replicar DFSR SYSVOL (por ejemplo, "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> No ejecutes todos los controladores de dominio en un dominio o bosque en el mismo host hipervisor. Que presenta un único punto de error que cripples AD DS, Exchange, SQL y otras operaciones empresariales cada vez que el hipervisor se queda sin conexión. Esto no es diferente de usar un único controlador de dominio para todo el dominio o bosque. Varios controladores de dominio en varias plataformas ayudan a proporcionar redundancia y tolerancia a errores.  
  
#### <a name="post-snapshot-replication"></a>Replicación posterior a la instantánea  
No restaurar instantáneas hasta que todos los cambios localmente originales realizados después de la creación de instantáneas se replicaron salientes. Cambios en el origen se perderán para siempre si otros controladores de dominio no ya recibió ellos a través de replicación.  
  
Usar Repadmin.exe para mostrar los cambios de salida sin replicados entre un controlador de dominio y sus socios:  
  
1.  Volver del controlador de dominio de nombres y el GUID de objeto DSA con:  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Devolver la replicación entrante pendiente de controlador de dominio asociado al controlador de dominio restaurarse:  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
Como alternativa, solo para ver el número de cambios no replicados:  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Por ejemplo (con salida modificada para mejorar la legibilidad y entradas importantes ***en cursiva***), aquí mirar las asociaciones de replicación de DC4:  
  
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
  
Ahora ya sabes que se replica con DC2 y DC3. A continuación, mostrar la lista de los cambios que DC2 Estados aún no tienen de DC4 y comprueba que hay un nuevo grupo:  
  
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
  
También podría probar otro asociado para garantizar que había no ya replicaron.  
  
Como alternativa, si no le preocupa qué objetos no tenían replicados y solo atención que todos los objetos eran pendientes, puedes usar la **/statistics** opción:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Prueba a todos los asociados de escritura si ves replicación pendiente o errores. Al menos uno convergen, siempre que es seguro por lo general restaurar la instantánea, como la replicación transitivos finalmente reconcilia a los otros servidores.  
>   
> Asegúrate de tener en cuenta los errores de replicación muestra /showchanges y no continúe son fijas.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Cmdlets de Windows PowerShell instantánea  
Los siguientes cmdlets del módulo de Windows PowerShell y Hyper-V proporcionan funcionalidades de instantáneas en Windows Server 2012:  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


