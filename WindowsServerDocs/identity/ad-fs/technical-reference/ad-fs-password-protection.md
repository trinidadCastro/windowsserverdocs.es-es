---
title: AD FS protección contra ataques de contraseñas
description: En este documento se describe cómo proteger AD FS usuarios frente a ataques de contraseñas
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 68624e2307e5ddefc6e32160cabfcd140f609966
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407245"
---
# <a name="ad-fs-password-attack-protection"></a>AD FS protección contra ataques de contraseñas

## <a name="what-is-a-password-attack"></a>¿Qué es un ataque de contraseña?

Un requisito para el inicio de sesión único federado es la disponibilidad de los puntos de conexión para autenticarse a través de Internet. La disponibilidad de los extremos de autenticación en Internet permite a los usuarios obtener acceso a las aplicaciones incluso cuando no están en una red corporativa. 

Sin embargo, esto también significa que algunos actores no válidos pueden aprovechar los puntos de conexión federados disponibles en Internet y usar estos puntos de conexión para probar y determinar contraseñas o para crear ataques de denegación de servicio. Uno de estos ataques es cada vez más común y se denomina **ataque de contraseña**. 

Hay dos tipos de ataques de contraseña comunes. Ataque con aerosol de contraseña & ataques por fuerza bruta de contraseñas. 

### <a name="password-spray-attack"></a>Ataque de pulverización de contraseñas
En un ataque de pulverización de contraseña, estos actores no válidos intentarán usar las contraseñas más comunes en muchas cuentas y servicios diferentes para obtener acceso a los recursos protegidos por contraseña que encuentren. Normalmente, se trata de muchas organizaciones y proveedores de identidades diferentes. Por ejemplo, un atacante usará un kit de herramientas disponible con frecuencia para enumerar todos los usuarios de varias organizaciones y, a continuación, probar "P @ $ $w 0rd" y "Contraseña1" en todas esas cuentas. Para darle la idea, un ataque podría ser similar al siguiente:


|  Usuario de destino   | Contraseña de destino |
|----------------|-----------------|
| User1@org1.com |    Contraseña1    |
| User2@org1.com |    Contraseña1    |
| User1@org2.com |    Contraseña1    |
| User2@org2.com |    Contraseña1    |
|       …        |        …        |
| User1@org1.com |    P @ $ $w 0rd     |
| User2@org1.com |    P @ $ $w 0rd     |
| User1@org2.com |    P @ $ $w 0rd     |
| User2@org2.com |    P @ $ $w 0rd     |

Este patrón de ataque eludirá la mayoría de las técnicas de detección porque, desde el punto de estratégicos de un usuario individual o una empresa, el ataque solo se parece a un inicio de sesión con errores aislado.

En el caso de los atacantes, es un juego de números: saben que hay algunas contraseñas que son muy comunes.  El atacante obtendrá algunos éxitos por cada mil cuentas atacadas y eso es suficiente para ser efectivo. Usan las cuentas para obtener datos de los correos electrónicos, recopilar información de contacto y enviar vínculos de suplantación de identidad (phishing) o simplemente expandir el grupo de destino aspersor de contraseñas. Los atacantes no se interesan mucho de quiénes son los objetivos iniciales, solo que tienen algunos éxitos que pueden aprovechar.

Pero al realizar algunos pasos para configurar el AD FS y la red correctamente, AD FS puntos de conexión se pueden proteger contra este tipo de ataques. En este artículo se tratan tres áreas que deben configurarse correctamente para protegerse frente a estos ataques.

### <a name="brute-force-password-attack"></a>Ataque por fuerza bruta de contraseñas 
En este tipo de ataque, un atacante intentará varios intentos de contraseña en un conjunto de cuentas de destino. En muchos casos, estas cuentas se dirigirán a los usuarios que tengan un nivel de acceso superior dentro de la organización. Pueden ser ejecutivos de la organización o administradores que administran la infraestructura crítica.  

Este tipo de ataque también podría dar lugar a patrones de DOS. Puede tratarse del nivel de servicio en el que ADFS no puede procesar un número elevado de solicitudes debido a un número insuficiente de servidores o podría estar en un nivel de usuario en el que un usuario está bloqueado en su cuenta.  

## <a name="securing-ad-fs-against-password-attacks"></a>Protección de AD FS contra ataques de contraseñas 

Pero al realizar algunos pasos para configurar el AD FS y la red correctamente, AD FS puntos de conexión se pueden proteger contra estos tipos de ataques. En este artículo se tratan tres áreas que deben configurarse correctamente para protegerse frente a estos ataques. 


- Nivel 1, línea base: Esta es la configuración básica que debe configurarse en un servidor de AD FS para asegurarse de que los actores no válidos no pueden provocar ataques por fuerza bruta a usuarios federados. 
- Nivel 2: protección de la extranet: Estos son los valores que se deben configurar para asegurarse de que el acceso a la extranet está configurado para usar protocolos seguros, directivas de autenticación y las aplicaciones adecuadas. 
- Nivel 3, desplace a contraseña: menos para el acceso a Extranet: Se trata de una configuración avanzada e instrucciones para permitir el acceso a recursos federados con credenciales más seguras en lugar de contraseñas que son propensos a ataques. 

## <a name="level-1-baseline"></a>Nivel 1: BASE1

