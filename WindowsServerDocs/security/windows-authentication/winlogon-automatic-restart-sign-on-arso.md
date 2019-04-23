---
title: Inicio de sesión con reinicio automático de Winlogon (ARSO)
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 172eb34fbfdb8a91adf55e35f888e90f5688d0e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849246"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Inicio de sesión con reinicio automático de Winlogon (ARSO)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Autor**: Justin Turner, jefe ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
## <a name="overview"></a>Información general  
Windows 8 introdujo las aplicaciones de pantalla de bloqueo.  Estas son las aplicaciones que se ejecutan y mostrar las notificaciones mientras la sesión del usuario está bloqueada (calendario citas, correo electrónico y mensajes, etcetera.).  Los dispositivos que se reinicien Debido al proceso de actualización de Windows no podrán mostrar estas notificaciones de pantalla de bloqueo tras el reinicio.  Algunos usuarios dependen de estas aplicaciones de la pantalla de bloqueo.  
  
## <a name="whats-changed"></a>¿Qué es lo que ha cambiado?  
Cuando un usuario inicia sesión en un dispositivo Windows 8.1, LSA guardará las credenciales de usuario en la memoria cifrada accesibles solo por lsass.exe. Cuando Windows Update se inicia un reinicio automático sin la presencia de usuario, estas credenciales se usarán para configurar el inicio de sesión automático para el usuario. Actualización de Windows se ejecuta como sistema con privilegio TCB iniciará la llamada RPC para hacerlo.  
  
Al reiniciar, el usuario configurará automáticamente y se inicia sesión a través del mecanismo de inicio de sesión automático y, a continuación, además de bloqueado para proteger la sesión del usuario. El bloqueo, se iniciará a través de Winlogon, mientras que la administración de credenciales se realiza mediante la LSA.  Al iniciar sesión automáticamente en y el bloqueo del usuario en la consola, aplicaciones de pantalla de bloqueo del usuario será reinicien y estén disponibles.  
  
> [!NOTE]  
> Después de una actualización de Windows inducidos por el reinicio, el último usuario interactivo es iniciar sesión automáticamente en y la sesión está bloqueada por lo que pueden ejecutar aplicaciones de pantalla de bloqueo del usuario.  
  
![Captura de pantalla muestra la pantalla de bloqueo](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)  
  
![Captura de pantalla que muestra el bloqueo de las aplicaciones de pantalla](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)  
  
**Introducción rápida**  
  
-   Windows Update requiere reinicio  
  
-   ¿Es posible reiniciar equipo (*no hay aplicaciones que se ejecutan que podrían perder datos*)?  
  
    -   Reiniciar automáticamente  
  
    -   Volver a iniciar sesión  
  
    -   Máquina de bloqueo  
  
-   Habilitar o deshabilitar la directiva de grupo  
  
    -   Está deshabilitado de forma predeterminada en la SKU de servidor  
  
-   ¿Por qué?  
  
    -   Algunas actualizaciones no pueden finalizar hasta que el usuario inicie sesión en.  
  
    -   Mejor experiencia del usuario: no tiene que esperar 15 minutos para finalizar la instalación de actualizaciones  
  
-   ¿Cómo? AutoLogon  
  
    -   almacena la contraseña, esa credencial se utiliza para iniciar la sesión  
  
    -   credencial guarda como un secreto LSA en la memoria paginada  
  
    -   Solo puede habilitarse si BitLocker está habilitado  
  
## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Directiva de grupo: Inicio de sesión de último usuario interactivo automáticamente después del reinicio iniciadas por el sistema  
En Windows 8.1 / Windows Server 2012 R2, optar por la SKU de servidor y dejar de participar para las SKU de cliente de inicio de sesión automático del usuario de pantalla de bloqueo después del reinicio de Windows Update.  
  
**Ubicación de la directiva:** Configuración del equipo > directivas > plantillas administrativas > componentes de Windows > opción de inicio de sesión de Windows  
  
**Nombre de la directiva:** Inicio de sesión de último usuario interactivo automáticamente después del reinicio iniciadas por el sistema  
  
**Compatible con:** Al menos Windows Server 2012 R2, Windows 8.1 o Windows RT 8.1  
  
**Descripción/Help:**  
  
Esta configuración de directiva controla si un dispositivo se automáticamente inicio de sesión en el último usuario interactivo después de la actualización de Windows reinicia el sistema.  
  
