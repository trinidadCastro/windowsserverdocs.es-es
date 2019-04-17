---
title: Características de VPN siempre activada
description: En este tema, aprenderás sobre las características y funcionalidades de VPN siempre activada.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066859"
---
# Las funcionalidades y características de VPN siempre activada

>Se aplica a: Windows Server \(Semi-Annual Channel\), Windows Server 2016, Windows 10

& #171;  [ **Anterior:** siempre en la implementación de VPN para Windows Server y Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
& #187; [ **Siguiente:** obtener información sobre las mejoras de VPN siempre activadas](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

En este tema, aprenderás sobre las características y funcionalidades de VPN siempre activada.  En la tabla siguiente no es una lista exhaustiva, sin embargo, incluyen algunas de las características y funcionalidades que se usan en soluciones de acceso remoto más comunes. 

>[!TIP]
>Si usas actualmente DirectAccess, te recomendamos que investigar la funcionalidad de VPN siempre activada con cuidado para determinar si se dirige a que todas sus necesidades de acceso remoto antes de migrar de forman DirectAccess a VPN siempre activada.  

| Área funcional | VPN de Always On  |
| ---- | ---- |
| Conectividad perfecta transparente a la red corporativa. | Puedes configurar VPN siempre activada para admitir el desencadenamiento automático en función de solicitudes de resolución de espacio de nombres o el inicio de la aplicación.<p><p>Definir mediante:<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/DomainNameInformationList/AutoTrigger** |
| Uso de un túnel dedicada de infraestructura para proporcionar conectividad a los usuarios no inició sesión en la red corporativa. | Puedes lograr esta funcionalidad mediante la característica de túnel de dispositivo en el perfil de VPN.<p><p>_**Nota.**_<br>Túnel de dispositivo solo se puede configurar en los dispositivos Unidos a un dominio mediante IKEv2 con autenticación de certificados de equipo.<p><p>Definir mediante:<br>**VPNv2/ProfileName/DeviceTunnel** |
| Uso de fuera de administrar para permitir la conectividad remota a los clientes de sistemas de administración que se encuentran en la red corporativa. | Puedes lograr esta funcionalidad mediante la característica de túnel de dispositivo en el perfil VPN, combinada con la configuración de la conexión VPN para registrar dinámicamente las direcciones IP asignadas a la interfaz VPN con servicios DNS internos.<p><p>_**Nota.**_<br>Si activar filtros de tráfico en el perfil de túnel de dispositivo, el túnel de dispositivo deniega el tráfico entrante (desde la red corporativa para el cliente).<p><p>Definir mediante:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Entran cuando los clientes están detrás de firewalls o servidores proxy. | Puedes configurar para volver a SSTP (de IKEv2) mediante el tipo de protocolo de túnel automático en el perfil VPN.<p><p>_**Nota.**_<br>Túnel de usuario admite SSTP y IKEv2 y túnel de dispositivo admite IKEv2 solo con ninguna compatibilidad con la reserva SSTP.<p>Definir mediante:<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Compatibilidad con el modo de acceso de extremo a extremo. | VPN siempre activada proporciona conectividad a los recursos corporativos mediante el uso de las directivas de túnel que requieren autenticación y cifrado hasta que llegan a la puerta de enlace VPN. De manera predeterminada, las sesiones de túnel terminan en la puerta de enlace VPN, que también funcione como la puerta de enlace IKEv2, lo que proporciona seguridad de extremo a extremo. |
| Compatibilidad con la autenticación de certificado de máquina. | El tipo de protocolo IKEv2 disponible como parte de la plataforma de VPN siempre activada admite específicamente el uso de certificados de equipo o máquina para la autenticación de VPN.<p><p>_**Nota.**_<br>IKEv2 es el único protocolo admitido de túnel de dispositivo y no hay ninguna opción de soporte técnico de reserva SSTP. <p>Definir mediante:<br>**VPNv2/ProfileName/NativeProfile/autenticación/MachineMethod** |
| Usar grupos de seguridad para limitar la funcionalidad de acceso remoto a clientes específicos. | Puedes configurar VPN siempre activada para admitir la autorización granular al usar la radio, que incluye el uso de grupos de seguridad para controlar el acceso VPN. |
| Soporte para servidores detrás de un firewall de edge o un dispositivo NAT. | VPN siempre activada te ofrece la posibilidad de usar protocolos como IKEv2 y SSTP que son totalmente compatibles con el uso de una puerta de enlace VPN que se encuentra detrás de un firewall de dispositivo o el borde NAT.<p><p>_**Nota.**_<br>Túnel de usuario admite SSTP y IKEv2 y túnel de dispositivo admite IKEv2 solo con ninguna compatibilidad con la reserva SSTP. |
| Capacidad para determinar la conectividad a la intranet cuando estén conectados a la red corporativa. | Detección de redes de confianza proporciona la capacidad de detectar las conexiones de red corporativa y se basa en una evaluación del sufijo DNS específico de la conexión asignado a las interfaces de red y el perfil de red.<p><p>Definir mediante:<br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| Cumplimiento con la protección de acceso a redes (NAP). | El cliente VPN siempre activada puede integrarse con el acceso condicional de Azure para aplicar MFA, cumplimiento del dispositivo o una combinación de ambos. Cuando se cumple con las directivas de acceso condicional, Azure AD emite un certificado de autenticación de IPsec corta duración (de forma predeterminada, 60 minutos) que, a continuación, usar el cliente para autenticarse en la puerta de enlace VPN. Cumplimiento del dispositivo aprovecha las ventajas de las directivas de cumplimiento de System Center Configuration Manager/Intune, que puede incluir el estado de atestación de estado de dispositivo. En este momento, el acceso condicional de Azure VPN proporciona el reemplazo más cercano a la solución NAP existente, aunque no hay ninguna forma de corrección las funcionalidades de red de servicio o poner en cuarentena. Para obtener más información, consulta la [VPN y acceso condicional](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access).<p>Definir mediante:<br>**VPNv2/ProfileName/DeviceCompliance** |
| Capacidad de definir qué servidores de administración son accesibles antes de la sesión del usuario. | Puedes lograr esta funcionalidad en VPN siempre activada mediante la característica de túnel de dispositivo (disponible en la versión 1709: para solo IKEv2) en el perfil VPN, combinado con filtros de tráfico para controlar qué sistemas de administración de la red corporativa son accesibles a través de la Túnel de dispositivo.<p><p>_**Nota.**_<br>Si activar filtros de tráfico en el perfil de túnel de dispositivo, el túnel de dispositivo deniega el tráfico entrante (desde la red corporativa para el cliente).<p>Definir mediante:<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## Funcionalidades adicionales

