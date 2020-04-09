---
title: Integrar un servidor local de Exchange Server con Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a18da3e8de1337eb3253725a7c50c1ebed1dc6c7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852878"
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Integrar un servidor local de Exchange Server con Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta guía proporciona información e instrucciones básicas que le ayudarán a configurar e integrar un servidor local que ejecuta Exchange Server con un servidor que ejecuta Windows Server Essentials.  

 Lea esta guía antes de intentar implementar un servidor local que ejecuta Exchange Server en una red de Windows Server Essentials.  

> [!NOTE]
>  Exchange Server 2010 no permite la instalación de equipos que ejecutan Windows Server 2012.  

## <a name="prerequisites"></a>Requisitos previos  
 Antes de instalar Exchange Server en una red de Windows Server Essentials, asegúrese de completar las tareas descritas en esta sección.  

-   [Configurar un servidor que ejecute Windows Server Essentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  

-   [Preparar un segundo servidor en el que instalar Exchange Server](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  

-   [Configurar el nombre de dominio de Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  

###  <a name="set-up-a-server-that-is-running-windows-server-essentials"></a><a name="BKMK_SetUpSBS8"></a>Configurar un servidor que ejecute Windows Server Essentials  
 Debe haber configurado un servidor que ejecuta Windows Server Essentials. Este será el controlador de dominio para el servidor que ejecuta Exchange Server. Para obtener información acerca de cómo configurar Windows Server Essentials, vea [Instalar Windows Server Essentials](../install/Install-Windows-Server-Essentials.md).  

###  <a name="prepare-a-second-server-on-which-to-install-exchange-server"></a><a name="BKMK_SecondServer"></a>Preparar un segundo servidor en el que instalar Exchange Server  
 Debe instalar Exchange Server en un segundo servidor que ejecute una versión del sistema operativo Windows Server que admita oficialmente Exchange Server 2010 o Exchange Server 2013. Después, debe unir el segundo servidor al dominio de Windows Server Essentials.  

 Para obtener información sobre cómo unir un segundo servidor al dominio de Windows Server Essentials, consulte unir un segundo servidor a la red en [conectarse](../use/Get-Connected-in-Windows-Server-Essentials.md).  

> [!NOTE]
>  Microsoft no admite la instalación de Exchange Server en un servidor que ejecute Windows Server Essentials.  

###  <a name="configure-your-internet-domain-name"></a><a name="BKMK_DomainNames"></a>Configurar el nombre de dominio de Internet  
 Para integrar un servidor local que ejecuta Exchange Server con Windows Server Essentials, debe haber registrado un nombre de dominio de Internet válido para su empresa (por ejemplo, *contoso.com*). También debe trabajar con su proveedor de nombres de dominio para crear los registros de recursos DNS que Exchange Server necesita.  

 Por ejemplo, si el nombre de dominio de Internet de su compañía es contoso.com y quiere usar el nombre completo (FQDN) de *mail.contoso.com* para hacer referencia al servidor local que ejecuta Exchange Server, trabaje con su proveedor de nombres de dominio para crear los registros de recursos DNS de la siguiente tabla.  


| Nombre de registro de recurso |     Tipo de registro     |                                                                         Configuración del registro                                                                          |                                                                                                                                                                                                                                                              Descripción                                                                                                                                                                                                                                                              |
|----------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         correo         |      host (A)       |                                                        Address=*dirección IP pública asignada por su ISP*                                                         |                                                                                                                                                                                                   Exchange Server recibirá el correo dirigido a mail.contoso.com.<br /><br /> Puede usar un nombre diferente que elija.                                                                                                                                                                                                    |
|          MX          | Agente de intercambio de correo (MX) |                                            Hostname=@<br /><br /> Address=mail.contoso.com<br /><br /> Preference=0                                             |                                                                                                                                                                                                      Proporciona el enrutamiento de los mensajes de correo electrónico para que email@contoso.com lleguen al servidor local que ejecuta Exchange Server.                                                                                                                                                                                                       |
|         SPF          |     texto (TXT)      |                                                                        v=spf1 a mx ~all                                                                         |                                                                                                                                                                                                                      Registro de recurso que ayuda a evitar que el correo electrónico que se envía desde su servidor se identifique como spam.                                                                                                                                                                                                                      |
|  autodiscover._tcp   |    servicio (SRV)    | Service: _autodiscover<br /><br /> Protocol: _tcp<br /><br /> Prioridad: 0<br /><br /> Peso: 0<br /><br /> Puerto: 443<br /><br /> Target host: mail.contoso.com | Permite que Microsoft Office Outlook y los dispositivos móviles detecten automáticamente el servidor local que ejecuta Exchange Server.<br /><br /> **Nota:** También puede configurar un registro de recursos de host (A) de detección automática y apuntar el registro a la dirección IP pública del servidor local que ejecuta Exchange Server. Sin embargo, si implementa esta opción, también debe proporcionar un certificado SSL de nombre alternativo del firmante (SAN) que admita los nombres de dominio mail.contoso.com y autodiscover.contoso.com. |

> [!NOTE]
>  -   Reemplace las apariciones de *contoso.com* en este ejemplo con el nombre de dominio de Internet que haya registrado.  

 Para su servidor local que ejecuta Exchange Server, debe elegir un FQDN diferente del FQDN que usa para el servidor que ejecuta Windows Server Essentials. Por ejemplo, puede elegir usar *remote.contoso.com* como FQDN que los equipos usarán para tener acceso al servidor que ejecuta Windows Server Essentials desde Internet. Puede usar *mail.contoso.com* como FQDN para enrutar el correo electrónico hacia el servidor local que ejecuta Exchange Server.  

## <a name="install-exchange-server"></a>Instalar Exchange Server  
 La característica de integración de Exchange Server de Windows Server Essentials admite las siguientes versiones de Exchange Server:  

- Exchange Server 2013  

- Exchange Server 2010 con Service Pack 1 (SP1)  

  Antes de instalar Exchange Server en el segundo servidor, primero debe agregar la cuenta de administrador actual al grupo **Enterprise Admins**.  

#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Para agregar la cuenta de administrador actual al grupo Administradores de empresa  

1.  Inicie sesión en Windows Server Essentials como administrador.  

2.  Ejecute Windows PowerShell como administrador.  

3.  En el símbolo del sistema de Windows PowerShell, escriba **Add-ADGroupMember "Enterprise Admins" $env: username**y, a continuación, presione Entrar.  

#### <a name="to-install-exchange-server"></a>Para instalar Exchange Server  

1.  Inicie sesión en el segundo servidor como administrador.  

2.  Abra su explorador de Internet y navegue al sitio web [Exchange Server Deployment Assistant](https://go.microsoft.com/fwlink/p/?LinkID=249163) (Asistente de implementación de Exchange Server).  

3.  Haga clic en **On-Premises Only** (Solo local).  

4.  Haga clic en la opción de nueva instalación para la versión de Exchange Server que va a instalar.  

    > [!NOTE]
    >  Si está migrando desde una instalación de Windows Small Business Server, debe seleccionar la opción de actualización apropiada que incluya los pasos de migración.  

5.  En la página siguiente, acepte la configuración predeterminada y haga clic en **Next** (Siguiente).  

    > [!NOTE]
    >  Si tiene pensado usar carpetas públicas en la nueva instalación de Exchange Server, cambie esa opción de configuración a **Yes** (Sí).  

6.  Siga las instrucciones paso a paso de la lista de comprobación para implementar Exchange Server.  

     El asistente de implementación de Exchange Server también permite:  

    -   Imprimir una copia de la lista de comprobación.  

    -   Enviar una copia de la lista de comprobación a un destinatario de correo electrónico.  

    -   Descargar la lista de comprobación como un archivo PDF.  

> [!NOTE]
> - Debe elegir siempre instalar las Herramientas de administración en el servidor que ejecuta Exchange Server. La característica de integración de Exchange Server en Windows Server Essentials requiere las herramientas de administración.  
>   -   Si necesita configurar directorios virtuales, recomendamos que establezca también la propiedad **InternalUrl** en la misma dirección URL que la propiedad **ExternalUrl** para cada directorio virtual. Para obtener más información, consulte [Administrar directorios virtuales de servidores de acceso de cliente](https://go.microsoft.com/fwlink/p/?LinkId=251058) en el sitio web de Ayuda en pantalla de Exchange Server 2010.  
>   -   Si quiere tener acceso a Outlook Web Access (OWA) desde el sitio de Acceso Web remoto de Windows Server Essentials, debe establecer la propiedad Dirección URL externa de OWA.  

 Si va a instalar Exchange Server 2010 con una instalación limpia, también puede usar los siguientes scripts para instalar Exchange Server.  

#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Para usar scripts para instalar Exchange Server  

1.  Abra el Bloc de notas y pegue el siguiente script en un archivo nuevo:  

```powershell
Import-Module ServerManager

Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  Restart
```

2. Guarde el archivo como **InstallDependencies.ps1**.  

3. Copie el certificado SSL de Exchange en una ubicación del servidor.  

4. Abra un nuevo archivo del Bloc de notas y copie el siguiente texto en el archivo:  

```powershell
param (
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]
    $CertPath = "c:\certificates\ExchangeCertificate.pfx",
    [Security.SecureString]
    [Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]
    $CertPassword = $null,
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]
    $DomainName = "contoso.com",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]
    $ServerIpAddress = "192.168.0.1",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]
    $InternalIpRange = "192.168.0.0-192.168.0.255"
)

#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.

Import-ExchangeCertificate  -FileData ([Byte[]]$(Get-content -Path $CertPath  -Encoding byte  -ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force

#New AcceptedDomain and set it to default

New-AcceptedDomain  -Name "official name"  -DomainName $domainname

Set-AcceptedDomain  -Identity "official name"  -MakeDefault $true

#New EmailAddress Policy

$address = "%m@" + $DomainName

New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address

#Set owa and ecp VirtualDirectory ExternalUrl

$hostname = "mail." + $DomainName

$owa = "https://" + $hostname + "/owa"

$ecp = "https://" + $hostname + "/ecp"

$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"

$oab = "https://" + $hostname + "/OAB"

$ews = "https://" + $hostname + "/EWS/Exchange.asmx"

Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  -ExternalUrl $owa  -InternalUrl $owa

Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  -ExternalUrl $ecp  -InternalUrl $ecp

Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  -InternalUrl $activesync

Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true

Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force

#Enable outlook Anywhere

Enable-OutlookAnywhere  -ClientAuthenticationMethod:Basic  -ExternalHostname:$hostname  -SSLOffloading:$false

#new receive/send connector

$machinename = get-content env:computername

$bindingIpaddress = $ServerIpAddress + ":25"

$ReceiveConnectorName = $machinename + "\Default " + $machinename

Set-ReceiveConnector $ReceiveConnectorName -RemoteIPRanges $InternalIpRange

New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated

New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename
```

5. Establezca los parámetros al comienzo del script para que reflejen su entorno de red.  

6. Guarde el archivo como **ConfigureExchange.ps1**.  

7. Ejecute Windows PowerShell como administrador.  

8. En el símbolo del sistema de Windows PowerShell, escriba **Set-ExecutionPolicy RemoteSigned** y presione ENTRAR.  

9. Ejecute el script **InstallDependencies.ps1**.  

10. Reinicie el servidor y, después, ejecute Windows PowerShell como administrador.  

11. Ejecute el siguiente script en el símbolo del sistema de Windows PowerShell:  

    `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  

   > [!NOTE]
   >  Asegúrese de escribir la ruta de acceso correcta al programa de instalación de Exchange Server.  

12. Una vez completada la instalación de Exchange Server, abra el Shell de administración de Exchange como administrador.  

13. En el símbolo del sistema del Shell de administración de Exchange, escriba **Set-ExecutionPolicy RemoteSigned** y presione Entrar.  

14. Ejecute el script **ConfigureExchange.ps1**.  

15. Reinicie el servidor.  

> [!NOTE]
>  Si decide usar un certificado SSL de confianza pública en lugar de un certificado emitido automáticamente, puede seguir las instrucciones de la guía de instalación para crear una solicitud de certificado y enviarla a la entidad de certificación seleccionada. También puede usar un cmdlet de Exchange PowerShell para crear una solicitud de certificado. A continuación encontrará un ejemplo.  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personalice los parámetros del script para que reflejen su entorno de trabajo.  

## <a name="post-installation-tasks"></a>Tareas de postinstalación  
 En esta sección se describen las tareas de configuración del servidor que debe completar en la fase de postinstalación, y contienen información específica para configurar un servidor local que ejecuta Exchange Server en una red de Windows Server Essentials.  

### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Agregar el dominio de correo electrónico público y configurar las directivas de direcciones de correo electrónico  

> [!NOTE]
>  Esta es una tarea obligatoria si está realizando una instalación limpia. Omita este paso si está migrando desde Windows Small Business Server.  

 Debe especificar que su dominio de correo electrónico sea el dominio aceptado predeterminado y, después, configurar la directiva de direcciones de correo electrónico.  

##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Para agregar su dominio de correo electrónico como dominio aceptado predeterminado  

1.  Siga las instrucciones del artículo sobre Exchange Server [Crear un dominio aceptado](https://go.microsoft.com/fwlink/p/?LinkId=249174) para agregar un dominio aceptado.  

2.  Inicie sesión en el segundo servidor como administrador, abra la Consola de administración de Exchange y navegue a la pestaña **Transporte de concentradores** de la **Configuración de la organización**.  

3.  En el área de trabajo de la Consola de administración de Exchange, haga clic con el botón secundario en el nuevo dominio aceptado y, a continuación, haga clic en **Establecer como predeterminado**.  

4.  Siga las instrucciones del artículo sobre Exchange Server [Crear una directiva de direcciones de correo electrónico](https://go.microsoft.com/fwlink/p/?LinkId=249179) para crear una nueva directiva de direcciones de correo electrónico. Puede aceptar todos los valores predeterminados excepto la dirección de correo electrónico. Para la dirección de correo electrónico, especifique su dominio de correo electrónico público.  

### <a name="create-smtp-send-and-receive-connectors"></a>Crear conectores de envío y recepción SMTP  

> [!NOTE]
>  Esta es una tarea obligatoria.  

 Debe configurar un conector de envío SMTP y un conector de recepción SMTP para la transmisión de entrada y de salida de los mensajes de correo electrónico.  

 Para crear un conector de envío SMTP, siga las instrucciones del artículo sobre Exchange Server [Crear un conector de envío SMTP](https://technet.microsoft.com/library/aa997285.aspx).  

 Para crear un conector de recepción SMTP, siga las instrucciones del artículo sobre Exchange Server [Crear un conector de recepción SMTP](https://technet.microsoft.com/library/bb125159.aspx).  

 Opcionalmente, puede hacer referencia al script descrito anteriormente en este documento para crear los conectores de envío y recepción por medio de cmdlets de Exchange PowerShell.  

### <a name="configure-the-network-router"></a>Configurar en enrutador de red  

> [!NOTE]
>  Esta es una tarea obligatoria si está realizando una instalación limpia. Si va a efectuar una migración desde Windows Small Business Server, vea [Migrar datos del servidor a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) para obtener instrucciones sobre cómo configurar la red.  

 Como mínimo, debe configurar las siguientes opciones de puerto en el enrutador:  

|Puerto del enrutador|IP de destino|Puerto de destino|Nota|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|Dirección IP interna del servidor local que ejecuta Exchange Server.|25||  
|80 (HTTP)|Dirección IP interna del servidor que ejecuta Windows Server Essentials|80||  
|443 (HTTPS)|Dirección IP interna del servidor que ejecuta Windows Server Essentials|443||  

 Si admite los protocolos de mensajería POP3 o IMAP en su red, debe configurar también los desvíos de puertos para estos protocolos. Para obtener información relacionada, consulte la sección **Servidores de acceso de cliente** en el tema [Referencia del puerto de red de Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) de la biblioteca de TechNet de Exchange Server.  

> [!NOTE]
> - Recomendamos configurar direcciones IP estáticas para el servidor que ejecuta Windows Server Essentials y para el segundo servidor que ejecuta Exchange Server. Para obtener instrucciones sobre cómo configurar una dirección IP estática en un equipo que ejecuta Windows Server 2003 o Windows Server 2008 R2, consulte [Configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) en la biblioteca de TechNet de Windows Server.  
> 
>   **Nota:** La configuración del servidor DNS siempre debe apuntar a la dirección IP del servidor que ejecuta Windows Server Essentials.  
>   -   En el enrutador, asegúrese de que las direcciones IP del servidor que ejecuta Windows Server Essentials y del servidor que ejecuta Exchange Server estén reservadas o estén fuera del intervalo de direcciones IP DHCP.  
>   -   En la configuración del enrutador de esta sección, se da por supuesto que su proveedor de servicios de Internet (ISP) solo le ha asignado una dirección IP pública. Si tiene varias direcciones IP públicas, puede asignar una dirección IP diferente al servidor que ejecuta Windows Server Essentials y al servidor que ejecuta Exchange Server y, después, crear desvíos de puertos basados en las direcciones IP públicas.  

### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Habilitar la integración local de Exchange Server en Windows Server Essentials  

> [!NOTE]
>  Si está migrando desde una instalación de Windows Small Business Server, le recomendamos que omita este paso por el momento y que lo realice después de desinstalar la instalación anterior de Exchange Server en el servidor de origen.  

 Después de desinstalar y configurar un servidor que ejecuta Exchange Server, debe habilitar la integración de Exchange Server local en el servidor que ejecuta Windows Server Essentials.  

##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Para habilitar la integración de Exchange Server local desde el panel  

1.  Inicie sesión en el servidor que ejecuta Windows Server Essentials como administrador y, después, abra el panel.  

2.  En la página **principal**, haga clic en **Connect to My Email Service** (Conectar con mi servicio de correo electrónico) y haga clic en **Integrate your Exchange Server** (Integrar su servidor de Exchange Server).  

3.  En el panel de información, haga clic en **Set up Exchange Server Integration** (Configurar integración de Exchange Server).  

4.  Siga las instrucciones del asistente.  

### <a name="configure-a-reverse-proxy"></a>Configurar un proxy inverso  

> [!NOTE]
>  Esta es una tarea obligatoria si solo tiene una conexión de Internet desde su proveedor de servicios de Internet.  

 Tanto Windows Server Essentials como Exchange Server admiten algunos escenarios de acceso remoto para usuarios de red. Por ejemplo, si activa Acceso desde cualquier lugar en el servidor que ejecuta Windows Server Essentials, puede tener acceso de forma remota al sitio Acceso Web remoto o usar una red privada virtual (VPN) para conectar de forma remota con la red de Windows Server Essentials. Para acceder de forma remota a los mensajes de correo electrónico, debe usar Outlook en cualquier lugar, Outlook Web Access (OWA) o ActiveSync.  

 Si Windows Server Essentials y el servidor que ejecuta Exchange Server están conectados al mismo enrutador y solo hay una conexión de entrada desde su proveedor de servicios de Internet al enrutador, debe usar una solución de proxy inverso para enrutar los diferentes tipos de solicitudes de acceso remoto desde Internet en función de los nombres de host de destino. Le recomendamos que use la extensión Enrutamiento de solicitud de aplicaciones (ARR) de IIS admitida por Microsoft como solución de proxy inverso. Para obtener más información sobre el Enrutamiento de solicitud de aplicaciones de IIS, visite el [sitio web de Enrutamiento de solicitud de aplicaciones](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

##### <a name="to-install-and-configure-application-request-routing"></a>Para instalar y configurar Enrutamiento de solicitud de aplicaciones  

1. Inicie sesión en Windows Server Essentials como administrador.  

2. Abra su explorador de Internet y navegue al [sitio web de Enrutamiento de solicitud de aplicaciones](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

3. En el sitio web de ARR, haga clic en **Instalar** y siga las instrucciones para instalar ARR.  

   > [!NOTE]
   >  Debe seleccionar el Módulo URL Rewrite durante la instalación de ARR.  
   >   
   >  Quizás reciba un error al final de la instalación de ARR que indica que KB 2589179 para ARR 2.5 no se instaló correctamente. Puede ignorar este error sin problema alguno.  

4. Una vez completa la instalación de ARR, reinicie el servicio **Puerta de enlace de Escritorio remoto** si no se está ejecutando.  

   > [!NOTE]
   >  Después de instalar ARR quizás se detenga el servicio **Puerta de enlace de Escritorio remoto**. Para reiniciar manualmente el servicio, abra la herramienta administrativa **Servicios** y reinicie el servicio **Puerta de enlace de Escritorio remoto**.  

5. [Descargue KB2732764 para ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302)e instale la actualización en el servidor que ejecuta Windows Server Essentials.  

6. Copie el archivo de certificado SSL para Exchange Server en el servidor que ejecuta Windows Server Essentials. El archivo de certificado debe contener la clave privada y debe tener el formato de archivo PFX.  

   > [!NOTE]
   >  Si está usando un certificado emitido automáticamente, siga las instrucciones del artículo sobre Exchange Server [Exportar un certificado de Exchange](https://technet.microsoft.com/library/dd351274.aspx) para exportar el certificado.  

7. En función de la versión de Windows Server Essentials que ejecute, realice una de las acciones siguientes:  

   -   En Windows Server Essentials: Abra una ventana de comandos como administrador y abra el directorio%ProgramFiles%\Windows Server\bin.  

   -   En Windows Server Essentials: Abra una ventana de comandos como administrador y abra el directorio%WINDIR%\system32\essentials.  

8. En función de su escenario de instalación, siga uno de estos pasos para configurar ARR:  

   - Si está realizando una instalación limpia, ejecute el siguiente comando:  

      **ARRConfig config:** _ruta de acceso de certificado al archivo_ **de certificado:** _nombres de host nombres de host para Exchange Server_  

     > [!NOTE]
     >  Por ejemplo, **ARRConfig config-CERT** _c:\temp\certificate.pfx_ **-hostnames** _mail.contoso.com_  
     > 
     >  Reemplace *mail.contoso.com* con el nombre del dominio que está protegido por el certificado.  

   - Si está migrando desde Windows Small Business Server, ejecute el siguiente comando:  

      **ARRConfig config:** _ruta de acceso de certificado al archivo de certificado_ **:** _nombres de host nombres de host para Exchange Server_ **-targetserver** _nombre de servidor de Exchange Server_  

      Por ejemplo, **ARRConfig config-CERT** _c:\temp\certificate.pfx_ **-hostnames** _mail.contoso.com_ **-targetserver** _ExchangeSvr_  

      Reemplace *mail.contoso.com* con el nombre de su dominio. Reemplace *ExchangeSvr* con el nombre del servidor que ejecuta Exchange Server.  

9. Cuando se le pregunte, escriba la contraseña del certificado.  

> [!NOTE]
> - Los nombres de host que proporcione deben estar contenidos en el certificado SSL que adquirió para Exchange Server.  
>   -   Si tiene varios nombres de host, use una coma (,) para separarlos.  

 Para comprobar que la configuración funciona, intente tener acceso al sitio web de OWA para el servidor que ejecuta Exchange Server (https://mail. *nombreDeDominio*.com/owa) desde un equipo que no sea miembro del dominio. Para solucionar los problemas de conectividad, también puede usar la herramienta en línea [Analizador de conectividad remota de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) .  

### <a name="configure-split-dns-for-exchange-server"></a>Configurar DNS dividido para Exchange Server  

> [!NOTE]
>  Esta es una tarea recomendada.  

 DNS dividido permite configurar diferentes direcciones IP en DNS para el mismo nombre de host, según dónde se origine la solicitud DNS. Si el equipo cliente está en la intranet, la solicitud DNS se resuelve en una dirección IP de intranet. Si el equipo cliente está en Internet, la solicitud DNS se resuelve en una dirección IP de Internet. Este proceso es transparente para los usuarios.  

 Le recomendamos que configure DNS dividido de manera que los usuarios puedan utilizar siempre el mismo nombre de host para acceder a los servicios de Exchange Server, independientemente de su ubicación.  

##### <a name="to-configure-split-dns-for-exchange-server"></a>Para configurar DNS dividido para Exchange Server  

1.  Inicie sesión en Windows Server Essentials como administrador y abra Administrador de DNS.  

2.  En el árbol de la consola del Administrador de DNS, haga clic con el botón derecho en el servidor y haga clic en **Zona nueva**. Se abre el **Asistente para nueva zona**.  

3.  En la página **Tipo de zona**, acepte la opción predeterminada y haga clic en **Siguiente**.  

4.  En la página **Ámbito de replicación de zona de Active Directory**, acepte la opción predeterminada y haga clic en **Siguiente**.  

5.  En la página **Zona de búsqueda directa o inversa**, acepte o seleccione **Zona de búsqueda directa** y haga clic en **Siguiente**.  

6.  En la página **Nombre de zona**, escriba el nombre completo (FQDN) del servidor que ejecuta Exchange Server (por ejemplo, *mail.contoso.com*) y haga clic en **Siguiente**.  

7.  En la página **Actualización dinámica**, acepte la opción predeterminada, haga clic en **Siguiente** y, después, haga clic en **Finalizar**.  

8.  En el árbol de consola del Administrador de DNS, haga clic con el botón secundario en la nueva zona de búsqueda directa y, a continuación, haga clic en **Host nuevo (A o AAAA)** .  

9. En la página **Host nuevo**, deje el campo **Nombre** en blanco, escriba la dirección IP de intranet del servidor que ejecuta Exchange Server y, a continuación, haga clic en **Agregar host**.  

    > [!NOTE]
    >  Cuando se deja el campo **Nombre** en blanco, el servidor usa el nombre de dominio primario de forma predeterminada.  

10. En la página **Host nuevo**, haga clic en **Listo**.  

> [!NOTE]
>  Si usa ActiveSync pero no puede sincronizar el correo electrónico de algunas cuentas de buzón, compruebe si esas cuentas son miembros de algún grupo protegido, como Administradores de dominio. Para obtener información relacionada que pueda ayudarle a resolver este problema, consulte [Exchange ActiveSync devolvió un error HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  

## <a name="related-topics"></a>Temas relacionados  
 Vea las siguientes secciones para obtener más información acerca de cómo integrar un servidor local de Exchange.  

### <a name="what-happens-if-i-disable-exchange-integration"></a>¿Qué ocurre si deshabilito la integración de Exchange?  
 Si deshabilita la integración con un servidor local de Exchange ya no podrá usar el panel de Windows Server Essentials para ver, crear o administrar buzones de correo de Exchange Server.  

### <a name="what-do-i-need-to-know-about-email-accounts"></a>¿Qué debo saber acerca de las cuentas de correo electrónico?  
 Hay una solución de correo electrónico hospedado configurada en el servidor. Una solución de un proveedor de correo electrónico hospedado, como Microsoft Office 365, puede proporcionar cuentas de correo electrónico individuales para los usuarios de la red. Al ejecutar el asistente para agregar cuentas de usuario en Windows Server Essentials, el asistente intenta agregar la cuenta de usuario a la solución disponible de correo electrónico hospedado. Al mismo tiempo, el asistente asigna un nombre de correo electrónico (alias) al usuario y establece el tamaño máximo del buzón (cuota). El tamaño máximo del buzón varía en función del proveedor de correo electrónico que use. Después de agregar la cuenta de usuario puede seguir administrando la información del alias y la cuota de buzón desde la página de propiedades del usuario. Para tener una administración completa de las cuentas de usuario y el proveedor de correo electrónico hospedado, use la consola de administración de su proveedor hospedado. En función del proveedor, puede tener acceso a su consola de administración desde un portal basado en web o desde una pestaña en el panel del servidor.  

 Se envía el alias proporcionado al ejecutar el asistente para agregar cuentas de usuario al proveedor de correo electrónico hospedado como nombre sugerido para el alias del usuario. Por ejemplo, si el alias de usuario es *FrankM*, la dirección de correo electrónico del usuario podría ser <em>FrankM@Contoso.com</em>.  

 Además, la contraseña establecida para el usuario en el asistente para agregar cuentas de usuario será la contraseña inicial del usuario en la solución de correo electrónico hospedado.  

 Por último, si elimina el usuario mediante el asistente para eliminar cuentas de usuario en el servidor, el asistente también enviará una solicitud al proveedor de correo electrónico hospedado para eliminar también el usuario del sistema. Es posible que el proveedor elimine la cuenta del usuario y el correo electrónico asociado a la cuenta.  

 Para obtener información de usuario acerca de cómo configurar el software necesario del cliente de correo electrónico, o cómo obtener acceso a una cuenta de correo electrónico, vea la documentación de ayuda proporcionada por el proveedor de correo electrónico hospedado.  

### <a name="what-is-a-mailbox-quota"></a>¿Qué es una cuota de buzón?  
 La cantidad de espacio de almacenamiento que se asigna a los datos de buzones de Exchange de un usuario de red se denomina cuota de buzón.  

 Al ejecutar la tarea **Configurar integración del servidor Exchange** en el panel, el asistente agrega una página al asistente para agregar cuentas de usuario que le permite elegir si quiere aplicar cuotas de buzón y especificar el tamaño de la cuota. De manera predeterminada, la opción **Cumplir cuotas del buzón** está seleccionada (activada) y los buzones de usuario tienen asignados 2 GB de espacio de almacenamiento. Los administradores de Exchange pueden personalizar la configuración de la cuota de buzón para satisfacer sus necesidades empresariales.  

## <a name="see-also"></a>Vea también  

-   [Requisitos del sistema para Windows Server Essentials](../get-started/system-requirements.md)  

-   [Administrar la integración del servicio de correo electrónico](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
