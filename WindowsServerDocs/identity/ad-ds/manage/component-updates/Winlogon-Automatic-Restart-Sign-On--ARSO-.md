---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: "Winlogon automático reinicio sesión (ARSO)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 27d4285d34105908555458a95bd70fc04fd2901a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon automático reinicio sesión (ARSO)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, Senior ingeniero de soporte técnico con el grupo de Windows

> [!NOTE]
> Este contenido está escrito por un ingeniero de soporte técnico al cliente de Microsoft y está destinada a arquitectos de sistemas que buscan explicaciones técnicas más profundos de características y soluciones de Windows Server 2012 R2 que normalmente que proporcionan los temas en TechNet y administradores experimentados. Sin embargo, no ha sufrido las mismas fases de edición, forma parte del lenguaje que parezca que menos refinada que lo que normalmente se encuentra en TechNet.

## <a name="overview"></a>Introducción
Windows 8 introdujo aplicaciones de la pantalla de bloqueo.  Estas son las aplicaciones que ejecutan y mostrar notificaciones mientras esté bloqueada la sesión del usuario (calendario citas, correo electrónico y los mensajes, etcetera.).  Los dispositivos que se reinicien Debido al proceso de actualización de Windows no se muestren estas notificaciones de pantalla de bloqueo al reiniciar.  Algunos usuarios dependen de estas aplicaciones de la pantalla de bloqueo.

## <a name="whats-changed"></a>¿Qué ha cambiado?
Cuando un usuario inicia sesión en un dispositivo de Windows 8.1, LSA guardará las credenciales del usuario en la memoria cifrada accesible solo por lsass.exe. Cuando Windows Update se inicia un reinicio automático sin la presencia del usuario, estas credenciales se pueden usar para configurar el inicio de sesión automático para el usuario. Actualización de Windows que se ejecutan como sistema con privilegios TCB iniciará la llamada RPC a hacerlo.

En el reinicio, el usuario se hará automáticamente registrarse mediante el mecanismo de inicio de sesión automático y, a continuación, además de bloqueado para proteger la sesión del usuario. El bloqueo se iniciará a través de Winlogon mientras que la administración de credenciales se realiza mediante la LSA.  Si inicia sesión automáticamente y bloquea el usuario en la consola, las aplicaciones de pantalla de bloqueo del usuario será reiniciado y estará disponible.

> [!NOTE]
> Después de una actualización de Windows induce reinicio, del último usuario interactivo inicia automáticamente y se bloquee la sesión por lo tanto, pueden ejecutar aplicaciones de pantalla de bloqueo del usuario.

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**Introducción rápida**

-   Windows Update requiere reiniciar

-   ¿Es posible reiniciar equipo (*ninguna aplicación que ejecuta perdería datos*)?

    -   Reiniciar automáticamente

    -   Vuelva a iniciar sesión

    -   Bloquear la máquina

-   Habilita o deshabilita la directiva de grupo

    -   Deshabilitado de manera predeterminada en las SKU de servidor

-   ¿Por qué?

    -   Algunas actualizaciones no se pueden finalizar hasta que el usuario inicie sesión en.

    -   Una mejor experiencia del usuario: no tiene que esperar 15 minutos para finalizar la instalación de actualizaciones

-   ¿Cómo? Inicio de sesión automático

    -   Almacena la contraseña, usa ese credenciales para iniciar sesión

    -   Guarda credenciales como un secreto de LSA en la memoria paginada

    -   Solo puede habilitarse si BitLocker está habilitado

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Directiva de grupo: Inicio de sesión último usuario interactivo automáticamente después de reiniciar iniciadas por el sistema
En Windows 8.1 o Windows Server 2012 R2, optar por la SKU de servidor y optar por no participar para las SKU de cliente de inicio de sesión automático del usuario de la pantalla de bloqueo después del reinicio de Windows Update.

**Ubicación de las directivas:** configuración del equipo > directivas > plantillas administrativas > componentes de Windows > opción de inicio de sesión de Windows

**Nombre de la directiva:** sesión último usuario interactivo automáticamente después de reiniciar iniciadas por el sistema

