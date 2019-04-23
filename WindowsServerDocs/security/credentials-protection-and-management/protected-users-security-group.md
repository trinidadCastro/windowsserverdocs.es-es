---
title: Grupo de seguridad de usuarios protegidos
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862366"
---
# <a name="protected-users-security-group"></a>Grupo de seguridad de usuarios protegidos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema dirigido a profesionales de TI se describe el grupo de seguridad de usuarios protegidos de Active Directory y se explica cómo funciona. Este grupo se introdujo en los controladores de dominio de Windows Server 2012 R2.

## <a name="BKMK_ProtectedUsers"></a>Información general

Este grupo de seguridad está diseñado como parte de una estrategia para administrar la exposición de credenciales dentro de la empresa. Las cuentas de los miembros de este grupo reciben automáticamente protecciones que no son configurables. La pertenencia al grupo de usuarios protegidos es de naturaleza restrictiva y segura proactivamente de forma predeterminada. El único método con el que estas protecciones de cuenta se pueden modificar consiste en quitar la cuenta del grupo de seguridad.

> [!WARNING]
> Cuentas de servicios y equipos nunca deben ser miembros del grupo usuarios protegidos. Este grupo podría proporciona protección incompleta de todos modos porque la contraseña o certificado siempre está disponible en el host. Se producirá un error de autenticación con el error \"el nombre de usuario o la contraseña es incorrecta\" para cualquier servicio o equipo que se agrega al grupo usuarios protegidos.

Este grupo global, relacionados con el dominio activa una protección no configurable en dispositivos y equipos host que ejecutan Windows Server 2012 R2 y Windows 8.1 o posterior para los usuarios en dominios con un controlador de dominio principal que ejecuta Windows Server 2012 R2. Esto reduce en gran medida el consumo de memoria predeterminada de credenciales cuando los usuarios iniciar sesión en equipos con estas protecciones.

