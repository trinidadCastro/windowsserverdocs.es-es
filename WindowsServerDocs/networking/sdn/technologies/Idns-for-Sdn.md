---
title: Servicio de DNS interno (IDN) para SDN
description: Este tema explica cómo puedes proporcionar servicios DNS para las cargas de trabajo de inquilino hospedado mediante DNS interno (IDN), que se integra con Software definido de red en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4bdb495b585cf60315dee60f9365e282877fdc1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="internal-dns-service-idns-for-sdn"></a>\(IDNS\) de servicio DNS interno para SDN

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Si trabajas para un proveedor de servicios de nube \(CSP\) o empresa que se va a implementar Software definido redes \(SDN\) en Windows Server 2016, puede proporcionar servicios DNS para las cargas de trabajo de inquilino hospedado mediante DNS interno \(iDNS\), que se integra con SDN.

\(VMs\) hospedado máquinas virtuales y las aplicaciones requieren DNS para la comunicación entrante en sus propias redes con recursos externos en Internet. Con IDN, puedes proporcionar inquilinos con servicios de resolución de nombres DNS de su espacio de nombres aislado, local y de recursos de Internet.

Dado que el servicio de IDN no es accesible desde el inquilino de redes virtuales, que no sea a través del proxy IDN, el servidor no es vulnerable a actividades malintencionadas en redes de inquilino.

**Características principales**

A continuación es las características clave para IDN.

- Proporciona el que nombre de DNS compartidas servicios de resolución para inquilino cargas de trabajo
- Servicio autorizado de DNS para la resolución de nombres y el registro DNS dentro del espacio de nombre de inquilino
- Servicio de Recursive DNS para la resolución de nombres de Internet de inquilino de máquinas virtuales.
- Si lo deseas, puedes configurar simultáneas de hospedaje de nombres fabric e inquilino
- Una solución rentable de DNS - inquilinos no es necesario implementar su propia infraestructura DNS
- Alta disponibilidad con la integración de Active Directory, que es necesaria.

Además de estas características, si te preocupa sobre cómo mantener tu anuncio integrado servidores DNS abran a Internet, puedes implementar servidores IDN detrás de otro resolución recursiva en la red perimetral.

Como IDN es un servidor centralizado para todas las consultas DNS, un CSP o una empresa puede también implementar inquilino DNS firewalls, aplicar filtros, detectar actividades malintencionadas y auditar las transacciones en una ubicación central

## <a name="idns-infrastructure"></a>IDN infraestructura
La infraestructura de IDN incluye IDN servidores e IDN proxy.

### <a name="idns-servers"></a>IDN servidores
IDN incluye un conjunto de servidores DNS que hospedan los datos específicos de inquilinos, como registros de recursos de DNS de máquina virtual.

son los servidores autorizados para sus zonas DNS internos IDN servidores; Además, actuar como una resolución de nombres públicos inquilinos cuando intento de máquinas virtuales para conectarse a recursos externos.

Todos los nombres de host para máquinas virtuales en redes virtuales se almacenan como registros de recursos de DNS en la misma zona. Por ejemplo, si implementas IDN para una zona denominada contoso.local, los registros de recursos de DNS para las máquinas virtuales en esa red se almacenan en la zona contoso.local.

Inquilino VM nombres de dominio completos \(FQDNs\) consisten en el nombre del equipo y la cadena de sufijo DNS para la red Virtual, en formato GUID. Por ejemplo, si tienes un máquina virtual denominado TENANT1 que se encuentra en el red Virtual de contoso, local, del inquilino FQDN de la máquina virtual es TENANT1. *vn guid*. contoso.local, donde *vn guid* es la cadena de sufijo DNS para la red Virtual.

>[!NOTE]
>Si eres un administrador fabric, puedes usar la infraestructura CSP o empresa DNS como servidores IDN en lugar de implementar nuevos servidores DNS específicamente para usan como IDN servidores. Si implementas nuevos servidores para IDN o usar la infraestructura existente, IDN se basa en Active Directory para ofrecer una alta disponibilidad. Por lo tanto, los servidores de IDN deben estar integrados con Active Directory.

### <a name="idns-proxy"></a>IDN Proxy
proxy IDN es un servicio de Windows que se ejecuta en todos los host, y que reenvía el inquilino de tráfico de DNS de red Virtual para el servidor IDN.

La ilustración siguiente muestra las rutas de tráfico DNS de inquilino redes virtuales a través del proxy IDN para el servidor IDN e Internet.

