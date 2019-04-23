---
title: Funciones de VPN de Always On
description: En este tema, obtendrá información sobre las características y funcionalidad de VPN de Always On.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874816"
---
# <a name="always-on-vpn-features-and-functionalities"></a>Funcionalidades y características de VPN de always On

>Se aplica a: Windows Server \(canal semianual\), Windows Server 2016 y Windows 10

&#171;  [**Anterior:** Siempre en la implementación de VPN para Windows Server y Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
&#187;[ **Siguiente:** Obtenga información sobre las mejoras de VPN de Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

En este tema, obtendrá información sobre las características y funcionalidades de VPN de Always On.  En la tabla siguiente no es una lista exhaustiva, sin embargo, incluye algunas de las características y funcionalidades que se usa en soluciones de acceso remoto más comunes. 

>[!TIP]
>Si actualmente usa DirectAccess, recomendamos que investigue la funcionalidad de VPN de Always On con cuidado para determinar si direcciona que todas sus necesidades de acceso remoto antes de migrar de DirectAccess para VPN de Always On de formulario.  

| Área funcional | VPN de Always On  |
| ---- | ---- |
| Conectividad perfecta y transparente a la red corporativa. | Puede configurar VPN de Always On para admitir el desencadenamiento automático basándose en las solicitudes de resolución de lanzamiento o espacio de nombres de aplicación.<p><p>Definir mediante:<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/DomainNameInformationList/AutoTrigger** |
| Uso de un túnel de infraestructura dedicada para proporcionar conectividad a usuarios que no ha iniciado sesión en la red corporativa. | Esta funcionalidad se puede lograr mediante la característica de túnel de dispositivo en el perfil de VPN.<p><p>_**Tenga en cuenta.**_<br>Túnel de dispositivo solo puede configurarse en dispositivos Unidos a dominio con IKEv2 con autenticación de certificados de equipo.<p><p>Definir mediante:<br>**VPNv2/ProfileName/DeviceTunnel** |
| Uso de administración de salida para permitir la conectividad remota a los clientes de sistemas de administración que se encuentra en la red corporativa. | Esta funcionalidad se puede lograr mediante la característica de túnel de dispositivo en el perfil VPN que se combina con la configuración de la conexión VPN registre dinámicamente las direcciones IP asignadas a la interfaz VPN con los servicios DNS internos.<p><p>_**Tenga en cuenta.**_<br>Si activa los filtros de tráfico en el perfil de dispositivo túnel, el túnel de dispositivo deniega el tráfico entrante (desde la red corporativa para el cliente).<p><p>Definir mediante:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Se incluyen cuando los clientes que están detrás de firewalls o servidores proxy. | Puede configurar para recurrir a SSTP (de IKEv2) con el tipo de protocolo de túnel automático o en el perfil de VPN.<p><p>_**Tenga en cuenta.**_<br>Túnel de usuario es compatible con SSTP e IKEv2 y túnel del dispositivo es compatible con IKEv2 sólo con no se admite para la reserva SSTP.<p>Definir mediante:<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Compatibilidad con modo de acceso de extremo a perímetro. | VPN de Always On proporciona conectividad a los recursos corporativos mediante el uso de directivas de túnel que requieren autenticación y cifrado, hasta que llegan a la puerta de enlace VPN. De forma predeterminada, las sesiones de túnel terminan en la puerta de enlace VPN, que también funciona como la puerta de enlace de IKEv2, que proporciona seguridad de extremo a perímetro. |
| Compatibilidad con la autenticación de certificado de máquina. | El tipo de protocolo IKEv2 disponible como parte de la plataforma de VPN de Always On en concreto admite el uso de certificados de equipo o máquina para la autenticación de VPN.<p><p>_**Tenga en cuenta.**_<br>IKEv2 es el único protocolo admitido para el túnel de dispositivo y no hay ninguna opción de soporte técnico para la reserva SSTP. <p>Definir mediante:<br>**VPNv2/ProfileName/NativeProfile/Authentication/MachineMethod** |
| Usar grupos de seguridad para limitar la funcionalidad de acceso remoto a clientes específicos. | Puede configurar VPN de Always On para admitir una autorización específica cuando se usa RADIUS, que incluye el uso de grupos de seguridad para controlar el acceso VPN. |
| Compatibilidad con servidores detrás de un firewall perimetral o un dispositivo NAT. | VPN de Always On le ofrece la capacidad de usar protocolos como IKEv2 y SSTP son totalmente compatibles con el uso de una puerta de enlace VPN que está detrás de un firewall perimetral o de dispositivo NAT.<p><p>_**Tenga en cuenta.**_<br>Túnel de usuario es compatible con SSTP e IKEv2 y túnel del dispositivo es compatible con IKEv2 sólo con no se admite para la reserva SSTP. |
| Capacidad para determinar la conectividad de la intranet cuando se conecta a la red corporativa. | Detección de redes de confianza proporciona la capacidad para detectar las conexiones de red corporativa, y se basa en una evaluación del sufijo DNS específico de la conexión asignado a las interfaces de red y el perfil de red.<p><p>Definir mediante:<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| Compatibilidad con protección de acceso a redes (NAP). | El cliente de VPN de Always On puede integrarse con acceso condicional de Azure para exigir MFA, cumplimiento de dispositivos o una combinación de ambos. Cuando es compatible con las directivas de acceso condicional, Azure AD emite un certificado de autenticación de IPsec corta duración (de forma predeterminada, 60 minutos) que el cliente, a continuación, puede usar para autenticarse en la puerta de enlace VPN. Cumplimiento del dispositivo aprovecha las ventajas de las directivas de cumplimiento de System Center Configuration Manager o Intune, que puede incluir el estado de atestación de estado de dispositivo. En este momento, acceso condicional de VPN de Azure proporciona la sustitución más cercana a la solución NAP existente, aunque no hay ninguna forma de corrección capacidades de red de servicio o poner en cuarentena. Para obtener más información, consulte [VPN y el acceso condicional](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access).<p>Definir mediante:<br>**VPNv2/ProfileName/DeviceCompliance** |
| Capacidad para definir qué servidores de administración son accesibles antes de inicio de sesión de usuario. | Puede lograr esta funcionalidad en VPN de Always On mediante la característica de túnel de dispositivo (disponible en la versión 1709 – sólo IKEv2) en el perfil VPN que se combinan con filtros de tráfico para controlar los sistemas de administración en la red corporativa que son accesibles a través de la Túnel de dispositivo.<p><p>_**Tenga en cuenta.**_<br>Si activa los filtros de tráfico en el perfil de dispositivo túnel, el túnel de dispositivo deniega el tráfico entrante (desde la red corporativa para el cliente).<p>Definir mediante:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## <a name="additional-functionalities"></a>Funcionalidades adicionales

