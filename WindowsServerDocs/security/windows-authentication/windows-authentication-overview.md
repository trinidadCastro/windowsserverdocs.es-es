---
title: Información general de la autenticación de Windows
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2139a6b3a2b22bfe629df7ed073e16eabe31cac2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861718"
---
# <a name="windows-authentication-overview"></a>Información general de la autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema sobre navegación para profesionales de TI, se enumeran los recursos de documentación para las tecnologías de inicio de sesión y autenticación de Windows, que incluyen la evaluación del producto, guías de tareas iniciales, procedimientos, guías de diseño e implementación, referencias técnicas y referencias de comandos.

## <a name="feature-description"></a>Descripción de la característica
La autenticación es un proceso que comprueba la identidad de un objeto, un servicio o una persona. Cuando se autentica un objeto, el objetivo es comprobar que el objeto sea auténtico. Cuando se autentica un servicio o una persona, el objetivo es comprobar que las credenciales presentadas sean auténticas.

En un contexto de redes, la autenticación es el acto de probar la identidad a una aplicación o recurso de red. Normalmente, la identidad se demuestra mediante una operación criptográfica que utiliza una clave que solo el usuario conoce como con la criptografía de clave pública o una clave compartida. La parte del servidor del intercambio de autenticación compara los datos firmados con una clave criptográfica conocida para validar el intento de autenticación.

Almacenar las claves criptográficas en una ubicación central segura hace que el proceso de autenticación sea escalable y fácil de mantener. Active Directory Domain Services es la tecnología predeterminada y recomendada para almacenar información de identidades \(incluidas las claves criptográficas que son las credenciales del usuario\). Se requiere Active Directory para implementaciones de NTLM y Kerberos predeterminadas.

Las técnicas de autenticación abarcan desde un inicio de sesión sencillo, que identifica a los usuarios en función de algo que solo el usuario conoce (como una contraseña) hasta mecanismos de seguridad más eficaces que usan algo que el usuario tiene como tokens, certificados de clave pública y biometría. En un entorno empresarial, los servicios o los usuarios podrían acceder a varias aplicaciones o recursos en diversos tipos de servidores dentro de una sola ubicación o en distintas ubicaciones. Por dichos motivos, la autenticación debe admitir entornos para otras plataformas y para otros sistemas operativos de Windows.

El sistema operativo Windows implementa un conjunto predeterminado de protocolos de autenticación, como Kerberos, NTLM, seguridad de la capa de transporte\/Capa de sockets seguros \(TLS\/SSL\)y Digest, como parte de una arquitectura extensible. Además, algunos protocolos se combinan en paquetes de autenticación como Negotiate y el Proveedor de soporte técnico de seguridad de credenciales. Estos protocolos y paquetes permiten la autenticación de usuarios, equipos y servicios. El proceso de autenticación, a su vez, permite que los usuarios y servicios autorizados obtengan acceso a recursos de un modo seguro.

Para obtener más información acerca de Autenticación de Windows que incluye

-   [Conceptos de autenticación de Windows](windows-authentication-concepts.md)

-   [Escenarios de inicio de sesión de Windows](windows-logon-scenarios.md)

-   [Arquitectura de autenticación de Windows](windows-authentication-architecture.md)

-   [Security Support Provider Interface Architecture](security-support-provider-interface-architecture.md)

-   [Procesos de las credenciales en la autenticación de Windows](credentials-processes-in-windows-authentication.md)

-   [Configuración de directiva de grupo usada en la autenticación de Windows](group-policy-settings-used-in-windows-authentication.md)

