---
title: Impedir el cambio de contraseña de Kerberos que usa claves secretas RC4
ms.prod: windows-server
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-credential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 4b335ca4432f17acd60d9246de81081cf0441552
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858828"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Impedir que Kerberos cambie la contraseña que usa claves secretas RC4

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2008 R2 y Windows Server 2008

En este tema para profesionales de TI se explican algunas limitaciones del protocolo Kerberos que podrían conducir a que un usuario malintencionado tome el control de la cuenta de un usuario. Existe una limitación en el estándar del servicio de autenticación de red (V5) de Kerberos (RFC 4120), que es muy conocido dentro del sector, por el que un atacante puede autenticarse como un usuario o cambiar la contraseña de ese usuario si el atacante conoce la clave secreta del usuario.

La posesión de las claves secretas de Kerberos derivadas de la contraseña de un usuario (RC4 y Estándar de cifrado avanzado [AES] de forma predeterminada) se valida durante el intercambio de cambio de contraseña de Kerberos por RFC 4757. La contraseña de texto no cifrado del usuario nunca se proporciona al Centro de distribución de claves (KDC) y, de forma predeterminada, Active Directory controladores de dominio no poseen una copia de las contraseñas de texto sin formato para las cuentas. Si el controlador de dominio no admite un tipo de cifrado Kerberos, esa clave secreta no se puede usar para cambiar la contraseña. 

En los sistemas operativos de Windows designados en la lista se aplica a que se encuentra al principio de este tema, hay tres maneras de bloquear la capacidad de cambiar las contraseñas mediante Kerberos con claves secretas RC4:

- Configure la cuenta de usuario para que incluya la opción de cuenta la tarjeta inteligente es necesaria para el inicio de sesión interactivo. Esto limita al usuario a iniciar sesión solo con una tarjeta inteligente válida para que se rechacen las solicitudes de servicio de autenticación RC4 (AS-req). Para establecer las opciones de cuenta en una cuenta, haga clic con el botón secundario en la cuenta, haga clic en propiedades y, a continuación, haga clic en la pestaña cuenta. 

- Deshabilite la compatibilidad con RC4 para Kerberos en todos los controladores de dominio. Esto requiere un nivel funcional de dominio de Windows Server 2008 como mínimo y un entorno en el que todos los clientes Kerberos, los servidores de aplicaciones y las relaciones de confianza entre el dominio deben admitir AES. La compatibilidad con AES se presentó en Windows Server 2008 y Windows Vista.

    [!NOTE]
    Hay un problema conocido con la deshabilitación de RC4 que puede hacer que el sistema se reinicie. Consulte las siguientes revisiones:
    - [Windows Server 2012 R2](https://support.microsoft.com/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/kb/3086213)
    - No hay ninguna revisión disponible para versiones anteriores de Windows Server

- Implementar dominios establecidos en el nivel funcional de dominio de Windows Server 2012 R2 o superior, y configurar los usuarios como miembros del grupo de seguridad usuarios protegidos. Dado que esta característica interrumpe más que el uso de RC4 en el protocolo Kerberos, consulte los recursos que [se indican en](#see-also) la siguiente sección.

## <a name="see-also"></a>Consulta también

- Para obtener información acerca de cómo evitar el uso del tipo de cifrado RC4 en los dominios de Windows Server 2012 R2, consulte [grupo de seguridad de usuarios protegidos](/../credentials-protection-and-management/protected-users-security-group.md)y [configuración de cuentas protegidas](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Para obtener explicaciones sobre RFC 4120 y RFC 4757, consulte [documentos de IETF](http://tools.ietf.org/html/).
