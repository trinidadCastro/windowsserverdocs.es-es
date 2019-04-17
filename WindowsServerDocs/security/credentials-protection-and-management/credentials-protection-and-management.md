---
title: "Administración y protección de credenciales"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 16cc1f2260bf0552da6902dc3e97de65d29c7931
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-protection-and-management"></a>Administración y protección de credenciales

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI se describe características y los métodos que se habilitó en Windows Server 2012 R2 y Windows 8.1 para la protección de credenciales y los controles de autenticación de dominio para reducir el robo de credenciales.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Modo de administrador restringido para la conexión a Escritorio remoto
Modo de administrador restringido, proporciona un método de inicio de sesión interactivamente a un servidor de host remoto sin transmitir sus credenciales en el servidor. Esto impide que las credenciales se cosechado durante el proceso de conexión inicial si el servidor ha habido algún riesgo.

Con credenciales de administrador en este modo, intenta forma interactiva de inicio de sesión a un host que también lo admitan este modo sin enviar las credenciales de cliente de escritorio remoto. Cuando el host comprueba que la cuenta de usuario conectar a ella tiene derechos de administrador y admite el modo de administrador restringido, la conexión se realiza correctamente. De lo contrario, se produce un error en el intento de conexión. Modo de administrador restringido hace no en cualquier texto o enviar punto u otras formas de volver a utilizar de credenciales con equipos remotos.

### <a name="lsa-protection"></a>Protección de LSA
La autoridad de seguridad Local (LSA), que se encuentra dentro del proceso de servicio de seguridad de autoridad de seguridad Local (LSASS), valida los usuarios para inicios de sesión locales y remotos y aplica directivas de seguridad local. El sistema operativo de Windows 8.1 proporciona protección adicional para la LSA evitar la inserción de código procesos no protegidas. Esto proporciona mayor seguridad de las credenciales que la LSA almacena y administra. Esta configuración de proceso protegido de LSA se pueden configurar en Windows 8.1, pero está activada de manera predeterminada en Windows RT 8.1 y no se puede cambiar.

Para obtener información sobre la configuración de protección de LSA, consulte [configurar la protección adicional de LSA](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Grupo de seguridad de los usuarios protegido
Este nuevo grupo global de dominio desencadena nueva protección no se pueden configurar en los dispositivos y equipos host que ejecutan Windows Server 2012 R2 y Windows 8.1. El grupo usuarios protegido permite una protección adicional para controladores de dominio y dominios en los dominios de Windows Server 2012 R2. Esto reduce en gran medida los tipos de credenciales disponibles cuando los usuarios han iniciado sesión en equipos de la red desde un equipo no puesto en peligro.

Los miembros del grupo usuarios protegido están limitados por los siguientes métodos de autenticación:

-   Un miembro del grupo usuarios protegidas solo puede iniciar sesión mediante el protocolo Kerberos. La cuenta no puede autenticarse con NTLM, la autenticación implícita o CredSSP. En un dispositivo que ejecuta Windows 8.1, las contraseñas no se almacenan en caché, por lo que se producirá un error del dispositivo que usa uno de estos proveedores de compatibilidad para seguridad (SSP) autenticar a un dominio, cuando la cuenta es miembro del grupo de usuarios protegido.

-   El protocolo Kerberos no usará los tipos de cifrado DES o RC4 más débiles en el proceso de autenticación previa. Esto significa que el dominio debe estar configurado para admitir al menos el conjunto de cifrado AES.

-   No se puede delegar la cuenta del usuario con Kerberos limitada o no tienen ninguna restricción delegación. Esto significa que anterior conexiones a otros sistemas pueden fallar si el usuario es un miembro del grupo usuarios protegido.

-   El valor del ciclo de vida de Kerberos vale de concesión de vales (TGT) predeterminado de cuatro horas es configurable con las directivas de autenticación y Silos accede a través del centro de administrativas de directorio (ADAC) activo. Esto significa que cuando se pasan los cuatro horas, el usuario debe volver a autenticar.

> [!WARNING]
> Cuentas de servicios y equipos no deben ser miembros del grupo usuarios protegido. Este grupo no proporciona local protección porque la contraseña o certificado siempre está disponible en el equipo host. Se producirá un error de autenticación con el error "el nombre de usuario o la contraseña es incorrecta" para cualquier servicio o un equipo que se agrega al grupo usuarios protegido.

Para obtener más información acerca de este grupo, consulta [protegido el grupo de seguridad de los usuarios](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Directiva de autenticación y Silos de directiva de autenticación
Se introducen las directivas basada en el bosque de Active Directory, y pueden aplicarse a cuentas en un dominio con un nivel funcional de dominio de Windows Server 2012 R2. Estas directivas de autenticación pueden controlar los hosts que un usuario puede usar para iniciar sesión. Funcionan junto con el grupo de seguridad de los usuarios proteger y administradores pueden aplicar las condiciones de control de acceso para la autenticación a las cuentas. Estas directivas de autenticación aislar cuentas relacionadas para limitar el ámbito de una red.

La nueva clase de objeto de Active Directory, directiva de autenticación, te permite aplicar la configuración de autenticación para las clases de la cuenta en dominios con un nivel funcional de dominio de Windows Server 2012 R2. Directivas de autenticación se aplican durante la AS de Kerberos o de TGS exchange. Clases de cuenta de Active Directory son:

-   Usuario

-   Equipo

-   Cuenta de servicio administrada

-   Cuenta de grupo de servicio administrada

Para obtener más información, consulta [Silos de directiva de autenticación y directivas de autenticación](authentication-policies-and-authentication-policy-silos.md).

Para obtener más información, cómo configurar protegido cuentas, consulta [cómo configurar cuentas protegido](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Consulta también
Para obtener más información sobre la LSA y el LSASS, consulta el [inicio de sesión de Windows y la información general técnica de autenticación](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



