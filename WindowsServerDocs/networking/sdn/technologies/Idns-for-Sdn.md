---
title: Servicio DNS interno (iDNS) para SDN
description: En este tema se explica cómo puede proporcionar servicios DNS a las cargas de trabajo de inquilino hospedado mediante el uso de DNS interno (IDN), que se integra con las redes definidas por software en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a7e5aa9e1ae7442c706c1bdbdb56d65234fe5ae8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405962"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Servicio DNS interno (iDNS) para SDN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Si trabaja para un proveedor de servicios en la nube \(CSP\) o Enterprise que planea implementar redes definidas por software \(SDN\) en Windows Server 2016, puede proporcionar servicios DNS a las cargas de trabajo de inquilino hospedado mediante el uso de DNS interno \(IDN\), que se integra con SDN.

Máquinas virtuales hospedadas \(máquinas virtuales\) y las aplicaciones requieren que el DNS se comunique dentro de sus propias redes y con recursos externos en Internet. Con IDN, puede proporcionar a los inquilinos los servicios de resolución de nombres DNS para su espacio de nombres local aislado y para los recursos de Internet.

Dado que el servicio IDN no es accesible desde redes virtuales de inquilinos que no sean a través del proxy de IDN, el servidor no es vulnerable a las actividades malintencionadas en las redes de inquilinos.

**Características clave**

A continuación se indican las características clave de IDN.

- Proporciona servicios de resolución de nombres DNS compartidos para cargas de trabajo de inquilino
- Servicio DNS autoritativo para la resolución de nombres y el registro DNS en el espacio de nombres de inquilino
- Servicio DNS recursivo para la resolución de nombres de Internet de máquinas virtuales de inquilinos.
- Si lo desea, puede configurar el hospedaje simultáneo de nombres de tejido y de inquilinos.
- Una solución DNS rentable: los inquilinos no necesitan implementar su propia infraestructura DNS.
- Alta disponibilidad con la integración de Active Directory, que es necesaria.

Además de estas características, si le preocupa mantener los servidores DNS integrados en AD abiertos a Internet, puede implementar servidores de IDN detrás de otra resolución recursiva en la red perimetral.

Dado que IDN es un servidor centralizado para todas las consultas DNS, un CSP o una empresa también puede implementar firewalls DNS de inquilino, aplicar filtros, detectar actividades malintencionadas y auditar transacciones en una ubicación central.

## <a name="idns-infrastructure"></a>Infraestructura de IDN
La infraestructura de IDN incluye servidores IDN y proxy IDN.

### <a name="idns-servers"></a>Servidores de IDN
IDN incluye un conjunto de servidores DNS que hospedan datos específicos del inquilino, como los registros de recursos DNS de la máquina virtual.

los servidores de IDN son los servidores autoritativos para sus zonas DNS internas y también actúan como una resolución de nombres públicos cuando las máquinas virtuales de inquilino intentan conectarse a recursos externos.

Todos los nombres de host de las máquinas virtuales de las redes virtuales se almacenan como registros de recursos DNS en la misma zona. Por ejemplo, si implementa IDN para una zona denominada contoso. local, los registros de recursos DNS de las máquinas virtuales de esa red se almacenan en la zona contoso. local.

Los nombres de dominio completos de las máquinas virtuales de inquilino \(FQDN\) constan del nombre del equipo y de la cadena del sufijo DNS para el Virtual Network, en formato GUID. Por ejemplo, si tiene una máquina virtual de inquilino denominada TENANT1 que está en el Virtual Network Contoso, local, el FQDN de la máquina virtual es TENANT1. *vn-GUID*. contoso. local, donde *vn-GUID* es la cadena de sufijo DNS para el Virtual Network.

>[!NOTE]
>Si es administrador del tejido, puede usar la infraestructura de CSP o DNS empresarial como servidores de IDN en lugar de implementar nuevos servidores DNS específicamente para su uso como servidores de IDN. Tanto si implementa nuevos servidores para IDN como si usa la infraestructura existente, IDN se basa en Active Directory para proporcionar alta disponibilidad. Por lo tanto, los servidores de IDN deben integrarse con Active Directory.

### <a name="idns-proxy"></a>Proxy IDN
el proxy de IDN es un servicio de Windows que se ejecuta en todos los hosts y que reenvía el tráfico DNS del inquilino Virtual Network al servidor de IDN.

En la ilustración siguiente se muestran las rutas de tráfico DNS de las redes virtuales de inquilino a través del proxy de IDN al servidor de IDN e Internet.

