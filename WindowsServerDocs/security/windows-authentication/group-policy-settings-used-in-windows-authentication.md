---
title: Configuración de directiva de grupo usada en la autenticación de Windows
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 38df2033b57c0394b96f539f54efe6a3579500f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847936"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Configuración de directiva de grupo usada en la autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema de referencia para profesionales de TI describe el uso y el impacto de las configuraciones de directiva de grupo en el proceso de autenticación.

Puede administrar la autenticación en los sistemas operativos de Windows mediante la adición de usuario, equipo y las cuentas de servicio a grupos y, a continuación, aplicando las directivas de autenticación a esos grupos. Estas directivas se definen como directivas de seguridad local, como plantillas administrativas, también conocido como configuración de directiva de grupo. Ambos conjuntos se pueden configurar y distribuir en toda la organización mediante Directiva de grupo.

> [!NOTE]
> Las características introducidas en Windows Server 2012 R2, le permiten configurar directivas de autenticación para servicios de destino o aplicaciones, comúnmente denominado silos de autenticación mediante cuentas protegidas. Para obtener información acerca de cómo hacer esto en Active Directory, vea [How to Configure Protected Accounts](how-to-configure-protected-accounts.md).

Por ejemplo, puede aplicar las siguientes directivas a grupos, según su función en la organización:

-   Iniciar sesión localmente o a un dominio

-   Iniciar sesión a través de una red

-   Restablecimiento de cuentas

-   Crear cuentas

En la tabla siguiente se enumera los grupos de directivas pertinentes para la autenticación y proporciona vínculos a documentación que puede ayudarle a configuración esas directivas.

