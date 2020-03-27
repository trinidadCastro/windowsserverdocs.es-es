---
title: Implementar Experiencia con Windows Server Essentials como servidor hospedado
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 519fdad7445426b5c4e4ef4d89903c029cea68d4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311813"
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Implementar Experiencia con Windows Server Essentials como servidor hospedado

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este documento se incluye información específica de los proveedores de hospedaje que desean implementar Microsoft Windows Server 16 con el rol de experiencia con Windows Server Essentials (conocido como Windows Server Essentials en el resto del documento) instalado en su laboratorio y pretende ofrecer la experiencia con Windows Server Essentials como servicio a sus clientes. En este documento se incluyen las siguientes secciones:  
  

-   [Información general sobre la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Ventajas de hospedar la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opciones de implementación admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologías de red admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar la imagen del rol de experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar la implementación de la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar datos de Windows Small Business Server a la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Realizar tareas comunes mediante Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integración de correo electrónico con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Supervisar y administrar mediante herramientas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Escenarios de prueba](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Información de soporte técnico](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Información general sobre la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Ventajas de hospedar la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opciones de implementación admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologías de red admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar la imagen del rol de experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar la implementación de la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar datos de Windows Small Business Server a la experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Realizar tareas comunes mediante Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integración de correo electrónico con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Supervisar y administrar mediante herramientas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Escenarios de prueba](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Información de soporte técnico](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="windows-server-essentials-experience-overview"></a><a name="BKMK_WSEEOverview"></a>Información general sobre la experiencia con Windows Server Essentials  
 La experiencia con Windows Server Essentials es un rol de servidor que está disponible en Windows Server 2012 R2 Standard y Windows Server 2012 R2 Datacenter. Cuando el rol de experiencia con Windows Server Essentials se instala en un servidor que ejecuta Windows Server 2012 R2, el cliente puede aprovechar todas las características que están disponibles en Windows Server Essentials sin los bloqueos y límites. La experiencia con Windows Server Essentials permite las siguientes soluciones entre locales para pequeñas y medianas empresas:  
  
-   **Almacenamiento y protección de datos** Puede almacenar los datos del cliente "¢ s en una ubicación centralizada y proteger los datos del servidor y del cliente mediante la copia de seguridad del servidor y los equipos cliente (menos de 75) dentro de la red.  
  
-   **Administración de usuarios** Puede administrar los usuarios y grupos mediante el panel de servidor simplificado. Además, la integración con Microsoft Azure Active Directory (Azure AD) permite un acceso sencillo a los datos para Microsoft servicios en línea (por ejemplo, Office 365, Exchange Online y SharePoint Online) para los usuarios mediante el uso de sus credenciales de dominio.  
  
-   **Integración de servicios** Puede integrar el servidor con Microsoft servicios en línea (como Office 365, SharePoint Online y Microsoft Azure Backup). También puede integrar el servidor con sus servicios o los servicios proporcionados por proveedores de terceros.  
  
-   **Acceso desde cualquier lugar** El cliente puede acceder a los servidores, equipos de la red y datos desde prácticamente cualquier lugar con una conexión a Internet y usando casi cualquier dispositivo. El Acceso web remoto les permite tener acceso a las aplicaciones y los datos con una experiencia optimizada y táctil a través de un explorador. La aplicación My Server les permite acceder a los datos desde un Windows Phone o una aplicación Microsoft Store.  
  
-   **Transmisión por secuencias de multimedia** Si instala el paquete de medios en un servidor con la experiencia con Windows Server Essentials habilitada, el cliente final puede almacenar música, vídeo y fotografías en carpetas compartidas y, a continuación, tener acceso a estos archivos multimedia desde equipos conectados en red o acceso Web remoto.  
  
-   **Supervisión del estado** Puede supervisar el estado de la red y obtener informes de estado personalizado.  
  
##  <a name="benefits-of-hosting-windows-server-essentials-experience"></a><a name="BKMK_Benefits"></a>Ventajas de hospedar la experiencia con Windows Server Essentials  
  La experiencia con Windows Server Essentials es un rol de Windows Server, por lo que puede volver a usar el marco de trabajo de implementación y administración existente en Windows Server para implementar y configurar el rol de experiencia con Windows Server Essentials. Hospedar el rol experiencia con Windows Server Essentials proporciona las siguientes ventajas:  
  
-   **Implementación simplificada** Con solo activar el rol experiencia con Windows Server Essentials, algunos de los roles y características más usados se activan y configuran con los procedimientos recomendados para pequeñas y medianas empresas. Puede personalizar las características de Windows Server Essentials u ocultar algunas de las características locales. Si usa la Windows Azure Pack, puede descargar la plantilla de la galería para la experiencia con Windows Server Essentials en Windows Server 2012 R2.  
  
-   **Panel simplificado** El panel de Windows Server Essentials simplifica las tareas comunes, como administrar carpetas de servidor, el almacenamiento del servidor, la copia de seguridad y la restauración, las cuentas de usuario o de grupo, los dispositivos, el acceso remoto y el correo electrónico. Las pequeñas y medianas empresas pueden realizar tareas de administración diarias en lugar de llamar al departamento de soporte técnico.  
  
-   **Extensibilidad** El panel y el software del Conector de Windows Server Essentials son extensibles. Puede agregar su propia marca y servicios de integración como punto de entrada para los clientes en todo lo relacionado con su servidor y su servicio.  
  
-   **Monitor** Está disponible una nueva versión del módulo de supervisión de System Center para supervisar y administrar varios servidores que ejecuten Windows Server Essentials. Para descargar el módulo de administración, consulte el [módulo de administración de System Center para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="supported-deployment-options"></a><a name="BKMK_SupportedDeployment"></a>Opciones de implementación admitidas  
  La experiencia con Windows Server Essentials se puede implementar como un controlador de dominio en un nuevo entorno de Active Directory; o bien, se puede implementar en un entorno de Active Directory existente como miembro del dominio.  
  
 Se recomienda implementar primero Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter y, a continuación, instalar el rol de experiencia con Windows Server Essentials. Con este método de implementación, obtendrá todas las funcionalidades de la edición de Windows Server Essentials, sin los bloqueos y límites.  
  

 Para obtener más información acerca de cómo instalar Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials, vea [instalar y configurar Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="supported-network-topologies"></a><a name="BKMK_SupportedToplogy"></a>Topologías de red admitidas  
 Para usar la experiencia con Windows Server Essentials desde un cliente móvil, debe estar habilitada la VPN. Para habilitar el acceso remoto al servidor de los clientes móviles, deberá abrir el puerto 443 y el puerto 80 en el servidor.  
  
 A continuación se describen dos topologías típicas de red de servidor y cómo se podrían configurar una VPN y el Acceso web remoto:  
  
- **Topología 1** (topología preferida que coloca todos los servidores y el intervalo IP de la VPN en la misma subred):  
  
  -   Configurar el servidor en una red virtual independiente en un dispositivo de traducción de direcciones de red (NAT).  
  
  -   Habilitar el servicio DHCP en la red virtual o asignar una dirección IP estática para el servidor.  
  
  -   Reenviar el puerto 443 de dirección IP pública en el enrutador a la dirección de red local del servidor.  
  
  -   Permitir el paso de VPN para el puerto 443.  
  
  -   Establecer el grupo de direcciones IPv4 de la VPN en el mismo intervalo de subred que la dirección del servidor.  
  
  -   Asignar a los servidores secundarios una dirección IP estática dentro de la misma subred, pero fuera del grupo de direcciones de VPN.  
  
- **Topología 2**:  
  
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
  
##  <a name="customize-the-image-of-windows-server-essentials-experience-role"></a><a name="BKMK_CustomizeImage"></a>Personalizar la imagen del rol de experiencia con Windows Server Essentials  
 Puede personalizar la imagen antes de configurar el rol experiencia con Windows Server Essentials. Para obtener más información sobre el proceso de Sysprep de Windows Server estándar, consulte [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx). Después de preparar la imagen mediante Sysprep, puede usarla o incluirla en Install.wim para una nueva implementación.  
  
 Si está utilizando Virtual Machine Manager, puede crear una plantilla mediante la instancia en ejecución. Este proceso utiliza Sysprep para preparar la instancia y apaga el equipo. Después de almacenar la plantilla en la biblioteca, puede usarla para casos específicos.  
  
 Después de instalar el rol de experiencia con Windows Server Essentials, puede personalizar las características de Windows Server Essentials. Una de las personalizaciones más importantes es la clave del Registro **IsHosted** : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Si se establece en 0x1, algunas de las características locales cambiarán de comportamiento. Entre los cambios de características, se incluyen los siguientes:  
  
- **Copia de seguridad de clientes** Las copias de seguridad de clientes se deshabilitarán de forma predeterminada para los equipos cliente que se hayan unido recientemente.  
  
- **Servicio de restauración de clientes** Se deshabilitará el Servicio de restauración de clientes y se ocultará la interfaz de usuario en el panel.  
  
- **Historial de archivos** El servidor no administrará automáticamente la configuración del Historial de archivos de las cuentas de usuario recién creadas.  
  
- **Copia de seguridad del servidor** Se deshabilitará el servicio de copia de seguridad del servidor y, a continuación, se ocultará la interfaz de usuario de Copias de seguridad del servidor en el panel.  
  
- **Espacios de almacenamiento** Se ocultará la interfaz de usuario para crear o administrar espacios de almacenamiento desde el panel.  
  
- **Acceso desde cualquier lugar** Se omitirá de forma predeterminada la configuración de VPN y del enrutador al ejecutar el asistente de configuración del Acceso desde cualquier lugar.  
  
  Si desea controlar el comportamiento de cada característica incluida, puede establecer la clave de registro correspondiente para cada una. Para obtener información sobre cómo configurar la clave del Registro, consulte [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="automate-the-deployment-of-windows-server-essentials-experience"></a><a name="BKMK_AutomateDeployment"></a>Automatizar la implementación de la experiencia con Windows Server Essentials  
 Para automatizar la implementación, debe implementar primero el sistema operativo y, a continuación, instalar el rol de experiencia con Windows Server Essentials.  
  
-   Para implementar automáticamente Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, siga las instrucciones de [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Para obtener información sobre cómo instalar el rol de experiencia con Windows Server Essentials mediante Windows PowerShell, consulte [instalar y configurar Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Asegúrese de que la configuración de zona horaria de la máquina virtual del host y la experiencia con Windows Server Essentials son las mismas. De lo contrario, pueden producirse varios errores. Estos incluyen: la configuración inicial del servidor puede no tener éxito en las tareas relacionadas con los certificados, puede que el certificado no funcione durante unas horas después de instalar el rol de experiencia con Windows Server Essentials y que la información del dispositivo no se actualice. manera.  
  
 Después de la implementación, use el cmdlet de Windows PowerShell **Get-WssConfigurationStatus** para comprobar si la configuración inicial se llevó a cabo correctamente. El estado devuelto debería ser uno de los siguientes: **Notstarted**, **FinishedWithWarning**, **Running**, **Finished**, **Failed**o **PendingReboot**.  
  
 El servidor se reiniciará durante la configuración inicial. Si necesita impedir el reinicio automático, puede utilizar el siguiente comando para agregar una clave de registro antes de la configuración inicial:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Una vez que empiece la configuración inicial, puede usar **Get-WssConfigurationStatus** para comprobar el estado de la configuración inicial. Cuando el estado sea **PendingReboot**, puede reiniciar el servidor.  
  
##  <a name="migrate-data-from-windows-small-business-server-to-windows-server-essentials-experience"></a><a name="BKMK_Migrate"></a>Migrar datos de Windows Small Business Server a la experiencia con Windows Server Essentials  
 Puede migrar datos de servidores que ejecuten Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 o Windows Server Essentials al servidor que ejecuta Windows Server Essentials. Revise la guía de migración de [migrar a Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) para 2migrations locales y realice las personalizaciones necesarias en función del entorno de hospedaje.  
  
> [!NOTE]
>  Se recomienda colocar el servidor de origen y el servidor de destino en la misma subred. Si no fuera posible, tenga en cuenta las siguientes precauciones:  
> 
> - El servidor de origen y el servidor de destino pueden tener acceso entre sí "¢ s nombres DNS internos.  
>   -   Todos los puertos necesarios están abiertos.  
  
 Después de la migración, puede actualizar sus licencias para quitar los bloqueos y límites. Para obtener más información, consulte [transición de Windows Server Essentials a Windows server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="perform-common-tasks-by-using-windows-powershell"></a><a name="BKMK_PowerShell"></a>Realizar tareas comunes mediante Windows PowerShell  
 Esta sección explica algunas de las tareas comunes que puede realizar mediante Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Habilitar el acceso Web remoto  
 **Sintaxis**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Ejemplo**:  
  
 $Enable WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 Este comando habilitará el acceso Web remoto con el enrutador configurado automáticamente y cambiará los permisos de acceso predeterminados de todos los usuarios existentes.  
  
### <a name="add-user"></a>Agregar usuario  
 **Sintaxis**:  
  
 Add-WssUser [-name] < cadena\> [-password] < SecureString\> [-AccessLevel < String\> {User &#124; Administrator}] [-FirstName < String\>] [-LastName < String\>] [-AllowRemoteAccess] [-AllowVpnAccess] [< CommonParameters\>]  
  
 **Ejemplo**:  
  
 $password = ConvertTo-SecureString "Passw0rd!" -asplaintext œforce $ Add-WssUser-Name User2Test-password $password-Accesslevel administrador-FirstName user2-LastName test  
  
 Este comando agregará un administrador denominado User2Test con la contraseña Passw0rd!.  
  
### <a name="add-server-folder"></a>Agregar carpeta de servidor  
 **Sintaxis**:  
  
 Add-WssFolder [-name] < cadena\> [-path] < cadena\> [[-Description] < cadena\>] [-KeepPermissions] [< CommonParameters\>]  
  
 **Ejemplo**:  
  
 $Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
  
 Este comando agregará una carpeta de servidor denominada MyTestFolder en la ubicación especificada.  
  
##  <a name="email-integration-with-windows-server-essentials"></a><a name="BKMK_EmailIntegration"></a>Integración de correo electrónico con Windows Server Essentials  
 Puede integrar la experiencia con Windows Server Essentials con Office 365 o Exchange Server hospedado. Si quiere que el cliente use el correo electrónico hospedado, desarrolle un complemento para integrar la Experiencia de Windows Server Essentials con la solución de correo electrónico hospedada. Para obtener más información, consulte el [SDK de Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="monitor-and-manage-by-using-native-tools"></a><a name="BKMK_Monitoring"></a>Supervisar y administrar mediante herramientas nativas  
 En esta sección se describen las herramientas nativas que están disponibles en Windows Server 2012 R2 para supervisar y administrar el servidor.  
  
### <a name="group-policy"></a>Directiva de grupo  
  La experiencia con Windows Server Essentials aprovecha la compatibilidad con directiva de grupo nativa en Windows Server 2012 R2 y proporciona una interfaz de usuario para configurar la redirección de carpetas y la configuración de seguridad.  
  
> [!NOTE]
>  Si se habilita la redirección de carpetas para un perfil de usuario en un entorno hospedado, los usuarios finales podrían tardar más en iniciar sesión cuando el tamaño de los datos sea grande.  
  
### <a name="system-center-monitoring-pack"></a>Modulo de supervisión de System Center  
 El módulo de supervisión de System Center para la experiencia con Windows Server Essentials supervisa el sistema de alertas de estado para ayudarle a administrar un gran número de servidores que ejecutan Windows Server Essentials y que están dedicados a organizaciones de pequeñas empresas. Para obtener más información, consulte el [módulo de administración de System Center para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Copia de seguridad y restauración  
  Windows Server 2012 R2 con experiencia con Windows Server Essentials le permite realizar copias de seguridad de equipos cliente y servidores de la red.  
  
#### <a name="server-backup"></a>Copia de seguridad del servidor  
 Windows Server Essentials admite dos formas de copia de seguridad del servidor: copia de seguridad local y no local. Puede personalizar estas opciones si desea implementar su propia solución de copia de seguridad del servidor.  
  
-   La**copia de seguridad local** le permite realizar copias de seguridad incrementales en el nivel de bloque de forma periódica en un disco aparte. Como proveedor de hospedaje, puede conectar un disco duro virtual a la máquina virtual que ejecuta Windows Server Essentials y, a continuación, configurar la copia de seguridad del servidor a este disco duro virtual. El disco duro virtual debe estar ubicado en un disco físico diferente al de la máquina virtual que ejecuta Windows Server Essentials.  
  
    > [!NOTE]
    >  Si tiene otras soluciones de copia de seguridad para las máquinas virtuales y no quiere que los usuarios vean la característica de copia de seguridad de servidor nativa de Windows Server Essentials, puede desactivarla y quitar la interfaz de usuario del panel. Para obtener más información, consulte la sección [Personalizar la copia de seguridad del servidor](https://technet.microsoft.com/library/dn293413.aspx) de [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   La**copia de seguridad no local** le permite realizar copias de seguridad periódica de los datos del servidor en un servicio de nube. Puede descargar e instalar el módulo de integración de Microsoft Azure Backup para Windows Server Essentials para aprovechar los Azure Backup proporcionados por Microsoft.  
  
     Para obtener más información, consulte la sección integrar Windows Azure Backup con Windows Server Essentials en [administrar copias de seguridad del servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Si usted o sus usuarios prefieren otro servicio de nube, debe:  
  
    -   Actualice la interfaz de usuario del panel para que se vincule a su servicio en la nube preferido en lugar de a la Azure Backup predeterminada.  
  
    -   (Opcional) Desarrollar un complemento para el panel que configure y administre el servicio de copia de seguridad en la nube.  
  
#### <a name="client-computer-backup"></a>Copia de seguridad del equipo cliente  
 La Experiencia con Windows Server Essentials admite dos tipos de copia de seguridad de datos de clientes: la copia de seguridad completa del cliente y la del Historial de archivos.  
  
> [!NOTE]
>  La realización de una copia de seguridad del cliente podría afectar al rendimiento dado que los datos se han de transferir del cliente al servidor a través de VPN.  
  
##### <a name="full-client-backup"></a>Copia de seguridad completa del cliente  
 La copia de seguridad completa del cliente está habilitada de forma predeterminada para todos los dispositivos de cliente que estén conectados a la red de Windows Server Essentials. Hace una copia de seguridad completa de la información del sistema y de los datos para el cliente y admite la desduplicación de datos. Los datos de copia de seguridad se almacenarán en el servidor que ejecuta Windows Server Essentials. Esto permite que un cliente recupere los datos desde un punto de copia de seguridad anterior en caso de error.  
  
 Algunos aspectos que deben tenerse en cuenta para la copia de seguridad completa del cliente:  
  
-   **Rendimiento** La copia de seguridad inicial del cliente podría llevar tiempo debido a la cantidad de datos que se van a cargar.  
  
-   **Estabilidad** En ocasiones la conexión a Internet no es estable en el lado del cliente. La copia de seguridad del cliente se ha diseñado para reanudarse automáticamente y la base de datos de copias de seguridad del cliente crea un punto de control cada vez que se hace una copia de seguridad de 40 GB. Este valor se puede cambiar por un número más pequeño si cree que la conexión a Internet es inestable.  
  
    -   Para habilitar una tarea de punto de control: En el servidor, establezca la clave de registro de **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** a 1.  
  
    -   Para cambiar el umbral de punto de control: En el cliente, cambie el valor predeterminado de **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** a 40 GB.  
  
-   **Restauración completa del cliente** Como el entorno de preinstalación de Windows no admite conexiones VPN, no es posible la restauración completa del cliente. Debe ocultar la tarea del Servicio de restauración de clientes siguiendo los pasos indicados en [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Historial de archivos  
 Historial de archivos es una característica de Windows 8.1 y Windows 8 para la copia de seguridad de datos de perfil (Bibliotecas, Escritorio, Contactos, Favoritos) en un recurso de red compartido. Puede administrar centralmente la configuración del Historial de archivos de todos los equipos que ejecutan Windows 8.1 o Windows 8 que se unan a la red de Windows Server Essentials. Los datos de la copia de seguridad se almacenan en el servidor que ejecuta Windows Server Essentials. Debe ocultar la tarea del Servicio de restauración de clientes siguiendo los pasos indicados en [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
### <a name="storage-management"></a>Administración del almacenamiento  
 Espacios de almacenamiento permite combinar la capacidad de almacenamiento físico de unidades de disco duro separadas, agregar unidades de disco duro de forma dinámica y crear volúmenes de datos con niveles específicos de resistencia. Puede hacerlo en el host o en la máquina virtual. Si desea ocultar esta característica en una máquina virtual que ejecute Windows Server Essentials, siga las instrucciones indicadas en [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="test-scenarios"></a><a name="BKMK_Scenarios"></a>Escenarios de prueba  
 En cuanto al hospedaje, se recomienda probar los siguientes escenarios:  
  

-   [Implementación del servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configuración del servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Administración de servidores](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Experiencia del cliente](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="server-deployment"></a><a name="BKMK_ServerDeploy"></a>Implementación del servidor  
 Puede probar los siguientes escenarios de implementación de servidor:  
  
-   Implemente un servidor que ejecute Windows Server 2012 R2 como controlador de dominio en el entorno de laboratorio y, a continuación, instale el rol de experiencia con Windows Server Essentials.  
  
-   Implementar un servidor que ejecute Windows Server 2012 R2 en el entorno de laboratorio, unir este servidor a un dominio existente y, a continuación, instalar el rol de experiencia con Windows Server Essentials.  
  
-   Personalizar la imagen de Windows Server Essentials según sea necesario.  
  
-   Automatizar la implementación de Windows Server Essentials con Windows PowerShell y el archivo de instalación desatendida.  
  
-   Migrar servidores locales que ejecuten Windows Small Business Server a servidores hospedados que ejecutan Windows Server Essentials.  
  
###  <a name="server-configuration"></a><a name="BKMK_ServerConfig2"></a>Configuración del servidor  
 Puede probar los siguientes escenarios de configuración del servidor:  
  
-   Configurar el Acceso desde cualquier lugar (red privada virtual, el Acceso web remoto y DirectAccess).  
  
-   Configurar el almacenamiento y las carpetas del servidor.  
  
-   Activar BranchCache.  
  
-   (Si fuera necesario) Configurar la copia de seguridad del servidor, la copia de seguridad en línea, la copia de seguridad del cliente y el Historial de archivos.  
  
-   (Si es aplicable) Configurar y administrar Espacios de almacenamiento.  
  
-   (Si fuera necesario) Configurar la integración de soluciones de correo electrónico (Office 365 y Exchange Server hospedado).  
  
-   (Si fuera necesario) Configurar la integración con otros servicios en línea de Microsoft.  
  
-   (Si es aplicable) Configurar el servidor multimedia.  
  
###  <a name="server-management"></a><a name="BKMK_ServerManage"></a>Administración de servidores  
 Puede probar los siguientes escenarios de administración de servidor:  
  
-   Administrar usuarios y grupos.  
  
-   Configurar y recibir notificaciones de alertas por correo electrónico.  
  
-   Ejecutar el Analizador de procedimientos recomendados para ver si hay un error o un mensaje de advertencia.  
  
-   Configurar el módulo de administración de operaciones de System Center.  
  
-   Configurar la recuperación del servidor en caso de daño en el sistema operativo.  
  
###  <a name="client-experience"></a><a name="BKMK_ClientXP"></a>Experiencia del cliente  
 Puede probar los siguientes escenarios de usuario final:  
  
-   Implementar los clientes a través de Internet (sistemas operativos PC o Mac).  
  
-   Utilizar Launchpad en el cliente para acceder a las carpetas compartidas.  
  
-   Acceder a los activos del servidor a través del Acceso web remoto y desde diferentes dispositivos (PC, teléfono, tableta).  
  
-   Acceder a la aplicación My Server para Windows Phone.  
  
-   (Si fuera necesario) Acceder al Historial de archivos, a la copia de seguridad y restauración de clientes y a la redirección de carpetas.  
  
-   (Si fuera necesario) Comprobar la experiencia de integración de correo electrónico.  
  
##  <a name="support-information"></a><a name="BKMK_Support"></a>Información de soporte técnico  
 Puede descargar el kit de desarrollo de software (SDK) de Windows Server Essentials y Windows Server Essentials Assessment and Deployment Kit (ADK):  
  
-   [Kit de desarrollo de software de Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx) SKD  
  
-   [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Vea también  
  
-   [Novedades de Windows Server Essentials](../get-started/what-s-new.md)  

-   [Instalación de Windows Server 2012 Essentials](Install-Windows-Server-Essentials.md)  

-   [Introducción a Windows Server Essentials](../get-started/get-started.md)
