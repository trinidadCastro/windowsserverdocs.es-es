---
title: Implementación de VPN de Always On para Windows Server y Windows 10
description: Puede usar esta implementación para implementar Always On conexiones de red privada virtual (VPN) para empleados remotos mediante el acceso remoto en Windows Server 2016 o posterior y Always On perfiles de VPN para equipos cliente de Windows 10.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ca064f887a524c5f41b29837e8f8fec586a8d928
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313265"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Implementación de Always On VPN para Windows Server y Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Acceso remoto](../../../Remote-Access.md)<br>
- [**Siguiente:** Más información sobre las características y funcionalidades de VPN Always On](../../vpn-map-da.md)

Always On VPN proporciona una solución única y coherente para el acceso remoto y admite dispositivos Unidos a un dominio, que no están Unidos a un dominio (grupo de trabajo) o que están Unidos a Azure AD, incluso a los dispositivos de propiedad personal. Con VPN de Always On, el tipo de conexión no tiene que ser exclusivamente de usuario o de dispositivo, sino que puede ser una combinación de ambos. Por ejemplo, podría habilitar la autenticación de dispositivos para la administración de dispositivos remotos y, a continuación, habilitar la autenticación de usuarios para la conectividad a servicios y sitios internos de la empresa.

## <a name="prerequisites"></a>Requisitos previos

Lo más probable es que tenga implementadas las tecnologías que puede usar para implementar Always On VPN. Aparte de los servidores DC/DNS, la implementación de VPN Always On requiere un servidor NPS (RADIUS), un servidor de entidad de certificación (CA) y un servidor de acceso remoto (enrutamiento/VPN). Una vez configurada la infraestructura, debe inscribir a los clientes y, a continuación, conectar los clientes a su entorno local de forma segura a través de varios cambios de red.

- Active Directory infraestructura de dominio, incluidos uno o más servidores de sistema de nombres de dominio (DNS). Se requieren zonas de sistema de nombres de dominio (DNS) internas y externas, lo que supone que la zona interna es un subdominio delegado de la zona externa (por ejemplo, corp.contoso.com y contoso.com).
- Infraestructura de clave pública (PKI) basada en Active Directory y servicios de certificados de Active Directory (AD CS).
- Servidor, ya sea virtual o físico, existente o nuevo, para instalar el servidor de directivas de redes (NPS). Si ya tiene servidores NPS en la red, puede modificar una configuración de servidor NPS existente en lugar de agregar un nuevo servidor.
- Acceso remoto como un servidor VPN de puerta de enlace RAS con un pequeño subconjunto de características que admiten conexiones VPN de IKEv2 y enrutamiento de LAN.
- Red perimetral que incluye dos firewalls.  Asegúrese de que los firewalls permiten que el tráfico que es necesario para que las comunicaciones VPN y RADIUS funcionen correctamente. Para obtener más información, consulte [Always on información general sobre la tecnología VPN](../always-on-vpn-technology-overview.md).
- Servidor físico o máquina virtual (VM) de la red perimetral con dos adaptadores de red Ethernet físicos para instalar el acceso remoto como un servidor VPN de puerta de enlace de RAS. Las máquinas virtuales requieren LAN virtual (VLAN) para el host. 
- La pertenencia al grupo administradores, o equivalente, es lo mínimo necesario.
- Lea la sección planeación de esta guía para asegurarse de que está preparado para esta implementación antes de realizar la implementación.
- Revise las guías de diseño e implementación de cada una de las tecnologías utilizadas. Estas guías pueden ayudarle a determinar si los escenarios de implementación proporcionan los servicios y la configuración que necesita para la red de su organización. Para obtener más información, consulte [Always on información general sobre la tecnología VPN](../always-on-vpn-technology-overview.md).
- Plataforma de administración de su elección para implementar la Always On configuración de VPN porque el CSP no es específico del proveedor.

>[!IMPORTANT]
>Para esta implementación, no es necesario que los servidores de infraestructura, como los equipos que ejecutan Active Directory Domain Services, Active Directory servicios de Certificate Server y el servidor de directivas de redes, ejecuten Windows Server 2016. Puede usar versiones anteriores de Windows Server, como Windows Server 2012 R2, para los servidores de infraestructura y para el servidor que ejecuta acceso remoto.
>
>No intente implementar el acceso remoto en una máquina virtual (VM) en Microsoft Azure. No se admite el uso de acceso remoto en Microsoft Azure, como la VPN de acceso remoto y DirectAccess. Para obtener más información, vea [compatibilidad de software de servidor de Microsoft con máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="about-this-deployment"></a>Acerca de esta implementación

Las instrucciones proporcionadas le guiarán a través de la implementación de acceso remoto como una puerta de enlace RAS de VPN de un solo inquilino para las conexiones VPN de punto a sitio, mediante cualquiera de los escenarios que se mencionan a continuación, para equipos cliente remotos que ejecutan Windows 10. También encontrará instrucciones para modificar parte de la infraestructura existente para la implementación. Además, en esta implementación, encontrará vínculos que le ayudarán a obtener más información sobre el proceso de conexión VPN, los servidores que se van a configurar, el nodo ProfileXML VPNv2 CSP y otras tecnologías para implementar Always On VPN.

**Always On escenarios de implementación de VPN:**

1. Implemente Always On solo VPN.
2. Implemente Always On VPN con acceso condicional para la conectividad VPN mediante Azure AD.

Para obtener más información y el flujo de trabajo de los escenarios presentados, consulte [implementar Always on VPN](always-on-vpn-deploy-deployment.md).

## <a name="what-isnt-provided-in-this-deployment"></a>Lo que no se proporciona en esta implementación

Esta implementación no proporciona instrucciones para:

- Active Directory Domain Services (AD DS).
- Active Directory servicios de Certificate Server (AD CS) y una infraestructura de clave pública (PKI).
- Protocolo de configuración dinámica de host (DHCP).
- Hardware de red, como cableado Ethernet, firewalls, conmutadores y concentradores.
- Recursos de red adicionales, como servidores de archivos y aplicaciones, a los que los usuarios remotos pueden tener acceso a través de una conexión VPN Always On.
- Conectividad a Internet o acceso condicional para la conectividad a Internet mediante Azure AD. Para obtener más información, consulte [acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-steps"></a>Pasos siguientes

- [Más información acerca de las características y funcionalidades de VPN Always On](../../vpn-map-da.md)

- [Más información acerca de las mejoras de VPN de Always On](../always-on-vpn-enhancements.md)

- [Obtenga información acerca de algunas de las características avanzadas de VPN Always On](always-on-vpn-adv-options.md)

- [Más información acerca de la tecnología de VPN de Always On](../always-on-vpn-technology-overview.md)

- [Inicio de la planeación de la implementación de VPN Always On](always-on-vpn-deploy-deployment.md)