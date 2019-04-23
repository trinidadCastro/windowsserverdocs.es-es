---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar la protección de bloqueo de Extranet de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 37b8c4b9b07e3111fce1bfc0a9aae10c8754bb3a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884636"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurar la protección de bloqueo de Extranet de AD FS

>Se aplica a: Windows Server 2012 R2

En AD FS en Windows Server 2012 R2, introdujimos una característica de seguridad denominada bloqueo de Extranet.  Con esta característica, AD FS se "detendrá" autenticar la cuenta de usuario "malintencionado" desde fuera de un período de tiempo.  Esto evita que las cuentas de usuario que se bloquee en Active Directory.  Además de proteger a los usuarios de un bloqueo de cuenta de AD, bloqueo de extranet de AD FS también protege contra los ataques de averiguación de contraseña por fuerza bruta

> [!NOTE]
> Esta característica solo funciona para la **escenario de extranet** donde la autenticación de las solicitudes lleguen a través de Web Application Proxy y solo se aplica a **autenticación de nombre de usuario y contraseña**.

## <a name="advantages-of-extranet-lockout"></a>Ventajas de bloqueo de Extranet
Bloqueo de extranet proporciona las siguientes ventajas principales:
- Protege las cuentas de usuario de **ataques por fuerza bruta** donde un atacante intenta adivinar una contraseña de usuario mediante el envío continuamente las solicitudes de autenticación. En este caso, AD FS se bloqueará la cuenta de usuario malintencionado obtener acceso a la extranet
- Protege las cuentas de usuario de **el bloqueo de cuenta malintencionado** donde quiere que un atacante bloquear una cuenta de usuario mediante el envío de solicitudes de autenticación con contraseñas incorrectas. En este caso, aunque la cuenta de usuario se bloqueará por AD FS para acceso de extranet, la cuenta de usuario real de AD no está bloqueada y el usuario todavía puede acceder a recursos corporativos dentro de la organización. Esto se conoce como un **bloqueo temporal**.

## <a name="how-it-works"></a>Cómo funciona
Hay 3 configuración de AD FS que se debe configurar para habilitar esta característica: 
- **EnableExtranetLockout &lt;booleano&gt;**  establezca este valor booleano a ser True si desea habilitar el bloqueo de Extranet.
- **ExtranetLockoutThreshold &lt;entero&gt;**  define el número máximo de intentos con contraseñas incorrectas. Una vez que se alcanza el umbral, AD FS rechazará las solicitudes de extranet inmediatamente sin intentar ponerse en contacto con el controlador de dominio para la autenticación, sin importar si la contraseña es correcto o incorrecto, hasta que se pasa a la ventana de observación de extranet. Esto significa que el valor de **badPwdCount** atributo de una cuenta de AD no aumentará mientras la cuenta está bloqueada temporalmente out.
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  determina cuánto del usuario de la cuenta estará soft-bloqueada. AD FS se iniciará realizar la autenticación de nombre de usuario y contraseña de nuevo cuando se pasa a la ventana. AD FS usa el badPasswordTime del atributo de AD como referencia para determinar si la ventana de observación de extranet se ha superado o no. La ventana ha pasado si actual tiempo > badPasswordTime + ExtranetObservationWindow. 

> [!NOTE]
> Funciones de bloqueo de extranet de AD FS independientemente de las directivas de bloqueo de AD. Sin embargo, le recomendamos que establezca el **ExtranetLockoutThreshold** valor del parámetro en un valor que es menor que el umbral de bloqueo de cuentas de AD. Lo contrario, daría como resultado en AD FS no puede impedir que las cuentas que se bloquee en Active Directory. 

Un ejemplo de cómo habilitar la característica de bloqueo de Extranet con 15 número máximo de intentos de contraseña incorrecta y la duración del bloqueo temporal de 30 minutos es como sigue:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Esta configuración se aplicará a todos los dominios que puede autenticar el servicio AD FS. La manera en que funciona es que cuando AD FS recibe una solicitud de autenticación, el controlador de dominio principal (PDC) de acceso a través de una llamada LDAP y realizar una búsqueda para el **badPwdCount** atributo para el usuario en el PDC. Si AD FS encuentra el valor de **badPwdCount** > = ExtranetLockoutThreshold configuración y no ha superado el tiempo definido en la ventana de observación Extranet aún, AD FS rechazará la solicitud inmediatamente, lo que significa que independientemente de si el usuario que escriba una contraseña correcto o incorrecta de extranet, se producirá un error en el inicio de sesión porque AD FS no envía las credenciales a AD. AD FS no mantiene ningún estado con respecto a **badPwdCount** o bloqueo de cuentas de usuario. AD FS usa AD para todos los Estados de seguimiento. 

> [!warning]
> Cuando está habilitado el bloqueo de Extranet de AD FS en Server 2012 R2 AD FS en el PDC validadas todas las solicitudes de autenticación a través de lo WAP. Cuando el controlador de dominio principal no está disponible, los usuarios no podrán autenticarse desde la extranet.

Server 2016 ofrece un parámetro adicional que permite que AD FS para retroceder a otro controlador de dominio cuando no está disponible el PDC:

- **ExtranetLockoutRequirePDC &lt;booleano&gt;**  : cuando habilita: bloqueo de extranet requiere un controlador de dominio principal (PDC). Cuando deshabilita: bloqueo de extranet retrocederá para otro controlador de dominio en caso de que el PDC no está disponible.

