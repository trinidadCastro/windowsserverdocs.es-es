---
title: Configuración de directiva de grupo utilizada en la autenticación de Windows
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: b26d9a454361c5487ceac408f0ab78db1657fa4b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640069"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Configuración de directiva de grupo utilizada en la autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de referencia para profesionales de TI se describe el uso y el impacto de la configuración de directiva de grupo en el proceso de autenticación.

Puede administrar la autenticación en los sistemas operativos Windows agregando cuentas de usuario, equipo y servicio a grupos y, a continuación, aplicando directivas de autenticación a esos grupos. Estas directivas se definen como directivas de seguridad local y como plantillas administrativas, también conocidas como directiva de grupo configuración. Ambos conjuntos se pueden configurar y distribuir en toda la organización mediante el uso de directiva de grupo.

> [!NOTE]
> Las características introducidas en Windows Server 2012 R2 le permiten configurar directivas de autenticación para servicios o aplicaciones dirigidos, normalmente denominados silos de autenticación, mediante el uso de cuentas protegidas. Para obtener información sobre cómo hacer esto en Active Directory, consulte [How to Configure Protected accounts](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md).

Por ejemplo, puede aplicar las directivas siguientes a los grupos, en función de su función en la organización:

-   Iniciar sesión localmente o en un dominio

-   Iniciar sesión a través de una red

-   Restablecer cuentas

-   Creación de cuentas

En la tabla siguiente se enumeran los grupos de directivas relevantes para la autenticación y se proporcionan vínculos a documentación que pueden ayudarle a configurar esas directivas.

