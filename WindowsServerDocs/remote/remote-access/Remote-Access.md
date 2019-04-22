---
title: Acceso remoto
description: Este tema proporciona información general sobre el rol de servidor de acceso remoto en Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: faf12ad22678fa58ea933613759e3e8414966bca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818366"
---
# <a name="remote-access"></a>Acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

La Guía de acceso remoto proporciona una visión general del rol de servidor de acceso remoto en Windows Server 2016 y trata a los siguientes temas:

- [Siempre en la Guía de implementación de VPN](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Protocolo de puerta de enlace de borde &#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [RAS Gateway](ras-gateway/RAS-Gateway.md) 
- [Documentación de rol de servidor de acceso remoto](ras/Remote-Access-Server-Role-Documentation.md)
- [Puerta de enlace RAS para SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [Red privada virtual (VPN)](vpn/vpn-top.md)
 
Para obtener más información acerca de otras tecnologías de red, consulte [a redes en Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).

El rol de servidor de acceso remoto es una agrupación lógica de estas tecnologías de acceso de red relacionados: [Servicio de acceso remoto (RAS)](#bkmk_da), [enrutamiento](#bkmk_rras), y [Proxy de aplicación Web](#bkmk_proxy). Estas tecnologías son los *Servicios de rol* del rol de servidor de acceso remoto. Al instalar el rol de servidor de acceso remoto con el **agregar Roles y características Asistente** o Windows PowerShell, puede instalar uno o varios de estos tres servicios de rol.

>[!IMPORTANT]
>No intente implementar el acceso remoto en una máquina virtual \(VM\) en Microsoft Azure. No se admite el uso de acceso remoto en Microsoft Azure. No se puede usar el acceso remoto en una máquina virtual de Azure para implementar la VPN, DirectAccess o cualquier otra característica de acceso remoto en Windows Server 2016 o versiones anteriores de Windows Server. Para obtener más información, consulte [soporte de software de servidor de Microsoft para las máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="bkmk_da"></a>Servicio de acceso remoto \(RAS\) -puerta de enlace RAS

Al instalar el **DirectAccess y VPN (RAS)** servicio de rol que va a implementar la puerta de enlace de servicio de acceso remoto \( **puerta de enlace RAS**\). Puede implementar la puerta de enlace de RAS de una red privada virtual de puerta de enlace RAS de inquilino único \(VPN\) , un servidor VPN de puerta de enlace RAS para varios inquilinos y como un servidor de DirectAccess.

- **Puerta de enlace RAS - único inquilino**. Mediante el uso de puerta de enlace RAS, puede implementar conexiones VPN para proporcionar a los usuarios finales con acceso remoto a la red y los recursos de su organización. Si los clientes ejecutan Windows 10, puede implementar la VPN de Always On, que mantiene una conexión persistente entre los clientes y la red de su organización, siempre que los equipos remotos están conectados a Internet. Con la puerta de enlace de RAS, también puede crear una conexión VPN de sitio a sitio entre dos servidores en ubicaciones diferentes, como entre la oficina principal y una sucursal y utilizará la traducción de direcciones de red \(NAT\) para que los usuarios dentro de la red puede tener acceso a recursos externos, como Internet. Además, la puerta de enlace RAS admite Border Gateway Protocol (BGP), que proporciona servicios de enrutamiento dinámicos cuando sus oficinas remotas también tienen las puertas de enlace de borde que admiten BGP.

- **Puerta de enlace RAS - Multitenant**. Puede implementar la puerta de enlace de RAS como un enrutador, puerta de enlace de borde basado en software y para varios inquilinos cuando se usa Hyper\-V virtualización de red o tiene redes de VM implementadas con redes de área Local virtual \(VLAN\). Con la puerta de enlace de RAS, proveedores de servicios de nube \(CSP\) y las empresas pueden habilitar el centro de datos y en la nube de enrutamiento de tráfico de red entre redes físicas y virtuales, incluido Internet. Con la puerta de enlace RAS, los inquilinos pueden usar las conexiones VPN de sitio para punto para tener acceso a sus recursos de red de máquina virtual en el centro de datos desde cualquier lugar. También puede proporcionar inquilinos con conexiones VPN de sitio a sitio entre sus sitios remotos y su centro de datos CSP. Además, puede configurar la puerta de enlace de RAS con BGP para el enrutamiento dinámico, y puede habilitar la traducción de direcciones de red \(NAT\) para proporcionar acceso a Internet para las máquinas virtuales en redes de máquina virtual.

>[!IMPORTANT]
> La puerta de enlace de RAS con funciones multiempresa también está disponible en Windows Server 2012 R2.

- **VPN de Always On**. VPN de Always On permite a los usuarios remotos obtener acceso seguro a recursos compartidos, sitios Web de intranet y aplicaciones en una red interna sin necesidad de conectarse a una VPN. 

Para obtener más información, consulte [puerta de enlace RAS](ras-gateway/RAS-Gateway.md) y [Border Gateway Protocol (BGP)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="bkmk_rras"></a>Enrutamiento

Puede usar el acceso remoto para enrutar el tráfico de red entre subredes en la red de área Local. Enrutamiento proporciona compatibilidad para los enrutadores de traducción de direcciones de red (NAT), enrutadores LAN ejecutan BGP, protocolo de información de enrutamiento (RIP) y los enrutadores que admiten multidifusión mediante el protocolo de administración de grupos de Internet (IGMP). Como un enrutador completa, puede implementar en un equipo de servidor o como una máquina virtual (VM) en un equipo que ejecuta Hyper-V RAS.

Para instalar acceso remoto como un enrutador LAN, ya sea usar el Asistente de las características y agregar Roles en el administrador del servidor y seleccione el **acceso remoto** rol de servidor y el **enrutamiento** servicio de rol; o escriba lo siguiente comando en un símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Proxy de aplicación Web

Proxy de aplicación Web es un servicio de rol acceso remoto en Windows Server 2016. Web Application Proxy proporciona funcionalidad de proxy inverso para aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo tener acceso a ellas desde fuera de la red corporativa. Proxy de aplicación Web autentica previamente el acceso a aplicaciones web mediante los servicios de federación de Active Directory (AD FS) y también funciona como un proxy de AD FS.

Para instalar acceso remoto como un Proxy de aplicación Web, ya sea usar el Asistente de las características y agregar Roles en el administrador del servidor y seleccione el **acceso remoto** rol de servidor y el **Proxy de aplicación Web** servicio de rol; o Escriba el siguiente comando en un símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

Para obtener más información, consulte [Proxy de aplicación Web](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).


---