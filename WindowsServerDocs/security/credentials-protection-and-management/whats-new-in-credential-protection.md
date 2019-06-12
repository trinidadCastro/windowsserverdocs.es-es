---
title: Novedades en protección de credenciales
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 475b6a0b24b811008ee213c1604d98d9aa9eb092
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447037"
---
# <a name="whats-new-in-credential-protection"></a>Novedades en protección de credenciales

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard para el usuario con sesión iniciada

A partir de Windows 10, versión 1507, Kerberos y NTLM usan seguridad basada en virtualización para proteger los secretos de Kerberos y NTLM de la sesión de inicio de sesión de usuario con sesión iniciada. 

A partir de Windows 10, versión 1511, Administrador de credenciales usa seguridad basada en virtualización para proteger las credenciales guardadas de tipo de credencial de dominio. No ha iniciado sesión credenciales y las credenciales de dominio guardado se pasarán a un host remoto mediante Escritorio remoto. Credential Guard puede habilitarse sin bloqueo UEFI.

A partir de Windows 10, versión 1607, modo de usuario aislado se incluye con Hyper-V así que ya no se instala por separado para la implementación de Credential Guard.

[Más información sobre Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>De Credential Guard remoto para el usuario con sesión iniciada

Windows 10, versión 1607, a partir de Credential Guard remoto protege las credenciales de usuario con sesión iniciada al usar Escritorio remoto mediante la protección de los secretos de Kerberos y NTLM en el dispositivo cliente. Para que el host remoto evaluar los recursos de red como el usuario, las solicitudes de autenticación requieren que el dispositivo de cliente para usar los secretos.

A partir de Windows 10, versión 1703, de Credential Guard remoto protege las credenciales de usuario proporcionada al usar Escritorio remoto.

[Más información sobre credential guard remoto](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protecciones de dominio

Protecciones de dominio requieren un dominio de Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Compatibilidad con dispositivos Unidos a un dominio para la autenticación mediante clave pública

A partir de Windows 10 versión 1507 y Windows Server 2016, si un dispositivo unido al dominio es capaz de registrar su clave pública enlazado con un controlador de dominio (DC) de Windows Server 2016, a continuación, el dispositivo se puede autenticar con la clave pública mediante Kerberos PKINIT autenticación en un controlador de dominio de Windows Server 2016.

A partir de Windows Server 2016, los KDC admiten la autenticación mediante clave de confianza de Kerberos.  

[Más información sobre la compatibilidad de clave pública para dispositivos Unidos a dominio & relación de confianza Kerberos clave](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Compatibilidad con las extensiones frescura PKINIT

A partir de Windows 10, versión 1507 y Windows Server 2016, los clientes Kerberos tratará de la extensión de la actualización de PKInit para pública clave basado en inicios de sesión. 

A partir de Windows Server 2016, los KDC pueden admitir la extensión de la actualización de PKInit.  De forma predeterminada, los KDC no ofrecerá la extensión de la actualización de PKInit. 

[Más información sobre la compatibilidad con las extensiones PKINIT frescura](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Las sucesivas pública clave única secretos del usuario NTLM

A partir de Windows Server 2016 nivel funcional del dominio (DFL), los controladores de dominio pueden admitir gradual una pública clave única secretos del usuario NTLM. Esta característica es sus en DFLs inferior.

> [!WARNING] 
> Agregar un controlador de dominio a un dominio con las sucesivas secretos NTLM habilitados antes de que el controlador de dominio se ha actualizado con al menos 8 de noviembre de 2016 de mantenimiento se ejecuta el riesgo de que el bloqueo de controlador de dominio. 

Configuración: Dominios de nuevo, esta característica está habilitada de forma predeterminada. Para los dominios existentes, debe configurarse en el centro de administración de Active Directory: 

1. Desde el centro administrativo de Active Directory, haga clic en el dominio en el panel izquierdo y seleccione **propiedades**.

    ![Propiedades de dominio](../media/Credentials-Protection-And-Management/domain-properties.png)

2. Seleccione **habilitar gradual de secretos NTLM que van a expirar durante el inicio de sesión, para los usuarios que son necesarios para usar Microsoft Passport o una tarjeta inteligente para el inicio de sesión interactivo**.

    ![Secretos NTLM que van a expirar Autoroll](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Haga clic en **Aceptar**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Permitir red NTLM cuando el usuario está restringido a determinados dispositivos Unidos a dominio

A partir de Windows Server 2016 nivel funcional del dominio (DFL), los controladores de dominio puede admitir la red permitiendo NTLM cuando un usuario está restringido a determinados dispositivos Unidos a dominio. Esta característica no está disponible en DFLs inferior.

Configuración: En la directiva de autenticación, haga clic en **la autenticación de red permiten NTLM cuando el usuario está restringido a los dispositivos seleccionados**. 

[Más información sobre las directivas de autenticación](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
