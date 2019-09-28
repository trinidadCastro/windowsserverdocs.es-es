---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar AD FS protección de bloqueo de extranet
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb5958f8205271fe3ab2258ed9812ae03f2a0be0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358212"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>Configurar AD FS protección de bloqueo de extranet

En AD FS en Windows Server 2012 R2, se presentó una característica de seguridad denominada bloqueo de extranet.  Con esta característica, AD FS "detendrá" la autenticación de la cuenta de usuario "malintencionada" desde fuera durante un período de tiempo.  Esto evita que las cuentas de usuario se bloqueen en Active Directory.  Además de proteger a los usuarios de un bloqueo de cuenta de AD, AD FS el bloqueo de extranet también protege contra los ataques de adivinación de contraseñas por fuerza bruta.

> [!NOTE]
> Esta característica solo funciona para el **escenario de extranet** en el que las solicitudes de autenticación llegan a través del proxy de aplicación web y solo se aplican a la **autenticación de nombre de usuario y contraseña**.

## <a name="advantages-of-extranet-lockout"></a>Ventajas del bloqueo de extranet
El bloqueo de extranet proporciona las siguientes ventajas clave:
- Protege las cuentas de usuario de **ataques por fuerza bruta** en los que un atacante intenta adivinar la contraseña de un usuario al enviar continuamente solicitudes de autenticación. En este caso, AD FS bloqueará la cuenta de usuario malintencionado para el acceso a la extranet
- Protege las cuentas de usuario de **bloqueos de cuentas malintencionados** en los que un atacante desea bloquear una cuenta de usuario mediante el envío de solicitudes de autenticación con contraseñas incorrectas. En este caso, aunque la cuenta de usuario se bloqueará mediante AD FS para el acceso a la extranet, la cuenta de usuario real en AD no se bloquea y el usuario todavía puede acceder a los recursos corporativos dentro de la organización. Esto se conoce como **bloqueo flexible**.

## <a name="how-it-works"></a>Cómo funciona
Hay 3 configuraciones en AD FS que se deben configurar para habilitar esta característica: 
- **EnableExtranetLockout &lt;Boolean @ no__t-2** establezca este valor booleano en true si desea habilitar el bloqueo de extranet.
- **ExtranetLockoutThreshold &lt;Integer @ no__t-2** define el número máximo de intentos de contraseña incorrectos. Una vez alcanzado el umbral, AD FS rechazará inmediatamente las solicitudes de la extranet sin intentar ponerse en contacto con el controlador de dominio para la autenticación, independientemente de si la contraseña es correcta o no, hasta que se pase la ventana de observación de extranet. Esto significa que el valor del atributo **badPwdCount** de una cuenta de ad no aumentará mientras la cuenta esté bloqueada de forma temporal.
- **ExtranetObservationWindow &lt;TimeSpan @ no__t-2** determina durante cuánto tiempo se bloqueará la cuenta de usuario. AD FS comenzará a realizar la autenticación de nombre de usuario y contraseña cuando se pase la ventana. AD FS usa el atributo de AD badPasswordTime como referencia para determinar si la ventana de observación de extranet se ha superado o no. La ventana ha pasado si la hora actual > badPasswordTime + ExtranetObservationWindow. 

> [!NOTE]
> AD FS el bloqueo de la extranet funciona de forma independiente de las directivas de bloqueo de AD. Sin embargo, se recomienda encarecidamente establecer el valor del parámetro **ExtranetLockoutThreshold** en un valor que sea menor que el umbral de bloqueo de cuenta de ad. Si no lo hace, AD FS no puede proteger las cuentas para que no se bloqueen en Active Directory. 

A continuación se muestra un ejemplo de cómo habilitar la característica de bloqueo de extranet con un máximo de 15 intentos de contraseña incorrecta y una duración de bloqueo temporal de 30 minutos:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

Esta configuración se aplicará a todos los dominios que el servicio AD FS pueda autenticar. La forma en que funciona es que, cuando AD FS recibe una solicitud de autenticación, tendrá acceso al controlador de dominio principal (PDC) a través de una llamada LDAP y realizará una búsqueda del atributo **badPwdCount** del usuario en el PDC. Si AD FS encuentra el valor de **badPwdCount** > = ExtranetLockoutThreshold y el tiempo definido en la ventana de observación de extranet no se ha pasado todavía, AD FS rechazará la solicitud inmediatamente, lo que significa que, independientemente de si el usuario escribe una buena o mala contraseña de extranet, se producirá un error en el inicio de sesión porque AD FS no envía las credenciales a AD. AD FS no mantiene ningún estado con respecto a las cuentas de usuario de **badPwdCount** o bloqueadas. AD FS usa AD para todo el seguimiento de estado. 

> [!warning]
> Cuando AD FS el bloqueo de extranet en el servidor 2012 R2 está habilitado, se validan todas las solicitudes de autenticación a través de WAP AD FS en el PDC. Cuando el PDC no está disponible, los usuarios no podrán autenticarse desde la extranet.

El servidor 2016 ofrece un parámetro adicional que permite a AD FS retroceder a otro controlador de dominio cuando el PDC no está disponible:

- **ExtranetLockoutRequirePDC &lt;Boolean @ no__t-2** -cuando está habilitado: el bloqueo de extranet requiere un controlador de dominio principal (PDC). Cuando se deshabilita: el bloqueo de la extranet se reservará a otro controlador de dominio en caso de que el PDC no esté disponible.

