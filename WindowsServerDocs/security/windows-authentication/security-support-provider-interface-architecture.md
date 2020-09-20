---
title: Security Support Provider Interface Architecture
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: da7c390427a3f0f2348d91e14d0affef905db390
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766958"
---
# <a name="security-support-provider-interface-architecture"></a>Security Support Provider Interface Architecture

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de referencia para profesionales de TI se describen los protocolos de autenticación de Windows que se utilizan en la arquitectura de la interfaz del proveedor de compatibilidad para seguridad (SSPI).

La interfaz del proveedor de compatibilidad para seguridad (SSPI) de Microsoft es la base para la autenticación de Windows. Las aplicaciones y los servicios de infraestructura que requieren autenticación utilizan SSPI para proporcionarlos.

SSPI es la implementación de la API de servicio de seguridad genérico (GSSAPI) en los sistemas operativos Windows Server. Para obtener más información acerca de GSSAPI, consulte RFC 2743 y RFC 2744 en la base de datos de RFC de IETF.

Los proveedores de compatibilidad para seguridad (SSP) predeterminados que invocan protocolos de autenticación específicos en Windows se incorporan a la SSPI como archivos dll. Estos SSP predeterminados se describen en las secciones siguientes. Se pueden incorporar SSP adicionales si pueden funcionar con la SSPI.

Como se muestra en la siguiente imagen, la SSPI en Windows proporciona un mecanismo que transporta los tokens de autenticación a través del canal de comunicación existente entre el equipo cliente y el servidor. Cuando es necesario autenticar dos equipos o dispositivos para que puedan comunicarse de forma segura, las solicitudes de autenticación se enrutan a la SSPI, que completa el proceso de autenticación, independientemente del Protocolo de red actualmente en uso. SSPI devuelve objetos binarios grandes transparentes. Se pasan entre las aplicaciones, momento en el que se pueden pasar a la capa SSPI. Por lo tanto, la SSPI permite a una aplicación utilizar varios modelos de seguridad disponibles en un equipo o red sin cambiar la interfaz al sistema de seguridad.

