---
title: Grupo de seguridad de usuarios protegidos
description: Obtenga información acerca de la característica de usuarios protegidos del grupo de seguridad de Active Directory y cómo funciona.
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 0e663cf40481fd0c89863cd7cd206e963e2d72e0
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113491"
---
# <a name="protected-users-security-group"></a>Grupo de seguridad de usuarios protegidos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema dirigido a profesionales de TI se describe el grupo de seguridad de usuarios protegidos de Active Directory y se explica cómo funciona. Este grupo se presentó en los controladores de dominio de Windows Server 2012 R2.

## <a name="overview"></a><a name="BKMK_ProtectedUsers"></a>Información general

Este grupo de seguridad está diseñado como parte de una estrategia para administrar la exposición de credenciales en la empresa. Las cuentas de los miembros de este grupo reciben automáticamente protecciones que no son configurables. La pertenencia al grupo de usuarios protegidos es de naturaleza restrictiva y segura proactivamente de forma predeterminada. El único método con el que estas protecciones de cuenta se pueden modificar consiste en quitar la cuenta del grupo de seguridad.

> [!WARNING]
> Las cuentas de servicios y equipos nunca deben ser miembros del grupo usuarios protegidos. Este grupo proporciona protección incompleta, ya que la contraseña o el certificado siempre están disponibles en el host. Se producirá un error de autenticación y el \" nombre de usuario o la contraseña no son correctos \" para cualquier servicio o equipo que se agregue al grupo usuarios protegidos.

Este grupo global relacionado con el dominio desencadena una protección no configurable en dispositivos y equipos host que ejecutan Windows Server 2012 R2 y Windows 8.1 o posterior para los usuarios de dominios con un controlador de dominio principal que ejecuta Windows Server 2012 R2. Esto reduce en gran medida la superficie de memoria predeterminada de las credenciales cuando los usuarios inician sesión en equipos con estas protecciones.

