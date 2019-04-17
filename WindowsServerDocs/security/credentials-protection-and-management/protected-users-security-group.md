---
title: Grupo de seguridad de los usuarios protegido
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bd6b53c0febdfb2d344136097a9654c981405568
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="protected-users-security-group"></a>Grupo de seguridad de los usuarios protegido

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI se describe el grupo de seguridad de Active Directory protegido a los usuarios y se explica cómo funciona. Este grupo se introdujo en controladores de dominio de Windows Server 2012 R2.

## <a name="BKMK_ProtectedUsers"></a>Introducción

Este grupo de seguridad está diseñado como parte de una estrategia para administrar credenciales exposición dentro de la empresa. Los miembros de este grupo automáticamente tienen no configurables protecciones aplicadas a sus cuentas. Pertenencia al grupo usuarios protegido está diseñada para ser restrictivas y seguro de forma proactiva de manera predeterminada. Es el único método para modificar estas protecciones de una cuenta quitar la cuenta del grupo de seguridad.

> [!WARNING]
> Cuentas de servicios y equipos jamás deben ser miembros del grupo usuarios protegido. Este grupo se proporciona protección incompleta todos modos porque la contraseña o certificado siempre está disponible en el host. Se producirá un error de autenticación con el nombre de usuario de \"the de error o la contraseña es incorrect\" para cualquier servicio o un equipo que se agrega al grupo usuarios protegido.

Este grupo global, relacionados con el dominio desencadena protección no se pueden configurar en los dispositivos y equipos host que ejecutan Windows Server 2012 R2 y Windows 8.1 o posterior para los usuarios de dominios con un controlador de dominio principal que se ejecute Windows Server 2012 R2. Esto reduce en gran medida la superficie de memoria de manera predeterminada de credenciales cuando los usuarios iniciar sesión en equipos con estas protecciones.

