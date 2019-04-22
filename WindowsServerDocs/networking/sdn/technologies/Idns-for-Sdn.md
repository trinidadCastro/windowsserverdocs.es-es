---
title: Servicio DNS interno (iDNS) para SDN
description: En este tema se explica cómo puede proporcionar servicios DNS para las cargas de trabajo de inquilino hospedado mediante el uso de DNS interno (iDNS), que está integrado con redes definidas por Software en Windows Server 2016.
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
ms.openlocfilehash: 4d4ae5ee5f5600d86349ca26b7acbdb284b45bac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824086"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Servicio DNS interno (iDNS) para SDN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Si trabaja para un proveedor de servicios en la nube \(CSP\) o empresa que planea implementar redes definidas por Software \(SDN\) en Windows Server 2016, puede proporcionar servicios DNS para las cargas de trabajo de inquilinos hospedados mediante el uso de DNS interno \(IDN\), que está integrado con SDN.

Hospeda las máquinas virtuales \(máquinas virtuales\) y las aplicaciones requieren DNS para la comunicación dentro de sus propias redes y con recursos externos en Internet. Con iDNS, puede proporcionar inquilinos con servicios de resolución de nombres DNS de su espacio de nombres local, aislado y los recursos de Internet.

Dado que el servicio de IDN no es accesible desde redes virtuales de inquilino, distinto a través del proxy de IDN, el servidor no es vulnerable a actividades malintencionadas en redes de inquilinos.

**Características clave**

A continuación es las principales características de IDN.

- Proporciona el que nombre de DNS compartidos servicios de resolución para el inquilino de cargas de trabajo
- Servicio DNS autoritativo para la resolución de nombres y el registro DNS en el espacio de nombres de inquilino
- Servicio Recursive DNS para resolución de nombres Internet de las máquinas virtuales de inquilino.
- Si lo desea, puede configurar el hospedaje simultáneas de los nombres de fabric y de inquilinos
- Una solución rentable de DNS: los inquilinos no es necesario implementar su propia infraestructura de DNS
- Alta disponibilidad con la integración de Active Directory, que es necesaria.

Además de estas características, si le preocupa mantener su AD integrado servidores DNS abiertos a Internet, puede implementar servidores de IDN detrás de otra resolución recursiva de la red perimetral.

Dado que iDNS es un servidor centralizado para todas las consultas DNS, un CSP o Enterprise también puede implementar servidores de seguridad DNS de inquilino, aplicar filtros, detectar actividades malintencionadas y auditar las transacciones en una ubicación central

## <a name="idns-infrastructure"></a>Infraestructura de IDN
La infraestructura de IDN incluye servidores de IDN y proxy de IDN.

### <a name="idns-servers"></a>Servidores de IDN
IDN incluye un conjunto de servidores DNS que hospedan datos específicos del inquilino, como registros de recursos DNS de máquina virtual.

servidores de IDN son los servidores autoritativos para sus zonas DNS internos y también actúan como una resolución de nombres públicos cuando intento de máquinas virtuales para conectarse a recursos externos del inquilino.

Todos los nombres de host para máquinas virtuales en redes virtuales se almacenan como registros de recursos DNS en la misma zona. Por ejemplo, si implementa iDNS para una zona denominada contoso.local, los registros de recursos DNS para las máquinas virtuales en esa red se almacenan en la zona de contoso.local.

Máquina virtual de nombres de dominio completo del inquilino \(FQDN\) consisten en el nombre del equipo y la cadena de sufijo DNS para la red Virtual, en formato GUID. Por ejemplo, si tiene un inquilino de máquina virtual denominada TENANT1 que se encuentra en la red Virtual de contoso, local, el FQDN de la máquina virtual es TENANT1. *vn guid*. contoso.local, donde *vn guid* es la cadena de sufijo DNS para la red Virtual.

>[!NOTE]
>Si es un administrador del tejido, puede usar su infraestructura de DNS de la empresa o de CSP como servidores de IDN en lugar de implementar nuevos servidores DNS específicamente para utilizan como IDN servidores. Si implementa nuevos servidores IDN o utilizar la infraestructura existente, IDN se basa en Active Directory para proporcionar alta disponibilidad. Por lo tanto, los servidores de IDN deben estar integrados con Active Directory.

### <a name="idns-proxy"></a>Proxy de IDN
proxy de IDN es un servicio de Windows que se ejecuta en todos los hosts, y que reenvía el inquilino tráfico de DNS de red Virtual para el servidor de IDN.

La siguiente ilustración muestra las rutas de tráfico DNS en redes virtuales de inquilino a través del proxy de IDN para los IDN Server e Internet.

