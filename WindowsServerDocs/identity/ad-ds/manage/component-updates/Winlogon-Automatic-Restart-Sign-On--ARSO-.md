---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Inicio de sesión con reinicio automático de Winlogon (ARSO)
description: Cómo el inicio de sesión con reinicio automático de Windows puede ayudar a los usuarios a ser más productivos.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56f485491340b3974d8bf5ba697c6cf01f3e56ac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868209"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Inicio de sesión con reinicio automático de Winlogon (ARSO)

Durante una Windows Update, hay procesos específicos de usuario que deben realizarse para que se complete la actualización. Estos procesos requieren que el usuario inicie sesión en su dispositivo. En el primer inicio de sesión después de que se haya iniciado una actualización, los usuarios deben esperar hasta que se completen estos procesos específicos de usuario para poder empezar a usar su dispositivo.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Cuando Windows Update inicia un reinicio automático, ARSO extrae las credenciales derivadas del usuario que ha iniciado sesión actualmente, las conserva en el disco y configura el inicio de sesión automático para el usuario. Windows Update que se ejecuta como sistema con privilegio TCB iniciará la llamada RPC para hacerlo.

Después del reinicio de Windows Update final, el usuario iniciará sesión automáticamente mediante el mecanismo de inicio de sesión automático y la sesión del usuario se rehidratará con los secretos persistentes. Además, el dispositivo está bloqueado para proteger la sesión del usuario. El bloqueo se iniciará a través de Winlogon, mientras que la autoridad de seguridad local (LSA) realiza la administración de las credenciales. Tras una configuración de ARSO y un inicio de sesión correctos, las credenciales guardadas se eliminan inmediatamente del disco.

Al iniciar sesión automáticamente y bloquear el usuario en la consola de, Windows Update puede completar los procesos específicos del usuario antes de que el usuario vuelva al dispositivo. De esta manera, el usuario puede empezar a usar inmediatamente su dispositivo.

ARSO trata los dispositivos administrados y no administrados de forma diferente. En el caso de los dispositivos no administrados, se usa el cifrado de dispositivos, pero no es necesario para que el usuario obtenga ARSO. En el caso de los dispositivos administrados, se necesitan TPM 2,0, SecureBoot y BitLocker para la configuración de ARSO. Los administradores de TI pueden invalidar este requisito a través de directiva de grupo. ARSO para dispositivos administrados solo está disponible actualmente para dispositivos Unidos a Azure Active Directory.

|   | Windows Update| apagado-g-t 0  | Reinicios iniciados por el usuario | API con marcas SHUTDOWN_ARSO/EWX_ARSO |
| --- | :---: | :---: | :---: | :---: |
| Dispositivos administrados | :heavy_check_mark:  | :heavy_check_mark: |   | :heavy_check_mark: |
| Dispositivos no administrados | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!NOTE]
> Después de un reinicio inducido por Windows Update, el último usuario interactivo inicia sesión automáticamente y la sesión está bloqueada. Esto permite que las aplicaciones de la pantalla de bloqueo de un usuario sigan ejecutándose a pesar del reinicio de la Windows Update.

