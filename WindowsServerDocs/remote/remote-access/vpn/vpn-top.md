---
title: Redes privadas virtuales (VPN)
description: Puede usar este tema para obtener información sobre las características y la funcionalidad de VPN de Windows Server 2016 y Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fdd409dee5a7a957580daeeb05209336edfd86f6
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822618"
---
# <a name="virtual-private-networking-vpn"></a>Redes privadas virtuales (VPN)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Puerta de enlace RAS como servidor VPN de un solo inquilino

En Windows Server 2016, el rol de servidor de acceso remoto es una agrupación lógica de las siguientes tecnologías de acceso de red relacionadas.

- Servicio de acceso remoto (RAS)
- Enrutamiento
- Proxy de aplicación web

Estas tecnologías son los servicios de rol del rol de servidor de acceso remoto.

Al instalar el rol de servidor de acceso remoto con el Asistente para agregar roles y características o Windows PowerShell, puede instalar uno o varios de estos tres servicios de rol.

Al instalar el servicio de rol **DirectAccess y VPN (RAS)** , está implementando la puerta de enlace de servicio de acceso remoto (**puerta de enlace ras**). Puede implementar la puerta de enlace RAS como un servidor de red privada virtual (VPN) de puerta de enlace RAS de un solo inquilino que proporciona muchas características avanzadas y funcionalidad mejorada.

>[!NOTE]
>También puede implementar la puerta de enlace RAS como un servidor VPN multiinquilino para usarlo con redes definidas por software (SDN) o como un servidor de DirectAccess. Para obtener más información, consulte [puerta de enlace ras](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [redes definidas por software (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)y [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Temas relacionados
- [Always on funcionalidades y características de VPN](vpn-map-da.md): en este tema, obtendrá información sobre las características y la funcionalidad de Always on VPN. 

- [Configurar túneles de dispositivo VPN en Windows 10](vpn-device-tunnel-config.md): always on VPN le ofrece la capacidad de crear un perfil de VPN dedicado para el dispositivo o la máquina. Always On conexiones VPN incluyen dos tipos de túneles: _túnel de dispositivo_ y _túnel de usuario_. El túnel de dispositivo se usa para los escenarios de conectividad previa al inicio de sesión y para la administración de dispositivos. El túnel de usuario permite a los usuarios tener acceso a los recursos de la organización a través de servidores VPN.

- [Always on la implementación de VPN para Windows Server 2016 y Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): proporciona instrucciones sobre cómo implementar el acceso remoto como una puerta de enlace ras de VPN de un solo inquilino para conexiones VPN de punto a sitio que permiten a los empleados remotos conectarse a la red de la organización con conexiones VPN Always on. Se recomienda que revise las guías de diseño e implementación de cada una de las tecnologías que se usan en esta implementación.

- [Guía técnica de VPN de Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): le guiará a través de las decisiones que realizará para los clientes de Windows 10 en la solución VPN de la empresa y sobre cómo configurar la implementación. Puede buscar referencias al proveedor de servicios de configuración de VPNv2 (CSP) y proporciona instrucciones de configuración de administración de dispositivos móviles (MDM) mediante Microsoft Intune y la plantilla de Perfil de VPN para Windows 10.

- [Cómo crear perfiles de VPN en Configuration Manager](https://docs.microsoft.com/configmgr/protect/deploy-use/create-vpn-profiles): en este tema, aprenderá a crear perfiles de vpn en Configuration Manager.

- [Configuración de conexiones VPN de Always on cliente de Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): en este tema se describen las opciones y el esquema de ProfileXML y cómo crear la VPN ProfileXML. Después de configurar la infraestructura de servidor, debe configurar los equipos cliente de Windows 10 para que se comuniquen con esa infraestructura con una conexión VPN.

- [Opciones de Perfil de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): en este tema se describe la configuración del perfil de VPN en Windows 10 y se explica cómo configurar perfiles de VPN mediante Intune o Configuration Manager. Puede configurar todas las opciones de VPN en Windows 10 mediante el nodo ProfileXML en el CSP VPNv2.
