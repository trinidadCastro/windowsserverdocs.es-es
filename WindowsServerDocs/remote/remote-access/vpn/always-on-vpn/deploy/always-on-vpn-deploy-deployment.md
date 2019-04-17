---
title: Implementar VPN de Always On
description: Este tema proporciona instrucciones detalladas para la implementación de VPN siempre activada en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067488"
---
# Implementar VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #0171; [ **Anterior:** más información sobre la VPN siempre activada características avanzadas](always-on-vpn-adv-options.md)<br>
& #0187; [ **Siguiente:** paso 1. Empezar a planificar la implementación de VPN siempre activada](always-on-vpn-deploy-planning.md)

En esta sección, aprenderás sobre el flujo de trabajo para la implementación de las conexiones de VPN siempre activada para los equipos cliente de Windows 10 de unido al dominio remotos. Si quieres **Configurar el acceso condicional** ajustar cómo los usuarios VPN acceder a los recursos, consulta [el acceso condicional para la conectividad VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Para obtener más información sobre el acceso condicional para la conectividad VPN con Azure AD, consulta [el acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 


El siguiente diagrama muestra el proceso de flujo de trabajo para los diferentes escenarios al implementar la VPN siempre activada: 

_Haz clic en la imagen para ampliarla_.

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![miniatura](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>Para esta implementación, no es un requisito de que los servidores de infraestructura, como los equipos que ejecutan Active Directory Domain Services, servicios de certificados de Active Directory y el servidor de directivas de red, se está ejecutando Windows Server 2016. Las versiones anteriores de Windows Server, como Windows Server 2012 R2, puedes usar para los servidores de infraestructura y para el servidor que se está ejecutando el acceso remoto.

## [Paso 1. Planear la implementación de VPN de Always On](always-on-vpn-deploy-planning.md)

En este paso, se iniciará planear y preparar la implementación de VPN siempre activada. Antes de instalar el rol de servidor de acceso remoto en el equipo que tienes previsto sobre el uso de un servidor VPN. Después de una planificación adecuada, puedes implementar VPN siempre activada y, opcionalmente, configurar el acceso condicional para la conectividad VPN con Azure AD.

## [Paso 2. Configurar la infraestructura de servidor de VPN de Always On](vpn-deploy-server-infrastructure.md)

En este paso, instalar y configurar los componentes del lado servidor necesarios para admitir la VPN. Los componentes del lado servidor incluyen la configuración de PKI para distribuir los certificados usados por los usuarios, el servidor VPN y el servidor NPS.  También configurar RRAS para admitir las conexiones IKEv2 y el servidor NPS para realizar la autorización para las conexiones de VPN.

Para configurar la infraestructura de servidor, debe realizar las siguientes tareas:
- **En un servidor configurado con los servicios de dominio de Active Directory:** Habilitar la inscripción automática de certificado en la directiva de grupo para equipos y usuarios, crear el grupo de usuarios de VPN, el grupo de servidores VPN y el grupo de servidores NPS y agregar a miembros a cada grupo.
- **En un servidor de certificados de Active Directory CA:** Crear las plantillas de certificado de autenticación de usuario, autenticación de servidor VPN y autenticación de servidor NPS.
- **En clientes Unidos a un dominio de Windows 10:** Inscribir y validar certificados de usuario.

## [Paso 3. Configurar el servidor de acceso remoto para VPN de Always On](vpn-deploy-ras.md)

En este paso, configuren VPN de acceso remoto para permitir conexiones VPN IKEv2, denegar conexiones desde otros protocolos VPN y asignar un conjunto de direcciones IP estáticas para la emisión de direcciones IP a los clientes VPN autorizados que se conectan.

Para configurar RAS, debe realizar las siguientes tareas:
- Inscribir y validar el certificado del servidor VPN
- Instalar y configurar VPN de acceso remoto

## [Paso 4. Instalar y configurar el servidor NPS](vpn-deploy-nps.md)

En este paso, puedes instalar el servidor de directivas de redes (NPS) mediante Windows PowerShell o el administrador agregar Roles de servidor y Features Wizard. También configuración NPS para controlar todos los derechos de administración de cuentas de solicitud de conexión que recibe desde el servidor VPN, autorización y autenticación.

Para configurar NPS, debe realizar las siguientes tareas:
- Registrar el servidor NPS en Active Directory
- Configurar la administración de cuentas de tu servidor NPS RADIUS
- Agregue el servidor VPN como un cliente RADIUS en NPS
- Configurar la directiva de red de NPS
- Inscribir automáticamente el certificado de servidor NPS

## [Paso 5. Configurar DNS y la configuración de Firewall para siempre en VPN](vpn-deploy-dns-firewall.md)

En este paso, configurar DNS y el Firewall de configuración. Cuando se conectan los clientes VPN remotos, usan los mismos servidores DNS que usan los clientes internos, lo que les permite resolver los nombres de la misma manera que el resto de las estaciones de trabajo internas. 

## [Paso 6. Configurar las conexiones VPN de Always On del cliente de Windows 10](vpn-deploy-client-vpn-connections.md)

En este paso, vas a configurar los equipos de cliente de Windows 10 para comunicarse con los que la infraestructura con una conexión VPN. Puedes usar varias tecnologías para configurar a los clientes de VPN de Windows 10, como Intune, System Center Configuration Manager y Windows PowerShell. Los tres requieren un perfil de VPN de XML para configurar la configuración de VPN adecuada. 

## [Paso 7. (Opcional) Configurar el acceso condicional para la conectividad de VPN](../../ad-ca-vpn-connectivity-windows10.md) 
En este paso opcional, puedes ajustar VPN cómo autorizado a los usuarios acceder a los recursos. Con el acceso condicional de Azure AD para la conectividad de VPN, puede ayudar a proteger las conexiones VPN. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier Azure AD aplicación conectada. Para obtener más información, consulta el [acceso condicional de Azure Active Directory (AD Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).


## Paso siguiente
[Paso 1. Planear la implementación de VPN siempre activada](always-on-vpn-deploy-planning.md): antes de instalar el rol de servidor de acceso remoto en el equipo que tienes previsto sobre el uso de un servidor VPN. Después de una planificación adecuada, puedes implementar VPN siempre activada y, opcionalmente, configurar el acceso condicional para la conectividad VPN con Azure AD.  



---