Si habilita o no configura esta directiva, el dispositivo guarda de forma segura las credenciales del usuario (incluido el nombre de usuario, dominio y contraseña cifrada) para configurar el inicio de sesión automático después de reiniciar una actualización de Windows. Tras el reinicio de Windows Update, es el usuario ha iniciado sesión automáticamente y la sesión se bloquee automáticamente con todas las aplicaciones de pantalla de bloqueo configuradas para ese usuario.  
  
Si deshabilita a esta configuración de directiva, el dispositivo no almacena las credenciales del usuario para el inicio de sesión automático después de reiniciar una actualización de Windows. No se reinician las aplicaciones de pantalla de bloqueo de los usuarios una vez reiniciado el sistema.  
  
**Editor del registro**  
  
|Nombre del valor|Tipo|Datos|  
|-------|----|----|  
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Ejemplo:**<br /><br />0 (habilitado)<br /><br />1 (deshabilitado)|  
  
**Ubicación del registro de directiva:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System  
  
**Tipo:** DWORD  
  
**Nombre del registro:** DisableAutomaticRestartSignOn  
  
Valor: 0 o 1  
  
0 = habilitado  
  
1 = deshabilitado  
  
![Captura de pantalla muestra la configuración de directiva controla la interfaz de usuario que se puede especificar si un dispositivo se automáticamente inicio de sesión en el último usuario interactivo después de que Windows Update se reinicia el sistema](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)  
  
## <a name="troubleshooting"></a>Solución de problemas  
Cuando WinLogon se bloquee automáticamente, seguimiento de estado de WinLogon se almacenarán en el registro de eventos de WinLogon.  
  
Se registra el estado de un intento de configuración de inicio de sesión automático  
  
-   Si es correcto  
  
    -   por lo tanto lo graba  
  
-   Si se trata de un error:  
  
    -   registra el error  
  
-   Cuando se cambia el estado de BitLocker:  
  
    -   se registrará la eliminación de credenciales  
  
        -   Estos se almacenarán en el registro operativo de LSA.  
  
### <a name="reasons-why-autologon-might-fail"></a>Motivos de por qué podría producir un error de inicio de sesión automático  
Hay varios casos en los que no se puede lograr un inicio de sesión de usuario automático.  En esta sección está diseñada para capturar el escenario conocido en el que esto puede ocurrir.  
  
### <a name="user-must-change-password-at-next-login"></a>Usuario debe cambiar la contraseña en el siguiente inicio de sesión  
Inicio de sesión de usuario puede escribir un estado bloqueado cuando se requiere cambio de contraseña en el siguiente inicio de sesión.  Esto puede ser detectado antes de reinicio en la mayoría de los casos, pero no todos los (por ejemplo, puede tener acceso la expiración de contraseña entre apagado e inicio de sesión siguiente.  
  
### <a name="user-account-disabled"></a>Cuenta de usuario deshabilitada  
Incluso si se deshabilita, se puede mantener una sesión de usuario existente.  Reinicio de una cuenta que está deshabilitada se puede detectar localmente en la mayoría de los casos de antemano, dependiendo de la directiva de grupo no puede ser para cuentas de dominio (algún dominio en caché trabajo escenarios de inicio de sesión incluso si la cuenta está deshabilitada en el controlador de dominio).  
  
### <a name="logon-hours-and-parental-controls"></a>Horas de inicio de sesión y el control parental  
Las horas de inicio de sesión y el control parental puede impedir que se crea una nueva sesión de usuario.  Si fuera un reinicio para que se produzca durante este período, el usuario no estarían permitido para iniciar sesión.  No hay directivas adicionales que provoca el bloqueo o cierre de sesión como una acción de cumplimiento.  Esto podía resultar problemático en muchos casos secundarios que puede producirse el bloqueo de cuenta entre la hora de la cama y reactivación, especialmente si la ventana de mantenimiento es habitualmente durante este tiempo.  
  
## <a name="additional-resources"></a>Recursos adicionales  
**Tabla SEQ tabla \\ \* árabe 3: Glosario ARSO**  
  
|Término|Definición|  
|----|-------|  
|Autologon|Inicio de sesión automático es una característica que ha estado presente en Windows para varias versiones.  Es una característica de Windows que incluso tiene herramientas como el inicio de sesión automático para Windows v3.01 documentada  *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Permite que un usuario único del dispositivo iniciar sesión automáticamente sin especificar las credenciales. Las credenciales son configuradas y almacenadas en el registro como un secreto LSA cifrado.|  
  

