---
title: Información general de la autenticación de Windows
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2021ccf1e0015cc910f43966f9400e6bd56a230c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870976"
---
# <a name="windows-authentication-overview"></a>Información general de la autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema sobre navegación para profesionales de TI, se enumeran los recursos de documentación para las tecnologías de inicio de sesión y autenticación de Windows, que incluyen la evaluación del producto, guías de tareas iniciales, procedimientos, guías de diseño e implementación, referencias técnicas y referencias de comandos.

## <a name="feature-description"></a>Descripción de la característica
La autenticación es un proceso que comprueba la identidad de un objeto, un servicio o una persona. Cuando se autentica un objeto, el objetivo es comprobar que el objeto sea auténtico. Cuando se autentica un servicio o una persona, el objetivo es comprobar que las credenciales presentadas sean auténticas.

En un contexto de redes, la autenticación es el acto de probar la identidad a una aplicación o recurso de red. Normalmente, la identidad se demuestra mediante una operación de cifrado que utiliza una clave que solo el usuario conoce: al igual que con criptografía de clave pública - o una clave compartida. La parte del servidor del intercambio de autenticación compara los datos firmados con una clave criptográfica conocida para validar el intento de autenticación.

Almacenar las claves criptográficas en una ubicación central segura hace que el proceso de autenticación sea escalable y fácil de mantener. Servicios de dominio de Active Directory es la recomendada y la tecnología predeterminada para almacenar información de identidad \(incluidas las claves criptográficas que son las credenciales del usuario\). Se requiere Active Directory para implementaciones de NTLM y Kerberos predeterminadas.

Técnicas de autenticación van desde un inicio de sesión simple, que identifica a los usuarios en función de algo que solo el usuario conoce, como una contraseña a los mecanismos de seguridad más eficaces que emplean algo que tiene el usuario, como los tokens, certificados de clave pública, y biométrica. En un entorno empresarial, los servicios o los usuarios podrían acceder a varias aplicaciones o recursos en diversos tipos de servidores dentro de una sola ubicación o en distintas ubicaciones. Por dichos motivos, la autenticación debe admitir entornos para otras plataformas y para otros sistemas operativos de Windows.

El sistema operativo Windows implementa un conjunto predeterminado de protocolos de autenticación, como Kerberos, NTLM, Transport Layer Security\/Secure Sockets Layer \(TLS\/SSL\)e implícita, como parte de un arquitectura extensible. Además, algunos protocolos se combinan en paquetes de autenticación como Negotiate y el Proveedor de soporte técnico de seguridad de credenciales. Estos protocolos y paquetes permiten la autenticación de usuarios, equipos y servicios. El proceso de autenticación, a su vez, permite que los usuarios y servicios autorizados obtengan acceso a recursos de un modo seguro.

Para obtener más información acerca de Autenticación de Windows que incluye

-   [Conceptos de autenticación de Windows](windows-authentication-concepts.md)

-   [Escenarios de inicio de sesión de Windows](windows-logon-scenarios.md)

-   [Arquitectura de autenticación de Windows](windows-authentication-architecture.md)

-   [Arquitectura de interfaz de proveedor de soporte técnico de seguridad](security-support-provider-interface-architecture.md)

-   [Procesos de las credenciales de autenticación de Windows](credentials-processes-in-windows-authentication.md)

-   [Configuración de directiva de grupo utilizada en la autenticación de Windows](group-policy-settings-used-in-windows-authentication.md)

