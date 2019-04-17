---
title: "Configuración de directiva de grupo que usa autenticación de Windows"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Configuración de directiva de grupo que usa autenticación de Windows

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de referencia para profesionales de TI describe el uso y el impacto de las configuraciones de directiva de grupo en el proceso de autenticación.

Puedes administrar la autenticación en los sistemas operativos Windows al agregar cuentas de usuario, equipo y servicio a grupos y, a continuación, aplicando las directivas de autenticación a los grupos. Estas directivas se definen como directivas de seguridad local, como plantillas administrativas, también conocida como la configuración de directiva de grupo. Ambos conjuntos pueden configurados y distribuirse en toda la organización mediante la directiva de grupo.

> [!NOTE]
> Características, introducidas en Windows Server 2012 R2, te permiten configurar las directivas de autenticación para los servicios de destino o protegido de aplicaciones, comúnmente denominados silos de autenticación, mediante el uso de cuentas. Para obtener información sobre cómo hacer esto en Active Directory, consulte [cómo configurar cuentas protegido](how-to-configure-protected-accounts.md).

Por ejemplo, puedes aplicar las siguientes directivas a grupos, en función de su función en la organización:

-   Iniciar sesión localmente o a un dominio

-   Iniciar sesión a través de una red

-   Restablecer cuentas

-   Crear cuentas

La siguiente tabla enumera pertinentes para la autenticación de grupos de directivas y proporciona vínculos a documentación que te ayudarán a configurar las directivas.

