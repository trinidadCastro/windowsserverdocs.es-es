---
title: Implementación de VPN de Always On para Windows Server y Windows 10
description: Puede usar esta implementación para implementar conexiones siempre en red privada Virtual (VPN) para los empleados remotos mediante el acceso remoto en Windows Server 2016 o posterior y perfiles de VPN de Always On para los equipos cliente de Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 533f0273f6802be209ae5ad79b57f46dd6775149
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749471"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Implementación de VPN de Always On para Windows Server y Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Acceso remoto](../../../Remote-Access.md)<br>
- [**Siguiente:** Obtenga información sobre las funciones y características de VPN de Always On](../../vpn-map-da.md)

VPN de Always On proporciona una solución única y coherente para el acceso remoto y admite unido al dominio, unido a permaneciendo (grupo de trabajo) o AD – dispositivos Unidos a Azure, dispositivos de propiedad incluso personal. Con VPN de Always On, el tipo de conexión no tiene que ser exclusivamente de usuario o de dispositivo, sino que puede ser una combinación de ambos. Por ejemplo, podría habilitar la autenticación de dispositivos para la administración de dispositivos remotos y, a continuación, habilitar la autenticación de usuarios para la conectividad a servicios y sitios internos de la empresa.

## <a name="prerequisites"></a>Requisitos previos

Es muy probable que las tecnologías implementadas que puede usar para implementar la VPN de Always On. Distinto de los servidores de DC/DNS, la implementación de VPN de Always On requiere un servidor NPS (RADIUS), un servidor de la entidad de certificación (CA) y un servidor de acceso remoto (VPN de enrutamiento). Una vez configurada la infraestructura, debe inscribir a clientes y, a continuación, conectar a los clientes a sus instalaciones en forma segura a través de varios cambios de red.

- Dominio infraestructura de Active Directory como uno o más servidores de sistema de nombres de dominio (DNS). Zonas del sistema de nombres de dominio (DNS) internos y externos son necesarias, que se da por supuesto que la zona interna es un subdominio delegado de la zona externa (por ejemplo, corp.contoso.com y contoso.com).
- Active Directory de infraestructura de clave pública (PKI) y los servicios de certificados de Active Directory (AD CS).
- Server, virtual o físico, nuevo o existente, para instalar el servidor de directivas de redes (NPS). Si ya tiene servidores NPS de la red, puede modificar una configuración de servidor existente de NPS en lugar de agregar un nuevo servidor.
- Acceso remoto como un servidor VPN de puerta de enlace de RAS con un pequeño subconjunto de las características que admiten las conexiones VPN de IKEv2 y enrutamiento LAN.
- Red perimetral que incluye dos firewalls.  Asegúrese de que los firewalls permitan el tráfico que es necesario para las comunicaciones de VPN y RADIUS funcionar correctamente. Para obtener más información, consulte [siempre en tecnología de información general sobre VPN](../always-on-vpn-technology-overview.md).
- Servidor físico o máquina virtual (VM) en la red perimetral con dos adaptadores de red Ethernet físicos para instalar acceso remoto como un servidor VPN de puerta de enlace de RAS. Las máquinas virtuales requieren LAN virtual (VLAN) para el host. 
- Pertenencia al grupo Administradores o equivalente, es lo mínimo necesario.
- Lea la sección de planeación de esta guía para asegurarse de que estén preparados para esta implementación antes de realizar la implementación.
- Revise a las guías de diseño e implementación para cada una de las tecnologías utilizadas. Estas guías pueden ayudarle a determinar si los escenarios de implementación proporcionan los servicios y la configuración que necesita para la red de su organización. Para obtener más información, consulte [siempre en tecnología de información general sobre VPN](../always-on-vpn-technology-overview.md).
- Plataforma de administración de su elección para la implementación de la configuración de VPN de Always On porque el CSP no es específico del proveedor.

>[!IMPORTANT]
>Para esta implementación, no es un requisito que sus servidores de infraestructura, como los equipos que ejecutan servicios de dominio de Active Directory, servicios de certificados de Active Directory y servidor de directivas de red, se está ejecutando Windows Server 2016. Puede usar las versiones anteriores de Windows Server, como Windows Server 2012 R2, para los servidores de infraestructura y para el servidor que ejecuta el acceso remoto.
>
>No intente implementar el acceso remoto en una máquina virtual (VM) de Microsoft Azure. Usar el acceso remoto en Microsoft Azure no se admite, incluido el acceso remoto VPN y DirectAccess. Para obtener más información, consulte [soporte de software de servidor de Microsoft para las máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="about-this-deployment"></a>Acerca de esta implementación

Las instrucciones proporcionadas le guiarán a través de la implementación de acceso remoto como un único inquilino puerta de enlace de RAS de VPN para conexiones VPN de punto a sitio, mediante cualquiera de los escenarios que se mencionan a continuación, para los equipos cliente remotos que ejecutan Windows 10. También encontrará instrucciones para modificar algunas de la infraestructura existente para la implementación. También a lo largo de esta implementación, encontrará vínculos que le ayudarán a obtener más información sobre el proceso de conexión de VPN, servidores para configurar, nodo de CSP de VPNv2 ProfileXML y otras tecnologías de implementación de VPN de Always On.

**Escenarios de implementación de VPN de Always On:**

1. Siempre en VPN solo implementar.
2. Implementación siempre VPN con acceso condicional para la conectividad VPN con Azure AD.

Para obtener más información y flujo de trabajo de los escenarios presentados, consulte [implementar VPN de Always On](always-on-vpn-deploy-deployment.md).

## <a name="what-isnt-provided-in-this-deployment"></a>Lo que no se proporciona en esta implementación

Esta implementación no proporciona instrucciones para:

- Servicios de dominio de Active Directory (AD DS).
- Servicios de certificados de Active Directory (AD CS) y una infraestructura de clave pública (PKI).
- Protocolo de configuración dinámica de Host (DHCP).
- Hardware, como Ethernet cableado, firewalls, conmutadores y concentradores de red.
- Recursos de red adicionales, como los servidores de archivos y las aplicaciones, que pueden tener acceso los usuarios remotos a través de una conexión VPN de Always On.
- Conectividad a Internet o acceso condicional para la conectividad a Internet mediante Azure AD. Para obtener más información, consulte [acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre las funciones y características de VPN de Always On](../../vpn-map-da.md)

- [Más información sobre las mejoras de VPN de Always On](../always-on-vpn-enhancements.md)

- [Obtenga información sobre algunas de las características avanzadas de VPN de Always On](always-on-vpn-adv-options.md)

- [Más información sobre la tecnología de VPN de Always On](../always-on-vpn-technology-overview.md)

- [Comience a planear la implementación de VPN de Always On](always-on-vpn-deploy-deployment.md)