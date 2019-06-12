---
title: Configurar las conexiones VPN de Always On del cliente de Windows 10
description: En este paso, obtenga información sobre las opciones de ProfileXML y el esquema y configurar los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: fdf640d7e98781ca054ab982027bdfe8c3fb64d6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749625"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Paso 6. Configurar las conexiones VPN de Always On de cliente de Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 5. Definir el DNS y la configuración del firewall](vpn-deploy-dns-firewall.md)<br>
- [**Siguiente:** Paso 7. (Opcional) Acceso condicional para la conectividad VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

En este paso, podrá obtener información sobre las opciones de ProfileXML y el esquema y configurar los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN.

Puede configurar al cliente de VPN de Always On a través de PowerShell, SCCM o Intune. Las tres requieren un perfil de VPN de XML para la configuración de VPN adecuado. Automatización de PowerShell la inscripción para organizaciones sin SCCM o Intune es posible.

>[!NOTE]
>Directiva de grupo no incluye plantillas administrativas para configurar al cliente de Windows 10 siempre en VPN de acceso remoto.  Sin embargo, puede usar scripts de inicio de sesión.

## <a name="profilexml-overview"></a>Información general de ProfileXML

ProfileXML es un nodo URI en el CSP de VPNv2. En lugar de configurar individualmente cada nodo de CSP de VPNv2, como los desencadenadores, enrutar listas y protocolos de autenticación, utilice este nodo para configurar un cliente de VPN de Windows 10 y que proporciona toda la configuración como un único bloque XML a un único nodo CSP. El esquema de ProfileXML coincide con el esquema de los nodos de CSP de VPNv2 prácticamente idéntica, pero algunos de los términos son ligeramente diferentes.

Use ProfileXML en todos los métodos de entrega que se describe en esta implementación, incluidos Windows PowerShell, System Center Configuration Manager y Intune. Hay dos formas de configurar el nodo de CSP de VPNv2 ProfileXML en esta implementación:

- **OMA-DM**. Una consiste en usar un proveedor MDM mediante OMA-DM, como se explicó anteriormente en la sección [nodos de CSP de VPNv2](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Con este método, puede insertar fácilmente el marcado XML de configuración de perfil VPN en el nodo de ProfileXML CSP al usar Intune.

- **Windows Management Instrumentation (WMI) - to - puente CSP**. El segundo método para configurar el nodo de ProfileXML CSP es usar el puente de WMI a CSP, llama una clase WMI **MDM_VPNv2_01**, que puede tener acceso a los CSP de VPNv2 y el nodo de ProfileXML. Cuando se crea una nueva instancia de la clase WMI, WMI utiliza el CSP para crear el perfil de VPN al usar Windows PowerShell y System Center Configuration Manager.

Aunque estos métodos de configuración difieren, ambos requieren un perfil de VPN de XML con el formato correcto. Para usar la configuración de ProfileXML VPNv2 CSP, generar XML mediante el esquema de ProfileXML para configurar las etiquetas necesarias para el escenario de implementación simple. Para obtener más información, consulte [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

A continuación busque cada una de las opciones necesarias y la correspondiente etiqueta de ProfileXML. Configurar cada opción en una etiqueta específica dentro del esquema de ProfileXML y no todos ellos se encuentran en el perfil nativo. Para la ubicación de la etiqueta adicional, consulte el esquema de ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo de conexión:** IKEv2 nativo

Elemento ProfileXML:

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**Enrutamiento:** Tunelización dividida

Elemento ProfileXML:

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**Resolución de nombres:** Sufijo de la lista de información de nombre de dominio y DNS

Elementos de ProfileXML:

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**Desencadenar:** Detección de red siempre encendido y confianza

Elementos de ProfileXML:

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**Autenticación:** PEAP-TLS con certificados de usuario protegidas

Elementos de ProfileXML:

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

Puede usar etiquetas simples para configurar algunos mecanismos de autenticación de VPN. Sin embargo, PEAP y EAP son más complejas. Es la manera más fácil de crear el marcado XML configurar a un cliente VPN con su configuración de EAP y, a continuación, exportar dicha configuración a XML.

Para obtener más información acerca de la configuración de EAP, consulte [configuración de EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration).

## <a name="manually-create-a-template-connection-profile"></a>Crear manualmente un perfil de conexión de la plantilla

En este paso, se utiliza el protocolo de autenticación Extensible protegido (PEAP) para proteger la comunicación entre el cliente y el servidor. A diferencia de un nombre de usuario simple y una contraseña, esta conexión requiere una única sección EAPConfiguration en el perfil de VPN para que funcione.

En lugar de describir cómo se crea el marcado XML desde el principio, usar configuración de Windows para crear una plantilla de perfil de VPN. Después de crear la plantilla de perfil de VPN, usa Windows PowerShell para consumir la parte EAPConfiguration desde esa plantilla para crear el ProfileXML final que se implemente más adelante en la implementación.

### <a name="record-nps-certificate-settings"></a>Configuración del certificado de registro NPS

Antes de crear la plantilla, tome nota del nombre de host o el nombre de dominio completo (FQDN) del servidor NPS desde el certificado del servidor y el nombre de la entidad de certificación que emitió el certificado.

**Procedimiento:**

1. En el servidor NPS, abra el servidor de directivas de red.

2. En la consola NPS, en directivas, haga clic en **las directivas de red**.

3. Haga clic en **las conexiones de red privada Virtual (VPN)** y haga clic en **propiedades**.

4. Haga clic en el **restricciones** ficha y haga clic en **métodos de autenticación**.

5. En los tipos de EAP, haga clic en **Microsoft: EAP protegido (PEAP)** y haga clic en **editar**.

6. Registre los valores de **certificado emitido para** y **emisor**.

    Use estos valores en la próxima configuración de plantilla VPN. Por ejemplo, si el FQDN del servidor es nps01.corp.contoso.com y el nombre de host es NPS01, el nombre del certificado se basa en el nombre DNS o FQDN del servidor, por ejemplo, nps01.corp.contoso.com.

7. Cancele el cuadro de diálogo Editar propiedades de EAP protegido.

8. Cancele el cuadro de diálogo Propiedades de conexiones de red privada Virtual (VPN).

9. Cierre el servidor de directivas de red.

>[!NOTE]
>Si tiene varios servidores NPS, complete estos pasos en cada uno de ellos para que el perfil de VPN cada uno de ellos puede comprobar se deben usar.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurar la plantilla de perfil de VPN en un equipo cliente unido al dominio

Ahora que tiene la información necesaria: configurar la plantilla de perfil de VPN en un equipo cliente unido al dominio. El tipo de usuario de la cuenta use (es decir, un usuario estándar o administrador) para esta parte del proceso no importa.

Sin embargo, si aún no ha reiniciado el equipo desde la configuración de inscripción automática de certificados, hacerlo antes de configurar la plantilla de la conexión VPN para asegurarse de que tener un certificado utilizable inscrito en él.

>[!NOTE]
>No hay ninguna manera de agregar manualmente las propiedades avanzadas de VPN, como las reglas NRPT, Always On, la detección de red, etcetera de confianza. En el paso siguiente, se crea una conexión VPN de prueba para comprobar la configuración del servidor VPN y que puede establecer una conexión VPN al servidor.

**Crear manualmente una conexión VPN de prueba única**

1.  Inicie sesión en un equipo cliente unido al dominio como un miembro de la **usuarios de VPN** grupo.

2.  En el menú Inicio, escriba **VPN**, y presione ENTRAR.

3.  En el panel de detalles, haga clic en **agregar una conexión VPN**.

4.  En la lista de proveedores de VPN, haga clic en **Windows (integrada)** .

5.  En el nombre de la conexión, escriba **plantilla**.

6.  En el nombre del servidor o dirección, escriba el **externo** FQDN del servidor VPN (por ejemplo, **vpn.contoso.com**).

7.  Haga clic en **Guardar**.

8.  En la configuración relacionada, haga clic en **cambiar las opciones de adaptador**.

9.  Haga clic en **plantilla**y haga clic en **propiedades**.

10. En el **seguridad** ficha **tipo de VPN**, haga clic en **IKEv2**.

11. En el cifrado de datos, haga clic en **nivel máximo de cifrado**.

12. Haga clic en **usar el protocolo de autenticación Extensible (EAP)** ; a continuación, en **usar el protocolo de autenticación Extensible (EAP)** , haga clic en **Microsoft: EAP protegido (PEAP) (cifrado habilitado)** .

13. Haga clic en **propiedades** para abrir el cuadro de diálogo Propiedades de EAP protegido y complete los pasos siguientes:

    a. En el **conectarse a estos servidores** , escriba el nombre del servidor NPS que recupera de la configuración de autenticación del servidor NPS anteriormente en esta sección (por ejemplo, NPS01).

    >[!NOTE]
    >El nombre de servidor que escriba debe coincidir con el nombre del certificado. Recuperar este nombre anteriormente en esta sección. Si no coincide con el nombre, la conexión se producirá un error, que indica que "se impidió la conexión debido a una directiva configurada en el servidor RAS/VPN."

    b.  En entidades de certificación raíz de confianza, seleccione la CA que emitió el certificado del servidor NPS (por ejemplo, contoso-CA) raíz.

    c.  En las notificaciones antes de conectarse, haga clic en **no pide al usuario que autorice nuevos servidores o entidades emisoras de certificados de confianza**.

    d.  En Seleccionar método de autenticación, haga clic en **tarjeta inteligente u otro certificado**y haga clic en **configurar**. Se abrirá la tarjeta inteligente u otro cuadro de diálogo Propiedades del certificado.

    e.  Haga clic en **usar un certificado en este equipo**.

    f.  En el cuadro conectar con estos servidores, escriba el nombre del servidor NPS que recupera de la configuración de autenticación del servidor NPS en los pasos anteriores.

    g.  En entidades de certificación raíz de confianza, seleccione la CA raíz que emitió el certificado del servidor NPS.

    h.  Seleccione el **no solicitará el usuario para autorizar nuevos servidores o entidades de certificación de confianza** casilla de verificación.

    i.  Haga clic en **Aceptar** para cerrar la tarjeta inteligente u otro cuadro de diálogo Propiedades del certificado.

    j.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de EAP protegido.

14. Haga clic en **Aceptar** para cerrar el cuadro de diálogo Propiedades de la plantilla.

15. Cierre la ventana Conexiones de red.

16. En configuración, pruebe la conexión VPN haciendo **plantilla**y haga clic en **Connect**.

>[!IMPORTANT]
>Asegúrese de que la plantilla de la conexión VPN en el servidor VPN es correcta. Este modo se garantiza que la configuración de EAP es correcta antes de usarlos en el ejemplo siguiente. Debe conectarse al menos una vez antes de continuar; en caso contrario, el perfil no contendrá toda la información necesaria para conectarse a la VPN.

## <a name="create-the-profilexml-configuration-files"></a>Crear los archivos de configuración de ProfileXML

Antes de completar esta sección, asegúrese de que ha creado y probado la conexión VPN de la plantilla que la sección [crear manualmente un perfil de conexión de la plantilla](#manually-create-a-template-connection-profile) describe. Probar la conexión VPN es necesario asegurarse de que el perfil contiene toda la información necesaria para conectarse a la VPN.

El script de Windows PowerShell en el listado 1 crea dos archivos en el escritorio, ambos de los cuales contienen **EAPConfiguration** etiquetas según el perfil de conexión de la plantilla que creó anteriormente:

- **VPN_Profile.xml.** Este archivo contiene el marcado XML necesario para configurar el nodo de ProfileXML en el CSP de VPNv2. Use este archivo con los servicios compatibles con OMA-DM MDM, como Intune.

- **VPN_Profile.ps1.** Este archivo es un script de Windows PowerShell que se puede ejecutar en los equipos cliente para configurar el nodo de ProfileXML en el CSP de VPNv2. También puede configurar el CSP mediante la implementación de esta secuencia de comandos a través de System Center Configuration Manager. No se puede ejecutar este script en una sesión de escritorio remoto, incluyendo una sesión de Hyper-V mejorado.

>[!IMPORTANT]
>Los siguientes comandos de ejemplo requieren la compilación de Windows 10 1607 o posterior.

**Crear VPN_Profile.xml y VPN_Proflie.ps1**

1. Inicie sesión en el equipo cliente unido a un dominio que contiene la plantilla de perfil de VPN con el mismo usuario que cuenta la sección [crear manualmente un perfil de conexión de la plantilla](#manually-create-a-template-connection-profile) descrito.

2. Pegue listado 1 en el entorno de scripting integrado (ISE) de Windows PowerShell y personalizar los parámetros descritos en los comentarios. Estos son $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork y $DNSServers. Es una descripción completa de cada opción de configuración en los comentarios.

3. Ejecute el script para generar **VPN_Profile.xml** y **VPN_Profile.ps1** en el escritorio.

#### <a name="listing-1-understanding-makeprofileps1"></a>Listado 1. Descripción MakeProfile.ps1

Esta sección explica el código de ejemplo que puede usar para obtener una descripción de cómo crear un perfil de VPN, específicamente para la configuración de ProfileXML en el CSP de VPNv2.

Después de ensamblar una secuencia de comandos de este código de ejemplo y ejecutar el script, el script genera dos archivos: VPN_Profile.XML y VPN_Profile.ps1. Use VPN_Profile.xml para configurar ProfileXML de OMA-DM compatible con los servicios MDM, como Microsoft Intune.

Use la **VPN_Profile.ps1** secuencia de comandos de Windows PowerShell o System Center Configuration Manager para configurar ProfileXML en el escritorio de Windows 10.

>[!NOTE]
>Para ver el script de ejemplo completo, vea la sección [Script completo MakeProfile.ps1](#makeprofileps1-full-script).

#### <a name="parameters"></a>Parámetros

Configure los parámetros siguientes:

**$Template**. El nombre de la plantilla desde el que se va a recuperar la configuración de EAP.

**$ProfileName**. Identificador alfanumérico único para el perfil. El nombre del perfil no debe incluir una barra diagonal (/). Si el nombre del perfil tiene un espacio u otros caracteres no alfanuméricos, debe convertirse correctamente según el estándar de codificación de dirección URL.

**$Servers**. Dirección IP pública o enrutable o nombre DNS para la puerta de enlace VPN. Dirección IP de una puerta de enlace o una dirección IP virtual para una granja de servidores puede señalar a la externa. Ejemplos, 208.147.66.130 o vpn.contoso.com.

**$DnsSuffix**. Especifica los sufijos DNS de separados por comas de uno o más. El primer elemento de la lista también se usa como el sufijo DNS específico de la conexión para la interfaz VPN. Toda la lista también se agregarán a la SuffixSearchList.

**$DomainName**. Se utiliza para indicar el espacio de nombres al que se aplica la directiva. Cuando se emite una consulta con nombre, el cliente DNS compara el nombre de la consulta a todos los espacios de nombres en DomainNameInformationList para encontrar a una coincidencia. Este parámetro puede ser uno de los siguientes tipos:

- FQDN - nombre de dominio completo
- Sufijo - un sufijo de dominio que se anexará a la consulta de nombre corto para la resolución DNS. Para especificar un sufijo, anteponer un punto (.) para el sufijo DNS.

**$DNSServers**. Lista de IP del servidor DNS separados por comas de direcciones que se usará para el espacio de nombres.

**$TrustedNetwork**. Cadena separada por comas para identificar la red de confianza. VPN no conectarse automáticamente cuando el usuario esté en su red inalámbrica corporativa donde los recursos protegidos son accesibles directamente al dispositivo.

Los siguientes son los valores de los parámetros utilizados en los siguientes comandos de ejemplo. Asegúrese de cambiar estos valores para su entorno.

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

Los siguientes comandos de ejemplo obtención configuración de EAP de perfil de la plantilla:

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

### <a name="output-vpnprofilexml-for-intune"></a>Salida VPN_Profile.xml para Intune

Puede usar el siguiente comando de ejemplo para guardar el archivo XML de perfil:

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>VPN_Profile.ps1 de salida para el escritorio y System Center Configuration Manager

Ejemplo de código siguiente configura una conexión VPN de IKEv2 de AlwaysOn mediante el nodo ProfileXML en el CSP de VPNv2.

Puede usar este script en el escritorio de Windows 10 o en System Center Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Definir parámetros de perfil VPN de clave

```xml
$Script = '$ProfileName = ''' + $ProfileName + '''
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

### <a name="define-wmi-to-csp-bridge-properties"></a>Definir las propiedades del puente de CSP de WMI

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

### <a name="define-wmi-session"></a>Definir la sesión de WMI:

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

### <a name="create-the-vpn-profile"></a>Crear el perfil de VPN:

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
Write-Host "$Message"'
```

### <a name="save-the-profile-xml-file"></a>Guarde el archivo XML de perfil

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>Script completo MakeProfile.ps1

La mayoría de ejemplos use el cmdlet Set-WmiInstance Windows PowerShell para insertar ProfileXML en una nueva instancia de la clase WMI de MDM_VPNv2_01. 

Sin embargo, esto no funciona en System Center Configuration Manager porque no se puede ejecutar el paquete en el contexto de los usuarios finales. Por lo tanto, este script usa el modelo de información común para crear una sesión WMI en el contexto del usuario y, a continuación, crea una nueva instancia de la clase WMI MDM_VPNv2_01 en esa sesión. Esta clase WMI usa el puente de WMI a CSP para configurar el CSP de VPNv2. Por lo tanto, mediante la adición de la instancia de clase, configure el CSP. 

>[!IMPORTANT]
>Puente de CSP de WMI requiere derechos de administrador local, por diseño. Para implementar por los perfiles de VPN de usuario debe mediante SCCM o la MDM.

>[!NOTE]
>El script VPN_Profile.ps1 usa a SID del usuario actual para identificar el contexto del usuario. Dado que ningún SID está disponible en una sesión de escritorio remoto, la secuencia de comandos no funciona en una sesión de escritorio remoto. Del mismo modo, no funciona en una sesión de Hyper-V mejorado. Si está probando un siempre en VPN de acceso remoto en máquinas virtuales, deshabilite la sesión mejorada en el cliente de las máquinas virtuales antes de ejecutar esta secuencia de comandos.

El siguiente script de ejemplo incluye todos los ejemplos de código de las secciones anteriores. Asegúrese de que cambia los valores de ejemplo para los valores adecuados para su entorno.
    
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

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configurar al cliente VPN mediante Windows PowerShell

Para configurar el CSP de VPNv2 en un equipo cliente de Windows 10, ejecute el script VPN_Profile.ps1 Windows PowerShell que creó en el [crear el perfil XML](#create-the-profile-xml) sección. Abra Windows PowerShell como administrador; en caso contrario, recibirá un error que dice, _acceso denegado_.

Después de ejecutar VPN_Profile.ps1 para configurar el perfil de VPN, puede comprobar en cualquier momento que se realizó correctamente ejecutando el siguiente comando en Windows PowerShell ISE:

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**Resultados correctos desde el cmdlet Get-WmiObject**

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

La configuración de ProfileXML debe ser correcta en la estructura, ortografía, configuración y, a veces, mayúsculas y minúsculas. Si ve algo diferente en la estructura a la lista 1, el marcado de ProfileXML probable que contiene un error.

Si necesita solucionar el marcado, es más fácil ponerlo en un editor XML que al solucionar problemas de Windows PowerShell ISE. En cualquier caso, comience con la versión más sencilla del perfil y agregar componentes de vuelta una vez hasta que el problema se produce de nuevo.

## <a name="configure-the-vpn-client-by-using-system-center-configuration-manager"></a>Configurar al cliente VPN mediante el uso de System Center Configuration Manager

En System Center Configuration Manager, puede implementar perfiles de VPN mediante el nodo de ProfileXML CSP, al igual que en Windows PowerShell. Aquí, se usa la secuencia de comandos VPN_Profile.ps1 Windows PowerShell que creó en la sección [crear los archivos de configuración de ProfileXML](#create-the-profilexml-configuration-files).

Para usar System Center Configuration Manager para implementar un perfil siempre en VPN de acceso remoto a los equipos cliente de Windows 10, debe comenzar mediante la creación de un grupo de equipos o usuarios a los que se implemente el perfil. En este escenario, creará un grupo de usuarios para implementar el script de configuración.

### <a name="create-a-user-group"></a>Crear un grupo de usuarios

1.  En la consola de Configuration Manager, abra activos y compatibilidad\\recopilaciones de usuarios.

2.  En el **inicio** la cinta de opciones, en el **crear** grupo, haga clic en **crear recopilación de usuarios**.

3.  En la página General, complete los pasos siguientes:

    a. En **nombre**, tipo **usuarios de VPN**.

    b. Haga clic en **examinar**, haga clic en **todos los usuarios** y haga clic en **Aceptar**.

    c. Haz clic en **Siguiente**.

4.  En la página reglas de pertenencia, complete los pasos siguientes:

    a.  En **reglas de pertenencia**, haga clic en **Agregar regla**y haga clic en **regla directa**. En este ejemplo, agrega usuarios individuales a la colección. Sin embargo, podría usar una regla de consulta para agregar usuarios a esta colección dinámicamente para una implementación de gran escala.

    b.  En el **bienvenida** página, haga clic en **siguiente**.

    c.  En la página de recursos, busque en **valor**, escriba el nombre del usuario que desea agregar. El nombre de recurso incluye el dominio del usuario. Para incluir resultados en función de una coincidencia parcial, inserte el **%** caracteres en cualquier extremo de su criterio de búsqueda. Por ejemplo, para buscar todos los usuarios que contiene la cadena "lori", escriba **% lori %** . Haz clic en **Siguiente**.

    d.  En la página Seleccionar recursos, seleccione los usuarios que desea agregar al grupo y haga clic en **siguiente**.

    e.  En la página Resumen, haga clic en **siguiente**.

    f.  En la página Finalización, haga clic en **cerrar**.

6.  En la página de reglas de pertenencia del Asistente para crear recopilación de usuario, haga clic en **siguiente**.

7.  En la página Resumen, haga clic en **siguiente**.

8.  En la página Finalización, haga clic en **cerrar**.

Después de crear el grupo de usuarios para recibir el perfil de VPN, puede crear un paquete y programa para implementar el script de configuración de Windows PowerShell que creó en la sección [crear los archivos de configuración de ProfileXML](#create-the-profilexml-configuration-files).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Crear un paquete que contiene el script de configuración de ProfileXML

1.  Hospedar el script VPN_Profile.ps1 en un recurso compartido de red que puede tener acceso la cuenta de equipo del servidor de sitio.

2.  En la consola de Configuration Manager, abra **biblioteca de Software\\Application Management\\paquetes**.

3.  En el **inicio** la cinta de opciones, en el **crear** grupo, haga clic en **crear paquete** para iniciar el Asistente de programa y crear un paquete.

4.  En la página del paquete, complete los pasos siguientes:

    a. En **nombre**, tipo **Windows 10 siempre en perfil de VPN**.

    b. Seleccione el **este paquete contiene archivos de código fuente** casilla de verificación y haga clic en **examinar**.

    c. En el cuadro de diálogo Establecer carpeta de origen, haga clic en **examinar**, seleccione el recurso compartido de archivos que contiene VPN_Profile.ps1 y haga clic en **Aceptar**.
        Asegúrese de que seleccionar una ruta de acceso de red, no una ruta de acceso local. En otras palabras, la ruta de acceso debe parecerse al siguiente  *\\fileserver\\vpnscript*, no *c:\\vpnscript*.

1.  Haz clic en **Siguiente**.

2.  En la página tipo de programa, haga clic en **siguiente**.

3.  En la página programa estándar, complete los pasos siguientes:

    a.  En **nombre**, tipo **Script de perfil de VPN**.

    b.  En **línea de comandos**, tipo **PowerShell.exe - ExecutionPolicy Bypass - archivo "VPN_Profile.ps1"** .

    c.  En **modo de ejecución**, haga clic en **ejecutar con derechos administrativos**.

    d.  Haz clic en **Siguiente**.

4.  En la página de requisitos, complete los pasos siguientes:

    a.  Seleccione **este programa puede ejecutarse solo en las plataformas especificadas**.

    b.  Seleccione el **todo Windows 10 (32-bit)** y **todo Windows 10 (64 bits)** casillas de verificación.

    c.  En **espacio en disco estimado**, tipo **1**.

    d.  En **máximo permitido de tiempo de ejecución (minutos)** , tipo **15**.

    e.  Haz clic en **Siguiente**.

5.  En la página Resumen, haga clic en **siguiente**.

6.  En la página Finalización, haga clic en **cerrar**.

En el paquete y programa creado, deberá implementarla en el **usuarios de VPN** grupo.

### <a name="deploy-the-profilexml-configuration-script"></a>Implementar el script de configuración de ProfileXML

1.  En la consola de Configuration Manager, abra la biblioteca de Software\\Application Management\\paquetes.

2.  En **paquetes**, haga clic en **Windows 10 siempre en perfil de VPN**.

3.  En el **programas** ficha, en la parte inferior del panel de detalles, haga clic en **Script de perfil de VPN**, haga clic en **propiedades**y complete los pasos siguientes:

    a.  En el **avanzadas** ficha **cuando este programa se asigna a un equipo**, haga clic en **una vez para cada usuario que inicia sesión**.

    b.  Haga clic en **Aceptar**.

4.  Haga clic en **Script de perfil de VPN** y haga clic en **implementar** para iniciar el Asistente para implementar Software.

5.  En la página General, complete los pasos siguientes:

    a.  Junto a **colección**, haga clic en **examinar**.

    b.  En el **tipos de colección** lista (parte superior izquierda), haga clic en **recopilaciones de usuarios**.

    c.  Haga clic en **usuarios de VPN**y haga clic en **Aceptar**.

    d.  Haz clic en **Siguiente**.

6.  En la página de contenido, realice los pasos siguientes:

    a.  Haga clic en **agregar**y haga clic en **punto de distribución**.

    b.  En **puntos de distribución disponibles**, seleccione los puntos de distribución a la que desea distribuir el script de configuración de ProfileXML y haga clic en **Aceptar**.

    c.  Haz clic en **Siguiente**.

7.  En la página de configuración de implementación, haga clic en **siguiente**.

8.  En la página programación, complete los pasos siguientes:

    a.  Haga clic en **New** para abrir el cuadro de diálogo de programación de asignación.

    b.  Haga clic en **asignar inmediatamente después de este evento**y haga clic en **Aceptar**.

    c.  Haz clic en **Siguiente**.

9.  En la página de la experiencia del usuario, complete los pasos siguientes:

    1.  Seleccione el **instalación de Software** casilla de verificación.

    2.  Haga clic en **resumen**.

10. En la página Resumen, haga clic en **siguiente**.

11. En la página Finalización, haga clic en **cerrar**.

El script de configuración de ProfileXML implementado, inicie sesión en un equipo cliente de Windows 10 con la cuenta de usuario que se seleccionó cuando generó la colección de usuarios. Compruebe la configuración del cliente VPN.

>[!NOTE]
>La secuencia de comandos VPN_Profile.ps1 no funciona en una sesión de escritorio remoto. Del mismo modo, no funciona en una sesión de Hyper-V mejorado. Si está probando un siempre en VPN de acceso remoto en máquinas virtuales, deshabilite la sesión mejorada en el cliente de las máquinas virtuales antes de continuar.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Compruebe la configuración del cliente VPN

1.  En el Panel de Control, en **sistema\\seguridad**, haga clic en **Configuration Manager**. 

2.  En el cuadro de diálogo Propiedades de Configuration Manager, en el **acciones** ficha, realice los pasos siguientes:

    a.  Haga clic en **ciclo de evaluación y recuperación de directiva de equipo**, haga clic en **ejecutar ahora**y haga clic en **Aceptar**.

    b.  Haga clic en **ciclo de evaluación y recuperación de directivas de usuario**, haga clic en **ejecutar ahora**y haga clic en **Aceptar**.

    c.  Haga clic en **Aceptar**.

3.  Cierre el Panel de Control.

Debería ver el nuevo perfil VPN en breve.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configurar al cliente VPN mediante Intune

Para usar Intune para implementar perfiles de Windows 10 siempre en VPN de acceso remoto, puede configurar el nodo de ProfileXML CSP mediante el perfil VPN que creó en la sección [crear los archivos de configuración de ProfileXML](#create-the-profilexml-configuration-files), o bien puede usar el EAP de base A continuación proporcionado un ejemplo XML.

>[!NOTE]
>Ahora Intune usa grupos de Azure AD. Si Azure AD Connect sincroniza el grupo de usuarios de VPN desde local a Azure AD y los usuarios están asignados al grupo usuarios de VPN, está listo para continuar.

Crear la directiva de configuración del dispositivo VPN para configurar los equipos cliente de Windows 10 para todos los usuarios agregados al grupo. Puesto que la plantilla de Intune proporciona los parámetros VPN, solo copiar la \<EapHostConfig > \</EapHostConfig > parte del archivo VPN_ProfileXML.

### <a name="create-the-always-on-vpn-configuration-policy"></a>Crear la directiva de configuración de VPN de Always On

1.  Inicie sesión en el [portal Azure](https://portal.azure.com/).

2.  Vaya a **Intune** > **configuración del dispositivo** > **perfiles**.

3.  Haga clic en **Crear perfil** para iniciar el Asistente para crear perfil.

4.  Escriba un **nombre** para el perfil de VPN y (opcionalmente) una descripción.

1.   En **plataforma**, seleccione **Windows 10 o posterior**y elija **VPN** en la lista desplegable de tipo de perfil.

     >[!TIP]
     >Si está creando un profileXML personalizado de VPN, consulte [ProfileXML se aplican mediante Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) para obtener instrucciones.

2. En el **VPN Base** ficha, compruebe o configure las opciones siguientes:

    - **Nombre de conexión:** Escriba el nombre de la conexión VPN, tal y como aparece en el equipo cliente en el **VPN** pestaña **configuración**, por ejemplo, _Contoso AutoVPN_.  
    
    - **servidores:** Agregue uno o más servidores VPN haciendo **agregar.**
    
    - **Descripción** y **dirección IP o FQDN:** Escriba la descripción y la dirección IP o FQDN del servidor VPN. Estos valores deben alinearse con el nombre de sujeto de certificado de autenticación del servidor VPN. 
    
    - **Servidor predeterminado:** Si se trata el servidor VPN de forma predeterminada, se establece en **True**. Esto permite habilitar a este servidor como servidor predeterminado que usarán los dispositivos para establecer la conexión. 
    
    - **Tipo de conexión:** Establecido en **IKEv2**.  
    
    - **AlwaysOn:** Establecido en **habilitar** para conectarse a la VPN automáticamente en el inicio de sesión y permanecer conectado hasta que el usuario se desconecta de forma manual.
    
    - **Recordar credenciales en cada inicio de sesión**:  Valor booleano (true o false) para almacenar en caché las credenciales. Si el conjunto de credenciales es true, se almacenan en caché siempre que sea posible.

3.  Copie la siguiente cadena XML en un editor de texto:
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  Reemplace el  **\<TrustedRootCA > 5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1C c2 68 ser 4b <\/TrustedRootCA >** en el ejemplo con la huella digital de su entidad de certificación raíz en el entorno local en ambos lugares.
  
    >[!Important]
    >No utilice la huella digital de ejemplo en el \<TrustedRootCA >\</TrustedRootCA > sección más adelante.  El TrustedRootCA debe ser la huella digital del certificado de la autoridad de certificados de raíz en el entorno local que emitió el certificado de autenticación de servidor para servidores NPS y RRAS. **No debe ser el certificado raíz en la nube, ni la huella digital intermedia certificado de CA emisora**.

5.  Reemplace el  **\<ServerNames > NPS.contoso.com\</ServerNames >** en el ejemplo de XML con el FQDN de NPS unido al dominio donde realiza la autenticación. 

6.  Copie la cadena XML revisada y pegue en el **Xml de EAP** bajo la ficha VPN Base y haga clic en **Aceptar**.
    Se crea una directiva siempre en configuración del dispositivo VPN, el uso de EAP en Intune.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Sincronización de la directiva de configuración de VPN de Always On con Intune

Para probar la directiva de configuración, inicie sesión en un equipo cliente de Windows 10 como el usuario que ha agregado a la **siempre en los usuarios de VPN** grupo y, a continuación, en sincronización con Intune.

1.  En el menú Inicio, haga clic en **configuración**.

2.  En configuración, haga clic en **cuentas**y haga clic en **acceso profesional o educativo**.

3.  Haga clic en el perfil MDM y haga clic en **información**.

4.  Haga clic en **sincronización** para forzar una evaluación de directivas de Intune y la recuperación.

5.  Configuración de cierre. Después de la sincronización, verá el perfil de VPN disponible en el equipo.

## <a name="next-steps"></a>Pasos siguientes

Implementación siempre VPN está listo.  Para otras características que puede configurar, consulte la tabla siguiente:

|Si desea...  |A continuación, vea...  |
|---------|---------|
|Configurar el acceso condicional para VPN    |[Paso 7. (Opcional) Configurar el acceso condicional para la conectividad VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md): En este paso, puede ajustar cómo autorizado acceso a los usuarios VPN los recursos mediante [acceso condicional de Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con acceso condicional de Azure AD para la conectividad de red privada virtual (VPN), puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD).         |
|Más información sobre las características avanzadas de VPN  |[Características VPN avanzadas](always-on-vpn-adv-options.md#advanced-vpn-features): Esta página proporciona instrucciones sobre cómo habilitar los filtros de tráfico VPN, cómo configurar conexiones VPN automáticas mediante desencadenadores de aplicación y cómo configurar NPS para permitir solo conexiones VPN de los clientes que usan los certificados emitidos por Azure AD.        |