|Grupo de directivas|Ubicación|Descripción|
|--------|------|--------|
|**Directiva de contraseñas**|Equipo local local\Configuración equipo\Configuración Windows\Configuración de Seguridad\directivas|Las directivas de contraseña afectan a las características y comportamiento de las contraseñas. Las directivas de contraseña se usan para las cuentas de dominio o cuentas de usuario locales. Determinan la configuración de contraseñas, como la aplicación y la duración.<br /><br />Para obtener información sobre la configuración específica, consulta [directiva de contraseñas](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Directiva de bloqueo de cuenta**|Equipo local local\Configuración equipo\Configuración Windows\Configuración de Seguridad\directivas|Opciones de directiva de bloqueo de cuenta deshabilitan cuentas tras un número de intentos no válidos. Uso de estas opciones puede ayudarte a detectar y bloquear los intentos para interrumpir las contraseñas.<br /><br />Para obtener información sobre las opciones de directiva de bloqueo de cuenta, consulta [directiva de bloqueo de cuenta](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Directiva Kerberos**|Equipo local local\Configuración equipo\Configuración Windows\Configuración de Seguridad\directivas|Configuración relacionada con Kerberos incluir reglas de ciclo de vida y el cumplimiento de vale. Directiva Kerberos no se aplica a las bases de datos de la cuenta local porque no se usa el protocolo de autenticación Kerberos para autenticar las cuentas locales. Por lo tanto, se puede configurar la configuración de directiva Kerberos solo mediante el objeto de directiva de grupo (GPO), que afecta a los inicios de sesión de dominio del dominio predeterminado.<br /><br />Para obtener información sobre las opciones de directiva Kerberos para el controlador de dominio, consulta [directiva Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Directiva de auditoría**|Equipo local local\Configuración equipo\Configuración Windows\Configuración de seguridad\Directivas Locales\directiva|Directiva de auditoría te permite controlar y comprender el acceso a objetos, como archivos y carpetas y para administrar cuentas de usuario y grupo y los inicios de sesión de usuario y cierre de sesión. Las directivas de auditoría pueden especificar las categorías de eventos que quieres auditar, establecer el tamaño y el comportamiento del registro de seguridad y determinar de qué objetos desea supervisar el acceso y el tipo de acceso que desea supervisar.<br /><br />|
|**Asignación de derechos de usuario**|Equipo local local\Configuración equipo\Configuración Windows\Configuración seguridad\Directivas Locales\asignación de derechos|Por lo general se asignan derechos de usuario en función de los grupos de seguridad al que pertenece un usuario, como los administradores, usuarios avanzados o los usuarios. La configuración de directiva de esta categoría se suelen usar para conceder o denegar el permiso para acceder a un equipo basado en el método de pertenencia a grupos de seguridad y de acceso.|
|**Opciones de seguridad**|Equipo local local\Configuración equipo\Configuración Windows\Configuración de seguridad\Directivas Locales\opciones|Las directivas pertinentes para la autenticación incluyen:<br /><br />-Los dispositivos<br />: Controlador de dominio<br />: Miembro de dominio<br />-Inicio de sesión interactivo<br />: Servidor de red Microsoft<br />: Acceso a redes<br />: Seguridad de red<br />: Consola de recuperación<br />: Apagado<br /><br />|
|**Delegación de credenciales**|Equipo equipo\Plantillas Templates\System\Credentials delegación|La delegación de credenciales es un mecanismo que permite credenciales locales se pueden usar en otros sistemas, lo más destacable es servidores miembros y controladores de dominio dentro de un dominio. Estas opciones de configuración se aplican a las aplicaciones mediante el proveedor de soporte técnico de seguridad de credenciales (credenciales SSP). Conexión a Escritorio remoto es un ejemplo.|
|**KDC**|Equipo equipo\Plantillas Templates\System\KDC|Esta configuración de directiva afecta al modo en que el centro de distribución de claves (KDC), que es un servicio en el controlador de dominio, controla las solicitudes de autenticación de Kerberos.|
|**Kerberos**|Equipo equipo\Plantillas Templates\System\Kerberos|Esta configuración de directiva afecta a la configuración de Kerberos para controlar el soporte técnico para reclamaciones, protección de Kerberos, autenticación compuesta, identificación servidores proxy y otras configuraciones.|
|**Inicio de sesión**|Nota de configuración del equipo|Esta configuración de directiva controla la manera en que el sistema presenta la experiencia de inicio de sesión de usuarios.|
|**Inicio de sesión**|Inicio de sesión de equipo equipo\Plantillas Templates\System\Net|Esta configuración de directiva controla cómo el sistema controla las solicitudes de inicio de sesión de red incluidas cómo se comporta el localizador de controlador de dominio.<br /><br />Para obtener más información sobre cómo se ajusta el localizador de controlador de dominio en los procesos de replicación, consulta [descripción de replicación entre sitios](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biometría**|Equipo Configuración del equipo\Plantillas administrativas\Componentes Components\Biometrics|Por lo general, esta configuración de directiva permite o denegar el uso de la biométrica como método de autenticación.<br /><br />Para obtener información sobre la implementación de Windows de la biométrica, consulta la introducción de marco biométrico de Windows.|
|**Interfaz de usuario de credenciales**|Equipo Configuración del equipo\Plantillas administrativas\Componentes Windows\interfaz del usuario|Esta configuración de directiva controla cómo se administran las credenciales en el punto de entrada.|
|**Sincronización de contraseñas**|Equipo Configuración del equipo\Plantillas administrativas\Componentes Components\Password sincronización|Esta configuración de directiva determina cómo el sistema administra la sincronización de contraseñas entre Windows y sistemas operativos basados en UNIX.<br /><br />Para obtener más información, consulta [la sincronización de contraseñas](https://technet.microsoft.com/library/cc732609.aspx).|
|**Tarjeta inteligente**|Equipo Configuración del equipo\Plantillas administrativas\Componentes Components\Smart tarjeta|Esta configuración de directiva controla el modo en que el sistema administra los inicios de sesión de tarjeta inteligente.<br /><br />|
|**Opciones de inicio de sesión de Windows**|Opciones de inicio de sesión de equipo Configuración del equipo\Plantillas administrativas\Componentes ofrecidas|Esta configuración de directiva controla cuándo y cómo existen oportunidades de inicio de sesión.|
|**Opciones de Ctrl + Alt + Supr**|Opciones de equipo Configuración del equipo\Plantillas administrativas\Componentes Components\Ctrl + Alt + Supr|Esta configuración de directiva afecta a la apariencia de y accesibilidad a características en el inicio de sesión (escritorio seguro) de la interfaz de usuario, como el Administrador de tareas y el bloqueo de teclado del equipo.|
|**Inicio de sesión**|Equipo Configuración del equipo\Plantillas administrativas\Componentes Components\Logon|Esta configuración de directiva determina si o los procesos que se pueden ejecutar cuando el usuario inicie sesión.|

## <a name="see-also"></a>Consulta también
[Introducción técnica de autenticación de Windows](windows-authentication-technical-overview.md)


