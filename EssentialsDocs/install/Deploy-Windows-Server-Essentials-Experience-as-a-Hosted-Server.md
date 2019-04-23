---
title: Implementar Experiencia con Windows Server Essentials como servidor hospedado
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b44b395a39a53194b73a0d503c2310edcbe53a2c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876076"
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Implementar Experiencia con Windows Server Essentials como servidor hospedado

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento incluye información específica de los proveedores de hospedaje que prevén implementar Microsoft Windows Server 16 con el rol experiencia con Windows Server Essentials (denominado Windows Server Essentials en el resto del documento) instalado en su laboratorio y se pretende ofrecer la experiencia con Windows Server Essentials como servicio a sus clientes. Este documento incluye las siguientes secciones:  
  

-   [Introducción a experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Ventajas de alojar la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opciones de implementación compatibles](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologías de red admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar la imagen del rol de experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar la implementación de Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar datos desde Windows Small Business Server a Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Realizar tareas comunes mediante Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integración de correo electrónico con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Supervisar y administrar mediante las herramientas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Escenarios de prueba](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Información de soporte técnico](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Introducción a experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Ventajas de alojar la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opciones de implementación compatibles](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologías de red admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar la imagen del rol de experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar la implementación de Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar datos desde Windows Small Business Server a Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Realizar tareas comunes mediante Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integración de correo electrónico con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Supervisar y administrar mediante las herramientas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Escenarios de prueba](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Información de soporte técnico](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a> Introducción a experiencia con Windows Server Essentials  
 La experiencia con Windows Server Essentials es un rol de servidor que está disponible en Windows Server 2012 R2 Standard y Windows Server 2012 R2 Datacenter. Cuando se instala el rol experiencia con Windows Server Essentials en un servidor que ejecuta Windows Server 2012 R2, el cliente puede aprovechar todas las características que están disponibles en Windows Server Essentials sin los bloqueos y límites. La experiencia con Windows Server Essentials permite las siguientes soluciones entre entornos de pequeñas y medianas empresas:  
  
-   **Almacenamiento de datos y la protección** puede almacenar el cliente "datos de s™ en una ubicación centralizada y proteger los datos de cliente y servidor con una copia de seguridad de los equipos servidor y cliente (menos de 75) dentro de la red.  
  
-   **Administración de usuarios** Puede administrar los usuarios y grupos mediante el panel de servidor simplificado. Además, la integración con Microsoft Azure Active Directory (Azure AD) permite fácilmente a los datos de Microsoft online services (por ejemplo, Office 365, Exchange Online y SharePoint Online) para los usuarios mediante sus credenciales de dominio.  
  
-   **Integración de servicios** puede integrar el servidor con Microsoft online services (por ejemplo, Office 365, SharePoint Online y Microsoft Azure Backup). También puede integrar el servidor con sus servicios o los servicios proporcionados por proveedores de terceros.  
  
-   **Acceso desde cualquier lugar** El cliente puede acceder a los servidores, equipos de la red y datos desde prácticamente cualquier lugar con una conexión a Internet y usando casi cualquier dispositivo. El Acceso web remoto les permite tener acceso a las aplicaciones y los datos con una experiencia optimizada y táctil a través de un explorador. La aplicación My Serverles permite tener acceso a datos desde un Windows Phone o una aplicación de Microsoft Store.  
  
-   **Streaming multimedia** si instala el paquete de medios en un servidor con experiencia con Windows Server Essentials habilitado, el cliente final puede almacenar música, vídeo y fotografías en las carpetas compartidas y luego acceder a estos archivos multimedia desde equipos en red o Acceso Web remoto.  
  
-   **Supervisión del estado** Puede supervisar el estado de la red y obtener informes de estado personalizado.  
  
##  <a name="BKMK_Benefits"></a> Ventajas de alojar la experiencia con Windows Server Essentials  
  Experiencia con Windows Server Essentials es un rol de Windows Server, por lo que se puede reutilizar la implementación existente y el marco de administración de Windows Server para implementar y configurar el rol experiencia con Windows Server Essentials. Hospeda el rol experiencia con Windows Server Essentials proporciona las siguientes ventajas:  
  
-   **Implementación simplificada** con solo activar el rol experiencia con Windows Server Essentials, algunas de las más usan roles y características están encendidas y configuradas con los procedimientos recomendados para pequeñas y medianas empresas. Puede personalizar las características de Windows Server Essentials u ocultar algunas de las características locales. Si usa Windows Azure Pack, puede descargar la plantilla de la Galería de experiencia con Windows Server Essentials en Windows Server 2012 R2.  
  
-   **Panel simplificado** El panel de Windows Server Essentials simplifica las tareas comunes, como administrar carpetas de servidor, el almacenamiento del servidor, la copia de seguridad y la restauración, las cuentas de usuario o de grupo, los dispositivos, el acceso remoto y el correo electrónico. Las pequeñas y medianas empresas pueden realizar tareas de administración diarias en lugar de llamar al departamento de soporte técnico.  
  
-   **Extensibilidad** El panel y el software del Conector de Windows Server Essentials son extensibles. Puede agregar su propia marca y servicios de integración como punto de entrada para los clientes en todo lo relacionado con su servidor y su servicio.  
  
-   **Monitor** Está disponible una nueva versión del módulo de supervisión de System Center para supervisar y administrar varios servidores que ejecuten Windows Server Essentials. Para descargar el módulo de administración, consulte [System Center Management Pack para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="BKMK_SupportedDeployment"></a> Opciones de implementación compatibles  
  Experiencia con Windows Server Essentials puede implementarse como un controlador de dominio en un nuevo entorno de Active Directory; o bien puede implementarse en un entorno de Active Directory existente como miembro del dominio.  
  
 Se recomienda que primero implementar Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter y, a continuación, instale el rol experiencia con Windows Server Essentials. Con este método de implementación, obtendrá todas las funcionalidades de edición de Windows Server Essentials, sin los bloqueos y límites.  
  

 Para obtener más información acerca de cómo instalar Windows Server 2012 R2 con el rol experiencia con Windows Server Essentials, consulte [instalar y configurar Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="BKMK_SupportedToplogy"></a> Topologías de red admitidas  
 Para usar la experiencia con Windows Server Essentials desde un cliente móvil, se debe habilitar una VPN. Para habilitar el acceso remoto al servidor de los clientes móviles, deberá abrir el puerto 443 y el puerto 80 en el servidor.  
  
 A continuación se describen dos topologías típicas de red de servidor y cómo se podrían configurar una VPN y el Acceso web remoto:  
  
-   **Topología 1** (topología preferida que coloca todos los servidores y el intervalo IP de la VPN en la misma subred):  
  
    -   Configurar el servidor en una red virtual independiente en un dispositivo de traducción de direcciones de red (NAT).  
  
    -   Habilitar el servicio DHCP en la red virtual o asignar una dirección IP estática para el servidor.  
  
    -   Reenviar el puerto 443 de dirección IP pública en el enrutador a la dirección de red local del servidor.  
  
    -   Permitir el paso de VPN para el puerto 443.  
  
    -   Establecer el grupo de direcciones IPv4 de la VPN en el mismo intervalo de subred que la dirección del servidor.  
  
    -   Asignar a los servidores secundarios una dirección IP estática dentro de la misma subred, pero fuera del grupo de direcciones de VPN.  
  
-   **Topología 2**:  
  
    -   Asignar una dirección IP privada al servidor.  
  
    -   Permitir el puerto 443 en el servidor para acceder a una dirección IP pública del puerto 443.  
  
    -   Permitir el paso de VPN para el puerto 443.  
  
    -   Asignar intervalos diferentes para el grupo de direcciones IPv4 de VPN y la dirección del servidor.  
  
 Con la topología 2, no se admiten servidores secundarios porque no se puede agregar otro servidor al mismo dominio.  
  
 Puede habilitar una VPN durante la implementación automática usando nuestro script de Windows PowerShell. También puede configurarla con el asistente tras la configuración inicial.  
  
 Para habilitar una VPN con Windows PowerShell, ejecute el siguiente comando con privilegios de administrador en el servidor que ejecuta Windows Server Essentials y facilite toda la información necesaria.  
  
```  
##  
## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
##  
  
$myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
$mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
$mySslCertificatePassword = ConvertTo-SecureString  œAsPlainText  œForce '******';   ## password for private key of the SSL certificate  
$skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
Set-WssDomainNameConfiguration  œDomainName $myExternalDomainName  œCertificatePath $mySslCertificateFile  œCertificateFilePassword $mySslCertificatePassword  œNoCertificateVerification  
##  
## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
##  
  
Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
  
```  
  
> [!NOTE]
>  Si no puede proporcionar una conexión VPN antes de que el cliente se apropie del servidor, asegúrese de que el puerto 3389 del servidor sea accesible por Internet. De esta forma, el cliente puede usar el Protocolo de escritorio remoto para conectarse al servidor y configurarlo.  
  
##  <a name="BKMK_CustomizeImage"></a> Personalizar la imagen del rol de experiencia con Windows Server Essentials  
 Puede personalizar la imagen antes de configurar el rol experiencia con Windows Server Essentials. Para obtener más información sobre el proceso de Sysprep de Windows Server estándar, consulte [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx). Después de preparar la imagen mediante Sysprep, puede usarla o incluirla en Install.wim para una nueva implementación.  
  
 Si está utilizando Virtual Machine Manager, puede crear una plantilla mediante la instancia en ejecución. Este proceso utiliza Sysprep para preparar la instancia y apaga el equipo. Después de almacenar la plantilla en la biblioteca, puede usarla para casos específicos.  
  
 Después de instalar el rol experiencia con Windows Server Essentials, puede personalizar las características de Windows Server Essentials. Una de las personalizaciones más importantes es la clave de registro **IsHosted**: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Si se establece en 0x1, algunas de las características locales cambiarán de comportamiento. Entre los cambios de características, se incluyen los siguientes:  
  
-   **Copia de seguridad de clientes** Las copias de seguridad de clientes se deshabilitarán de forma predeterminada para los equipos cliente que se hayan unido recientemente.  
  
-   **Servicio de restauración de clientes** Se deshabilitará el Servicio de restauración de clientes y se ocultará la interfaz de usuario en el panel.  
  
-   **Historial de archivos** El servidor no administrará automáticamente la configuración del Historial de archivos de las cuentas de usuario recién creadas.  
  
-   **Copia de seguridad del servidor** Se deshabilitará el servicio de copia de seguridad del servidor y, a continuación, se ocultará la interfaz de usuario de Copias de seguridad del servidor en el panel.  
  
-   **Espacios de almacenamiento** Se ocultará la interfaz de usuario para crear o administrar espacios de almacenamiento desde el panel.  
  
-   **Acceso desde cualquier lugar** Se omitirá de forma predeterminada la configuración de VPN y del enrutador al ejecutar el asistente de configuración del Acceso desde cualquier lugar.  
  
 Si desea controlar el comportamiento de cada característica incluida, puede establecer la clave de registro correspondiente para cada una. Para obtener información sobre cómo configurar la clave del Registro, consulte [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_AutomateDeployment"></a> Automatizar la implementación de Windows Server Essentials Experience  
 Para automatizar la implementación, deberá implementar primero el sistema operativo y, a continuación, instalar el rol experiencia con Windows Server Essentials.  
  
-   Para implementar automáticamente Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, siga las instrucciones de [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Para obtener información sobre cómo instalar el rol experiencia con Windows Server Essentials mediante Windows PowerShell, consulte [instalar y configurar Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Asegúrese de que la configuración de zona horaria de la máquina virtual host y la experiencia con Windows Server Essentials es los mismos. De lo contrario, pueden producirse varios errores. Entre ellos se incluyen: la configuración inicial del servidor no sea correcta en tareas relacionadas de certificado, el certificado no funcionen durante algunas horas después de instalar el rol experiencia con Windows Server Essentials, y no se actualizará la información del dispositivo correctamente.  
  
 Después de la implementación, use el cmdlet de Windows PowerShell **Get-WssConfigurationStatus** para comprobar si la configuración inicial se llevó a cabo correctamente. El estado devuelto debería ser uno de los siguientes: **Notstarted**, **FinishedWithWarning**, **Running**, **Finished**, **Failed** o **PendingReboot**.  
  
 El servidor se reiniciará durante la configuración inicial. Si necesita impedir el reinicio automático, puede utilizar el siguiente comando para agregar una clave de registro antes de la configuración inicial:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Una vez que empiece la configuración inicial, puede usar **Get-WssConfigurationStatus** para comprobar el estado de la configuración inicial. Cuando el estado sea **PendingReboot**, puede reiniciar el servidor.  
  
##  <a name="BKMK_Migrate"></a> Migrar datos desde Windows Small Business Server a Windows Server Essentials Experience  
 Puede migrar datos desde servidores que ejecuten Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 o Windows Server Essentials al servidor que ejecuta Windows Server Essentials. Revise el [migrar a Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) migración manual para 2migrations en el entorno local y realizar las personalizaciones necesarias en función de su entorno de hospedaje.  
  
> [!NOTE]
>  Se recomienda colocar el servidor de origen y el servidor de destino en la misma subred. Si no fuera posible, tenga en cuenta las siguientes precauciones:  
>   
>  -   El servidor de origen y el servidor de destino pueden tener acceso entre sí "™ s nombres DNS internos.  
> -   Todos los puertos necesarios están abiertos.  
  
 Después de la migración, puede actualizar sus licencias para quitar los bloqueos y límites. Para obtener más información, consulte [transición desde Windows Server Essentials a Windows Server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="BKMK_PowerShell"></a> Realizar tareas comunes mediante Windows PowerShell  
 Esta sección explica algunas de las tareas comunes que puede realizar mediante Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Habilitar el acceso Web remoto  
 **Sintaxis**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Ejemplo**:  
  
 $Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
  
 Este comando habilitará el acceso Web remoto con el enrutador configurado automáticamente y cambiará los permisos de acceso predeterminados de todos los usuarios existentes.  
  
### <a name="add-user"></a>Agregar usuario  
 **Sintaxis**:  
  
 Agregar-WssUser [-nombre] < cadena\> [-contraseña] < securestring\> [-AccessLevel < cadena\> {usuario &#124; administrador}] [-FirstName < cadena\>] [-LastName < cadena\>] [- AllowRemoteAccess] [-AllowVpnAccess] [< CommonParameters\>]  
  
 **Ejemplo**:  
  
 $password = ConvertTo-SecureString "Passw0rd!" -asplaintext œforce$ Add-WssUser-nombre User2Test-contraseña $password - Accesslevel administrador - FirstName usuario2 - LastName de la prueba  
  
 Este comando agregará un administrador llamado User2Test con contraseña Passw0rd!.  
  
### <a name="add-server-folder"></a>Agregar carpeta de servidor  
 **Sintaxis**:  
  
 Add-WssFolder [-Name] <string\> [-Path] <string\> [[-Description] <string\>] [-KeepPermissions] [<CommonParameters\>]  
  
 **Ejemplo**:  
  
 $Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
  
 Este comando agregará una carpeta de servidor denominada MyTestFolder en la ubicación especificada.  
  
##  <a name="BKMK_EmailIntegration"></a> Integración de correo electrónico con Windows Server Essentials  
 Puede integrar la experiencia con Windows Server Essentials con Office 365 o el servidor hospedado Exchange. Si quiere que el cliente use el correo electrónico hospedado, desarrolle un complemento para integrar la Experiencia de Windows Server Essentials con la solución de correo electrónico hospedada. Para obtener más información, consulte el [SDK de Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="BKMK_Monitoring"></a> Supervisar y administrar mediante las herramientas nativas  
 Esta sección describe las herramientas nativas que están disponibles en Windows Server 2012 R2 para supervisar y administrar el servidor.  
  
### <a name="group-policy"></a>Directiva de grupo  
  Experiencia con Windows Server Essentials aprovecha la compatibilidad nativa con la directiva de grupo en Windows Server 2012 R2 y proporciona una interfaz de usuario para configurar las opciones de seguridad y redirección de carpetas.  
  
> [!NOTE]
>  Si se habilita la redirección de carpetas para un perfil de usuario en un entorno hospedado, los usuarios finales podrían tardar más en iniciar sesión cuando el tamaño de los datos sea grande.  
  
### <a name="system-center-monitoring-pack"></a>Modulo de supervisión de System Center  
 Módulo de supervisión de System Center para los monitores de experiencia con Windows Server Essentials, el sistema de alerta de estado que le ayudarán a administrar un gran número de servidores que ejecutan Windows Server Essentials que se dedican a las pequeñas empresas. Para obtener más información, consulte [System Center Management Pack para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Copia de seguridad y restauración  
  Windows Server 2012 R2 con experiencia con Windows Server Essentials permite realizar una copia de seguridad de servidor y los equipos cliente de la red.  
  
#### <a name="server-backup"></a>Copia de seguridad del servidor  
 Windows Server Essentials admite dos formas de copia de seguridad del servidor: copia de seguridad local y no local. Puede personalizar estas opciones si desea implementar su propia solución de copia de seguridad del servidor.  
  
-   La**copia de seguridad local** le permite realizar copias de seguridad incrementales en el nivel de bloque de forma periódica en un disco aparte. Como proveedor de hospedaje, puede conectar un disco duro virtual a la máquina virtual que ejecuta Windows Server Essentials y, a continuación, configurar la copia de seguridad del servidor a este disco duro virtual. El disco duro virtual debe estar ubicado en un disco físico diferente al de la máquina virtual que ejecuta Windows Server Essentials.  
  
    > [!NOTE]
    >  Si tiene otras soluciones de copia de seguridad para las máquinas virtuales y no quiere que los usuarios vean la característica de copia de seguridad de servidor nativa de Windows Server Essentials, puede desactivarla y quitar la interfaz de usuario del panel. Para obtener más información, consulte la sección [Personalizar la copia de seguridad del servidor](https://technet.microsoft.com/library/dn293413.aspx) de [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   La**copia de seguridad no local** le permite realizar copias de seguridad periódica de los datos del servidor en un servicio de nube. Puede descargar e instalar el Microsoft Azure copia de seguridad de integración de módulo para Windows Server Essentials para aprovechar la copia de seguridad de Azure proporcionada por Microsoft.  
  
     Para obtener más información, consulte la integración de Windows Azure Backup con la sección de Windows Server Essentials en [Manage Server Backup](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Si usted o sus usuarios prefieren otro servicio de nube, debe:  
  
    -   Actualizar la interfaz de usuario del panel para que quede vinculada a su servicio de nube preferido en lugar de en el valor predeterminado de copia de seguridad de Azure.  
  
    -   (Opcional) Desarrollar un complemento para el panel que configure y administre el servicio de copia de seguridad en la nube.  
  
#### <a name="client-computer-backup"></a>Copia de seguridad del equipo cliente  
 La Experiencia con Windows Server Essentials admite dos tipos de copia de seguridad de datos de clientes: la copia de seguridad completa del cliente y la del Historial de archivos.  
  
> [!NOTE]
>  La realización de una copia de seguridad del cliente podría afectar al rendimiento dado que los datos se han de transferir del cliente al servidor a través de VPN.  
  
##### <a name="full-client-backup"></a>Copia de seguridad completa del cliente  
 La copia de seguridad completa del cliente está habilitada de forma predeterminada para todos los dispositivos de cliente que estén conectados a la red de Windows Server Essentials. Hace una copia de seguridad completa de la información del sistema y de los datos para el cliente y admite la desduplicación de datos. Los datos de copia de seguridad se almacenarán en el servidor que ejecuta Windows Server Essentials. Esto permite que un cliente recupere los datos desde un punto de copia de seguridad anterior en caso de error.  
  
 Algunos aspectos que deben tenerse en cuenta para la copia de seguridad completa del cliente:  
  
-   **Rendimiento** La copia de seguridad inicial del cliente podría llevar tiempo debido a la cantidad de datos que se van a cargar.  
  
-   **Estabilidad** En ocasiones la conexión a Internet no es estable en el lado del cliente. Copia de seguridad de cliente está diseñado para reanudarse automáticamente y la base de datos de copia de seguridad de cliente crea un punto de control cada vez que se copian 40 GB de datos de. Este valor se puede cambiar por un número más pequeño si cree que la conexión a Internet es inestable.  
  
    -   Para habilitar una tarea de punto de control: En el servidor, establezca la clave de registro de **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** a 1.  
  
    -   Para cambiar el umbral de punto de control: En el cliente, cambie **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** desde su valor predeterminado de 40 GB.  
  
-   **Restauración completa del cliente** Como el entorno de preinstalación de Windows no admite conexiones VPN, no es posible la restauración completa del cliente. Debe ocultar la tarea del Servicio de restauración de clientes siguiendo los pasos indicados en [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Historial de archivos  
 Historial de archivos es una característica de Windows 8.1 y Windows 8 para la copia de seguridad de datos de perfil (Bibliotecas, Escritorio, Contactos, Favoritos) en un recurso de red compartido. Puede administrar centralmente la configuración del Historial de archivos de todos los equipos que ejecutan Windows 8.1 o Windows 8 que se unan a la red de Windows Server Essentials. Los datos de la copia de seguridad se almacenan en el servidor que ejecuta Windows Server Essentials. Debe ocultar la tarea del Servicio de restauración de clientes siguiendo los pasos indicados en [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
### <a name="storage-management"></a>Administración del almacenamiento  
 Espacios de almacenamiento permite combinar la capacidad de almacenamiento físico de unidades de disco duro separadas, agregar unidades de disco duro de forma dinámica y crear volúmenes de datos con niveles específicos de resistencia. Puede hacerlo en el host o en la máquina virtual. Si desea ocultar esta característica en una máquina virtual que ejecute Windows Server Essentials, siga las instrucciones indicadas en [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_Scenarios"></a> Escenarios de prueba  
 En cuanto al hospedaje, se recomienda probar los siguientes escenarios:  
  

-   [Implementación de servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configuración del servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Administración del servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Experiencia del cliente](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a> Implementación de servidor  
 Puede probar los siguientes escenarios de implementación de servidor:  
  
-   Implementar un servidor que ejecuta Windows Server 2012 R2 como un controlador de dominio en el entorno de laboratorio y, a continuación, instale el rol experiencia con Windows Server Essentials.  
  
-   Implementar un servidor que ejecuta Windows Server 2012 R2 en el entorno de laboratorio, unir este servidor a un dominio existente y, a continuación, instalar el rol experiencia con Windows Server Essentials.  
  
-   Personalizar la imagen de Windows Server Essentials según sea necesario.  
  
-   Automatizar la implementación de Windows Server Essentials con Windows PowerShell y el archivo de instalación desatendida.  
  
-   Migrar servidores locales que ejecuten Windows Small Business Server a servidores hospedados que ejecutan Windows Server Essentials.  
  
###  <a name="BKMK_ServerConfig2"></a> Configuración del servidor  
 Puede probar los siguientes escenarios de configuración del servidor:  
  
-   Configurar el Acceso desde cualquier lugar (red privada virtual, el Acceso web remoto y DirectAccess).  
  
-   Configurar el almacenamiento y las carpetas del servidor.  
  
-   Activar BranchCache.  
  
-   (Si fuera necesario) Configurar la copia de seguridad del servidor, la copia de seguridad en línea, la copia de seguridad del cliente y el Historial de archivos.  
  
-   (Si es aplicable) Configurar y administrar Espacios de almacenamiento.  
  
-   (Si fuera necesario) Configurar la integración de soluciones de correo electrónico (Office 365 y Exchange Server hospedado).  
  
-   (Si fuera necesario) Configurar la integración con otros servicios en línea de Microsoft.  
  
-   (Si es aplicable) Configurar el servidor multimedia.  
  
###  <a name="BKMK_ServerManage"></a> Administración del servidor  
 Puede probar los siguientes escenarios de administración de servidor:  
  
-   Administrar usuarios y grupos.  
  
-   Configurar y recibir notificaciones de alertas por correo electrónico.  
  
-   Ejecutar el Analizador de procedimientos recomendados para ver si hay un error o un mensaje de advertencia.  
  
-   Configurar el módulo de administración de operaciones de System Center.  
  
-   Configurar la recuperación del servidor en caso de daño en el sistema operativo.  
  
###  <a name="BKMK_ClientXP"></a> Experiencia del cliente  
 Puede probar los siguientes escenarios de usuario final:  
  
-   Implementar los clientes a través de Internet (sistemas operativos PC o Mac).  
  
-   Utilizar Launchpad en el cliente para acceder a las carpetas compartidas.  
  
-   Acceder a los activos del servidor a través del Acceso web remoto y desde diferentes dispositivos (PC, teléfono, tableta).  
  
-   Acceder a la aplicación My Server para Windows Phone.  
  
-   (Si fuera necesario) Acceder al Historial de archivos, a la copia de seguridad y restauración de clientes y a la redirección de carpetas.  
  
-   (Si fuera necesario) Comprobar la experiencia de integración de correo electrónico.  
  
##  <a name="BKMK_Support"></a> Información de soporte técnico  
 Puede descargar el Kit de desarrollo de Software (SDK) de Windows Server Essentials y el Windows Server Essentials Assessment y Deployment Kit (ADK):  
  
-   [Kit de desarrollo de Software de Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Vea también  
  
-   [Novedades en Windows Server Essentials](../get-started/what-s-new.md)  

-   [Instalar Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Introducción a Windows Server Essentials](../get-started/get-started.md)