consulte [información técnica de autenticación de Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Aplicaciones prácticas
Se usa Autenticación de Windows para comprobar que la información provenga de una fuente de confianza, ya sea de una persona o un objeto de equipo, como otro equipo. Windows proporciona muchos métodos diferentes para alcanzar este objetivo como se describe a continuación.

|Para...|Característica|Descripción|
|----|------|--------|
|Autenticación dentro de un dominio de Active Directory|Kerberos|Los sistemas operativos de Microsoft Windows&nbsp;Server implementan el protocolo de autenticación Kerberos versión 5 y las extensiones para la autenticación de clave pública. El cliente de autenticación Kerberos se implementa como un proveedor de compatibilidad para seguridad \(SSP\) y se puede obtener acceso a él a través de la interfaz del proveedor de compatibilidad de seguridad \(\)SSPI. La autenticación inicial de usuario se integra con el inicio de sesión único de Winlogon\-en la arquitectura. El Centro de distribución de claves Kerberos \(KDC\) está integrado con otros servicios de seguridad de Windows Server que se ejecutan en el controlador de dominio. El KDC usa la base de datos del servicio de directorio de Active Directory del dominio como base de datos de cuentas de seguridad. Se requiere Active Directory para implementaciones de Kerberos predeterminadas.<p>Para obtener recursos adicionales, consulte [Información general de la autenticación Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticación segura en la red|TLS\/SSL tal como se implementó en el proveedor de compatibilidad con seguridad de Schannel|El protocolo de seguridad de la capa de transporte \(TLS\) las versiones 1,0, 1,1 y 1,2, Capa de sockets seguros \(protocolo SSL\), las versiones 2,0 y 3,0, el protocolo de seguridad de la capa de transporte de datagramas, versión 1,0, y el transporte de comunicaciones privadas \(protocolo PCT\), versión 1,0, se basan en la criptografía de clave pública. El conjunto de protocolos de autenticación del proveedor de canal seguro \(Schannel\) proporciona estos protocolos. Todos los protocolos de Schannel usan un modelo de cliente y servidor.<p>Para obtener recursos adicionales, consulte [información general &#40;sobre TLS&#41; -SSL Schannel SSP](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticación para una aplicación o un servicio web|Autenticación de Windows integrada<p>Autenticación implícita|Para obtener recursos adicionales, vea autenticación de [Windows integrada](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx) y autenticación [implícita](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx)y [autenticación implícita avanzada](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticación para aplicaciones heredadas|NTLM|NTLM es un desafío\-protocolo de autenticación de estilo de respuesta. Además de la autenticación, el protocolo NTLM proporciona opcionalmente la seguridad de la sesión, específicamente la integridad y confidencialidad de los mensajes a través de funciones de firma y sellado en NTLM.<p>Para obtener recursos adicionales, consulte [Información general de NTLM](../kerberos/ntlm-overview.md).|
|Uso de la autenticación multifactor|Autorización mediante tarjeta inteligente<p>Compatibilidad biométrica|Las tarjetas inteligentes son una manipulación\-resistente y portátil para proporcionar soluciones de seguridad para tareas como la autenticación de clientes, el inicio de sesión en dominios, la firma de código y la protección de e\-mail.<p>La biométrica depende de la medición de una característica física inalterable de una persona para identificar únicamente a esa persona. Las huellas digitales son una de las características biométricas más utilizadas, ya que se cuenta con millones de dispositivos biométricos que captan huellas digitales integrados a equipos personales y periféricos.<p>Para obtener recursos adicionales, consulte la [referencia técnica de la tarjeta inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Proporcionar administración, almacenamiento y reutilización locales de credenciales|Administración de credenciales<p>Autoridad de seguridad local<p>Contraseñas|La administración de credenciales en Windows se asegura de que las credenciales se almacenen de forma segura. Las credenciales se recopilan en el escritorio seguro \(para el acceso local o de dominio\), a través de aplicaciones o sitios web, para que se presenten las credenciales correctas cada vez que se tenga acceso a un recurso.<p>
|Extensión de la protección de la autenticación moderna a sistemas heredados|Protección extendida para autenticación|Esta característica mejora la protección y el control de las credenciales al autenticar las conexiones de red mediante la autenticación integrada de Windows \(IWA\).|

## <a name="software-requirements"></a>Requisitos de software
El diseño de Autenticación de Windows permite que sea compatible con versiones anteriores del sistema operativo de Windows. Sin embargo, las mejoras que se presentan con cada versión no se aplican necesariamente a las versiones anteriores. Consulte la documentación sobre características específicas para obtener más información.

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor
Muchas características de autenticación pueden configurarse con la Directiva de grupo, que se puede instalar con Administrador del servidor. La característica Marco biométrico de Windows se instala con el Administrador del servidor. Otros roles de servidor que dependen de métodos de autenticación, como servidor Web \(IIS\) y Active Directory Domain Services, también se pueden instalar con Administrador del servidor.

## <a name="related-resources"></a>Recursos relacionados

|Tecnologías de autenticación|Recursos|
|----------------|-------|
|Autenticación de Windows|[Información técnica sobre autenticación de Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Incluye temas que tratan las diferencias entre versiones, conceptos generales de autenticación, escenarios de inicio de sesión, arquitecturas para versiones compatibles y configuraciones aplicables.|
|Kerberos|[Información general sobre la autenticación Kerberos](../kerberos/kerberos-authentication-overview.md)<p>[Introducción a la delegación restringida de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<p>[Referencia técnica de autenticación Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<p>[Guía de supervivencia de Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet wiki\)|
|Proveedor de compatibilidad con seguridad Schannel\/SSL y DTLS \(Schannel\)|[Información general de &#40;TLS-&#41; SSL Schannel SSP](../tls/tls-ssl-schannel-ssp-overview.md)<p>[Referencia técnica del proveedor de compatibilidad con seguridad de Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticación implícita|[Referencia técnica de autenticación implícita](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Información general de NTLM](../kerberos/ntlm-overview.md)<br />Contiene vínculos a recursos actuales y pasados|
|PKU2U|[Introducción a PKU2U en Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Tarjeta inteligente|[Referencia técnica de la tarjeta inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<p>
|Credenciales|[Protección y administración de credenciales](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contiene vínculos a recursos actuales y pasados<p>[Información general sobre contraseñas](../kerberos/passwords-overview.md)<br />Contiene vínculos a recursos actuales y pasados|


