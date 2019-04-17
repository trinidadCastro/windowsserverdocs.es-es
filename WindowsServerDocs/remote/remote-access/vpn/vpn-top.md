---
title: Redes privadas virtuales (VPN)
description: Puedes usar este tema para obtener información sobre la funcionalidad y las características de Windows Server 2016 y VPN de Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067476"
---
# \(VPN\) de red privada virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

## Puerta de enlace RAS como un servidor VPN solo inquilino

En Windows Server 2016, el rol de servidor de acceso remoto es una agrupación lógica de las siguientes tecnologías de acceso de red relacionado.

- Servicio de acceso remoto (RAS)
- Enrutamiento
- Proxy de aplicación web

Estas tecnologías son los servicios de rol del rol de servidor de acceso remoto.

Al instalar el rol de servidor de acceso remoto con el agregar Roles y características Asistente o Windows PowerShell, puedes instalar uno o varios de estos tres servicios de rol.

Al instalar el servicio de rol de **DirectAccess y VPN (RAS)** , vas a implementar la puerta de enlace de servicio de acceso remoto \ (**Puerta de enlace RAS**\). Para implementar la puerta de enlace RAS como un servidor \(VPN\) de red privada virtual de puerta de enlace RAS solo inquilino que proporciona muchas características avanzadas y funciones mejoradas.

>[!NOTE]
>También puedes implementar la puerta de enlace RAS como un servidor VPN anfitrión para su uso con redes definidas por Software \(SDN\) o como un servidor de DirectAccess. Para obtener más información, consulta la [Puerta de enlace RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [Redes definidas por Software (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)y [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## Temas relacionados
- [Características y funcionalidades de VPN siempre activada](vpn-map-da.md): en este tema, aprenderás sobre las características y funcionalidades de VPN siempre activada. 

- [Configurar túneles de dispositivo de VPN en Windows 10](vpn-device-tunnel-config.md): VPN siempre activada te ofrece la capacidad para crear un perfil de VPN dedicado para el dispositivo o máquina. Las conexiones de VPN siempre activada incluyen dos tipos de túneles: _túnel de dispositivo_ y _túnel de usuario_. Túnel de dispositivo se usa para escenarios de conectividad de inicio de sesión y con fines de administración de dispositivos. Túnel de usuario permite a los usuarios acceso a los recursos de la organización a través de servidores VPN.

- [Siempre en implementación de VPN para Windows Server 2016 y Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): proporciona instrucciones sobre cómo implementar el acceso remoto como un solo inquilino puerta de enlace de RAS de VPN para las conexiones de VPN de sitio de to\ de punto que permiten a los empleados remotos conectarse a la organización red con conexiones de VPN siempre activada. Se recomienda que revises a las guías de diseño e implementación para cada una de las tecnologías que se usan en esta implementación.

- [Guía técnica de VPN de Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): te guía a través de las decisiones que tomarás para los clientes de Windows 10 en la solución VPN de la empresa y cómo configurar la implementación. Puedes encontrar referencias para el proveedor de servicios de configuración (CSP) VPNv2 y proporcionan administración de dispositivos móviles de las instrucciones de configuración (MDM) mediante Microsoft Intune y la plantilla de perfil de VPN para Windows 10.

- [Cómo crear VPN perfiles en System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): en este tema, aprenderás a crear perfiles de VPN en System Center Configuration Manager (SCCM).

- [Configurar Windows 10 siempre en conexiones VPN de clientes](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): en este tema se describe la ProfileXML opciones y esquema y cómo crear la VPN ProfileXML. Después de configurar la infraestructura de servidor, debes configurar los equipos de cliente de Windows 10 para comunicarse con los que la infraestructura con una conexión VPN. 

- [Opciones de perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): en este tema se describe la configuración de perfil VPN en Windows 10 y aprende a configurar perfiles de VPN con Intune o SCCM. Puedes configurar toda la configuración de VPN en Windows 10 usando el nodo ProfileXML en el CSP de VPNv2.

---