Puede usar el siguiente comando de Windows PowerShell para configurar el bloqueo de extranet de AD FS en Server 2016:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Trabajar con la directiva de bloqueo de Active Directory
La característica de bloqueo de Extranet de AD FS funciona independientemente de la directiva de bloqueo de AD. Sin embargo, es necesario para asegurarse de que la configuración para el bloqueo de Extranet está configurado correctamente para que puede servir para su propósito de seguridad con la directiva de bloqueo de AD.
Echemos un vistazo a la directiva de bloqueo de AD primero. Hay tres opciones con respecto a la directiva de bloqueo de AD:
- **Umbral de bloqueo de cuenta**: esta configuración es similar a la configuración de ExtranetLockoutThreshold en AD FS. Determina el número de intentos de inicio de sesión erróneos que hará que una cuenta de usuario que se bloquee. Para proteger las cuentas de usuario de un ataque de bloqueo de cuenta malintencionado, que desea establecer el valor de ExtranetLockoutThreshold en AD FS &lt; el valor de umbral de bloqueo de cuenta de AD
- **Duración del bloqueo de cuenta**: esta configuración determina el tiempo que un usuario para la cuenta está bloqueada. Esta configuración no tiene mucha importancia en esta conversación como bloqueo de Extranet siempre debe ocurrir antes de bloqueo de AD se produce si se configura correctamente
- **Restablecer contador de bloqueo de cuenta después**: esta configuración determina cuánto tiempo debe transcurrir desde el último error de inicio de sesión del usuario antes de **badPwdCount** se restablece a 0. En orden para la característica de bloqueo de Extranet de AD FS para que funcione bien con la directiva de bloqueo de AD, desea asegurarse de que el valor de ExtranetObservationWindow en AD FS &gt; el valor Restablecer contador de bloqueo de cuenta después de AD. Los ejemplos siguientes explican por qué.  

Vamos a Eche un vistazo a dos ejemplos y vea cómo **badPwdCount** cambia con el tiempo en función de las distintas configuraciones y Estados. Supongamos que en ambos ejemplos **umbral de bloqueo de cuenta** = 4 y **ExtranetLockoutThreshold** = 2. El **rojo** flecha representa el intento de contraseña incorrecta, el **verde** flecha representa un intento de contraseña válida. En el ejemplo #1, **ExtranetObservationWindow** &gt; **Restablecer contador de bloqueo de cuenta después**. En el ejemplo #2, **ExtranetObservationWindow** &lt; **Restablecer contador de bloqueo de cuenta después**. 

### <a name="example-1"></a>Ejemplo 1
![Ejemplo 1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Ejemplo 2
![Ejemplo 1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Como puede ver en lo anterior, hay dos condiciones cuando **badPwdCount** se restablecerá en 0. Uno es cuando hay un inicio de sesión correcto. El otro es cuando se trata de restablecer este contador tal como se define en **Restablecer contador de bloqueo de cuenta después** configuración. Cuando **Restablecer contador de bloqueo de cuenta después** &lt; **ExtranetObservationWindow**, una cuenta no tiene ningún riesgo de que se bloquee por AD. Sin embargo, si **Restablecer contador de bloqueo de cuenta después** &gt; **ExtranetObservationWindow**, es probable que una cuenta puede bloquearse por AD, pero en un "modo retrasa". Puede tardar un rato en recuperarlas una cuenta bloqueada por AD según la configuración como AD FS sólo permitirá que un intento de contraseña incorrecta durante su período de observación hasta **badPwdCount** alcanza **umbral de bloqueo de cuenta** .

Para obtener más información, consulte [configuración de bloqueo de cuenta](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/). 

## <a name="known-issues"></a>Problemas conocidos
Hay un problema conocido que no se puede autenticar la cuenta de usuario de AD con AD FS porque el **badPwdCount** atributo no se replica en el controlador de dominio que está consultando ADFS. Consulte [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) para obtener más detalles. Puede encontrar todos los AD FS QFE que se han publicado hasta ahora [aquí](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Puntos clave que recordar
- La característica de bloqueo de Extranet solo funciona para la **escenario de extranet** donde entran las solicitudes de autenticación a través del Proxy de aplicación Web
- La característica de bloqueo de Extranet solo se aplica a **autenticación de nombre de usuario y contraseña**
- AD FS no mantiene ninguna pista de **badPwdCount** o los usuarios que son soft-bloqueada. AD FS usa AD para el seguimiento del estado de todos los
- AD FS realiza una búsqueda para el **badPwdCount** atributo mediante la llamada LDAP para el usuario en el PDC para cada intento de autenticación  
- Anterior a 2016 de AD FS se producirá un error si no se puede tener acceso el PDC. AD FS 2016 presenta mejoras que permitirán a AD FS para recurrir a otros controladores de dominio en el caso de controlador de dominio principal no está disponible. 
- AD FS permitirá que las solicitudes de autenticación de extranet if badPwdCount < ExtranetLockoutThreshold 
- Si **badPwdCount** >= **ExtranetLockoutThreshold** AND **badPasswordTime** + **ExtranetObservationWindow** < Hora actual, AD FS rechazará las solicitudes de autenticación de extranet
- Para evitar el bloqueo de cuenta malintencionado, debe asegurarse de que **ExtranetLockoutThreshold** < **umbral de bloqueo de cuenta** AND **ExtranetObservationWindow**  >  **Restablecer contador de bloqueo de cuenta**


## <a name="additional-references"></a>Referencias adicionales  
- [Procedimientos recomendados para proteger los servicios de federación de Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Delegar el acceso de Commandlet de Powershell de AD FS a los usuarios que no son administradores](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