1. Si ADFS 2016, implemente el bloqueo inteligente de extranet de acceso directo de [extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) realiza un seguimiento de las ubicaciones conocidas y permite que un usuario válido se lleve a cabo si previamente ha iniciado sesión correctamente desde esa ubicación. Mediante el uso del bloqueo inteligente de extranet, puede asegurarse de que los actores no válidos no puedan realizar ataques por fuerza bruta a los usuarios y, al mismo tiempo, permitan que el usuario legítimo sea productivo.
    - Si no está en AD FS 2016, se recomienda encarecidamente que [actualice](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) a AD FS 2016. Se trata de una ruta de actualización sencilla desde AD FS 2012 R2. Si está en AD FS 2012 R2, implemente el bloqueo de la [extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). Una desventaja de este enfoque es que los usuarios válidos pueden estar bloqueados para el acceso a la extranet si se encuentra en un patrón de fuerza bruta. AD FS en el servidor 2016 no tiene este inconveniente.

2. Supervisar & bloquear direcciones IP sospechosas 
    - Si tiene Azure AD Premium, implemente el estado de Connect para ADFS y use las notificaciones de [Informe de IP de riesgo](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) que proporciona.

        a. La concesión de licencias no es para todos los usuarios y requiere 25 licencias/ADFS/servidor WAP, lo que puede ser fácil para un cliente.

        b. Ahora puede investigar las IP que están generando un número elevado de inicios de sesión con errores.

        c. Esto le obligará a habilitar la auditoría en los servidores de ADFS.

3.  Bloquear IP sospechosas.  Esto bloquea potencialmente los ataques de DOS.

    a. Si en 2016, use la característica de [*direcciones IP prohibidas*](../../ad-fs/operations/configure-ad-fs-banned-ip.md) de la extranet para bloquear cualquier solicitud de IP marcada por #3 (o análisis manual).

    b. Si está en AD FS 2012 R2 o inferior, bloquee la dirección IP directamente en Exchange Online y, opcionalmente, en el firewall.

4. Si tiene Azure AD Premium, use [Azure ad protección con contraseña](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) para evitar que las contraseñas adivinables se adquieran Azure ad  

    a. Tenga en cuenta que si tiene contraseñas adivinables, puede descifrarlas con solo 1-3 intentos. Esta característica evita que se establezcan. 

    b. Desde nuestras estadísticas de vista previa, casi el 20-50% de las nuevas contraseñas se bloquean de su establecimiento. Esto implica que el% de los usuarios son vulnerables a las contraseñas con una adivinación sencilla.

## <a name="level-2-protect-your-extranet"></a>Nivel 2: Proteger la extranet

5. Desplácese a la autenticación moderna para los clientes que tienen acceso desde la extranet. Los clientes de correo forman parte de este. 

    a. Tendrá que usar Outlook Mobile para dispositivos móviles. La nueva aplicación de correo electrónico nativo de iOS también admite la autenticación moderna. 

    b. Tendrá que usar Outlook 2013 (con las últimas revisiones de CU) o Outlook 2016.

6. Habilite MFA para todo el acceso a la extranet. Esto le proporciona protección adicional para cualquier acceso a la extranet.

   a.  Si tiene Azure AD Premium, use [Azure ad directivas de acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) para controlar esto.  Esto es mejor que implementar las reglas en AD FS.  Esto se debe a que las aplicaciones cliente modernas se aplican con mayor frecuencia.  Esto ocurre, en Azure AD, al solicitar un nuevo token de acceso (normalmente cada hora) mediante un token de actualización.  

   b.  Si no tiene Azure AD Premium o tiene aplicaciones adicionales en AD FS que permite el acceso basado en Internet, implemente MFA (puede ser Azure MFA también en AD FS 2016) y una [Directiva de MFA global](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) para todo el acceso a la extranet.

## <a name="level-3-move-to-password-less-for-extranet-access"></a>Nivel 3: Moverse a la contraseña menos para el acceso a Extranet

7. Desplácese a la ventana 10 y use [Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. En el caso de otros dispositivos, si en AD FS 2016, puede usar la [OTP de Azure MFA](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) como primer factor y contraseña como segundo factor. 

9. En el caso de los dispositivos móviles, si solo permite dispositivos administrados por MDM, puede usar [certificados](../../ad-fs/operations/configure-user-certificate-authentication.md) para iniciar la sesión del usuario. 

## <a name="urgent-handling"></a>Control urgente

Si el entorno de AD FS está bajo ataque activo, los siguientes pasos se deben implementar en el momento más antiguo:

 - Deshabilite el punto de conexión U/P en ADFS y solicite a todos los usuarios a VPN que obtengan acceso o que estén dentro de la red. Esto requiere que el nivel de paso **2 #1a** completado. De lo contrario, todas las solicitudes internas de Outlook se enrutarán a través de la nube a través de la autenticación del proxy EXO.
 - Si el ataque solo llega a través de EXO, puede deshabilitar la autenticación básica para los protocolos de Exchange (POP, IMAP, SMTP, EWS, etc.) mediante las directivas de autenticación, estos protocolos y métodos de autenticación se usan en la gran mayoría de estos ataques. Además, las reglas de acceso de cliente en EXO y la habilitación del Protocolo por buzón se evalúan después de la autenticación y no ayudan a mitigar los ataques. 
 - Ofrezca acceso de forma selectiva a Extranet con el nivel 3 #1-3.

## <a name="next-steps"></a>Pasos siguientes

- [Actualizar al servidor de AD FS 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Bloqueo inteligente de extranet en AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurar directivas de acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Azure AD protección con contraseña](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
