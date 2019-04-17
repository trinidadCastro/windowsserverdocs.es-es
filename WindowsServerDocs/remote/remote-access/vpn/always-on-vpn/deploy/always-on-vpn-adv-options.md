---
title: Características avanzadas de VPN de Always On
description: Más allá de escenario de implementación que se proporcionan en esta implementación, puedes agregar otras características avanzadas de VPN para mejorar la seguridad y la disponibilidad de la conexión VPN.
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067540"
---
# Características avanzadas de VPN siempre activada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** más información sobre la tecnología de VPN siempre activada](../always-on-vpn-technology-overview.md)<br>
& #187; [ **Siguiente:** empezar a planificar la implementación de VPN siempre activada](always-on-vpn-deploy-planning.md)

Más allá de los escenarios de implementación proporcionaras, puedes agregar otras características avanzadas de VPN para mejorar la seguridad y la disponibilidad de la conexión VPN. Por ejemplo, estos componentes pueden ayudar a garantizar que el cliente que se conecta es correcto antes de permitir que una conexión.


## Alta disponibilidad

Siguientes son las opciones adicionales para alta disponibilidad.


|Opción  |Descripción  |
|---------|---------|
|Resistencia de servidor y equilibrio de carga     |En entornos que requieren alta disponibilidad o compatibilidad con un gran número de solicitudes, puede aumentar el rendimiento y la resistencia de acceso remoto mediante el uso de equilibrio de carga entre varios servidores que están ejecutando el servidor de directivas de redes (NPS) y habilitar remota Servidor de acceso de clústeres por error.<p>Documentos relacionados:<ul><li>[Equilibrio de carga de servidor en el Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Implementación del acceso remoto en un clúster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Resistencia de sitios geográfica     |Para la ubicación geográfica basados en IP, puedes usar el administrador Global de tráfico con DNS en Windows Server 2016. Equilibrio de carga geográfica más sólida, puedes usar soluciones Global equilibrio de carga de servidor, por ejemplo, el Administrador de tráfico de Azure de Microsoft.<p>Documentos relacionados:<ul><li>[Introducción al administrador de tráfico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Administrador de tráfico de Microsoft Azure](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## Autenticación avanzada

Siguientes son las opciones adicionales para la autenticación.


|Opción  |Descripción  |
|---------|---------|
|Windows Hello para empresas     |En Windows 10, Windows Hello para empresas reemplaza las contraseñas por la autenticación segura en dos fases, tanto en equipos como en dispositivos móviles. Esta autenticación consiste en un nuevo tipo de número de identificación Personal (PIN) o las credenciales de usuario que está vinculada a un dispositivo y usa un dispositivo biométrico.<p>El cliente VPN de Windows 10 es compatible con Windows Hello para empresas. Después de que el usuario inicia sesión con un gesto, la conexión VPN usa Windows Hello para el certificado de empresa para la autenticación basada en certificados.<p>Documentos relacionados:<ul><li>[Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Caso práctico técnico: [Permitir el acceso remoto con Windows Hello para empresas en Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure Multifactor Authentication (MFA)     |Azure MFA tiene en la nube y local versiones que se pueden integrar con el mecanismo de autenticación de VPN de Windows.<p>Para obtener más información sobre cómo funciona este mecanismo, consulta [RADIUS integrar la autenticación con el servidor Multi-factor Authentication de Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

---

## Características avanzadas de VPN

Siguientes son las opciones adicionales para las características avanzadas.


|Opción  |Descripción  |
|---------|---------|
|Filtrado de tráfico     |Si es necesario aplicar las aplicaciones que VPN que pueden tener acceso los clientes, puedes habilitar filtros de tráfico VPN.<p>Para obtener más información, consulta [las características de seguridad VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN activadas por aplicaciones     |Puedes configurar perfiles VPN para conectarse automáticamente al inician determinadas aplicaciones o los tipos de aplicaciones.<p>Para obtener más información acerca de esta y otras opciones de activación, consulta [Opciones desencadenadas automáticamente de perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Acceso condicional de VPN   |Cumplimiento de dispositivo y acceso condicional puede requerir que los dispositivos administrados para cumplir los estándares antes de poder conectarse a la VPN. Una de las características avanzadas para el acceso condicional de VPN permite restringir las conexiones VPN solo aquellas donde el certificado de autenticación de cliente contiene 'De acceso condicional de AAD OID de ' 1.3.6.1.4.1.311.87'.<p>Para restringir las conexiones VPN, debes:<ol><li>En el servidor NPS, abre el complemento de **Servidor de directivas de red** .</li><li>Expanda **directivas** > **las directivas de red**.</li><li>Haz clic en la directiva de red de **las conexiones de red privada Virtual (VPN)** y selecciona **Propiedades**.</li><li>Selecciona la pestaña **configuración** .</li><li>Selecciona **Específico de proveedor** y haz clic en **Agregar**.</li><li>Selecciona la opción de **OID de certificado de permitidas** y, a continuación, haz clic en **Agregar**.</li><li>Pegar el OID de acceso condicional de AAD de **1.3.6.1.4.1.311.87** como el valor del atributo y, a continuación, haz clic en **Aceptar** dos veces.</li><li>Haz clic en **Cerrar** y, a continuación, en **Aplicar**.<p>Ahora cuando los clientes VPN intentan conectarse con cualquier certificado distinto del certificado de corta duración en la nube, la conexión se producirá un error.</li></ol>Para obtener más información sobre el acceso condicional, consulta la [VPN y acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |

---


## Protección adicional

**Atestación de clave de plataforma segura (TPM) del módulo**<p>
Un certificado de usuario con una clave de atestación para TPM proporciona mayor garantía de seguridad, copias de seguridad mediante no exportabilidad, repetición y el aislamiento de claves proporcionada por el TPM.

Para obtener más información sobre la atestación de clave de TPM en Windows 10, consulta la [Atestación de clave de TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## Paso siguiente
[Empezar a planificar la implementación de VPN siempre activada](always-on-vpn-deploy-planning.md): antes de instalar el rol de servidor de acceso remoto en el equipo que tienes previsto sobre el uso de un servidor VPN, realizar las siguientes tareas. Después de una planificación adecuada, puedes implementar VPN siempre activada y, opcionalmente, configurar el acceso condicional para la conectividad VPN con Azure AD.  

---

## Temas relacionados
- [Equilibrio de carga de servidor Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): los clientes de servicio de autenticación remota telefónica de usuario (RADIUS), que son los servidores de acceso de red, como servidores de red privada virtual (VPN) y puntos de acceso inalámbrico, crear solicitudes de conexión y enviarlos a RADIUS servidores, como NPS. En algunos casos, un servidor NPS posible que recibas demasiadas solicitudes de conexión a la vez, lo que provoca que disminución del rendimiento o una sobrecarga.

- [Información general del Administrador de tráfico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): en este tema proporciona una visión general de Azure tráfico Manager, que te permite controlar la distribución de tráfico de usuario de puntos de conexión de servicio. Administrador de tráfico, se usa el sistema de nombres de dominio (DNS) para dirigir las solicitudes de cliente para el punto de conexión más adecuado en función de un método de enrutamiento de tráfico y el estado de los puntos de conexión. 

- [Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): en este tema proporciona los requisitos previos, por ejemplo, las implementaciones de solo en la nube e implementaciones híbridas.  Este tema también enumeran las preguntas más frecuentes sobre Windows Hello para empresas.

- [Técnico caso práctico: habilitar el acceso remoto con Windows Hello para empresas en Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): en este caso práctico se muestra cómo Microsoft implementa acceso remoto con Windows Hello para empresas.  Windows Hello para empresas es una clave pública y privada o el método de autenticación basada en certificados para las organizaciones y los consumidores que va más allá de las contraseñas. Este tipo de autenticación se basa en las credenciales de un par de claves que pueden reemplazar contraseñas y resistente a las infracciones, robos y suplantación de identidad. 

- [Autenticación de RADIUS integrar con el servidor Multi-factor Authentication de Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): este tema te guiará a través de agregar y configurar una autenticación de cliente de radio con el servidor Multi-factor Authentication de Azure. Es un protocolo estándar para aceptar solicitudes de autenticación y procesar las solicitudes. El servidor de autenticación multifactor de Azure puede actuar como servidor RADIUS. 

- [Las características de seguridad VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): este tema proporciona instrucciones de seguridad VPN para LockDown VPN, integración de Windows Information Protection (WIP) con VPN, y filtros de tráfico. 

- [Opciones desencadenadas automáticamente de perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): en este tema se proporciona opciones de desencadenadas automáticamente de perfil VPN, como desencadenador de aplicaciones, el desencadenador basado en nombre y siempre activado.

- [VPN y acceso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): en este tema se proporciona una visión general de la plataforma de acceso condicional basado en la nube para proporcionar una opción de cumplimiento del dispositivo para los clientes remotos. Acceso condicional es un motor de evaluación basado en directivas que te permite crear reglas de acceso para cualquier aplicación conectada de Azure Active Directory (Azure AD). 

- [Atestación de clave de TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): en este tema se proporciona una descripción general del módulo de plataforma segura (TPM) y atestación de clave de pasos para implementar el TPM. También encontrarás información de solución de problemas y pasos para resolver problemas. 

---