Para obtener más información, consulta [cómo los usuarios protegidos grupo funciona](#BKMK_HowItWorks) en este tema.



## <a name="BKMK_Requirements"></a>Requisitos de grupo de usuarios protegidos
Requisitos para proporcionar las protecciones de dispositivo para los miembros del grupo usuarios protegida se incluyen:

- El grupo de seguridad global de usuarios protegidos se replica en todos los controladores de dominio en el dominio de cuenta.

- Windows 8.1 y Windows Server 2012 R2 se ha agregado compatibilidad de manera predeterminada. [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) agrega compatibilidad para Windows 7, Windows Server 2008 R2 y Windows Server 2012.

Requisitos para proporcionar protección de controlador de dominio para los miembros del grupo usuarios protegido incluyen:

- Los usuarios deben en los dominios de Windows Server 2012 R2 o posterior nivel funcional del dominio.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Agregar grupo de seguridad global del usuario protegido para dominios de nivel inferior

Controladores de dominio que ejecuten un sistema operativo anterior a Windows Server 2012 R2 pueden admitir agregar miembros al nuevo grupo de seguridad de usuario protegido. Esto permite a los usuarios beneficiarse de protecciones de dispositivo antes de actualiza el dominio. 

> [!Note]
> Los controladores de dominio no será compatible con las protecciones de dominio. 

Grupo de usuarios protegido puede crearse mediante [transferir el rol de emulador de dominio principal (PDC) del controlador](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) a un controlador de dominio que ejecute Windows Server 2012 R2. Después de ese objeto de grupo se replica en otros controladores de dominio, el rol de emulador PDC puede estar alojado en un controlador de dominio que ejecute una versión anterior de Windows Server.

### <a name="BKMK_ADgroup"></a>Propiedades de grupo AD protegidas de los usuarios

La tabla siguiente especifica las propiedades del grupo usuarios protegido.

|Atributo|Valor|
|-------|-----|
|SID/RID conocida|S-1-5-21 -<domain>-525|
|Tipo|Global de dominio|
|Contenedor predeterminado|CN = usuarios, DC =<domain>, DC =|
|Miembros predeterminados|Ninguno|
|Miembro predeterminado de|Ninguno|
|¿Protegido por ADMINSDHOLDER?|No|
|¿Mueve fuera del contenedor predeterminado es seguro?|Sí|
|¿Delegar la administración de este grupo de administradores de servicio no es seguro?|No|
|Derechos de usuario predeterminado|Sin derechos de usuario predeterminado|

## <a name="BKMK_HowItWorks"></a>Cómo funciona el grupo usuarios protegido
Esta sección explica cómo el grupo usuarios protegido funciona cuando:

-   Registra en un dispositivo de Windows

-   Dominio de la cuenta de usuario está en un Windows Server 2012 R2 o posterior nivel funcional del dominio

### <a name="device-protections-for-signed-in-protected-users"></a>Protecciones de dispositivo para iniciado sesión los usuarios protegido
Cuando el usuario inició sesión un miembro del grupo usuarios protegido se aplican las siguientes protecciones:

-   Credenciales voluntad delegación (CredSSP) no caché las credenciales del usuario texto sin formato incluso cuando la **Permitir delegación de credenciales predeterminadas** se habilita la configuración de directiva de grupo.

-   A partir de Windows 8.1 y Windows Server 2012 R2, Windows implícita se almacena en caché las credenciales del usuario texto sin formato incluso cuando está habilitado el resumen de Windows.

> [!Note]
> Después de instalar [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) implícita de Windows seguirán credenciales almacenadas en caché hasta que se configura la clave del registro. Consulta [aviso de seguridad de Microsoft: actualización para mejorar la protección y administración de credenciales: 13 de mayo de 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) para obtener instrucciones.

-   NTLM no se almacena en caché las credenciales del usuario texto sin formato o función unidireccional de NT (NTOWF).

-   Kerberos dejan de crear claves DES o RC4. También se almacena en caché al usuario las credenciales de texto sin formato o a largo plazo claves después de adquirir el TGT inicial.

-   Un comprobador almacenado en caché no se crea al iniciar sesión o desbloqueo, por lo que ya no se admite el inicio de sesión sin conexión.

Después de agrega la cuenta de usuario al grupo usuarios protegido, protección se iniciará cuando el usuario inicia sesión en el dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Protecciones de controlador de dominio para los usuarios protegido
Las cuentas que son miembros del grupo usuarios protegido que autentican a un dominio de Windows Server 2012 R2 son no se puede:

-   Autenticar con la autenticación NTLM.

-   Usar tipos de cifrado DES o RC4 en la autenticación previa de Kerberos.

-   Delegarse con la delegación restringida o sin restricciones.

-   Renovar los vales Kerberos más allá de la duración de cuatro horas inicial.

Configuración no son configurables para la expiración TGT se establece para cada cuenta en el grupo usuarios protegido. Normalmente, el controlador de dominio establece la duración de TGT y renovación, en función de las directivas de dominio, **vigencia máxima del vale de usuario** y **vigencia máxima de renovación de vales de usuario**. El grupo usuarios protegido, 600 minutos se establece para estas directivas de dominio.

Para obtener más información, consulta [cómo configurar cuentas protegido](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Solución de problemas
Dos los registros operativos administrativos están disponibles para ayudar a solucionar los eventos que están relacionados con los usuarios protegido. Estos registros nuevas se encuentran en el Visor de eventos están deshabilitados de manera predeterminada y se encuentran en **aplicaciones y servicios Logs\Microsoft\Windows\Microsoft\Authentication**.

|Registro y el identificador de evento|Descripción|
|----------|--------|
|104<br /><br />**ProtectedUser cliente**|Causa: El paquete de seguridad en el cliente no contiene las credenciales.<br /><br />Cuando la cuenta es miembro del grupo de seguridad de los usuarios protegido, el error se registra en el equipo cliente. Este evento indica que el paquete de seguridad no almacenar en caché las credenciales que son necesarios para autenticar en el servidor.<br /><br />Muestra el nombre del paquete, el nombre de usuario, el nombre de dominio y el nombre del servidor.|
|304<br /><br />**ProtectedUser cliente**|Causa: El paquete de seguridad no almacena las credenciales de usuario protegido.<br /><br />En el cliente para indicar que el paquete de seguridad no almacenar en caché las credenciales del usuario de inicio de sesión, se registra un evento informativo. Se espera que no tengan las credenciales de inicio de sesión para los usuarios protegido implícita (WDigest), la delegación de credenciales (CredSSP) y NTLM. Aplicaciones aún pueden realizarse correctamente si te piden las credenciales.<br /><br />Muestra el nombre del paquete, el nombre de usuario y el nombre de dominio.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Causa: Se produce un error de inicio de sesión de NTLM para una cuenta que es miembro del grupo de seguridad de los usuarios protegido.<br /><br />Se registra un error en el controlador de dominio para indicar que ha fallado la autenticación NTLM porque la cuenta ya era miembro del grupo de seguridad de los usuarios protegido.<br /><br />Muestra el nombre de cuenta y nombre del dispositivo.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Causa: Tipos de cifrado DES o RC4 se usan para la autenticación de Kerberos y se produce un error de inicio de sesión de un usuario en el grupo de seguridad de usuario protegido.<br /><br />No se pudo realizar la autenticación previa de Kerberos porque no se pueden usar los tipos de cifrado DES y RC4 cuando la cuenta es miembro del grupo de seguridad de los usuarios protegido.<br /><br />(AES es aceptable).|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Motivo: Un Kerberos vale-concesión de vales (TGT) se emitió correctamente para un miembro del grupo de usuarios protegido.|



## <a name="additional-resources"></a>Recursos adicionales

-   [Administración y protección de credenciales](credentials-protection-and-management.md)

-   [Directivas de autenticación y Silos de directiva de autenticación](authentication-policies-and-authentication-policy-silos.md)

-   [Cómo configurar cuentas protegidas](how-to-configure-protected-accounts.md)


