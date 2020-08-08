---
title: Configurar el túnel de dispositivo VPN en Windows 10
description: Obtenga información sobre cómo crear un túnel de dispositivo VPN en Windows 10.
ms.date: 11/05/2018
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.openlocfilehash: 04500e2a9d5623aa9ce9796088bda2e4a6a5eccd
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996786"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configuración de túneles de dispositivo VPN en Windows 10

>Se aplica a: Windows 10 versión 1709

Always On VPN le ofrece la posibilidad de crear un perfil de VPN dedicado para el dispositivo o la máquina. Las conexiones VPN de Always On incluyen dos tipos de túneles:

- El _túnel de dispositivo_ se conecta a los servidores VPN especificados antes de que los usuarios inicien sesión en el dispositivo. Los túneles de dispositivo se utilizan en escenarios de conectividad previos al inicio de sesión y para administrar dispositivos.

- El _túnel de usuario_ solo se conecta después de que un usuario inicie sesión en el dispositivo. Los túneles de usuario permiten a los usuarios acceder a los recursos de la organización utilizando servidores VPN.

A diferencia del _túnel de usuario_, que solo se conecta después de que un usuario inicia sesión en el dispositivo o el equipo, el túnel de _dispositivo_ permite que la VPN establezca la conectividad antes de que el usuario inicie sesión. Tanto el túnel de _dispositivo_ como el _túnel de usuario_ funcionan de forma independiente con sus perfiles de VPN, se pueden conectar al mismo tiempo y pueden usar diferentes métodos de autenticación y otras opciones de configuración de VPN según corresponda. El túnel de usuario es compatible con SSTP y IKEv2, y el túnel de dispositivo solo admite IKEv2 sin compatibilidad con la reserva de SSTP.

El túnel de usuario se admite en dispositivos Unidos a un dominio, no Unidos a un dominio (grupo de trabajo) o Unidos a Azure AD para permitir escenarios tanto empresariales como BYOD. Está disponible en todas las ediciones de Windows y las características de la plataforma están disponibles para terceros mediante la compatibilidad con el complemento de VPN de UWP.

El túnel de dispositivo solo se puede configurar en dispositivos Unidos a un dominio que ejecuten Windows 10 Enterprise o Education versión 1709 o posterior. No se admite el control de terceros del túnel de dispositivo. El túnel de dispositivo no admite el uso de la tabla de directivas de resolución de nombres (NRPT). El túnel de dispositivo no admite el túnel forzado. Debe configurarlo como túnel dividido.


## <a name="device-tunnel-requirements-and-features"></a>Características y requisitos del túnel de dispositivo
Debe habilitar la autenticación de certificados de equipo para las conexiones VPN y definir una entidad de certificación raíz para autenticar las conexiones VPN entrantes.

```PowerShell
$VPNRootCertAuthority = "Common Name of trusted root certification authority"
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like "*$VPNRootCertAuthority*" })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Requisitos y características del túnel de dispositivo](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configuración de túnel de dispositivo VPN

El XML de Perfil de ejemplo siguiente proporciona una buena guía para escenarios en los que solo se requieren las extracciones iniciadas por el cliente a través del túnel de dispositivo.  Los filtros de tráfico se aprovechan para restringir el túnel de dispositivo solo al tráfico de administración.  Esta configuración funciona bien para los escenarios de Windows Update, directiva de grupo típicos (GP) y Microsoft Endpoint Configuration Manager Update, así como la conectividad VPN para el primer inicio de sesión sin credenciales almacenadas en caché o escenarios de restablecimiento de contraseña.

En el caso de los casos de inserciones iniciados por el servidor, como Administración remota de Windows (WinRM), los escenarios de la actualización remota de los Configuration Manager y los de actualización, debe permitir el tráfico entrante en el túnel del dispositivo, por lo que no se pueden usar filtros de tráfico.  Si en el perfil de túnel de dispositivo activa los filtros de tráfico, el túnel de dispositivo deniega el tráfico entrante.  Esta limitación se va a quitar en futuras versiones.


### <a name="sample-vpn-profilexml"></a>ProfileXML de VPN de ejemplo

A continuación se muestra el profileXML de VPN de ejemplo.

``` xml
<VPNProfile>
  <NativeProfile>
<Servers>vpn.contoso.com</Servers>
<NativeProtocolType>IKEv2</NativeProtocolType>
<Authentication>
  <MachineMethod>Certificate</MachineMethod>
</Authentication>
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
 <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
<DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>
  </NativeProfile>
  <!-- use host routes(/32) to prevent routing conflicts -->
  <Route>
<Address>10.10.0.2</Address>
<PrefixSize>32</PrefixSize>
  </Route>
  <Route>
