---
title: Novedades de la autenticación Kerberos
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 90107bd49268f232fd6d532c304c2fdd050bcbf5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391504"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>Se aplica a: Windows Server 2016 y Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Compatibilidad de KDC para la autenticación de cliente basada en claves públicas de confianza

A partir de Windows Server 2016, los KDC admiten ninguna forma de asignación de claves pública. Si la clave pública se ha aprovisionado para una cuenta, el KDC es compatible con Kerberos PKInit explícitamente con esa clave. Puesto que no hay ninguna validación de certificado, se admiten certificados autofirmados y no se admite la comprobación del mecanismo de autenticación.

Claves de confianza es preferible cuando se configura para una cuenta, independientemente del valor UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Cliente Kerberos y KDC de compatibilidad para RFC 8070 PKInit frescura extensión

A partir de Windows 10, versión 1607 y Windows Server 2016, los clientes Kerberos intentan el [extensión de la actualización de RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) para la clave pública basados en inicios de sesión. 

A partir de Windows Server 2016, los KDC pueden admitir la extensión de la actualización de PKInit. De forma predeterminada, los KDC no ofrecen la extensión de la actualización de PKInit. Para habilitarla, use la nueva compatibilidad KDC para la configuración de directiva de plantilla administrativa KDC de extensión de actualización de PKInit en todos los controladores de dominio en el dominio. Cuando se configura, se admiten las siguientes opciones cuando el dominio es Windows Server 2016 nivel funcional del dominio (DFL):

- **Deshabilitado**: El KDC nunca ofrece la extensión de la actualización de PKInit y acepta las solicitudes de autenticación válido sin comprobación de actualización. Los usuarios nunca recibirá la identidad de clave pública nueva SID.
- **Admite**: Extensión de la actualización de PKInit es compatible con la solicitud. Los clientes de Kerberos para autenticar correctamente con la extensión de la actualización de PKInit reciben la identidad de clave pública nueva SID.
- **Requerido**: Extensión de actualización de PKInit es necesaria para la autenticación correcta. Los clientes de Kerberos que no admiten la extensión de la actualización de PKInit siempre se producirá un error al usar las credenciales de clave públicas.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Compatibilidad con dispositivos Unidos a un dominio para la autenticación mediante clave pública

A partir de Windows 10 versión 1507 y Windows Server 2016, si un dispositivo unido al dominio es capaz de registrar su clave pública enlazado con un controlador de dominio (DC) de Windows Server 2016, a continuación, el dispositivo se puede autenticar con la clave pública mediante la autenticación Kerberos un controlador de dominio de Windows Server 2016. Para obtener más información, consulte [autenticación de clave pública dispositivo unido al dominio](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Los clientes Kerberos permiten los nombres de host de las direcciones IPv4 e IPv6 en los nombres de entidad de servicio (SPN)

A partir de Windows 10 versión 1507 y Windows Server 2016, los clientes Kerberos pueden configurarse para admitir los nombres de host IPv4 e IPv6 en los SPN. 

Ruta de acceso del registro:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Para configurar la compatibilidad con nombres de host de dirección IP en los SPN, cree una entrada TryIPSPN. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. Si no está configurado, los nombres de host de dirección IP no se intenta realizar.

Si el SPN se registra en Active Directory, a continuación, la autenticación se realiza correctamente con Kerberos. 

Para obtener más información consulte el documento [configurar Kerberos para las direcciones IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Compatibilidad del KDC con asignación de cuentas de claves de confianza

A partir de Windows Server 2016, los controladores de dominio tienen compatibilidad con claves de confianza de asignación de cuentas, así como reserva para el nombre Principal de usuario (UPN) y AltSecID existentes en el comportamiento de SAN. Cuando se establece UseSubjectAltName en:

- 0: Asignación explícita es necesaria. A continuación, debe haber uno:
    - Claves de confianza (novedad en Windows Server 2016)
    - ExplicitAltSecID
- 1: Asignación implícita está permitida (valor predeterminado):
    1. Si las claves de confianza está configurado para la cuenta, a continuación, se utiliza para la asignación (nueva con Windows Server 2016).
    2. Si no hay ningún UPN en la red SAN, se intenta AltSecID para la asignación.
    3. Si hay un UPN de la red SAN, se intenta UPN para la asignación.

## <a name="see-also"></a>Vea también

- [Introducción a la autenticación Kerberos](kerberos-authentication-overview.md)
