---
title: "Directivas de autenticación y Silos de directiva de autenticación"
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
ms.openlocfilehash: 460837c79c0e0d2c48331ddaaffcd118fd16ebc1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Directivas de autenticación y Silos de directiva de autenticación

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI describe silos de directiva de autenticación y las directivas que se pueden restringir cuentas a esos silos. También se explica cómo las directivas de autenticación pueden usarse para restringir el ámbito de cuentas.

Silos de directiva de autenticación y las directivas que lo acompañan proporcionan una manera para que contenga las credenciales de privilegios altos a sistemas que solo son pertinentes para los usuarios, equipos o servicios. Silos se puede definir y administrar en los servicios de dominio de Active Directory (AD DS) mediante el centro de administración de Active Directory y los cmdlets de Windows PowerShell de Active Directory.

Silos de directiva de autenticación son contenedores a la que los administradores pueden asignar cuentas de usuario, las cuentas de equipo y cuentas de servicio. Conjuntos de cuentas pueden administrarse mediante las directivas de autenticación que se han aplicado a ese contenedor. Esto reduce la necesidad de que el administrador realizar un seguimiento del acceso a los recursos para las cuentas individuales y contribuye a evitar que los usuarios malintencionados obtener acceso a otros recursos a través de robo de credenciales.

Funciones introducidas en Windows Server 2012 R2, te permiten crear autenticación silos de directiva, que hospeda un conjunto de usuarios de privilegios de alto. A continuación, puedes asignar directivas de autenticación para este contenedor al límite donde se puedan usar cuentas con privilegios en el dominio. Cuando cuentas están en el grupo de seguridad de los usuarios protegido, se aplican controles adicionales, como el uso exclusivo del protocolo Kerberos.

Con estas funcionalidades, puede limitar el uso de la cuenta de gran valor a los hosts de gran valor. Por ejemplo, podrías crear un silo de los administradores de bosque nuevo que contenga enterprise, esquema y los administradores de dominio. A continuación, se puede configurar del sitio con una directiva de autenticación para que la autenticación basada en la tarjeta inteligente de los sistemas que no sean los controladores de dominio y consolas de administrador de dominio y la contraseña se produciría un error.

