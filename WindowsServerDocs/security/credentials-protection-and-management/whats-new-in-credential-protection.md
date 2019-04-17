---
title: "Novedades de protección de credenciales"
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
ms.openlocfilehash: 0556c606b987a69eae663b0196467f532d5a307a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-credential-protection"></a>Novedades de protección de credenciales

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard para el usuario que inició sesión

A partir de Windows 10, versión 1507, Kerberos y NTLM usan seguridad basada en virtualización para proteger los secretos de Kerberos y NTLM de la sesión de inicio de sesión de usuario que inició sesión. 

A partir de Windows 10, versión 1511, el Administrador de credenciales usa seguridad basada en virtualización para proteger las credenciales guardadas de tipo de credenciales de dominio. Las credenciales de inicio de sesión y las credenciales de dominio guardadas no se pasarán a un host remoto mediante Escritorio remoto. Se puede habilitar Credential Guard sin bloqueo UEFI.

A partir de Windows 10, versión 1607, modo de usuario aislado se incluye con Hyper-V por lo que ya no se instala por separado para la implementación de Credential Guard.

[Más información sobre Credential Guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Remoto Credential Guard para el usuario que inició sesión

A partir de Windows 10, versión 1607, remoto Credential Guard protege las credenciales de usuario que inició sesión al usar Escritorio remoto mediante la protección de los secretos de Kerberos y NTLM en el dispositivo cliente. Para que el host remoto evaluar los recursos de red como el usuario, las solicitudes de autenticación requieren que el dispositivo de cliente para usar los secretos.

A partir de Windows 10, versión 1703, remoto Credential Guard protege las credenciales de usuario proporcionada al usar Escritorio remoto.

[Más información sobre protección de credenciales remotas](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protecciones de dominio

Protecciones de dominio requieren un dominio de Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Soporte técnico de dispositivo unido al dominio para la autenticación con clave pública

Empezando por versión 1507 de Windows 10 y Windows Server 2016, si un dispositivo unido al dominio es capaz de registrar su clave pública enlazado con un controlador de dominio de Windows Server 2016 (corriente continua), a continuación, el dispositivo puede autenticar con la clave pública mediante la autenticación de Kerberos PKINIT a un controlador de dominio de Windows Server 2016.

A partir de Windows Server 2016, KDC admiten la autenticación usando la confianza clave Kerberos.  

[Más información sobre soporte técnico de clave pública para dispositivos Unidos a un dominio y de confianza de claves Kerberos](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Compatibilidad con las extensiones PKINIT actualidad

A partir de Windows 10, versión 1507 y Windows Server 2016, los clientes de Kerberos intentará la extensión de actualidad PKInit para públicos clave en función de inicios de sesión. 

A partir de Windows Server 2016, KDC pueden admitir la extensión de actualidad PKInit.  De manera predeterminada, KDC no ofrecerá la extensión de actualidad PKInit. 

[Más información sobre la compatibilidad de extensión de actualidad PKINIT](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Giro públicos clave única secretos del usuario NTLM

A partir de Windows Server 2016 nivel funcional del dominio (DFL), controladores de dominio pueden admitir giro secretos NTLM públicos clave única de un usuario. Esta característica es sus en DFLs inferior.

> [!WARNING] 
> Agregar un controlador de dominio a un dominio con sucesiva secretos NTLM habilitados antes de que el controlador de dominio se ha actualizado con al menos 8 de noviembre de 2016 mantenimiento se ejecuta el riesgo de que el controlador de dominio se bloquea. 

Configuración: Para dominios nuevo, esta característica está habilitada de manera predeterminada. Para los dominios existentes, debe configurarse en el centro de administración de Active Directory: 

1. Desde el centro de administración de Active Directory, haz clic en el dominio en el panel izquierdo y selecciona **propiedades**.

    ![Propiedades de dominio](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. Selecciona **habilitar de giro de los secretos NTLM caducados durante el inicio de sesión, para los usuarios que son necesarios para usar Microsoft Passport o una tarjeta inteligente para el inicio de sesión interactivo**.

    ![Secretos NTLM caducados Autoroll](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Haz clic en **Aceptar**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Permitir que la red NTLM cuando el usuario está restringido a dispositivos específicos Unidos a un dominio

A partir de Windows Server 2016 nivel funcional del dominio (DFL), controladores de dominio puede admitir la red que permitiría que NTLM cuando un usuario está restringida a determinados dispositivos Unidos a un dominio. Esta característica no está disponible en DFLs inferior.

Configuración: En la directiva de autenticación, haz clic en **la autenticación de red permitir NTLM cuando el usuario está restringido a los dispositivos seleccionados**. 

[Más información acerca de las directivas de autenticación](https://technet.microsoft.com/en-us/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
