---
title: Directivas de autenticación y silos de directivas de autenticación
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 55dd38c966fea8448231b06cde9c1dd63b024197
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266769"
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Directivas de autenticación y silos de directivas de autenticación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para los profesionales de TI se describen los silos de directivas de autenticación y las directivas que pueden restringir las cuentas a esos silos. También se explica cómo usar las directivas de autenticación para restringir el ámbito de las cuentas.

Los silos de directivas de autenticación y las directivas correspondientes proporcionan una manera de contener credenciales de privilegios elevados a sistemas que solo son pertinentes para los usuarios, equipos o servicios seleccionados. Los silos se pueden definir y administrar en los Servicios de dominio de Active Directory (AD DS) usando el Centro de administración de Active Directory y los cmdlets de Active Directory Windows PowerShell.

Los silos de directivas de autenticación son contenedores a los que los administradores pueden asignar cuentas de usuario, cuentas de equipo y cuentas de servicio. Las directivas de autenticación que se han aplicado a un contenedor pueden administrar conjuntos de cuentas. De esta manera se reduce la necesidad de que el administrador realice un seguimiento del acceso de las cuentas individuales a los recursos, y ayuda a evitar que usuarios malintencionados accedan a otros recursos mediante el robo de credenciales.

Las funciones introducidas en Windows Server 2012 R2, permiten crear silos de directivas, que hospedan un conjunto de usuarios con privilegios elevados de autenticación. Después, puedes asignar directivas de autenticación para este contenedor para limitar en qué lugar del dominio se pueden usar las cuentas con privilegios. Cuando las cuentas están en el grupo de seguridad Usuarios protegidos, se aplican otros controles adicionales, como el uso exclusivo del protocolo Kerberos.

Con estas funcionalidades, puedes limitar el uso de las cuentas valiosas a los hosts valiosos. Por ejemplo, podrías crear un nuevo silo Administradores del bosque que contiene los administradores de organización, de esquema y dominio. Después, podrías configurar el silo con una directiva de autenticación para que se produzca un error en la autenticación basada en contraseña y en tarjetas inteligentes de sistemas que no sean controladores de dominio o administradores de dominio.

