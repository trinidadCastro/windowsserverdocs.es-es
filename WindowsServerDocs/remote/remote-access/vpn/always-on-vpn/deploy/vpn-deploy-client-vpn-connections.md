---
title: Configurar las conexiones VPN de Always On del cliente de Windows 10
description: En este paso, obtendrá información sobre las opciones y el esquema de ProfileXML, y configurará los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: c853926157a4efa414c56d97affa0a1d2faad629
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314348"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Paso 6. Configuración de conexiones VPN de Always On cliente de Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Previo** Paso 5. Definir el DNS y la configuración del firewall](vpn-deploy-dns-firewall.md)<br>
- [**Nueva** Paso 7. Opta Acceso condicional para la conectividad VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

En este paso, obtendrá información sobre las opciones y el esquema de ProfileXML, y configurará los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN.

Puede configurar el cliente VPN de Always On a través de PowerShell, SCCM o Intune. Los tres requieren un perfil de VPN de XML para establecer la configuración de VPN adecuada. Es posible automatizar la inscripción de PowerShell para organizaciones sin SCCM o Intune.

>[!NOTE]
>Directiva de grupo no incluye plantillas administrativas para configurar el cliente VPN de acceso remoto de Windows 10 Always On.  Sin embargo, puede usar scripts de inicio de sesión.

## <a name="profilexml-overview"></a>Información general de ProfileXML

ProfileXML es un nodo URI dentro del CSP VPNv2. En lugar de configurar cada nodo de CSP de VPNv2 individualmente (por ejemplo, los desencadenadores, las listas de rutas y los protocolos de autenticación), use este nodo para configurar un cliente VPN de Windows 10. para ello, entregue toda la configuración como un solo bloque XML a un solo nodo de CSP. El esquema ProfileXML coincide con el esquema de los nodos de CSP VPNv2 casi idénticos, pero algunos términos son ligeramente diferentes.

Use ProfileXML en todos los métodos de entrega que describe esta implementación, incluidos Windows PowerShell, System Center Configuration Manager e Intune. Hay dos maneras de configurar el nodo ProfileXML VPNv2 CSP en esta implementación:

- **OMA-DM**. Una manera es usar un proveedor de MDM mediante OMA-DM, como se explicó anteriormente en la sección VPNv2 de los [nodos de CSP](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Con este método, puede insertar fácilmente el marcado XML de configuración del perfil de VPN en el nodo ProfileXML CSP al usar Intune.

- **Puente de instrumental de administración de Windows (WMI) a CSP**. El segundo método para configurar el nodo ProfileXML CSP es usar el puente de WMI a CSP, una clase WMI denominada **MDM_VPNv2_01**, que puede tener acceso al CSP VPNv2 y al nodo ProfileXML. Cuando se crea una nueva instancia de esa clase WMI, WMI usa el CSP para crear el perfil de VPN al usar Windows PowerShell y System Center Configuration Manager.

Aunque estos métodos de configuración difieren, ambos requieren un perfil de VPN XML con el formato correcto. Para usar la configuración de ProfileXML VPNv2 CSP, cree XML mediante el esquema ProfileXML para configurar las etiquetas necesarias para el escenario de implementación simple. Para obtener más información, vea [PROFILEXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

A continuación, encontrará cada una de las configuraciones necesarias y su etiqueta ProfileXML correspondiente. Configure cada opción en una etiqueta específica dentro del esquema ProfileXML y no todas ellas se encuentran en el perfil nativo. Para obtener más ubicación de etiquetas, consulte el esquema ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo de conexión:** IKEv2 nativo

Elemento ProfileXML:

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**Automático** Tunelización dividida

Elemento ProfileXML:

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**Resolución de nombres:** Lista de información de nombres de dominio y sufijo DNS

Elementos ProfileXML:

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**Desencadenar** Always On y detección de redes de confianza

Elementos ProfileXML:

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**Autenticación** PEAP-TLS con certificados de usuario protegidos con TPM

Elementos ProfileXML:

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

Puede usar etiquetas simples para configurar algunos mecanismos de autenticación de VPN. Sin embargo, EAP y PEAP son más complicados. La forma más fácil de crear el marcado XML es configurar un cliente VPN con su configuración de EAP y, a continuación, exportar esa configuración a XML.

Para obtener más información acerca de la configuración de EAP, consulte [configuración de EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration).

## <a name="manually-create-a-template-connection-profile"></a>Crear manualmente un perfil de conexión de plantilla

En este paso, se usa el protocolo de autenticación extensible protegido (PEAP) para proteger la comunicación entre el cliente y el servidor. A diferencia de un nombre de usuario y una contraseña simples, esta conexión requiere una sección EAPConfiguration única en el perfil de VPN para que funcione.

En lugar de describir cómo crear el marcado XML desde cero, use la configuración de Windows para crear un perfil de VPN de plantilla. Después de crear el perfil de VPN de plantilla, use Windows PowerShell para consumir la parte EAPConfiguration de esa plantilla con el fin de crear el ProfileXML final que implementará más adelante en la implementación.

### <a name="record-nps-certificate-settings"></a>Grabar configuración de certificado NPS

Antes de crear la plantilla, tome nota del nombre de host o el nombre de dominio completo (FQDN) del servidor NPS del certificado del servidor y el nombre de la CA que emitió el certificado.

**Pasos**

1. En el servidor NPS, abra el servidor de directivas de redes.

2. En la consola de NPS, en directivas, haga clic en **directivas de red**.

3. Haga clic con el botón secundario en **conexiones de red privada virtual (VPN)** y haga clic en **propiedades**.

4. Haga clic en la pestaña **restricciones** y en **métodos de autenticación**.

5. En tipos de EAP, **haga clic en Microsoft: EAP protegido (PEAP)** y haga clic en **Editar**.

6. Registre los valores del **certificado emitido para** y el **emisor**.

    Estos valores se usan en la próxima configuración de plantilla de VPN. Por ejemplo, si el FQDN del servidor es nps01.corp.contoso.com y el nombre del host es NPS01, el nombre del certificado se basa en el FQDN o el nombre DNS del servidor, por ejemplo, nps01.corp.contoso.com.

7. Cancele el cuadro de diálogo Editar propiedades de EAP protegido.

8. Cancele el cuadro de diálogo Propiedades de las conexiones de red privada virtual (VPN).

9. Cierre el servidor de directivas de redes.

>[!NOTE]
>Si tiene varios servidores NPS, siga estos pasos en cada uno de ellos para que el perfil de VPN pueda comprobar cada uno de ellos en caso de que se usen.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurar el perfil de VPN de plantilla en un equipo cliente unido a un dominio

Ahora que tiene la información necesaria, configure el perfil de VPN de plantilla en un equipo cliente unido a un dominio. No importa el tipo de cuenta de usuario que use (es decir, el usuario o administrador estándar) para esta parte del proceso.

Sin embargo, si no ha reiniciado el equipo desde la configuración de la inscripción automática de certificados, hágalo antes de configurar la conexión VPN de la plantilla para asegurarse de que tiene un certificado que se puede usar inscrito en él.

>[!NOTE]
>No hay ninguna manera de agregar manualmente las propiedades avanzadas de VPN, como las reglas de NRPT, Always On, la detección de redes de confianza, etc. En el paso siguiente, se crea una conexión VPN de prueba para comprobar la configuración del servidor VPN y se puede establecer una conexión VPN al servidor.

**Crear manualmente una sola conexión VPN de prueba**

1.  Inicie sesión en un equipo cliente unido a un dominio como miembro del grupo de **usuarios de VPN** .

2.  En el menú Inicio, escriba **VPN**y presione Entrar.

3.  En el panel de detalles, haga clic en **Agregar una conexión VPN**.

4.  En la lista proveedor de VPN, haga clic en **Windows (integrado)** .

5.  En nombre de la conexión, escriba **plantilla**.

6.  En nombre o dirección del servidor, escriba el FQDN **externo** del servidor VPN (por ejemplo, **VPN.contoso.com**).

7.  Haga clic en **Guardar**.

8.  En configuración relacionada, haga clic en **cambiar opciones de adaptador**.

9.  Haga clic con el botón derecho en **plantilla**y haga clic en **propiedades**.

10. En la pestaña **seguridad** , en **tipo de VPN**, haga clic en **IKEv2**.

11. En cifrado de datos, haga clic en cifrado de **máximo nivel**.

12. Haga clic en **usar protocolo de autenticación extensible (EAP)** . a continuación, en **usar protocolo de autenticación extensible (EAP)** , haga **clic en Microsoft: EAP protegido (PEAP) (cifrado habilitado)** .

13. Haga clic en **propiedades** para abrir el cuadro de diálogo Propiedades de EAP protegido y realice los pasos siguientes:

    a. En el cuadro **conectar a estos servidores** , escriba el nombre del servidor NPS que recuperó en la configuración de autenticación del servidor NPS anteriormente en esta sección (por ejemplo, NPS01).

    >[!NOTE]
    >El nombre del servidor que escriba debe coincidir con el nombre del certificado. Ha recuperado este nombre anteriormente en esta sección. Si el nombre no coincide, se producirá un error en la conexión y se indicará "se impidió la conexión debido a una directiva configurada en el servidor RAS/VPN".

    b.  En entidades de certificación raíz de confianza, seleccione la CA raíz que emitió el certificado del servidor NPS (por ejemplo, contoso-CA).

    c.  En notificaciones antes de la conexión, haga clic en **no pedir al usuario que autorice nuevos servidores o entidades de certificación de confianza**.

    d.  En seleccionar método de autenticación, haga clic en **tarjeta inteligente u otro certificado**y, a continuación, haga clic en **configurar**. Se abre el cuadro de diálogo Propiedades de tarjeta inteligente u otro certificado.

    e.  Haga clic en **usar un certificado en este equipo**.

    f.  En el cuadro conectar a estos servidores, escriba el nombre del servidor NPS que recuperó en la configuración de autenticación del servidor NPS en los pasos anteriores.

    g.  En entidades de certificación raíz de confianza, seleccione la CA raíz que emitió el certificado del servidor NPS.

    h.  Active la casilla **no solicitar al usuario que autorice nuevos servidores o entidades de certificación de confianza** .

    i.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de tarjeta inteligente u otro certificado.

    j.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de EAP protegido.

14. Haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de la plantilla.

15. Cierre la ventana conexiones de red.

16. En configuración, pruebe la VPN; para ello, haga clic en **plantilla**y, a continuación, haga clic en **conectar**.

>[!IMPORTANT]
>Asegúrese de que la conexión VPN de plantilla con el servidor VPN se ha realizado correctamente. Esto garantiza que la configuración de EAP sea correcta antes de usarla en el ejemplo siguiente. Debe conectarse al menos una vez antes de continuar; de lo contrario, el perfil no contendrá toda la información necesaria para conectarse a la VPN.

## <a name="create-the-profilexml-configuration-files"></a>Crear los archivos de configuración de ProfileXML

Antes de completar esta sección, asegúrese de que ha creado y probado la conexión VPN de plantilla que describe la sección [creación manual de un perfil de conexión de plantillas](#manually-create-a-template-connection-profile) . Es necesario probar la conexión VPN para asegurarse de que el perfil contiene toda la información necesaria para conectarse a la VPN.

El script de Windows PowerShell de la lista 1 crea dos archivos en el escritorio, que contienen etiquetas **EAPConfiguration** basadas en el perfil de conexión de plantilla que creó anteriormente:

- **VPN_Profile. Xml.** Este archivo contiene el marcado XML necesario para configurar el nodo ProfileXML en el CSP VPNv2. Use este archivo con servicios MDM-DM compatibles, como Intune.

- **VPN_Profile. ps1.** Este archivo es un script de Windows PowerShell que se puede ejecutar en los equipos cliente para configurar el nodo ProfileXML en el CSP VPNv2. También puede configurar el CSP implementando este script a través de System Center Configuration Manager. No se puede ejecutar este script en una sesión de Escritorio remoto, incluida una sesión mejorada de Hyper-V.

>[!IMPORTANT]
>Los siguientes comandos de ejemplo requieren la compilación 1607 de Windows 10 o posterior.

**Crear VPN_Profile. XML y VPN_Proflie. ps1**

1. Inicie sesión en el equipo cliente unido a un dominio que contiene el perfil de VPN de plantilla con la misma cuenta de usuario que la sección [crea manualmente un perfil de conexión de plantilla](#manually-create-a-template-connection-profile) descrito.

2. Pegue la lista 1 en el entorno de scripting integrado (ISE) de Windows PowerShell y personalice los parámetros descritos en los comentarios. Se trata de $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork y $DNSServers. Una descripción completa de cada opción de configuración se encuentra en los comentarios.

3. Ejecute el script para generar **VPN_Profile. XML** y **VPN_Profile. PS1** en el escritorio.

#### <a name="listing-1-understanding-makeprofileps1"></a>Lista 1. Descripción de MakeProfile. ps1

En esta sección se explica el código de ejemplo que puede usar para comprender cómo crear un perfil de VPN, específicamente para configurar ProfileXML en el CSP VPNv2.

Después de ensamblar un script de este código de ejemplo y ejecutar el script, el script genera dos archivos: VPN_Profile. XML y VPN_Profile. ps1. Use VPN_Profile. XML para configurar ProfileXML en servicios MDM-DM compatibles, como Microsoft Intune.

Use el script **VPN_Profile. PS1** de Windows PowerShell o System Center Configuration Manager para configurar ProfileXML en el escritorio de Windows 10.

>[!NOTE]
>Para ver el script de ejemplo completo, vea la sección [script completo MakeProfile. PS1](#makeprofileps1-full-script).

#### <a name="parameters"></a>Parámetros

Configure los parámetros siguientes:

**$Template**. Nombre de la plantilla de la que se va a recuperar la configuración de EAP.

**$ProfileName**. Identificador alfanumérico único para el perfil. El nombre del perfil no debe incluir una barra diagonal (/). Si el nombre del perfil tiene un espacio u otros caracteres no alfanuméricos, debe tener un carácter de escape correcto según el estándar de codificación de direcciones URL.

**$Servers**. Dirección IP pública o enrutable o nombre DNS para la puerta de enlace de VPN. Puede apuntar a la dirección IP externa de una puerta de enlace o de una dirección IP virtual para una granja de servidores. Ejemplos, 208.147.66.130 o vpn.contoso.com.

**$DnsSuffix**. Especifica una o más sufijos DNS separados por comas. La primera de la lista también se utiliza como sufijo DNS específico de la conexión principal para la interfaz VPN. La lista completa también se agregará a SuffixSearchList.

**$Domainname**. Se usa para indicar el espacio de nombres al que se aplica la Directiva. Cuando se emite una consulta de nombre, el cliente DNS compara el nombre de la consulta con todos los espacios de nombres de DomainNameInformationList para encontrar una coincidencia. Este parámetro puede ser uno de los siguientes tipos:

- FQDN: nombre de dominio completo
- Sufijo: sufijo de dominio que se anexará a la consulta de ShortName para la resolución de DNS. Para especificar un sufijo, anteponga un punto (.) al sufijo DNS.

**$DNSServers**. Lista de direcciones IP de servidores DNS separados por comas que se usarán para el espacio de nombres.

**$TrustedNetwork**. Cadena separada por comas para identificar la red de confianza. VPN no se conecta automáticamente cuando el usuario está en su red inalámbrica corporativa, donde los recursos protegidos son directamente accesibles para el dispositivo.

A continuación se muestran los valores de ejemplo para los parámetros que se usan en los siguientes comandos. Asegúrese de cambiar estos valores para su entorno.

```xml
$TemplateName = 'Template'
$ProfileName = 'Contoso AlwaysOn VPN'
$Servers = 'vpn.contoso.com'
$DnsSuffix = 'corp.contoso.com'
$DomainName = '.corp.contoso.com'
$DNSServers = '10.10.0.2,10.10.0.3'
$TrustedNetwork = 'corp.contoso.com'
```

### <a name="prepare-and-create-the-profile-xml"></a>Preparar y crear el perfil XML

En los siguientes comandos de ejemplo se obtiene la configuración de EAP del perfil de plantilla:

```xml
$Connection = Get-VpnConnection -Name $TemplateName
if(!$Connection)
{
$Message = "Unable to get $TemplateName connection profile: $_"
Write-Host "$Message"
exit
}
$EAPSettings= $Connection.EapConfigXmlStream.InnerXml
```

### <a name="create-the-profile-xml"></a>Crear el perfil XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

```xml
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
```

### <a name="output-vpnprofilexml-for-intune"></a>Salida de VPN_Profile. XML para Intune

Puede usar el siguiente comando de ejemplo para guardar el archivo XML de perfil:

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>Salida VPN_Profile. PS1 para el escritorio y System Center Configuration Manager

En el código de ejemplo siguiente se configura una conexión VPN de IKEv2 AlwaysOn mediante el nodo ProfileXML en el CSP VPNv2.

Puede usar este script en el escritorio de Windows 10 o en System Center Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Definir parámetros clave del perfil de VPN

```xml
$Script = '$ProfileName = ''' + $ProfileName + ''''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

## <a name="define-vpn-profilexml"></a>Definir ProfileXML VPN

```xml
$ProfileXML = ''' + $ProfileXML + '''
```

### <a name="escape-special-characters-in-the-profile"></a>Caracteres especiales de escape en el perfil

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>Definir propiedades de puente de WMI a CSP

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>Determinar el SID de usuario para el perfil de VPN:

```xml
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
```

### <a name="define-wmi-session"></a>Definir sesión WMI:

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>Detectar y eliminar el perfil de VPN anterior:

```xml
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
```

### <a name="create-the-vpn-profile"></a>Cree el perfil de VPN:

```xml
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
Write-Host "$Message"
```

### <a name="save-the-profile-xml-file"></a>Guardar el archivo XML de perfil

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>Script completo de MakeProfile. ps1

La mayoría de los ejemplos usan el cmdlet Set-WmiInstance de Windows PowerShell para insertar ProfileXML en una nueva instancia de la clase WMI MDM_VPNv2_01. 

Sin embargo, esto no funciona en System Center Configuration Manager porque no se puede ejecutar el paquete en el contexto de los usuarios finales. Por lo tanto, este script utiliza el Modelo de información común para crear una sesión WMI en el contexto del usuario y, a continuación, crea una nueva instancia de la clase WMI MDM_VPNv2_01 en esa sesión. Esta clase WMI usa el puente de WMI a CSP para configurar el CSP VPNv2. Por lo tanto, al agregar la instancia de clase, se configura el CSP. 

>[!IMPORTANT]
>El puente de WMI a CSP requiere derechos de administrador local, por diseño. Para implementar perfiles de VPN por usuario, debe usar SCCM o MDM.

>[!NOTE]
>El script VPN_Profile. PS1 usa el SID del usuario actual para identificar el contexto del usuario. Dado que no hay ningún SID disponible en una sesión de Escritorio remoto, el script no funciona en una sesión de Escritorio remoto. Del mismo modo, no funciona en una sesión mejorada de Hyper-V. Si va a probar un acceso remoto Always On VPN en máquinas virtuales, deshabilite la sesión mejorada en las máquinas virtuales de cliente antes de ejecutar este script.

El siguiente script de ejemplo incluye todos los ejemplos de código de las secciones anteriores. Asegúrese de cambiar los valores de ejemplo a los valores adecuados para su entorno.
    
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
    
    $ProfileXML = ''' + $ProfileXML + ''''
    
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
    Write-Host "$Message"
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configuración del cliente VPN mediante Windows PowerShell

Para configurar el CSP de VPNv2 en un equipo cliente de Windows 10, ejecute el script de Windows PowerShell VPN_Profile. PS1 que creó en la sección [creación del perfil XML](#create-the-profile-xml) . Abra Windows PowerShell como administrador. de lo contrario, recibirá un error que indica que se denegó el _acceso_.

Después de ejecutar VPN_Profile. PS1 para configurar el perfil de VPN, puede comprobar en cualquier momento que se ejecutó correctamente ejecutando el siguiente comando en el Windows PowerShell ISE:

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**Resultados correctos del cmdlet Get-WmiObject**

```powershell
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
```

La configuración de ProfileXML debe ser correcta en la estructura, la ortografía, la configuración y, a veces, el uso de letras mayúsculas. Si ve algo diferente en la estructura de la lista 1, es probable que el marcado ProfileXML contenga un error.

Si necesita solucionar los problemas de marcado, es más fácil colocarlo en un editor XML que solucionarlo en el Windows PowerShell ISE. En cualquier caso, empiece con la versión más sencilla del perfil y agregue los componentes de nuevo de uno en uno hasta que el problema vuelva a producirse.

## <a name="configure-the-vpn-client-by-using-system-center-configuration-manager"></a>Configurar el cliente VPN mediante System Center Configuration Manager

En System Center Configuration Manager, puede implementar perfiles de VPN mediante el nodo CSP ProfileXML, tal como hizo en Windows PowerShell. Aquí se usa el script de Windows PowerShell VPN_Profile. PS1 que creó en la sección [creación de los archivos de configuración de ProfileXML](#create-the-profilexml-configuration-files).

Para usar System Center Configuration Manager para implementar un perfil de VPN de Always On de acceso remoto en los equipos cliente de Windows 10, debe empezar por crear un grupo de equipos o usuarios en los que implemente el perfil. En este escenario, cree un grupo de usuarios para implementar el script de configuración.

### <a name="create-a-user-group"></a>Crear un grupo de usuarios

1.  En la consola de Configuration Manager, abra activos y\\compatibilidad recopilaciones de usuarios.

2.  En la cinta **Inicio** , en el grupo **crear** , haga clic en **crear recopilación de usuarios**.

3.  En la página general, complete los pasos siguientes:

    a. En **nombre**, escriba **usuarios de VPN**.

    b. Haga clic en **examinar**, en **todos los usuarios** y en **Aceptar**.

    c. Haga clic en **Next**.

4.  En la página reglas de pertenencia, complete los pasos siguientes:

    a.  En **reglas**de pertenencia, haga clic en **Agregar regla**y en **regla directa**. En este ejemplo, va a agregar usuarios individuales a la recopilación de usuarios. Sin embargo, puede usar una regla de consulta para agregar usuarios a esta colección de forma dinámica para una implementación a mayor escala.

    b.  En la página **principal**, haga clic en **Siguiente**.

    c.  En la página buscar recursos, en **valor**, escriba el nombre del usuario que desea agregar. El nombre del recurso incluye el dominio del usuario. Para incluir resultados basados en una coincidencia parcial, inserte el **%** carácter en cualquier extremo del criterio de búsqueda. Por ejemplo, para buscar todos los usuarios que contengan la cadena "Lori", escriba **% Lori%** . Haga clic en **Next**.

    d.  En la página seleccionar recursos, seleccione los usuarios que desea agregar al grupo y haga clic en **siguiente**.

    e.  En la página Resumen, haga clic en **siguiente**.

    f.  En la página finalización, haga clic en **cerrar**.

6.  En la página reglas de pertenencia del Asistente para crear recopilación de usuarios, haga clic en **siguiente**.

7.  En la página Resumen, haga clic en **siguiente**.

8.  En la página finalización, haga clic en **cerrar**.

Después de crear el grupo de usuarios para recibir el perfil de VPN, puede crear un paquete y un programa para implementar el script de configuración de Windows PowerShell que creó en la sección [crear los archivos de configuración de ProfileXML](#create-the-profilexml-configuration-files).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Crear un paquete que contenga el script de configuración de ProfileXML

1.  Hospede el script VPN_Profile. PS1 en un recurso compartido de red al que puede tener acceso la cuenta de equipo del servidor de sitio.

2.  En la consola de Configuration Manager, Abra **biblioteca\\de software\\administración de aplicaciones paquetes**.

3.  En la cinta **Inicio** , en el grupo **crear** , haga clic en **crear paquete** para iniciar el Asistente para crear paquetes y programas.

4.  En la página paquete, realice los pasos siguientes:

    a. En **nombre**, escriba **Windows 10 Always on Perfil de VPN**.

    b. Active la casilla **este paquete contiene archivos de origen** y haga clic en **examinar**.

    c. En el cuadro de diálogo establecer carpeta de origen, haga clic en **examinar**, seleccione el recurso compartido de archivos que contiene VPN_Profile. PS1 y haga clic en **Aceptar**.
        Asegúrese de seleccionar una ruta de acceso de red, no una ruta de acceso local. En otras palabras, la ruta de acceso debe ser  *\\similar\\a fileserver vpnscript*, no *c:\\vpnscript*.

1.  Haga clic en **Next**.

2.  En la página tipo de programa, haga clic en **siguiente**.

3.  En la página programa estándar, realice los pasos siguientes:

    a.  En **nombre**, escriba **script de Perfil de VPN**.

    b.  En la **línea de comandos**, escriba **PowerShell. exe-ExecutionPolicy bypass-File "VPN_Profile. PS1"** .

    c.  En el **modo de ejecución**, haga clic en **ejecutar con derechos administrativos**.

    d.  Haga clic en **Next**.

4.  En la página requisitos, complete los pasos siguientes:

    a.  Seleccione **este programa solo puede ejecutarse en las plataformas especificadas**.

    b.  Active las casillas **todo Windows 10 (32 bits)** y **todas las ventanas de windows 10 (64 bits)** .

    c.  En **espacio en disco Estimado**, escriba **1**.

    d.  En **tiempo de ejecución máximo permitido (minutos)** , escriba **15**.

    e.  Haga clic en **Next**.

5.  En la página Resumen, haga clic en **siguiente**.

6.  En la página finalización, haga clic en **cerrar**.

Con el paquete y el programa creados, debe implementarlo en el grupo de **usuarios de VPN** .

### <a name="deploy-the-profilexml-configuration-script"></a>Implementación del script de configuración de ProfileXML

1.  En la consola de Configuration Manager, abra biblioteca\\de software\\administración de aplicaciones paquetes.

2.  En **paquetes**, haga clic en **Windows 10 Always on Perfil de VPN**.

3.  En la pestaña **programas** , en la parte inferior del panel de detalles, haga clic con el botón secundario en **script de Perfil de VPN**, haga clic en **propiedades**y realice los pasos siguientes:

    a.  En la pestaña **Opciones avanzadas** , en **cuando se asigne este programa a un equipo**, haga clic en **una vez por cada usuario que inicie sesión**.

    b.  Haga clic en **Aceptar**.

4.  Haga clic con el botón secundario en **script de Perfil de VPN** y haga clic en **implementar** para iniciar el Asistente para implementar software.

5.  En la página general, complete los pasos siguientes:

    a.  Junto a **colección**, haga clic en **examinar**.

    b.  En la lista **tipos de colección** (parte superior izquierda), haga clic en recopilaciones de **usuarios**.

    c.  Haga clic en **usuarios de VPN**y en **Aceptar**.

    d.  Haga clic en **Next**.

6.  En la página contenido, realice los pasos siguientes:

    a.  Haga clic en **Agregar**y en **punto de distribución**.

    b.  En **puntos de distribución disponibles**, seleccione los puntos de distribución a los que desea distribuir el script de configuración de ProfileXML y haga clic en **Aceptar**.

    c.  Haga clic en **Next**.

7.  En la página Configuración de implementación, haga clic en **siguiente**.

8.  En la página programación, realice los pasos siguientes:

    a.  Haga clic en **nuevo** para abrir el cuadro de diálogo programación de asignación.

    b.  Haga clic en **asignar inmediatamente después de este evento**y haga clic en **Aceptar**.

    c.  Haga clic en **Next**.

9.  En la página experiencia del usuario, complete los pasos siguientes:

    1.  Active la casilla **instalación de software** .

    2.  Haga clic en **Resumen**.

10. En la página Resumen, haga clic en **siguiente**.

11. En la página finalización, haga clic en **cerrar**.

Con el script de configuración de ProfileXML implementado, inicie sesión en un equipo cliente de Windows 10 con la cuenta de usuario que seleccionó al generar la recopilación de usuarios. Compruebe la configuración del cliente de VPN.

>[!NOTE]
>El script VPN_Profile. PS1 no funciona en una sesión Escritorio remoto. Del mismo modo, no funciona en una sesión mejorada de Hyper-V. Si va a probar un acceso remoto Always On VPN en máquinas virtuales, deshabilite la sesión mejorada en las máquinas virtuales cliente antes de continuar.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Comprobar la configuración del cliente de VPN

1.  En el panel de control, en **seguridad del sistema\\** , haga clic en **Configuration Manager**. 

2.  En el cuadro de diálogo Propiedades de Configuration Manager, en la pestaña **acciones** , realice los pasos siguientes:

    a.  Haga clic en **recuperación de directiva de equipo & ciclo de evaluación**, haga clic en **Ejecutar ahora**y haga clic en **Aceptar**.

    b.  Haga clic en **recuperación de directiva de usuario & ciclo de evaluación**, haga clic en **Ejecutar ahora**y haga clic en **Aceptar**.

    c.  Haga clic en **Aceptar**.

3.  Cierre el panel de control.

Debería ver el nuevo perfil de VPN en breve.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configuración del cliente VPN mediante Intune

Para usar Intune para implementar el acceso remoto de Windows 10 Always On perfiles de VPN, puede configurar el nodo ProfileXML CSP mediante el perfil de VPN que creó en la sección [crear los archivos de configuración de ProfileXML](#create-the-profilexml-configuration-files), o puede usar el ejemplo de XML de EAP de base proporcionado. menor.

>[!NOTE]
>Intune ahora usa grupos de Azure AD. Si Azure AD Connect sincronizar el grupo de usuarios de VPN de local a Azure AD y los usuarios se asignan al grupo de usuarios de VPN, está listo para continuar.

Cree la Directiva de configuración de dispositivos VPN para configurar los equipos cliente de Windows 10 para todos los usuarios agregados al grupo. Dado que la plantilla de Intune proporciona parámetros de VPN, \<copie solo \<la parte EapHostConfig >/EapHostConfig > del archivo VPN_ProfileXML.

### <a name="create-the-always-on-vpn-configuration-policy"></a>Crear la Directiva de configuración de VPN de Always On

1.  Inicia sesión en [Azure Portal](https://portal.azure.com/).

2.  Vaya a **Intune** > **configuración** > de dispositivos**perfiles**.

3.  Haga clic en **crear perfil** para iniciar el Asistente para crear perfiles.

4.  Escriba un **nombre** para el perfil de VPN y, opcionalmente, una descripción.

1.   En **plataforma**, seleccione **Windows 10 o posterior**y elija **VPN** en la lista desplegable tipo de perfil.

     >[!TIP]
     >Si va a crear un profileXML de VPN personalizado, consulte [aplicación de profileXML con Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) para obtener instrucciones.

2. En la pestaña **base VPN** , compruebe o establezca la configuración siguiente:

    - **Nombre de la conexión:** Escriba el nombre de la conexión VPN tal como aparece en el equipo cliente en la pestaña **VPN** , en **configuración**, por ejemplo, _contoso AutoVPN_.  
    
    - **Servidores** Agregue uno o más servidores VPN; para ello, haga clic en **Agregar.**
    
    - **Descripción** y **dirección IP o FQDN:** Escriba la descripción y la dirección IP o el FQDN del servidor VPN. Estos valores se deben alinear con el nombre del sujeto en el certificado de autenticación del servidor VPN. 
    
    - **Servidor predeterminado:** Si este es el servidor VPN predeterminado, establézcalo en **true**. Esto habilita este servidor como el servidor predeterminado que usan los dispositivos para establecer la conexión. 
    
    - **Tipo de conexión:** Establezca en **IKEv2**.  
    
    - **Always On:** Establezca en **habilitado** para conectarse a la VPN automáticamente en el inicio de sesión y manténgase conectado hasta que el usuario se desconecte manualmente.
    
    - **Recordar credenciales en cada inicio de sesión**:  Valor booleano (true o false) para almacenar en caché las credenciales. Si se establece en true, las credenciales se almacenan en caché siempre que sea posible.

3.  Copie la siguiente cadena XML en un editor de texto:
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  Reemplace el  **\<TrustedRootCA > 5A 89 fe CB 5B 49 A7 0B 1A 52 63 B7 35 EE D7 1C C2 68 a 4B <\/TrustedRootCA >** en el ejemplo con la huella digital del certificado de la entidad de certificación raíz local en ambos lugares.
  
    >[!Important]
    >No use la huella digital de ejemplo en \<la sección\<TrustedRootCA >/TrustedRootCA > a continuación.  TrustedRootCA debe ser la huella digital del certificado de la entidad de certificación raíz local que emitió el certificado de autenticación de servidor para los servidores RRAS y NPS. **No debe ser el certificado raíz de la nube ni la huella digital del certificado de CA emisora intermedia**.

5.  Reemplace los nombres de  **\<servidor > NPS. contoso.\<com/ServerNames >** en el XML de ejemplo con el FQDN del NPS unido al dominio donde se realiza la autenticación. 

6.  Copie la cadena XML revisada y péguela en el cuadro **EAP XML** en la pestaña base VPN y haga clic en **Aceptar**.
    Una Always On Directiva de configuración de dispositivos VPN que usa EAP se crea en Intune.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Sincronización de la Directiva de configuración de VPN de Always On con Intune

Para probar la Directiva de configuración, inicie sesión en un equipo cliente de Windows 10 como el usuario que agregó al grupo **Always on usuarios de VPN** y, a continuación, sincronice con Intune.

1.  En el menú Inicio, haga clic en **configuración**.

2.  En configuración, haga clic en **cuentas**y en **obtener acceso a trabajo o escuela**.

3.  Haga clic en el perfil MDM y haga clic en **información**.

4.  Haga  clic en sincronizar para forzar la evaluación y recuperación de directivas de Intune.

5.  Cierre la configuración. Después de la sincronización, verá que el perfil de VPN está disponible en el equipo.

## <a name="next-steps"></a>Pasos siguientes

Ha terminado de implementar Always On VPN.  Para ver otras características que puede configurar, consulte la tabla siguiente:

|Si desea...  |A continuación, vea...  |
|---------|---------|
|Configuración del acceso condicional para VPN    |[Paso 7. Opta Configure el acceso condicional para la conectividad](../../ad-ca-vpn-connectivity-windows10.md)VPN mediante Azure ad: En este paso, puede ajustar el modo en que los usuarios de VPN autorizados acceden a los recursos mediante el [acceso condicional de Azure Active Directory (Azure ad)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con Azure AD acceso condicional para la conectividad de la red privada virtual (VPN), puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD).         |
|Más información acerca de las características avanzadas de VPN  |[Características avanzadas de VPN](always-on-vpn-adv-options.md#advanced-vpn-features): En esta página se proporcionan instrucciones sobre cómo habilitar los filtros de tráfico de VPN, cómo configurar conexiones VPN automáticas mediante desencadenadores de aplicaciones y cómo configurar NPS para permitir solo las conexiones VPN de los clientes que usan certificados emitidos por Azure AD.        |
