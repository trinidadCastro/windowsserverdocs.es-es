---
title: Redes privadas virtuales (VPN)
description: Puede utilizar este tema para obtener información sobre la funcionalidad y las características de Windows Server 2016 y VPN de Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bfd00b7a13e9fad113da1191e7ccd33965223070
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749555"
---
# <a name="virtual-private-networking-vpn"></a>Redes privadas virtuales (VPN)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Puerta de enlace RAS como un servidor VPN de inquilino único

En Windows Server 2016, el rol de servidor de acceso remoto es una agrupación lógica de las siguientes tecnologías de acceso de red relacionados.

- Servicio de acceso remoto (RAS)
- Enrutamiento
- Proxy de aplicación web

Estas tecnologías son los servicios de rol del rol de servidor de acceso remoto.

Cuando se instala el rol de servidor de acceso remoto con el agregar Roles y características Asistente o Windows PowerShell, puede instalar uno o varios de estos tres servicios de rol.

Al instalar el **DirectAccess y VPN (RAS)** servicio de rol que va a implementar la puerta de enlace de servicio de acceso remoto (**puerta de enlace RAS**). Puede implementar la puerta de enlace de RAS como un servidor de red privada virtual (VPN) de puerta de enlace RAS de inquilino único que proporciona muchas características avanzadas y una mejor funcionalidad.

>[!NOTE]
>También puede implementar la puerta de enlace de RAS como un servidor VPN para varios inquilinos para su uso con Software Defined Networking (SDN) o como un servidor de DirectAccess. Para obtener más información, consulte [puerta de enlace RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [redes definidas por Software (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking), y [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Temas relacionados
- [Características de VPN de Always On y la funcionalidad](vpn-map-da.md): En este tema, obtendrá información sobre las características y funcionalidad de VPN de Always On. 

- [Configurar dispositivo túneles VPN en Windows 10](vpn-device-tunnel-config.md): VPN de Always On le ofrece la capacidad para crear un perfil de VPN dedicado para el dispositivo o equipo. Las conexiones VPN de Always On incluyen dos tipos de túneles: _túnel dispositivo_ y _túnel usuario_. Túnel de dispositivo se usa para escenarios de conectividad de inicio de sesión previo y fines de administración de dispositivos. Túnel de usuario permite a los usuarios tener acceso a recursos de la organización a través de los servidores VPN.

- [Siempre en la implementación de VPN para Windows Server 2016 y Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): Proporciona instrucciones sobre la implementación de acceso remoto como un único inquilino de puerta de enlace de RAS de VPN para conexiones VPN de punto a sitio que permiten a los empleados remotos para conectarse a la red de su organización con conexiones VPN de Always On. Se recomienda que revise a las guías de diseño e implementación para cada una de las tecnologías que se usan en esta implementación.

- [Guía técnica de VPN de Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Le guía a través de las decisiones que tomará para que clientes de Windows 10 en su solución de VPN de la empresa y cómo configurar la implementación. Puede buscar referencias para el proveedor de servicios de configuración (CSP) de VPNv2 y proporciona administración de dispositivos móviles en las instrucciones de configuración (MDM) con Microsoft Intune y la plantilla de perfil de VPN para Windows 10.

- [Cómo crear perfiles de VPN en System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): En este tema, aprenderá a crear perfiles de VPN en System Center Configuration Manager (SCCM).

- [Configuración de cliente Windows 10 siempre en las conexiones VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Este tema describe el ProfileXML opciones de esquema y cómo crear la VPN ProfileXML. Después de configurar la infraestructura de servidor, debe configurar los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN.

- [Opciones de perfil de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): En este tema se describe la configuración de perfil VPN en Windows 10 y obtenga información sobre cómo configurar perfiles de VPN mediante Intune o SCCM. Puede configurar todas las configuraciones de VPN en Windows 10 mediante el nodo ProfileXML en el CSP de VPNv2.
