---
title: "Arquitectura de interfaz de proveedor de soporte técnico de seguridad"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="security-support-provider-interface-architecture"></a>Arquitectura de interfaz de proveedor de soporte técnico de seguridad

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de referencia para profesionales de TI describe los protocolos de autenticación de Windows que se usan dentro de la arquitectura de interfaz de proveedor de soporte técnico de seguridad (SSPI).

La interfaz de proveedor de soporte técnico de Microsoft Security (SSPI) es la base para la autenticación de Windows. Aplicaciones y servicios de infraestructura que requieren una autenticación usan SSPI para proporcionarlos.

SSPI es la implementación de la genérica seguridad servicio API (GSSAPI) en los sistemas operativos Windows Server. Para obtener más información sobre GSSAPI, consulta RFC 2743 y 2744 RFC en la base de datos de IETF RFC.

Los proveedores de soporte técnico de seguridad (SSP) de predeterminado que invocan los protocolos de autenticación específicos en Windows se incorporan SSPI como archivos DLL. En las siguientes secciones, se describe estas SSP de forma predeterminada. Se puede integrar SSP adicionales si funcionan con la SSPI.

Como se muestra en la siguiente imagen, SSPI Windows proporciona un mecanismo que lleva tokens de autenticación a través del canal de comunicación existente entre el equipo cliente y el servidor. Si dos equipos o dispositivos tienen que estar autenticada para que puedan comunicarse de forma segura, las solicitudes de autenticación se enrutan a la SSPI, que completa el proceso de autenticación, independientemente del protocolo de red actualmente en uso. La SSPI devuelve transparentes objetos binarios grandes. Estos se pasan entre las aplicaciones, en cuyo punto se pueden pasar a la capa SSPI. Por lo tanto, la SSPI permite que una aplicación usar distintos modelos de seguridad en un equipo o red sin cambiar la interfaz para el sistema de seguridad.

