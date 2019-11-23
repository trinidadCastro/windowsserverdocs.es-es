---
title: Implementar VPN de Always On
description: En este tema se proporcionan instrucciones detalladas para implementar Always On VPN en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 054a41df281ff9720d381fd4854f34f56ed0307b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388191"
---
# <a name="deploy-always-on-vpn"></a>Implementar VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Más información sobre las características avanzadas de VPN Always On](always-on-vpn-adv-options.md)
- [**Siguiente:** Paso 1. Inicio de la planeación de la Always On la implementación de VPN](always-on-vpn-deploy-planning.md)

En esta sección, aprenderá sobre el flujo de trabajo para implementar Always On conexiones VPN para equipos cliente de Windows 10 Unidos a un dominio remoto. Si desea configurar el **acceso condicional** para ajustar la forma en que los usuarios de VPN acceden a los recursos, consulte [acceso condicional para la conectividad VPN mediante Azure ad](../../ad-ca-vpn-connectivity-windows10.md). Para más información sobre el acceso condicional para la conectividad VPN con Azure AD, consulte [acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 

En el diagrama siguiente se ilustra el proceso de flujo de trabajo para los distintos escenarios al implementar Always On VPN:

![Diagrama de flujo del flujo de trabajo de implementación de VPN Always On](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)

>[!IMPORTANT]
>Para esta implementación, no es necesario que los servidores de infraestructura, como los equipos que ejecutan Active Directory Domain Services, Active Directory servicios de Certificate Server y el servidor de directivas de redes, ejecuten Windows Server 2016. Puede usar versiones anteriores de Windows Server, como Windows Server 2012 R2, para los servidores de infraestructura y para el servidor que ejecuta acceso remoto.

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[Paso 1. Planear la Always On la implementación de VPN](always-on-vpn-deploy-planning.md)

En este paso, comenzará a planear y preparar la implementación de VPN Always On. Antes de instalar el rol de servidor de acceso remoto en el equipo en el que está planeando usar como un servidor VPN. Después de la planeación adecuada, puede implementar Always On VPN y, opcionalmente, configurar el acceso condicional para la conectividad VPN mediante Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[Paso 2. Configurar la infraestructura del servidor VPN Always On](vpn-deploy-server-infrastructure.md)

En este paso, instalará y configurará los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de la PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.  También configura RRAS para admitir conexiones IKEv2 y el servidor NPS para realizar la autorización para las conexiones VPN.

Para configurar la infraestructura de servidor, debe realizar las siguientes tareas:

- **En un servidor configurado con Active Directory Domain Services:** Habilite la inscripción automática de certificados en directiva de grupo para equipos y usuarios, cree el grupo de usuarios de VPN, el grupo de servidores VPN y el grupo de servidores NPS, y agregue miembros a cada grupo.
- **En una entidad de certificación del servidor de certificados Active Directory:** Cree las plantillas de certificado autenticación de usuario, autenticación de servidor VPN y autenticación de servidor NPS.
- **En los clientes de Windows 10 Unidos a un dominio:** Inscribir y validar certificados de usuario.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[Paso 3. Configurar el servidor de acceso remoto para Always On VPN](vpn-deploy-ras.md)

En este paso, configurará VPN de acceso remoto para permitir conexiones VPN IKEv2, denegar conexiones desde otros protocolos VPN y asignar un grupo de direcciones IP estáticas para la emisión de direcciones IP a los clientes VPN autorizados.

Para configurar RAS, debe realizar las siguientes tareas:

- Inscripción y validación del certificado de servidor VPN
- Instalación y configuración de VPN de acceso remoto

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[Paso 4. Instalación y configuración del servidor NPS](vpn-deploy-nps.md)

En este paso, instalará Administrador del servidor el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Asistente para agregar roles y características. También puede configurar NPS para que controle todas las tareas de autenticación, autorización y contabilidad para la solicitud de conexión que recibe desde el servidor VPN.

Para configurar NPS, debe realizar las siguientes tareas:

- Registrar el servidor NPS en Active Directory
- Configuración de cuentas RADIUS para el servidor NPS
- Agregar el servidor VPN como cliente RADIUS en NPS
- Configuración de la Directiva de red en NPS
- Inscripción automática del certificado de servidor NPS

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[Paso 5. Configuración de DNS y firewall para Always On VPN](vpn-deploy-dns-firewall.md)

En este paso, se configuran los valores de DNS y firewall. Cuando los clientes de VPN remotos se conectan, usan los mismos servidores DNS que usan los clientes internos, lo que les permite resolver nombres de la misma manera que el resto de las estaciones de trabajo internas. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[Paso 6. Configuración de conexiones VPN de Always On cliente de Windows 10](vpn-deploy-client-vpn-connections.md)

En este paso, se configuran los equipos cliente de Windows 10 para que se comuniquen con esa infraestructura con una conexión VPN. Puede usar varias tecnologías para configurar los clientes de VPN de Windows 10, como Windows PowerShell, System Center Configuration Manager e Intune. Los tres requieren un perfil de VPN de XML para establecer la configuración de VPN adecuada.

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[Paso 7. Opta Configurar el acceso condicional para la conectividad VPN](../../ad-ca-vpn-connectivity-windows10.md)

En este paso opcional, puede ajustar el modo en que los usuarios de VPN autorizados acceden a los recursos. Con Azure AD acceso condicional para la conectividad VPN, puede ayudar a proteger las conexiones VPN. El acceso condicional es un motor de evaluación basado en directivas que le permite crear reglas de acceso para cualquier aplicación conectada Azure AD. Para obtener más información, consulte [acceso condicional de Azure Active Directory (Azure ad)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-step"></a>Paso siguiente

[Paso 1. Planear la Always On la implementación de VPN](always-on-vpn-deploy-planning.md): antes de instalar el rol de servidor de acceso remoto en el equipo en el que está planeando usar como un servidor VPN. Después de la planeación adecuada, puede implementar Always On VPN y, opcionalmente, configurar el acceso condicional para la conectividad VPN mediante Azure AD.  
