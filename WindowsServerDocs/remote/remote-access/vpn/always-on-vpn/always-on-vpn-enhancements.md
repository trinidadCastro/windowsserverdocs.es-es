---
title: Mejoras de VPN de Always On
description: VPN de Always On tiene muchas ventajas respecto a las soluciones VPN de Windows del pasado. Las principales mejoras en la integración, seguridad, conectividad, redes control y compatibilidad alinean VPN de Always On con la visión de Microsoft en la nube primero, móvil primero.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836526"
---
# <a name="always-on-vpn-enhancements"></a>Mejoras de VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

&#171;[ **Anterior:** Obtenga información acerca de las características de VPN de Always On](../vpn-map-da.md)<br>
&#187;[ **Siguiente:** Obtenga información sobre la tecnología de VPN de Always On](always-on-vpn-technology-overview.md)

VPN de Always On tiene muchas ventajas respecto a las soluciones VPN de Windows del pasado. Las siguientes mejoras clave alinean VPN de Always On con la visión de Microsoft en la nube primero, móvil primero:

- **Integración de plataforma:** VPN de Always On tiene integración mejorada con el sistema operativo de Windows y soluciones de terceros para proporcionar una plataforma sólida para escenarios de conexión avanzada innumerables.

- **Seguridad:** VPN de Always On tiene funcionalidades de seguridad nuevas y avanzadas para restringir el tipo de tráfico, las aplicaciones que pueden usar la conexión VPN, y qué métodos de autenticación puede usar para iniciar la conexión. Cuando la conexión está activa la mayoría de los casos, es especialmente importante proteger la conexión. Para obtener más información, consulte [las opciones de autenticación de VPN](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication).

- **Conectividad VPN:** VPN de Always On, con o sin dispositivos túnel proporciona funcionalidad de desencadenamiento automático. Antes de VPN de Always On, la posibilidad de desencadenar una conexión automática a través de usuario o la autenticación de dispositivos no era posible.  

- **Control de las redes:** VPN de Always On permite a los administradores especificar directivas de enrutamiento en un nivel más granular, incluso hasta la aplicación individual, que es idónea para aplicaciones de línea de negocio (LOB) que requieren acceso remoto especial.  VPN de Always On también es totalmente compatible con el protocolo de Internet versión 4 (IPv4) y versión 6 (IPv6). A diferencia de DirectAccess, no hay ninguna dependencia concreta en IPv6.

  >[!Note]
  >Antes de comenzar, asegúrese de habilitar IPv6 en el servidor VPN. En caso contrario, no se puede establecer una conexión y muestra un mensaje de error.
  
- **Compatibilidad y configuración:** VPN de Always On se pueden implementar y administrar varias maneras, que ofrece varias ventajas sobre el software de cliente VPN de VPN de Always On.

## <a name="platform-integration"></a>Integración de plataforma

Microsoft ha introducido o mejorado las siguientes capacidades de integración de VPN de Always On:

| Importante mejora ahora | Descripción |
| ---- | ---- |
| **[Windows Information Protection (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | Integración con WIP permite la aplicación de directivas de red determinar si se permite el tráfico para ir a través de VPN. Si el perfil de usuario está activo y se aplican las directivas de WIP, se desencadena automáticamente siempre VPN para conectarse. Además, cuando se usa el trabajo en curso, no hay para especificar las reglas AppTriggerList y TrafficFilterList por separado en el perfil de VPN (a menos que desee configuración más avanzada) porque las directivas WIP y listas de la aplicación tiene efecto automáticamente. |
| **[Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | VPN de Always On admite de forma nativa Windows Hello para empresas (en modo de autenticación basada en certificados) para proporcionar una única inicio de sesión experiencia perfecta para ambos iniciar sesión en el equipo y conexión a la VPN. Por lo tanto, no se necesita ninguna autenticación secundaria (credenciales de usuario) para la conexión VPN, lo que permite usar una conexión de Always On con Windows Hello para la autenticación de la empresa. |
| **[Acceso condicional de Microsoft Azure](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | El cliente de VPN de Always On puede integrarse con la plataforma de acceso condicional de Azure para exigir la autenticación multifactor (MFA), cumplimiento de dispositivos o una combinación de ambos. Cuando es compatible con las directivas de acceso condicional, Azure Active Directory (Azure AD) emite un certificado de autenticación de seguridad IP (IPsec) corta duración (de forma predeterminada, 60 minutos) que, a continuación, se puede usar para autenticarse en la puerta de enlace VPN. Cumplimiento del dispositivo usa las directivas de cumplimiento de System Center Configuration Manager o Intune, que pueden incluir el estado de atestación de estado de dispositivo como parte de la comprobación de cumplimiento de la conexión. |
| **Azure MFA** | Cuando se combina con los servicios de servicio de autenticación remota telefónica de usuario (RADIUS) y la extensión de servidor de directivas de redes (NPS) para Azure MFA, autenticación de VPN puede usar MFA seguro. |
| **Complemento de VPN de terceros** | Con la plataforma Universal de Windows (UWP), proveedores VPN de terceros pueden crear una aplicación única para el intervalo completo de Windows 10 dispositivos. UWP ofrece una capa de API principales garantizado en todos los dispositivos, lo que elimina la complejidad de y los problemas asociados a menudo con la escritura de los controladores de nivel de kernel. Actualmente, existen los complementos de VPN de Windows 10 UWP para [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), [F5 acceso](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), [ SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz), y [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); sin duda, otros usuarios aparecerán en el futuro. |
---

## <a name="security"></a>Seguridad

Son las principales mejoras de seguridad en las áreas siguientes:

| Importante mejora ahora | Descripción |
| ---- | ---- |
| **Filtros de tráfico** | A través de los filtros de tráfico, puede especificar las directivas de cliente que determinan qué tráfico se permite en la red corporativa. De este modo, los administradores pueden aplicar restricciones de aplicación o el tráfico en la interfaz VPN, limitar su uso a determinados orígenes, los puertos de destino y las direcciones IP. Existen dos tipos de reglas de filtrado:<ul><li>**Reglas en la aplicación.** Las reglas de firewall en la aplicación se basan en una lista de aplicaciones especificadas para que solo el tráfico que se origina en estas aplicaciones tienen permiso para ir a través de la interfaz VPN.</li><li>**Reglas de tráfico.** Las reglas de firewall basado en el tráfico se basan en los requisitos de red, como los protocolos, direcciones y puertos. Use estas reglas solo para el tráfico que cumple estas condiciones específicas pueden ir a través de la interfaz VPN.<p><p>_**Tenga en cuenta.**_<br>Estas reglas se aplican solo al tráfico saliente desde el dispositivo. Uso de tráfico filtra bloquea el tráfico entrante desde la red corporativa al cliente. </li></ul> |
| **VPN por aplicación** | VPN por aplicación es como tener un filtro de tráfico basado en aplicación, pero va más allá para combinar los desencadenadores de aplicación con un filtro de tráfico basado en aplicación para que la conectividad VPN está restringida a una aplicación específica en lugar de todas las aplicaciones en el cliente VPN. La característica se inicia automáticamente cuando se inicia la aplicación. |
| **Compatibilidad con algoritmos de criptografía personalizados de IPsec** | VPN de Always On se admite el uso de RSA y algoritmos criptográficos personalizados basados en criptografía en curva elíptica para cumplir los estricto gobierno o las directivas de seguridad de la organización. |
| **Compatibilidad nativa con el protocolo de autenticación Extensible (EAP)** | VPN de Always On EAP, que le permite usar un conjunto diverso de Microsoft y los tipos de EAP de terceros como parte del flujo de trabajo de autenticación admite de forma nativa. EAP ofrece autenticación segura basada en los siguientes tipos de autenticación:<ul><li>Nombre de usuario y contraseña</li><li>Tarjeta (físico y virtual)</li><li>Certificados de usuario</li><li>Windows Hello para empresas</li><li>Compatibilidad de MFA por medio de la integración de RADIUS de EAP</li></ul>El proveedor de la aplicación controla los métodos de autenticación de complemento de UWP VPN de terceros, aunque tienen una matriz de las opciones disponibles, incluidos los tipos de credenciales personalizadas y soporte técnico OTP. |
---

## <a name="vpn-connectivity"></a>Conectividad VPN

Estas son las principales mejoras de conectividad de VPN de Always On:

| Importante mejora ahora | Descripción |
| ---- | ---- |
| **Siempre activado** | Always On es una característica de Windows 10 que permite que el perfil de VPN activo para conectarse automáticamente y permanecen conectados basados en desencadenadores, es decir, inicio de sesión de usuario, cambio de estado de la red o la pantalla del dispositivo activada. AlwaysOn también se integra en la experiencia conectada en espera para maximizar la duración de la batería. |
| **Desencadenamiento de la aplicación** | Puede configurar perfiles de VPN en Windows 10 para conectar automáticamente en el inicio de un conjunto de aplicaciones especificado. Puede configurar el escritorio y aplicaciones de UWP para desencadenar una conexión VPN. |
| **Activación basada en nombres** | Con VPN de Always On, puede definir reglas para que las consultas de nombres de dominio específico desencadenan la conexión VPN. Windows 10 admite ahora la basada en el nombre de la activación para equipos unidos al dominio y unido a permaneciendo (anteriormente, se admitían los equipos unidos a un permaneciendo solo). |
| **Detección de redes de confianza** | VPN de Always On incluye esta característica para garantizar que la conectividad VPN no se desencadena si un usuario está conectado a una red de confianza dentro de los límites corporativos. Esta característica se puede combinar con cualquiera de los métodos de activación que se ha mencionado anteriormente para proporcionar una experiencia de usuario "conectar sólo cuando sea necesario". |
| **[Túnel de dispositivo](../vpn-device-tunnel-config.md)** | VPN de Always On le ofrece la capacidad para crear un perfil de VPN dedicado para el dispositivo o equipo. A diferencia de _usuario túnel_, que se conecta sólo después de un usuario inicia sesión en el dispositivo o equipo, _dispositivo túnel_ permite la VPN establecer la conectividad antes de inicio de sesión de usuario. Túnel de dispositivo y usuario túnel funcionan de forma independiente con sus perfiles VPN, se pueden conectar al mismo tiempo y pueden usar diferentes métodos de autenticación y otras opciones de configuración de VPN según corresponda. |
---

## <a name="networking"></a>Funciones de red

Estas son algunas de las mejoras de red en VPN de Always On:

| Importante mejora ahora | Descripción |
| ---- | ---- |
| **Compatibilidad con doble pila IPv4 e IPv6** | VPN de Always On admite el uso de IPv4 e IPv6 de forma nativa en un enfoque de doble pila. No tiene ninguna dependencia específica en un protocolo a través de la otra, lo que permite la compatibilidad de aplicaciones IPv4/IPv6 máxima combinada con la compatibilidad con IPv6 futuras necesidades de red.<p>**_Tenga en cuenta._** Antes de comenzar, asegúrese de habilitar IPv6 en el servidor VPN. En caso contrario, no se puede establecer una conexión y muestra un mensaje de error.|
| **Directivas de enrutamiento específicos de la aplicación** | Además de definir globales directivas de enrutamiento de conexión VPN para separar el tráfico de internet e intranet, es posible agregar directivas de enrutamiento para controlar el uso de túnel dividido o forzar las configuraciones de túnel por la aplicación. Esta opción le ofrece control pormenorizado sobre qué aplicaciones pueden interactuar con los recursos a través del túnel VPN. |
| **Rutas de exclusión** | VPN de Always On es compatible con la capacidad de especificar las rutas de exclusión que controlan el comportamiento de enrutamiento para definir qué tráfico debe recorrer la VPN solo y no vaya a través de la interfaz de red físico en concreto.<p><p>_**Notas.**_<br>-Las rutas de exclusión funcionan actualmente para el tráfico dentro de la misma subred que el cliente, por ejemplo, LinkLocal.<br>-Las rutas de exclusión solo funcionan en una configuración de túnel dividido. |
---

## <a name="configuration-and-compatibility"></a>Compatibilidad y configuración 

Estas son algunas de las mejoras de compatibilidad y configuración de VPN de Always On:

| Importante mejora ahora | Descripción |
| ---- | ---- |
| **Compatibilidad de puerta de enlace VPN de terceros** | El cliente de VPN de Always On no requiere el uso de una puerta de enlace VPN basadas en Microsoft para que funcione. El cliente a través de la compatibilidad del protocolo IKEv2, facilita la interoperabilidad con las puertas de enlace VPN de terceros que admiten este tipo túnel estándar del sector. También se puede lograr la interoperabilidad con las puertas de enlace VPN de terceros mediante un complemento de VPN de UWP combinado con un tipo túnel personalizado sin tener que sacrificar las ventajas y características de la plataforma de VPN de Always On.<p><p>_**Tenga en cuenta.**_<br>Consulte con el proveedor de aplicaciones de back-end de puerta de enlace o de terceros en las configuraciones y compatibilidad con VPN de Always On y el túnel de dispositivo con IKEv2. |
| **Compatibilidad con el protocolo estándar del sector VPN IKEv2** | El cliente de VPN de Always On es compatible con IKEv2, uno de hoy más ampliamente usados de protocolos de túnel estándar del sector. Esta compatibilidad maximiza la interoperabilidad con las puertas de enlace VPN de terceros. |
| **Compatibilidad con plataformas** | Admite VPN On AAlways unido al dominio, unido a permaneciendo (grupo de trabajo) o dispositivos Unidos a AD – Azure para permitir tanto las empresas y traiga su propio dispositivo escenarios (BYOD). Además, la VPN de Always On está disponible en todas las ediciones de Windows. |
| **Diversos mecanismos de implementación y administración** | Puede usar varios mecanismos de implementación y administración para administrar la configuración de VPN (llamado un _perfil de VPN_), incluidos Windows PowerShell, System Center Configuration Manager, Intune (o herramienta de administración [MDM] de otros fabricantes de dispositivos móviles) y el Diseñador de configuración de Windows. Estas opciones simplifican la configuración de VPN de Always On, independientemente de las herramientas de administración de cliente que usa. |
| **Definición de perfil VPN estándar** | VPN de Always On admite la configuración mediante un perfil XML estándar (ProfileXML), que proporciona un formato de plantilla de configuración estándar que usan mayoría de los conjuntos de herramientas de implementación y administración. |
---


## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información sobre algunas de las características avanzadas de VPN de Always On](deploy/always-on-vpn-adv-options.md)

- [Más información sobre la tecnología de VPN de Always On](always-on-vpn-technology-overview.md)

- [Comience a planear la implementación de VPN de Always On](deploy/always-on-vpn-deploy-deployment.md)



---