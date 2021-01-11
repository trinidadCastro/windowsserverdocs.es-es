---
title: 'Referencia de herramientas administrativas y tipos de inicio de sesión: Windows Server'
description: Identifique el riesgo de exposición de credenciales asociado a diferentes herramientas administrativas.
ms.topic: reference
ms.date: 12/15/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 9b3457cf489e11ce7ede4e0965ec8555102754ec
ms.sourcegitcommit: 4bbb284c909b91cc02b5988b28056f976bc2ca6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97676087"
---
# <a name="administrative-tools-and-logon-types"></a>Herramientas administrativas y tipos de inicio de sesión

Esta información de referencia se proporciona para ayudar a identificar el riesgo de exposición de credenciales asociado a diferentes herramientas administrativas para la administración remota.

En un escenario de administración remota, las credenciales siempre se exponen en el equipo de origen, de tal forma que siempre se recomienda una estación de trabajo de acceso con privilegios de confianza (PAW) para las cuentas confidenciales o de alto impacto. Si las credenciales están expuestas a posibles robos en el equipo de destino (remoto), depende principalmente del tipo de inicio de sesión de Windows utilizado por el método de conexión.

En esta tabla se incluye orientación para las herramientas administrativas más comunes y los métodos de conexión:

|Método de conexión|Tipo de inicio de sesión|Credenciales reutilizables en destino|Comentarios|
|-----------|-------|--------------------|------|
|Iniciar sesión en consola|Interactive (Interactivo)|v|Incluye el acceso remoto de hardware y apaga los KVM de red y de tarjetas.|
|RUNAS|Interactive (Interactivo)|v||
|RUNAS /NETWORK|NewCredentials|v|Clona la sesión actual de LSA para el acceso local, pero usa nuevas credenciales al conectarse a recursos de red.|
|Escritorio remoto (correcto)|RemoteInteractive|v|Si el cliente de Escritorio remoto está configurado para compartir recursos y dispositivos locales, también pueden verse comprometidos.|
|Escritorio remoto (error: el tipo de inicio de sesión se ha denegado)|RemoteInteractive|-|De forma predeterminada, si se produce un error de sesión de RDP, las credenciales solo se almacenan durante un breve período. Esto puede no ser el caso si el equipo está en peligro.|
|NET use * \\\SERVER|Red|-||
|Net use * \\\SERVER /u:user|Red|-||
|Complementos MMC para un equipo remoto|Red|-|Ejemplo: administración del equipo, visor de sucesos, administrador de dispositivos, servicios|
|PowerShell WinRM|Red|-|Ejemplo: Enter-PSSession server|
|PowerShell WinRM con CredSSP|NetworkClearText|v|New-PSSession<br />-Authentication Credssp<br />-Credential cred|
|PsExec sin credenciales explícitas|Red|-|Ejemplo: PsExec \\\server cmd|
|PsExec con credenciales explícitas|Red + interactivo|v|PsExec \\\server -u user -p pwd cmd<br />Crea varias sesiones de inicio de sesión.|
|Registro remoto|Red|-||
|Puerta de enlace de Escritorio remoto|Red|-|Autenticación en Puerta de enlace de Escritorio remoto.|
|Tarea programada|Batch|v|La contraseña también se guardará como secreto de LSA en el disco.|
|Ejecutar herramientas como un servicio|Service|v|La contraseña también se guardará como secreto de LSA en el disco.|
|Detectores de vulnerabilidades|Red|-|La mayoría de los detectores usan de forma predeterminada los inicios de sesión de red, aunque algunos proveedores pueden implementar inicios de sesión sin conexión de red y presentan mayor riesgo de robo de credenciales.|

Para la autenticación web, utilice la referencia de la tabla siguiente:

|Método de conexión|Tipo de inicio de sesión|Credenciales reutilizables en destino|Comentarios|
|-----------|-------|--------------------|------|
|IIS "Autenticación básica"|NetworkCleartext<br />(IIS 6.0+)<p>Interactive (Interactivo)<br />(anteriores a IIS 6.0)|v||
|IIS "Autenticación de Windows integrada"|Red|-|Proveedores de NTLM y Kerberos.|