Consulte la [información técnica sobre la autenticación de Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Aplicaciones prácticas
Se usa Autenticación de Windows para comprobar que la información provenga de una fuente de confianza, ya sea de una persona o un objeto de equipo, como otro equipo. Windows proporciona muchos métodos diferentes para alcanzar este objetivo como se describe a continuación.

|Para...|Característica|Descripción|
|----|------|--------|
|Autenticación dentro de un dominio de Active Directory|Kerberos|El Windows Microsoft&nbsp;sistemas operativos de servidor implementan las extensiones de autenticación de clave pública y el protocolo de autenticación Kerberos versión 5. El cliente de autenticación de Kerberos se implementa como un proveedor de soporte técnico de seguridad \(SSP\) y puede accederse a través de la interfaz del proveedor de soporte técnico de seguridad \(SSPI\). Autenticación de usuario inicial se integra con el inicio de sesión único de Winlogon\-sobre la arquitectura. El centro de distribución de claves Kerberos \(KDC\) está integrado con otros servicios de seguridad de Windows Server que se ejecuta en el controlador de dominio. El KDC utiliza la base de datos de servicio de directorio de Active Directory del dominio como su base de datos de la cuenta de seguridad. Se requiere Active Directory para implementaciones de Kerberos predeterminadas.<br /><br />Para obtener recursos adicionales, consulte [Información general de la autenticación Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticación segura en la red|TLS\/SSL tal como está implementado en el proveedor de soporte técnico de seguridad de Schannel|La seguridad de la capa de transporte \(TLS\) las versiones 1.0, 1.1 y 1.2, capa de Sockets seguros del protocolo \(SSL\) protocol, Protocolo versión 1.0 de las versiones 2.0 y 3.0, seguridad de capa de transporte de datagrama y privado Transporte de comunicaciones \(PCT\) protocolo, versión 1.0, se basan en la criptografía de clave pública. El canal seguro \(Schannel\) conjunto de protocolos de autenticación de proveedor proporciona estos protocolos. Todos los protocolos de Schannel usan un modelo de cliente y servidor.<br /><br />Para obtener recursos adicionales, consulte [TLS / SSL &#40;Schannel SSP&#41; Introducción](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticación para una aplicación o un servicio web|Autenticación de Windows integrada<br /><br />Autenticación implícita|Para obtener recursos adicionales, consulte [autenticación integrada de Windows](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx) y [autenticación implícita](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), y [autenticación implícita avanzada](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticación para aplicaciones heredadas|NTLM|NTLM es un desafío\-protocolo de autenticación de estilo de respuesta. Además de la autenticación, el protocolo NTLM, opcionalmente, proporciona seguridad de sesión, específicamente integridad del mensaje y confidencialidad a través de la firma y sello de las funciones de NTLM.<br /><br />Para obtener recursos adicionales, consulte [Información general de NTLM](../kerberos/ntlm-overview.md).|
|Uso de la autenticación multifactor|Autorización mediante tarjeta inteligente<br /><br />Compatibilidad biométrica|Las tarjetas inteligentes son una alteración\-portátil y difícil de manera de proporcionar soluciones de seguridad para tareas como la autenticación de cliente, inicio de sesión en dominios, firma de código y protección e\-correo.<br /><br />La biométrica depende de la medición de una característica física inalterable de una persona para identificar únicamente a esa persona. Las huellas digitales son una de las características biométricas más utilizadas, ya que se cuenta con millones de dispositivos biométricos que captan huellas digitales integrados a equipos personales y periféricos.<br /><br />Para obtener recursos adicionales, consulte [referencia técnica de tarjeta inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Proporcionar administración, almacenamiento y reutilización locales de credenciales|Administración de credenciales<br /><br />Autoridad de seguridad local<br /><br />Contraseñas|La administración de credenciales en Windows se asegura de que las credenciales se almacenen de forma segura. Las credenciales se recopilan en el escritorio seguro \(para el acceso local o de dominio\), a través de las aplicaciones o sitios Web para que se presenten las credenciales correctas cada vez que se tiene acceso a un recurso.<br /><br />
|Extensión de la protección de la autenticación moderna a sistemas heredados|Protección extendida para autenticación|Esta característica mejora la protección y control de credenciales al autenticar las conexiones de red mediante el uso de la autenticación integrada de Windows \(IWA\).|

## <a name="software-requirements"></a>Requisitos de software
El diseño de Autenticación de Windows permite que sea compatible con versiones anteriores del sistema operativo de Windows. Sin embargo, las mejoras que se presentan con cada versión no se aplican necesariamente a las versiones anteriores. Consulte la documentación sobre características específicas para obtener más información.

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor
Muchas características de autenticación pueden configurarse con la Directiva de grupo, que se puede instalar con Administrador del servidor. La característica Marco biométrico de Windows se instala con el Administrador del servidor. Otros roles de servidor que dependen de los métodos de autenticación, como servidor Web \(IIS\) y servicios de dominio de Active Directory, también puede instalarse mediante el administrador del servidor.

## <a name="related-resources"></a>Recursos relacionados

|Tecnologías de autenticación|Recursos|
|----------------|-------|
|Autenticación de Windows.|[Introducción técnica a la autenticación de Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Incluye temas que se dirige a las diferencias entre versiones, conceptos generales de autenticación, escenarios de inicio de sesión, arquitecturas para versiones compatibles y la configuración aplicable.|
|Kerberos|[Introducción a la autenticación Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Introducción a la delegación restringida de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Referencia técnica de autenticación de Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Guía de supervivencia de Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(Wiki de TechNet\)|
|TLS\/SSL y DTLS \(proveedor de soporte técnico de seguridad de Schannel\)|[TLS / SSL &#40;SSP de Schannel&#41; información general](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Referencia técnica del proveedor de compatibilidad con seguridad Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticación implícita|[Referencia técnica de autenticación de texto implícita](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Información general de NTLM](../kerberos/ntlm-overview.md)<br />Contiene vínculos a recursos actuales y pasados|
|PKU2U|[Introducción a PKU2U en Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Tarjeta inteligente|[Referencia técnica de tarjeta inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenciales|[Protección y administración de credenciales](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contiene vínculos a recursos actuales y pasados<br /><br />[Información general de contraseñas](../kerberos/passwords-overview.md)<br />Contiene vínculos a recursos actuales y pasados|


