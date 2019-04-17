---
title: Integrar un servidor de Exchange local con Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2020e08b94800a9750f095a2f772afb14ba5f0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Integrar un servidor de Exchange local con Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta guía proporciona información e instrucciones básicas que te ayudarán a configurar e integrar un servidor local que ejecuta Exchange Server con un servidor que ejecuta Windows Server Essentials.  
  
 Debes leer a esta guía antes de intentar implementar un servidor local que ejecuta Exchange Server en una red de Windows Server Essentials.  
  
> [!NOTE]
>  Exchange Server 2010 no admite la instalación en equipos que ejecutan Windows Server 2012.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Antes de instalar Exchange Server en una red de Windows Server Essentials, asegúrate de completar las tareas detalladas en esta sección.  
  
-   [Configurar un servidor que ejecuta Windows Server Essentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  
  
-   [Preparar un segundo servidor en el que se va a instalar el servidor Exchange](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  
  
-   [Configurar el nombre de dominio de Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  
  
###  <a name="BKMK_SetUpSBS8"></a>Configurar un servidor que ejecuta Windows Server Essentials  
 Debe haber configurar un servidor que ejecuta Windows Server Essentials. Esto hará que el controlador de dominio del servidor que ejecuta Exchange Server. Para obtener información sobre cómo configurar Windows Server Essentials, consulte [instalar Windows Server Essentials](../install/Install-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecondServer"></a>Preparar un segundo servidor en el que se va a instalar el servidor Exchange  
 Debes instalar a Exchange Server en un segundo servidor que ejecuta una versión del sistema operativo Windows Server que admita oficialmente ejecuta Exchange Server 2010 o Exchange Server 2013. El segundo servidor, a continuación, debes unir al dominio de Windows Server Essentials.  
  
 Para obtener información acerca de cómo unirse a un segundo servidor en el dominio de Windows Server Essentials, consulte unirse a un segundo servidor a la red en [conectarse](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Microsoft no admite la instalación de Exchange Server en un servidor que ejecuta Windows Server Essentials.  
  
###  <a name="BKMK_DomainNames"></a>Configurar el nombre de dominio de Internet  
 Para integrar un servidor local que ejecuta Exchange Server con Windows Server Essentials, debe ha registrado un nombre de dominio de Internet válido para tu empresa (como *contoso.com*). También debe trabajar con su proveedor de nombre de dominio para crear los recursos de DNS que requiere el servidor Exchange.  
  
 Por ejemplo, si el nombre de dominio de Internet de tu empresa es contoso.com y quieres usar el nombre de dominio completo (FQDN) de *mail.contoso.com* para hacer referencia a su servidor local que ejecuta Exchange Server, trabajar con su proveedor de nombre de dominio para crear los registros de recursos DNS en la siguiente tabla.  
  
|Nombre de registro de recurso|Tipo de registro|Configuración de registro|Descripción|  
|--------------------------|-----------------|--------------------|-----------------|  
|correo|Host (A)|Dirección =*dirección IP pública asignada por el ISP*|Exchange Server recibirá mensajes remitidos a mail.contoso.com.<br /><br /> Puedes usar un nombre diferente en tu propia selección.|  
|MX|Intercambio de correo (MX)|Nombre de host = @<br /><br /> Dirección = mail.contoso.com<br /><br /> Preferencia = 0|Proporciona enrutamiento de mensajes de correo electrónico para email@contoso.comque llegue a tu servidor local que ejecuta Exchange Server.|  
|SPF|Texto (TXT)|V = spf1 un mx ~ todos|Registro de recursos que te ayuda a impedir el correo electrónico enviado desde el servidor como identificarse como correo no deseado.|  
|Autodiscover._tcp|Servicio (SRV)|Servicio: _autodiscover<br /><br /> Protocolo: _tcp<br /><br /> Prioridad: 0<br /><br /> Peso: 0<br /><br /> Puerto: 443<br /><br /> Host de destino: mail.contoso.com|Permite a Microsoft Office Outlook y dispositivos móviles detectar automáticamente el servidor local que ejecuta Exchange Server.<br /><br /> **Nota:** también puede configurar un registro de recursos de host (A) de detección automática y apunta el registro a la dirección IP pública del servidor local que ejecuta Exchange Server. Sin embargo, si implementas esta opción, también debes proporcionar certificado SSL de asunto nombre alternativo (SAN) que admita mail.contoso.com y autodiscover.contoso.com nombres de dominio.|  
  
> [!NOTE]
>  -   Reemplazar instancias de *contoso.com* en este ejemplo con el nombre de dominio de Internet que registrado.  
  
 Debes elegir un FQDN diferente de tu servidor local que ejecuta Exchange Server que el FQDN que usas para el servidor que ejecuta Windows Server Essentials. Por ejemplo, puedes elegir usar *remote.contoso.com* como el FQDN que usan los equipos para tener acceso al servidor que ejecuta Windows Server Essentials desde Internet. Puedes usar *mail.contoso.com* como el FQDN en el que se usa para enrutar el correo electrónico a tu servidor local que ejecuta Exchange Server.  
  
## <a name="install-exchange-server"></a>Instalar el servidor Exchange  
 La característica de integración de Exchange Server en Windows Server Essentials admite las siguientes versiones de Exchange Server:  
  
-   Exchange Server 2013  
  
-   Exchange Server 2010 con Service Pack 1 (SP1)  
  
 Antes de instalar el servidor Exchange en el segundo servidor, primero debes agregar la cuenta de administrador actual a la **administradores** grupo.  
  
#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Para agregar la cuenta de administrador actual al grupo de administradores de empresa  
  
1.  Iniciar sesión en Windows Server Essentials como administrador.  
  
2.  Ejecutar Windows PowerShell como administrador.  
  
3.  En el símbolo del sistema de Windows PowerShell, escribe **agregar ADGroupMember ˜Enterprise administradores $env: nombre de usuario**, y, a continuación, presione ENTRAR.  
  
#### <a name="to-install-exchange-server"></a>Para instalar el servidor Exchange  
  
1.  Iniciar sesión en el segundo servidor como administrador.  
  
2.  Abre el Explorador de Internet y luego navegar a la [Asistente para la implementación del servidor Exchange](https://go.microsoft.com/fwlink/p/?LinkID=249163) sitio Web.  
  
3.  Haz clic en **local solo**.  
  
4.  Haz clic en la nueva opción de instalación para la versión del servidor Exchange que vayas a instalar.  
  
    > [!NOTE]
    >  Si vas a migrar desde una instalación de Windows Small Business Server, debe seleccionar la opción adecuada actualización que se describe los pasos de migración.  
  
5.  En la página siguiente, acepta la configuración predeterminada y, a continuación, haz clic en **siguiente**.  
  
    > [!NOTE]
    >  Si tienes previsto usar carpetas públicas en la nueva instalación de Exchange Server, cambiar esa configuración para **Sí**.  
  
6.  Sigue las instrucciones paso a paso en la lista de comprobación para implementar el servidor Exchange.  
  
     El Asistente para la implementación del servidor Exchange también te permite:  
  
    -   Imprimir una copia de la lista de comprobación.  
  
    -   Enviar una copia de la lista de comprobación a un destinatario de correo electrónico.  
  
    -   Descarga la lista de comprobación como un archivo PDF.  
  
> [!NOTE]
>  -   Siempre debes elegir instalar las herramientas de administración en el servidor que ejecuta Exchange Server. Las herramientas de administración son necesarios para la característica de integración de Exchange Server en Windows Server Essentials.  
> -   Si necesitas configurar directorios virtuales, te recomendamos que también establezcas el **InternalUrl** propiedad sea la misma dirección URL como el **ExternalUrl** propiedad para cada directorio virtual. Para obtener más información, consulta [administrar cliente acceso a directorios virtuales Server](https://go.microsoft.com/fwlink/p/?LinkId=251058) en el sitio Web de ayuda en línea de Exchange Server 2010.  
> -   Si quieres acceder a Outlook Web Access (OWA) desde dentro del sitio acceso Web remoto en Windows Server Essentials, debes establecer la propiedad de dirección URL para OWA.  
  
 Si vas a instalar Exchange Server 2010 en una instalación limpia, también puedes usar los siguientes scripts para configurar el servidor Exchange.  
  
#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Usar scripts para configurar el servidor Exchange  
  
1.  Abre el Bloc de notas y pega el siguiente script en un archivo nuevo:  
  
     `Import-Module ServerManager`  
  
     `Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  �Restart`  
  
2.  Guarda el archivo como **InstallDependencies.ps1**.  
  
3.  Copia el certificado SSL de Exchange en una ubicación en el servidor.  
  
4.  Abre un nuevo archivo del Bloc de notas y copia el texto siguiente al archivo:  
  
     `param (`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]`  
  
     `$CertPath = "c:\certificates\ExchangeCertificate.pfx",`  
  
     `[Security.SecureString]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]`  
  
     `$CertPassword = $null,`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]`  
  
     `$DomainName = "contoso.com",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]`  
  
     `$ServerIpAddress = "192.168.0.1",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]`  
  
     `$InternalIpRange = "192.168.0.0-192.168.0.255"`  
  
     `)`  
  
     `#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.`  
  
     `Import-ExchangeCertificate  �FileData ([Byte[]]$(Get-content -Path $CertPath  �Encoding byte  �ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force`  
  
     `#New AcceptedDomain and set it to default`  
  
     `New-AcceptedDomain  �Name "official name"  �DomainName $domainname`  
  
     `Set-AcceptedDomain  �Identity "official name"  �MakeDefault $true`  
  
     `#New EmailAddress Policy`  
  
     `$address = "%m@" + $DomainName`  
  
     `New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address`  
  
     `#Set owa and ecp VirtualDirectory ExternalUrl`  
  
     `$hostname = "mail." + $DomainName`  
  
     `$owa = "https://" + $hostname + "/owa"`  
  
     `$ecp = "https://" + $hostname + "/ecp"`  
  
     `$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"`  
  
     `$oab = "https://" + $hostname + "/OAB"`  
  
     `$ews = "https://" + $hostname + "/EWS/Exchange.asmx"`  
  
     `Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  �ExternalUrl $owa  �InternalUrl $owa`  
  
     `Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  �ExternalUrl $ecp  �InternalUrl $ecp`  
  
     `Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  �InternalUrl $activesync`  
  
     `Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true`  
  
     `Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force`  
  
     `#Enable outlook Anywhere`  
  
     `Enable-OutlookAnywhere  �ClientAuthenticationMethod:Basic  �ExternalHostname:$hostname  �SSLOffloading:$false`  
  
     `#new receive/send connector`  
  
     `$machinename = get-content env:computername`  
  
     `$bindingIpaddress = $ServerIpAddress + ":25"`  
  
     `$RecevieConnectorName = $machinename + "\Default " + $machinename`  
  
     `Set-ReceiveConnector $RecevieConnectorName -RemoteIPRanges $InternalIpRange`  
  
     `New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated`  
  
     `New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename`  
  
5.  Establecer los parámetros del principio del script para reflejar el entorno de red.  
  
6.  Guarda el archivo como **ConfigureExchange.ps1**.  
  
7.  Ejecutar Windows PowerShell como administrador.  
  
8.  En el símbolo del sistema de Windows PowerShell, escribe **Set-ExecutionPolicy RemoteSigned**, y, a continuación, presione ENTRAR.  
  
9. Ejecutar el script **InstallDependencies.ps1**.  
  
10. Reinicia el servidor y, a continuación, ejecutar Windows PowerShell como administrador.  
  
11. En el símbolo del sistema de Windows PowerShell, ejecuta el siguiente script:  
  
     `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  
  
    > [!NOTE]
    >  Asegúrate de escribir la ruta de acceso correcta para el programa de instalación de Exchange Server.  
  
12. Una vez completado el programa de instalación de Exchange Server, abre el Shell de administración de Exchange como administrador.  
  
13. En el símbolo del sistema de Shell de administración de Exchange, escribe **Set-ExecutionPolicy RemoteSigned**, y, a continuación, presione ENTRAR.  
  
14. Ejecutar el script **ConfigureExchange.ps1**.  
  
15. Reiniciar el servidor.  
  
> [!NOTE]
>  Si decides usar un certificado de SSL públicamente confianza en lugar de un certificado emitido automáticamente, puedes seguir las instrucciones que aparecen en la Guía de instalación para crear una solicitud de certificado y enviarla a la entidad de certificación seleccionado. También puedes usar un cmdlet de PowerShell de Exchange para crear una solicitud de certificado. A continuación se muestra un ejemplo.  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personalizar los parámetros de script para reflejar el entorno de red.  
  
## <a name="post-installation-tasks"></a>Tareas posteriores a Post-installation  
 Esta sección describe las tareas de configuración del servidor que debe llevar a cabo en la fase después de la instalación que contiene información específica para configurar un servidor local que ejecuta Exchange Server en una red de Windows Server Essentials.  
  
### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Agregar el dominio de correo electrónico pública y configurar las directivas de la dirección de correo electrónico  
  
> [!NOTE]
>  Esta es una tarea necesaria si estás realizando una instalación limpia. Omitir este paso si vas a migrar de Windows Small Business Server.  
  
 Debes especificar el dominio de correo electrónico para que sea el dominio aceptado predeterminado y, a continuación, configura la directiva de la dirección de correo electrónico.  
  
##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Para agregar tu dominio de correo electrónico como el valor predeterminado de dominio aceptada  
  
1.  Sigue las instrucciones en el artículo de Exchange Server [crear un dominio aceptan](https://go.microsoft.com/fwlink/p/?LinkId=249174) para agregar un dominio aceptado.  
  
2.  Iniciar sesión en el segundo servidor como administrador, abre la consola de administración de Exchange y luego navegar a la **transporte de concentradores** ficha de la **configuración de la organización**.  
  
3.  En el panel de trabajo de la consola de administración de Exchange, haz clic en el dominio aceptado nuevo y, a continuación, haz clic en **establecer como predeterminado**.  
  
4.  Sigue las instrucciones en el artículo de Exchange Server [crear una directiva de la dirección de correo electrónico](https://go.microsoft.com/fwlink/p/?LinkId=249179) para crear una nueva directiva de dirección de correo electrónico. Puedes aceptar todos los valores predeterminados, excepto la dirección de correo electrónico. Dirección de correo electrónico, especifica tu dominio de correo electrónico pública.  
  
### <a name="create-smtp-send-and-receive-connectors"></a>Crear SMTP enviar y recibir conectores  
  
> [!NOTE]
>  Se trata de una tarea necesaria.  
  
 Debes configurar un conector de envío SMTP y un conector de recepción SMTP para la transmisión entrante o saliente de mensajes de correo electrónico.  
  
 Para crear un conector de envío SMTP, sigue las instrucciones en el artículo de Exchange Server [crear un conector de envío SMTP](https://technet.microsoft.com/library/aa997285.aspx).  
  
 Para crear un conector de recepción SMTP, sigue las instrucciones en el artículo de Exchange Server [crear un conector de recibir SMTP](https://technet.microsoft.com/library/bb125159.aspx).  
  
 Como una opción, puede hacer referencia a la secuencia de comandos anteriormente en este documento para crear el envío y conectores de recepción mediante los cmdlets de PowerShell de Exchange.  
  
### <a name="configure-the-network-router"></a>Configurar el enrutador de red  
  
> [!NOTE]
>  Esta es una tarea necesaria si estás realizando una instalación limpia. Si vas a migrar de Windows Small Business Server, consulta [migrar datos del servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) para obtener instrucciones sobre cómo configurar la red.  
  
 Como mínimo, debes configurar la siguiente configuración de puerto en el enrutador:  
  
|Puerto del enrutador|Destino IP|Puerto de destino|Ten en cuenta|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|IP interna del servidor local que ejecuta Exchange Server.|25||  
|80 (HTTP)|IP interna del servidor que ejecuta Windows Server Essentials|80||  
|443 (HTTPS)|IP interna del servidor que ejecuta Windows Server Essentials|443||  
  
 Si admites la POP3 o IMAP protocolos en la red de mensajería, también debes configurar forwardings de puerto para estos protocolos. For-related información, consulta la sección **servidores de acceso de cliente** en el tema [referencia de puerto de red de Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) en la biblioteca de TechNet de Exchange Server.  
  
> [!NOTE]
>  -   Te recomendamos que configures direcciones IP estáticas para el servidor que ejecuta Windows Server Essentials y del segundo servidor ejecuta Exchange Server. Para obtener instrucciones sobre cómo configurar una dirección IP estática en un equipo que ejecute Windows Server 2003 o Windows Server 2008 R2, consulta [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) en la biblioteca de TechNet de Windows Server  
>   
>      **Nota:** la configuración del servidor DNS siempre debe apuntar a la dirección IP del servidor que ejecuta Windows Server Essentials.  
> -   En el enrutador, asegúrate de que las direcciones IP del servidor que ejecuta Windows Server Essentials y el servidor que ejecuta Exchange Server están reservadas o fuera del intervalo de direcciones IP de DHCP.  
> -   La configuración del enrutador en esta sección se da por hecho que tienes que solo una dirección IP pública asignada por el proveedor de servicios de Internet (ISP). Si tienes varias direcciones IP públicas, puedes asignar una dirección IP diferente al servidor que ejecuta Windows Server Essentials y al servidor que ejecuta Exchange Server y, a continuación, crea forwardings puerto en función de las direcciones IP públicas.  
  
### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Habilitar la integración de Exchange Server local en Windows Server Essentials  
  
> [!NOTE]
>  Si vas a migrar desde una instalación de Windows Small Business Server, te recomendamos que omitir este paso por ahora y ejecútalo después de desinstalar la instalación anterior de Exchange Server en el servidor de origen.  
  
 Después de instalar y configurar un servidor que ejecuta Exchange Server, debes habilitar la integración de Exchange Server local en el servidor que ejecuta Windows Server Essentials.  
  
##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Para habilitar la integración de Exchange Server local desde el panel  
  
1.  Iniciar sesión en el servidor que ejecuta Windows Server Essentials como administrador y, a continuación, abra el panel.  
  
2.  En la **Home** página, haz clic en **conectarse a mi servicio de correo electrónico**y, a continuación, haz clic en **integrar tu servidor Exchange**.  
  
3.  En el panel de información, haz clic en **configurar la integración de Exchange Server**.  
  
4.  Sigue las instrucciones del asistente.  
  
### <a name="configure-a-reverse-proxy"></a>Configurar a un servidor proxy inverso  
  
> [!NOTE]
>  Esto es una tarea obligatoria si tienes solo una conexión a Internet de tu proveedor de servicios de Internet.  
  
 Windows Server Essentials y Exchange Server admiten algunos escenarios de acceso remoto para que los usuarios de red. Por ejemplo, si activar acceso desde cualquier lugar en el servidor que ejecuta Windows Server Essentials, remotamente puede acceder al sitio de acceso Web remoto o utilice redes privadas virtuales (VPN) para conectarse de forma remota a la red de Windows Server Essentials. Para obtener acceso remoto a los mensajes de correo electrónico, debes usar Outlook desde cualquier lugar, Outlook Web Access (OWA) o ActiveSync.  
  
 Si el servidor que ejecuta Exchange Server y Windows Server Essentials están conectados al mismo enrutador y no hay solo una entrada conexión a Internet de tu proveedor de servicios de Internet al enrutador, debes usar una solución de proxy inverso para enrutar distintos tipos de solicitudes de acceso remoto de Internet en función de los nombres de host de destino. Te recomendamos que uses Microsoft admite la extensión de IIS aplicación solicitar enrutamiento (ARR) como la solución de proxy inverso. Para obtener más información acerca de IIS solicitud de ruta de la solicitud, visite la [sitio Web de la aplicación solicitar enrutamiento](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
##### <a name="to-install-and-configure-application-request-routing"></a>Para instalar y configurar la solicitud de ruta de la solicitud  
  
1.  Iniciar sesión en Windows Server Essentials como administrador.  
  
2.  Abre el navegador de Internet y navegar a la [sitio Web de la aplicación solicitar enrutamiento](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
3.  En el sitio Web ARR, haz clic en **instalar**y, a continuación, sigue las instrucciones para instalar ARR.  
  
    > [!NOTE]
    >  Debes seleccionar el módulo de dirección URL reescribir durante la instalación de ARR.  
    >   
    >  Puede que recibas un error al final de la instalación de ARR KB 2589179 para ARR 2.5 no se instaló correctamente. Puede ignorar este error.  
  
4.  Una vez completada ARR instalación, reinicia el **puerta de enlace de escritorio remoto** servicio si no se está ejecutando.  
  
    > [!NOTE]
    >  Después de instalar ARR, la **puerta de enlace de escritorio remoto** se puede detener el servicio. Para reiniciar manualmente el servicio, abre el **servicios** herramienta administrativa y, a continuación, reinicia el **puerta de enlace de escritorio remoto** servicio.  
  
5.  [Descargar KB2732764 para ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302)y, a continuación, instalar la actualización en el servidor que ejecuta Windows Server Essentials.  
  
6.  Copia el archivo de certificado SSL para Exchange Server en el servidor que ejecuta Windows Server Essentials. El archivo de certificado debe contener la clave privada, y debe estar en el formato de archivo PFX.  
  
    > [!NOTE]
    >  Si estás usando un certificado emitido automática, siga las instrucciones en el artículo de Exchange Server [exportar un certificado de intercambio](https://technet.microsoft.com/library/dd351274.aspx) para exportar el certificado.  
  
7.  Dependiendo de qué versión de Windows Server Essentials se ejecuta, realiza una de las siguientes acciones:  
  
    -   En Windows Server Essentials: Abre una ventana de comando como administrador y, a continuación, abre el directorio de %ProgramFiles%\Windows Server\Bin  
  
    -   En Windows Server Essentials: Abre una ventana de comando como administrador y, a continuación, abre el directorio de %Windir%\System32\Essentials.  
  
8.  En función de su escenario de instalación, sigue uno de estos pasos para configurar ARR:  
  
    -   Si estás realizando una instalación limpia, ejecuta el siguiente comando:  
  
         ** ARRConfig config certificado ** *ruta de acceso al archivo de certificado* ** nombres de host ** *nombres para Exchange Server de host* ****  
  
        > [!NOTE]
        >  Por ejemplo; ** ARRConfig config certificado ***c:\temp\certificate.pfx*** nombres de host *** mail.contoso.com ***  
        >   
        >  Reemplazar *mail.contoso.com* con el nombre del dominio que está protegido por el certificado.  
  
    -   En va a migrar de Windows Small Business Server, ejecuta el siguiente comando:  
  
         ** ARRConfig config certificado ** *ruta de acceso al archivo de certificado* ** nombres de host ** *nombres para Exchange Server de host* ** servidor de destino ** *nombre del servidor Exchange* ****  
  
         Por ejemplo; ** ARRConfig config certificado ***c:\temp\certificate.pfx*** nombres de host ***mail.contoso.com*** servidor de destino *** ExchangeSvr ***  
  
         Reemplazar *mail.contoso.com* con el nombre de tu dominio. Reemplazar *ExchangeSvr* con el nombre del servidor que ejecuta Exchange Server.  
  
9. Cuando se te pida, escribe la contraseña para el certificado.  
  
> [!NOTE]
>  -   Los nombres de host que proporcionas deben estar incluidos en el certificado SSL que compraste para Exchange Server.  
> -   Si tienes varios nombres de host, usa una coma (,) para separarlos grupos.  
  
 Para comprobar que la configuración funciona, intente acceder al sitio Web OWA para que el servidor que ejecuta Exchange Server (https://mail. *yourDomainName*.com/owa) desde un equipo que no es un miembro del dominio. Para solucionar problemas de conectividad, también puedes usar en línea [analizador de conectividad remota de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) herramienta.  
  
### <a name="configure-split-dns-for-exchange-server"></a>Configurar dividir DNS para Exchange Server  
  
> [!NOTE]
>  Se trata de una tarea recomendada.  
  
 DNS dividida te permite configurar diferentes direcciones IP en DNS para el mismo nombre de host, dependiendo de dónde se origina la solicitud DNS. Si el equipo cliente está en la intranet, la solicitud DNS se resuelve en una dirección IP de intranet. Si el equipo cliente se encuentra en Internet, la solicitud DNS se resuelve en una dirección IP de Internet. Esto es transparente para los usuarios.  
  
 Te recomendamos que configures dividida servicios DNS de manera que permite a los usuarios siempre usen el mismo nombre de host para tener acceso a Exchange Server, sin importar su ubicación.  
  
##### <a name="to-configure-split-dns-for-exchange-server"></a>Para configurar DNS dividida para Exchange Server  
  
1.  Iniciar sesión en Windows Server Essentials como administrador y, a continuación, abre el Administrador de DNS.  
  
2.  En el árbol de consola del Administrador de DNS, haz clic en el servidor y, a continuación, haz clic en **nueva zona **. La **Asistente para nueva zona** aparece.  
  
3.  En la **tipo de zona** página del asistente, acepta la opción predeterminada y, a continuación, haz clic en **siguiente **.  
  
4.  En la **ámbito de replicación de Active Directory zona** página, acepta la opción predeterminada y, a continuación, haz clic en **siguiente **.  
  
5.  En la **hacia delante o la zona de búsqueda inversa** página, acepte o seleccione **zona de búsqueda directa**y, a continuación, haz clic en **siguiente **.  
  
6.  En la **nombre de la zona**, escriba el nombre completo del servidor que ejecuta Exchange Server (por ejemplo; *mail.contoso.com*) y, a continuación, haz clic en **siguiente **.  
  
7.  En la **actualización dinámica** página, acepta la opción predeterminada, haz clic en **siguiente**y, a continuación, haz clic en **finalizar **.  
  
8.  En el árbol de consola del Administrador de DNS, haz clic en la nueva zona de búsqueda directa y, a continuación, haz clic en **nuevo Host (A o AAAA)**.  
  
9. En la **nuevo Host** página, deja la **nombre** campo en blanco, escribe la dirección IP de intranet de tu servidor que ejecuta Exchange Server y, a continuación, haz clic en **agregar Host **.  
  
    > [!NOTE]
    >  Si dejas el **nombre** campo en blanco, el servidor usa el nombre del dominio principal de forma predeterminada.  
  
10. En la **nuevo Host** página, haz clic en **listo **.  
  
> [!NOTE]
>  Si utiliza ActiveSync, pero no puede sincronizar el correo electrónico para algunas cuentas de buzón de correo, determinar si esas cuentas son miembros de uno o varios grupos protegidos, como los administradores de dominio. Información For-related que puede ayudarte a resolver este problema, consulta [Exchange ActiveSync devuelve un Error de HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  
  
## <a name="related-topics"></a>Temas relacionados  
 Para obtener más información acerca de cómo integrar un servidor de Exchange local, consulta las secciones siguientes.  
  
### <a name="what-happens-if-i-disable-exchange-integration"></a>¿Qué sucede si deshabilitar la integración de Exchange?  
 Si deshabilitas la integración con un servidor de Exchange local, ya no podrás usar el escritorio de Windows Server Essentials para ver, crear o administrar buzones de Exchange Server.  
  
### <a name="what-do-i-need-to-know-about-email-accounts"></a>¿Qué debo saber acerca de las cuentas de correo electrónico?  
 Una solución de correo electrónico hospedadas está configurada en el servidor. Una solución de un proveedor de correo electrónico hospedadas, como Microsoft Office 365, puedes proporcionar cuentas de correo electrónico individual para usuarios de la red. Cuando ejecutes el agregar un asistente de cuenta de usuario en Windows Server Essentials para crear una cuenta de usuario, el asistente intenta agregar la cuenta de usuario a la solución de correo electrónico disponible. Al mismo tiempo, el asistente asigna un nombre de correo electrónico (alias) al usuario y establece el tamaño máximo del buzón (cuota). El tamaño máximo del buzón varía según el proveedor de correo electrónico que usas. Después de agregar la cuenta de usuario, puedes seguir administrar la información de alias y cuota de buzón de la página de propiedades para el usuario. Para la administración completa de tus cuentas de usuario y el proveedor de correo electrónico hospedadas, usa la consola de administración de tu proveedor de hospedado. Dependiendo del proveedor, pueden acceder a su consola de administración desde un portal basado en web, o desde una pestaña en el panel del servidor.  
  
 El alias que proporcionas cuando ejecutas el agregar un asistente de cuenta de usuario se envía al proveedor de correo electrónico hospedadas como el nombre sugerido para el alias del usuario. Por ejemplo, si es el alias de usuario *FrankM*, podría ser la dirección de correo electrónico del usuario s *FrankM@Contoso.com*.  
  
 Además, la contraseña que estableces para el usuario en el asistente Agregar un usuario cuenta será la contraseña del usuario en la solución de correo electrónico hospedadas inicial.  
  
 Por último, si eliminas el usuario mediante el uso de la eliminación de un asistente de cuenta de usuario en el servidor, el asistente también envía una solicitud al proveedor de correo electrónico hospedadas para eliminar el usuario de su sistema también. Puede eliminar el proveedor de la cuenta de usuario s y el correo electrónico que está asociado con la cuenta.  
  
 Para obtener información del usuario acerca de cómo configurar el software de cliente de correo electrónico necesaria, o cómo tener acceso a una cuenta de correo electrónico, consulta la documentación de ayuda proporcionada por el proveedor de correo electrónico hospedadas.  
  
### <a name="what-is-a-mailbox-quota"></a>¿Qué es una cuota de buzón?  
 La cantidad de espacio de almacenamiento que se asigna para que un usuario de la red s datos de buzón de Exchange se conoce como la cuota de buzón.  
  
 Cuando ejecutas la **configurar la integración de Exchange Server** tareas en el panel, el Asistente agregan una página en el Asistente para agregar un cuenta de usuario que le permite elegir si se va a aplicar buzones y para especificar el tamaño de la cuota. De manera predeterminada, la **aplicar buzones** opción está activada (activado) y los buzones de usuario se asignan 2 GB de espacio de almacenamiento. Los administradores de Exchange pueden personalizar la configuración de la cuota de buzón para satisfacer las necesidades de su negocio.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Requisitos del sistema de Windows Server Essentials](../get-started/system-requirements.md)  
  
-   [Administrar la integración de servicio de correo electrónico](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
