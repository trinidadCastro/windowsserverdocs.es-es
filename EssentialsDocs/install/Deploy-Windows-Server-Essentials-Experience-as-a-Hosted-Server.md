---
title: Implementar Windows Server Essentials Experience como un servidor hospedado
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: c299d2b5f3d6b48693c473754a5205a7d26b5d6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Implementar Windows Server Essentials Experience como un servidor hospedado

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento incluye información que es específica de host pretende implementar 16 de servidor de Windows de Microsoft con el rol de Windows Server Essentials Experience (denominado Windows Server Essentials en el resto del documento) instalada en el laboratorio y va a ofrecer la experiencia con Windows Server Essentials como un servicio a sus clientes. Este documento incluye en las siguientes secciones:  
  

-   [Introducción a Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Ventajas de hospedaje de Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opciones de implementación admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologías de red admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar la imagen del rol de experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar la implementación de Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar datos desde Windows Small Business Server a Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Realizar tareas comunes mediante Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integración de correo electrónico con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Supervisar y administrar mediante herramientas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Escenarios de prueba](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Información de soporte técnico](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Introducción a Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Ventajas de hospedaje de Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opciones de implementación admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologías de red admitidas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar la imagen del rol de experiencia con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar la implementación de Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar datos desde Windows Small Business Server a Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Realizar tareas comunes mediante Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integración de correo electrónico con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Supervisar y administrar mediante herramientas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Escenarios de prueba](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Información de soporte técnico](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a>Introducción a Windows Server Essentials Experience  
 Windows Server Essentials Experience es un rol de servidor que está disponible en Windows Server 2012 R2 Standard y Windows Server 2012 R2 Datacenter. Cuando se instala el rol de experiencia con Windows Server Essentials en un servidor que ejecuta Windows Server 2012 R2, el cliente puede sacar partido de todas las características que están disponibles en Windows Server Essentials sin los bloqueos y los límites. Windows Server Essentials Experience habilita las siguientes soluciones entre local para pequeñas y medianas empresas:  
  
-   **Almacenamiento de datos y la protección** puede almacenar el cliente "¢ datos en una ubicación centralizada y proteger los datos de cliente y servidor de copia de seguridad de los equipos servidor y cliente (menos de 75) dentro de la red.  
  
-   **Administración de usuarios** puede administrar los usuarios y grupos a través del panel de servidor simplificado. Además, integración con Microsoft Azure Active Directory (AD Azure) permite el acceso de datos sea más fácil para los servicios en línea de Microsoft (por ejemplo, Office 365, Exchange Online y SharePoint Online) para los usuarios con sus credenciales de dominio.  
  
-   **Integración de servicios** puede integrar el servidor con Microsoft online services (como Office 365, SharePoint Online y copia de seguridad de Microsoft Azure). También puedes integrar el servidor con sus servicios o los servicios proporcionados por los proveedores terceros.  
  
-   **Acceso desde cualquier lugar** el cliente puede tener acceso a los servidores, equipos de la red y datos desde prácticamente cualquier lugar tienen una conexión a Internet y mediante el uso de casi cualquier dispositivo. Acceso Web remoto les permite tener acceso a aplicaciones y datos con una experiencia del navegador optimizadas para pantallas táctiles. La aplicación del servidor mi les permite acceder a datos de un Windows Phone o una aplicación de Microsoft Store.  
  
-   **Transmisión por secuencias de multimedia** si instala el paquete multimedia en un servidor con Windows Server Essentials Experience habilitado, el cliente final almacenar música, vídeo y fotos en las carpetas compartidas, después, puedes acceder a estos archivos multimedia desde equipos de la red o de acceso Web remoto.  
  
-   **Supervisión de estado** puede supervisar el estado de la red y obtener informes de estado personalizado.  
  
##  <a name="BKMK_Benefits"></a>Ventajas de hospedaje de Windows Server Essentials Experience  
  Windows Server Essentials Experience es un rol de servidor de Windows, para poder volver a la implementación existente y el marco de administración de Windows Server para implementar y configurar el rol de Windows Server Essentials Experience. Hospedar el rol de experiencia con Windows Server Essentials ofrece las siguientes ventajas:  
  
-   **Mejorada la implementación** activando simplemente el rol de Windows Server Essentials Experience, algunos de los más frecuente roles y características están encendidas y configuradas con los procedimientos recomendados para pequeñas y medianas empresas. Puedes personalizar las características de Windows Server Essentials, u ocultar algunas de las características de locales. Si usas el paquete de Windows Azure, puedes descargar la plantilla de la Galería de Windows Server Essentials Experience en Windows Server 2012 R2.  
  
-   **Panel simplificada** el panel de Windows Server Essentials simplifica las tareas comunes como la administración de carpetas del servidor, almacenamiento del servidor, copia de seguridad y restauración, usuario o cuentas de grupo, dispositivos, acceso remoto y correo electrónico. Pequeñas y medianas empresas pueden realizar tareas de administración de cada día en lugar de llamar el servicio de asistencia para obtener soporte técnico.  
  
-   **Extensibilidad** el panel de Windows Server Essentials y Windows Server Essentials Connector software son extensibles. Puedes agregar tu propia personalización de marca e integración, de servicios para que los clientes tengan un punto de entrada para todo acerca de su servidor y servicio.  
  
-   **Monitor** una nueva versión de System Center supervisión Pack está disponible para supervisar y administrar varios servidores que ejecutan Windows Server Essentials. Para descargar el paquete de administración, consulta [sistema Centro de administración de paquete para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="BKMK_SupportedDeployment"></a>Opciones de implementación admitidas  
  Windows Server Essentials Experience puede implementarse como un controlador de dominio en un nuevo entorno de Active Directory; o bien, se puede implementar en un entorno de Active Directory existente como un miembro de dominio.  
  
 Te recomendamos que primero implementar Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter y, a continuación, instala el rol de Windows Server Essentials Experience. Con este método de implementación, obtendrás todas las funciones de edición de Windows Server Essentials, sin los bloqueos y los límites.  
  

 Para obtener más información sobre la instalación de Windows Server 2012 R2 con el rol de Windows Server Essentials Experience, consulta [instalar y configurar Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="BKMK_SupportedToplogy"></a>Topologías de red admitidas  
 Para usar Windows Server Essentials Experience desde un cliente móvil, debe habilitarse la VPN. Para habilitar el acceso remoto al servidor de los clientes itinerantes, debes abrir el puerto 443 y el puerto 80 en el servidor.  
  
 Estas son las dos topologías típicas de red del servidor, y cómo se pueden configurar el acceso Web remoto y VPN:  
  
-   **Topología de 1** (Esto es la topología preferida y coloca todos los servidores y el intervalo de IP de VPN en la misma subred.):  
  
    -   Configurar el servidor en una red virtual independiente en un dispositivo de traducción de direcciones de red (NAT).  
  
    -   Habilitar el servicio DHCP en la red virtual o asignar una dirección IP estática del servidor.  
  
    -   Reenviar pública IP puerto 443 del enrutador a la dirección de red local del servidor.  
  
    -   Permitir VPN paso a través para el puerto 443.  
  
    -   Establecer el grupo de direcciones IPv4 de VPN en el mismo intervalo de subred que la dirección del servidor.  
  
    -   Asignar una dirección IP estática de la misma subred, que fuera el conjunto de direcciones VPN de los servidores de segundo.  
  
-   **Topología de 2**:  
  
    -   Asigna al servidor en una dirección IP privada.  
  
    -   Permitir el puerto 443 en el servidor para llegar a una dirección IP de pública puerto 443.  
  
    -   Permitir VPN paso a través para el puerto 443.  
  
    -   Asignar distintos intervalos del conjunto de direcciones IPv4 de VPN y la dirección del servidor.  
  
 Con la topología de 2, no se admiten escenarios de servidor segundos porque no se puede agregar otro servidor al mismo dominio.  
  
 Puedes habilitar VPN durante una instalación desatendida implementación mediante el script de PowerShell de Windows o puede configurarse con el Asistente para después de la configuración inicial.  
  
 Para habilitar la VPN mediante Windows PowerShell, ejecuta el siguiente comando con privilegios administrativos en el servidor que ejecuta Windows Server Essentials y proporcionar toda la información necesaria.  
  
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
>  Si no puede proporcionar una conexión VPN antes de que el cliente tome posesión del servidor, asegúrate de que el puerto del servidor 3389 es accesible a través de Internet para que el cliente puede usar el protocolo de escritorio remoto para conectarse al servidor y configurarlo.  
  
##  <a name="BKMK_CustomizeImage"></a>Personalizar la imagen del rol de experiencia con Windows Server Essentials  
 Puedes personalizar la imagen antes de configurar el rol de Windows Server Essentials Experience. Para obtener información sobre el proceso de Sysprep de servidor de Windows estándar, consulta [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx). Después de preparar la imagen mediante el uso de Sysprep, puedes usarla o reseal en Install.wim para una nueva implementación.  
  
 Si estás usando Virtual Machine Manager, puedes crear una plantilla mediante el uso de la instancia de ejecución. Este proceso usa Sysprep para preparar la instancia y se apaga el equipo. Después de la plantilla se almacena en la biblioteca, puedes usarlo en caso por caso.  
  
 Después de instalar el rol de Windows Server Essentials Experience, puedes personalizar las características de Windows Server Essentials. Una de las personalizaciones más importantes es la **IsHosted** clave del registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Si esta clave se establece en 0 x 1, algunas de las características de local cambiará el comportamiento. Estos cambios de características incluyen:  
  
-   **Copia de seguridad de cliente** copia de seguridad de cliente se desactivará de manera predeterminada para los equipos cliente recientemente unido.  
  
-   **Servicio de restauración de cliente** se deshabilitará el servicio de restauración de cliente y se ocultará la interfaz de usuario desde el panel.  
  
-   **Historial de archivos** configuración del historial de archivos para cuentas de usuario recién creado no se administrará automáticamente el servidor.  
  
-   **Copia de seguridad del servidor** se deshabilitará el servicio de copia de seguridad del servidor y se ocultará la interfaz de usuario de copia de seguridad del servidor desde el panel.  
  
-   **Espacios de almacenamiento** se ocultará la interfaz de usuario para crear o administrar espacios de almacenamiento desde el panel.  
  
-   **Cualquier acceso** configuración de enrutador y VPN se omitirá de manera predeterminada al ejecutar el desde cualquier lugar acceso asistente Configurar.  
  
 Si desea controlar el comportamiento de cada característica enumerado, puedes establecer la clave del registro correspondiente para cada uno de ellos. Para obtener información sobre cómo establecer la clave del registro, consulte la [personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a>Automatizar la implementación de Windows Server Essentials Experience  
 Para automatizar la implementación, deberás implementar el sistema operativo y, a continuación, instala el rol de Windows Server Essentials Experience.  
  
-   Para implementar automáticamente Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, sigue las instrucciones de [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Para obtener información sobre cómo instalar el rol de Windows Server Essentials Experience mediante Windows PowerShell, consulta [instalar y configurar Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Asegúrate de que la configuración de zona horaria de la máquina virtual de host y Windows Server Essentials Experience es el mismo. De lo contrario, puede experimentar varios errores. Estos incluyen: la configuración inicial del servidor no puede tener éxito en tareas relacionadas de certificados, el certificado puede no funcionar para unas pocas horas después de instala el rol de Windows Server Essentials Experience y no se actualizará correctamente la información del dispositivo.  
  
 Después de la implementación, usa el cmdlet de Windows PowerShell **Get WssConfigurationStatus** para comprobar si la configuración inicial se realizó correctamente. El estado devuelto debe ser una de las siguientes acciones: **no iniciado**, **FinishedWithWarning**, **ejecutando**, **terminado**, **Failed**, o **PendingReboot**.  
  
 Se reiniciará el servidor durante la configuración inicial. Si es necesario evitar el reinicio automático, puedes usar el siguiente comando para agregar una clave del registro antes de iniciar la configuración inicial:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Después de inicia la configuración inicial, puedes usar **Get WssConfigurationStatus** para comprobar el estado de la configuración inicial, y cuando el estado es **PendingReboot**, puede reiniciar el servidor.  
  
##  <a name="BKMK_Migrate"></a>Migrar datos desde Windows Small Business Server a Windows Server Essentials Experience  
 Puedes migrar datos desde servidores que ejecutan Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 o Windows Server Essentials al servidor que ejecuta Windows Server Essentials. Revisa la [migrar a Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) migración guía para 2migrations local y realizar personalizaciones necesarias en función de tu entorno de host.  
  
> [!NOTE]
>  Te recomendamos que poner el servidor de origen y el servidor de destino en la misma subred. Si esto no es posible, debes asegurarte:  
>   
>  -   El servidor de origen y el servidor de destino pueden acceder a ellos "¢ s nombres DNS internos.  
> -   Todos los puertos necesarios están abiertos.  
  
 Después de la migración, puedes actualizar sus licencias para quitar los bloqueos y los límites. Para obtener más información, consulta [transición desde Windows Server Essentials para Windows Server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="BKMK_PowerShell"></a>Realizar tareas comunes mediante Windows PowerShell  
 En esta sección se explica algunas de las tareas comunes que se pueden realizar mediante Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Habilitar el acceso Web remoto  
 **Sintaxis**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Ejemplo**:  
  
 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 Este comando habilitar el acceso Web remoto con el enrutador configurado automáticamente y cambiar los permisos de acceso de forma predeterminada para todos los usuarios existentes.  
  
### <a name="add-user"></a>Agregar usuario  
 **Sintaxis**:  
  
 Add-WssUser [-Nombre] < string\ > [-contraseña] < securestring\ > [-AccessLevel < string\ > {usuario & #124; Administrador}] [-FirstName < string\ >] [-LastName < string\ >] [-AllowRemoteAccess] [-AllowVpnAccess] [< CommonParameters\ >]  
  
 **Ejemplo**:  
  
 $password = ConvertTo-SecureString "Passw0rd!" -asplaintext œforce$ agregar-WssUser-User2Test nombre-contraseña $password - Accesslevel administrador - FirstName User2 - LastName probar  
  
 Este comando agrega un administrador denominado User2Test con contraseña Passw0rd!.  
  
### <a name="add-server-folder"></a>Agregar la carpeta del servidor  
 **Sintaxis**:  
  
 Add-WssFolder [-Nombre] < string\ > [: ruta de acceso] < string\ > [[-descripción] < string\ >] [-KeepPermissions] [< CommonParameters\ >]  
  
 **Ejemplo**:  
  
 $Agregar-WssFolder-el nombre "MyTestFolder"-"C:\ServerFolders\MyTestFolder" de la ruta de acceso  
  
 Este comando agrega una carpeta del servidor denominada MyTestFolder en la ubicación especificada.  
  
##  <a name="BKMK_EmailIntegration"></a>Integración de correo electrónico con Windows Server Essentials  
 Puedes integran Windows Server Essentials Experience con Office 365 u hospedan Exchange Server. Si quieres que tu cliente que usa tu correo electrónico hospedado, que necesitas para desarrollar un complemento para integrar Windows Server Essentials Experience con la solución de correo electrónico hospedadas. Para obtener más información, consulta [SDK de Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="BKMK_Monitoring"></a>Supervisar y administrar mediante herramientas nativas  
 Esta sección describe las herramientas nativas que están disponibles en Windows Server 2012 R2 para supervisar y administrar el servidor.  
  
### <a name="group-policy"></a>Directiva de grupo  
  Windows Server Essentials Experience aprovecha la compatibilidad nativa con la directiva de grupo en Windows Server 2012 R2 y proporciona una interfaz de usuario para establecer la configuración de seguridad y la redirección de carpetas.  
  
> [!NOTE]
>  En un entorno hospedado, si se habilita la redirección de carpetas de un perfil de usuario, puede aumentar el tiempo para el usuario final inicie sesión cuando el tamaño de los datos es grande.  
  
### <a name="system-center-monitoring-pack"></a>Paquete de supervisión de System Center  
 Paquete de supervisión de System Center para monitores de Windows Server Essentials Experience el sistema de alertas de estado que te ayudarán a administrar grandes cantidades de servidores que ejecutan Windows Server Essentials que son específicas de las organizaciones pequeñas empresas. Para obtener más información, consulta [sistema Centro de administración de paquete para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Copia de seguridad y restauración  
  Windows Server 2012 R2 con Windows Server Essentials Experience le permite hacer copia de seguridad de los equipos servidor y cliente en la red.  
  
#### <a name="server-backup"></a>Copia de seguridad del servidor  
 Windows Server Essentials admite dos métodos para hacer copia de seguridad del servidor: copia de seguridad local y copia de seguridad externo. Puedes personalizar estas opciones si quieres implementar tu propia solución de copia de seguridad del servidor.  
  
-   **Copia de seguridad local** te permite realizar copias de seguridad de nivel de bloque incrementales de forma periódica a un disco separado. Como un host, puede adjuntar un disco duro virtual a la máquina virtual que ejecutan Windows Server Essentials y, a continuación, configura la copia de seguridad de servidor en este disco duro virtual. El disco duro virtual debe estar ubicado en un disco físico diferente que la máquina virtual que ejecutan Windows Server Essentials.  
  
    > [!NOTE]
    >  Si tienes otras soluciones de copia de seguridad para las máquinas virtuales y no desea que los usuarios para ver la característica copia de seguridad de servidor nativa de Windows Server Essentials, puedes desactivarlo y quita la interfaz de usuario relacionados del panel. Para obtener más información, consulta el [personalizar copia de seguridad del servidor](https://technet.microsoft.com/library/dn293413.aspx) sección de [personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   **Copia de seguridad externo** permite realizar periódicamente una copia los datos del servidor a un servicio basado en la nube. Puede descargar e instalar el Microsoft Azure copia de seguridad de integración módulo para Windows Server Essentials para aprovechar la copia de seguridad de Azure que proporciona Microsoft.  
  
     Para obtener más información, consulta la integrar Windows Azure copia de seguridad con la sección de Windows Server Essentials en [administrar copias de seguridad del servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Si los usuarios prefiere otro servicio de nube, debes considerar lo siguiente:  
  
    -   Actualiza la interfaz de usuario del panel para que ofrece vínculos a tu servicio de nube preferido en lugar de en el valor predeterminado de copia de seguridad de Azure.  
  
    -   (Opcional) Desarrolla un complemento para el escritorio configurar y administrar el servicio de copia de seguridad en la nube.  
  
#### <a name="client-computer-backup"></a>Copia de seguridad de equipo cliente  
 Windows Server Essentials Experience admite dos tipos de copia de seguridad de datos de cliente: copia de seguridad de cliente y el historial de archivos completos.  
  
> [!NOTE]
>  Copia el cliente puede afectar al rendimiento porque los datos se transfieren desde el cliente al servidor a través de VPN.  
  
##### <a name="full-client-backup"></a>Copia de seguridad de cliente completa  
 De forma predeterminada para todos los dispositivos cliente que están conectados a la red de Windows Server Essentials, copia de seguridad de cliente completa está habilitada. Totalmente copia los datos y la información del sistema para el cliente y admite desduplicación de datos. Los datos de copia de seguridad, se almacenará en el servidor que ejecuta Windows Server Essentials. Esto permite que un cliente de error recuperar datos desde un punto de copia de seguridad anterior.  
  
 Algunas consideraciones para la copia de seguridad de cliente completa equipo son:  
  
-   **Rendimiento** copia de seguridad de cliente inicial podría llevar mucho tiempo debido a la cantidad de datos que se carguen.  
  
-   **Estabilidad** a veces no es estable en el lado del cliente de la conexión a Internet. Copia de seguridad de cliente está diseñado para reanudar automáticamente y la base de datos de copia de seguridad de cliente crea un punto de control cada vez que se copia 40 GB de datos. Puedes cambiar este valor a un número menor si esperas que la conexión a Internet no es confiable.  
  
    -   Para habilitar un trabajo de punto de control: en el servidor, Establece la clave del registro **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** en 1.  
  
    -   Para cambiar el umbral de punto de control: en el cliente, cambiar **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** desde su valor predeterminado de 40 GB.  
  
-   **Cliente recuperación** como el entorno de preinstalación de Windows no admite una conexión VPN, restauración de cliente reconstrucción completa no es compatible. Debes ocultar la tarea servicio de restauración de clientes siguiendo los pasos de [personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Historial de archivos  
 Historial de archivos es una característica de Windows 8.1 y Windows 8 para realizar copias de seguridad de los datos de perfil (bibliotecas, escritorio, contactos, Favoritos) en un recurso compartido de red. Puede administrar de forma centralizada la configuración del historial de archivos de todos los equipos que ejecutan Windows 8.1 o Windows 8 y que están unidos a la red de Windows Server Essentials. Los datos de copia de seguridad se almacenan en el servidor que ejecuta Windows Server Essentials. Debes ocultar la tarea servicio de restauración de clientes siguiendo los pasos de [personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>Administración de almacenamiento  
 Espacios de almacenamiento te permite agregar la capacidad de almacenamiento físico de diferentes unidades de disco duro, dinámicamente agregar unidades de disco duro y crear volúmenes de datos con los niveles especificados de resistencia. Puedes hacerlo en el equipo host o en la máquina virtual. Si quieres ocultar esta característica en una máquina virtual ejecuta Windows Server Essentials, sigue las instrucciones en [personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_Scenarios"></a>Escenarios de prueba  
 Desde la perspectiva de hospedaje, te recomendamos que pruebes los siguientes escenarios:  
  

-   [Implementación del servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configuración del servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Administración del servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Experiencia del cliente](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a>Implementación del servidor  
 Puedes probar los siguientes escenarios de implementación del servidor:  
  
-   Implementa un servidor que ejecuta Windows Server 2012 R2 como un controlador de dominio en el entorno de laboratorio y, a continuación, instala el rol de Windows Server Essentials Experience.  
  
-   Implementar un servidor que ejecuta Windows Server 2012 R2 en tu entorno de laboratorio, unirse a este servidor a un dominio existente y, a continuación, instala el rol de Windows Server Essentials Experience.  
  
-   Personaliza la imagen de Windows Server Essentials según sea necesario.  
  
-   Automatizar la implementación de Windows Server Essentials con archivos de instalación desatendida y Windows PowerShell.  
  
-   Migrar servidores locales que ejecutan Windows Small Business Server a hospedado servidores que ejecutan Windows Server Essentials.  
  
###  <a name="BKMK_ServerConfig2"></a>Configuración del servidor  
 Puedes probar los siguientes escenarios de configuración del servidor:  
  
-   Configurar acceso desde cualquier lugar (DirectAccess, acceso Web remoto y red privada virtual).  
  
-   Configurar el almacenamiento y carpetas del servidor.  
  
-   Activar BranchCache.  
  
-   (Si procede) Configurar copia de seguridad del servidor, la copia de seguridad en línea, la copia de seguridad de cliente y el historial de archivos.  
  
-   (Si procede) Configurar y administrar espacios de almacenamiento.  
  
-   (Si procede) Configurar la integración de solución de correo electrónico (Office 365 y hospedado Exchange Server).  
  
-   (Si procede) Configurar la integración con otros servicios en línea de Microsoft.  
  
-   (Si procede) Configurar el servidor multimedia.  
  
###  <a name="BKMK_ServerManage"></a>Administración del servidor  
 Puedes probar los siguientes escenarios de administración de servidor:  
  
-   Administrar los usuarios y grupos.  
  
-   Configurar y recibir correo electrónico de notificación de alertas.  
  
-   Ejecuta el analizador de procedimientos recomendados para ver si hay un error o un mensaje de advertencia.  
  
-   Configurar el paquete de System Center Operations Manager.  
  
-   Configurar la recuperación de servidor, en caso de los daños en el sistema operativo.  
  
###  <a name="BKMK_ClientXP"></a>Experiencia del cliente  
 Puedes probar los siguientes escenarios de usuario final:  
  
-   Implementar a los clientes a través de Internet (PC o Mac sistemas operativos).  
  
-   Launchpad de uso en el cliente para obtener acceso a carpetas compartidas.  
  
-   Activos de servidor de acceso a través de acceso Web remoto desde distintos dispositivos (PC, teléfono y tableta).  
  
-   Obtener acceso a mi aplicación de Server para Windows Phone.  
  
-   (Si procede) Historial de archivos de acceso, copia de seguridad de cliente y restauración y redirección de carpetas.  
  
-   (Si procede) Comprueba la experiencia de integración de correo electrónico.  
  
##  <a name="BKMK_Support"></a>Información de soporte técnico  
 Puedes descargar el Kit de desarrollo de Software (SDK) de Windows Server Essentials y la evaluación de Windows Server Essentials y Deployment Kit (ADK):  
  
-   [Kit de desarrollo de Software de Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [Personalizar e implementar Windows Server Essentials en Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Consulta también  
  
-   [Novedades en Windows Server Essentials](../get-started/what-s-new.md)  

-   [Instalar Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Introducción a Windows Server Essentials](../get-started/get-started.md)