<Address>10.10.0.3</Address>
<PrefixSize>32</PrefixSize>
  </Route>
<!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel -->
  <TrafficFilter>
<RemoteAddressRanges>10.10.0.2, 10.10.0.3</RemoteAddressRanges>
  </TrafficFilter>
<!-- need to specify always on = true -->
  <AlwaysOn>true</AlwaysOn>
<!-- new node to specify that this is a device tunnel -->
 <DeviceTunnel>true</DeviceTunnel>
<!--new node to register client IP address in DNS to enable manage out -->
<RegisterDNS>true</RegisterDNS>
</VPNProfile>
```

En función de las necesidades de cada escenario de implementación concreto, otra característica de VPN que se puede configurar con el túnel de dispositivo es la [detección de redes de confianza](/answers/topics/windows-server-infrastructure.html).

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>Implementación y prueba

Puede configurar túneles de dispositivo mediante el uso de un script de Windows PowerShell y el puente de Instrumental de administración de Windows (WMI). El túnel de dispositivo VPN Always On debe configurarse en el contexto de la cuenta de **sistema local** . Para ello, será necesario usar [PsExec](/sysinternals/downloads/psexec), una de las [PsTools](/sysinternals/downloads/pstools) incluidas en el conjunto de aplicaciones de [Sysinternals](/sysinternals/) .

Para obtener instrucciones sobre cómo implementar un perfil por dispositivo `(.\Device)` y un perfil por usuario `(.\User)` , consulte [usar scripting de PowerShell con el proveedor de puente WMI](/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider).

Ejecute el siguiente comando de Windows PowerShell para comprobar que ha implementado correctamente un perfil de dispositivo:

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

La salida muestra una lista de los perfiles de VPN de todo el dispositivo \- que se implementan en el dispositivo.

### <a name="example-windows-powershell-script"></a>Script de ejemplo de Windows PowerShell

Puede usar el siguiente script de Windows PowerShell para ayudar a crear su propio script para la creación de perfiles.

```PowerShell
Param(
[string]$xmlFilePath,
[string]$ProfileName
)

$a = Test-Path $xmlFilePath
echo $a

$ProfileXML = Get-Content $xmlFilePath

echo $XML

$ProfileNameEscaped = $ProfileName -replace ' ', '%20'

$Version = 201606090004

$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'

$nodeCSPURI = './Vendor/MSFT/VPNv2'
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"

$session = New-CimSession

try
{
$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
$newInstance.CimInstanceProperties.Add($property)

$session.CreateInstance($namespaceName, $newInstance)
$Message = "Created $ProfileName profile."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}
$Message = "Complete."
Write-Host "$Message"
```

## <a name="additional-resources"></a>Recursos adicionales

Los siguientes son recursos adicionales que le ayudarán en la implementación de la VPN.

### <a name="vpn-client-configuration-resources"></a>Recursos de configuración de cliente VPN

Los siguientes son recursos de configuración de cliente de VPN.

- [Cómo crear perfiles de VPN en Configuration Manager](/configmgr/protect/deploy-use/create-vpn-profiles)
- [Configuración de conexiones VPN de Always On cliente de Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opciones de perfil de VPN](/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>Recursos de puerta de enlace de servidor de acceso remoto

A continuación se muestran los recursos de puerta de enlace del servidor de acceso remoto (RAS).

- [Configurar RRAS con un certificado de autenticación de equipo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd458982(v=ws.11))
- [Solución de problemas de conexiones VPN de IKEv2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941612(v=ws.10))
- [Configuración del acceso remoto basado en IKEv2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff687731(v=ws.10))

>[!IMPORTANT]
>Al usar el túnel de dispositivo con una puerta de enlace RAS de Microsoft, tendrá que configurar el servidor RRAS para que admita la autenticación de certificado de equipo IKEv2 habilitando el método de autenticación **permitir la autenticación de certificados de equipo para IKEv2** como se describe [aquí](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ee922682%28v=ws.10%29). Una vez habilitada esta configuración, se recomienda encarecidamente que se use el cmdlet de PowerShell **set-VpnAuthProtocol** , junto con el parámetro opcional **RootCertificateNameToAccept** , para asegurarse de que las conexiones IKEv2 de RRAS solo se permiten para los certificados de cliente VPN que se encadenan a una entidad de certificación raíz interna o privada definida explícitamente. Como alternativa, el almacén de **entidades de certificación raíz de confianza** en el servidor RRAS debe modificarse para asegurarse de que no contiene entidades de certificación públicas, como se describe [aquí](/archive/blogs/rrasblog/what-type-of-certificate-to-install-on-the-vpn-server). También es posible que sea necesario tener en cuenta métodos similares para otras puertas de enlace de VPN.