Cada elemento de esta sección es un escenario de caso de uso o funcionalidad de acceso remoto usados para el que ha mejorado su funcionalidad VPN de Always On, ya sea a través de una expansión de la funcionalidad o la eliminación de una limitación anterior.


| Área funcional | VPN de Always On  |
| ---- | ---- |
| Dispositivos Unidos a dominio con el requisito de las SKU de Enterprise. | Admite la VPN de Always On unido al dominio, unido a permaneciendo (grupo de trabajo) o dispositivos Unidos a AD – Azure para permitir enterprise y escenarios de BYOD. VPN de Always On está disponible en todas las ediciones de Windows y las características de plataforma están disponibles a terceros por medio de la compatibilidad con los complemento UWP VPN.<p><p>_**Tenga en cuenta.**_<br>Túnel de dispositivo solo puede configurarse en dispositivos Unidos a un dominio que ejecutan Windows 10 Enterprise o educación, versión 1709 o versiones posteriores. No hay ninguna compatibilidad para control de terceros del túnel de dispositivo. |
| Compatibilidad con IPv4 e IPv6. | Con VPN de Always On, los usuarios pueden acceder a recursos de IPv4 e IPv6 en la red corporativa. El cliente de VPN de Always On usa un enfoque de doble pila que específicamente no depende de IPv6 o la necesidad de la puerta de enlace VPN para proporcionar servicios de traducción de NAT64 o DNS64. |
| Compatibilidad con autenticación OTP o de dos fases. |La plataforma de VPN de Always On EAP, que permite el uso de Microsoft distintos y tipos EAP de terceros como parte del flujo de trabajo de autenticación admite de forma nativa. VPN de Always On en concreto admite tarjeta (físico y virtual) y Windows Hello para certificados de empresa para satisfacer los requisitos de autenticación en dos fases. Además, siempre VPN admite OTP a través de MFA (no admitido de forma nativa, solo se admite en los complementos de terceros) por medio de la integración de RADIUS de EAP.<p><p>Definir mediante:<br>**VPNv2/ProfileName/NativeProfile/Authentication** |
| Compatibilidad con varios dominios y bosques. | La plataforma de VPN de Always On no tiene ninguna dependencia en la topología de bosques o dominios de Active Directory Domain Services (AD DS) (o niveles de esquema/funcional asociado) porque no requiere el cliente VPN estar unido a la función. Directiva de grupo es, por tanto, no una dependencia para definir la configuración del perfil de VPN porque no se usan durante la configuración de cliente. Cuando se requiere la integración de autorización de Active Directory, puede lograr a través de RADIUS como parte del proceso de autenticación y autorización de EAP. |
| Compatibilidad con ambos división y forzar la tunelización de separación del tráfico de internet o intranet. | Puede configurar VPN de Always On para admitir ambos forzar túnel (el modo de funcionamiento de forma predeterminada) y dividir el túnel de forma nativa. VPN de Always On proporciona una granularidad adicional para directivas de enrutamiento específicos de la aplicación.<p><p>_**Tenga en cuenta.**_<br>Admite solo el túnel de usuario.<p><p>Definir mediante:<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Compatibilidad con varios protocolos. | VPN de Always On puede configurarse para admitir de forma nativa SSTP si se requiere la capa de Sockets seguros reserva de IKEv2.<p><p>_**Tenga en cuenta.**_<br>Túnel de usuario es compatible con SSTP e IKEv2 y túnel del dispositivo es compatible con IKEv2 sólo con no se admite para la reserva SSTP.  |
| Asistente de conectividad para proporcionar el estado de conectividad corporativa. | VPN de Always On está totalmente integrado con el Asistente de conectividad de red nativa y proporciona el estado de conectividad de la interfaz de la vista todas las redes. Con la llegada de Windows 10 Creators Update (versión 1703), el estado de la conexión VPN y el control de conexión de VPN de túnel de usuario ahora también están disponibles a través de la ventana flotante de red (para el Windows cliente VPN integrado). |
| Resolución de nombres de los recursos corporativos mediante el nombre corto, el nombre de dominio completo (FQDN) y el sufijo DNS. | VPN de Always On forma nativa se puede definir uno o varios sufijos DNS como parte de la conexión VPN y el proceso de asignación de dirección IP, incluidos la resolución de nombres de los recursos corporativos para los nombres cortos, FQDN o espacios de nombres DNS completo. VPN de Always On también admite el uso de tablas de directiva de resolución de nombres para proporcionar la granularidad de resolución de espacio de nombres específicos.<p><p>_**Tenga en cuenta.**_<br>Evite el uso de sufijos Global como interfieren con la resolución de nombre corto cuando se usan tablas de la directiva de resolución de nombre.<p><p>Definir mediante:<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/DomainNameInformationList** |
---



## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre las mejoras de VPN de Always On](always-on-vpn/always-on-vpn-enhancements.md)

- [Obtenga información sobre algunas de las características avanzadas de VPN de Always On](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [Más información sobre la tecnología de VPN de Always On](always-on-vpn/always-on-vpn-technology-overview.md)

- [Comience a planear la implementación de VPN de Always On](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
