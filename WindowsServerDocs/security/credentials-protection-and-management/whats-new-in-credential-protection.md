---
title: Novedades de la protección de credenciales
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 2351be82ad1d8b9af17715ce363836f57c71ea66
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386912"
---
# <a name="whats-new-in-credential-protection"></a>Novedades de la protección de credenciales

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard para el usuario con sesión iniciada

A partir de Windows 10, versión 1507, Kerberos y NTLM usan la seguridad basada en la virtualización para proteger los secretos de Kerberos & NTLM de la sesión de inicio de sesión de usuario con sesión iniciada. 

A partir de Windows 10, versión 1511, el administrador de credenciales usa la seguridad basada en la virtualización para proteger las credenciales guardadas del tipo de credencial de dominio. Las credenciales de sesión iniciadas y las credenciales de dominio guardadas no se pasarán a un host remoto mediante escritorio remoto. Credential Guard se puede habilitar sin el bloqueo UEFI.

A partir de Windows 10, versión 1607, el modo de usuario aislado se incluye con Hyper-V, por lo que ya no se instala de forma independiente para la implementación de Credential Guard.

[Más información sobre Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Credential Guard remoto para el usuario con sesión iniciada

A partir de Windows 10, versión 1607, la protección de credenciales remotas protege las credenciales de usuario con sesión iniciada al usar Escritorio remoto mediante la protección de los secretos de Kerberos y NTLM en el dispositivo cliente. Para que el host remoto evalúe los recursos de red como usuario, las solicitudes de autenticación requieren que el dispositivo cliente use los secretos.

A partir de Windows 10, versión 1703, Credential Guard remoto protege las credenciales de usuario suministradas al utilizar Escritorio remoto.

[Más información sobre Credential Guard remoto](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protecciones de dominio

Las protecciones de dominio requieren un dominio de Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Compatibilidad con dispositivos Unidos a un dominio para la autenticación mediante la clave pública

A partir de Windows 10 versión 1507 y Windows Server 2016, si un dispositivo unido a un dominio puede registrar su clave pública enlazada con un controlador de dominio (DC) de Windows Server 2016, el dispositivo puede autenticarse con la clave pública mediante Kerberos PKINIT. autenticación en un controlador de dominio de Windows Server 2016.

A partir de Windows Server 2016, los KDC admiten la autenticación mediante la confianza de clave Kerberos.  

[Obtenga más información sobre la compatibilidad con claves públicas para dispositivos Unidos a un dominio & la confianza de clave Kerberos](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Compatibilidad con la extensión de actualización PKINIT

A partir de Windows 10, versión 1507 y Windows Server 2016, los clientes Kerberos intentarán la extensión de actualización PKInit para inicios de sesión basados en claves públicas. 

A partir de Windows Server 2016, los KDC pueden admitir la extensión de actualización PKInit.  De forma predeterminada, los KDC no proporcionarán la extensión de actualización PKInit. 

[Más información sobre la compatibilidad con la extensión de actualización de PKINIT](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Solo los secretos NTLM del usuario de la clave pública

A partir del nivel funcional del dominio de Windows Server 2016 (nivel funcional), los controladores de dominio pueden admitir el reinicio de los secretos de NTLM de los usuarios de una clave pública. Esta característica no está disponible en DFLs inferiores.

> [!WARNING] 
> La adición de un controlador de dominio a un dominio con los secretos de NTLM sucesivos habilitados antes de que el controlador de dominio se haya actualizado con al menos el 8 de noviembre de 2016, se corre el riesgo de que se produzca un bloqueo del DC. 

Configuración: En el caso de los dominios nuevos, esta característica está habilitada de forma predeterminada. En el caso de los dominios existentes, debe configurarse en el Active Directory centro de administración: 

1. En el centro de administración de Active Directory, haga clic con el botón secundario en el dominio en el panel izquierdo y seleccione **propiedades**.

    ![Propiedades de dominio](../media/Credentials-Protection-And-Management/domain-properties.png)

2. Seleccione habilitar la finalización **de los secretos de NTLM que expiran durante el inicio de sesión para los usuarios que necesitan usar Microsoft Passport o tarjeta inteligente para el inicio de sesión interactivo**.

    ![El relanzamiento de los secretos de NTLM expira](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Haga clic en **Aceptar**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Permitir la red NTLM cuando el usuario está restringido a determinados dispositivos Unidos a un dominio

A partir del nivel funcional de dominio de Windows Server 2016 (nivel funcional), los controladores de dominio pueden permitir la red NTLM cuando un usuario está restringido a determinados dispositivos Unidos a un dominio. Esta característica no está disponible en DFLs inferiores.

Configuración: En la Directiva de autenticación, haga clic en **permitir la autenticación de red NTLM cuando el usuario está restringido a los dispositivos seleccionados**. 

[Más información sobre las directivas de autenticación](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