|Grupo de directivas|Location|Descripción|
|--------|------|--------|
|**Directiva de contraseñas**|Equipo local local\Configuración equipo\Configuración de Windows\Configuración Seguridad\directivas|Directivas de contraseña afectan a las características y comportamiento de las contraseñas. Las directivas de contraseñas se usan para las cuentas de dominio o cuentas de usuario local. Determinan la configuración de contraseñas, como el cumplimiento y la duración.<br /><br />Para obtener información sobre la configuración específica, consulte [directiva de contraseñas](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Directiva de bloqueo de cuenta**|Equipo local local\Configuración equipo\Configuración de Windows\Configuración Seguridad\directivas|Las opciones de directiva de bloqueo de cuenta deshabilitan cuentas después de un número determinado de intentos de inicio de sesión. Uso de estas opciones puede ayudarle a detectar y bloquear los intentos para interrumpir las contraseñas.<br /><br />Para obtener información acerca de las opciones de directiva de bloqueo de cuenta, consulte [directiva de bloqueo de cuenta](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Directiva Kerberos**|Equipo local local\Configuración equipo\Configuración de Windows\Configuración Seguridad\directivas|Configuración relacionada con Kerberos incluye reglas de duración y el cumplimiento de vale. Directiva Kerberos no se aplica a las bases de datos de la cuenta local porque no se usa el protocolo de autenticación Kerberos para autenticar cuentas locales. Por lo tanto, la configuración de directiva Kerberos puede configurarse únicamente con el objeto de directiva de grupo (GPO), que afecta a los inicios de sesión de dominio del dominio predeterminado.<br /><br />Para obtener información sobre las opciones de directiva Kerberos para el controlador de dominio, consulte [directiva Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**La directiva de auditoría**|Directiva de equipo local local\Configuración equipo\Configuración Windows\Configuración seguridad\Directivas locales\Directiva de auditoría|La directiva de auditoría le permite controlar y comprender el acceso a objetos, como archivos y carpetas y administrar cuentas de usuario y grupo y los inicios de sesión de usuario y cierres de sesión. Las directivas de auditoría pueden especificar las categorías de eventos que desee auditar, establecer el tamaño y el comportamiento del registro de seguridad y determinar de qué objetos desea supervisar el acceso y qué tipo de acceso que desea supervisar.<br /><br />|
|**Asignación de derechos de usuario**|Equipo local local\Configuración equipo\Configuración Windows\Configuración configuración de seguridad\Directivas Locales\asignación de derechos|Normalmente se asignan derechos de usuario basándose en los grupos de seguridad al que pertenece un usuario, como administradores, usuarios avanzados o usuarios. La configuración de directiva de esta categoría se usa normalmente para conceder o denegar el permiso para tener acceso a un equipo basado en el método de acceso y seguridad pertenencias a grupos.|
|**Opciones de seguridad**|Equipo local local\Configuración equipo\Configuración Windows\Configuración de seguridad\Directivas Locales\opciones|Las directivas pertinentes para la autenticación se incluyen:<br /><br />-Devices<br />: Controlador de dominio<br />: Miembro de dominio<br />-Inicio de sesión interactivo<br />: Servidor de red Microsoft<br />: Acceso a la red<br />: Seguridad de red<br />: Consola de recuperación<br />-Cierre<br /><br />|
|**Delegación de credenciales**|Equipo Configuración del equipo\Plantillas administrativas\sistema\delegación delegación|La delegación de credenciales es un mecanismo que permite que las credenciales locales se usa en otros sistemas, especialmente los servidores miembro y controladores de dominio dentro de un dominio. Esta configuración se aplica a las aplicaciones utilizando el proveedor de soporte técnico de seguridad de credenciales (Cred SSP). Conexión a Escritorio remoto es un ejemplo.|
|**KDC**|Equipo Configuración del equipo\Plantillas Templates\System\KDC|Esta configuración de directiva afecta a cómo el centro de distribución de claves (KDC), que es un servicio en el controlador de dominio, controla las solicitudes de autenticación de Kerberos.|
|**Kerberos**|Equipo Configuración del equipo\Plantillas Templates\System\Kerberos|Esta configuración de directiva afecta a cómo se configura Kerberos para controlar la compatibilidad con notificaciones, la protección de Kerberos, autenticación compuesta, identificación de los servidores proxy y otras configuraciones.|
|**Inicio de sesión**|Configuración del equipo\Plantillas administrativas\Sistema\Inicio de sesión|Esta configuración de directiva controla cómo el sistema presenta la experiencia de inicio de sesión de usuarios.|
|**Inicio de sesión**|Inicio de sesión de equipo equipo\Plantillas Templates\System\Net|Esta configuración de directiva controla cómo el sistema controla las solicitudes de inicio de sesión de red incluido cómo se comporta el ubicador del controlador de dominio.<br /><br />Para obtener más información acerca de cómo el ubicador del controlador de dominio se adapta a procesos de replicación, vea [descripción de la replicación entre sitios](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biométrica**|Equipo Configuración de usuario\Plantillas administrativas\Componentes Components\Biometrics|Por lo general, esta configuración de directiva permite o prohibir el uso de la biometría como método de autenticación.<br /><br />Para obtener información sobre la implementación de Windows de la biometría, consulte Introducción al marco biométrico de Windows.|
|**Interfaz de usuario de credenciales**|Equipo Configuración de usuario\Plantillas administrativas\Componentes Windows\interfaz del usuario|Esta configuración de directiva controla cómo se administran las credenciales en el punto de entrada.|
|**Sincronización de contraseña**|Equipo Configuración de usuario\Plantillas administrativas\Componentes Components\Password sincronización|Esta configuración de directiva determina cómo el sistema administra la sincronización de contraseñas entre Windows y sistemas operativos basados en UNIX.<br /><br />Para obtener más información, consulte [la sincronización de contraseña](https://technet.microsoft.com/library/cc732609.aspx).|
|**Tarjeta inteligente**|Equipo Configuración de usuario\Plantillas administrativas\Componentes Components\Smart tarjeta|Esta configuración de directiva controla cómo el sistema administra los inicios de sesión de tarjeta inteligente.<br /><br />|
|**Opciones de inicio de sesión de Windows**|Opciones de inicio de sesión de equipo Configuración de usuario\Plantillas administrativas\Componentes Windows\Windows|Esta configuración de directiva controla cuándo y cómo las oportunidades de inicio de sesión están disponibles.|
|**Opciones de Ctrl + Alt + Supr**|Opciones del equipo Configuración de usuario\Plantillas administrativas\Componentes Components\Ctrl + Alt + Supr|Esta configuración de directiva afecta a la apariencia de y la accesibilidad a las características del inicio de sesión (escritorio seguro) de la interfaz de usuario, como el Administrador de tareas y el bloqueo del teclado del equipo.|
|**Inicio de sesión**|Equipo Configuración de usuario\Plantillas administrativas\Componentes Components\Logon|Esta configuración de directiva determina si o los procesos que se pueden ejecutar cuando el usuario inicia sesión.|

## <a name="see-also"></a>Vea también
[Introducción técnica a la autenticación de Windows](windows-authentication-technical-overview.md)