**Compatible con:** al menos Windows Server 2012 R2, Windows 8.1 o Windows RT 8.1

**Descripción/ayuda:**

Esta configuración de directiva controla si un dispositivo se automáticamente sesión del último usuario interactivo tras la actualización de Windows reinicia el sistema.

Si habilitar o no configura esta directiva, el dispositivo de forma segura guarda las credenciales del usuario (incluido el nombre de usuario, dominio y contraseña) para configurar el inicio de sesión automático después de reiniciar una actualización de Windows. Después del reinicio de Windows Update, el usuario ha iniciado sesión automáticamente y se bloquee automáticamente la sesión con todas las aplicaciones de pantalla de bloqueo configuradas para que el usuario.

Si deshabilitas a esta configuración de directiva, el dispositivo no almacena las credenciales del usuario para el inicio de sesión automático después de reiniciar una actualización de Windows. Aplicaciones de pantalla de bloqueo a los usuarios no se reinician tras reiniciar el sistema.

**Editor del registro**

|Nombre de valor|Tipo|Datos|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Ejemplo:**<br /><br />0 (habilitada)<br /><br />1 (deshabilita)|

**Ubicación de registro de la directiva:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

**Nombre del registro:** DisableAutomaticRestartSignOn

Valor: 0 o 1

0 = habilitado

1 = deshabilitado

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Solución de problemas
Cuando se bloquea automáticamente WinLogon, seguimiento de estado de WinLogon se almacenará en el registro de eventos de WinLogon.

Se registra el estado de un intento de configuración de inicio de sesión automático

-   Si es correcta

    -   Registra como tal

-   Si se trata de un error:

    -   Registra el error fue

-   Cuando cambia el estado de BitLocker:

    -   Se registrará la eliminación de credenciales

        -   Se almacenará en el registro operativo de LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivos por qué puede producir un error de inicio de sesión automático
Hay varios casos en los que no se puede lograr un inicio de sesión automático de usuario.  Esta sección está pensada para capturar los escenarios conocidos en el que esto puede ocurrir.

### <a name="user-must-change-password-at-next-login"></a>Usuario debe cambiar la contraseña en el siguiente inicio de sesión
Inicio de sesión de usuario puede entrar en estado bloqueado cuando es necesario cambiar de contraseña en el siguiente inicio de sesión.  Esto puede ser detectado antes de reiniciar en la mayoría de los casos, pero no todos (por ejemplo, puede alcanzarse expiración de contraseña entre el apagado y el siguiente inicio de sesión.

### <a name="user-account-disabled"></a>Cuenta de usuario deshabilitada
Incluso si deshabilitado, se puede mantener una sesión de usuario existente.  Reinicio de una cuenta que está deshabilitada se pueden detectar localmente en la mayoría de los casos por adelantado, dependiendo de gp puede que no sea para cuentas de dominio (algunas dominio en caché trabajo de escenarios de inicio de sesión incluso si la cuenta está deshabilitada en el controlador de dominio).

### <a name="logon-hours-and-parental-controls"></a>Horas de inicio de sesión y control parental
Las horas de inicio de sesión y los controles parentales pueden prohibir a una nueva sesión de usuario que se crean.  Si se produce durante este período de reinicio, el usuario no se permitirían para iniciar sesión.  No hay directivas adicionales que provoca el bloqueo o como una acción de cumplimiento de cierre de sesión.  Esto podría ser problemático para muchos casos de secundarios donde se produzcan bloqueo de cuenta entre la hora de la cama y reactivación, especialmente si la ventana de mantenimiento habitualmente durante este tiempo.

## <a name="additional-resources"></a>Recursos adicionales
**Tabla de tabla SEQ \\\ * árabe 3: ARSO glosario**

|Término|Definición|
|--------|--------------|
|Inicio de sesión automático|Inicio de sesión automático es una característica que se haya presente en Windows para varias versiones.  Es una característica de Windows que tenga incluso herramientas como el inicio de sesión automático para Windows v3.01 documentada *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Permite que un usuario único del dispositivo iniciar sesión automáticamente sin necesidad de escribir credenciales. Las credenciales son configuradas y almacena en el registro como un secreto LSA cifrado.|