![Diagrama que muestra la arquitectura de interfaz de proveedor de soporte técnico de seguridad](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Las siguientes secciones describen los SSP predeterminado que interactúan con la SSPI. Los SSP sirven de diferentes maneras en sistemas operativos Windows para promover la comunicación segura en un entorno de red no segura.

-   [Proveedor de soporte técnico de seguridad de Kerberos](#BKMK_KerbSSP)

-   [Proveedor de soporte técnico de seguridad NTLM](#BKMK_NTLMSSP)

-   [Proveedor de soporte técnico de seguridad de resumen](#BKMK_DigestSSP)

-   [Proveedor de soporte técnico de seguridad Schannel](#BKMK_SchannelSSP)

-   [Negociar el proveedor de soporte técnico de seguridad](#BKMK_NegoSSP)

-   [Proveedor de soporte técnico de seguridad de credenciales](#BKMK_CredSSP)

-   [Proveedor de soporte técnico de seguridad de las extensiones de Negotiate](#BKMK_NegoExtsSSP)

-   [Proveedor de soporte técnico de seguridad de PKU2U](#BKMK_PKU2USSP)

También se incluyen en este tema:

[Selección de proveedor de soporte técnico de seguridad](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Proveedor de soporte técnico de seguridad de Kerberos
Este SSP usa solamente el protocolo de la versión 5 de Kerberos como implementados por Microsoft. Este protocolo se basa en de la red grupo de trabajo RFC 4120 y revisiones de borrador. Es un protocolo estándar del sector que se usa con una contraseña o una tarjeta inteligente para un inicio de sesión interactivo. También es el método de autenticación preferido para servicios de Windows.

Como el protocolo Kerberos ha sido el protocolo de autenticación de forma predeterminada desde Windows 2000, todos los servicios de dominio admiten la SSP. Kerberos Estos servicios se incluyen:

-   Active Directory consultas que utilizan el protocolo ligero de acceso a directorios (LDAP)

-   Administración remota de servidor o estación de trabajo que usa el servicio de llamada a procedimiento remoto

-   Servicios de impresión

-   Autenticación cliente-servidor.

-   Acceso remoto a archivos que usa el protocolo bloque de mensajes de servidor (SMB) (también conocido como sistema de archivos comunes de Internet o CIFS)

-   Administración del sistema de archivos distribuido y referencia

-   Autenticación de intranet a Internet Information Services (IIS)

-   Autenticación de autoridad de seguridad para la seguridad de protocolo de Internet (IPsec)

-   Solicitudes de certificados en los servicios de certificados de Active Directory para equipos y usuarios de dominio

Ubicación: %windir%\Windows\System32\kerberos.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo Kerberos y el SSP de Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: extensiones del protocolo Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: extensiones del protocolo Kerberos: servicio de usuario y la especificación del protocolo de la delegación restringida](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [PA/SSP de Kerberos (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Mejoras de Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) para Windows Vista

-   [Cambios en la autenticación Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) para Windows 7 

-   [Referencia técnica de autenticación de Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Proveedor de soporte técnico de seguridad NTLM
El proveedor de soporte técnico de seguridad de NTLM (NTLM SSP) es un binario protocolo usado por la interfaz de proveedor de soporte técnico de seguridad (SSPI) para permitir la autenticación desafío / respuesta NTLM y negociar confidencialidad e integridad de opciones de mensajería. NTLM se usa siempre que se usa la autenticación SSPI, incluidos para la autenticación de bloque de mensajes de servidor o CIFS, autenticación Negotiate de HTTP (por ejemplo, autenticación de Web de Internet) y el servicio de llamada a procedimiento remoto. El NTLM SSP incluye NTLM y NTLM versión 2 (NTLMv2) protocolos de autenticación.

Los sistemas operativos compatibles de Windows puede utilizar el SSP NTLM para los siguientes elementos:

-   Autenticación de cliente/servidor

-   Servicios de impresión

-   Acceso a archivos mediante el uso de CIFS (SMB)

-   El servicio de llamada a procedimiento remoto seguro o DCOM

Ubicación: %windir%\Windows\System32\msv1_0.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo NTLM y NTLM SSP**

-   [Paquete de autenticación MSV1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Cambios en la autenticación NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) en Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Restringir la Guía de uso NTLM y auditoría](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Proveedor de soporte técnico de seguridad de resumen
La autenticación implícita es un estándar del sector que se usa para el protocolo ligero de acceso a directorios (LDAP) y de autenticación web. La autenticación implícita transmite credenciales a través de la red como un código de mensaje o hash MD5.

Resumen SSP (Wdigest.dll) se usa para los siguientes elementos:

-   Acceso a Internet Explorer y Internet Information Services (IIS)

-   Consultas LDAP

Ubicación: %windir%\Windows\System32\Digest.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para el protocolo de resumen y el SSP de resumen**

-   [Autenticación implícita de Microsoft (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: Descubra las extensiones de protocolo](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Proveedor de soporte técnico de seguridad Schannel
El canal seguro (Schannel) se usa para la autenticación de servidor basado en web, como cuando un usuario intenta acceder a un servidor web seguro.

El protocolo TLS, protocolo SSL, el protocolo de tecnología de comunicaciones privadas (PCT) y el protocolo de capa de transporte de datagramas (DTLS) se basan en criptografía de clave pública. Schannel proporciona todos estos protocolos. Todos los protocolos de Schannel utilizan un modelo de cliente y servidor. El SSP Schannel usa certificados de clave pública para autenticar las partes. Al autenticar partes, Schannel SSP selecciona un protocolo en el siguiente orden de preferencia de:

-   Seguridad (TLS) versión 1.0 de la capa de transporte

-   Transporte capa de seguridad (TLS) versión 1.1

-   Transporte capa de seguridad (TLS) versión 1.2

-   Socket capa seguros (SSL) versión 2.0

-   Socket capa seguros (SSL) versión 3.0

-   Tecnología de comunicaciones privadas (PCT)

    **Ten en cuenta** PCT está deshabilitado de manera predeterminada.

El protocolo que se ha seleccionado es el protocolo de autenticación preferido que pueden admitir el cliente y el servidor. Por ejemplo, si un servidor es compatible con todos los protocolos de Schannel el cliente admite solo SSL 3.0 y SSL 2.0, el proceso de autenticación usa SSL 3.0.

DTLS se usa cuando se llama explícitamente a la aplicación. Para obtener más información sobre DTLS y el resto de los protocolos que usa el proveedor Schannel, consulta [referencia técnica del proveedor de soporte técnico seguridad Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Ubicación: %windir%\Windows\System32\Schannel.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

> [!NOTE]
> TLS 1.2 se introdujo en este proveedor en Windows Server 2008 R2 y Windows 7. DTLS se introdujo en este proveedor en Windows Server 2012 y Windows 8.

**Recursos adicionales para los protocolos TLS y SSL y el SSP Schannel**

-   [Canal seguro (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Referencia técnica de TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: perfil de seguridad (TLS) de la capa de transporte](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negociar el proveedor de soporte técnico de seguridad
El Simple y el mecanismo de negociación GSS API protegido (SPNEGO) constituye la base para el SSP Negotiate, whichcan usarse negociar un protocolo de autenticación específicos. Cuando una aplicación llama a SSPI para iniciar sesión en una red, puede especificar un SSP para procesar la solicitud. Si la aplicación especifica el SSP Negotiate, analiza la solicitud y elige el proveedor adecuado para controlar la solicitud, en función de las directivas de seguridad de cliente configurado.

SPNEGO se especifica en RFC 2478.

En versiones compatibles de los sistemas operativos Windows, la seguridad de Negotiate admite selecciona proveedor entre el protocolo Kerberos y NTLM. Negociar selecciona protocolo Kerberos de manera predeterminada, a menos que ese protocolo no se pueden usar con uno de los sistemas implicados en la autenticación o la aplicación de llamada no proporcionó suficiente información para usar el protocolo Kerberos.

Ubicación: %windir%\Windows\System32\lsasrv.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema, además de Windows Server 2003 y Windows XP.

**Recursos adicionales para el SSP Negotiate**

-   [Microsoft negociar (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: extensiones de API de GSS simple y protegidos negociación mecanismo (SPNEGO)](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: negociar y especificaciones de protocolo de autenticación de HTTP Nego2](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Proveedor de soporte técnico de seguridad de credenciales
El proveedor de servicios de seguridad de credenciales (CredSSP) proporciona una experiencia de usuario de único inicio de sesión único (SSO) al iniciar nuevas sesiones de Terminal Services y los servicios de escritorio remoto. CredSSP permite que las aplicaciones delegar credenciales de los usuarios del equipo cliente (mediante el SSP de cliente) en el servidor de destino (mediante el SSP de servidor), en función de las directivas del cliente. CredSSP directivas configuradas mediante la directiva de grupo y la delegación de credenciales está desactivada de manera predeterminada.

Ubicación: %windir%\Windows\System32\credssp.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema.

**Recursos adicionales para el SSP de credenciales**

-   [\[MS-CSSP\]: especificación del protocolo de proveedor (CredSSP) de soporte técnico de seguridad de credenciales](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Proveedor de servicios de seguridad y SSO para Terminal de credenciales de inicio de sesión de servicios](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Proveedor de soporte técnico de seguridad de las extensiones de Negotiate
Extensiones de Negotiate (NegoExts) es un paquete de autenticación que se negocia el uso de SSP, que no sean el protocolo Kerberos o NTLM para escenarios y aplicaciones implementan por software de Microsoft y otra empresas.

Esta extensión para el paquete de negociación permite los siguientes escenarios:

-   **Disponibilidad de cliente enriquecido dentro de un sistema federado.** Pueden tener acceso a documentos en sitios de SharePoint y se pueden editar mediante el uso de una aplicación completa de Microsoft Office.

-   **Cliente enriquecido compatibilidad con los servicios de Microsoft Office.** Los usuarios pueden iniciar sesión en servicios de Microsoft Office y usar una aplicación de Microsoft Office completa.

-   **Hospedado Microsoft Exchange Server y Outlook.** No hay ninguna confianza de dominio que establecer porque el servidor Exchange hospedado en la web. Outlook usa el servicio de Windows Live para autenticar a los usuarios.

-   **Disponibilidad de cliente enriquecido entre los equipos cliente y servidores.** Se usan componentes de red y autenticación del sistema operativo.

El paquete Windows Negotiate trata el SSP NegoExts de la misma manera, como lo hace de Kerberos y NTLM. NegoExts.dll se carga en la autoridad de sistema Local (LSA) en el inicio. Cuando se recibe una solicitud de autenticación, basada en el origen de la solicitud, se negocia NegoExts entre los SSP compatibles. Recopila las credenciales y las directivas, cifra de forma y envía dicha información al SSP apropiado, donde se crea el token de seguridad.

Los SSP admitidos por NegoExts no son independientes SSP como Kerberos y NTLM. Por lo tanto, dentro de SSP NegoExts, cuando se produce un error en el método de autenticación por cualquier motivo, un mensaje de error de autenticación aparecerá e iniciado sesión. No hay métodos de autenticación negociar o reserva son posibles.

Ubicación: %windir%\Windows\System32\negoexts.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema, excepto Windows Server 2008 y Windows Vista.

### <a name="BKMK_PKU2USSP"></a>Proveedor de soporte técnico de seguridad de PKU2U
El protocolo PKU2U se habilitó y se implementa como un SSP en Windows 7 y Windows Server 2008 R2. Este SSP habilita la autenticación de punto a punto, especialmente a través de los medios y los archivos compartidos característica denominada grupo hogar, que se introdujo en Windows 7. La característica permite el uso compartido entre equipos que no son miembros de un dominio.

Ubicación: %windir%\Windows\System32\pku2u.dll

Este proveedor se incluye de manera predeterminada en las versiones indicadas en el **se aplica a** lista al principio de este tema, excepto Windows Server 2008 y Windows Vista.

**Recursos adicionales para el protocolo PKU2U y el SSP PKU2U**

-   [Introducción a la integración de identidad en línea](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Selección de proveedor de soporte técnico de seguridad
La SSPI de Windows puede usar cualquiera de los protocolos que son compatibles con los proveedores de soporte técnico de seguridad instalados. Sin embargo, dado que no todos los sistemas operativos admite los mismos paquetes SSP como los equipos ejecutan Windows Server, clientes y servidores deben negociar usar un protocolo que ambas admiten. Windows Server prefiere los equipos cliente y aplicaciones para usar el protocolo Kerberos, un protocolo seguro basado en estándares, cuando sea posible, pero el sistema operativo continúa para que las aplicaciones que no admiten el protocolo Kerberos para autenticar cliente y los equipos cliente.

Antes de realizar la autenticación de comunicación lugar los dos equipos deben coincidir en un protocolo que pueden admitir. Para que cualquier protocolo que se pueda usar a través de la SSPI, cada equipo debe tener el SSP apropiado. Por ejemplo, para que un equipo cliente y servidor para usar el protocolo de autenticación de Kerberos, debe ser compatibles con Kerberos v5. Servidor de Windows usa la función **EnumerateSecurityPackages** para identificar qué SSP se admite en un equipo y las capacidades de los SSP son.

La selección de un protocolo de autenticación puede controlarse en una de las siguientes dos maneras:

1.  [Protocolo de autenticación solo](#BKMK_SingleAuth)

2.  [Negociar opción](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocolo de autenticación solo
Cuando se especifica un protocolo aceptable en el servidor, el equipo cliente debe admitir el protocolo especificado o la comunicación de produce un error. Cuando se especifica un protocolo aceptable, el intercambio de autenticación tiene lugar como sigue:

1.  El equipo cliente solicita acceso a un servicio.

2.  El servidor responde a la solicitud y especifica el protocolo que se usará.

3.  El equipo cliente examina el contenido de la respuesta y comprueba para determinar si admite el protocolo especificado. Si el equipo cliente admiten el protocolo especificado, continúa de la autenticación. Si el equipo cliente no admite el protocolo, la autenticación se produce un error, independientemente de si el equipo cliente está autorizado para acceder al recurso.

### <a name="BKMK_Negotiate"></a>Negociar opción
La opción de negotiate puede usarse para permitir que el cliente y servidor intentar encontrar un protocolo aceptable. Esto se basa en los Simple y el mecanismo de negociación GSS API protegido (SPNEGO). Cuando comienza la autenticación con la opción de negociar para un protocolo de autenticación, el intercambio de autenticación SPNEGO toma el lugar de la siguiente manera:

1.  El equipo cliente solicita acceso a un servicio.

2.  El servidor responde con una lista de protocolos de autenticación que puede admitir y un desafío de autenticación o la respuesta, según el protocolo que es la primera opción. Por ejemplo, el servidor puede enumerar el protocolo Kerberos y NTLM y enviar una respuesta de autenticación de Kerberos.

3.  El equipo cliente examina el contenido de la respuesta y comprueba para determinar si admite cualquiera de los protocolos especificados.

    -   Si el equipo cliente admite el protocolo preferido, ganancias por la autenticación.

    -   Si el equipo cliente no admite el protocolo preferido, pero es compatible con uno de los otros protocolos indicados en el servidor, el equipo cliente permite que el servidor sepa que admite el protocolo de autenticación y la autenticación se realiza.

    -   Si el equipo cliente no admite ninguno de los protocolos enumerados, se produce un error en el intercambio de autenticación.

## <a name="see-also"></a>Consulta también
[Arquitectura de autenticación de Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