Puede usar el siguiente comando de Windows PowerShell para configurar el AD FS el bloqueo de la Extranet en el servidor 2016:

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>Trabajar con la Directiva de bloqueo de Active Directory
La característica de bloqueo de la extranet de AD FS funciona independientemente de la Directiva de bloqueo de AD. Sin embargo, debe asegurarse de que la configuración del bloqueo de la extranet está configurada correctamente para que pueda atender su finalidad de seguridad con la Directiva de bloqueo de AD.
Echemos un vistazo primero a la Directiva de bloqueo de AD. Hay tres opciones de configuración relacionadas con la Directiva de bloqueo en AD:
- **Umbral de bloqueo de cuenta**: este valor es similar al valor de ExtranetLockoutThreshold en AD FS. Determina el número de intentos de inicio de sesión erróneos que harán que se bloquee una cuenta de usuario. Para proteger las cuentas de usuario de un ataque de bloqueo de cuenta malintencionado, desea establecer el valor de ExtranetLockoutThreshold en AD FS &lt; el valor de umbral de bloqueo de cuenta en AD
- **Duración del bloqueo de cuenta**: esta opción determina durante cuánto tiempo se bloquea una cuenta de usuario. Esta configuración no es muy importante en esta conversación, ya que el bloqueo de extranet siempre debe realizarse antes de que se produzca el bloqueo de AD si se configura correctamente.
- **Restablecer contador de bloqueo de cuenta después**de: esta opción determina cuánto tiempo debe transcurrir desde el último error de inicio de sesión del usuario antes de que **badPwdCount** se restablezca en 0. Para que la característica de bloqueo de la extranet de AD FS funcione bien con la Directiva de bloqueo de AD, desea asegurarse de que el valor de ExtranetObservationWindow en AD FS &gt; el contador de bloqueo de cuenta de restablecimiento después del valor de AD. En los ejemplos siguientes se explicará por qué.  

Echemos un vistazo a dos ejemplos y veamos cómo **badPwdCount** cambia con el tiempo según la configuración y los Estados diferentes. Supongamos que en ambos ejemplos, **umbral de bloqueo de cuenta** = 4 y **ExtranetLockoutThreshold** = 2. La flecha **roja** representa un intento incorrecto de contraseña, la flecha **verde** representa un buen intento de contraseña. En el ejemplo #1, **ExtranetObservationWindow** &gt; **restablece el contador de bloqueo de cuenta después**de. En el ejemplo #2, **ExtranetObservationWindow** &lt; **restablece el contador de bloqueo de cuenta después**de. 

### <a name="example-1"></a>Ejemplo 1
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>Ejemplo 2
![Example1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

Como puede ver en la anterior, hay dos condiciones en las que **badPwdCount** se restablecerá a 0. Una es cuando se produce un inicio de sesión correcto. La otra es cuando es el momento de restablecer este contador tal y como se define en **restablecer cuenta de bloqueo del contador después** de la configuración. Al **restablecer el contador de bloqueo de cuenta después** de &lt; **ExtranetObservationWindow**, una cuenta no tiene ningún riesgo de que ad lo bloquee. Sin embargo, si se **restablece el contador de bloqueo de cuenta después** de &gt; **ExtranetObservationWindow**, existe la posibilidad de que ad pueda bloquear una cuenta, pero en un "modo retrasado". Puede tardar un rato en obtener una cuenta bloqueada por AD en función de la configuración, ya que AD FS solo permitirá un intento incorrecto de contraseña durante su ventana de observación hasta que **badPwdCount** alcance el **umbral de bloqueo de cuenta**.

Para obtener más información, consulte [configuración del bloqueo de cuentas](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/). 

## <a name="known-issues"></a>Problemas conocidos
Existe un problema conocido en el que la cuenta de usuario de AD no se puede autenticar con AD FS porque el atributo **badPwdCount** no se replica en el controlador de dominio que ADFS está consultando. Consulte [2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) para obtener más detalles. Puede encontrar todos los AD FS QFE que se han publicado hasta [aquí](../deployment/updates-for-active-directory-federation-services-ad-fs.md).

## <a name="key-points-to-remember"></a>Puntos clave que se deben recordar
- La característica de bloqueo de la extranet solo funciona para el **escenario de extranet** en el que las solicitudes de autenticación llegan a través del proxy de aplicación Web.
- La característica de bloqueo de la extranet solo se aplica a la **autenticación de nombre de usuario & contraseña**
- AD FS no mantiene ninguna pista de **badPwdCount** o de los usuarios que están bloqueados de software. AD FS usa AD para todo el seguimiento de estado
- AD FS realiza una búsqueda para el atributo **badPwdCount** a través de la llamada LDAP para el usuario en el PDC para cada intento de autenticación  
- AD FS anteriores a 2016 producirán un error si no puede tener acceso al PDC. AD FS 2016 presentó mejoras que permitirán a AD FS revertir a otros controladores de dominio en caso de que el PDC no esté disponible. 
- AD FS permitirá las solicitudes de autenticación desde extranet si badPwdCount < ExtranetLockoutThreshold 
- Si **badPwdCount** >= **ExtranetLockoutThreshold** y **badPasswordTime** + **ExtranetObservationWindow** < hora actual, AD FS rechazará las solicitudes de autenticación de la extranet
- Para evitar**el bloqueo malintencionado** de cuentas, debe asegurarse de que el**umbral de bloqueo de cuenta** de **ExtranetLockoutThreshold** <  y el de @no__t **ExtranetObservationWindow**


## <a name="additional-references"></a>Referencias adicionales  
- [Prácticas recomendadas para proteger Servicios de federación de Active Directory (AD FS)](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [Delegar el acceso de Commandlet de Powershell de AD FS a los usuarios que no son administradores](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
