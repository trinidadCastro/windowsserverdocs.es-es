---
description: 'Más información acerca de: Sign-On de reinicio automático de Winlogon (ARSO)'
title: Inicio de sesión con reinicio automático de Winlogon (ARSO)
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 4a9aec72bd91bc28975dea1c9ba6c28cbb512c6c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050223"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Inicio de sesión con reinicio automático de Winlogon (ARSO)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Autor**: Diego Turner, Ingeniero de soporte técnico de nivel superior con el grupo de Windows

> [!NOTE]
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.

## <a name="overview"></a>Información general
Windows 8 presentó aplicaciones de pantalla de bloqueo.  Estas son las aplicaciones que ejecutan y muestran las notificaciones mientras la sesión del usuario está bloqueada (citas del calendario, correo electrónico y mensajes, etc.).  Los dispositivos que se reinician debido al proceso de Windows Update no pueden mostrar estas notificaciones de la pantalla de bloqueo tras el reinicio.  Algunos usuarios dependen de estas aplicaciones de pantalla de bloqueo.

## <a name="whats-changed"></a>¿Qué es lo que ha cambiado?
Cuando un usuario inicia sesión en un dispositivo Windows 8.1, LSA guardará las credenciales de usuario en memoria cifrada a la que solo pueda tener acceso lsass.exe. Cuando Windows Update inicia un reinicio automático sin presencia de usuario, estas credenciales se usarán para configurar el inicio de sesión automático para el usuario. Windows Update que se ejecuta como sistema con privilegio TCB iniciará la llamada RPC para hacerlo.

Al reiniciar, el usuario iniciará sesión automáticamente a través del mecanismo de inicio de sesión automático y, a continuación, se bloqueará para proteger la sesión del usuario. El bloqueo se iniciará a través de Winlogon, mientras que la administración de credenciales se realiza mediante LSA.  Al iniciar sesión automáticamente y bloquear el usuario en la consola, las aplicaciones de la pantalla de bloqueo del usuario se reiniciarán y estarán disponibles.

> [!NOTE]
> Después de un reinicio inducido por Windows Update, el último usuario interactivo inicia sesión automáticamente y se bloquea la sesión, por lo que se pueden ejecutar las aplicaciones de la pantalla de bloqueo del usuario.

![Captura de pantalla que muestra la pantalla de bloqueo](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)

![Captura de pantalla que muestra las aplicaciones de pantalla de bloqueo](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)

**Información general rápida**

-   Windows Update requiere reiniciar

-   ¿El equipo puede reiniciarse (*no se ejecutan aplicaciones que podrían perder datos*)?

    -   Reiniciar para usted

    -   Volver a iniciar sesión

    -   Bloquear equipo

-   Habilitada o deshabilitada por directiva de grupo

    -   Deshabilitado de forma predeterminada en las SKU de servidor

-   ¿Por qué?

    -   Algunas actualizaciones no pueden finalizar hasta que el usuario vuelva a iniciar sesión.

    -   Mejor experiencia de usuario: no tiene que esperar 15 minutos a que finalice la instalación de las actualizaciones

-   ¿Cómo lo hago? Inicio de sesión automático

    -   almacena la contraseña, usa esa credencial para iniciar sesión.

    -   guarda la credencial como secreto de LSA en la memoria paginada

    -   Solo se puede habilitar si BitLocker está habilitado

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Directiva de grupo: iniciar sesión último usuario interactivo automáticamente después de un reinicio Iniciado por el sistema
En Windows 8.1/Windows Server 2012 R2, el inicio de sesión automático del usuario de la pantalla de bloqueo después de un reinicio de Windows Update es participar en las SKU de servidor y no participar en las SKU de cliente.

**Ubicación de la Directiva:** Configuración del equipo > directivas > Plantillas administrativas > componentes de Windows > opción de inicio de sesión de Windows

**Nombre de la Directiva:** Iniciar sesión último usuario interactivo automáticamente después de un reinicio Iniciado por el sistema

**Compatible con:** Al menos Windows Server 2012 R2, Windows 8.1 o Windows RT 8,1

**Descripción/ayuda:**

Esta configuración de directiva controla si un dispositivo iniciará sesión automáticamente en el último usuario interactivo después de que Windows Update reinicie el sistema.