Definiciones de columna:

- **Tipo de inicio de sesión**: identifica el tipo de inicio de sesión iniciado por la conexión.
- **Credenciales reutilizables en destino**: indica que los siguientes tipos de credenciales se almacenarán en la memoria de proceso LSASS en el equipo de destino, donde la cuenta especificada ha iniciado sesión localmente:
   - Hashes de LM y NT
   - TGT de Kerberos
   - Contraseña en texto sin formato (si corresponde).

Los símbolos de esta tabla se definen como sigue:

- (-) indica cuándo las credenciales no están expuestas.
- (v) indica cuándo las credenciales están expuestas.

Para las aplicaciones de administración que no están en esta tabla, puede determinar el tipo de inicio de sesión en el campo de tipo de inicio de sesión en los eventos de inicio de sesión de auditoría. Para más información, consulte [Auditar sucesos de inicio de sesión](/previous-versions/windows/it-pro/windows-server-2003/cc787567(v=ws.10)).

En los equipos basados en Windows, se procesan todas las autenticaciones como uno de los distintos tipos de inicio de sesión, con independencia del protocolo de autenticación o autenticador que se utilice. En esta tabla se incluyen los tipos más comunes de inicio de sesión y sus atributos en relación con el robo de credenciales:

|Tipo de inicio de sesión|#|Autenticadores aceptados|Credenciales reutilizables en sesión LSA|Ejemplos|
|-------|---|--------------|--------------------|------|
|Interactivo (también conocido como inicio de sesión local)|2|Contraseña, tarjeta inteligente,<br />otro|Sí|Inicio de sesión de la consola;<br />RUNAS;<br />Soluciones de control remoto de hardware (como KVM de red o Acceso remoto / apagado de tarjeta en el servidor)<br />Autenticación básica de IIS (antes de IIS 6.0)|
|Red|3|Contraseña,<br />Hash de NT,<br />Vale de Kerberos|N (excepto si la delegación está habilitada, vales de Kerberos presentes)|USO DE NET;<br />Llamadas RPC;<br />Registro remoto;<br />Autenticación de Windows integrada IIS;<br />Autenticación de Windows SQL;|
|Batch|4|Contraseña (almacenada como secreto de LSA)|Sí|Tareas programadas|
|Service|5|Contraseña (almacenada como secreto de LSA)|Sí|Servicios de Windows|
|NetworkCleartext|8|Contraseña|Sí|Autenticación básica de IIS (IIS 6.0 y posterior);<br />Windows PowerShell con CredSSP|
|NewCredentials|9|Contraseña|Sí|RUNAS /NETWORK|
|RemoteInteractive|10|Contraseña, tarjeta inteligente,<br />otro|Sí|Escritorio remoto (anteriormente conocido como "Terminal Services")|

Definiciones de columna:

- **Tipo de inicio de sesión**: tipo de inicio de sesión solicitado.
- **#** : identificador numérico para el tipo de inicio de sesión que se notifica en los eventos de auditoría en el registro de eventos de seguridad.
- **Autenticadores aceptados**: indica qué tipos de autenticadores podrán iniciar sesión de este tipo.
- **Credenciales reutilizables en la sesión de LSA**: indica si el tipo de inicio de sesión conlleva que la sesión LSA contiene credenciales, como contraseñas de texto sin formato, hashes de NT o vales de Kerberos, que se podrían usar para autenticarse en otros recursos de red.
- **Ejemplos**: muestra los escenarios comunes en los que se usa el tipo de inicio de sesión.

> [!NOTE]
> Para más información acerca de los tipos de inicio de sesión, consulte [Enumeración SECURITY_LOGON_TYPE](/windows/win32/api/ntsecapi/ne-ntsecapi-security_logon_type).

**Pasos siguientes**

[Planeación y diseño de AD DS](../ad-ds/plan/AD-DS-Design-and-Planning.md)
