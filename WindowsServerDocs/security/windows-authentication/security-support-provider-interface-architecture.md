---
title: Security Support Provider Interface Architecture
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8b0a74089c5d8a88a380f8a56e3b9e03b84081c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827026"
---
# <a name="security-support-provider-interface-architecture"></a>Security Support Provider Interface Architecture

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema de referencia para profesionales de TI describe los protocolos de autenticación de Windows que se utilizan dentro de la arquitectura de interfaz de proveedor de compatibilidad para seguridad (SSPI).

La interfaz de proveedor de soporte técnico de seguridad (SSPI) de Microsoft es la base para la autenticación de Windows. Las aplicaciones y servicios de infraestructura que requieren autenticación utilizan SSPI para proporcionarla.

SSPI es la implementación de los genéricos seguridad Service API (GSSAPI) en sistemas operativos Windows Server. Para obtener más información sobre GSSAPI, consulte RFC 2743 y 2744 RFC en la base de datos de RFC de IETF.

Los proveedores de soporte técnico de seguridad predeterminados (SSP) que invocar protocolos de autenticación específicos de Windows se incorporan a la SSPI como archivos DLL. Estos SSP predeterminado se describe en las secciones siguientes. SSP adicionales se puede incorporar si trabajan con la SSPI.

Como se muestra en la siguiente imagen, la SSPI de Windows proporciona un mecanismo que transporta tokens de autenticación a través del canal de comunicación existente entre el equipo cliente y el servidor. Cuando dos equipos o dispositivos deben estar autenticados para que se puedan comunicar de forma segura, se enrutan las solicitudes de autenticación a la SSPI, que se complete el proceso de autenticación, independientemente del protocolo de red actualmente en uso. SSPI devuelve transparentes objetos binarios grandes. Estos se pasan entre las aplicaciones, momento en que puede pasarse a la capa SSPI. Por lo tanto, la SSPI permite a una aplicación utilizar diversos modelos de seguridad en un equipo o red sin cambiar la interfaz para el sistema de seguridad.

