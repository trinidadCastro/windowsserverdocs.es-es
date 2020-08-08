---
title: Red privada virtual (VPN)
description: Puede usar este tema para obtener información sobre las características y la funcionalidad de VPN de Windows Server 2016 y Windows 10.
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.openlocfilehash: 30f08f02bf7a06619b9a32206863a9ddefc0fc2d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87968922"
---
# <a name="virtual-private-networking-vpn"></a>Red privada virtual (VPN)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Puerta de enlace RAS como servidor VPN de un solo inquilino

En Windows Server 2016, el rol de servidor de acceso remoto es una agrupación lógica de las siguientes tecnologías de acceso de red relacionadas.

- Servicio de acceso remoto (RAS)
- Enrutamiento
- Proxy de aplicación web

Estas tecnologías son los Servicios de rol del rol de servidor de acceso remoto.

Al instalar el rol de servidor de acceso remoto con el Asistente para agregar roles y características o Windows PowerShell, puede instalar uno o varios de estos tres servicios de rol.

Al instalar el servicio de rol **DirectAccess y VPN (RAS)** , está implementando la puerta de enlace de servicio de acceso remoto (**puerta de enlace ras**). Puede implementar la puerta de enlace RAS como un servidor de red privada virtual (VPN) de puerta de enlace RAS de un solo inquilino que proporciona muchas características avanzadas y funcionalidad mejorada.

>[!NOTE]
>También puede implementar la puerta de enlace RAS como un servidor VPN multiinquilino para usarlo con redes definidas por software (SDN) o como un servidor de DirectAccess. Para obtener más información, consulte [puerta de enlace ras](../ras-gateway/ras-gateway.md), [redes definidas por software (SDN)](../../../networking/sdn/software-defined-networking.md)y [DirectAccess](../directaccess/directaccess.md).

## <a name="related-topics"></a>Temas relacionados
- [Always on funcionalidades y características de VPN](vpn-map-da.md): en este tema, obtendrá información sobre las características y la funcionalidad de Always on VPN.

- [Configurar túneles de dispositivo VPN en Windows 10](vpn-device-tunnel-config.md): always on VPN le ofrece la capacidad de crear un perfil de VPN dedicado para el dispositivo o la máquina. Always On conexiones VPN incluyen dos tipos de túneles: _túnel de dispositivo_ y _túnel de usuario_. El túnel de dispositivo se usa para los escenarios de conectividad previa al inicio de sesión y para la administración de dispositivos. Los túneles de usuario permiten a los usuarios acceder a los recursos de la organización utilizando servidores VPN.

- [Always on la implementación de VPN para Windows Server 2016 y Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): proporciona instrucciones sobre cómo implementar el acceso remoto como una puerta de enlace ras de VPN de un solo inquilino para conexiones VPN de punto a sitio que permiten a los empleados remotos conectarse a la red de la organización con conexiones VPN Always on. Se recomienda que revise las guías de diseño e implementación de cada una de las tecnologías que se usan en esta implementación.

- [Guía técnica de VPN de Windows 10](/windows/access-protection/vpn/vpn-guide): le guiará a través de las decisiones que realizará para los clientes de Windows 10 en la solución VPN de la empresa y sobre cómo configurar la implementación. Puede buscar referencias al proveedor de servicios de configuración de VPNv2 (CSP) y proporciona instrucciones de configuración de administración de dispositivos móviles (MDM) mediante Microsoft Intune y la plantilla de Perfil de VPN para Windows 10.

- [Cómo crear perfiles de VPN en Configuration Manager](/configmgr/protect/deploy-use/create-vpn-profiles): en este tema, aprenderá a crear perfiles de vpn en Configuration Manager.

- [Configuración de conexiones VPN de Always on cliente de Windows 10](./always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md): en este tema se describen las opciones y el esquema de ProfileXML y cómo crear la VPN ProfileXML. Después de configurar la infraestructura de servidor, debe configurar los equipos cliente de Windows 10 para que se comuniquen con esa infraestructura con una conexión VPN.

- [Opciones de Perfil de VPN](/windows/access-protection/vpn/vpn-profile-options): en este tema se describe la configuración del perfil de VPN en Windows 10 y se explica cómo configurar perfiles de VPN mediante Intune o Configuration Manager. Puede configurar todas las opciones de VPN en Windows 10 mediante el nodo ProfileXML en el CSP VPNv2.
