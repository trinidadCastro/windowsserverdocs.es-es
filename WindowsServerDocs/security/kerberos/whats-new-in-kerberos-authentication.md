---
title: What's New in Kerberos Authentication
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: a0916abf1076b5f791a856f0c85f54ad17f6d64c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403478"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>Se aplica a: Windows Server 2016 y Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Compatibilidad de KDC con la autenticación de cliente basada en confianza pública

A partir de Windows Server 2016, los KDC admiten una forma de asignación de claves públicas. Si la clave pública se aprovisiona para una cuenta, el KDC admite Kerberos PKInit explícitamente con esa clave. Dado que no hay ninguna validación de certificados, se admiten los certificados autofirmados y no se admite el mecanismo de autenticación.

La confianza de clave es preferible cuando se configura para una cuenta, independientemente del valor de UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Compatibilidad del cliente Kerberos con el KDC para la extensión de actualización de PKInit de RFC 8070

A partir de Windows 10, versión 1607 y Windows Server 2016, los clientes de Kerberos intentan la [extensión de actualización de PKInit de RFC 8070](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) para inicios de sesión basados en claves públicas. 

A partir de Windows Server 2016, los KDC pueden admitir la extensión de actualización PKInit. De forma predeterminada, los KDC no ofrecen la extensión de actualización PKInit. Para habilitarla, use la nueva configuración de la Directiva de plantillas administrativas KDC compatibilidad de KDC para la extensión de actualización de PKInit en todos los controladores de dominio del dominio. Cuando se configura, se admiten las siguientes opciones cuando el dominio es el nivel funcional del dominio de Windows Server 2016 (nivel funcional):

- **Deshabilitado**: El KDC nunca ofrece la extensión de actualización PKInit y acepta solicitudes de autenticación válidas sin comprobar si hay actualizaciones. Los usuarios nunca recibirán el SID de identidad de clave pública nueva.
- **Compatible**: La extensión de actualización PKInit se admite a petición. Los clientes Kerberos que se autentican correctamente con la extensión de actualización PKInit reciben el SID de identidad de clave pública nuevo.
- **Requerido**: La extensión de actualización PKInit es necesaria para la autenticación correcta. Los clientes Kerberos que no admiten la extensión de actualización de PKInit siempre producirán un error al usar credenciales de clave pública.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Compatibilidad con dispositivos Unidos a un dominio para la autenticación mediante la clave pública

A partir de la versión 1507 de Windows 10 y Windows Server 2016, si un dispositivo unido a un dominio puede registrar su clave pública enlazada con un controlador de dominio (DC) de Windows Server 2016, el dispositivo puede autenticarse con la clave pública mediante la autenticación Kerberos en un controlador de dominio de Windows Server 2016. Para obtener más información, consulte [autenticación de clave pública de dispositivo unido a un dominio](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Los clientes Kerberos permiten nombres de host de direcciones IPv4 e IPv6 en nombres de entidad de seguridad de servicio (SPN)

A partir de Windows 10 versión 1507 y Windows Server 2016, los clientes Kerberos se pueden configurar para admitir nombres de host IPv4 e IPv6 en SPN. 

Ruta de acceso del registro:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Para configurar la compatibilidad con los nombres de host de direcciones IP en los SPN, cree una entrada TryIPSPN. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. Si no está configurado, los nombres de host de la dirección IP no se intentan.

Si el SPN está registrado en Active Directory, la autenticación se realizará correctamente con Kerberos. 

Para obtener más información, consulte el documento [configuración de Kerberos para direcciones IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Compatibilidad de KDC con la asignación de cuentas de confianza de claves

A partir de Windows Server 2016, los controladores de dominio tienen compatibilidad con la asignación de cuentas de confianza de claves, así como la reserva a AltSecID existentes y el nombre principal de usuario (UPN) en el comportamiento de SAN. Cuando UseSubjectAltName se establece en:

- 0: La asignación explícita es obligatoria. Entonces debe haber:
    - Confianza de clave (novedad en Windows Server 2016)
    - ExplicitAltSecID
- 1: Se permite la asignación implícita (valor predeterminado):
    1. Si la confianza de claves está configurada para la cuenta, se usa para la asignación (novedad en Windows Server 2016).
    2. Si no hay ningún UPN en la SAN, se intenta realizar la asignación de AltSecID.
    3. Si hay un UPN en la SAN, se intenta la asignación del UPN.

## <a name="see-also"></a>Vea también

- [Información general sobre la autenticación Kerberos](kerberos-authentication-overview.md)
