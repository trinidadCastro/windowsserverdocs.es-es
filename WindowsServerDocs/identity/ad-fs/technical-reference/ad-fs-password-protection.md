---
title: Protección frente a ataques de contraseña de FS AD
description: Este documento describe cómo proteger a los usuarios de AD FS frente a ataques de contraseña
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de1af9712b54c977c591953c68eec506c80d3cdd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822006"
---
# <a name="ad-fs-password-attack-protection"></a>Protección frente a ataques de contraseña de FS AD

## <a name="what-is-a-password-attack"></a>¿Qué es un ataque de contraseña?

Un requisito para un inicio de sesión único federado es la disponibilidad de puntos de conexión para autenticar a través de internet. La disponibilidad de extremos de autenticación de internet permite a los usuarios acceder a las aplicaciones incluso cuando no estén en una red corporativa. 

Sin embargo, esto también significa que algunos los actores no válidos puede aprovechar las ventajas de los puntos de conexión federados disponibles en internet y usar estos puntos de conexión e intente determinar las contraseñas o crear la denegación de servicio. Se llama a un ataque de este tipo que se está generalizando un **ataque de contraseña**. 

Hay 2 tipos comunes de ataques de contraseña. Ataque de contraseña aerosol & bruta forzar ataque de contraseña. 

### <a name="password-spray-attack"></a>Ataque de protector de contraseña
En un ataque de protector de contraseña, estos actores no válidos tratará las contraseñas más común en muchas cuentas diferentes y servicios para obtener acceso a los recursos protegidos por contraseña que pueden encontrar. Normalmente, estos abarcan muchas organizaciones diferentes y proveedores de identidades. Por ejemplo, un atacante usará un Kit de herramientas normalmente disponible para enumerar todos los usuarios en varias organizaciones y, a continuación, intente "P@$$w0rd" y "Password1" con todas esas cuentas. Para darle la idea, un ataque podría ser similar:

|Usuario de destino|Contraseña de destino|
|-----|-----|-----|
|User1@org1.com|Password1|
|User2@org1.com|Password1|
|User1@org2.com|Password1|
|User2@org2.com|Password1|
|…|…|
|User1@org1.com|P@$$w0rd|
|User2@org1.com|P@$$w0rd|
|User1@org2.com|P@$$w0rd|
|User2@org2.com|P@$$w0rd|

Este patrón de ataque evades la mayoría de las técnicas de detección porque desde el punto de estratégicos de un usuario individual o de la empresa, el ataque ve como un inicio de sesión con errores aislado.

Para los atacantes, es un juego de números: saben que hay ahí fuera algunas contraseñas son muy comunes.  El atacante recibirá algunos éxitos por cada mil cuentas atacadas y eso es suficiente para ser efectivo. Utilizan las cuentas para obtener datos de los correos electrónicos, recopilar información de contacto y envío de vínculos de "phishing" o simplemente expanda el grupo de destino de protector de contraseña. Los atacantes no importa mucho sobre quiénes son los destinos iniciales, solo que tienen algo de éxito que pueden utilizar.

Utilizan las cuentas para obtener datos de los correos electrónicos, recopilar información de contacto y envío de vínculos de "phishing" o simplemente expanda el grupo de destino de protector de contraseña. Los atacantes no importa mucho sobre quiénes son los destinos iniciales, solo que tienen algo de éxito que pueden utilizar.

Pero, al tomar unos pasos para configurar AD FS y de red correctamente, se pueden proteger los extremos de AD FS frente a estos tipos de ataques. En este artículo abarca 3 áreas que deben configurarse correctamente para ayudar a proteger frente a estos ataques.

### <a name="brute-force-password-attack"></a>Ataques de fuerza bruta 
En esta forma de ataque, un atacante tratará de varios intentos de contraseña en un conjunto de cuentas de destino. En muchos casos, estas cuentas se destinará frente a los usuarios que tienen un mayor nivel de acceso dentro de la organización. Podría tratarse de ejecutivos dentro de la organización o administradores que administran la infraestructura crítica.  

También podría dar este tipo de ataque en los patrones de denegación de servicio. Esto puede ser en el nivel de servicio donde es no se puede procesar un gran número de solicitudes debido a un número insuficiente de servidores de AD FS o podría estar en un nivel de usuario donde un usuario está bloqueado en su cuenta.  

## <a name="securing-ad-fs-against-password-attacks"></a>Protección de AD FS frente a ataques de contraseña 
 
Pero, al tomar unos pasos para configurar AD FS y de red correctamente, se pueden proteger los extremos de AD FS frente a estos tipos de ataques. En este artículo abarca 3 áreas que deben configurarse correctamente para ayudar a proteger frente a estos ataques. 


- Nivel 1, línea de base: Estas son las opciones básicas que se deben configurar en un servidor de AD FS para asegurarse de que los actores no válidos no pueden hacerlo los usuarios federada de ataque de fuerza bruta. 
- Nivel 2, protección de la extranet: Estos son los valores que se deben configurar para garantizar que el acceso de extranet está configurado para usar protocolos seguros, las directivas de autenticación y aplicaciones adecuadas. 
- Nivel 3, desplácese a sin contraseña para el acceso de extranet: Estos son avanzados opciones e instrucciones para habilitar el acceso a los recursos federados con las credenciales más seguro en lugar de contraseñas, lo que son propensas a los ataques. 

## <a name="level-1-baseline"></a>Nivel 1: Línea base

