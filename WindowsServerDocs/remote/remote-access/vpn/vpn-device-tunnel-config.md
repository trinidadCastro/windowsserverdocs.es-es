---
title: Configurar el túnel de dispositivo VPN en Windows 10
description: Obtén información sobre cómo crear un túnel de dispositivo VPN en Windows 10.
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 005721873ad3a0df942bc9e23eba13728965ccba
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067480"
---
# Configurar túneles de dispositivo VPN en Windows 10

>Se aplica a: Windows 10 versión 1709

VPN siempre activada te ofrece la capacidad para crear un perfil de VPN dedicado para el dispositivo o máquina. Las conexiones de VPN siempre activada incluyen dos tipos de túneles: 

- _Túnel de dispositivo_ se conecta a los servidores VPN especificados antes de usuarios inicien sesión en el dispositivo. Escenarios de conectividad de inicio de sesión previo y con fines de administración de dispositivos usan túnel de dispositivo.

- _Túnel de usuario_ se conecta solo después de una sesión de usuario en el dispositivo. Túnel de usuario permite a los usuarios acceso a los recursos de la organización a través de servidores VPN.

A diferencia de _túnel de usuario_, que solo se conecta después de una sesión de usuario en el equipo o dispositivo, _túnel de dispositivo_ permite la conexión VPN establecer la conexión antes de que el usuario inicie sesión. _Túnel de dispositivo_ y _túnel de usuario_ funcionan de forma independiente con sus perfiles VPN, se pueden conectar al mismo tiempo y usan distintos métodos de autenticación y otras opciones de configuración de VPN según corresponda. Túnel de usuario admite SSTP y IKEv2 y túnel de dispositivo admite IKEv2 solo con ninguna compatibilidad con la reserva SSTP.

Túnel de usuario se admite en-unido al dominio unido pertenece (grupo de trabajo), o a Azure AD: para permitir la empresa y los escenarios tipo BYOD. Está disponible en todas las ediciones de Windows y las características de plataforma están disponibles a terceros por medio de soporte técnico de complemento de VPN de UWP.

Túnel de dispositivo solo se puede configurar en los dispositivos Unidos a un dominio que ejecute Windows 10 Enterprise o Education versión 1709 o posterior. No hay ninguna compatibilidad para control de aplicaciones de terceros de túnel de dispositivo.


## Características y los requisitos de túnel de dispositivo
Debes habilitar la autenticación de certificado de máquina para las conexiones VPN y definir una entidad de certificación raíz para autenticar las conexiones VPN entrantes. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Requisitos y características de túnel de dispositivo](../../media/device-tunnel-feature-and-requirements.png)

## Configuración de túnel de dispositivo VPN

El perfil de ejemplo XML siguiente proporciona una buena orientación para escenarios donde solo cliente inicia extrae son necesarias sobre el túnel de dispositivo.  Filtros de tráfico se aprovechan para restringir el túnel de dispositivo para solo el tráfico de administración.  Esta configuración funciona bien para Windows Update, directiva de grupo típica (GP) y escenarios de actualización de System Center Configuration Manager (SCCM), así como las conexiones VPN para el primer inicio de sesión sin credenciales almacenadas en caché, o escenarios de restablecimiento de contraseña. 

Para los casos de inserción iniciados por el servidor, como la administración remota de Windows (WinRM), GPUpdate remoto y escenarios de actualización SCCM remotos: debe permitir el tráfico entrante en el túnel de dispositivo, por lo que no se pueden usar filtros de tráfico.  Si en el perfil de túnel de dispositivo activar filtros de tráfico, el túnel de dispositivo deniega el tráfico entrante.  Esta limitación se va a quitar en futuras versiones.


### ProfileXML VPN de muestra

Siguiente es el profileXML VPN de muestra.

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

Según las necesidades de cada escenario de implementación particular, otra característica VPN que puede configurarse con el túnel de dispositivo es la [Detección de redes de confianza](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection --> 
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection> 
```

## Implementación y las pruebas

Puedes configurar túneles de dispositivo mediante un script de Windows PowerShell y el puente de dispositivo de Windows Management Instrumentation \(WMI\). El túnel de dispositivo de VPN siempre activada debe estar configurado en el contexto de la cuenta del **Sistema LOCAL** . Para ello, será necesario usar [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), uno de [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) incluidas en la serie de [Sysinternals](https://docs.microsoft.com/sysinternals/) de utilidades.

Para obtener instrucciones sobre cómo implementar una por dispositivo `(.\Device)` frente a un por usuario `(.\User)` de perfil, consulta el [scripting de PowerShell utilizando con el proveedor de puentes de WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider). 

Ejecuta el siguiente comando de Windows PowerShell para comprobar que ha implementado correctamente un perfil de dispositivo:

    `Get-VpnConnection -AllUserConnection`

El resultado muestra una lista de los perfiles VPN de todo el Selecc que se implementan en el dispositivo.


### Script de PowerShell de Windows de ejemplo

Puedes usar el siguiente script de Windows PowerShell para ayudar a crear tu propio script para la creación del perfil.

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

## Recursos adicionales

A continuación es recursos adicionales para ayudar con la implementación de VPN.

### Recursos de configuración de cliente VPN

Estos son los recursos de configuración de cliente VPN.

- [Cómo crear VPN perfiles en System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurar las conexiones VPN de Always On del cliente de Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opciones de perfil de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### Remoto \(RAS\) servidor de acceso a recursos de puerta de enlace

A continuación es recursos de puerta de enlace RAS.

- [Configurar RRAS con un certificado de autenticación de equipo](https://technet.microsoft.com/library/dd458982.aspx)
- [Solución de problemas de las conexiones VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurar el acceso remoto basadas en IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Al usar túnel de dispositivo con una puerta de enlace de RAS de Microsoft, tendrás que configurar el servidor RRAS para admitir la autenticación de certificado de máquina IKEv2 habilitando el método de autenticación de **Permitir la autenticación de certificado de máquina para IKEv2** , tal como se describe [aquí](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Cuando se habilita esta configuración, se recomienda encarecidamente que se usa el cmdlet de PowerShell **Conjunto VpnAuthProtocol** , junto con el parámetro opcional **RootCertificateNameToAccept** , para garantizar que las conexiones RRAS IKEv2 solo se permiten para Cliente VPN certificados que se encadenan a un definidos explícitamente interno o privado entidad de certificación raíz. Como alternativa, el almacén de **Entidades de certificación raíz de confianza** en el servidor RRAS debe modificarse para garantizar que no contienen entidades de certificación pública como se describe [aquí](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Métodos similares también tengas que deben considerarse para otras puertas de enlace VPN.

---
