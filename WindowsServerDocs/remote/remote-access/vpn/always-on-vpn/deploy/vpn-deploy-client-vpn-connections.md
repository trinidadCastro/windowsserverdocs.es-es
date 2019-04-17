---
title: Configurar las conexiones VPN de Always On del cliente de Windows 10
description: En este paso, puedes obtener información sobre las opciones de ProfileXML y el esquema y configuración los equipos de cliente de Windows 10 para comunicarse con el que la infraestructura con una conexión VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: 6b383076686092e20448977bed3766f7d7d1c2b8
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031319"
---
# Paso 6. Configurar conexiones de VPN de Always On del cliente de Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Anterior:** paso 5. Configurar DNS y la configuración de Firewall](vpn-deploy-dns-firewall.md)<br>
& #187; [ **Siguiente:** paso 7. (Opcional) Acceso condicional para la conectividad VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

En este paso, puedes obtener información sobre las opciones de ProfileXML y el esquema y configuración los equipos de cliente de Windows 10 para comunicarse con el que la infraestructura con una conexión VPN. 

Puedes configurar al cliente de VPN de Always On a través de PowerShell, SCCM o Intune. Los tres requieren un perfil de VPN de XML para establecer la configuración de VPN adecuada. Inscripción para organizaciones sin SCCM o Intune y automatizar PowerShell es posible.

>[!NOTE]
>La directiva de grupo no incluye plantillas administrativas para configurar al cliente de Windows 10 siempre en VPN de acceso remoto.  Sin embargo, puedes usar scripts de inicio de sesión.

## Introducción a ProfileXML

ProfileXML es un nodo URI en el CSP de VPNv2. En lugar de configurar individualmente cada nodo VPNv2 CSP, como los desencadenadores, la ruta listas y los protocolos de autenticación: usa este nodo para configurar un cliente VPN de Windows 10, proporcionando toda la configuración como un solo bloque XML en un nodo único de CSP. El esquema de ProfileXML coincide con el esquema de los nodos de VPNv2 CSP casi idéntico, pero algunos de los términos son ligeramente diferentes.

Puedes usar ProfileXML en todos los métodos de distribución que se describe en esta implementación, como Windows PowerShell, System Center Configuration Manager y Intune. Hay dos formas de configurar el nodo ProfileXML VPNv2 CSP en esta implementación:

