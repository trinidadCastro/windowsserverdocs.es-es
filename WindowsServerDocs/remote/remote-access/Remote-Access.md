---
title: Acceso remoto
description: En este tema se proporciona información general sobre el rol de servidor de acceso remoto en Windows Server 2016.
manager: dougkim
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/18/2018
ms.openlocfilehash: 3efbc9d05beacdcc1d8ceb466f25dfc59f2bae0c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970452"
---
# <a name="remote-access"></a>Acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

La guía de acceso remoto le proporciona información general sobre el rol de servidor de acceso remoto en Windows Server 2016 y trata los temas siguientes:

- [Guía de implementación de Always On VPN](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Protocolo de puerta de enlace de borde &#40;&#41;BGP](bgp/Border-Gateway-Protocol-BGP.md)
- [Puerta de enlace RAS](ras-gateway/RAS-Gateway.md)
- [Documentación de rol de servidor de acceso remoto](ras/Remote-Access-Server-Role-Documentation.md)
- [Puerta de enlace RAS para SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [Redes privadas virtuales (VPN)](vpn/vpn-top.md)

Para obtener más información acerca de otras tecnologías de red, consulte [funciones de red en Windows Server 2016](../../networking/index.yml).

El rol de servidor de acceso remoto es una agrupación lógica de estas tecnologías de acceso a la red relacionadas: [servicio de acceso remoto (RAS)](#bkmk_da), [enrutamiento](#bkmk_rras)y [proxy de aplicación web](#bkmk_proxy). Estas tecnologías son los *Servicios de rol* del rol de servidor de acceso remoto. Al instalar el rol de servidor de acceso remoto con el **Asistente para agregar roles y características** o Windows PowerShell, puede instalar uno o varios de estos tres servicios de rol.

>[!IMPORTANT]
>No intente implementar el acceso remoto en una \( VM de máquina virtual \) en Microsoft Azure. No se admite el uso de acceso remoto en Microsoft Azure. No se puede usar el acceso remoto en una máquina virtual de Azure para implementar VPN, DirectAccess o cualquier otra característica de acceso remoto en Windows Server 2016 o versiones anteriores de Windows Server. Para obtener más información, vea [compatibilidad de software de servidor de Microsoft con máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="remote-access-service-ras---ras-gateway"></a><a name="bkmk_da"></a>\(Ras del servicio de acceso remoto \) -puerta de enlace ras

Al instalar el servicio de rol **DirectAccess y VPN (RAS)** , está implementando la \( **puerta de enlace ras**de puerta de enlace de servicio de acceso remoto \) . Puede implementar la puerta de enlace RAS en un servidor VPN de red privada virtual de puerta de enlace RAS de un solo inquilino \( \) , un servidor VPN de puerta de enlace ras multiempresa y como un servidor de DirectAccess.

- **Puerta de enlace ras: un solo inquilino**. Mediante el uso de la puerta de enlace RAS, puede implementar conexiones VPN para proporcionar a los usuarios finales acceso remoto a la red y los recursos de la organización. Si los clientes ejecutan Windows 10, puede implementar Always On VPN, que mantiene una conexión persistente entre los clientes y la red de la organización siempre que los equipos remotos estén conectados a Internet. Con la puerta de enlace de RAS, también puede crear una conexión VPN de sitio a sitio entre dos servidores en diferentes ubicaciones, como entre la oficina principal y una sucursal, y usar NAT de traducción de direcciones de red \( \) para que los usuarios dentro de la red puedan acceder a recursos externos, como Internet. Además, la puerta de enlace RAS admite Protocolo de puerta de enlace de borde (BGP), que proporciona servicios de enrutamiento dinámico cuando las ubicaciones de oficinas remotas también tienen puertas de enlace perimetrales que admiten BGP.

- **Puerta de enlace ras: multiinquilino**. Puede implementar la puerta de enlace RAS como un enrutador y puerta de enlace perimetral basado en software y multiinquilino cuando se usa la \- virtualización de red de Hyper V o se han implementado redes de VM con redes de área local virtuales \( VLAN \) . Con la puerta de enlace RAS, los proveedores de servicios en la nube \( CSP \) y empresas pueden habilitar el enrutamiento del tráfico de red del centro de recursos y la nube entre redes físicas y virtuales, incluido Internet. Con la puerta de enlace RAS, los inquilinos pueden usar conexiones VPN de punto a sitio para tener acceso a los recursos de red de la máquina virtual en el centro de bits desde cualquier lugar. También puede proporcionar inquilinos con conexiones VPN de sitio a sitio entre sus sitios remotos y el centro de recursos de CSP. Además, puede configurar la puerta de enlace RAS con BGP para el enrutamiento dinámico, y puede habilitar NAT de traducción de direcciones de red \( \) para proporcionar acceso a Internet para las máquinas virtuales en redes de VM.

>[!IMPORTANT]
> La puerta de enlace RAS con capacidades multiinquilino también está disponible en Windows Server 2012 R2.

- **VPN Always on**. Always On VPN permite a los usuarios remotos acceder de forma segura a los recursos compartidos, sitios web de la intranet y aplicaciones de una red interna sin necesidad de conectarse a una VPN.

Para obtener más información, consulte [puerta de enlace de Ras](ras-gateway/RAS-Gateway.md) y [Protocolo de puerta de enlace de borde (BGP)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="routing"></a><a name="bkmk_rras"></a>Enrutamiento

Puede usar el acceso remoto para enrutar el tráfico de red entre subredes de la red de área local. El enrutamiento proporciona compatibilidad con los enrutadores de traducción de direcciones de red (NAT), los enrutadores de LAN que ejecutan BGP, el protocolo de información de enrutamiento (RIP) y los enrutadores compatibles con multidifusión mediante el protocolo de administración de grupos de Internet (IGMP). Como enrutador completo, puede implementar RAS en un equipo servidor o como una máquina virtual (VM) en un equipo que ejecuta Hyper-V.

Para instalar el acceso remoto como un enrutador LAN, use el Asistente para agregar roles y características en Administrador del servidor y seleccione el rol de servidor de **acceso remoto** y el servicio de rol de **enrutamiento** . o bien, escriba el siguiente comando en un símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

```
Install-RemoteAccess -VpnType RoutingOnly
```

## <a name="web-application-proxy"></a><a name="bkmk_proxy"></a>Proxy de aplicación web

Proxy de aplicación web es un servicio de rol de acceso remoto en Windows Server 2016. Web Application Proxy proporciona funcionalidad de proxy inverso para aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa. Proxy de aplicación web autentica previamente el acceso a las aplicaciones web mediante Servicios de federación de Active Directory (AD FS) (AD FS) y también funciona como un proxy AD FS.

Para instalar el acceso remoto como un proxy de aplicación Web, use el Asistente para agregar roles y características en Administrador del servidor y seleccione el rol de servidor de **acceso remoto** y el servicio de rol **proxy de aplicación web** . o bien, escriba el siguiente comando en un símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.

```
Install-RemoteAccess -VpnType SstpProxy
```

Para obtener más información, vea [proxy de aplicación web](./web-application-proxy/web-application-proxy-windows-server.md).


---