Para obtener más información, vea [Cómo funciona el grupo de usuarios protegidos](#BKMK_HowItWorks) en este tema.


## <a name="protected-users-group-requirements"></a><a name="BKMK_Requirements"></a>Requisitos de grupo de usuarios protegidos
Entre los requisitos para proporcionar protección de dispositivos a los miembros del grupo de usuarios protegidos se incluyen los siguientes:

- El grupo de seguridad global de usuarios protegidos se replica en todos los controladores de dominio del dominio de la cuenta.

- Windows 8.1 y Windows Server 2012 R2 agregaron compatibilidad de forma predeterminada. El [aviso de seguridad de Microsoft 2871997](/security-updates/SecurityAdvisories/2016/2871997) agrega compatibilidad con Windows 7, windows Server 2008 R2 y windows Server 2012.

Estos son los requisitos para proporcionar protección de controlador de dominio a los miembros del grupo de usuarios protegidos:

- Los usuarios deben estar en dominios que tengan el nivel funcional de dominio de Windows Server 2012 R2 o posterior.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Adición de un grupo de seguridad global de usuario protegido a dominios de nivel inferior

Los controladores de dominio que ejecutan un sistema operativo anterior a Windows Server 2012 R2 pueden admitir la adición de miembros al nuevo grupo de seguridad de usuarios protegidos. Esto permite que los usuarios se beneficien de las protecciones de dispositivo antes de que se actualice el dominio.

> [!Note]
> Los controladores de dominio no serán compatibles con las protecciones de dominio.

El grupo de usuarios protegidos se puede crear [transfiriendo el rol de emulador de controlador de dominio principal (PDC)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816944(v=ws.10)) a un controlador de dominio que ejecute Windows Server 2012 R2. Una vez replicado ese grupo a otros controladores de dominio, el rol de emulador de PDC se puede hospedar en un controlador de dominio que ejecuta una versión anterior de Windows Server.

### <a name="protected-users-group-ad-properties"></a><a name="BKMK_ADgroup"></a>Propiedades de AD del grupo de usuarios protegidos

En la siguiente tabla se especifican las propiedades del grupo de usuarios protegidos.

|Atributo|Value|
|-------|-----|
|SID/RID conocido|S-1-5-21-<domain>-525|
|Tipo|Global de dominio|
|Contenedor predeterminado|CN=Usuarios, DC=<domain>, DC=|
|Miembros predeterminados|None|
|Miembro predeterminado de|None|
|¿Protegido por ADMINSDHOLDER?|No|
|¿Es seguro sacarlo del contenedor predeterminado?|Sí|
|¿Es seguro delegar la administración de este grupo en administradores que no son de servicio?|No|
|Derechos de usuario predeterminados|No hay derechos de usuario predeterminados|

## <a name="how-protected-users-group-works"></a><a name="BKMK_HowItWorks"></a>Cómo funciona el grupo de usuarios protegidos
En esta sección se describe el funcionamiento del grupo de usuarios protegidos en las siguientes circunstancias:

- Firmado en un dispositivo Windows

- El dominio de la cuenta de usuario está en un nivel funcional de dominio de Windows Server 2012 R2 o posterior

### <a name="device-protections-for-signed-in-protected-users"></a>Protecciones de dispositivo para usuarios protegidos con sesión iniciada
Cuando el usuario que ha iniciado sesión es miembro del grupo usuarios protegidos, se aplican las siguientes protecciones:

- La delegación de credenciales (CredSSP) no almacenará en caché las credenciales de texto sin formato del usuario, ni siquiera si está habilitada la opción **permitir la delegación de credenciales predeterminadas** Directiva de grupo.

- A partir de Windows 8.1 y Windows Server 2012 R2, el Resumen de Windows no almacenará en caché las credenciales de texto sin formato del usuario aunque el Resumen de Windows esté habilitado.

> [!Note]
> Después de instalar el [aviso de seguridad de Microsoft 2871997](/security-updates/SecurityAdvisories/2016/2871997) el Resumen de Windows seguirá almacenando credenciales en caché hasta que se configure la clave del registro. Vea el [aviso de seguridad de Microsoft: actualización para mejorar la protección y la administración de las credenciales: 13 de mayo de 2014](https://support.microsoft.com/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) para obtener instrucciones.

- NTLM no almacenará en caché las credenciales de texto sin formato del usuario ni la función unidireccional NT (NTOWF).

- Kerberos ya no creará claves DES o RC4. Además, no almacenará en caché las credenciales de texto sin formato del usuario ni las claves a largo plazo después de adquirir el TGT inicial.

- Un comprobador almacenado en caché no se crea al iniciar o cerrar sesión, por lo que ya no se admite el inicio de sesión sin conexión.

Después de agregar la cuenta de usuario al grupo usuarios protegidos, la protección se iniciará cuando el usuario inicie sesión en el dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Protecciones de controlador de dominio para usuarios protegidos
Las cuentas que son miembros del grupo de usuarios protegidos que se autentican en un dominio de Windows Server 2012 R2 no pueden:

- Autenticarse mediante la autenticación NTLM.

- Usar los tipos de cifrado DES o RC4 en la autenticación previa de Kerberos.

- Delegarse mediante una delegación con o sin restricciones.

- Renovar los TGT de Kerberos una vez transcurridas las cuatro horas de vigencia.

Las opciones no configurables de expiración de los TGT se establecen en cada cuenta individual del grupo de usuarios protegidos. Por lo general, el controlador de dominio establece la vigencia y renovación de los TGT según las directivas de dominio **Vigencia máxima del vale de usuario** y **Vigencia máxima de renovación de vales de usuario**. En el caso del grupo de usuarios protegidos, estas directivas de dominio están establecidas en 600 minutos.

Para obtener más información, consulta [Configurar cuentas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Solución de problemas
Existen dos registros administrativos funcionales que son de ayuda a la hora de resolver problemas de eventos relacionados con el grupo de usuarios protegidos. Estos nuevos registros se encuentran en Visor de eventos y están deshabilitados de forma predeterminada y se encuentran en **aplicaciones y servicios Logs\Microsoft\Windows\Authentication**.

|Identificador de evento y registro|Descripción|
|----------|--------|
|104<p>**ProtectedUser-Client**|Motivo: el paquete de seguridad del cliente no contiene las credenciales.<p>El error se registra en el equipo cliente cuando la cuenta es miembro del grupo de seguridad de usuarios protegidos. Este evento pone de manifiesto que el paquete de seguridad no almacena en caché las credenciales necesarias para autenticarse en el servidor.<p>Incluye el nombre del paquete, el nombre de usuario, el nombre del dominio y el nombre del servidor.|
|304<p>**ProtectedUser-Client**|Motivo: el paquete de seguridad no almacena las credenciales del usuario protegido.<p>Se registra un evento informativo en el cliente para indicar que el paquete de seguridad no almacena en caché las credenciales de inicio de sesión del usuario. Se espera que la autenticación implícita (WDigest), la delegación de credenciales (CredSSP) y NTLM no puedan tener credenciales para los usuarios protegidos. Las aplicaciones se pueden autenticar correctamente si solicitan credenciales.<p>Incluye el nombre del paquete, el nombre de usuario y el nombre del dominio.|
|100<p>**ProtectedUserFailures-DomainController**|Motivo: se produce un error de inicio de sesión NTLM para una cuenta que se encuentra en el grupo de seguridad usuarios protegidos.<p>Se registra un error en el controlador de dominio para señalar que la autenticación NTLM no se ha podido llevar a cabo porque la cuenta es miembro del grupo de seguridad de usuarios protegidos.<p>Incluye el nombre de cuenta y el nombre de dispositivo.|
|104<p>**ProtectedUserFailures-DomainController**|Motivo: los tipos de cifrado DES o RC4 se usan para la autenticación Kerberos y se produce un error de inicio de sesión para un usuario en el grupo de seguridad de usuarios protegidos.<p>La autenticación previa de Kerberos no puede realizarse porque los tipos de cifrado DES y RC4 no se pueden usar cuando la cuenta es miembro del grupo de seguridad de usuarios protegidos.<p>(Se acepta AES.)|
|303<p>**ProtectedUserSuccesses-DomainController**|Motivo: se emitió correctamente un vale de concesión de vales (TGT) de Kerberos para un miembro del grupo de usuarios protegidos.|


## <a name="additional-resources"></a>Recursos adicionales

- [Protección y administración de credenciales](credentials-protection-and-management.md)

- [Directivas de autenticación y silos de directivas de autenticación](authentication-policies-and-authentication-policy-silos.md)

- [Cómo configurar cuentas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)