- **OMA DM**. Una forma es usar un proveedor de MDM con OMA-DM, como se explicó anteriormente en la sección [nodo VPNv2 CSP](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Usa este método, puede insertar fácilmente el marcado XML de configuración de perfil VPN en el nodo ProfileXML CSP cuando con Intune.

- **Instrumental de administración de Windows (WMI) - to - puente de CSP**. El segundo método de configurar el nodo ProfileXML CSP es usar el puente de WMI a CSP, una clase WMI denominada **MDM_VPNv2_01**, que puede acceder a VPNv2 CSP y el nodo ProfileXML. Al crear una nueva instancia de la clase WMI, WMI utiliza el CSP para crear el perfil de VPN al usar Windows PowerShell y System Center Configuration Manager.

Aunque estos métodos de configuración son diferentes, ambas requieren un perfil de VPN de XML con el formato correcto. Para usar la configuración de CSP de VPNv2 ProfileXML, puedes construir el XML con el esquema de ProfileXML para configurar las etiquetas necesarias para el escenario de implementación sencilla. Para obtener más información, consulta [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

A continuación encontrarás cada una de las opciones necesarias y su etiqueta ProfileXML correspondiente. Configurar cada opción de configuración en una etiqueta específica dentro del esquema ProfileXML y no todos ellos se encuentran en el perfil nativo. Para la posición de etiqueta adicionales, consulta el esquema ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo de conexión:** IKEv2 nativo

ProfileXML elemento:

    <NativeProtocolType>IKEv2</NativeProtocolType>

**Enrutamiento:** Dividir túnel

ProfileXML elemento:

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**La resolución de nombres:** Sufijo DNS y la lista de información de nombre de dominio

ProfileXML elementos:

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**Desencadenar:** Detección de red siempre activado y de confianza

ProfileXML elementos:

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**Autenticación:** PEAP-TLS con certificados de usuario protegido de TPM

ProfileXML elementos:

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

Puedes usar etiquetas simple para configurar algunos mecanismos de autenticación de VPN. Sin embargo, PEAP y EAP son más complicadas. La forma más sencilla para crear el archivo XML es configurar a un cliente VPN con su configuración de EAP y, a continuación, exportar esa configuración a XML. 

Para obtener más información sobre la configuración de EAP, consulta la [configuración de EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration). 

## <a name="bkmk_profile"></a>Crear manualmente un perfil de conexión de la plantilla

En este paso, se usa el protocolo de autenticación Extensible protegido \(PEAP\) para proteger la comunicación entre el cliente y el servidor. A diferencia de un nombre de usuario simple y una contraseña, esta conexión requiere una sección de EAPConfiguration única en el perfil VPN para que funcione. 

En lugar de describir cómo crear el archivo XML desde cero, usa configuración de Windows para crear una plantilla de perfil de VPN. Después de crear la plantilla de perfil de VPN, usan Windows PowerShell para consumir la parte EAPConfiguration desde esa plantilla para crear el ProfileXML final que implementar más adelante en la implementación.

### Registra la configuración de certificados NPS

Antes de crear la plantilla, tenga en cuenta el nombre de host o el nombre de dominio completo (FQDN) del servidor NPS desde el certificado del servidor y el nombre de la entidad de certificación que emitió el certificado.

**Procedimiento:**

1.  En el servidor NPS, abre el servidor de directivas de red.

2.  En la consola NPS en las directivas, haz clic en **Las directivas de red**.

3.  Haz clic en **conexiones de red privada Virtual \(VPN\)** y haga clic en **Propiedades**.

4.  Haz clic en la ficha **restricciones** y haz clic en los **Métodos de autenticación**.

5.  En los tipos de EAP, haz clic en **Microsoft: EAP protegido (PEAP)** y haz clic en **Editar**.

6.  Registra los valores para el **certificado emitido para** y **emisor**.<p>Puedes usar estos valores en la configuración de plantilla VPN próximas. Por ejemplo, si el FQDN del servidor es nps01.corp.contoso.com y el nombre de host es NPS01, el nombre del certificado se basa en el nombre DNS o el FQDN del servidor: por ejemplo, nps01.corp.contoso.com.

7.  Cancelar el cuadro de diálogo de editar propiedades de EAP protegido.

8.  Cancelar el cuadro de diálogo de propiedades de las conexiones de red privada Virtual (VPN).

9.  Cierra el servidor de directivas de red.

>[!NOTE]
>Si tienes varios servidores NPS, completa estos pasos en cada uno de ellos, para que el perfil de VPN cada una de ellas puede comprobar se deben usar.

### Configurar la plantilla de perfil de VPN en un equipo cliente unido a domain\

Ahora que tienes la información necesaria: configurar la plantilla de perfil de VPN en un equipo cliente unido al dominio. El tipo de cuenta de usuario que usas \ (es decir, un usuario estándar o administrator\) para esta parte del proceso no importa. 

Sin embargo, si no has reiniciado el equipo desde configurar la inscripción automática de certificado, hacerlo antes de configurar la plantilla de la conexión VPN para asegurarte de que tienes un certificado utilizable inscrito en él.

>[!NOTE]
>No hay ninguna manera de agregar manualmente las propiedades avanzadas de VPN, como las reglas de la tabla NRPT, siempre activado, detección de red, etcetera de confianza. En el siguiente paso, crea una conexión de VPN de prueba para comprobar la configuración del servidor VPN y que puede establecer una conexión VPN en el servidor.

**Crear manualmente una sola prueba conexión VPN.**

1.  Inicia sesión como un miembro del grupo de **Usuarios de VPN** en un equipo cliente unido al dominio.

2.  En el menú Inicio, escribe la **VPN**y presiona ENTRAR.

3.  En el panel de detalles, haz clic en **Agregar una conexión VPN**.

4.  En la lista de proveedores de VPN, haz clic en **Windows (integrados)**.

5.  En el nombre de la conexión, tipo de **plantilla**.

6.  Nombre del servidor o la dirección, escribe el **externo** FQDN del servidor VPN \ (por ejemplo,**vpn.contoso.com**\).

7.  Haz clic en **Guardar**.

8.  En la configuración relacionada, haz clic en **cambiar las opciones de adaptador**.

9.  Haz clic en la **plantilla**y haga clic en **Propiedades**.

10. En la pestaña **seguridad** , en el **Tipo de VPN**, haz clic en **IKEv2**.

11. En el cifrado de datos, haz clic en el **nivel máximo de cifrado**.

12. Haz clic en **utilizar el protocolo de autenticación Extensible (EAP)**; a continuación, en **Usar el protocolo de autenticación Extensible (EAP)**, haz clic en **Microsoft: EAP protegido (PEAP) (cifrado habilitado)**.

13. Haz clic en **Propiedades** para abrir el cuadro de diálogo de propiedades de EAP protegido y realiza los siguientes pasos:

    a. En el cuadro **Conectar a estos servidores** , escribe el nombre del servidor NPS que recupera desde la configuración de autenticación de servidor NPS anteriormente en esta sección (por ejemplo, NPS01).

    >[!NOTE]
    >Escribe el nombre del servidor debe coincidir con el nombre en el certificado. Recuperar este nombre anteriormente en esta sección. Si no coincide con el nombre, la conexión se producirá un error, que indica que "la conexión que no se pudo debido a una directiva configurada en el servidor VPN RAS."

    b.  En las entidades de certificación raíz de confianza, selecciona la entidad de certificación que emitió el certificado del servidor NPS (por ejemplo, contoso-CA) raíz.

    c.  En las notificaciones antes de conectar, haz clic en **no pide al usuario que autorice nuevos servidores o entidades emisoras de certificados de confianza**.

    d.  En Seleccionar el método de autenticación, haz clic en la **tarjeta inteligente u otro certificado**y haga clic en **Configurar**. Se abrirá la tarjeta inteligente u otro cuadro de diálogo de propiedades de certificado.

    e.  Haz clic en **usar un certificado en este equipo**.

    f.  En el cuadro conectar a estos servidores, escribe el nombre del servidor NPS que recuperan desde la configuración de autenticación de servidor NPS en los pasos anteriores.

    g.  En las entidades de certificación raíz de confianza, seleccione la entidad de certificación que emitió el certificado del servidor NPS de raíz.

    h.  Selecciona el **no pidas usuario para autorizar nuevos servidores o entidades de certificación de confianza** casilla de verificación.

    i.  Haz clic en **Aceptar** para cerrar la tarjeta inteligente u otro cuadro de diálogo de propiedades de certificado.

    j.  Haz clic en **Aceptar** para cerrar el cuadro de diálogo de propiedades de EAP protegido.

10. Haz clic en **Aceptar** para cerrar el cuadro de diálogo de propiedades de la plantilla.

11. Cerrar la ventana de las conexiones de red.

12. En la configuración, prueba la conexión VPN haciendo clic en la **plantilla**y **Conectar**.

>[!IMPORTANT]
>Asegúrate de que la plantilla de la conexión VPN en el servidor VPN es correcta. Al hacerlo, se garantiza que la configuración de EAP es correcta antes de usarlos en el siguiente ejemplo. Debes conectar al menos una vez antes de continuar; de lo contrario, el perfil no incluirá toda la información necesaria para conectarse a la VPN.

## <a name="bkmk_ProfileXML"></a>Crear los archivos de configuración de ProfileXML

Antes de finalizar esta sección, asegúrate de que has creado y probado la conexión VPN que se describe en la sección [crear manualmente un perfil de conexión de la plantilla](#bkmk_profile) de la plantilla. Prueba la conexión VPN es necesario para garantizar que el perfil contiene toda la información necesaria para conectarse a la VPN.

El script de Windows PowerShell en la lista 1 crea dos archivos en el escritorio, que contienen las etiquetas **EAPConfiguration** basadas en el perfil de conexión de la plantilla que creaste anteriormente:

-   **VPN_Profile.Xml.** Este archivo contiene el archivo XML necesario para configurar el nodo ProfileXML en el CSP de VPNv2. Usar este archivo con servicios de OMA DM: compatible con MDM, como Intune.

-   **VPN_Profile.ps1.** Este archivo es un script de Windows PowerShell que se puede ejecutar en los equipos cliente para configurar el nodo ProfileXML en el CSP de VPNv2. También puedes configurar el CSP implementando este script a través de System Center Configuration Manager. No puede ejecutar este script en una sesión de escritorio remoto, incluida una sesión de Hyper-V mejorado.

>[!IMPORTANT]
>Los comandos de ejemplo siguientes requieren la compilación de Windows 10 1607 o posterior.

**Crear VPN_Profile.xml y VPN_Proflie.ps1**

1. Inicio de sesión en el equipo cliente unido al dominio que contiene la plantilla de perfil de VPN con el mismo usuario de la cuenta que la sección "Crear manualmente un perfil de conexión de la plantilla" que se describe.

2. Pegar el listado 1 en el entorno de scripting integrado de Windows PowerShell \(ISE\) y personalizar los parámetros que se describe en los comentarios. Estos son $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork y $DNSServers. Es una descripción completa de cada opción de configuración en los comentarios.

3.  Ejecutar el script para generar **VPN_Profile.xml** y **VPN_Profile.ps1** en el escritorio.

#### Descripción de 1. Descripción MakeProfile.ps1

En esta sección se explica el código de ejemplo que puedes usar para obtener una descripción de cómo crear un perfil de VPN, específicamente para configurar ProfileXML en el CSP de VPNv2.

Después de ensamblar un script de este código de ejemplo y ejecutar el script, el script genera dos archivos: VPN_Profile.xml y VPN_Profile.ps1. Usar VPN_Profile.xml para configurar ProfileXML en servicios MDM compatible con OMA DM, como Microsoft Intune.

Usar el script **VPN_Profile.ps1** en Windows PowerShell o System Center Configuration Manager para configurar ProfileXML en el escritorio de Windows 10.

>[!NOTE]
>Para ver el script de ejemplo completo, consulta la sección [MakeProfile.ps1 completa Script](#bkmk_fullscript).

#### Parameters

Configura los siguientes parámetros:

**$Template**. El nombre de la plantilla desde el que se va a recuperar la configuración de EAP.

**$ProfileName**. Identificador alfanumérico único para el perfil. El nombre del perfil no debe incluir una barra diagonal (/). Si el nombre del perfil tiene un espacio u otros caracteres no alfanuméricos, deben ser escape correctamente según el estándar de codificación de la dirección URL.

**$Servers**. Dirección IP pública o enrutable, o el nombre DNS para la puerta de enlace VPN. Puede apuntar a la externa IP de puerta de enlace o una dirección IP virtual para una granja de servidores. Ejemplos, 208.147.66.130 o vpn.contoso.com.

**$DnsSuffix**. Especifica los sufijos DNS de separados por comas de uno o más. El primer elemento de la lista también se usa como el sufijo DNS específico de la conexión de la interfaz VPN. Toda la lista también se agregarán a la SuffixSearchList.

**$DomainName**. Se usa para indicar el espacio de nombres al que se aplica la directiva. Cuando se emite una consulta de nombre, el cliente DNS compara el nombre de la consulta a todos los espacios de nombres en DomainNameInformationList para encontrar a una coincidencia. Este parámetro puede ser uno de los siguientes tipos:

- Nombre de dominio completo - FQDN
- Sufijo: un sufijo de dominio que se anexará a la consulta shortname para la resolución DNS. Para especificar un sufijo, debes anteponer un período \ (.) en el sufijo DNS.

**$DNSServers**. Lista de IP del servidor DNS separados por comas de direcciones que se usará para el espacio de nombres.

**$TrustedNetwork**. Cadena separada por comas para identificar la red de confianza. VPN no conecta automáticamente cuando el usuario está en su red inalámbrica corporativa donde los recursos protegidos son accesibles directamente para el dispositivo.

A continuación se muestran ejemplos de valores de parámetros que se usan en los siguientes comandos. Asegúrate de que puedes cambiar estos valores para el entorno.

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### Preparar y crear el XML de perfil

Los siguientes comandos de ejemplo Obtén configuración de EAP desde el perfil de plantilla:


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### Crear el XML de perfil

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
     <AlwaysOn>true</AlwaysOn>
     <RememberCredentials>true</RememberCredentials>
     <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'


### VPN_Profile.xml de salida de Intune

Puedes usar el siguiente comando de ejemplo para guardar el archivo XML de perfil:

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### VPN_Profile.ps1 de salida para el escritorio y System Center Configuration Manager

El siguiente código de ejemplo configura una conexión a VPN IKEv2 AlwaysOn mediante el nodo ProfileXML en el CSP de VPNv2.

Puedes usar este script en el escritorio de Windows 10 o en System Center Configuration Manager.

### Definir los parámetros de perfil VPN de clave

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## Definir ProfileXML VPN

    $ProfileXML = ''' + $ProfileXML + '''
    
### Caracteres especiales de escape en el perfil

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### Definir las propiedades de puente de WMI a CSP

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### Determinar el SID del usuario para el perfil de VPN:

    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    

### Definir la sesión WMI:

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### Detectar y eliminar el perfil VPN anterior:

    try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    

### Crear el perfil de VPN:

    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."


        Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to create $ProfileName profile: $_"
    Write-Host "$Message"
    exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'

### Guarda el archivo XML de perfil

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>Script de MakeProfile.ps1 completo

La mayoría de los ejemplos usan el cmdlet Set-WmiInstance Windows PowerShell para insertar ProfileXML en una nueva instancia de la clase WMI MDM_VPNv2_01. 

Sin embargo, esto no funciona en System Center Configuration Manager porque no se puede ejecutar el paquete en el contexto de los usuarios finales. Por lo tanto, este script usa el modelo de información común para crear una sesión WMI en el contexto del usuario y, a continuación, crea una nueva instancia de la clase WMI MDM_VPNv2_01 en esa sesión. Esta clase WMI usa el puente de WMI a CSP para configurar VPNv2 CSP. Por lo tanto, mediante la adición de la instancia de clase, configura el CSP. 

>[!IMPORTANT]
>Puente de WMI a CSP requiere derechos de administrador local, por diseño. Para implementar por perfiles de VPN de usuario deben usar SCCM o MDM.

>[!NOTE]
>El script VPN_Profile.ps1 usa a SID del usuario actual para identificar el contexto del usuario. Dado que ningún SID está disponible en una sesión de escritorio remoto, el script no funciona en una sesión de escritorio remoto. De igual modo, no funciona en una sesión de Hyper-V mejorado. Si estás probando una siempre en VPN de acceso remoto en máquinas virtuales, deshabilitar sesión mejorada en el cliente de las máquinas virtuales antes de ejecutar este script.

El siguiente script de ejemplo incluye todos los ejemplos de código de las secciones anteriores. Asegúrate de que puedes cambiar los valores de ejemplo a valores que son adecuados para tu entorno.
    
   ```XML
    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'

    
    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml
    
    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'
    
    $ProfileXML = ''' + $ProfileXML + '''
    
    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"
    
    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    
    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
    
        try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."

        Write-Host "$Message"
    }
    catch [Exception]
    {
        $Message = "Unable to create $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## Configurar al cliente VPN mediante Windows PowerShell

Para configurar VPNv2 CSP en un equipo cliente de Windows 10, ejecuta el script VPN_Profile.ps1 Windows PowerShell que creaste en la sección [crear el XML de perfil](#create-the-profile-xml) . Abre Windows PowerShell como administrador; de lo contrario, recibirás un mensaje de error diciendo, _acceso denegado_.

Después de ejecutar VPN_Profile.ps1 para configurar el perfil de VPN, puedes comprobar en cualquier momento que se realizó correctamente ejecutando el siguiente comando en Windows PowerShell ISE:

    Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01

**Resultados se realiza correctamente desde el cmdlet Get-WmiObject**


    __GENUS : 2
    __CLASS : MDM_VPNv2_01
    __SUPERCLASS:
    __DYNASTY   : MDM_VPNv2_01
    __RELPATH   : MDM_VPNv2_01.InstanceID="Contoso%20AlwaysOn%20VPN",ParentID
      ="./Vendor/MSFT/VPNv2"
    __PROPERTY_COUNT: 10
    __DERIVATION: {}
    __SERVER: WIN01
    __NAMESPACE : root\cimv2\mdm\dmmap
    __PATH  : \\WIN01\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="Conto
      so%20AlwaysOn%20VPN",ParentID="./Vendor/MSFT/VPNv2"
    AlwaysOn: True
    ByPassForLocal  :
    DnsSuffix   : corp.contoso.com
    EdpModeId   :
    InstanceID  : Contoso%20AlwaysOn%20VPN
    LockDown:
    ParentID: ./Vendor/MSFT/VPNv2
    ProfileXML  : <VPNProfile><RememberCredentials>true</RememberCredentials>
      <AlwaysOn>true</AlwaysOn><DnsSuffix>corp.contoso.com</DnsSu
      ffix><TrustedNetworkDetection>corp.contoso.com</TrustedNetw
      orkDetection><NativeProfile><Servers>vpn.contoso.com;vpn.co
      ntoso.com</Servers><RoutingPolicyType>SplitTunnel</RoutingP
      olicyType><NativeProtocolType>Ikev2</NativeProtocolType><Au
      thentication><UserMethod>Eap</UserMethod><MachineMethod>Eap
      </MachineMethod><Eap><Configuration><EapHostConfig xmlns="h
      ttp://www.microsoft.com/provisioning/EapHostConfig"><EapMet
      hod><Type xmlns="https://www.microsoft.com/provisioning/EapC
      ommon">25</Type><VendorId xmlns="https://www.microsoft.com/p
      rovisioning/EapCommon">0</VendorId><VendorType xmlns="http:
      //www.microsoft.com/provisioning/EapCommon">0</VendorType><
      AuthorId xmlns="https://www.microsoft.com/provisioning/EapCo
      mmon">0</AuthorId></EapMethod><Config xmlns="https://www.mic
      rosoft.com/provisioning/EapHostConfig"><Eap xmlns="https://w
      ww.microsoft.com/provisioning/BaseEapConnectionPropertiesV1
      "><Type>25</Type><EapType xmlns="https://www.microsoft.com/p
      rovisioning/MsPeapConnectionPropertiesV1"><ServerValidation
      ><DisableUserPromptForServerValidation>true</DisableUserPro
      mptForServerValidation><ServerNames>NPS</ServerNames><Trust
      edRootCA>3f 07 88 e8 ac 00 32 e4 06 3f 30 f8 db 74 25 e1
      2e 5b 84 d1 </TrustedRootCA></ServerValidation><FastReconne
      ct>true</FastReconnect><InnerEapOptional>false</InnerEapOpt
      ional><Eap xmlns="https://www.microsoft.com/provisioning/Bas
      eEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="
      https://www.microsoft.com/provisioning/EapTlsConnectionPrope
      rtiesV1"><CredentialsSource><CertificateStore><SimpleCertSe
      lection>true</SimpleCertSelection></CertificateStore></Cred
      entialsSource><ServerValidation><DisableUserPromptForServer
      Validation>true</DisableUserPromptForServerValidation><Serv
      erNames>NPS</ServerNames><TrustedRootCA>3f 07 88 e8 ac 00
      32 e4 06 3f 30 f8 db 74 25 e1 2e 5b 84 d1 </TrustedRootCA><
      /ServerValidation><DifferentUsername>false</DifferentUserna
      me><PerformServerValidation xmlns="https://www.microsoft.com
      /provisioning/EapTlsConnectionPropertiesV2">true</PerformSe
      rverValidation><AcceptServerName xmlns="https://www.microsof
      t.com/provisioning/EapTlsConnectionPropertiesV2">true</Acce
      ptServerName></EapType></Eap><EnableQuarantineChecks>false<
      /EnableQuarantineChecks><RequireCryptoBinding>false</Requir
      eCryptoBinding><PeapExtensions><PerformServerValidation xml
      ns="https://www.microsoft.com/provisioning/MsPeapConnectionP
      ropertiesV2">true</PerformServerValidation><AcceptServerNam
      e xmlns="https://www.microsoft.com/provisioning/MsPeapConnec
      tionPropertiesV2">true</AcceptServerName></PeapExtensions><
      /EapType></Eap></Config></EapHostConfig></Configuration></E
      ap></Authentication></NativeProfile><DomainNameInformation>
      <DomainName>corp.contoso.com</DomainName><DnsServers>10.10.
      0.2,10.10.0.3</DnsServers><AutoTrigger>true</AutoTrigger></
      DomainNameInformation></VPNProfile>
    RememberCredentials : True
    TrustedNetworkDetection : corp.contoso.com
    PSComputerName  : WIN01

La configuración de ProfileXML debe ser correcta en la estructura, la revisión ortográfica, configuración y a veces mayúsculas y minúsculas. Si ves algo distinto en estructura a la lista 1, el marcado de ProfileXML es probable que contiene un error.

Si necesitas solucionar el marcado, es más fácil colocarlo en un editor XML que al solucionar el problema en Windows PowerShell ISE. En cualquier caso, empieza con la versión más sencilla del perfil y agregar componentes vuelve a la vez hasta que el problema se produce uno nuevo.

## <a name="vpn-deploy-client-sccm"></a>Configurar al cliente VPN mediante System Center Configuration Manager

En System Center Configuration Manager, puedes implementar perfiles de VPN mediante el uso del nodo ProfileXML CSP, al igual que lo hizo en Windows PowerShell. Aquí, puedes usar el script VPN_Profile.ps1 Windows PowerShell que creaste en la sección [crear los archivos de configuración ProfileXML](#bkmk_ProfileXML).

Para usar System Center Configuration Manager para implementar un perfil siempre en VPN de acceso remoto a los equipos de cliente de Windows 10, debes iniciar mediante la creación de un grupo de equipos o usuarios a la que implementar el perfil. En este escenario, crea un grupo de usuarios para implementar el script de configuración.

### Crear un grupo de usuarios

1.  En la consola de Configuration Manager, abre activos y Compliance\\User colecciones.

2.  En la cinta **Home** , en el grupo de **crear** , haz clic en **Crear recopilación de usuario**.

3.  En la página General, realiza los siguientes pasos:

    a. En **nombre**, escriba **Los usuarios de VPN**.

    b. Haga clic en **Examinar**y haz clic en **Aceptar** **Todos los usuarios** .

    c. Haz clic en **Siguiente**.

4.  En la página de reglas de pertenencia, realiza los siguientes pasos:

    a.  En **las reglas de pertenencia**, haz clic en **Agregar regla**y haz clic en la **Regla directa**. En este ejemplo, vas a agregar usuarios individuales a la colección de usuario. Sin embargo, podrías usar una regla de consulta para agregar usuarios a esta colección dinámicamente para una implementación de gran escala.

    b.  En la página **principal** , haz clic en **siguiente**.

    c.  En la página de búsqueda de recursos, en un **valor**, escribe el nombre del usuario que quieres agregar. El nombre del recurso incluye el dominio del usuario. Para incluir los resultados en función de una coincidencia parcial, insertar el **%** carácter en los extremos de los criterios de búsqueda. Por ejemplo, para buscar todos los usuarios que contiene la cadena "Elena", escribe **% Elena %**. Haz clic en **Siguiente**.

    d.  En la página Selección de recursos, selecciona los usuarios que quieras agregar al grupo y haga clic en **siguiente**.

    e.  En la página de resumen, haz clic en **siguiente**.

    f.  En la página Finalización, haz clic en **Cerrar**.

6.  Vuelve a activar la página de reglas de pertenencia del Asistente para crear recopilación de usuario, haz clic en **siguiente**.

7.  En la página de resumen, haz clic en **siguiente**.

8.  En la página Finalización, haz clic en **Cerrar**.

Después de crear el grupo de usuarios para que pueda recibir el perfil de VPN, puedes crear un paquete y el programa para implementar el script de configuración de Windows PowerShell que creaste en la sección [crear los archivos de configuración ProfileXML](#bkmk_ProfileXML).

### Crear un paquete que contiene el script de configuración de ProfileXML

1.  Hospedar el script VPN_Profile.ps1 en un recurso compartido de red que puede tener acceso la cuenta de equipo de servidor de sitio.

2.  En la consola de Configuration Manager, abre **Aplicaciones\\paquetes Library\\Application de Software**.

3.  En la cinta **Home** , en el grupo de **crear** , haz clic en **Crear un paquete** para iniciar el Asistente de programa y crear un paquete.

4.  En la página de paquete, realiza los siguientes pasos:

    a. En **nombre**, escriba **Windows 10 siempre en perfil de VPN**.

    b. Selecciona la casilla de verificación **este paquete contiene archivos de origen** y haga clic en **Examinar**.

    c. En el cuadro de diálogo Establecer la carpeta de origen, haga clic en **Examinar**, selecciona el recurso compartido de archivos que contengan VPN_Profile.ps1 y haz clic en **Aceptar**.<p>Asegúrate de que seleccionar una ruta de acceso de red, no una ruta de acceso local. En otras palabras, la ruta de acceso debe ser similar a *\\fileserver\\vpnscript*, no *c:\\vpnscript*.

1.  Haz clic en **Siguiente**.

2.  En la página de tipo de programa, haz clic en **siguiente**.

3.  En la página programa estándar, realiza los siguientes pasos:

    a.  En **nombre**, escriba el **Script de perfil de VPN**.

    b.  En la **línea de comandos**, escriba **PowerShell.exe - ExecutionPolicy Omitir - archivo "VPN_Profile.ps1"**.

    c.  En el **modo de ejecución**, haz clic en **Ejecutar con derechos administrativos**.

    d.  Haz clic en **Siguiente**.

4.  En la página de requisitos, realiza los siguientes pasos:

    a.  Selecciona **este programa se puede ejecutar solo en las plataformas especificadas**.

    b.  Selecciona las casillas de verificación **todo Windows 10 (32 bits)** y **todos los Windows 10 (64 bits)** .

    c.  En el **espacio en disco estimado**, escribe **1**.

    d.  En **tiempo de ejecución (minutos) máximo permitido**, escriba **15**.

    e.  Haz clic en **Siguiente**.

5.  En la página de resumen, haz clic en **siguiente**.

6.  En la página Finalización, haz clic en **Cerrar**.

Con el paquete y el programa creado, debes implementarla en el grupo de **Usuarios de VPN** .

### Implementar el script de configuración de ProfileXML

1.  En la consola de Configuration Manager, abre aplicaciones\\paquetes Library\\Application de Software.

2.  En **los paquetes**, haz clic en **Windows 10 siempre en perfil de VPN**.

3.  En la ficha **programas** , en la parte inferior del panel de detalles, haz clic en **El Script de perfil de VPN**, haga clic en **Propiedades**y realiza los siguientes pasos:

    a.  En la pestaña **Opciones avanzadas** , en **cuando este programa se asigna a un equipo**, haz clic en **una vez para cada usuario que inicie sesión**.

    b.  Haz clic en **Aceptar**.

4.  Haz clic en **El Script de perfil de VPN** y haga clic en **implementar** para iniciar al Asistente para implementar Software.

5.  En la página General, realiza los siguientes pasos:

    a.  Junto a la **colección**, haga clic en **Examinar**.

    b.  En la lista de **Los tipos de colección** (esquina superior izquierda), haz clic en **Las colecciones de usuario**.

    c.  Haz clic en **Los usuarios de VPN**y haz clic en **Aceptar**.

    d.  Haz clic en **Siguiente**.

6.  En la página de contenido, realiza los siguientes pasos:

    a.  Haz clic en **Agregar**y haz clic en el **Punto de distribución**.

    b.  En **los puntos de distribución disponibles**, selecciona los puntos de distribución a la que quieres distribuir el script de configuración de ProfileXML y haz clic en **Aceptar**.

    c.  Haz clic en **Siguiente**.

7.  En la página de configuración de implementación, haz clic en **siguiente**.

8.  En la página de programación, realiza los siguientes pasos:

    a.  Haz clic en **nuevo** para abrir el cuadro de diálogo de programación de asignación.

    b.  Haz clic en **asignar inmediatamente después de este evento**y haz clic en **Aceptar**.

    c.  Haz clic en **Siguiente**.

9.  En la página experiencia del usuario, realiza los siguientes pasos:

    1.  Selecciona la casilla de verificación de **Instalación de Software** .

    2.  Haz clic en el **Resumen**.

10. En la página de resumen, haz clic en **siguiente**.

11. En la página Finalización, haz clic en **Cerrar**.

Con el script de configuración de ProfileXML implementado, inicia sesión en un equipo cliente de Windows 10 con la cuenta de usuario que ha seleccionado al crear la colección de usuarios. Comprobar la configuración del cliente VPN.

>[!NOTE]
>El script VPN_Profile.ps1 no funciona en una sesión de escritorio remoto. De igual modo, no funciona en una sesión de Hyper-V mejorado. Si estás probando una siempre en VPN de acceso remoto en máquinas virtuales, deshabilitar sesión mejorada en el cliente de las máquinas virtuales antes de continuar.

### Comprobar la configuración del cliente VPN

1.  En el Panel de Control, **System\\Security**, haga clic en **Configuration Manager**. 

2.  En el cuadro de diálogo de propiedades de Configuration Manager, en la pestaña **acciones** , realiza los siguientes pasos:

    a.  Haz clic en **& ciclo de evaluación de recuperación de directivas de equipo**, haz clic en **Ejecutar ahora**y haz clic en **Aceptar**.

    b.  Haz clic en **& ciclo de evaluación de recuperación de directivas de usuario**, haz clic en **Ejecutar ahora**y haz clic en **Aceptar**.

    c.  Haz clic en **Aceptar**.

3.  Cierra el Panel de Control.

Deberías ver el nuevo perfil VPN en breve.

## Configurar al cliente VPN mediante Intune

Para usar Intune para implementar perfiles de Windows 10 siempre en VPN de acceso remoto, puedes configurar el nodo ProfileXML CSP mediante el perfil VPN que creaste en la sección [crear los archivos de configuración ProfileXML](#bkmk_ProfileXML)o puedes usar la muestra de EAP XML base proporcionada a continuación.

>[!NOTE]
>Ahora, Intune usa grupos de Azure AD. Si Azure AD Connect sincroniza el grupo de usuarios de VPN local a Azure AD y se asignan a los usuarios al grupo usuarios de VPN, estás listo para continuar.

Crear la directiva de configuración del dispositivo VPN para configurar los equipos de cliente de Windows 10 para todos los usuarios agregados al grupo. Dado que la plantilla de Intune proporciona los parámetros VPN, solo copia la parte \<EapHostConfig> \</EapHostConfig> del archivo VPN_ProfileXML. 


### Crear la directiva de configuración de VPN de Always On

1.  Inicia sesión en el [portal de Azure](https://portal.azure.com/).

2.  Ve a **Intune** > **Configuración del dispositivo** > **perfiles**.

3.  Haz clic en **Crear perfil** para iniciar el Asistente para crear perfil.

4.  Escribe un **nombre** para el perfil de VPN y (opcionalmente) una descripción.

5.   En la **plataforma**, selecciona **Windows 10 o posterior**y elige **VPN** de la lista desplegable de tipo de perfil.

     >[!TIP]
     >Si vas a crear un profileXML VPN personalizado, vea [Aplicar ProfileXML con Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) para las instrucciones.

6. En la pestaña de la **Base de VPN** , comprobar o establecer la configuración siguiente:

    - **Nombre de la conexión:** Escribe el nombre de la conexión VPN tal y como aparece en el equipo cliente en la pestaña **VPN** en **configuración**, por ejemplo, _Contoso AutoVPN_.  
    
    - **Servidores:** Agregar uno o más servidores VPN haciendo clic en **Agregar.**
    
    - **Descripción** y **dirección IP o FQDN:** escribe la descripción y la dirección IP o el FQDN del servidor VPN. Estos valores se deben alinear con el nombre del sujeto del certificado de autenticación del servidor VPN. 
    
    - **Servidor predeterminado:** Si este es el servidor VPN de forma predeterminada, se establece en **True**. Esto permite a este servidor como el servidor predeterminado que los dispositivos se usan para establecer la conexión. 
    
    - **Tipo de conexión:** Establece en **IKEv2**.  
    
    - **Siempre activado:** Establecer para **Habilitar** para conectarse a la VPN automáticamente al iniciar sesión y permanece conectada hasta que el usuario la desconecte manualmente.
    
    - **Recordar credenciales cada vez que inicie sesión**: valor booleano (verdadero o falso) para almacenar en caché las credenciales. Si se establece en true, credenciales se almacenan en caché siempre que sea posible.

7.  Copie la siguiente cadena XML en un editor de texto:<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  Reemplaza el **\<TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1C c2 68 ser 4b<\/TrustedRootCA>** en la muestra con huella digital del certificado de la entidad de certificación raíz de forma local en ambos lugares.
  
    >[!Important]
    >No uses la huella digital de muestra en la sección \<TrustedRootCA>\</TrustedRootCA> a continuación.  El TrustedRootCA debe ser la huella digital del certificado de la entidad de certificación raíz de local que emitió el certificado de autenticación de servidor para servidores RRAS y NPS. **Este no debe ser el certificado raíz en la nube, ni la huella digital intermedia certificado de entidad de certificación emisora**.

10. Reemplaza el **\<ServerNames>NPS.contoso.com\</ServerNames>** en la muestra de XML con el FQDN de NPS unido al dominio donde realiza la autenticación. 

11. Copiar la cadena XML revisada y pegarlo en el cuadro de **EAP Xml** en la pestaña de VPN de Base y haz clic en **Aceptar**.<p>Se crea una directiva siempre en dispositivo configuración de VPN con EAP en Intune.


### Sincronización de la directiva de configuración de VPN de Always On con Intune

Para probar la directiva de configuración, iniciar sesión en un equipo cliente de Windows 10 como el usuario que haya agregado al grupo **Siempre en los usuarios de VPN** y, a continuación, se sincronicen con Intune.

1.  En el menú Inicio, haz clic en **configuración**.

2.  En la configuración, haz clic en **las cuentas**y haz clic en **obtener acceso a trabajo o escuela**.

3.  Haz clic en el perfil MDM y haz clic en **información**.

4.  Haz clic en **la sincronización** para forzar una evaluación de la directiva de Intune y la recuperación.

5.  Cierra la configuración. Después de la sincronización, verás el perfil de VPN disponible en el equipo.

## Paso siguiente
Se realizan implementar VPN de Always On.  Para otras características que se puede configurar, consulta la siguiente tabla:

|Si quieres …  |A continuación, consulta …  |
|---------|---------|
|Configurar el acceso condicional para VPN    |Paso 7 de [. (Opcional) Configurar el acceso condicional para la conectividad VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md): en este paso, puede ajustar el acceso de usuarios VPN autorizado cómo los recursos con el [acceso condicional de Azure Active Directory (AD Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con el acceso condicional de Azure AD para la conectividad de red privada virtual (VPN), puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD).         |
|Más información sobre las características avanzadas de VPN  |[Características avanzadas de VPN](always-on-vpn-adv-options.md#advanced-vpn-features): esta página contiene instrucciones sobre cómo habilitar filtros de tráfico VPN, cómo configurar conexiones VPN automática mediante desencadenadores de la aplicación y cómo configurar NPS para permitir solo las conexiones VPN de clientes que usan certificados emitidos por Azure AD.        |


---