![IDN infraestructura](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Cómo implementar IDN
Cuando implementas SDN en Windows Server 2016 mediante el uso de scripts, IDN se incluye automáticamente en la implementación.

Para obtener más información, consulta los siguientes temas.

- [Implementar una infraestructura de red definido de Software mediante scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Descripción de los pasos de implementación de IDN
Puedes usar esta sección para comprender cómo IDN está instalado y configurado cuando implementas SDN mediante scripts.

La siguiente es un resumen de los pasos necesarios para implementar IDN.

>[!NOTE]
>Si ha implementado SDN mediante el uso de scripts, no necesitas realizar cualquiera de estos pasos. Se proporcionan los pasos para obtener información y solo con fines de solución de problemas.

### <a name="step-1-deploy-dns"></a>Paso 1: Implementar DNS
Puedes implementar un servidor DNS mediante el comando de Windows PowerShell de ejemplo siguiente.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Paso 2: Configurar IDN información en el controlador de red
Este segmento de script es una llamada REST se realiza por el administrador en el controlador de red, para informar sobre la configuración de zona IDN - como la dirección IP de la iDNSServer y la zona que se usa para hospedar los nombres IDN. 

```
    Url: https://<url>/networking/v1/iDnsServer/configuration
Method: PUT
{
      "properties": {
        "connections": [
          {
            "managementAddresses": [
              "10.0.0.9"
            ],
            "credential": {
              "resourceRef": "/credentials/iDnsServer-Credentials"
            },
            "credentialType": "usernamePassword"
          }
        ],
        "zone": "contoso.local"
      }
    }
```

>[!NOTE]
>Este es un fragmento de la sección **configuración ConfigureIDns** en SDNExpress.ps1. Para obtener más información, consulta [implementar una infraestructura de red definido de Software mediante scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Paso 3: Configurar el servicio de Proxy IDN
La se ejecuta el servicio de Proxy IDN en cada uno de los hosts de Hyper-V, que proporciona el puente entre las redes virtuales de inquilinos y la red física donde se encuentran los servidores de IDN. Las siguientes claves de registro deben crearse en todos los host Hyper-V.


**Puerto DNS:** puerto 53 fijo

- Clave del registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- Valor = "Puerto"
- DatosDeValor = 53
- Tipo de valor = "Dword"
       

**Puerto de Proxy de DNS:** puerto 53 fijo

- Clave del registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- Valor = "ProxyPort"
- DatosDeValor = 53
- Tipo de valor = "Dword"
        
**Dirección IP de DNS:** es la dirección IP fija configurada en la interfaz de red, en caso de que el inquilino decide usar el servicio de IDN.

- Clave del registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- Valor = "IP"
- DatosDeValor = "169.254.169.254"
- Tipo de valor = "String"

        
**Dirección MAC:** dirección Media Access Control del servidor DNS

- Clave del registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- Valor = "MAC"
- DatosDeValor = "aa-bb-cc-aa-bb-cc"
- Tipo de valor = "String"

**Dirección del servidor IDN:** lista de servidores de IDN de separados por comas.

- Clave del registro: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- Valor = "Servidores de reenvío"
- DatosDeValor = "10.0.0.9"
- Tipo de valor = "String"



>[!NOTE]
>Este es un fragmento de la sección **configuración ConfigureIDnsProxy** en SDNExpress.ps1. Para obtener más información, consulta [implementar una infraestructura de red definido de Software mediante scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Paso 4: Reiniciar el servicio de agente de Host de controlador de red
Puedes usar el siguiente comando de Windows PowerShell para reiniciar el servicio de agente de Host de controlador de red.
    
    Restart-Service nchostagent -Force
    
Para obtener más información, consulta [reinicio servicio](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Habilitar las reglas de firewall para el servicio de proxy DNS
Puedes usar el siguiente comando de Windows PowerShell para crear una regla de firewall que permite que las excepciones para el servidor proxy para comunicarse con el servidor IDN y de la máquina virtual.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Para obtener más información, consulta [habilitar NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Validar el servicio de IDN
Para validar el servicio de IDN, debes implementar una carga de trabajo del inquilino de muestra.

Para obtener más información, consulta [crear una máquina virtual y conectarse a una red Virtual de inquilino o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Si quieres que la máquina virtual para usar el servicio de IDN inquilino, debe dejar vacía la configuración de servidor DNS de interfaces de red de máquina virtual y permitir que las interfaces usar DHCP. 

Después de que se inicia la máquina virtual con una interfaz de red, recibe automáticamente una configuración que permite que la máquina virtual usar IDN y la máquina virtual inicia inmediatamente la realización de resolución de nombres mediante el servicio de IDN.

Si configuras al inquilino VM para usar el servicio de IDN deja en blanco la información de servidor DNS y el servidor DNS alternativo de la interfaz de red, el controlador de red proporciona la máquina virtual con una dirección IP y realiza un registro de nombre DNS en nombre de la máquina virtual con el servidor IDN. 

Controlador de red también informa al proxy IDN acerca de la máquina virtual y los detalles necesarios para realizar la resolución de nombres para la máquina virtual. 

Cuando la máquina virtual inicia una consulta DNS, el proxy actúa como servidor de envío de la consulta de la red Virtual para el servicio de IDN. 

El proxy DNS también garantiza que las consultas de máquina virtual de inquilino aisladas. Si el servidor IDN está autorizado para la consulta, el servidor de IDN responde con una respuesta autorizada. Si el servidor IDN no está autorizado para la consulta, realiza una recursividad DNS para resolver los nombres de Internet.

>[!NOTE]
>Esta información se incluye en la sección **configuración AttachToVirtualNetwork** en SDNExpressTenant.ps1. Para obtener más información, consulta [implementar una infraestructura de red definido de Software mediante scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