![Diagrama que muestra la arquitectura de la interfaz del proveedor de compatibilidad con seguridad](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

En las secciones siguientes se describen los SSP predeterminados que interactúan con SSPI. Los SSP se usan de maneras diferentes en los sistemas operativos Windows para promover la comunicación segura en un entorno de red no seguro.

-   [Proveedor de compatibilidad con seguridad de Kerberos](#BKMK_KerbSSP)

-   [Proveedor de compatibilidad con seguridad NTLM](#BKMK_NTLMSSP)

-   [Proveedor de compatibilidad con seguridad implícita](#BKMK_DigestSSP)

-   [Proveedor de compatibilidad con seguridad de Schannel](#BKMK_SchannelSSP)

-   [Negociar proveedor de compatibilidad de seguridad](#BKMK_NegoSSP)

-   [Proveedor de compatibilidad con seguridad de credenciales](#BKMK_CredSSP)

-   [Negociar el proveedor de compatibilidad de seguridad de extensiones](#BKMK_NegoExtsSSP)

-   [Proveedor de compatibilidad con seguridad de PKU2U](#BKMK_PKU2USSP)

También se incluye en este tema:

[Selección del proveedor de compatibilidad con seguridad](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="kerberos-security-support-provider"></a><a name="BKMK_KerbSSP"></a>Proveedor de compatibilidad con seguridad de Kerberos
Este SSP solo usa el protocolo Kerberos versión 5, tal y como lo implementa Microsoft. Este protocolo se basa en las revisiones RFC 4120 y draft del grupo de trabajo de red. Es un protocolo estándar del sector que se usa con una contraseña o una tarjeta inteligente para un inicio de sesión interactivo. También es el método de autenticación preferido para los servicios de Windows.

Dado que el protocolo Kerberos es el protocolo de autenticación predeterminado desde Windows 2000, todos los servicios de dominio admiten el SSP de Kerberos. Estos servicios incluyen:

-   Active Directory las consultas que utilizan el Protocolo ligero de acceso a directorios (LDAP)

-   Administración remota de servidores o estaciones de trabajo que utiliza el servicio llamada a procedimiento remoto

-   Servicios de impresión

-   Autenticación cliente-servidor

-   Acceso remoto a archivos que usa el protocolo de bloque de mensajes del servidor (SMB) (también conocido como sistema de archivos de Internet común o CIFS)

-   Referencia y administración del sistema de archivos distribuido

-   Autenticación de la intranet en Internet Information Services (IIS)

-   Autenticación de la entidad de seguridad para el protocolo de seguridad de Internet (IPsec)

-   Solicitudes de certificado para Active Directory servicios de Certificate Server para usuarios y equipos del dominio

Ubicación:% WINDIR% \Windows\System32\kerberos.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a** al principio de este tema, más windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo Kerberos y el SSP de Kerberos**

-   [Microsoft Kerberos (Windows)](/windows/win32/secauthn/microsoft-kerberos)

-   [\[MS-KILE \] : extensiones de protocolo Kerberos](/openspecs/windows_protocols/ms-kile/2a32282e-dd48-4ad9-a542-609804b02cc9)

-   [\[MS-SFU \] : extensiones de protocolo Kerberos: especificación del Protocolo de delegación limitada y de servicio para el usuario](/openspecs/windows_protocols/ms-sfu/3bff5864-8135-400e-bdd9-33b552051d94)

-   [SSP/SSP de Kerberos (Windows)](/windows/win32/secauthn/kerberos-ssp-ap)

-   [Mejoras de Kerberos](/previous-versions/windows/it-pro/windows-vista/cc749438(v=ws.10)) para Windows Vista

-   [Cambios en la autenticación Kerberos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560670(v=ws.10)) para Windows 7

-   [Referencia técnica de autenticación Kerberos](/previous-versions/windows/it-pro/windows-server-2003/cc739058(v=ws.10))

### <a name="ntlm-security-support-provider"></a><a name="BKMK_NTLMSSP"></a>Proveedor de compatibilidad con seguridad NTLM
El proveedor de compatibilidad para seguridad NTLM (SSP de NTLM) es un protocolo de mensajería binaria que usa la interfaz del proveedor de compatibilidad para seguridad (SSPI) para permitir la autenticación NTLM Challenge-Response y negociar opciones de integridad y confidencialidad. NTLM se usa siempre que se use la autenticación SSPI, incluido el bloque de mensajes del servidor o la autenticación de CIFS, la autenticación HTTP Negotiate (por ejemplo, la autenticación Web de Internet) y el servicio llamada a procedimiento remoto. El SSP de NTLM incluye los protocolos de autenticación NTLM y NTLM versión 2 (NTLMv2).

Los sistemas operativos Windows admitidos pueden usar el SSP de NTLM para lo siguiente:

-   Autenticación de cliente/servidor

-   Servicios de impresión

-   Acceso a archivos mediante CIFS (SMB)

-   Servicio de llamada a procedimiento remoto seguro o servicio DCOM

Ubicación:% WINDIR% \Windows\System32\msv1_0.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a** al principio de este tema, más windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo NTLM y el SSP de NTLM**

-   [Paquete de autenticación de MSV1_0 (Windows)](/windows/win32/secauthn/msv1-0-authentication-package)

-   [Cambios en la autenticación NTLM](/previous-versions/windows/it-pro/windows-7/dd566199(v=ws.10)) en Windows 7

-   [Microsoft NTLM (Windows)](/windows/win32/secauthn/microsoft-ntlm)

-   [Guía de auditoría y restricción del uso de NTLM](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/jj865674(v=ws.10))

### <a name="digest-security-support-provider"></a><a name="BKMK_DigestSSP"></a>Proveedor de compatibilidad con seguridad implícita
La autenticación implícita es un estándar del sector que se usa para el Protocolo ligero de acceso a directorios (LDAP) y la autenticación Web. La autenticación de texto implícita transmite las credenciales a través de la red como un hash MD5 o una síntesis del mensaje.

El SSP de síntesis (Wdigest.dll) se utiliza para lo siguiente:

-   Acceso a Internet Explorer y Internet Information Services (IIS)

-   Consultas LDAP

Ubicación:% WINDIR% \Windows\System32\Digest.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a** al principio de este tema, más windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo Digest y el SSP Digest**

-   [Autenticación Microsoft Digest (Windows)](/windows/win32/secauthn/microsoft-digest-ssp)

-   [\[MS-DPSP \] : extensiones del protocolo Digest](/openspecs/windows_protocols/ms-dpsp/3e44be62-2067-472a-9ef0-e937298b68fb)

### <a name="schannel-security-support-provider"></a><a name="BKMK_SchannelSSP"></a>Proveedor de compatibilidad con seguridad de Schannel
El canal seguro (Schannel) se usa para la autenticación de servidor basada en Web, como cuando un usuario intenta tener acceso a un servidor Web seguro.

El protocolo TLS, el protocolo SSL, el protocolo de tecnología de comunicaciones privadas (PCT) y el protocolo de capa de transporte de datagramas (DTLS) se basan en la criptografía de clave pública. Schannel proporciona todos estos protocolos. Todos los protocolos de Schannel usan un modelo cliente/servidor. El SSP de SChannel usa certificados de clave pública para autenticar las partes. Al autenticar entidades, Schannel SSP selecciona un protocolo en el siguiente orden de preferencia:

-   Seguridad de la capa de transporte (TLS) versión 1,0

-   Seguridad de la capa de transporte (TLS) versión 1,1

-   Seguridad de la capa de transporte (TLS) versión 1,2

-   Capa de sockets seguros (SSL) versión 2,0

-   Capa de sockets seguros (SSL) versión 3,0

-   Tecnología de comunicaciones privadas (PCT)

    **Nota:** PCT está deshabilitado de forma predeterminada.

El protocolo seleccionado es el protocolo de autenticación preferido que el cliente y el servidor pueden admitir. Por ejemplo, si un servidor admite todos los protocolos Schannel y el cliente solo admite SSL 3,0 y SSL 2,0, el proceso de autenticación utiliza SSL 3,0.

DTLS se utiliza cuando la aplicación lo llama explícitamente. Para obtener más información sobre DTLS y los demás protocolos utilizados por el proveedor de Schannel, consulte la [referencia técnica del proveedor de compatibilidad con seguridad de Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Ubicación:% WINDIR% \Windows\System32\Schannel.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a** al principio de este tema, más windows Server 2003 y Windows XP.

> [!NOTE]
> TLS 1,2 se presentó en este proveedor en Windows Server 2008 R2 y Windows 7. DTLS se presentó en este proveedor en Windows Server 2012 y Windows 8.

**Recursos adicionales para los protocolos TLS y SSL y el SSP de Schannel**

-   [Canal seguro (Windows)](/windows/win32/secauthn/secure-channel)

-   [Referencia técnica de TLS/SSL](/previous-versions/windows/it-pro/windows-server-2003/cc784149(v=ws.10))

-   [\[MS-TLSP \] : Perfil de seguridad de la capa de transporte (TLS)](/openspecs/windows_protocols/ms-tlsp/58aba05b-62b0-4cd1-b88b-dc8a24920346)

### <a name="negotiate-security-support-provider"></a><a name="BKMK_NegoSSP"></a>Negociar proveedor de compatibilidad de seguridad
El mecanismo de negociación simple y protegida de GSS-API (SPNEGO) constituye la base para negociar SSP, whichcan se puede usar para negociar un protocolo de autenticación específico. Cuando una aplicación llama en SSPI para iniciar sesión en una red, puede especificar un SSP para que procese la solicitud. Si la aplicación especifica el SSP Negotiate, analiza la solicitud y selecciona el proveedor adecuado para administrar la solicitud, en función de las directivas de seguridad configuradas por el cliente.

SPNEGO se especifica en RFC 2478.

En las versiones compatibles de los sistemas operativos Windows, el proveedor de compatibilidad con la seguridad Negotiate selecciona entre el protocolo Kerberos y NTLM. Negotiate selecciona el protocolo Kerberos de forma predeterminada a menos que uno de los sistemas implicados en la autenticación no pueda usar ese Protocolo, o bien la aplicación que realiza la llamada no proporcionó información suficiente para usar el protocolo Kerberos.

Ubicación:% WINDIR% \Windows\System32\lsasrv.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a** al principio de este tema, más windows Server 2003 y Windows XP.

**Recursos adicionales para el SSP Negotiate**

-   [Microsoft Negotiate (Windows)](/windows/win32/secauthn/microsoft-negotiate)

-   [\[MS-SPNG \] : extensiones simples y protegidas del mecanismo de negociación de API (SPNEGO)](/openspecs/windows_protocols/ms-spng/f377a379-c24f-4a0f-a3eb-0d835389e28a)

-   [\[Especificación del Protocolo de autenticación HTTP MS-N2HT \] : Negotiate y Nego2](/openspecs/windows_protocols/ms-n2ht/4b88aa77-4b12-4933-8740-0f32d8f4eacf)

### <a name="credential-security-support-provider"></a><a name="BKMK_CredSSP"></a>Proveedor de compatibilidad con seguridad de credenciales
El proveedor de servicios de seguridad de credenciales (CredSSP) proporciona una experiencia de usuario de inicio de sesión único (SSO) al iniciar nuevas sesiones de Terminal Services y Servicios de Escritorio remoto. CredSSP permite a las aplicaciones delegar las credenciales de los usuarios desde el equipo cliente (mediante el SSP del lado cliente) al servidor de destino (a través del SSP del servidor), en función de las directivas del cliente. Las directivas CredSSP se configuran mediante directiva de grupo y la delegación de credenciales está desactivada de forma predeterminada.

Ubicación:% WINDIR% \Windows\System32\credssp.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a que se** encuentra al principio de este tema.

**Recursos adicionales para el SSP de credenciales**

-   [\[MS-CSSP \] : especificación del Protocolo de proveedor de compatibilidad para seguridad de credenciales (CredSSP)](/openspecs/windows_protocols/ms-cssp/85f57821-40bb-46aa-bfcb-ba9590b8fc30)

-   [Proveedor de servicios de seguridad de credenciales e inicio de sesión único para Terminal Services](/previous-versions/windows/it-pro/windows-vista/cc749211(v=ws.10))

### <a name="negotiate-extensions-security-support-provider"></a><a name="BKMK_NegoExtsSSP"></a>Negociar el proveedor de compatibilidad de seguridad de extensiones
Negotiate Extensions (NegoExts) es un paquete de autenticación que negocia el uso de SSP, que no sea NTLM o el protocolo Kerberos, para aplicaciones y escenarios implementados por Microsoft y otras compañías de software.

Esta extensión para el paquete Negotiate permite los siguientes escenarios:

-   **Disponibilidad enriquecida del cliente dentro de un sistema federado.** Se puede tener acceso a los documentos en los sitios de SharePoint y se pueden editar con una aplicación Microsoft Office completa.

-   **Compatibilidad de cliente enriquecida para servicios de Microsoft Office.** Los usuarios pueden iniciar sesión en Microsoft Office Services y usar una aplicación Microsoft Office completa.

-   **Microsoft Exchange Server y Outlook hospedados.** No se ha establecido ninguna confianza de dominio porque Exchange Server está hospedado en la Web. Outlook usa el servicio Windows Live para autenticar a los usuarios.

-   **Disponibilidad enriquecida del cliente entre equipos cliente y servidores.** Se usan los componentes de red y autenticación del sistema operativo.

El paquete de Windows Negotiate trata el SSP de NegoExts de la misma manera que para Kerberos y NTLM. NegoExts.dll se carga en la entidad de sistema local (LSA) en el inicio. Cuando se recibe una solicitud de autenticación, según el origen de la solicitud, NegoExts negocia entre los SSP admitidos. Recopila las credenciales y directivas, las cifra y envía la información al SSP adecuado, donde se crea el token de seguridad.

Los SSP admitidos por NegoExts no son SSP independientes como Kerberos y NTLM. Por lo tanto, dentro del SSP de NegoExts, cuando se produce un error en el método de autenticación por cualquier motivo, se mostrará o registrará un mensaje de error de autenticación. No es posible ningún método de autenticación de renegociación o de reserva.

Ubicación:% WINDIR% \Windows\System32\negoexts.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a** al principio de este tema, excepto windows Server 2008 y Windows Vista.

### <a name="pku2u-security-support-provider"></a><a name="BKMK_PKU2USSP"></a>Proveedor de compatibilidad con seguridad de PKU2U
El protocolo PKU2U se presentó e implementó como SSP en Windows 7 y Windows Server 2008 R2. Este SSP habilita la autenticación punto a punto, especialmente a través de la característica de uso compartido de archivos y medios denominada grupo hogar, que se presentó en Windows 7. La característica permite el uso compartido entre equipos que no son miembros de un dominio.

Ubicación:% WINDIR% \Windows\System32\pku2u.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la lista **se aplica a** al principio de este tema, excepto windows Server 2008 y Windows Vista.

**Recursos adicionales para el protocolo PKU2U y el SSP de PKU2U**

-   [Introducción a la integración de identidades en línea](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560662(v=ws.10))

## <a name="security-support-provider-selection"></a><a name="BKMK_SecuritySupportProviderSelection"></a>Selección del proveedor de compatibilidad con seguridad
Windows SSPI puede usar cualquiera de los protocolos que se admiten a través de los proveedores de compatibilidad de seguridad instalados. Sin embargo, dado que no todos los sistemas operativos son compatibles con los mismos paquetes de SSP que cualquier equipo dado que ejecute Windows Server, los clientes y servidores deben negociar para usar un protocolo que ambos admitan. Windows Server prefiere que los equipos cliente y las aplicaciones usen el protocolo Kerberos, un protocolo seguro basado en estándares, siempre que sea posible, pero el sistema operativo sigue permitiendo que los equipos cliente y las aplicaciones cliente que no son compatibles con el protocolo Kerberos se autentiquen.

Antes de que se pueda realizar la autenticación, los dos equipos que se comunican deben estar de acuerdo en un protocolo que ambos pueden admitir. Para que cualquier protocolo se pueda usar a través de la SSPI, cada equipo debe tener el SSP adecuado. Por ejemplo, para que un equipo cliente y un servidor utilicen el protocolo de autenticación Kerberos, ambos deben ser compatibles con Kerberos V5. Windows Server usa la función **EnumerateSecurityPackages** para identificar qué SSP se admiten en un equipo y cuáles son las capacidades de esos SSP.

La selección de un protocolo de autenticación puede administrarse de una de las dos maneras siguientes:

1.  [Protocolo de autenticación única](#BKMK_SingleAuth)

2.  [Opción Negotiate](#BKMK_Negotiate)

### <a name="single-authentication-protocol"></a><a name="BKMK_SingleAuth"></a>Protocolo de autenticación única
Cuando se especifica un único protocolo aceptable en el servidor, el equipo cliente debe admitir el protocolo especificado o se produce un error en la comunicación. Cuando se especifica un solo protocolo aceptable, el intercambio de autenticación se realiza de la siguiente manera:

1.  El equipo cliente solicita acceso a un servicio.

2.  El servidor responde a la solicitud y especifica el protocolo que se utilizará.

3.  El equipo cliente examina el contenido de la respuesta y realiza las comprobaciones para determinar si es compatible con el protocolo especificado. Si el equipo cliente es compatible con el protocolo especificado, la autenticación continúa. Si el equipo cliente no admite el protocolo, se produce un error de autenticación, independientemente de si el equipo cliente está autorizado para tener acceso al recurso.

### <a name="negotiate-option"></a><a name="BKMK_Negotiate"></a>Opción Negotiate
La opción Negotiate se puede usar para permitir que el cliente y el servidor intenten encontrar un protocolo aceptable. Esto se basa en el mecanismo de negociación simple y protegida de GSS-API (SPNEGO). Cuando la autenticación comienza con la opción de negociar un protocolo de autenticación, el intercambio SPNEGO se realiza de la siguiente manera:

1.  El equipo cliente solicita acceso a un servicio.

2.  El servidor responde con una lista de protocolos de autenticación que puede admitir y un desafío o respuesta de autenticación, según el protocolo que sea su primera elección. Por ejemplo, el servidor podría mostrar el protocolo Kerberos y NTLM, y enviar una respuesta de autenticación Kerberos.

3.  El equipo cliente examina el contenido de la respuesta y realiza las comprobaciones para determinar si es compatible con cualquiera de los protocolos especificados.

    -   Si el equipo cliente es compatible con el protocolo preferido, la autenticación continúa.

    -   Si el equipo cliente no admite el protocolo preferido, pero admite uno de los otros protocolos enumerados por el servidor, el equipo cliente permite que el servidor sepa qué protocolo de autenticación admite y la autenticación continúa.

    -   Si el equipo cliente no es compatible con ninguno de los protocolos enumerados, se produce un error en el intercambio de autenticación.

## <a name="additional-references"></a>Referencias adicionales
[Arquitectura de autenticación de Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169024(v=ws.10))