![Infraestructura de IDN](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Cómo implementar IDN
Cuando se implementa SDN en Windows Server 2016 mediante el uso de secuencias de comandos, IDN se incluye automáticamente en su implementación.

Para obtener más información, vea los temas siguientes:

- [Implementar una infraestructura de red definida por Software con scripts](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Descripción de los pasos de implementación de IDN
Puede utilizar esta sección para obtener una descripción de cómo IDN está instalado y configurado cuando se implementa SDN mediante scripts.

Siguiente es un resumen de los pasos necesarios para implementar IDN.

>[!NOTE]
>Si ha implementado SDN mediante scripts, no es necesario realizar cualquiera de estos pasos. Se proporcionan los pasos para obtener información y solo con fines de solución de problemas.

### <a name="step-1-deploy-dns"></a>Paso 1: Implementar DNS
Puede implementar un servidor DNS mediante el siguiente comando de Windows PowerShell de ejemplo.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Paso 2: Configurar la información de IDN en la controladora de red
Este segmento de la secuencia de comandos es una llamada REST que se realiza por el administrador para la controladora de red, para informar sobre la configuración de zona de IDN: como la dirección IP de la iDNSServer y la zona que se usa para hospedar los nombres de IDN. 

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
>Esto es un extracto de la sección **configuración ConfigureIDns** en SDNExpress.ps1. Para obtener más información, consulte [implementar una infraestructura de red definida por Software con scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Paso 3: Configurar el servicio de Proxy de IDN
El servicio de Proxy de IDN se ejecuta en cada uno de los hosts de Hyper-V, que proporciona el puente entre las redes virtuales de inquilinos y la red física donde se encuentran los servidores de IDN. Las siguientes claves del registro deben crearse en cada host de Hyper-V.


**Puerto DNS:** Puerto 53 fijo

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "Port"
- ValueData = 53
- ValueType = "Dword"
       

**Puerto de Proxy de DNS:** Puerto 53 fijo

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**IP DE DNS:** Dirección IP fija configurado en la interfaz de red, en caso de que el inquilino decide usar el servicio de IDN

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "String"

        
**Dirección MAC:** Dirección de Media Access Control del servidor DNS

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = “aa-bb-cc-aa-bb-cc”
- ValueType = "String"

**Dirección del servidor de IDN:** Lista de servidores IDN de separados por una coma.

- Claves del Registro: HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "Reenviadores"
- ValueData = "10.0.0.9"
- ValueType = "String"



>[!NOTE]
>Esto es un extracto de la sección **configuración ConfigureIDnsProxy** en SDNExpress.ps1. Para obtener más información, consulte [implementar una infraestructura de red definida por Software con scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Paso 4: Reinicie el servicio de agente de Host de controlador de red
Puede usar el siguiente comando de Windows PowerShell para reiniciar el servicio de agente de Host del controlador de red.
    
    Restart-Service nchostagent -Force
    
Para obtener más información, consulte [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Habilitar reglas de firewall para el servicio de proxy DNS
Puede usar el siguiente comando de Windows PowerShell para crear una regla de firewall que permita excepciones para el proxy para comunicarse con la máquina virtual y el servidor de IDN.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Para obtener más información, consulte [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Validar el servicio de IDN
Para validar los IDN servicio, debe implementar una carga de trabajo de inquilinos de ejemplo.

Para obtener más información, consulte [crear una máquina virtual y conectarse a una red Virtual del inquilino o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Si desea que la máquina virtual para usar el servicio de IDN del inquilino, debe dejar la configuración de servidor DNS de las interfaces de red de máquina virtual en blanco y permitir que las interfaces que se va a usar DHCP. 

Una vez iniciada la máquina virtual con una interfaz de red de este tipo, recibe automáticamente una configuración que permite que la máquina virtual para usar IDN, y la máquina virtual empieza inmediatamente a realizar la resolución de nombres mediante el servicio de IDN.

Si configura la máquina virtual para usar el servicio de IDN si se deja en blanco la información de servidor DNS y servidor DNS alternativo de la interfaz de red del inquilino, controladora de red proporciona la máquina virtual con una dirección IP y realiza un registro de nombres DNS en nombre de la máquina virtual con el servidor de IDN . 

Controladora de red informa también del proxy de IDN acerca de la máquina virtual y los detalles necesarios para realizar la resolución de la máquina virtual. 

Cuando la máquina virtual inicia una consulta DNS, el servidor proxy actúa como un reenviador de la consulta desde la red Virtual para el servicio de IDN. 

El proxy DNS también garantiza que las consultas de la máquina virtual de inquilinos estén aisladas. Si el servidor de IDN es autoritativo para la consulta, el servidor de IDN responde con una respuesta autorizada. Si el servidor de IDN no es autoritativo para la consulta, lleva a cabo una recursividad DNS para resolver los nombres de Internet.

>[!NOTE]
>Esta información se incluye en la sección **configuración AttachToVirtualNetwork** en SDNExpressTenant.ps1. Para obtener más información, consulte [implementar una infraestructura de red definida por Software con scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

