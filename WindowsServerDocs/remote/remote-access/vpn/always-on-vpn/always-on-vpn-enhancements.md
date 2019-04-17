---
title: Mejoras de VPN de Always On
description: VPN siempre activada tiene muchas ventajas a través de las soluciones de VPN de Windows de los últimos. Las mejoras clave de integración, seguridad, conectividad, control de red y compatibilidad alinean VPN siempre activada con la visión de la nube y móvil, primero de Microsoft.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067495"
---
# Mejoras de VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

& #171; [ **Anterior:** más información sobre las características de VPN siempre activada](../vpn-map-da.md)<br>
& #187; [ **Siguiente:** más información sobre la tecnología de VPN siempre activada](always-on-vpn-technology-overview.md)

VPN siempre activada tiene muchas ventajas a través de las soluciones de VPN de Windows de los últimos. Las siguientes mejoras clave alinean VPN siempre activada con la visión de la nube y móvil, primero de Microsoft:

- **Integración de la plataforma:** VPN siempre activada tiene mejor integración con el sistema operativo Windows y las soluciones de terceros para proporcionar una plataforma sólida para creaba escenarios innumerables avanzadas de conexión.

- **Seguridad:** VPN siempre activada tiene funcionalidades de seguridad nuevas y avanzadas para restringir el tipo de tráfico, las aplicaciones que pueden usar la conexión VPN, y los métodos de autenticación pueden usar para iniciar la conexión. Cuando la conexión está activa en la mayoría de las veces, es especialmente importante proteger la conexión. Para obtener más información, consulta [las opciones de autenticación de VPN](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication).

- **Conectividad VPN:** VPN siempre activada, con o sin túnel de dispositivo proporciona funcionalidad de desencadenamiento automático. Antes de VPN siempre activada, la capacidad de desencadenar una conexión automática a través de usuario o autenticación de dispositivos no era posible.  

- **Control de redes:** VPN siempre activada permite a los administradores especificar directivas de enrutamiento en un nivel más detallado, incluso en la aplicación individual, que es perfecto para las aplicaciones de línea de negocio (LOB) que requieren acceso remoto especial.  VPN siempre activada también es totalmente compatible con ambas protocolo de Internet versión 4 (IPv4) y versión 6 (IPv6). A diferencia de DirectAccess, no hay ninguna dependencia específica sobre IPv6.

  >[!Note]
  >Antes de comenzar, asegúrate de habilitar IPv6 en el servidor VPN. De lo contrario, no se puede establecer una conexión y muestra un mensaje de error.
  
- **Configuración y compatibilidad:** VPN siempre activada se puede implementar y administrar varios aspectos, lo que proporciona varias ventajas sobre el software de cliente VPN de VPN siempre activada.

## Integración de la plataforma

Microsoft ha introducido o mejorar las siguientes capacidades de integración de VPN siempre activada:

| Mejora clave | Descripción |
| ---- | ---- |
| **[Windows Information Protection (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | Integración con WIP permite que la aplicación de directivas de red determinar si se permite el tráfico para ir a través de la VPN. Si el perfil de usuario está activo y se aplican directivas WIP, automáticamente se desencadena la VPN siempre activada para conectarte. Además, al usar WIP, no es necesario especificar reglas AppTriggerList y TrafficFilterList por separado en el perfil de VPN (a menos que quieras configuración más avanzada) porque las listas de aplicaciones y las directivas de WIP surten efecto automáticamente. |
| **[Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | VPN siempre activada forma nativa admite Windows Hello para empresas (en modo de autenticación basada en certificados) para proporcionar una única sesión experiencia óptima para ambos inicia sesión en la máquina y conexión a la VPN. Por lo tanto, no se necesita ninguna autenticación secundario (credenciales de usuario) para la conexión VPN, lo que permite usar una conexión siempre activado con Windows Hello para empresas la autenticación. |
| **[Acceso condicional de Azure de Microsoft](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | El cliente VPN siempre activada puede integrarse con la plataforma de acceso condicional de Azure para aplicar la autenticación multifactor (MFA), cumplimiento del dispositivo o una combinación de las dos. Cuando se cumple con las directivas de acceso condicional, Azure Active Directory (AD Azure) emite un certificado de autenticación de seguridad IP (IPsec) corta duración (de forma predeterminada, 60 minutos) que puede usarse para autenticar a la puerta de enlace VPN. Cumplimiento del dispositivo usa las directivas de cumplimiento de System Center Configuration Manager/Intune, que pueden incluir el estado de atestación de estado de dispositivo como parte de la comprobación del cumplimiento de conexión. |
| **Azure MFA** | Cuando se combina con los servicios de servicio de autenticación remota telefónica de usuario (RADIUS) y la extensión de servidor de directivas de redes (NPS) para Azure MFA, autenticación de VPN usar MFA segura. |
| **Complemento de VPN de terceros** | Con la plataforma Universal de Windows (UWP), los proveedores de VPN de terceros pueden crear una sola aplicación para el intervalo completo de Windows 10 dispositivos. UWP proporciona una capa de API principal garantizada en todos los dispositivos, lo que elimina la complejidad de y los problemas a menudo asociados con la escritura de los controladores de nivel de kernel. Actualmente, existen complementos de VPN de UWP de Windows 10 para [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), [F5 Access](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), [Check Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)y [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); sin duda, otros aparecerá en el futuro. |
---

## Seguridad

Las mejoras principales de seguridad se encuentran en las siguientes áreas:

| Mejora clave | Descripción |
| ---- | ---- |
| **Filtros de tráfico** | A través de los filtros de tráfico, puedes especificar directivas del lado cliente que determinan qué tráfico se permite en la red corporativa. De este modo, los administradores pueden aplicar restricciones de la aplicación o el tráfico en la interfaz VPN, limitar su uso a orígenes específicos, destino puertos y direcciones IP. Existen dos tipos de reglas de filtrado:<ul><li>**Reglas basadas en aplicaciones.** Reglas de firewall de la aplicación se basan en una lista de aplicaciones especificadas para que solo el tráfico procedente de dichas aplicaciones se permiten para ir a través de la interfaz VPN.</li><li>**Reglas basadas en el tráfico.** Reglas de firewall basadas en el tráfico se basan en los requisitos de red como puertos, direcciones y protocolos. Usa estas reglas solo para el tráfico que coincida con estas condiciones específicas están permitidas en la interfaz VPN.<p><p>_**Nota.**_<br>Estas reglas se aplican solo para el tráfico saliente desde el dispositivo. Uso de tráfico filtra bloquea el tráfico entrante de la red corporativa para el cliente. </li></ul> |
| **VPN por aplicación** | VPN por aplicación es similar a tener un filtro de tráfico basado en la aplicación, pero va más allá combinar los desencadenadores de la aplicación con un filtro de tráfico basado en la aplicación para que las conexiones VPN está restringido a una aplicación específica en contraposición a todas las aplicaciones en el cliente VPN. La característica se inicia automáticamente cuando se inicia la aplicación. |
| **Compatibilidad con algoritmos de criptografía de IPsec personalizados** | VPN siempre activada admite el uso de RSA y algoritmos criptográficos personalizado basado en la criptografía de curva elíptica para satisfacer government estricto o directivas de seguridad de la organización. |
| **Soporte de protocolo de autenticación Extensible (EAP) nativo** | VPN siempre activada admite de forma nativa EAP, que te permite usar una amplia variedad de Microsoft y los tipos EAP de terceros como parte del flujo de trabajo de autenticación. EAP proporciona una autenticación segura en función de los siguientes tipos de autenticación:<ul><li>Nombre de usuario y contraseña</li><li>Tarjeta (físico y virtual)</li><li>Certificados de usuario</li><li>Windows Hello para empresas</li><li>Soporte técnico MFA por medio de la integración de EAP RADIUS</li></ul>El proveedor de la aplicación controla los métodos de autenticación de complemento de VPN de UWP de terceros, aunque tienen una matriz de las opciones disponibles, incluidos los tipos de credencial personalizada y soporte técnico OTP. |
---

## Conectividad VPN

Las siguientes son las mejoras principales en la conectividad de VPN siempre activada:

| Mejora clave | Descripción |
| ---- | ---- |
| **Siempre activado** | Siempre activado es una característica de Windows 10 que permite que el perfil VPN activo para conectarse automáticamente y permanecer conectada en función de desencadenadores — es decir, en el inicio de sesión de usuario, cambio de estado de red o activo de pantalla del dispositivo. Siempre activado también está integrado en la experiencia del modo de espera conectada para maximizar la duración de la batería. |
| **Activación de la aplicación** | Puedes configurar perfiles de VPN en Windows 10 para conectarse automáticamente en el inicio de un conjunto de aplicaciones especificado. Puedes configurar escritorio y las aplicaciones para UWP para desencadenar una conexión VPN. |
| **Desencadenar basadas en nombres** | Con la VPN siempre activada, puedes definir reglas para que las consultas de nombre de dominio específico desencadenan la conexión VPN. Windows 10 admite ahora basadas en nombres desencadenar para los equipos unidos a un dominio y sin dominio unido (anteriormente, se admitían sólo los equipos unidos a un pertenece). |
| **Detección de redes de confianza** | VPN siempre activada incluye esta característica para asegurarse de que la conectividad VPN no se activará si un usuario está conectado a una red de confianza dentro de los límites corporativos. Puedes combinar esta característica con cualquiera de los métodos de activación que se ha mencionado anteriormente para proporcionar una experiencia de usuario transparente "conectar solo cuando sea necesario". |
| **[Túnel de dispositivo](../vpn-device-tunnel-config.md)** | VPN siempre activada te ofrece la capacidad para crear un perfil de VPN dedicado para el dispositivo o máquina. A diferencia de _Túnel de usuario_, que solo se conecta después de una sesión de usuario en el equipo o dispositivo, _Túnel de dispositivo_ permite la conexión VPN establecer la conexión antes de inicio de sesión de usuario. Túnel de dispositivo y usuario túnel funcionan de forma independiente con sus perfiles VPN, se pueden conectar al mismo tiempo y usan distintos métodos de autenticación y otras opciones de configuración de VPN según corresponda. |
---

## Redes

Los siguientes son algunas de las mejoras de redes de VPN siempre activada:

| Mejora clave | Descripción |
| ---- | ---- |
| **Compatibilidad de doble pila para IPv4 e IPv6** | VPN siempre activada admite de forma nativa el uso de IPv4 e IPv6 en un enfoque de la pila de doble. No tiene ninguna dependencia específica en un protocolo a través de la otra, lo que permite la máxima compatibilidad de aplicaciones de IPv4 o IPv6 combinada con compatibilidad con IPv6 futuras necesidades de red.<p>**_Nota._** Antes de comenzar, asegúrate de habilitar IPv6 en el servidor VPN. De lo contrario, no se puede establecer una conexión y muestra un mensaje de error.|
| **Directivas de enrutamiento específicos de la aplicación** | Además de definir globales directivas de enrutamiento de conexión VPN para la separación de tráfico de intranet y de internet, es posible agregar directivas de enrutamiento para controlar el uso de túnel dividido o forzar las configuraciones de túnel en forma individual para cada aplicación. Esta opción te ofrece un control más detallado sobre qué aplicaciones se pueden interactuar con los recursos mediante el túnel VPN. |
| **Rutas de exclusión** | VPN siempre activada admite la capacidad para especificar las rutas de exclusión que controlan específicamente el comportamiento de enrutamiento para definir qué tráfico debe recorrer la VPN solo y no se ve en la interfaz de red física.<p><p>_**Notas.**_<br>-Las rutas de exclusión actualmente funcionan para el tráfico dentro de la misma subred que el cliente, como por ejemplo, LinkLocal.<br>-Las rutas de exclusión solo funcionan en una configuración de túnel dividido. |
---

## Configuración y compatibilidad 

Los siguientes son algunas de las mejoras de compatibilidad y la configuración de VPN siempre activada:

| Mejora clave | Descripción |
| ---- | ---- |
| **Compatibilidad de puerta de enlace VPN de terceros** | El cliente VPN siempre activada no requiere el uso de una puerta de enlace VPN basados en Microsoft para que funcione. A través de la compatibilidad del protocolo IKEv2, el cliente facilita la interoperabilidad con las puertas de enlace VPN de terceros que admiten este tipo túnel estándar del sector. También puedes lograr la interoperabilidad con puertas de enlace VPN de terceros mediante el uso de un complemento de VPN de UWP combinado con un tipo túnel personalizado sin sacrificar ventajas y las características de plataforma de VPN siempre activada.<p><p>_**Nota.**_<br>Consulte con el fabricante del dispositivo de back-end de puerta de enlace o de terceros en configuraciones y compatibilidad con VPN siempre activada y túnel de dispositivo mediante IKEv2. |
| **Compatibilidad con el protocolo estándar del sector VPN IKEv2** | El cliente VPN siempre activada admite IKEv2, uno de hoy en día más usadas en protocolos de túnel estándar del sector. Esta compatibilidad maximiza la interoperabilidad con puertas de enlace VPN de terceros. |
| **Compatibilidad con la plataforma** | Admite AAlways en VPN unido al dominio, unidas a pertenece (grupo de trabajo) o dispositivos: unido a AD Azure para permitir tanto las empresas y conectar dispositivos propios escenarios (BYOD). Además, la VPN siempre activada está disponible en todas las ediciones de Windows. |
| **Diversos mecanismos de implementación y administración** | Puedes usar muchos mecanismos de implementación y administración para administrar la configuración de VPN (denominado un _perfil de VPN_), incluido Windows PowerShell, System Center Configuration Manager, Intune (o herramienta de administración de [MDM] de dispositivos móviles de terceros) y Windows Diseñador de configuraciones. Estas opciones de simplifican la configuración de VPN siempre activada, independientemente de las herramientas de administración de cliente que usas. |
| **Definición de perfil VPN estandarizado** | VPN siempre activada es compatible con la configuración con un perfil XML estándar (ProfileXML), que proporciona un formato de plantilla de configuración estándar que usan la mayoría de administración y conjuntos de herramientas de implementación. |
---


## Pasos siguientes

- [Obtén información sobre algunas de las características avanzadas de VPN siempre activada](deploy/always-on-vpn-adv-options.md)

- [Más información sobre la tecnología de VPN siempre activada](always-on-vpn-technology-overview.md)

- [Empezar a planificar la implementación de VPN siempre activada](deploy/always-on-vpn-deploy-deployment.md)



---