Cada elemento en esta sección es un escenario de casos de uso o la funcionalidad de acceso remoto usados para el que ha mejorado su funcionalidad VPN siempre activada, ya sea a través de una ampliación de funcionalidad o la eliminación de una limitación anterior.


| Área funcional | VPN de Always On  |
| ---- | ---- |
| Dispositivos Unidos a un dominio con el requisito de SKU de la empresa. | Admite VPN siempre activada unido al dominio, unidas a pertenece (grupo de trabajo) o dispositivos: unido a AD Azure para permitir la empresa y los escenarios tipo BYOD. VPN siempre activada está disponible en todas las ediciones de Windows y las características de plataforma están disponibles a terceros por medio de soporte técnico de complemento de VPN de UWP.<p><p>_**Nota.**_<br>Túnel de dispositivo solo se puede configurar en los dispositivos Unidos a un dominio que ejecute Windows 10 Enterprise o Education versión 1709 o posterior. No hay ninguna compatibilidad para control de aplicaciones de terceros de túnel de dispositivo. |
| Soporte para IPv4 e IPv6. | Con la VPN siempre activada, los usuarios pueden acceder a recursos de IPv4 e IPv6 en la red corporativa. El cliente de VPN siempre activada usa un enfoque de pila dual que específicamente no depende de IPv6 o la necesidad de la puerta de enlace VPN para proporcionar los servicios de traducción NAT64 o DNS64. |
| Compatibilidad con autenticación OTP o de dos factores. |La plataforma de VPN siempre activada admite de forma nativa EAP, lo que permite el uso de Microsoft diverso y tipos de EAP de terceros como parte del flujo de trabajo de autenticación. VPN siempre activada admite específicamente de tarjetas inteligentes (físicos y virtuales) y Windows Hello para certificados de empresa para satisfacer los requisitos de autenticación en dos fases. Además, VPN siempre activada admite OTP a través de MFA (no se admite de forma nativa, solo se admite en los complementos de terceros) mediante la integración de EAP RADIUS.<p><p>Definir mediante:<br>**VPNv2/ProfileName/NativeProfile/autenticación** |
| Compatibilidad con varios dominios y bosques. | La plataforma de VPN siempre activada no tiene ninguna dependencia de topología de bosques o dominio de Active Directory Domain Services (AD DS) (o niveles funcionales asociados o esquema) porque no requiere el cliente VPN para que sea el dominio unido a la función. La directiva de grupo no es, por tanto, una dependencia para definir la configuración de perfil VPN porque no se usa durante la configuración de cliente. Cuando la integración de autorización de Active Directory, puedes lograr a través de radio como parte del proceso de autenticación y autorización de EAP. |
| Compatibilidad con ambos en dos paneles y forzar túnel de separación de tráfico de internet o intranet. | Puedes configurar VPN siempre activada para admitir tanto túnel forzado (modo operativo predeterminado) y túnel dividido forma nativa. VPN siempre activada proporciona mayor granularidad para directivas de enrutamiento específicos de la aplicación.<p><p>_**Nota.**_<br>Admite solo túnel de usuario.<p><p>Definir mediante:<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Compatibilidad con varios protocolos. | VPN siempre activada puede configurarse para admitir SSTP de forma nativa si se requiere la capa de Sockets seguros reserva desde IKEv2.<p><p>_**Nota.**_<br>Túnel de usuario admite SSTP y IKEv2 y túnel de dispositivo admite IKEv2 solo con ninguna compatibilidad con la reserva SSTP.  |
| Asistente para la conectividad para proporcionar el estado de conectividad corporativa. | VPN siempre activada totalmente integrada con el Asistente de conectividad de red nativo y proporciona el estado de conectividad de la interfaz de ver todas las redes. Con la llegada de Windows 10 Creators Update (versión 1703), el estado de la conexión VPN y el control de conexión VPN para túnel de usuario están ahora disponibles mediante el control flotante de red (para el Windows cliente VPN integrado), también. |
| Resolución de nombres de los recursos corporativos con nombre corto, el nombre de dominio completo (FQDN) y el sufijo DNS. | VPN siempre activada forma nativa definir uno o más sufijos DNS como parte de la conexión VPN y el proceso de asignación de direcciones IP, incluida la resolución de nombres de recursos corporativos para nombres cortos, FQDN o toda espacios de nombres DNS. VPN siempre activada también admite el uso de tablas de directivas de resolución de nombres para proporcionar granularidad de resolución de espacio de nombres específicos.<p><p>_**Nota.**_<br>Evita el uso de sufijos Global como interfieren con una resolución shortname al usar tablas de directivas de resolución de nombre.<p><p>Definir mediante:<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/DomainNameInformationList** |
---



## Pasos siguientes

- [Más información sobre las mejoras de VPN siempre activada](always-on-vpn/always-on-vpn-enhancements.md)

- [Obtén información sobre algunas de las características avanzadas de VPN siempre activada](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [Más información sobre la tecnología de VPN siempre activada](always-on-vpn/always-on-vpn-technology-overview.md)

- [Empezar a planificar la implementación de VPN siempre activada](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
