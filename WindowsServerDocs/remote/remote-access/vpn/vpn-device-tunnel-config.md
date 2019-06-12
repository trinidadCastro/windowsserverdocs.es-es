---
title: Configurar el túnel de dispositivo VPN en Windows 10
description: Obtenga información sobre cómo crear un túnel VPN de dispositivos en Windows 10.
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 989216f90e78689b464240cff957bab1d9c1229b
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749567"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configurar dispositivo túneles VPN en Windows 10

>Se aplica a: Windows 10 versión 1709

VPN de Always On le ofrece la capacidad para crear un perfil de VPN dedicado para el dispositivo o equipo. Las conexiones VPN de Always On incluyen dos tipos de túneles: 

- _Túnel de dispositivo_ se conecta a servidores VPN especificados antes de los usuarios inicien sesión en el dispositivo. Escenarios de conectividad de inicio de sesión previo y fines de administración de dispositivos usan túnel del dispositivo.

- _Túnel de usuario_ se conecta sólo después de un usuario inicia sesión en el dispositivo. Túnel de usuario permite a los usuarios tener acceso a recursos de la organización a través de los servidores VPN.

A diferencia de _túnel usuario_, que se conecta sólo después de un usuario inicia sesión en el dispositivo o equipo, _túnel dispositivo_ permite la VPN establecer la conectividad antes de que el usuario inicia sesión. Ambos _túnel dispositivo_ y _túnel usuario_ funcionan de forma independiente con sus perfiles VPN, se puede conectar al mismo tiempo y puede usar diferentes métodos de autenticación y otras opciones de configuración de VPN según corresponda. Túnel de usuario es compatible con SSTP e IKEv2 y túnel del dispositivo es compatible con IKEv2 sólo con no se admite para la reserva SSTP.

Túnel de usuario es compatible con unido al dominio, unido a permaneciendo (grupo de trabajo) o dispositivos Unidos a AD – Azure para permitir enterprise y escenarios de BYOD. Está disponible en todas las ediciones de Windows y las características de plataforma están disponibles a terceros por medio de la compatibilidad con los complemento UWP VPN.

Túnel de dispositivo solo puede configurarse en dispositivos Unidos a un dominio que ejecutan Windows 10 Enterprise o educación, versión 1709 o versiones posteriores. No hay ninguna compatibilidad para control de terceros del túnel de dispositivo.


## <a name="device-tunnel-requirements-and-features"></a>Características y requisitos de túnel de dispositivo
Debe habilitar la autenticación de certificado de equipo para conexiones VPN y definir una entidad de certificación raíz para autenticar las conexiones VPN entrantes. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Requisitos y características de túnel de dispositivo](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configuración de túnel del dispositivo VPN

El perfil de ejemplo XML siguiente proporciona una buena orientación para escenarios donde extracciones que inició el cliente solo son necesarias a través del túnel del dispositivo.  Se usan filtros de tráfico para restringir el túnel de dispositivo a solo el tráfico de administración.  Esta configuración funciona bien para Windows Update, directiva de grupo típico (GP) y escenarios de actualización de System Center Configuration Manager (SCCM), así como la conectividad VPN para el primer inicio de sesión sin credenciales almacenadas en caché o escenarios de restablecimiento de contraseña. 

Inserción iniciadas por el servidor en los casos, como administración remota de Windows (WinRM), GPUpdate remoto y escenarios de actualización remotos SCCM: debe permitir el tráfico entrante en el túnel de dispositivo, por lo que no se puede usar filtros de tráfico.  Si el perfil de dispositivo túnel activar los filtros de tráfico, el túnel de dispositivo deniega el tráfico entrante.  Esta limitación se va a quitar en futuras versiones.


### <a name="sample-vpn-profilexml"></a>Ejemplo VPN profileXML

La siguiente es la profileXML VPN de ejemplo.

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

Según las necesidades de cada escenario de implementación concreta, es otra característica VPN que se puede configurar con el túnel de dispositivo [detección de red de confianza](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>Implementación y pruebas

Puede configurar los túneles de dispositivo mediante un script de Windows PowerShell y con el puente de Windows Management Instrumentation (WMI). El túnel de dispositivo VPN de Always On debe configurarse en el contexto de la **sistema LOCAL** cuenta. Para lograr esto, será necesario usar [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), uno de los [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) incluido en el [Sysinternals](https://docs.microsoft.com/sysinternals/) conjunto de utilidades.

Para obtener instrucciones sobre cómo implementar una por dispositivo `(.\Device)` frente a una por usuario `(.\User)` de perfil, vea [scripting con PowerShell con el proveedor de WMI puente](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider).

Ejecute el siguiente comando de Windows PowerShell para comprobar que ha implementado correctamente un perfil de dispositivo:

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

La salida muestra una lista de los dispositivos\-amplios perfiles de VPN que se implementan en el dispositivo.

### <a name="example-windows-powershell-script"></a>Script de PowerShell de Windows de ejemplo

Puede usar el siguiente script de Windows PowerShell para ayudar a crear su propio script de creación de perfiles.

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

Los siguientes son recursos adicionales que le ayudarán con la implementación de VPN.

### <a name="vpn-client-configuration-resources"></a>Recursos de configuración de cliente VPN

Los siguientes son recursos de configuración de cliente VPN.

- [Cómo crear perfiles de VPN en System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configuración de cliente Windows 10 siempre en las conexiones VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opciones de perfil de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>Recursos de puerta de enlace de servidor de acceso remotos

Estos son los recursos de puerta de enlace de servidor de acceso remoto (RAS).

- [Configuración de RRAS con un certificado de autenticación de equipo](https://technet.microsoft.com/library/dd458982.aspx)
- [Solución de problemas de conexiones VPN de IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurar el acceso remoto basada en IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Al usar el túnel de dispositivo con una puerta de enlace de RAS de Microsoft, deberá configurar el servidor RRAS para admitir la autenticación de certificados de equipo IKEv2 habilitando la **permitir la autenticación de certificado de equipo para IKEv2** método de autenticación como se describe [aquí](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Una vez habilitada esta opción, se recomienda encarecidamente que la **conjunto VpnAuthProtocol** cmdlet de PowerShell, junto con el **RootCertificateNameToAccept** parámetro opcional, se usa para asegurarse de que Solo se permiten las conexiones IKEv2 RRAS para los certificados de cliente VPN que se relacionan con un definido explícitamente interna o privada entidad de certificación raíz. Como alternativa, el **entidades emisoras raíz de confianza** almacén en el servidor RRAS debe modificarse para asegurarse de que no contiene las entidades de certificación pública como se explicó [aquí](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Métodos similares también tendrá que tener en cuenta otras puertas de enlace VPN.