|Grupo de directivas|Location|Descripción|
|--------|------|--------|
|**Directiva de contraseñas**|Directiva de \ de configuración del equipo local|Las directivas de contraseñas afectan a las características y el comportamiento de las contraseñas. Las directivas de contraseñas se utilizan para las cuentas de dominio o las cuentas de usuario locales. Determinan la configuración de contraseñas, como la aplicación y la duración.<p>Para obtener información sobre la configuración específica, consulte [Directiva de contraseñas](/windows/security/threat-protection/security-policy-settings/password-policy).|
|**Directiva de bloqueo de cuenta**|Directiva de \ de configuración del equipo local|Las opciones de directiva de bloqueo de cuenta deshabilitan las cuentas después de un número de intentos de inicio de sesión erróneos. El uso de estas opciones puede ayudarle a detectar y bloquear los intentos de interrumpir las contraseñas.<p>Para obtener información acerca de las opciones de directiva de bloqueo de cuenta, consulte [Directiva de bloqueo de cuenta](/windows/security/threat-protection/security-policy-settings/account-lockout-policy).|
|**Directiva Kerberos**|Directiva de \ de configuración del equipo local|La configuración relacionada con Kerberos incluye la duración de los vales y las reglas de cumplimiento. La Directiva Kerberos no se aplica a las bases de datos de cuentas locales porque el protocolo de autenticación Kerberos no se utiliza para autenticar las cuentas locales. Por lo tanto, la configuración de la Directiva de Kerberos solo se puede configurar mediante el objeto de directiva de grupo de dominio (GPO) predeterminado, donde afecta a los inicios de sesión de dominio.<p>Para obtener información sobre las opciones de la Directiva de Kerberos para el controlador de dominio, vea [Directiva de Kerberos](/windows/security/threat-protection/security-policy-settings/kerberos-policy).|
|**Directiva de auditoría**|Directiva de equipo local \ \ configuración de seguridad\Directivas de Locales\directiva|La Directiva de auditoría le permite controlar y comprender el acceso a objetos, como archivos y carpetas, y administrar cuentas de usuario y de grupo, así como inicios de sesión y cierres de sesión de usuario. Las directivas de auditoría pueden especificar las categorías de eventos que desea auditar, establecer el tamaño y el comportamiento del registro de seguridad y determinar los objetos que desea que supervisen el acceso y el tipo de acceso que desea supervisar.<p>|
|**Asignación de derechos de usuario**|Equipo local \ \ configuración de seguridad\Directivas \ asignación de derechos|Los derechos de usuario se asignan normalmente en función de los grupos de seguridad a los que pertenece un usuario, como administradores, usuarios avanzados o usuarios. La configuración de directiva de esta categoría se usa normalmente para conceder o denegar el permiso para obtener acceso a un equipo en función del método de acceso y de la pertenencia a grupos de seguridad.|
|**Opciones de seguridad**|Equipo local \ \ \ configuración de seguridad\Directivas Locales\opciones de opciones|Las directivas relacionadas con la autenticación incluyen:<p>-Dispositivos<br />-Controlador de dominio<br />-Miembro del dominio<br />-Inicio de sesión interactivo<br />-Servidor de red de Microsoft<br />-Acceso de red<br />-Seguridad de red<br />-Consola de recuperación<br />-Shutdown<p>|
|**Delegación de credenciales**|Equipo Equipo\plantillas Templates\System\Credentials delegación|La delegación de credenciales es un mecanismo que permite usar las credenciales locales en otros sistemas, especialmente los servidores miembro y los controladores de dominio dentro de un dominio. Esta configuración se aplica a las aplicaciones mediante el proveedor de compatibilidad para seguridad de credenciales (CRED SSP). Conexión a Escritorio remoto es un ejemplo.|
|**KDC**|Equipo\plantillas Templates\System\KDC de equipo|Esta configuración de directiva afecta al modo en que el Centro de distribución de claves (KDC), que es un servicio en el controlador de dominio, controla las solicitudes de autenticación Kerberos.|
|**Kerberos**|Equipo\plantillas Templates\System\Kerberos de equipo|Esta configuración de directiva afecta a cómo se configura Kerberos para controlar la compatibilidad con notificaciones, protección de Kerberos, autenticación compuesta, identificación de servidores proxy y otras configuraciones.|
|**Iniciar sesión**|Equipo\plantillas Templates\System\Logon de equipo|Esta configuración de directiva controla el modo en que el sistema presenta la experiencia de inicio de sesión para los usuarios.|
|**Net Logon**|Equipo Equipo\plantillas Templates\System\Net inicio de sesión|Esta configuración de directiva controla el modo en que el sistema controla las solicitudes de inicio de sesión de red, incluido el comportamiento del localizador de controladores de dominio.<p>Para obtener más información sobre cómo encaja el localizador del controlador de dominio en los procesos de replicación, consulte Descripción de la [replicación entre sitios](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771251(v=ws.11)).|
|**Biometría**|Equipo\plantillas Administrativas\componentes de Components\Biometrics de equipo|Normalmente, esta configuración de directiva permite o deniega el uso de biometría como método de autenticación.<p>Para obtener información sobre la implementación de biométrica de Windows, consulte Plataforma de biometría de Windows Overview.|
|**Interfaz de usuario de credenciales**|Equipo\plantillas Administrativas\componentes de la interfaz de usuario de Components\Credential|Esta configuración de directiva controla cómo se administran las credenciales en el punto de entrada.|
|**Sincronización de contraseñas**|Equipo\plantillas Administrativas\componentes de Components\Password sincronización de equipos|Esta configuración de directiva determina el modo en que el sistema administra la sincronización de contraseñas entre los sistemas operativos Windows y UNIX.<p>Para obtener más información, consulte [sincronización de contraseñas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732609(v=ws.11)).|
|**Tarjeta inteligente**|Equipo\plantillas Administrativas\componentes de la tarjeta Components\Smart|Esta configuración de directiva controla el modo en que el sistema administra los inicios de sesión de tarjeta inteligente.<p>|
|**Opciones de inicio de sesión de Windows**|Opciones del Equipo\plantillas Administrativas\componentes de Windows\windows Logon Options|Estas configuraciones de directiva controlan Cuándo y cómo se encuentran disponibles las oportunidades de inicio de sesión.|
|**Opciones de Ctrl + Alt + Supr**|Opciones del Equipo\plantillas Administrativas\componentes de Components\Ctrl + Alt + Supr|Esta configuración de directiva afecta a la apariencia y accesibilidad a las características de la interfaz de usuario de inicio de sesión (escritorio seguro), como el administrador de tareas y el bloqueo del teclado del equipo.|
|**Iniciar sesión**|Equipo\plantillas Administrativas\componentes de Components\Logon de equipo|Esta configuración de directiva determina si los procesos pueden ejecutarse cuando el usuario inicia sesión.|

## <a name="additional-references"></a>Referencias adicionales
[Información técnica sobre autenticación de Windows](windows-authentication-technical-overview.md)