![Infraestructura de IDN](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Cómo implementar IDN
Al implementar SDN en Windows Server 2016 mediante el uso de scripts, IDN se incluye automáticamente en la implementación.

Para obtener más información, vea los temas siguientes:

- [Implementar una infraestructura de red definida por software mediante scripts](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Descripción de los pasos de implementación de IDN
Puede usar esta sección para entender cómo se instala y configura IDN al implementar SDN mediante scripts.

A continuación se indica un resumen de los pasos necesarios para implementar IDN.

>[!NOTE]
>Si ha implementado SDN mediante el uso de scripts, no es necesario que realice ninguno de estos pasos. Los pasos se proporcionan únicamente con fines informativos y de solución de problemas.

### <a name="step-1-deploy-dns"></a>Paso 1: implementación de DNS
Puede implementar un servidor DNS mediante el siguiente comando de ejemplo de Windows PowerShell.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Paso 2: configuración de la información de IDN en la controladora de red
Este segmento de script es una llamada REST realizada por el administrador a la controladora de red, que le informa sobre la configuración de la zona IDN, como la dirección IP del iDNSServer y la zona que se usa para hospedar los nombres de IDN. 

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
>Este es un extracto de la sección **Configuration ConfigureIDns** en SDNExpress. ps1. Para obtener más información, vea [implementación de una infraestructura de red definida por software mediante scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Paso 3: configurar el servicio de proxy de IDN
El servicio de proxy IDN se ejecuta en cada uno de los hosts de Hyper-V y proporciona el puente entre las redes virtuales de los inquilinos y la red física donde se encuentran los servidores de IDN. Las siguientes claves del registro deben crearse en cada host de Hyper-V.


**Puerto DNS:** Puerto fijo 53

- Clave del registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "puerto"
- ValueData = 53
- ValueType = "DWORD"
       

**Puerto de proxy DNS:** Puerto fijo 53

- Clave del registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "DWORD"
        
**IP de DNS:** Dirección IP fija configurada en la interfaz de red, en caso de que el inquilino elija usar el servicio IDN

- Clave del registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "cadena"

        
**Dirección Mac:** Dirección de Access Control de medios del servidor DNS

- Clave del registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = "AA-BB-CC-AA-BB-CC"
- ValueType = "cadena"

**Dirección del servidor de IDN:** Una lista separada por comas de servidores de IDN.

- Clave del registro: HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "reenviadores"
- ValueData = "10.0.0.9"
- ValueType = "cadena"



>[!NOTE]
>Este es un extracto de la sección **Configuration ConfigureIDnsProxy** en SDNExpress. ps1. Para obtener más información, vea [implementación de una infraestructura de red definida por software mediante scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Paso 4: reiniciar el servicio del agente de host de la controladora de red
Puede usar el siguiente comando de Windows PowerShell para reiniciar el servicio del agente de host de la controladora de red.
    
    Restart-Service nchostagent -Force
    
Para obtener más información, vea [restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Habilitar reglas de Firewall para el servicio de proxy DNS
Puede usar el siguiente comando de Windows PowerShell para crear una regla de firewall que permita que las excepciones del proxy se comuniquen con la máquina virtual y el servidor IDN.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Para obtener más información, vea [enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Validar el servicio IDN
Para validar el servicio IDN, debe implementar una carga de trabajo de inquilino de ejemplo.

Para obtener más información, consulte [creación de una máquina virtual y conexión a un inquilino Virtual Network o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Si desea que la máquina virtual del inquilino use el servicio IDN, debe dejar en blanco la configuración del servidor DNS de las interfaces de red de la máquina virtual y permitir que las interfaces usen DHCP. 

Una vez que se inicia la máquina virtual con una interfaz de red de este tipo, recibe automáticamente una configuración que permite que la máquina virtual use IDN, y la máquina virtual comienza inmediatamente a realizar la resolución de nombres mediante el servicio IDN.

Si configura la máquina virtual de inquilino para usar el servicio IDN al dejar en blanco el servidor DNS de la interfaz de red y la información alternativa del servidor DNS, el controlador de red proporciona a la máquina virtual una dirección IP y realiza un registro de nombres DNS en nombre de la máquina virtual con el servidor IDN . 

La controladora de red también informa al proxy de IDN sobre la máquina virtual y los detalles necesarios para realizar la resolución de nombres de la máquina virtual. 

Cuando la máquina virtual inicia una consulta DNS, el proxy actúa como reenviador de la consulta desde el Virtual Network al servicio IDN. 

El proxy DNS también garantiza que las consultas de VM de inquilino estén aisladas. Si el servidor IDN es autoritativo para la consulta, el servidor de IDN responde con una respuesta autoritativa. Si el servidor IDN no es autoritativo para la consulta, realiza una recursividad DNS para resolver los nombres de Internet.

>[!NOTE]
>Esta información se incluye en la sección **Configuration AttachToVirtualNetwork** en SDNExpressTenant. ps1. Para obtener más información, vea [implementación de una infraestructura de red definida por software mediante scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

