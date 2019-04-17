---
title: Implementación de VPN de Always On para Windows Server y Windows 10
description: Puedes usar esta implementación para implementar las conexiones siempre en red privada Virtual (VPN) para los empleados remotos mediante el uso de perfiles de VPN de Always On y acceso remoto en Windows Server 2016 o posterior para los equipos cliente de Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: c435f91ef6f3ff5ffd2661291264b939d5ce4e2a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "8981135"
---
# Implementación de VPN de Always On para Windows Server y Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** acceso remoto](../../../Remote-Access.md)<br>
& #187; [ **Siguiente:** más información sobre las características de VPN de Always On y funciones](../../vpn-map-da.md)


VPN de Always On proporciona una solución única y coherente para acceso remoto y admite unido al dominio, sin dominio unido (grupo de trabajo) o AD: dispositivos Unidos a Azure, incluso personales con los dispositivos.  Con la VPN de Always On, el tipo de conexión no tiene que ser exclusivamente usuario o dispositivo, pero puede ser una combinación de ambos. Por ejemplo, podrías permitir una autenticación de dispositivos para la administración de dispositivos remotos y, a continuación, habilitar la autenticación de usuario para la conectividad a los sitios de empresa interna y servicios.



## Requisitos previos

Es muy probable que las tecnologías de implementar que puedes usar para implementar VPN de Always On. Que no sean los servidores DNS/controlador de dominio, la implementación de VPN de Always On requiere un servidor NPS (RADIUS), un servidor de entidad de certificación (CA) y un servidor de acceso remoto (VPN de enrutamiento). Una vez configurada la infraestructura, debe inscribirse a los clientes y, a continuación, conectar a los clientes con sus instalaciones en forma segura a través de varios cambios en la red.

- Dominio infraestructura de Active Directory como uno o varios servidores de sistema de nombres de dominio (DNS). Las zonas de sistema de nombres de dominio (DNS) internos y externos son necesarias, lo que da por hecho que la zona interna es un subdominio delegado de la zona externo (por ejemplo, corp.contoso.com y contoso.com).
- Active Directory-based infraestructura de clave pública (PKI) y los servicios de certificados de Active Directory (AD CS).
- Servidor, virtual o física, nuevo o existente, para instalar el servidor de directivas de redes (NPS). Si ya tienes servidores NPS en la red, puede modificar una configuración del servidor NPS existente en lugar de agregar un nuevo servidor.
- Acceso remoto como un servidor de VPN de puerta de enlace de RAS con un pequeño subconjunto de las características de compatibilidad con conexiones VPN IKEv2 y el enrutamiento de LAN.
- Red perimetral que incluya dos servidores de seguridad.  Asegúrate de que los servidores de seguridad permiten el tráfico que es necesario para las comunicaciones RADIUS y VPN funcione correctamente. Para obtener más información, consulta [Siempre en VPN Introducción a la tecnología](../always-on-vpn-technology-overview.md).
- Servidor físico o máquina virtual (VM) de la red perimetral con dos adaptadores de red Ethernet físicos para instalar el acceso remoto como un servidor VPN de puerta de enlace de RAS. Las máquinas virtuales requieren LAN virtual (VLAN) para el host. 
- La suscripción a los administradores de TI, o equivalente, es la cantidad mínima requerida.
- Leer la sección de programación de esta guía para asegurarse de que está preparado para esta implementación antes de realizar la implementación.
- Revisa a las guías de diseño e implementación para cada una de las tecnologías que use. Estas guías que te ayudarán a determinar si los escenarios de implementación proporcionan los servicios y la configuración que necesitas para la red de la organización. Para obtener más información, consulta [Siempre en VPN Introducción a la tecnología](../always-on-vpn-technology-overview.md).
- Plataforma de administración de tu elección para implementar la configuración de VPN de Always On porque el CSP no es específico del proveedor.


>[!IMPORTANT]
>Para esta implementación, no es un requisito de que los servidores de infraestructura, como los equipos que ejecutan Active Directory Domain Services, servicios de certificados de Active Directory y servidor de directivas de red, se está ejecutando Windows Server 2016. Las versiones anteriores de Windows Server, como Windows Server 2012 R2, puedes usar para los servidores de infraestructura y para el servidor que se está ejecutando el acceso remoto.
>
>No intentes implementar el acceso remoto en una máquina virtual \(VM\) en Microsoft Azure. Uso de acceso remoto en Microsoft Azure no se admite, incluidos DirectAccess y VPN de acceso remoto. Para obtener más información, consulta el [software de servidor de Microsoft que admiten para las máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="bkmk_about"></a>Esta implementación

Las instrucciones proporcionadas guiarán a través de la implementación de acceso remoto como un solo inquilino de puerta de enlace de RAS de VPN para las conexiones de VPN de sitio de to\ de punto, usando cualquiera de los escenarios mencionados a continuación, para los equipos cliente remotos que ejecutan Windows 10. También encontrarás instrucciones para modificar algunas de la infraestructura existente para la implementación. También a lo largo de esta implementación, encontrarás vínculos que te ayudarán a obtener más información sobre el proceso de conexión VPN, servidores para configurar, nodo ProfileXML VPNv2 CSP y otras tecnologías de implementación de VPN de Always On.

**Escenarios de implementación de VPN de Always On:**

1. Implementar de siempre VPN únicamente.
2. Implementar VPN de Always On con acceso condicional para la conectividad VPN con Azure AD.


Para obtener más información y flujo de trabajo de los escenarios presenta, consulta [Implementar VPN de Always On](always-on-vpn-deploy-deployment.md).


## <a name="bkmk_not"></a>Lo que no se proporciona en esta implementación

Esta implementación no proporciona instrucciones para:

- \(AD DS\) de servicios de dominio de Active Directory.
- \(AD CS\) y una infraestructura de clave pública de los servicios de certificados de Active Directory \(PKI\).
- \(DHCP\) de protocolo de configuración dinámica de Host. 
- Hardware, como cableado Ethernet, firewalls, modificadores y hubs de red.
- Recursos de red adicionales, como los servidores de archivos y aplicaciones, que los usuarios remotos pueden acceder a través de una conexión VPN de Always On.
- Conectividad a Internet o el acceso condicional para la conectividad de Internet con Azure AD. Para obtener más información, consulta la [instrucción condicional acceso en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).




## Pasos siguientes

- [Más información sobre las características de VPN de Always On y funciones](../../vpn-map-da.md)

- [Más información sobre las mejoras de VPN de Always On](../always-on-vpn-enhancements.md)

- [Obtén información sobre algunas de las características avanzadas de VPN de Always On](always-on-vpn-adv-options.md)

- [Más información sobre la tecnología de VPN de Always On](../always-on-vpn-technology-overview.md)

- [Empezar a planificar la implementación de VPN de Always On](always-on-vpn-deploy-deployment.md)


---
