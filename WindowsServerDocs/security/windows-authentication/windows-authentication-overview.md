---
title: "Introducción a la autenticación de Windows"
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
ms.openlocfilehash: c2ec55ed6b09628d1d80a24be766259980e84d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-overview"></a>Introducción a la autenticación de Windows

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de exploración para profesionales de TI enumeran los recursos de documentación para las tecnologías de inicio de sesión y autenticación de Windows que incluyen la evaluación del producto, obtener guías de introducción, procedimientos, guías de diseño e implementación, técnicos y referencias de comandos.

## <a name="feature-description"></a>Descripción de la característica
La autenticación es un proceso para comprobar la identidad de un objeto, el servicio o la persona. Cuando se autentica con un objeto, el objetivo es comprobar que el objeto es original. Al autenticar un servicio o una persona, el objetivo es comprobar que las credenciales presentadas están auténticas.

En un contexto de la red, la autenticación es el acto de demostrar la identidad para una aplicación de red o un recurso. Por lo general, la identidad es comprobada por una operación criptográfica que use cualquier una clave que solo el usuario sabe - como con la criptografía de clave pública - o una clave compartida. El lado del servidor de intercambio de autenticación compara los datos firmados con una clave criptográfica conocida para validar el intento de autenticación.

Almacenar las claves criptográficas en una ubicación centralizada segura hace que el proceso de autenticación escalable y fácil de mantener. Los servicios de dominio de Active Directory es la recomendada y la tecnología de forma predeterminada para almacenar información de identidad \ (incluidas las claves criptográficas credentials\ del usuario). Active Directory es necesario para implementaciones de Kerberos y NTLM de forma predeterminada.

Técnicas de autenticación abarcan desde un inicio de sesión sencilla que identifica a los usuarios en función de algo que solo el usuario sabe - como una contraseña, los mecanismos de seguridad más eficaces que usan algo que el usuario tiene - como tokens, certificados de clave pública y la biométrica. En un entorno empresarial, servicios o los usuarios pueden acceder a varias aplicaciones o los recursos en muchos tipos de servidores en una única ubicación o entre varias ubicaciones. Por estas razones, la autenticación debe admitir entornos para otras plataformas y otros sistemas operativos Windows.

El sistema operativo Windows implementa un conjunto predeterminado de protocolos de autenticación, como Kerberos, NTLM, \(TLS\/SSL\) capa de transporte Security\/Secure capa de Sockets y Digest, como parte de una arquitectura extensible. Además, algunos protocolos se combinan en paquetes de autenticación como Negotiate y el proveedor de soporte técnico de seguridad de credenciales. Estos protocolos y paquetes de habilitar la autenticación de usuarios, equipos y servicios; el proceso de autenticación, a su vez, permite que los usuarios autorizados y servicios acceder a recursos de manera segura.

Para obtener más información acerca de cómo incluir de la autenticación de Windows

-   [Conceptos de autenticación de Windows](windows-authentication-concepts.md)

-   [Escenarios de inicio de sesión de Windows](windows-logon-scenarios.md)

-   [Arquitectura de autenticación de Windows](windows-authentication-architecture.md)

-   [Arquitectura de interfaz de proveedor de soporte técnico de seguridad](security-support-provider-interface-architecture.md)

-   [Procesos de credenciales de autenticación de Windows](credentials-processes-in-windows-authentication.md)

-   [Configuración de directiva de grupo que usa autenticación de Windows](group-policy-settings-used-in-windows-authentication.md)

