---
title: Implementar VPN de Always On
description: Este tema proporciona instrucciones detalladas para la implementación de VPN de Always On en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867146"
---
# <a name="deploy-always-on-vpn"></a>Implementar VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#0171;[ **Anterior:** Obtenga información acerca de la VPN de Always On características avanzadas](always-on-vpn-adv-options.md)<br>
&#0187;[ **Siguiente:** Paso 1. Comience a planear la implementación de VPN de Always On](always-on-vpn-deploy-planning.md)

En esta sección, obtendrá información sobre el flujo de trabajo para implementar conexiones VPN de Always On para remotas equipos cliente Windows 10 unido al dominio. Si desea **configurar el acceso condicional** para ajustar cómo acceder a los usuarios VPN a los recursos, consulte [acceso condicional para la conectividad VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Para más información sobre el acceso condicional para la conectividad VPN con Azure AD, consulte [acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 


El siguiente diagrama ilustra el proceso de flujo de trabajo de los distintos escenarios al implementar la VPN de Always On: 

_Haga clic en la imagen para ampliarla_.

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![thumbnail](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>Para esta implementación, no es un requisito que sus servidores de infraestructura, como los equipos que ejecutan servicios de dominio de Active Directory, servicios de certificados de Active Directory y servidor de directivas de red, se está ejecutando Windows Server 2016. Puede usar las versiones anteriores de Windows Server, como Windows Server 2012 R2, para los servidores de infraestructura y para el servidor que ejecuta el acceso remoto.

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[Paso 1. Planear la implementación de VPN de Always On](always-on-vpn-deploy-planning.md)

En este paso, primero planear y preparar la implementación de VPN de Always On. Antes de instalar el rol de servidor de acceso remoto en el equipo que tiene previsto usar como un servidor VPN. Después de la planeación adecuada, puede implementar la VPN de Always On y, opcionalmente, configure el acceso condicional para la conectividad VPN con Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[Paso 2. Configurar la infraestructura de servidor VPN siempre activa](vpn-deploy-server-infrastructure.md)

En este paso, instale y configure los componentes de servidor necesarios para admitir la VPN. Los componentes de servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.  También configurar RRAS para admitir las conexiones IKEv2 y el servidor NPS para realizar la autorización para las conexiones VPN.

Para configurar la infraestructura de servidor, debe realizar las siguientes tareas:
- **En un servidor configurado con los servicios de dominio de Active Directory:** Habilitar la inscripción automática de certificados en la directiva de grupo para equipos y usuarios, crear el grupo de usuarios de VPN, el grupo de servidores VPN y el grupo de servidores NPS y agregar a miembros a cada grupo.
- **En un servidor de certificados CA de Active Directory:** Crear las plantillas de certificado de autenticación de usuario, autenticación de servidor VPN y autenticación del servidor NPS.
- **En los clientes de Windows 10 Unidos a dominio:** Inscribir y validar los certificados de usuario.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[Paso 3. Configurar el servidor de acceso remoto para siempre en VPN](vpn-deploy-ras.md)

En este paso, configure una VPN de acceso remoto para permitir conexiones VPN de IKEv2, denegar las conexiones desde otros protocolos VPN y asignar un grupo de direcciones IP estáticas para la emisión de direcciones IP a la conexión de los clientes VPN autorizados.

Para configurar RAS, debe realizar las siguientes tareas:
- Inscribir y validar el certificado del servidor VPN
- Instalar y configurar el acceso remoto VPN

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[Paso 4. Instalar y configurar el servidor NPS](vpn-deploy-nps.md)

En este paso, se instala el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Server Manager agregar Roles y características Asistente. También configura NPS para controlar todos los de autenticación, autorización y los derechos de administración de cuentas de solicitud de conexión que recibe desde el servidor VPN.

Para configurar NPS, debe realizar las siguientes tareas:
- Registrar el servidor NPS en Active Directory
- Configurar cuentas para el servidor NPS RADIUS
- Agregar el servidor VPN como cliente RADIUS en NPS
- Configurar la directiva de red en NPS
- Inscribir automáticamente el certificado de servidor NPS

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[Paso 5. Configuración de DNS y configuración de Firewall para AlwaysOn VPN](vpn-deploy-dns-firewall.md)

En este paso, configurará DNS y servidor de seguridad de configuración. Cuando se conectan los clientes VPN remotos, usan los mismos servidores DNS que usan los clientes internos, lo que les permite resolver nombres en la misma manera que el resto de las estaciones de trabajo internos. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[Paso 6. Configuración de cliente Windows 10 siempre en las conexiones VPN](vpn-deploy-client-vpn-connections.md)

En este paso, configure los equipos cliente de Windows 10 para comunicarse con esa infraestructura con una conexión VPN. Puede usar varias tecnologías para configurar a los clientes de VPN de Windows 10, incluidos Windows PowerShell, System Center Configuration Manager e Intune. Las tres requieren un perfil de VPN de XML para la configuración de VPN adecuado. 

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[Paso 7. (Opcional) Configurar el acceso condicional para la conectividad VPN](../../ad-ca-vpn-connectivity-windows10.md) 
En este paso opcional, puede ajustar cómo autorizado VPN a los usuarios acceder a los recursos. Con acceso condicional de Azure AD para la conectividad VPN, puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basada en directivas que le permite crear reglas de acceso para cualquier AD Azure aplicación conectada. Para obtener más información, consulte [acceso condicional de Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).


## <a name="next-step"></a>Paso siguiente
[Paso 1. Planear la implementación de VPN de Always On](always-on-vpn-deploy-planning.md): Antes de instalar el rol de servidor de acceso remoto en el equipo que tiene previsto usar como un servidor VPN. Después de la planeación adecuada, puede implementar la VPN de Always On y, opcionalmente, configure el acceso condicional para la conectividad VPN con Azure AD.  



---