Para obtener información acerca de la configuración de silos de directivas de autenticación y directivas de autenticación, vea [How to Configure Protected Accounts](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>Acerca de los silos de directivas de autenticación
Un silo de directivas de autenticación controla qué cuentas puede restringir el silo y define las directivas de autenticación que se aplicarán a los miembros. Puedes crear el silo según los requisitos de tu organización. Los silos son objetos de Active Directory para usuarios, equipos y servicios tal y como se definen en el esquema de la siguiente tabla.

**Esquema de Active Directory para silos de directivas de autenticación**

|Nombre para mostrar|Descripción|
|--------|--------|
|Authentication Policy Silo|Una instancia de esta clase define las directivas de autenticación y los comportamientos relacionados para los usuarios, equipos y servicios asignados.|
|Authentication Policy Silos|Un contenedor de esta clase puede contener objetos de silo de directivas de autenticación.|
|Authentication Policy Silo Enforced|Especifica si se aplica el silo de directivas de autenticación.<br /><br />Cuando no se aplica, la directiva está en modo de auditoría de forma predeterminada. Se generan eventos que indican posibles operaciones correctas o incorrectas, pero no se aplican protecciones al sistema.|
|Assigned Authentication Policy Silo Backlink|Este atributo es el vínculo de retroceso de msDS-AssignedAuthNPolicySilo.|
|Authentication Policy Silo Members|Especifica qué entidades de seguridad se asignan al AuthNPolicySilo.|
|Authentication Policy Silo Members Backlink|Este atributo es el vínculo de retroceso de msDS-AuthNPolicySiloMembers.|

Los silos de directivas de autenticación se pueden configurar mediante la Consola de administración de Active Directory o Windows PowerShell. Para obtener más información, consulta [Configurar cuentas protegidas](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>Acerca de las directivas de autenticación
Una directiva de autenticación define las propiedades de vigencia del vale de concesión de vales (TGT) del protocolo Kerberos y las condiciones de control de acceso de autenticación para un tipo de cuenta. La directiva se crea sobre el contenedor de AD DS conocido como silo de directivas de autenticación y lo controla.

Las directivas de autenticación controlan lo siguiente:

-   La vigencia del TGT para la cuenta, que está definido como no renovable.

-   Los criterios que las cuentas de dispositivo deben cumplir para iniciar sesión con una contraseña o certificado.

-   Los criterios que los usuarios y dispositivos deben cumplir para autenticarse en los servicios que se ejecutan como parte de la cuenta.

El tipo de cuenta de Active Directory determina el rol del llamador como uno de los siguientes:

-   **Usuario**

    Los usuarios siempre deben ser miembros del grupo de seguridad Usuarios protegidos que, de forma predeterminada, rechaza los intentos de autenticación con NTLM.

    Las directivas se pueden configurar para establecer la duración de TGT de una cuenta de usuario a un valor más corto o restringir los dispositivos en los que puede iniciar sesión una cuenta de usuario. Las expresiones enriquecidas se pueden configurar en la directiva de autenticación para controlar los criterios que deben cumplir los usuarios y sus dispositivos para autenticarse en el servicio.

    Para obtener más información, consulta [Grupo de seguridad de usuarios protegidos](protected-users-security-group.md).

-   **Service**

    Se usan cuentas de servicio administradas independientes, cuentas de servicio administradas de grupo o un objeto de cuenta personalizada que deriva de estos dos tipos de cuentas de servicio. Las directivas pueden establecer condiciones de control, que se utilizan para restringir las credenciales de cuenta de servicio administrada para dispositivos específicos con una identidad de Active Directory de acceso de un dispositivo. Los servicios nunca deben ser miembros del grupo de seguridad Usuarios protegidos porque se producirá un error de autenticación de entrada.

-   **Equipo**

    Se usa el objeto de cuenta de equipo o el objeto de cuenta personalizada que deriva del objeto de cuenta de equipo. Las directivas pueden establecer las condiciones del control de acceso necesarias para permitir la autenticación de la cuenta en función de las propiedades del usuario y dispositivo. Los equipos nunca deben ser miembros del grupo de seguridad Usuarios protegidos porque se producirá un error de autenticación de entrada. De forma predeterminada, se rechazan los intentos de usar autenticación NTLM. No se debe configurar una vigencia de TGT para cuentas de equipo.

> [!NOTE]
> Se puede establecer una directiva de autenticación en un conjunto de cuentas sin asociar la directiva a un silo de directivas de autenticación. Puedes usar esta estrategia cuando tienes una sola cuenta que proteger.

**Esquema de Active Directory para directivas de autenticación**

En el esquema de la siguiente tabla se definen las directivas para objetos de Active Directory para usuarios, equipos y servicios.

|Tipo|Nombre para mostrar|Descripción|
|----|--------|--------|
|Directiva|Directiva de autenticación|Una instancia de esta clase define los comportamientos de las directivas de autenticación para las entidades de seguridad asignadas.|
|Directiva|Authentication Policies|Un contenedor de esta clase puede contener objetos de directiva de autenticación.|
|Directiva|Authentication Policy Enforced|Especifica si se aplica la directiva de autenticación.<br /><br />Cuando no se aplica, la directiva está en modo de auditoría de forma predeterminada, y se generan eventos que indican posibles operaciones correctas o incorrectas, pero no se aplican protecciones al sistema.|
|Directiva|Assigned Authentication Policy Backlink|Este atributo es el vínculo de retroceso de msDS-AssignedAuthNPolicy.|
|Directiva|Assigned Authentication Policy|Especifica qué AuthNPolicy se debe aplicar a esta entidad de seguridad.|
|Usuario|User Authentication Policy|Especifica qué AuthNPolicy se debe aplicar a los usuarios que están asignados a este objeto de silo.|
|Usuario|User Authentication Policy Backlink|Este atributo es el vínculo de retroceso de msDS-UserAuthNPolicy.|
|Usuario|ms-DS-User-Allowed-To-Authenticate-To|Este atributo se usa para determinar el conjunto de entidades de seguridad que tienen permiso para autenticarse en un servicio que se ejecuta con la cuenta de usuario.|
|Usuario|ms-DS-User-Allowed-To-Authenticate-From|Este atributo se usa para determinar el conjunto de dispositivos en los que la cuenta de usuario tiene permiso para iniciar sesión.|
|Usuario|User TGT Lifetime|Especifica el tiempo máximo de un TGT de Kerberos emitido a un usuario (expresado en segundos). Los TGT resultantes no son renovables.|
|Computer|Computer Authentication Policy|Especifica qué AuthNPolicy se debe aplicar a los equipos que están asignados a este objeto de silo.|
|Computer|Computer Authentication Policy Backlink|Este atributo es el vínculo de retroceso de msDS-ComputerAuthNPolicy.|
|Computer|ms-DS-Computer-Allowed-To-Authenticate-To|Este atributo se usa para determinar el conjunto de entidades de seguridad que tienen permiso para autenticarse en un servicio que se ejecuta con la cuenta de equipo.|
|Computer|Computer TGT Lifetime|Especifica el tiempo máximo de un TGT de Kerberos emitido a un equipo (expresado en segundos). No se recomienda cambiar esta configuración.|
|Servicio|Service Authentication Policy|Especifica qué AuthNPolicy se debe aplicar a los servicios que están asignados a este objeto de silo.|
|Servicio|Service Authentication Policy Backlink|Este atributo es el vínculo de retroceso de msDS-ServiceAuthNPolicy.|
|Servicio|ms-DS-Service-Allowed-To-Authenticate-To|Este atributo se usa para determinar el conjunto de entidades de seguridad que tienen permiso para autenticarse en un servicio que se ejecuta con la cuenta de servicio.|
|Servicio|ms-DS-Service-Allowed-To-Authenticate-From|Este atributo se usa para determinar el conjunto de dispositivos en los que una cuenta de servicio tiene permiso para iniciar sesión.|
|Servicio|Service TGT Lifetime|Especifica el tiempo máximo de un TGT de Kerberos emitido a un servicio (expresado en segundos).|

Las directivas de autenticación se pueden configurar para cada silo mediante la Consola de administración de Active Directory o Windows PowerShell. Para obtener más información, consulta [Configurar cuentas protegidas](how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Cómo funciona
En esta sección se describe cómo funcionan los silos de directivas de autenticación y las directivas de autenticación junto con el grupo de seguridad Usuarios protegidos y la implementación del protocolo Kerberos en Windows.

-   [Cómo se usa el protocolo Kerberos con los silos de autenticación y directivas](#BKMK_HowKerbUsed)

-   [Cómo restringir un inicio de sesión de usuario funciona](#BKMK_HowRestrictingSignOn)

-   [Cómo funciona la restricción de emisión de vales de servicio](#BKMK_HowRestrictingServiceTicket)

**cuentas protegidas**

El grupo de seguridad usuarios protegidos activa una protección no configurable en dispositivos y equipos host que ejecutan Windows Server 2012 R2 y Windows 8.1 y en controladores de dominio en dominios con un controlador de dominio principal que ejecuta Windows Server 2012 R2. Según el nivel funcional de dominio de la cuenta, los miembros del grupo de seguridad Usuarios protegidos están aún más protegidos por los cambios en los métodos de autenticación que se admiten en Windows.

-   Los miembros del grupo de seguridad Usuarios protegidos no pueden autenticarse mediante NTLM, autenticación implícita o delegación de credenciales predeterminada CredSSP. En un dispositivo que ejecuta Windows 8.1 que usa uno de estos proveedores de compatibilidad para seguridad (SSP), se producirá un error de autenticación a un dominio cuando la cuenta es miembro del grupo de seguridad usuarios protegidos.

-   El protocolo Kerberos no usará los tipos de cifrado DES o RC4 más débiles en el proceso de autenticación previa. Esto significa que el dominio se debe configurar para que admita al menos el tipo de cifrado AES.

-   No se puede delegar la cuenta de usuario con Kerberos limitada o sin restricciones; la delegación. Esto significa que se producirán errores en las conexiones anteriores a otros sistemas si el usuario es miembro del grupo de seguridad Usuarios protegidos.

-   La vigencia predeterminada de los TGT de Kerberos de cuatro horas se pueden configurar mediante silos y directivas de autenticación, accesibles a través del Centro de administración de Active Directory. esto quiere decir que, una vez transcurridas esas cuatro horas, el usuario debe volver a autenticarse.

Para más información acerca de este grupo de seguridad, vea [How the Protected Users group works](protected-users-security-group.md#BKMK_HowItWorks).

**Silos y directivas de autenticación**

Los silos de directivas de autenticación y las directivas de autenticación aprovechan la infraestructura de autenticación de Windows existente. Se rechaza el uso del protocolo NTLM y se usa el protocolo Kerberos con nuevos tipos de cifrado. Las directivas de autenticación complementan el grupo de seguridad Usuarios protegidos porque proporcionan una manera de aplicar restricciones configurables a las cuentas, además de restricciones a las cuentas de servicios y equipos. Las directivas de autenticación se aplican durante el intercambio del servicio de autenticación (AS) o del servicio de concesión de vales (TGS) del protocolo Kerberos. Para obtener más información sobre cómo usa Windows el protocolo Kerberos y qué cambios se han realizado para admitir silos de directivas de autenticación y directivas de autenticación, consulta:

-   [Cómo funciona el protocolo de Kerberos versión 5 autenticación](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Cambios en la autenticación Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) (Windows Server 2008 R2 y Windows 7)

### <a name="BKMK_HowKerbUsed"></a>Cómo se usa el protocolo Kerberos con las directivas y los silos de directivas de autenticación
Cuando una cuenta de dominio se vincula a un silo de directivas de autenticación y el usuario inicia sesión, el Administrador de cuentas de seguridad agrega el tipo de notificación Silo de directivas de autenticación que incluye el silo como valor. Esta notificación en la cuenta proporciona acceso al silo de destino.

Cuando se aplica una directiva de autenticación y en el controlador de dominio se recibe una solicitud del servicio de autenticación para una cuenta de servicio, el controlador de dominio devuelve un TGT no renovable con la vigencia configurada (a menos que la vigencia del TGT del dominio sea menor).

> [!NOTE]
> La cuenta de dominio debe tener configurada una vigencia de TGT y debe estar vinculada directamente a la directiva o vinculada indirectamente a través de la pertenencia al silo.

Cuando una directiva de autenticación está en modo de auditoría y en el controlador de dominio se recibe una solicitud del servicio de autenticación para una cuenta de dominio, el controlador de dominio comprueba si la autenticación está permitida para el dispositivo para poder registrar una advertencia en caso de error. Una directiva de autenticación auditada no altera el proceso, por lo que no se producirán errores en las solicitudes de autenticación si no cumplen los requisitos de la directiva.

> [!NOTE]
> La cuenta de dominio debe estar vinculada directamente a la directiva o vinculada indirectamente a través de la pertenencia al silo.

Cuando se aplica una directiva de autenticación y el servicio de autenticación está protegido, y en el controlador de dominio se recibe una solicitud del servicio de autenticación para una cuenta de dominio, el controlador de dominio comprueba si la autenticación está permitida para el dispositivo. Si se produce un error, el controlador de dominio devuelve un mensaje de error y registra un evento.

> [!NOTE]
> La cuenta de dominio debe estar vinculada directamente a la directiva o vinculada indirectamente a través de la pertenencia al silo.

Cuando una directiva de autenticación está en modo de auditoría y se recibe una solicitud de concesión de vales de servicio por el controlador de dominio para una cuenta de dominio, el controlador de dominio comprueba si se permite la autenticación basada en certificado de atributo de privilegio de vale de la solicitud Datos (PAC) y se registra un mensaje de advertencia si se produce un error. El PAC contiene varios tipos de datos de autorización, incluidos los grupos a los que pertenece el usuario, los derechos que el usuario tiene y qué directivas se aplican al usuario. Esta información se usa para generar el token de acceso del usuario. Si es una directiva de autenticación aplicada que permite la autenticación a un usuario, dispositivo o servicio, el controlador de dominio comprueba si se permite la autenticación se basa en datos de PAC del vale de la solicitud. Si se produce un error, el controlador de dominio devuelve un mensaje de error y registra un evento.

> [!NOTE]
> La cuenta de dominio debe estar vinculada directamente o vinculada indirectamente a través de la pertenencia al silo a una directiva de autenticación auditada que permite la autenticación de un usuario, dispositivo o servicio.

Puedes usar una única directiva de autenticación para todos los miembros de un silo, o puedes usar directivas diferentes para usuarios, equipos y cuentas de servicio administradas.

Las directivas de autenticación se pueden configurar para cada silo mediante la Consola de administración de Active Directory o Windows PowerShell. Para obtener más información, consulta [Configurar cuentas protegidas](how-to-configure-protected-accounts.md).

### <a name="BKMK_HowRestrictingSignOn"></a>Cómo restringir un inicio de sesión de usuario funciona
Como estas directivas de autenticación se aplican a una cuenta, también se aplican a las cuentas que usan los servicios. Si quieres limitar el uso de una contraseña para un servicio a hosts específicos, esta configuración resulta útil. Por ejemplo, se configuran cuentas de servicio administradas de grupo para que los hosts tengan permiso para recuperar la contraseña de Active Directory Domain Services. Sin embargo, esa contraseña se puede usar desde cualquier host para la autenticación inicial. Se puede lograr un nivel adicional de protección aplicando una condición de control de acceso que limite la contraseña a tan solo el conjunto de hosts que pueden recuperar la contraseña.

Cuando los servicios que se ejecutan como sistema, servicio de red u otra identidad de servicio local se conectan a servicios de red, usan la cuenta de equipo del host. Los equipos cliente no se pueden restringir. Aunque el servicio esté usando una cuenta de equipo que no sea para un host de Windows, no se puede restringir.

Restringir el inicio de sesión de usuario a hosts específicos, requiere el controlador de dominio para validar la identidad del host. Cuando se usa la autenticación Kerberos con la protección Kerberos (que forma parte de Control de acceso dinámico), se proporciona el Centro de distribución de claves con el TGT del host desde el cual se está autenticando el usuario. El contenido de este TGT protegido se usa para completar una comprobación del acceso para determinar si el host está permitido.

Cuando un usuario inicia sesión en Windows o especifica sus credenciales de dominio en una petición de credenciales de una aplicación, de forma predeterminada, Windows envía una AS-REQ sin proteger al controlador de dominio. Si el usuario envía la solicitud desde un equipo que no admite protección, como los equipos que ejecutan Windows 7 o Windows Vista, se produce un error en la solicitud.

La lista siguiente describe el proceso:

-   El controlador de dominio en un dominio que ejecuta Windows Server 2012 R2 consulta para la cuenta de usuario y determina si está configurada con una directiva de autenticación que restringe la autenticación inicial que requiere solicitudes protegidas.

-   El controlador de dominio producirá un error en la solicitud.

-   Dado que se necesita protección, el usuario puede intentar iniciar sesión mediante el uso de un equipo que ejecuta Windows 8.1 o Windows 8, que está habilitado para admitir la protección de Kerberos para volver a intentar el proceso de inicio de sesión.

-   Windows detecta que el dominio admite la protección Kerberos y envía una AS-REQ protegida para reintentar la solicitud de inicio de sesión.

-   El controlador de dominio realiza una comprobación de acceso usando las condiciones de control de acceso configuradas y la información de identidad del sistema operativo de cliente en el TGT que se usó para proteger la solicitud.

-   Si la comprobación del acceso es incorrecta, el controlador de dominio rechaza la solicitud.

Aunque los sistemas operativos admitan la protección Kerberos, se pueden aplicar requisitos de control de acceso que deben cumplirse para que se conceda el acceso. Los usuarios inician sesión en Windows o especifican sus credenciales de dominio en una petición de credenciales de una aplicación. De forma predeterminada, Windows envía una AS-REQ sin proteger al controlador de dominio. Si el usuario envía la solicitud desde un equipo que admite la protección, como Windows 8.1 o Windows 8, las directivas de autenticación se evalúan como sigue:

1.  El controlador de dominio en un dominio que ejecuta Windows Server 2012 R2 consulta para la cuenta de usuario y determina si está configurada con una directiva de autenticación que restringe la autenticación inicial que requiere solicitudes protegidas.

2.  El controlador de dominio realiza una comprobación de acceso usando las condiciones de control de acceso configuradas y la información del sistema de identidad en el TGT que se usa para proteger la solicitud. La comprobación del acceso es correcta.

    > [!NOTE]
    > Si se han configurado restricciones de grupo de trabajo heredadas, se deben cumplir también.

3.  El controlador de dominio responde con una respuesta protegida (AS-REP) y la autenticación continúa.

### <a name="BKMK_HowRestrictingServiceTicket"></a>Cómo funciona la restricción de emisión de vales de servicio
Cuando no se permite una cuenta y un usuario que tiene un TGT intenta conectarse al servicio (por ejemplo, abriendo una aplicación que requiera autenticación a un servicio identificado por el nombre principal de servicio (SPN) del servicio, se produce la siguiente secuencia:

1.  En un intento por conectar con SPN1 desde SPN, Windows envía una TGS-REQ al controlador de dominio que está solicitando un vale de servicio a SPN1.

2.  El controlador de dominio en un dominio que ejecuta Windows Server 2012 R2 consulta spn1 para buscar la cuenta de Active Directory Domain Services para el servicio y determina que la cuenta está configurada con una directiva de autenticación que restringe la emisión de vales de servicio.

3.  El controlador de dominio realiza una comprobación de acceso usando las condiciones de control de acceso configuradas y la información de identidad del usuario en el TGT. La comprobación del acceso es incorrecta.

4.  El controlador de dominio rechaza la solicitud.

Cuando una cuenta está autorizada porque la cuenta cumple las condiciones de control de acceso establecidas por la directiva de autenticación y un usuario que tiene un TGT intenta conectarse al servicio (como abriendo una aplicación que requiere autenticación en un servicio que es identificado por el SPN del servicio), se produce la siguiente secuencia:

1.  En un intento por conectar con SPN1, Windows envía una TGS-REQ al controlador de dominio que está solicitando un vale de servicio a SPN1.

2.  El controlador de dominio en un dominio que ejecuta Windows Server 2012 R2 consulta spn1 para buscar la cuenta de Active Directory Domain Services para el servicio y determina que la cuenta está configurada con una directiva de autenticación que restringe la emisión de vales de servicio.

3.  El controlador de dominio realiza una comprobación de acceso usando las condiciones de control de acceso configuradas y la información de identidad del usuario en el TGT. La comprobación del acceso es correcta.

4.  El controlador de dominio responde a la solicitud con una respuesta de servicio de concesión de vales (TGS-REP).

## <a name="BKMK_ErrorandEvents"></a>Error asociado y los mensajes de evento informativo
En la tabla siguiente se describen los eventos asociados con el grupo de seguridad Usuarios protegidos y las directivas de autenticación que se aplican a los silos de directivas de autenticación.

Los eventos se registran en los registros de aplicaciones y servicios en **Microsoft\Windows\Authentication**.

Para solucionar problemas en los pasos que usan estos eventos, vea [Troubleshoot Authentication Policies](how-to-configure-protected-accounts.md#troubleshoot-authentication-policies) y [Troubleshoot events related to Protected Users](how-to-configure-protected-accounts.md#troubleshoot-events-related-to-protected-users).

|Identificador de evento y registro|Descripción|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Se produce un error de inicio de sesión de NTLM porque la directiva de autenticación está configurada.<br /><br />Se registra un evento en el controlador de dominio para indicar que se produjo un error de autenticación NTLM porque se necesitan restricciones de control de acceso, y esas restricciones no se pueden aplicar a NTLM.<br /><br />Muestra los nombres de cuenta, dispositivo, directiva y silo.|
|105<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Se produce un error de restricción Kerberos porque no se permite la autenticación desde un dispositivo determinado.<br /><br />Se registra un evento en el controlador de dominio para indicar que se denegó un TGT de Kerberos porque el dispositivo no cumple las restricciones de control de acceso aplicadas.<br /><br />Muestra los nombres de cuenta, dispositivo, directiva y silo, y la vigencia del TGT.|
|305<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Se podría producir un posible error de restricción Kerberos porque no se permite la autenticación desde un dispositivo determinado.<br /><br />En el modo de auditoría, se registra un evento informativo en el controlador de dominio para determinar si se denegará un TGT de Kerberos porque el dispositivo no cumple las restricciones de control de acceso aplicadas.<br /><br />Muestra los nombres de cuenta, dispositivo, directiva y silo, y la vigencia del TGT.|
|106<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Se produce un error de restricción Kerberos porque el usuario o dispositivo no están autorizados a autenticarse en el servidor.<br /><br />Se registra un evento en el controlador de dominio para indicar que se denegó un vale de servicio de Kerberos porque el usuario, el dispositivo o ambos no cumplen las restricciones de control de acceso aplicadas.<br /><br />Muestra los nombres de dispositivo, directiva y silo.|
|306<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Se podría producir un error de restricción Kerberos porque el usuario o dispositivo no están autorizados a autenticarse en el servidor.<br /><br />En el modo de auditoría, se registra un evento informativo en el controlador de dominio para indicar que se denegará un vale de servicio de Kerberos porque el usuario, el dispositivo o ambos no cumplen las restricciones de control de acceso.<br /><br />Muestra los nombres de dispositivo, directiva y silo.|

## <a name="see-also"></a>Vea también
[Cómo configurar cuentas protegidas](how-to-configure-protected-accounts.md)

[Protección y administración de credenciales](credentials-protection-and-management.md)

[Grupo de seguridad Usuarios protegidos](protected-users-security-group.md)