Consulta la [Introducción técnica de autenticación de Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Aplicaciones prácticas
Autenticación de Windows se utiliza para comprobar que la información proviene de un origen de confianza, tanto desde un objeto persona o el equipo, como otro equipo. Windows proporciona muchos métodos diferentes para lograr este objetivo, tal como se describe a continuación.

|Para...|Característica|Descripción|
|----|------|--------|
|Autenticar en un dominio de Active Directory|Kerberos|Microsoft Windows&nbsp;sistemas operativos de servidor implemente el protocolo de autenticación de Kerberos V5 y extensiones para la autenticación de clave pública. El cliente de autenticación de Kerberos se implementa como un proveedor de soporte técnico de seguridad \(SSP\) y se puede acceder a través de la interfaz del proveedor de soporte técnico de seguridad \(SSPI\). Autenticación de usuario inicial se integra con la arquitectura Winlogon solo en sign\. El \(KDC\) Centro de distribución de claves Kerberos se integra con otros servicios de seguridad de Windows Server ejecutando en el controlador de dominio. KDC usa la base de datos de servicio de directorio de Active Directory del dominio como su base de datos de cuentas de seguridad. Active Directory es necesario para las implementaciones de Kerberos predeterminado.<br /><br />Para obtener recursos adicionales, consulta [Introducción a la autenticación Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticación segura en la web|Según cómo esté implementado en el proveedor de seguridad Schannel TLS\/SSL|La seguridad de la capa de transporte \(TLS\) protocolo versiones 1.0, 1.1 y 1.2, el protocolo de capa de Sockets seguros \(SSL\), versión del protocolo de las versiones 2.0 y 3.0, seguridad de capa de transporte de datagramas 1.0 y el protocolo de transporte de comunicaciones privadas \(PCT\), versión 1.0, se basan en criptografía de clave pública. El conjunto de protocolos de autenticación de canal seguro \(Schannel\) proveedor proporciona estos protocolos. Todos los protocolos de Schannel utilizan un modelo de cliente y servidor.<br /><br />Para obtener recursos adicionales, consulta [TLS - SSL & #40; Schannel SSP & #41; Introducción a](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticar a un servicio web o una aplicación|Autenticación integrada de Windows<br /><br />Autenticación implícita|Para obtener recursos adicionales, consulta [autenticación de Windows integrado] (https://technet.microsoft.com/library/cc758557(v=WS.10.aspx and [Digest Authentication](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), and [Advanced Digest Authentication](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticar en las aplicaciones heredadas|NTLM|NTLM es una adición de autenticación protocol.In estilo challenge\ respuesta a la autenticación, el protocolo NTLM, opcionalmente, se proporciona por motivos de seguridad de sesión: específicamente la integridad de los mensajes y la confidencialidad a través de la firma y el sellado funciones de NTLM.<br /><br />Para obtener recursos adicionales, consulta [NTLM Introducción](../kerberos/ntlm-overview.md).|
|Autenticación multifactor Aproveche|Compatibilidad con tarjetas inteligentes<br /><br />Compatibilidad con la biométrica|Las tarjetas inteligentes son una manera tamper\ resistente y portátil para proporcionar soluciones de seguridad para tareas como la autenticación de cliente, inicia sesión en dominios, firma de código y proteger e\ correo.<br /><br />Biométrica se basa en la medición de una característica que no cambian física de una persona para identificar de esa persona. Huellas digitales son una de las características biométricas usadas con más frecuencia, con millones de dispositivos biométricos de huellas digitales que se incrustan en equipos personales y periféricos.<br /><br />Para obtener recursos adicionales, consulta [referencia técnica de tarjeta inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Proporcionar administración local, el almacenamiento y la reutilización de credenciales|Administración de credenciales<br /><br />Autoridad de seguridad local<br /><br />Contraseñas|Administración de credenciales de Windows garantiza que las credenciales se almacenan de forma segura. Se recopilan credenciales en el escritorio seguro \ (para el dominio o local texto\) a través de aplicaciones o sitios Web para que las credenciales correctas se presentan cada vez un recurso se tiene acceso.<br /><br />
|Ampliar la protección de autenticación modernos para los sistemas heredados|Protección extendida para la autenticación|Esta característica mejora la protección y administración de credenciales al autenticar las conexiones de red mediante el uso de autenticación de Windows integrada \(IWA\).|

## <a name="software-requirements"></a>Requisitos de software
Autenticación de Windows está diseñada para ser compatible con versiones anteriores del sistema operativo Windows. Sin embargo, mejoras con cada lanzamiento no son necesariamente aplicables a las versiones anteriores. Consulta la documentación sobre características específicas para obtener más información.

## <a name="server-manager-information"></a>Información del administrador del servidor
Muchas características de la autenticación pueden configurarse mediante Directiva de grupo, que se pueden instalar con el administrador del servidor. La característica de marco biométrico de Windows se instala con el administrador del servidor. Otros roles de servidor que dependen de los métodos de autenticación, como \(IIS\) servidor Web y servicios de dominio de Active Directory, también pueden instalarse con el administrador del servidor.

## <a name="related-resources"></a>Recursos relacionados

|Tecnologías de autenticación|Recursos|
|----------------|-------|
|Autenticación de Windows|[Introducción técnica de autenticación de Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Incluye temas de ocuparse de las diferencias entre versiones, conceptos generales de autenticación, los escenarios de inicio de sesión, arquitecturas para las versiones compatibles y la configuración correspondiente.|
|Kerberos|[Introducción a la autenticación Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Información general de la delegación restringida de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Referencia técnica de autenticación de Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Guía de subsistencia de Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL y DTLS \ (provider\ de soporte técnico de seguridad Schannel)|[TLS - SSL & #40; Schannel SSP & #41; Introducción](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Referencia técnica del proveedor de soporte técnico seguridad Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticación implícita|[Descubra referencia técnica de autenticación](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Introducción a NTLM](../kerberos/ntlm-overview.md)<br />Contiene vínculos a recursos actuales y anteriores|
|PKU2U|[Introducción a PKU2U en Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Tarjeta inteligente|[Referencia técnica de tarjeta inteligente](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenciales|[Administración y protección de credenciales](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contiene vínculos a recursos actuales y anteriores<br /><br />[Introducción a las contraseñas](../kerberos/passwords-overview.md)<br />Contiene vínculos a recursos actuales y anteriores|