Para obtener más información, consulte [cómo los usuarios protegidos grupo funciona](#BKMK_HowItWorks) en este tema.



## <a name="BKMK_Requirements"></a>Requisitos de grupo de usuarios protegidos
Requisitos para proporcionar protección de dispositivos para los miembros del grupo usuarios protegidos se incluyen:

- El grupo de seguridad global de usuarios protegidos se replica en todos los controladores de dominio del dominio de la cuenta.

- Windows 8.1 y Windows Server 2012 R2 agregan soporte de forma predeterminada. [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) agrega compatibilidad para Windows 7, Windows Server 2008 R2 y Windows Server 2012.

Estos son los requisitos para proporcionar protección de controlador de dominio a los miembros del grupo de usuarios protegidos:

- Los usuarios deben estar en dominios que tienen el mayor nivel funcional del dominio o Windows Server 2012 R2.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Agregar grupo de seguridad global de usuarios protegidos a los dominios de nivel inferior

Controladores de dominio que ejecuten un sistema operativo anterior a Windows Server 2012 R2 permite agregar miembros al nuevo grupo de seguridad usuarios protegidos. Esto permite a los usuarios para beneficiarse de las protecciones de dispositivo antes de actualiza el dominio. 

> [!Note]
> Los controladores de dominio no admitirá las protecciones de dominio. 

Grupo de usuarios protegidos se puede crear por [transfiriendo el rol de emulador de controlador (PDC) de dominio principal](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) a un controlador de dominio que ejecute Windows Server 2012 R2. Una vez replicado ese grupo a otros controladores de dominio, el rol de emulador de PDC se puede hospedar en un controlador de dominio que ejecuta una versión anterior de Windows Server.

### <a name="BKMK_ADgroup"></a>Propiedades de grupo de AD a los usuarios protegidas

En la siguiente tabla se especifican las propiedades del grupo de usuarios protegidos.

|Atributo|Valor|
|-------|-----|
|SID/RID conocido|S-1-5-21-<domain>-525|
|Tipo|Global de dominio|
|Contenedor predeterminado|CN=Usuarios, DC=<domain>, DC=|
|Miembros predeterminados|Ninguno|
|Miembro predeterminado de|Ninguno|
|¿Protegido por ADMINSDHOLDER?|No|
|¿Es seguro sacarlo del contenedor predeterminado?|Sí|
|¿Es seguro delegar la administración de este grupo en administradores que no son de servicio?|No|
|Derechos de usuario predeterminados|No hay derechos de usuario predeterminados.|

## <a name="BKMK_HowItWorks"></a>Cómo funciona el grupo de usuarios protegidos
En esta sección se describe el funcionamiento del grupo de usuarios protegidos en las siguientes circunstancias:

-   Inicien sesión en un dispositivo de Windows

-   Dominio de la cuenta de usuario está en un mayor nivel funcional del dominio o en Windows Server 2012 R2

### <a name="device-protections-for-signed-in-protected-users"></a>Protecciones de dispositivo para que inicien sesión de usuarios protegidos
Cuando el usuario ha iniciado sesión es miembro del grupo usuarios protegidos se aplican los siguientes mecanismos de protección:

-   Credencial will delegación (CredSSP) no almacenar en caché las credenciales del usuario texto sin formato incluso cuando el **Permitir delegación de credenciales predeterminadas** está habilitada la configuración de directiva de grupo.

-   A partir de Windows 8.1 y Windows Server 2012 R2, Windows implícita no almacenará en caché las credenciales del usuario texto sin formato incluso cuando está habilitada la síntesis de Windows.

> [!Note]
> Después de instalar [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) continuará implícita de Windows para credenciales almacenadas en caché hasta que se configura la clave del registro. Consulte [documento informativo sobre seguridad de Microsoft: Actualización para mejorar la protección y administración de credenciales: 13 de mayo de 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) para obtener instrucciones.

-   NTLM no almacenará en caché las credenciales del usuario texto sin formato o función unidireccional de NT (NTOWF).

-   Kerberos ya no creará las claves DES o RC4. También lo no almacenará en caché las credenciales de texto sin formato o claves a largo plazo del usuario después de adquirir el TGT inicial.

-   Un comprobador en caché no se crea al iniciar sesión o desbloquear, por lo que ya no se admite el inicio de sesión sin conexión.

Después de agrega la cuenta de usuario al grupo usuarios protegidos, protección se iniciará cuando el usuario inicia sesión en el dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Protecciones de controlador de dominio para los usuarios protegidos
Las cuentas que son miembros del grupo usuarios protegidos que se autentican en un dominio de Windows Server 2012 R2 son no se puede:

-   Autenticarse mediante la autenticación NTLM.

-   Usar los tipos de cifrado DES o RC4 en la autenticación previa de Kerberos.

-   Delegarse mediante una delegación con o sin restricciones.

-   Renovar los TGT de Kerberos una vez transcurridas las cuatro horas de vigencia.

Las opciones no configurables de expiración de los TGT se establecen en cada cuenta individual del grupo de usuarios protegidos. Por lo general, el controlador de dominio establece la vigencia y renovación de los TGT según las directivas de dominio **Vigencia máxima del vale de usuario** y **Vigencia máxima de renovación de vales de usuario**. En el caso del grupo de usuarios protegidos, estas directivas de dominio están establecidas en 600 minutos.

Para obtener más información, consulta [Configurar cuentas protegidas](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Solución de problemas
Existen dos registros administrativos funcionales que son de ayuda a la hora de resolver problemas de eventos relacionados con el grupo de usuarios protegidos. Estos dos nuevos registros se ubican en el Visor de eventos y están deshabilitados de forma predeterminada. Los encontrarás en **Registros de aplicaciones y servicios\Microsoft\Windows\Microsoft\Authentication**.

|Identificador de evento y registro|Descripción|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|Motivo: el paquete de seguridad en el cliente no contiene las credenciales.<br /><br />El error se registra en el equipo cliente cuando la cuenta es miembro del grupo de seguridad de usuarios protegidos. Este evento pone de manifiesto que el paquete de seguridad no almacena en caché las credenciales necesarias para autenticarse en el servidor.<br /><br />Incluye el nombre del paquete, el nombre de usuario, el nombre del dominio y el nombre del servidor.|
|304<br /><br />**ProtectedUser-Client**|Motivo: El paquete de seguridad no almacena las credenciales de usuario protegidos.<br /><br />Se registra un evento informativo en el cliente para indicar que el paquete de seguridad no almacena credenciales de inicio de sesión del usuario en caché. Se espera que la autenticación implícita (WDigest), la delegación de credenciales (CredSSP) y NTLM no puedan tener credenciales para los usuarios protegidos. Las aplicaciones se pueden autenticar correctamente si solicitan credenciales.<br /><br />Incluye el nombre del paquete, el nombre de usuario y el nombre del dominio.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Motivo: se produce un error de inicio de sesión de NTLM en el caso de las cuentas que forman parte del grupo de seguridad de usuarios protegidos.<br /><br />Se registra un error en el controlador de dominio para señalar que la autenticación NTLM no se ha podido llevar a cabo porque la cuenta es miembro del grupo de seguridad de usuarios protegidos.<br /><br />Incluye el nombre de cuenta y el nombre de dispositivo.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Motivo: los tipos de cifrado DES o RC4 se usan en la autenticación Kerberos y un usuario del grupo de seguridad de usuarios protegidos no puede iniciar sesión.<br /><br />La autenticación previa de Kerberos no puede realizarse porque los tipos de cifrado DES y RC4 no se pueden usar cuando la cuenta es miembro del grupo de seguridad de usuarios protegidos.<br /><br />(Se acepta AES.)|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Motivo: se ha emitido correctamente un vale de concesión de vales de Kerberos para un miembro del grupo de usuarios protegidos.|



## <a name="additional-resources"></a>Recursos adicionales

-   [Protección y administración de credenciales](credentials-protection-and-management.md)

-   [Las directivas de autenticación y Silos de directivas de autenticación](authentication-policies-and-authentication-policy-silos.md)

-   [Cómo configurar cuentas protegidas](how-to-configure-protected-accounts.md)