Si habilita o no establece esta configuración de Directiva, el dispositivo guarda de forma segura las credenciales del usuario (incluido el nombre de usuario, el dominio y la contraseña cifrada) para configurar el inicio de sesión automático después de un reinicio de Windows Update. Después del reinicio de Windows Update, el usuario inicia sesión automáticamente y la sesión se bloquea automáticamente con todas las aplicaciones de la pantalla de bloqueo configuradas para ese usuario.

Si deshabilita esta configuración de Directiva, el dispositivo no almacena las credenciales del usuario para el inicio de sesión automático después de un reinicio de Windows Update. Las aplicaciones de la pantalla de bloqueo de los usuarios no se reinician después de reiniciar el sistema.

**Editor del registro**

|Nombre del valor|Tipo|Datos|
|-------|----|----|
|DisableAutomaticRestartSignOn|DWORD|0<p>**Ejemplo**:<p>0 (habilitado)<p>1 (deshabilitado)|

**Ubicación del registro de directivas:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

**Nombre del registro:** DisableAutomaticRestartSignOn

Valor: 0 o 1

0 = habilitado

1 = deshabilitado

![Captura de pantalla que muestra la configuración de la Directiva interfaz de usuario, donde puede especificar si un dispositivo iniciará sesión automáticamente en el último usuario interactivo después de que Windows Update reinicie el sistema.](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Solución de problemas
Cuando WinLogon se bloquea automáticamente, el seguimiento de estado de WinLogon se almacenará en el registro de eventos de WinLogon.

Se registra el estado de un intento de configuración de inicio de sesión automático

-   Si se realiza correctamente

    -   lo registra como tal

-   Si es un error:

    -   registra el error

-   Cuando cambia el estado de BitLocker:

    -   se registrará la eliminación de credenciales

        -   Estos se almacenarán en el registro operativo de LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivos por los que se podría producir un error de inicio de sesión automático
Hay varios casos en los que no se puede lograr un inicio de sesión automático de usuario.  Esta sección está pensada para capturar los escenarios conocidos en los que esto puede ocurrir.

### <a name="user-must-change-password-at-next-login"></a>El usuario debe cambiar la contraseña en el siguiente inicio de sesión
El inicio de sesión de usuario puede entrar en un estado bloqueado cuando se requiere un cambio de contraseña en el siguiente inicio de sesión.  Esto puede detectarse antes de reiniciarse en la mayoría de los casos, pero no en todos (por ejemplo, se puede tener acceso a la expiración de contraseña entre el apagado y el siguiente inicio de sesión).

### <a name="user-account-disabled"></a>Cuenta de usuario deshabilitada
Se puede mantener una sesión de usuario existente incluso si está deshabilitada.  El reinicio de una cuenta que está deshabilitada se puede detectar localmente en la mayoría de los casos, en función de la Directiva de equipo, puede que no sea para las cuentas de dominio (algunos escenarios de inicio de sesión en caché de dominio funcionan aunque la cuenta esté deshabilitada en el controlador de dominio).

### <a name="logon-hours-and-parental-controls"></a>Horas de inicio de sesión y controles parentales
Las horas de inicio de sesión y los controles parentales pueden prohibir la creación de una nueva sesión de usuario.  Si se produjera un reinicio durante esta ventana, el usuario no tendría permiso para iniciar sesión.  Existe una directiva adicional que provoca el bloqueo o cierre de sesión como una acción de cumplimiento.  Esto podría ser problemático para muchos casos secundarios en los que puede producirse un bloqueo de cuenta entre el tiempo de cama y la reactivación, especialmente si la ventana de mantenimiento es normalmente durante este tiempo.

## <a name="additional-resources"></a>Recursos adicionales
**Tabla SEQ \\ \* letra 3: Glosario Arso**

|Término|Definición|
|----|-------|
|Autologon|El inicio de sesión automático es una característica que está presente en Windows para varias versiones.  Se trata de una característica documentada de Windows que incluso tiene herramientas como el inicio de sesión automático para Windows v 3.01 *[http:/technet. Microsoft. com/Sysinternals/bb963905. aspx.](/sysinternals/downloads/autologon)*<p>Permite a un solo usuario del dispositivo iniciar sesión automáticamente sin escribir credenciales. Las credenciales se configuran y almacenan en el registro como secreto de LSA cifrado.|