![Página Configuración](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>#1 de Directiva

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>Inicio de sesión y bloqueo del último usuario interactivo automáticamente después de un reinicio

En Windows 10, ARSO está deshabilitado para las SKU de servidor y no se tiene en de las SKU de cliente.

**Ubicación de la Directiva de Grupo:** Configuración del equipo > Plantillas administrativas > componentes de Windows > Opciones de inicio de sesión de Windows

**Directiva de Intune:**

- Plataforma Windows 10 y versiones posteriores
- Tipo de perfil: Plantillas administrativas
- Ruta de acceso: \Windows Windows\windows Logon Options

**Compatible con:** Al menos Windows 10 versión 1903

**Descripción:**

Esta configuración de directiva controla si un dispositivo iniciará sesión automáticamente y bloqueará el último usuario interactivo después de que el sistema se reinicie o después de un apagado y arranque en frío.

Esto solo se produce si el último usuario interactivo no cerró la sesión antes del reinicio o el apagado.

Si el dispositivo está unido a Active Directory o Azure Active Directory, esta directiva solo se aplica a los reinicios de Windows Update. De lo contrario, se aplicará a los reinicios de Windows Update y a los reinicios y apagados iniciados por el usuario.

Si no establece esta configuración de Directiva, se habilita de forma predeterminada. Cuando la Directiva está habilitada, el usuario inicia sesión automáticamente y la sesión se bloquea automáticamente con todas las aplicaciones de la pantalla de bloqueo configuradas para ese usuario después de que arranque el dispositivo.

Después de habilitar esta Directiva, puede configurar sus valores a través de la Directiva ConfigAutomaticRestartSignOn, que configura el modo de inicio de sesión automático y bloqueo del último usuario interactivo después de un reinicio o arranque en frío.

Si deshabilita esta configuración de Directiva, el dispositivo no configura el inicio de sesión automático. Las aplicaciones de la pantalla de bloqueo del usuario no se reinician después de reiniciar el sistema.

**Editor del registro:**

| Nombre del valor | Type | Datos |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0 (habilitar ARSO) |
|   |   | 1 (deshabilitar ARSO) |

**Ubicación del registro de directivas:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>#2 de Directiva

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>Configurar el modo de inicio de sesión automático y bloqueo del último usuario interactivo después de un reinicio o un arranque en frío

**Ubicación de la Directiva de Grupo:** Configuración del equipo > Plantillas administrativas > componentes de Windows > Opciones de inicio de sesión de Windows

**Directiva de Intune:**

- Plataforma Windows 10 y versiones posteriores
- Tipo de perfil: Plantillas administrativas
- Ruta de acceso: \Windows Windows\windows Logon Options

**Compatible con:** Al menos Windows 10 versión 1903

**Descripción:**

Esta configuración de directiva controla la configuración en la que se produce un reinicio automático e inicio de sesión y bloqueo después de un reinicio o un arranque en frío. Si eligió "deshabilitado" en la Directiva "iniciar sesión y bloquear el último usuario interactivo después de un reinicio", el inicio de sesión automático no se realizará y no es necesario configurar esta Directiva.

Si habilita esta configuración de Directiva, puede elegir una de las dos opciones siguientes:

1. "Habilitado si BitLocker está activado y no suspendido" especifica que el inicio de sesión automático y el bloqueo solo se producirán si BitLocker está activo y no se suspende durante el reinicio o el apagado. En este momento se puede obtener acceso a los datos personales en el disco duro del dispositivo si BitLocker no está encendido o suspendido durante una actualización. La suspensión de BitLocker quita temporalmente la protección de los componentes del sistema y los datos, pero es posible que se necesite en determinadas circunstancias para actualizar correctamente los componentes críticos para el arranque.
   - BitLocker se suspende durante las actualizaciones si:
      - El dispositivo no tiene TPM 2,0 y PCR7, o bien
      - El dispositivo no usa un protector solo TPM
2. "Always Enabled" especifica que el inicio de sesión automático se producirá aunque BitLocker esté desactivado o suspendido durante el reinicio o el apagado. Cuando BitLocker no está habilitado, se puede acceder a los datos personales en el disco duro. El reinicio automático y el inicio de sesión solo se deben ejecutar con esta condición si está seguro de que el dispositivo configurado está en una ubicación física segura.

Si deshabilita o no configura esta opción, el inicio de sesión automático se establecerá de forma predeterminada en el comportamiento "habilitado si BitLocker está activado y no suspendido".

**Editor del registro**

| Nombre del valor | Type | Datos |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0 (habilitar ARSO si es seguro) |
|   |   | 1 (habilitar ARSO siempre) |

**Ubicación del registro de directivas:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>Solución de problemas

Cuando WinLogon se bloquea automáticamente, el seguimiento de estado de WinLogon se almacenará en el registro de eventos de WinLogon.

Se registra el estado de un intento de configuración de inicio de sesión automático

- Si se realiza correctamente
   - lo registra como tal
- Si es un error:
   - registra el error
- Cuando cambia el estado de BitLocker:
   - se registrará la eliminación de credenciales
   - Estos se almacenarán en el registro operativo de LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivos por los que se podría producir un error de inicio de sesión automático

Hay varios casos en los que no se puede lograr un inicio de sesión automático de usuario.  Esta sección está pensada para capturar los escenarios conocidos en los que esto puede ocurrir.

### <a name="user-must-change-password-at-next-login"></a>El usuario debe cambiar la contraseña en el siguiente inicio de sesión

El inicio de sesión de usuario puede entrar en un estado bloqueado cuando se requiere un cambio de contraseña en el siguiente inicio de sesión.  Esto puede detectarse antes de reiniciarse en la mayoría de los casos, pero no en todos (por ejemplo, se puede tener acceso a la expiración de contraseña entre el apagado y el siguiente inicio de sesión).

### <a name="user-account-disabled"></a>Cuenta de usuario deshabilitada

Se puede mantener una sesión de usuario existente incluso si está deshabilitada.  El reinicio de una cuenta que está deshabilitada se puede detectar localmente en la mayoría de los casos, en función de la Directiva de equipo, puede que no sea para las cuentas de dominio (algunos escenarios de inicio de sesión en caché de dominio funcionan aunque la cuenta esté deshabilitada en el controlador de dominio).

### <a name="logon-hours-and-parental-controls"></a>Horas de inicio de sesión y controles parentales

Las horas de inicio de sesión y los controles parentales pueden prohibir la creación de una nueva sesión de usuario.  Si se produjera un reinicio durante esta ventana, el usuario no tendría permiso para iniciar sesión.  Existe una directiva adicional que provoca el bloqueo o cierre de sesión como una acción de cumplimiento. Se registra el estado de un intento de configuración de inicio de sesión automático.

## <a name="security-details"></a>Detalles de seguridad

### <a name="credentials-stored"></a>Credenciales almacenadas

|   | Hash de contraseña | Clave de credencial | Vale de concesión de vales | Token de actualización principal |
| --- | :---: | :---: | :---: | :---: |
| Cuenta local | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Cuenta de MSA | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Azure AD cuenta unida | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark: (si es híbrido) | :heavy_check_mark: |
| Cuenta unida a un dominio | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark: (si es híbrido) |

### <a name="credential-guard-interaction"></a>Interacción de Credential Guard

Si un dispositivo tiene habilitada la protección de credenciales, los secretos derivados de un usuario se cifran con una clave específica de la sesión de arranque actual. Por lo tanto, ARSO no se admite actualmente en dispositivos con Credential Guard habilitado.

## <a name="additional-resources"></a>Recursos adicionales

El inicio de sesión automático es una característica que está presente en Windows para varias versiones. Se trata de una característica documentada de Windows que incluso tiene herramientas como el inicio de sesión automático para Windows [http:/technet. Microsoft. com/Sysinternals/bb963905. aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx). Permite a un solo usuario del dispositivo iniciar sesión automáticamente sin escribir credenciales. Las credenciales se configuran y almacenan en el registro como secreto de LSA cifrado. Esto podría ser problemático para muchos casos secundarios en los que puede producirse un bloqueo de cuenta entre el tiempo de cama y la reactivación, especialmente si la ventana de mantenimiento es normalmente durante este tiempo.
