---
title: Características avanzadas de VPN de Always On
description: Más allá del escenario de implementación que se proporcionan en esta implementación, puede agregar otras características avanzadas de VPN para mejorar la seguridad y disponibilidad de la conexión VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: a544ac3c1a121874170a2fc78a34bd401b8bebe1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885086"
---
# <a name="advanced-features-of-always-on-vpn"></a>Características avanzadas de VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Anterior:** Obtenga información sobre la tecnología de VPN de Always On](../always-on-vpn-technology-overview.md)<br>
&#187;[ **Siguiente:** Comience a planear la implementación de VPN de Always On](always-on-vpn-deploy-planning.md)

Más allá de los escenarios de implementación proporcionados, puede agregar otras características avanzadas de VPN para mejorar la seguridad y disponibilidad de la conexión VPN. Por ejemplo, estos componentes pueden ayudar a asegurarse de que el cliente de conexión es correcto antes de permitir una conexión.


## <a name="high-availability"></a>Alta disponibilidad

Estos son opciones adicionales para alta disponibilidad.


|Opción  |Descripción  |
|---------|---------|
|Resistencia de servidor y el equilibrio de carga     |En entornos que requieren alta disponibilidad o la compatibilidad con gran número de solicitudes, puede aumentar el rendimiento y la resistencia de acceso remoto mediante el uso de equilibrio de carga entre varios servidores que se ejecuta el servidor de directivas de redes (NPS) y habilitar remotas Clústeres de servidor de acceso.<p>Documentos relacionados:<ul><li>[Equilibrio de carga del servidor NPS Proxy](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Implementar acceso remoto en un clúster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Elasticidad de sitio geográfica     |Para la geolocalización basada en IP, puede usar el Administrador de tráfico Global con DNS en Windows Server 2016. Equilibrio de carga geográfico más sólido, puede usar soluciones globales equilibrio de carga de servidor, como Microsoft Azure Traffic Manager.<p>Documentos relacionados:<ul><li>[Introducción al administrador de tráfico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## <a name="advanced-authentication"></a>Autenticación avanzada

Estos son opciones adicionales para la autenticación.


|Opción  |Descripción  |
|---------|---------|
|Windows Hello para empresas     |En Windows 10, Windows Hello para empresas reemplaza las contraseñas por la autenticación sólida en dos fases, tanto en equipos como en dispositivos móviles. Esta autenticación consta de un nuevo tipo de credencial de usuario que está asociado a un dispositivo y usa un dispositivo biométrico o número de identificación Personal (PIN).<p>El cliente de VPN de Windows 10 es compatible con Windows Hello para empresas. Después de que el usuario inicia sesión con un gesto, la conexión VPN usa el Windows Hello para certificado de empresa para la autenticación basada en certificados.<p>Documentos relacionados:<ul><li>[Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Caso práctico técnico: [Habilitación del acceso remoto con Windows Hello for Business en Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure Multi-factor Authentication (MFA)     |MFA de Azure tiene en la nube y en las versiones que pueden integrarse con el mecanismo de autenticación de VPN de Windows locales.<p>Para obtener más información sobre cómo funciona este mecanismo, vea [autenticación integrar RADIUS con servidor Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

---

## <a name="advanced-vpn-features"></a>Características avanzadas de VPN

Siguiente es opciones adicionales para características avanzadas.


|Opción  |Descripción  |
|---------|---------|
|Filtrado de tráfico     |Si desea exigir que las aplicaciones de VPN que pueden tener acceso los clientes, puede habilitar filtros de tráfico VPN.<p>Para obtener más información, consulte [características de seguridad de la VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN activada por aplicaciones     |Puede configurar perfiles de VPN para conectarse automáticamente al inician determinadas aplicaciones o los tipos de aplicaciones.<p>Para obtener más información sobre esta y otras opciones de activación, vea [opciones de VPN desencadenada automáticamente perfil](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Acceso condicional de VPN   |Cumplimiento de dispositivo y de acceso condicional puede requerir que los dispositivos administrados para cumplir los estándares antes de poder conectarse a la VPN. Una de las características avanzadas para el acceso condicional de VPN permite restringir las conexiones VPN a solo aquellos donde el certificado de autenticación de cliente contiene la "OID de acceso condicional de AAD de ' 1.3.6.1.4.1.311.87'.<p>Para restringir las conexiones VPN, deberá:<ol><li>En el servidor NPS, abra el **servidor de directivas de red** complemento.</li><li>Expanda **directivas** > **las directivas de red**.</li><li>Haga clic en el **las conexiones de red privada Virtual (VPN)** directiva de red y seleccione **propiedades**.</li><li>Seleccione el **configuración** ficha.</li><li>Seleccione **específico del proveedor** y haga clic en **agregar**.</li><li>Seleccione el **OID del certificado permitidos** opción y, a continuación, haga clic en **agregar**.</li><li>Pegue el OID de acceso condicional de AAD de **1.3.6.1.4.1.311.87** como valor de atributo y, a continuación, haga clic en **Aceptar** dos veces.</li><li>Haga clic en **cerrar** y, a continuación, **aplicar**.<p>Ahora cuando los clientes VPN intentan conectarse con cualquier certificado que no sea el certificado de corta duración en la nube, la conexión se producirá un error.</li></ol>Para obtener más información sobre el acceso condicional, consulte [VPN y el acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |

---


## <a name="additional-protection"></a>Protección adicional

**Atestación de clave de plataforma segura (TPM) del módulo**<p>
Un certificado de usuario con una clave de atestación de TPM ofrece la mayor garantía de seguridad, una copia de seguridad no exportabilidad, repetición anti y el aislamiento de claves proporcionado por el TPM.

Para obtener más información acerca de la atestación de clave de TPM en Windows 10, consulte [atestación de clave de TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Paso siguiente
[Comience a planear la implementación de VPN de Always On](always-on-vpn-deploy-planning.md): Antes de instalar el rol de servidor de acceso remoto en el equipo que tiene previsto usar como un servidor VPN, realice las tareas siguientes. Después de la planeación adecuada, puede implementar la VPN de Always On y, opcionalmente, configure el acceso condicional para la conectividad VPN con Azure AD.  

---

## <a name="related-topics"></a>Temas relacionados
- [Carga del servidor Proxy NPS equilibrio](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): Clientes remotos del servicio de autenticación telefónica de usuario (RADIUS), que son servidores de acceso de red, como servidores de red privada virtual (VPN) y puntos de acceso inalámbrico, crean solicitudes de conexión y los envían a servidores RADIUS como NPS. En algunos casos, un servidor NPS podría recibir demasiadas solicitudes de conexión al mismo tiempo, lo que resulta en un rendimiento degradado o una sobrecarga.

- [Introducción al administrador de tráfico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): En este tema se proporciona información general de Azure Traffic Manager, que le permite controlar la distribución del tráfico de usuario para los puntos de conexión de servicio. Traffic Manager usa el sistema de nombres de dominio (DNS) para dirigir las solicitudes de cliente al punto de conexión más adecuado en función de un método de enrutamiento de tráfico y el estado de los puntos de conexión. 

- [Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): Este tema proporcionan los requisitos previos, como las implementaciones de solo en la nube e implementaciones híbridas.  Este tema también enumeran las preguntas más frecuentes sobre Windows Hello para empresas.

- [Caso práctico técnico: Habilitación del acceso remoto con Windows Hello for Business en Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): En este caso práctico técnico aprenderá cómo Microsoft implementa el acceso remoto con Windows Hello para empresas.  Windows Hello para empresas es un enfoque de autenticación basada en certificados para las organizaciones y consumidores que va más allá de las contraseñas o la clave pública/privada. Esta forma de autenticación se basa en las credenciales de par de claves que se pueden reemplazar a las contraseñas y que resisten las infracciones, los robos y suplantación de identidad. 

- [Integración de la autenticación RADIUS con servidor Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Este tema le guiará a través de agregar y configurar una autenticación de clientes RADIUS con servidor Azure Multi-factor Authentication. RADIUS es un protocolo estándar que se usa para aceptar las solicitudes de autenticación y procesar dichas solicitudes. El servidor Azure Multi-factor Authentication puede actuar como un servidor RADIUS. 

- [Características de seguridad de la VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): En este tema proporciona directrices seguridad VPN para LockDown VPN, la integración de Windows Information Protection (WIP) con VPN y los filtros de tráfico. 

- [Opciones de VPN desencadenada automáticamente perfil](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): Este tema proporcionan las opciones de perfil de desencadenamiento automático VPN, como desencadenador de aplicación, según el nombre de desencadenador y Always On.

- [VPN y el acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): En este tema se proporciona información general de la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento de dispositivos para los clientes remotos. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD). 

- [Atestación de clave de TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): En este tema se proporciona información general del módulo de plataforma segura (TPM) y los pasos para implementar la atestación de clave de TPM. También puede encontrar información de solución de problemas y los pasos para resolver los problemas. 

---