![Diagrama que muestra la arquitectura de interfaz del proveedor de soporte técnico de seguridad](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Las secciones siguientes describen los SSP predeterminado que interactúan con la SSPI. El SSP se utilizan de maneras diferentes en los sistemas operativos de Windows para promover una comunicación segura en un entorno de red no segura.

-   [Proveedor de soporte técnico de seguridad de Kerberos](#BKMK_KerbSSP)

-   [Proveedor de seguridad NTLM](#BKMK_NTLMSSP)

-   [Proveedor de soporte técnico de seguridad de síntesis](#BKMK_DigestSSP)

-   [Proveedor de soporte técnico de seguridad de Schannel](#BKMK_SchannelSSP)

-   [Negociar el proveedor de soporte técnico de seguridad](#BKMK_NegoSSP)

-   [Proveedor de soporte técnico de seguridad de credenciales](#BKMK_CredSSP)

-   [Negociar el proveedor de soporte técnico de seguridad de las extensiones](#BKMK_NegoExtsSSP)

-   [Proveedor de soporte técnico de seguridad PKU2U](#BKMK_PKU2USSP)

También se incluyen en este tema:

[Selección del proveedor de soporte técnico de seguridad](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Proveedor de soporte técnico de seguridad de Kerberos
Este SSP usa sólo el protocolo Kerberos versión 5 como implementadas por Microsoft. Este protocolo se basa en la red funciona y del grupo 4120 RFC borrador revisiones. Es un protocolo estándar del sector que se usa con una contraseña o una tarjeta inteligente para el inicio de sesión interactivo. También es el método de autenticación preferido para los servicios de Windows.

Dado que el protocolo de Kerberos ha sido el protocolo de autenticación predeterminado desde Windows 2000, todos los servicios de dominio admiten la SSP. Kerberos Esos servicios incluyen:

-   Active Directory las consultas que usan el protocolo ligero de acceso a directorios (LDAP)

-   Administración de servidor o estación de trabajo remota que utiliza el servicio llamada a procedimiento remoto

-   Servicios de impresión

-   Autenticación de cliente / servidor

-   Acceso al archivo remoto que usa el protocolo bloque de mensajes del servidor (SMB) (también conocido como sistema de archivos de Internet común o CIFS)

-   Administración del sistema de archivos distribuido y de referencia

-   Autenticación de intranet en Internet Information Services (IIS)

-   Autenticación de entidad de seguridad para la seguridad de protocolo de Internet (IPsec)

-   Solicitudes de certificados para servicios de certificados de Active Directory para equipos y usuarios de dominio

Location: %windir%\Windows\System32\kerberos.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo Kerberos y el SSP de Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: Extensiones del protocolo Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: Extensiones del protocolo Kerberos: Servicio para el usuario y la especificación del protocolo de delegación restringida](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [SSP/PA de Kerberos (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Mejoras de Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) para Windows Vista

-   [Cambios en la autenticación Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) para Windows 7 

-   [Referencia técnica de autenticación de Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Proveedor de seguridad NTLM
El proveedor de soporte técnico de seguridad (SSP NTLM) de NTLM es un archivo binario utilizado por la interfaz de proveedor de soporte técnico de seguridad (SSPI) para permitir la autenticación de desafío / respuesta NTLM y negociar la integridad y confidencialidad de las opciones de protocolo de mensajería. Se utiliza NTLM siempre que se utiliza la autenticación de SSPI, incluidos para la autenticación de bloque de mensajes del servidor o CIFS, autenticación Negotiate de HTTP (por ejemplo, autenticación Web de Internet) y el servicio llamada a procedimiento remoto. El SSP de NTLM incluye el NTLM y NTLM versión 2 (NTLMv2) protocolos de autenticación.

Los sistemas operativos de Windows puede usar el SSP de NTLM para lo siguiente:

-   Autenticación de cliente/servidor

-   Servicios de impresión

-   Acceso a archivos mediante el uso de CIFS (SMB)

-   Proteger el servicio llamada a procedimiento remoto o servicio DCOM

Location: %windir%\Windows\System32\msv1_0.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo NTLM y el SSP de NTLM**

-   [Paquete de autenticación MSV1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Cambios en la autenticación NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) en Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Auditoría y restricción de la Guía de uso NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Proveedor de soporte técnico de seguridad de síntesis
La autenticación implícita es un estándar del sector que se usa para la autenticación de web y de protocolo ligero de acceso a directorios (LDAP). La autenticación implícita transmite credenciales a través de la red como un código de mensaje o hash MD5.

El SSP de síntesis (Wdigest.dll) se usa para lo siguiente:

-   Acceso a Internet Explorer y en Internet Information Services (IIS)

-   Consultas LDAP

Location: %windir%\Windows\System32\Digest.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo de texto implícita e implícita SSP**

-   [Autenticación implícita de Microsoft (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: Extensiones de protocolo de texto implícita](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Proveedor de soporte técnico de seguridad de Schannel
El canal seguro (Schannel) se usa para la autenticación de servidor basada en web, como cuando un usuario intenta tener acceso a un servidor web seguro.

El protocolo TLS, protocolo SSL, el protocolo de tecnología de comunicaciones privadas (PCT) y el protocolo de capa de transporte del datagrama (DTLS) se basan en la criptografía de clave pública. Schannel proporciona todos estos protocolos. Todos los protocolos de Schannel usan un modelo cliente/servidor. El SSP de SChannel usa certificados de clave pública para autenticar las partes. Al autenticar las partes, Schannel SSP selecciona un protocolo en el siguiente orden de preferencia:

-   Seguridad de capa (TLS) versión 1.0 de transporte

-   Seguridad de capa (TLS) versión 1.1 de transporte

-   Seguridad de capa (TLS) versión 1.2 de transporte

-   Secure Socket Layer (SSL) versión 2.0

-   Secure Socket Layer (SSL) versión 3.0

-   Tecnología de comunicaciones privadas (PCT)

    **Tenga en cuenta** PCT está deshabilitado de forma predeterminada.

El protocolo que se ha seleccionado es el protocolo de autenticación preferido que pueden admitir el cliente y el servidor. Por ejemplo, si un servidor es compatible con todos los protocolos de Schannel y el cliente solo admite SSL 3.0 y 2.0 de SSL, el proceso de autenticación usa SSL 3.0.

DTLS se utiliza cuando se llama explícitamente a la aplicación. Para obtener más información acerca de DTLS y el resto de los protocolos utilizados por el proveedor de Schannel, consulte [referencia técnica del proveedor de soporte técnico de seguridad Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Location: %windir%\Windows\System32\Schannel.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

> [!NOTE]
> TLS 1.2 se introdujo en este proveedor en Windows Server 2008 R2 y Windows 7. DTLS se introdujo en este proveedor en Windows Server 2012 y Windows 8.

**Recursos adicionales para los protocolos TLS y SSL y Schannel SSP**

-   [Canal seguro (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Referencia técnica de TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: Perfil de Transport Layer Security (TLS)](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negociar el proveedor de soporte técnico de seguridad
El Simple y el mecanismo de negociación GSS-API protegida (SPNEGO) constituye la base para negociar SSP whichcan se usan para negociar un protocolo de autenticación específico. Cuando una aplicación llama en SSPI para iniciar sesión en una red, puede especificar un SSP para procesar la solicitud. Si la aplicación especifica Negociar SSP, analiza la solicitud y escoge el proveedor adecuado para atender la solicitud, según las directivas de seguridad configurada por cliente.

SPNEGO se especifica en RFC 2478.

En las versiones compatibles de los sistemas operativos de Windows, la seguridad Negotiate admite proveedor selecciona entre el protocolo Kerberos y NTLM. Negociar selecciona el protocolo Kerberos de forma predeterminada a menos que dicho protocolo no se puede usar uno de los sistemas implicados en la autenticación o la aplicación que llama no proporcionó información suficiente para usar el protocolo Kerberos.

Location: %windir%\Windows\System32\lsasrv.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para negociar SSP**

-   [Negociación de Microsoft (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: Extensiones de mecanismo (SPNEGO) de negociación GSS-API simple y protegida](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: Negociar y especificación de protocolo de autenticación de HTTP Nego2](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Proveedor de soporte técnico de seguridad de credenciales
El proveedor de servicios de seguridad de credenciales (CredSSP) proporciona una experiencia de usuario de single sign-on (SSO) al iniciar nuevas sesiones de Terminal Services y servicios de escritorio remoto. CredSSP permite que las aplicaciones delegar las credenciales de usuarios desde el equipo cliente (mediante el SSP del lado cliente) al servidor de destino (mediante el SSP de lado servidor), según las directivas del cliente. Las directivas de CredSSP se configuran mediante la directiva de grupo y la delegación de credenciales está desactivada de forma predeterminada.

Location: %windir%\Windows\System32\credssp.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema.

**Recursos adicionales para el SSP de credenciales**

-   [\[MS-CSSP\]: Especificación del protocolo de credencial seguridad soporte CredSSP (proveedor)](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Credencial de proveedor de servicios de seguridad y el inicio de sesión único para Terminal Services de inicio de sesión](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Negociar el proveedor de soporte técnico de seguridad de las extensiones
Negociar extensiones (NegoExts) es un paquete de autenticación que se negocia el uso de SSP, que no sea NTLM o el protocolo Kerberos, para las aplicaciones y escenarios implementan por software de Microsoft y otra compañías.

Esta extensión para negociar paquete permite que los siguientes escenarios:

-   **Disponibilidad de cliente enriquecido dentro de un sistema federado.** Pueden tener acceso a documentos en sitios de SharePoint y se pueden editar mediante el uso de una aplicación completa de Microsoft Office.

-   **Compatibilidad con cliente enriquecida para los servicios de Microsoft Office.** Los usuarios pueden iniciar sesión en servicios de Microsoft Office y usar una aplicación completa de Microsoft Office.

-   **Microsoft Exchange Server hospedado y Outlook.** No hay ninguna confianza de dominio puede establecida porque el servidor de Exchange está hospedado en la web. Outlook usa el servicio de Windows Live para autenticar a los usuarios.

-   **Disponibilidad de cliente enriquecidas entre los equipos cliente y servidores.** Se usan los componentes del sistema operativo de red y autenticación.

El paquete Windows Negotiate trata NegoExts SSP de la misma manera que lo hace para Kerberos y NTLM. NegoExts.dll se carga en la autoridad de sistema Local (LSA) en el inicio. Cuando se recibe una solicitud de autenticación, según el origen de la solicitud, NegoExts se negocia entre el SSP admitidos. Recopila las credenciales y las directivas, los cifra y envía esa información para el SSP apropiado, donde se crea el token de seguridad.

El SSP compatibles con NegoExts no son SSP independiente, como Kerberos y NTLM. Por lo tanto, en el SSP NegoExts, cuando se produce un error en el método de autenticación por cualquier motivo, un mensaje de error de autenticación aparecerá e iniciado. Ningún método de autenticación de reserva o renegociación es posible.

Location: %windir%\Windows\System32\negoexts.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema, excepto Windows Server 2008 y Windows Vista.

### <a name="BKMK_PKU2USSP"></a>Proveedor de soporte técnico de seguridad PKU2U
Se introdujo el protocolo PKU2U y se implementa como un SSP en Windows 7 y Windows Server 2008 R2. Este SSP habilita la autenticación de punto a punto, sobre todo a través de los medios y la característica denominada grupo hogar, que se introdujo en Windows 7 de uso compartido de archivos. La característica permite el uso compartido entre los equipos que no son miembros de un dominio.

Location: %windir%\Windows\System32\pku2u.dll

Este proveedor se incluye de forma predeterminada en las versiones designadas en la **se aplica a** lista al principio de este tema, excepto Windows Server 2008 y Windows Vista.

**Recursos adicionales para el protocolo PKU2U y el SSP PKU2U**

-   [Introducción a la integración de identidades en línea](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Selección del proveedor de soporte técnico de seguridad
La SSPI de Windows puede usar cualquiera de los protocolos que se admiten a través de los proveedores instalados de soporte técnico de seguridad. Sin embargo, dado que no todos los sistemas operativos admiten los mismos paquetes SSP como los equipos que ejecutan Windows Server, los clientes y servidores deben negociar para usar un protocolo que admitan. Windows Server prefiere los equipos cliente y las aplicaciones usen el protocolo Kerberos, un protocolo seguro basado en estándares, cuando sea posible, pero el sistema operativo continúa permitir que los equipos cliente y cliente de las aplicaciones que no son compatibles con Kerberos Protocolo de autenticación.

Antes de poder realizar autenticación comunicación lugar los dos equipos deben coincidir en un protocolo que pueden admitir. Para que cualquier protocolo que se pueda usar a través de la SSPI, cada equipo debe tener el SSP adecuado. Por ejemplo, para que un equipo cliente y servidor que se usará el protocolo de autenticación Kerberos, debe ambos admiten Kerberos v5. Windows Server usa la función **EnumerateSecurityPackages** para identificar qué SSP se admite en un equipo y qué son las capacidades de esos SSP.

La selección de un protocolo de autenticación puede controlarse en una de las dos maneras siguientes:

1.  [Protocolo de autenticación único](#BKMK_SingleAuth)

2.  [Negociar la opción](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocolo de autenticación único
Cuando se especifica un único protocolo aceptable en el servidor, el equipo cliente debe admitir el protocolo especificado o la produce un error de comunicación. Cuando se especifica un único protocolo aceptable, el intercambio de autenticación realiza como sigue:

1.  El equipo cliente solicita acceso a un servicio.

2.  El servidor responde a la solicitud y especifica el protocolo que se usará.

3.  El equipo cliente examina el contenido de la respuesta y comprobaciones para determinar si admite el protocolo especificado. Si el equipo cliente es compatible con el protocolo especificado, la autenticación continúa. Si el equipo cliente no admite el protocolo, la autenticación se produce un error, independientemente de si el equipo cliente está autorizado para acceder al recurso.

### <a name="BKMK_Negotiate"></a>Negociar la opción
La opción negotiate puede utilizarse para permitir que el cliente y servidor intentar encontrar un protocolo aceptable. Esto se basa en el Simple y el mecanismo de negociación GSS-API protegida (SPNEGO). Cuando se inicia la autenticación con la opción negociar un protocolo de autenticación, el intercambio SPNEGO lleva a cabo como sigue:

1.  El equipo cliente solicita acceso a un servicio.

2.  El servidor responde con una lista de protocolos de autenticación que puede admitir y un desafío de autenticación o la respuesta, según el protocolo que es su primera opción. Por ejemplo, el servidor podría enumerar el protocolo Kerberos y NTLM y enviar una respuesta de autenticación Kerberos.

3.  El equipo cliente examina el contenido de la respuesta y comprobaciones para determinar si admite cualquiera de los protocolos especificados.

    -   Si el equipo cliente es compatible con el protocolo preferido, la autenticación continúa.

    -   Si el equipo cliente no admite el protocolo preferido, pero es compatible con uno de los otros protocolos enumerados por el servidor, el equipo cliente permite que el servidor sabe que admite el protocolo de autenticación y la autenticación continúa.

    -   Si el equipo cliente no es compatible con cualquiera de los protocolos enumerados, se produce un error en el intercambio de autenticación.

## <a name="see-also"></a>Vea también
[Arquitectura de autenticación de Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


