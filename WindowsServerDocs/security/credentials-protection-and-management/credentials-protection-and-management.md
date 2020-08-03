---
title: Protección y administración de credenciales
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-credential-protection
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 649c070fe477a51ca764bd1ad83ed013feb1b60b
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518021"
---
# <a name="credentials-protection-and-management"></a>Protección y administración de credenciales

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se describen las características y los métodos introducidos en Windows Server 2012 R2 y Windows 8.1 para la protección de credenciales y los controles de autenticación de dominios para reducir el robo de credenciales.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Modo de administración restringida para la conexión a Escritorio remoto
El modo de administración restringida proporciona un método para iniciar sesión de forma interactiva en un servidor host remoto sin transmitir las credenciales al servidor. Esto evita que las credenciales se recopilen durante el proceso de conexión inicial si el servidor está en peligro.

Al utilizar este modo con credenciales de administrador, el cliente de Escritorio remoto intenta iniciar sesión de forma interactiva en un host que también es compatible con este modo sin necesidad de enviar las credenciales. Cuando el host comprueba que la cuenta de usuario que se está conectando tiene derechos de administrador y es compatible con el modo de de administración restringida, la conexión se realiza correctamente. De lo contrario, se produce un error en el intento de conexión. El modo de administración restringida no envía en ningún momento texto sin formato u otras formas reutilizables de credenciales a los equipos remotos.

### <a name="lsa-protection"></a>Protección LSA
La Autoridad de seguridad local (LSA), que reside en el proceso del Servicio de seguridad de la Autoridad de seguridad local (LSASS), valida a los usuarios para los inicios de sesión locales y remotos y exige la aplicación de directivas de seguridad local. El sistema operativo Windows 8.1 proporciona protección adicional para LSA a fin de evitar la inserción de código por parte de procesos no protegidos. Esto proporciona seguridad adicional para las credenciales que LSA almacena y administra. Esta configuración del proceso protegido para LSA puede configurarse en Windows 8.1 pero está activada de forma predeterminada en Windows RT 8,1 y no se puede cambiar.

Para obtener información acerca de cómo configurar el la protección LSA, vea [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Grupo de seguridad Usuarios protegidos
Este nuevo grupo global de dominio desencadena una nueva protección no configurable en dispositivos y equipos host que ejecutan Windows Server 2012 R2 y Windows 8.1. El grupo usuarios protegidos habilita otras protecciones para los controladores de dominio y los dominios en los dominios de Windows Server 2012 R2. Esto reduce considerablemente los tipos de credenciales disponibles cuando los usuarios han iniciado sesión en equipos de la red desde un equipo que no está en peligro.

Los miembros del grupo Usuarios protegidos están también limitados por los siguientes métodos de autenticación:

-   Un miembro del grupo Usuarios protegidos solo puede iniciar sesión a través del protocolo Kerberos. La cuenta no puede autenticarse con NTLM, Digest Authentication o CredSSP. En un dispositivo que ejecuta Windows 8.1, las contraseñas no se almacenan en caché, por lo que el dispositivo que usa cualquiera de estos proveedores de compatibilidad para seguridad (SSP) no podrá autenticarse en un dominio cuando la cuenta sea miembro del grupo de usuarios protegidos.

-   El protocolo Kerberos no usará los tipos de cifrado DES o RC4 más débiles en el proceso de autenticación previa. Esto significa que el dominio debe estar configurado para ser compatible como mínimo con el conjunto de cifrado AES.

-   No se puede delegar la cuenta del usuario con una delegación restringida o no restringida de Kerberos. esto quiere decir que es posible que las conexiones anteriores a otros sistemas no puedan realizarse si el usuario es miembro del grupo de usuarios protegidos.

-   El ajuste predeterminado de cuatro horas para la duración de un vale de concesión de vales (TGT) de Kerberos puede configurarse mediante silos y directivas de autenticación, a los que se accede a través del Centro de administración de Active Directory (ADAC). esto quiere decir que, una vez transcurridas esas cuatro horas, el usuario debe volver a autenticarse.

> [!WARNING]
> Las cuentas de servicio y de equipo no deben ser miembros del grupo de usuarios protegidos. Este grupo no ofrece protección local, ya que la contraseña o certificado siempre está disponible en el host. Se producirá un error de autenticación con el mensaje "el nombre de usuario o la contraseña son incorrectos" para cualquier servicio o equipo que se agregue al grupo usuarios protegidos.

Para obtener más información acerca de este grupo, consulta [Grupo de seguridad Usuarios protegidos](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Directiva de autenticación y silos de directivas de autenticación
Las directivas de Active Directory basadas en bosques se introducen y se pueden aplicar a las cuentas de un dominio con un nivel funcional de dominio de Windows Server 2012 R2. Estas directivas de autenticación pueden controlar qué hosts puede usar un usuario para iniciar sesión. Funcionan conjuntamente con el grupo de seguridad Usuarios protegidos y los administradores pueden aplicar condiciones para el control del acceso para la autenticación de las cuentas. Estas políticas de autenticación aíslan las cuentas asociadas para restringir el ámbito de una red.

La nueva clase de objeto de Active Directory, la Directiva de autenticación, permite aplicar la configuración de autenticación a clases de cuentas en dominios con un nivel funcional de dominio de Windows Server 2012 R2. Las políticas de autenticación se aplican durante el intercambio AS o TGS de Kerberos. Las clases de cuentas de Active Directory son las siguientes:

-   Usuario

-   Computer

-   Cuenta de servicio administrada

-   Cuenta de servicio administrada de grupo

Para obtener más información, consulta [Directivas de autenticación y silos de directivas de autenticación](authentication-policies-and-authentication-policy-silos.md).

Para obtener más información sobre cómo configurar cuentas protegidas, consulta [Cómo configurar cuentas protegidas](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/how-to-configure-protected-accounts).

## <a name="additional-references"></a>Referencias adicionales
Para obtener más información sobre LSA y LSASS, consulte [Información técnica de inicio de sesión y autenticación de Windows](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