Para obtener información sobre la configuración de directivas de autenticación y silos de directiva de autenticación, vea [cómo configurar cuentas protegido](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>Acerca de silos de directiva de autenticación
Un silo de directiva de autenticación controla qué cuentas se pueden restringir mediante del sitio y define las directivas de autenticación para aplicar a los miembros. Puedes crear del sitio en función de los requisitos de la organización. Los depósitos son objetos de Active Directory para los usuarios, equipos y servicios, como se define en el esquema en la siguiente tabla.

**Esquema de Active Directory para silos de directiva de autenticación**

|Nombre para mostrar|Descripción|
|--------|--------|
|Silo de directiva de autenticación|Una instancia de esta clase define las directivas de autenticación y comportamientos relacionados para asignado a los usuarios, equipos y servicios.|
|Silos de directiva de autenticación|Un contenedor de esta clase puede contener objetos silo de directiva de autenticación.|
|Silo de directiva de autenticación es obligatorio|Especifica si se aplica el silo de directiva de autenticación.<br /><br />Cuando no es obligatorio, es la directiva predeterminada en modo auditoría. Se generan eventos que indican posibles éxitos y errores, pero no se aplican protecciones para el sistema.|
|Autenticación asignado directiva Silo vínculos hacia atrás|Este atributo es el vínculo anterior para msDS AssignedAuthNPolicySilo.|
|Silo miembros de la directiva de autenticación|Especifica qué entidades están asignados a la AuthNPolicySilo.|
|Autenticación directiva Silo miembros vínculos hacia atrás|Este atributo es el vínculo anterior para msDS AuthNPolicySiloMembers.|

Silos de directiva de autenticación pueden configurarse mediante la consola de administración de Active Directory o Windows PowerShell. Para obtener más información, consulta [cómo configurar cuentas protegido](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>Acerca de las directivas de autenticación
Una directiva de autenticación define las propiedades de ciclo de vida de vales (TGT) de concesión de vales de protocolo de Kerberos y condiciones de control de acceso de autenticación para un tipo de cuenta. Se basa en la directiva y controla el contenedor de AD DS conocido como del sitio de la directiva de autenticación.

Directivas de autenticación controlan las siguientes acciones:

-   La vigencia TGT de la cuenta, que se establece en no renovarse.

-   Los criterios que deben cumplir para iniciar sesión con una contraseña o un certificado cuentas del dispositivo.

-   Los criterios que deben cumplir para autenticar a los servicios que se ejecutan como parte de la cuenta de usuarios y dispositivos.

El tipo de cuenta de Active Directory determina el rol del llamador como una de las siguientes acciones:

-   **Usuario**

    Los usuarios siempre deben ser miembros del grupo de seguridad de los usuarios protegido, que rechaza los intentos de autenticación NTLM de usar de manera predeterminada.

    Las directivas pueden configurarse para establecer la duración TGT de una cuenta de usuario en un valor más corto o restringir los dispositivos a la que puedes iniciar sesión en una cuenta de usuario. Expresiones enriquecido pueden configurarse en la directiva de autenticación para controlar los criterios que los usuarios y sus dispositivos deben cumplir para autenticar el servicio.

    Para obtener más información, consulta [protegido el grupo de seguridad de los usuarios](protected-users-security-group.md).

-   **Servicio**

    Independiente administra las cuentas de servicio, cuentas de servicio de grupo que administrados o un objeto de cuenta personalizada que se deriva de estos dos tipos de cuentas se utilizan el servicio. Las directivas pueden establecer las condiciones de control, que se usan para restringir las credenciales de cuenta de servicio administrado a determinados dispositivos con una identidad de Active Directory de acceso de un dispositivo. Servicios jamás deben ser miembros del grupo de seguridad de los usuarios protegido porque toda la autenticación entrantes se producirá un error.

-   **Equipo**

    Se usa el objeto de cuenta de equipo o el objeto de cuenta personalizada que se deriva del objeto de cuenta de equipo. Las directivas pueden establecer el acceso a las condiciones de control que tienen que permiten la autenticación a la cuenta en función de las propiedades de dispositivo y usuario. Equipos jamás deben ser miembros del grupo de seguridad de los usuarios protegido porque toda la autenticación entrantes se producirá un error. De manera predeterminada, se rechazan intentos para usar la autenticación NTLM. Una duración TGT no debe configurarse para cuentas de equipo.

> [!NOTE]
> Es posible establecer una directiva de autenticación en un conjunto de cuentas sin asociar la directiva a un silo de directiva de autenticación. Puedes usar esta estrategia cuando haya una sola cuenta para proteger.

**Esquema de Active Directory para directivas de autenticación**

Las directivas para los objetos de Active Directory para los usuarios, equipos y servicios se definen mediante el esquema en la siguiente tabla.

|Tipo|Nombre para mostrar|Descripción|
|----|--------|--------|
|Directiva|Directiva de autenticación|Una instancia de esta clase define comportamientos de la directiva de autenticación para los principales asignados.|
|Directiva|Directivas de autenticación|Un contenedor de esta clase puede contener objetos de directiva de autenticación.|
|Directiva|Aplicar la directiva de autenticación|Especifica si se aplica la directiva de autenticación.<br /><br />Cuando no es obligatorio, la directiva predeterminada está en modo auditoría y se generan eventos que indican posibles éxitos y errores, pero no se aplican protecciones para el sistema.|
|Directiva|Directiva de autenticación asignado vínculos hacia atrás|Este atributo es el vínculo anterior para msDS AssignedAuthNPolicy.|
|Directiva|Directiva de autenticación asignado|Especifica que debe ser AuthNPolicy aplicado a esta entidad de seguridad.|
|Usuario|Directiva de autenticación de usuario|Especifica que debe ser AuthNPolicy aplicar a los usuarios asignados a este objeto silo.|
|Usuario|Vínculo primario de directiva de autenticación de usuario|Este atributo es el vínculo anterior para msDS UserAuthNPolicy.|
|Usuario|ms-DS-User-Allowed-To-Authenticate-To|Este atributo se usa para determinar el conjunto de entidades que se pueden autenticar en un servicio que se ejecuta en la cuenta de usuario.|
|Usuario|ms-DS-User-Allowed-To-Authenticate-From|Este atributo se usa para determinar el conjunto de dispositivos a los que una cuenta de usuario tiene permiso para iniciar sesión.|
|Usuario|Ciclo de vida de vale de usuario|Especifica la edad máxima de un TGT Kerberos emitido a un usuario (expresado en segundos). Son no renovables TGT resultantes.|
|Equipo|Directiva de autenticación de equipo|Especifica que debe ser AuthNPolicy aplicarse en equipos que se asignan a este objeto silo.|
|Equipo|Vínculo primario de directiva de autenticación de equipo|Este atributo es el vínculo anterior para msDS ComputerAuthNPolicy.|
|Equipo|ms-DS-Computer-Allowed-To-Authenticate-To|Este atributo se usa para determinar el conjunto de entidades que se pueden autenticar en un servicio que se ejecuta en la cuenta de equipo.|
|Equipo|Ciclo de vida TGT de equipo|Especifica la edad máxima de un TGT Kerberos emitido a un equipo (expresado en segundos). No se recomienda cambiar esta configuración.|
|Servicio|Directiva de autenticación del servicio|Especifica que debe ser AuthNPolicy aplicar a los servicios que se asignan a este objeto silo.|
|Servicio|Vínculo primario de directiva de autenticación de servicio|Este atributo es el vínculo anterior para msDS ServiceAuthNPolicy.|
|Servicio|ms-DS-Service-Allowed-To-Authenticate-To|Este atributo se usa para determinar el conjunto de entidades que se pueden autenticar en un servicio que se ejecuta en la cuenta de servicio.|
|Servicio|ms-DS-Service-Allowed-To-Authenticate-From|Este atributo se usa para determinar el conjunto de dispositivos a los que una cuenta de servicio tiene permiso para iniciar sesión.|
|Servicio|Duración del vale de servicio|Especifica la edad máxima de un TGT Kerberos emitido a un servicio (expresado en segundos).|

Directivas de autenticación pueden configurarse para cada silo mediante la consola de administración de Active Directory o Windows PowerShell. Para obtener más información, consulta [cómo configurar cuentas protegido](how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Cómo funciona
Esta sección describe cómo funcionan los silos de directiva de autenticación y directivas de autenticación junto con el grupo de seguridad de los usuarios protegido y la implementación del protocolo Kerberos en Windows.

-   [¿Cómo se usa el protocolo Kerberos con las directivas y silos de autenticación](#BKMK_HowKerbUsed)

-   [Cómo restringir un inicio de sesión de usuario funciona](#BKMK_HowRestrictingSignOn)

-   [Cómo funciona la opción restringir emisión de vale de servicio](#BKMK_HowRestrictingServiceTicket)

**Cuentas protegidas**

El grupo de seguridad de los usuarios protegido desencadena protección no se pueden configurar en los dispositivos y equipos host que ejecutan Windows Server 2012 R2 y Windows 8.1 y en los controladores de dominio de dominios con un controlador de dominio principal que se ejecute Windows Server 2012 R2. Según el nivel funcional de dominio de la cuenta, los miembros del grupo de seguridad de los usuarios protegido están protegidos aún más debido a cambios en los métodos de autenticación que se admiten en Windows.

-   No puede autenticar el miembro del grupo de seguridad de los usuarios protegido mediante el uso de NTLM, la autenticación implícita o delegación de credenciales CredSSP predeterminado. En un dispositivo que ejecuta Windows 8.1 que usa uno de estos proveedores de compatibilidad para seguridad (SSP), se producirá un error de autenticación a un dominio cuando la cuenta es miembro del grupo de seguridad de los usuarios protegido.

-   El protocolo Kerberos no usará los tipos de cifrado DES o RC4 más débiles en el proceso de autenticación previa. Esto significa que el dominio debe estar configurado para admitir al menos el tipo de cifrado AES.

-   No se puede delegar la cuenta del usuario con Kerberos limitada o no tienen ninguna restricción delegación. Esto significa que anterior conexiones a otros sistemas pueden fallar si el usuario es un miembro del grupo de seguridad de los usuarios protegido.

-   El valor de ciclo de vida de los vales Kerberos predeterminado de cuatro horas es configurable mediante directivas de autenticación y silos, que se puede acceder mediante el centro de administración de Active Directory. Esto significa que cuando se pasan los cuatro horas, el usuario debe volver a autenticar.

Para obtener más información acerca de este grupo de seguridad, consulta [cómo los usuarios protegidos grupo funciona](protected-users-security-group.md#BKMK_HowItWorks).

**Silos y directivas de autenticación**

Silos de directiva de autenticación y directivas de autenticación aprovechan la infraestructura de autenticación de Windows existente. Se rechaza el uso del protocolo NTLM, y se usa el protocolo Kerberos con nuevos tipos de cifrado. Directivas de autenticación complementan el grupo de seguridad de los usuarios protegido proporcionando una manera para aplicar restricciones configurables a las cuentas, además de proporcionar restricciones para cuentas para los servicios y equipos. Directivas de autenticación se aplican durante el servicio de autenticación del protocolo de Kerberos (AS) o exchange concesión de vales de servicio (TGS). Para obtener más información acerca de cómo Windows usa el protocolo Kerberos y qué cambios realizados para admitir silos de directiva de autenticación y directivas de autenticación, consulta:

-   [Cómo funciona el protocolo de autenticación 5 de Kerberos versión](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Cambios en la autenticación Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) (Windows Server 2008 R2 y Windows 7)

### <a name="BKMK_HowKerbUsed"></a>¿Cómo se usa el protocolo Kerberos con las directivas y silos de directiva de autenticación
Cuando una cuenta de dominio está vinculada a un silo de directiva de autenticación y el usuario inicie sesión, el Administrador de cuentas de seguridad agrega el tipo de notificación de Silo de directiva de autenticación que incluya del sitio, como el valor. Esta notificación en la cuenta proporciona el acceso a del sitio de destino.

Cuando se aplica una directiva de autenticación y se recibe la solicitud de servicio de autenticación para una cuenta de dominio en el controlador de dominio, el controlador de dominio devuelve un TGT no renovables con la duración configurada (a menos que la duración TGT de dominio es menor).

> [!NOTE]
> La cuenta de dominio debe tener una duración TGT configurada y debe directamente vinculados a la directiva o indirectamente vinculados a través de la pertenencia silo.

Cuando una directiva de autenticación que se está en modo auditoría y se recibe la solicitud de servicio de autenticación para una cuenta de dominio en el controlador de dominio, el controlador de dominio comprueba si se permite la autenticación para el dispositivo para que pueda iniciar una advertencia si se produce un error. Una directiva de autenticación auditados no modifica el proceso, para que las solicitudes de autenticación no se producirá un error si no cumplen los requisitos de la directiva.

> [!NOTE]
> La cuenta de dominio debe vincular a ella directamente a la directiva o indirectamente vinculada a través de la pertenencia silo.

Cuando se aplica una directiva de autenticación y el servicio de autenticación es blindado, se recibe la solicitud de servicio de autenticación para una cuenta de dominio en el controlador de dominio, el controlador de dominio comprueba si se permite la autenticación para el dispositivo. Si se produce un error, el controlador de dominio devuelve un mensaje de error y registra un evento.

> [!NOTE]
> La cuenta de dominio debe vincular a ella directamente a la directiva o indirectamente vinculada a través de la pertenencia silo.

Cuando una directiva de autenticación que se está en modo auditoría y se recibe una solicitud de servicio de concesión de vales por el controlador de dominio para una cuenta de dominio, el controlador de dominio comprueba si se permite la autenticación basada en la lista de la solicitud datos de certificado de atributos de privilegios (PAC), y registra un mensaje de advertencia si se produce un error. La PAC contiene diversos tipos de datos de autorización, incluidos los grupos que el usuario es un miembro de derechos que el usuario tiene, y qué directivas se aplican al usuario. Esta información se utiliza para generar el token de acceso del usuario. Si se trata de una directiva de autenticación forzada que permite la autenticación a un usuario, dispositivo o servicio, las comprobaciones de controlador de dominio si se permite la autenticación se basan en los datos de PAC vale de la solicitud. Si se produce un error, el controlador de dominio devuelve un mensaje de error y registra un evento.

> [!NOTE]
> La cuenta de dominio debe estar directamente vinculada o vinculada a través de la pertenencia de silo a una directiva de autenticación auditados que permite la autenticación a un usuario, dispositivo o servicio,

Puedes usar una directiva de autenticación solo para todos los miembros de un silo o puedes usar directivas independientes para los usuarios, equipos y cuentas de servicio administradas.

Directivas de autenticación pueden configurarse para cada silo mediante la consola de administración de Active Directory o Windows PowerShell. Para obtener más información, consulta [cómo configurar cuentas protegido](how-to-configure-protected-accounts.md).

### <a name="BKMK_HowRestrictingSignOn"></a>Cómo restringir un inicio de sesión de usuario funciona
Dado que estas directivas de autenticación se aplican a una cuenta, también se aplica a las cuentas que utilizan servicios. Si quieres limitar el uso de una contraseña para un servicio al host específico, esta configuración es útil. Por ejemplo, grupo administra cuentas estén configuradas donde se pueden recuperar la contraseña de los servicios de dominio de Active Directory los hosts del servicio. Sin embargo, esa contraseña puede usarse desde cualquier host para la autenticación inicial. Al aplicar una condición de control de acceso, un nivel adicional de protección puede conseguirse limitando la contraseña al conjunto de hosts que se puede recuperar la contraseña.

Cuando los servicios que se ejecutan como sistema, el servicio de red u otra identidad de servicio local se conectan a servicios de red, usan cuenta de equipo del host. Cuentas de equipo no pueden ser restringidas. Por lo tanto, incluso si el servicio está usando una cuenta de equipo que no es para un host de Windows, no puede ser restringida.

Restringir el inicio de sesión del usuario a determinados hosts requiere el controlador de dominio para validar la identidad del host. Al usar la autenticación de Kerberos con protección de Kerberos (que es parte del Control de acceso dinámico), el centro de distribución de claves se proporciona con el TGT del host desde el que el usuario se autentica. El contenido de este TGT blindado se usa para completar una comprobación de acceso para determinar si se permite el host.

Cuando un usuario inicia sesión en Windows o escribe sus credenciales de dominio en un símbolo del sistema de credenciales para una aplicación, de manera predeterminada, Windows envía una solicitud de AS unarmored al controlador de dominio. Si el usuario envía la solicitud de un equipo que no admite armadura, como equipos que ejecutan Windows 7 o Windows Vista, la solicitud se produce un error.

La siguiente lista describe el proceso:

-   El controlador de dominio en un dominio que ejecutan Windows Server 2012 R2 consultas para la cuenta de usuario y determina si se configura con una directiva de autenticación que restringe la autenticación inicial que requiere blindadas solicitudes.

-   El controlador de dominio se producirá un error de la solicitud.

-   Porque armadura es necesario, el usuario puede intentar iniciar sesión mediante el uso de un equipo que ejecute Windows 8.1 o Windows 8, que está habilitada para admitir la protección de Kerberos para volver a intentar el proceso de inicio de sesión.

-   Windows detecta que el dominio admite la protección de Kerberos y envía un requisito como blindado para volver a intentar la solicitud de inicio de sesión.

-   El controlador de dominio, realiza una comprobación de acceso mediante el uso de las condiciones de control de acceso configurada y la información de identidad del sistema operativo de cliente en la etiqueta TGT que se usó para armor la solicitud.

-   Si se produce un error en la comprobación de acceso, el controlador de dominio rechaza la solicitud.

Aunque los sistemas operativos compatibles con la protección de Kerberos, requisitos de control de acceso pueden aplicarse y deben cumplirse antes de concede el acceso. Los usuarios iniciar sesión en Windows o escribir sus credenciales de dominio en un símbolo del sistema de credenciales para una aplicación. De manera predeterminada, Windows envía una solicitud de AS unarmored al controlador de dominio. Si el usuario envía la solicitud de un equipo que admite armadura, como Windows 8.1 o Windows 8, las directivas de autenticación se evalúan como sigue:

1.  El controlador de dominio en un dominio que ejecutan Windows Server 2012 R2 consultas para la cuenta de usuario y determina si se configura con una directiva de autenticación que restringe la autenticación inicial que requiere blindadas solicitudes.

2.  El controlador de dominio, realiza una comprobación de acceso mediante el uso de las condiciones de control de acceso configurada y la información del sistema de identidad en la etiqueta TGT que se usa para armor la solicitud. La comprobación de acceso se realiza correctamente.

    > [!NOTE]
    > Si se configuran las restricciones de un grupo de trabajo heredado, aquellos también deben cumplirse.

3.  El controlador de dominio responde con una respuesta blindada (como REP) y continúa la autenticación.

### <a name="BKMK_HowRestrictingServiceTicket"></a>Cómo funciona la opción restringir emisión de vale de servicio
Cuando no se permite una cuenta y un usuario que tenga un TGT intenta conectarse al servicio (como al abrir una aplicación que requiere la autenticación a un servicio que se identifica por su nombre de entidad de seguridad de servicio (SPN) del servicio, se produce la siguiente secuencia:

1.  En un intento de conexión a SPN1 de SPN, Windows envía una solicitud de TGS al controlador de dominio que está solicitando un vale de servicio a SPN1.

2.  El controlador de dominio en un dominio que ejecutan Windows Server 2012 R2 busca SPN1 para buscar la cuenta de servicios de dominio de Active Directory para el servicio y determina que la cuenta está configurada con una directiva de autenticación que restringe la emisión de vales de servicio.

3.  El controlador de dominio realiza una comprobación de acceso mediante el uso de información de identidad del usuario y de las condiciones de control de acceso configurada en el TGT. Se produce un error en la comprobación de acceso.

4.  El controlador de dominio rechaza la solicitud.

Cuando se permite una cuenta porque la cuenta cumpla las condiciones de control de acceso que se establecen mediante la directiva de autenticación y un usuario que tenga un TGT intenta conectarse al servicio (como al abrir una aplicación que requiere la autenticación a un servicio que se identifica mediante SPN del servicio), se produce la siguiente secuencia:

1.  En un intento de conexión a SPN1, Windows envía una solicitud de TGS al controlador de dominio que está solicitando un vale de servicio a SPN1.

2.  El controlador de dominio en un dominio que ejecutan Windows Server 2012 R2 busca SPN1 para buscar la cuenta de servicios de dominio de Active Directory para el servicio y determina que la cuenta está configurada con una directiva de autenticación que restringe la emisión de vales de servicio.

3.  El controlador de dominio realiza una comprobación de acceso mediante el uso de información de identidad del usuario y de las condiciones de control de acceso configurada en el TGT. La comprobación de acceso se realiza correctamente.

4.  El controlador de dominio responde a la solicitud con una respuesta del servicio de concesión de vales (TGS REP).

## <a name="BKMK_ErrorandEvents"></a>Error relacionados y los mensajes de evento informativos
La siguiente tabla describe los eventos que están asociados con el grupo de seguridad de los usuarios protegidos y las directivas de autenticación que se aplican a silos de directiva de autenticación.

Los eventos se registran en las aplicaciones y los registros de servicios en **Microsoft\Windows\Authentication**.

Para solucionar problemas de los pasos que se usan estos eventos, consulta [solucionar problemas de directivas de autenticación](how-to-configure-protected-accounts.md#BKMK_TroubleshootAuthnPolicies) y [solucionar problemas de eventos relacionados con los usuarios protegido](how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents).

|Registro y el identificador de evento|Descripción|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController**|Causa: Se produce un error de inicio de sesión de NTLM porque se configura la directiva de autenticación.<br /><br />Un evento se registra en el controlador de dominio para indicar que la autenticación NTLM no se pudo debido a restricciones de control de acceso son necesarios y dichas restricciones no se puede aplicar a NTLM.<br /><br />Muestra la cuenta, el dispositivo, la directiva y la silo nombres.|
|105<br /><br />**AuthenticationPolicyFailures-DomainController**|Causa: Se produce un error de restricción de Kerberos porque no se permite la autenticación de un dispositivo determinado.<br /><br />Un evento se registra en el controlador de dominio para indicar que se ha denegado un TGT Kerberos porque el dispositivo no cumplía con las restricciones de control de acceso aplicada.<br /><br />Muestra la cuenta, el dispositivo, la directiva, la silo nombres y la duración TGT.|
|305<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Un posible error de restricción de Kerberos podría producirse porque no se permite la autenticación de un dispositivo determinado.<br /><br />En modo auditoría, se registra un evento de información en el controlador de dominio para determinar si se deniegan un TGT Kerberos porque el dispositivo no cumplía con las restricciones del control de acceso.<br /><br />Muestra la cuenta, el dispositivo, la directiva, la silo nombres y la duración TGT.|
|106<br /><br />**AuthenticationPolicyFailures-DomainController**|Causa: Se produce un error de restricción de Kerberos porque no se permite que el usuario o el dispositivo para autenticar en el servidor.<br /><br />Un evento se registra en el controlador de dominio para indicar que un vale de servicio Kerberos se denegó el usuario, dispositivo o ambos no cumplan con las restricciones de control de acceso aplicada.<br /><br />Muestra el dispositivo, la directiva y silo nombres.|
|306<br /><br />**AuthenticationPolicyFailures-DomainController**|Razón: Es posible que se producen a un error de restricción de Kerberos porque no se permite que el usuario o el dispositivo para autenticar en el servidor.<br /><br />En modo auditoría, se registra un evento de información en el controlador de dominio para indicar que un vale de servicio Kerberos se deniegan porque el usuario, dispositivo o ambos no cumplan con las restricciones del control de acceso.<br /><br />Muestra el dispositivo, la directiva y silo nombres.|

## <a name="see-also"></a>Consulta también
[Cómo configurar cuentas protegidas](how-to-configure-protected-accounts.md)

[Administración y protección de credenciales](credentials-protection-and-management.md)

[Grupo de seguridad de los usuarios protegido](protected-users-security-group.md)