1. Si implementa AD FS 2016, [bloqueo inteligente extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) Extranet bloqueo inteligente realiza el seguimiento de ubicaciones conocidas y permitirá que un usuario válido que lleguen a través si anteriormente iniciar sesión correctamente desde esa ubicación. Mediante el uso de bloqueo inteligente de extranet, puede asegurarse de que los actores no válidos no será capaz de fuerza bruta atacar a los usuarios y al mismo tiempo le permitirá al usuario legítimo a ser productivo.
    - Si no está en AD FS 2016, se recomienda encarecidamente [actualizar](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) a AD FS 2016. Es una ruta de actualización sencilla de AD FS 2012 R2. Si se encuentra en AD FS 2012 R2, implementar [bloqueo de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). Una desventaja de este enfoque es que los usuarios válidos pueden estar bloqueados contra el acceso de extranet si se encuentra en un patrón de fuerza bruta. AD FS en Server 2016 no presenta este inconveniente.

2. Monitor de & bloque de direcciones IP sospechosas 
    - Si tiene Azure AD Premium, que implemente Connect Health para AD FS y use la [informe de direcciones IP de riesgo](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) notificaciones que proporciona.
        
        a. Licencias no es para todos los usuarios y requiere 25 licencias/ADFS/WAP servidor que puede ser fácil para un cliente.
    
        b. Ahora puede investigar IP que está generando gran número de inicios de sesión erróneos
    
        c. Esto requerirá habilitar la auditoría en los servidores ADFS.

3.  Bloquear la dirección IP sospechosa.  Potencialmente, esto impide ataques de denegación de servicio.

    a. Si se encuentra en 2016, use el [ *direcciones IP no permitidas de Extranet* ](../../ad-fs/operations/configure-ad-fs-banned-ip.md) característica para bloquear todas las solicitudes de direcciones IP de marcado por #3 (o análisis manual).

    b. Si se encuentra en AD FS 2012 R2 o inferior, bloquear la dirección IP directamente en Exchange Online y, opcionalmente, en el firewall.

4. Si tiene Azure AD Premium, use [protección con contraseña de Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) para impedir que las contraseñas de adivinar introducción en Azure AD  

    a. Tenga en cuenta que si tiene contraseñas adivinar, puede averiguarlos con solo 1-3 intentos. Esta característica evita que estas desde la configuración. 

    b. En nuestras estadísticas de vista previa, casi 20 y un 50% de las nuevas contraseñas bloquearse desde que se va a establecer. Esto implica que el % de los usuarios son vulnerables a las contraseñas puede adivinadas con facilidad.

## <a name="level-2-protect-your-extranet"></a>Nivel 2: Proteger la extranet

5. Mover a la autenticación moderna para los clientes con acceso desde la extranet. Los clientes de correo electrónico son una parte importante de esto. 

    a. Deberá usar Outlook Mobile para dispositivos móviles. La nueva aplicación de correo electrónico nativo de iOS admite también la autenticación moderna. 

    b. Deberá usar Outlook 2013 (con las últimas revisiones CU) o Outlook 2016.

6.  Habilitar MFA para todos los accesos de extranet. Este proporciona un mayor protección para cualquier acceso de extranet.

    a.  Si dispone de Azure AD premium, use [las directivas de acceso condicional de Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) para controlar esto.  Esto es mejor que la implementación de las reglas en AD FS.  Esto es porque las aplicaciones cliente modernas se aplican con más frecuencia.  Esto ocurre, en Azure AD, cuando se solicita un nuevo token de acceso (normalmente cada hora) con un token de actualización.  

    b.  Si no dispone de Azure AD premium o disponen de acceso de basado en aplicaciones adicionales en AD FS que se permite en internet, implemente MFA (puede estar Azure MFA y AD FS 2016) y realice un [directiva MFA global](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) para todos los accesos de extranet.
 
## <a name="level-3-move-to-password-less-for-extranet-access"></a>Nivel 3: Mover a una contraseña menos para acceso de extranet

7. Mover a Windows 10 y usar [Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. Para otros dispositivos, si se encuentra en AD FS 2016, puede usar [Azure MFA OTP](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) como el primer factor y contraseña como 2nd factor. 

9.  Para dispositivos móviles, si sólo permite que los dispositivos administrados por MDM, puede usar [certificados](../../ad-fs/operations/configure-user-certificate-authentication.md) para iniciar sesión el usuario. 
 
## <a name="urgent-handling"></a>Control urgente

Si el entorno de AD FS está sufriendo un ataque activo, los pasos siguientes deben implementarse lo antes posible:

 - Deshabilitar punto de conexión de U y P en ADFS y requieren todos los usuarios a obtener acceso o estar dentro de la red VPN. Esto requiere que tenga paso **nivel 2 #1a** completado. De lo contrario, todas las solicitudes de Outlook internas todavía se enrutarán a través de la nube a través de la autenticación de EXO proxy.
 - Si el ataque solo estará disponible a través de EXO, puede deshabilitar la autenticación básica para los protocolos de Exchange (POP, IMAP, SMTP, EWS, etcetera) mediante las directivas de autenticación, estos protocolos y métodos de autenticación se usan en la mayoría de estos ataques. Además, las reglas de acceso de cliente en la habilitación de protocolo EXO y por buzón son evaluada autenticación posterior y no ayudará en mitigar los ataques. 
 - Selectivamente ofrecen acceso a extranet mediante el nivel 3 #1-3.

## <a name="next-steps"></a>Pasos siguientes

- [Actualizar al servidor de AD FS 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Bloqueo inteligente de la extranet de AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurar directivas de acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Protección mediante contraseña de Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
