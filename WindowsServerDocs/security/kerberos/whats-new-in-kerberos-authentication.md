---
title: "Novedades en la autenticación Kerberos"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 7b046490c606cdf9e1436f503bf46a9cd4280ea9
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2018
---
# <a name="whats-new-in-kerberos-authentication"></a>Novedades en la autenticación Kerberos

>Se aplica a: Windows Server 2016 y Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Compatibilidad KDC para la autenticación de cliente basado en la clave pública de confianza

A partir de Windows Server 2016, KDC admiten una forma de asignación de clave pública. Si la clave pública se aprovisione para una cuenta, KDC admite Kerberos PKInit explícitamente con dicha clave. Dado que no hay ninguna validación de certificado, se admiten certificados autofirmados y no se admite el mecanismo de autenticación.

Clave de confianza se prefiere cuando se configura para una cuenta independientemente de la configuración de UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Compatibilidad con cliente Kerberos y KDC para RFC 8070 PKInit actualidad extensión

A partir de Windows 10, versión 1607 y Windows Server 2016, los clientes de Kerberos intentan la [extensión de actualidad RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) para clave pública según los inicios de sesión. 

A partir de Windows Server 2016, KDC pueden admitir la extensión de actualidad PKInit. De manera predeterminada, KDC no ofrecen la extensión de actualidad PKInit. Para habilitarla, usa la nueva compatibilidad KDC para la configuración de directiva de plantilla administrativa PKInit actualidad extensión KDC en todos los controladores de dominio del dominio. Cuando se configura, se admiten las siguientes opciones cuando el dominio es el nivel funcional del dominio de Windows Server 2016 (DFL):

- **Deshabilitado**: el KDC nunca ofrece la extensión de actualidad PKInit y acepta las solicitudes de autenticación válido sin comprobar la actualidad. Los usuarios no recibirán nunca la identidad de clave pública SID desde cero.
- **Admite**: extensión de actualidad PKInit es compatible con la solicitud. Los clientes de Kerberos para autenticar correctamente con la extensión de actualidad PKInit recibirán la identidad de clave pública SID desde cero.
- **Requiere**: extensión de actualidad PKInit es necesaria para la autenticación correcta. Los clientes de Kerberos que no son compatibles con la extensión de actualidad PKInit siempre se producirá un error al usar las credenciales de clave públicas.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Soporte técnico de dispositivo unido al dominio para la autenticación con clave pública

Empezando por versión 1507 de Windows 10 y Windows Server 2016, si un dispositivo unido al dominio es capaz de registrar su clave pública enlazado con un controlador de dominio de Windows Server 2016 (corriente continua), a continuación, el dispositivo puede autenticar con la clave pública mediante la autenticación Kerberos para un controlador de dominio de Windows Server 2016. Para obtener más información, consulta [autenticación de clave pública dispositivos Unidos a un dominio](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Los clientes Kerberos permiten nombres de host de direcciones IPv4 e IPv6 en los nombres de entidad de seguridad de servicio (SPN)

A partir de Windows 10 versión 1507 y Windows Server 2016, los clientes de Kerberos pueden configurarse para admitir los nombres de host IPv4 e IPv6 en SPN. 

Ruta de acceso del registro:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Para configurar la compatibilidad con nombres de host de dirección IP de SPN, crea una entrada TryIPSPN. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 1. Si no está configurado, nombres de host de dirección IP no se intentan.

Si se registra el SPN en Active Directory, la autenticación se supera con Kerberos. 

## <a name="kdc-support-for-key-trust-account-mapping"></a>Compatibilidad de KDC con clave de confianza de asignación de cuenta

A partir de Windows Server 2016, controladores de dominio tienen compatibilidad para confiar en clave de asignación de cuenta, así como una reserva a nombre Principal de usuario (UPN) y AltSecID existentes en el comportamiento de SAN. Cuando se establece UseSubjectAltName en:

- 0: se requiere una asignación explícita. A continuación, debe haber uno:
    - Clave de confianza (nuevo con Windows Server 2016)
    - ExplicitAltSecID
- 1: se permite la asignación implícita (predeterminado):
    1. Si se configura la clave de confianza para la cuenta, a continuación, se utiliza para la asignación (nuevo con Windows Server 2016).
    2. Si no hay ningún UPN en el entorno de SAN, se intenta AltSecID para la asignación.
    3. Si hay un UPN en el entorno de SAN, se intenta UPN de asignación.

## <a name="see-also"></a>Consulta también

- [Introducción a la autenticación Kerberos](kerberos-authentication-overview.md)
