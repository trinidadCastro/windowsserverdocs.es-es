---
title: Kerberos evitar cambiar la contraseña que usan claves secretas de RC4
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816276"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Impedir que Kerberos cambie la contraseña que usa claves secretas RC4

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2008 R2 y Windows Server 2008

En este tema para profesionales de TI se explica algunas limitaciones en el protocolo de Kerberos que podrían dar lugar a un usuario malintencionado tome el control de una cuenta de usuario. Hay una limitación en el estándar de servicio de autenticación de red de Kerberos (V5) (RFC 4120), que es muy conocido en la industria, mediante el cual un atacante puede autenticarse como un usuario o cambiar la contraseña del usuario si el atacante conoce la clave secreta del usuario.

Se valida la posesión de derivada de la contraseña secretas claves de Kerberos un usuario (RC4 y estándar de cifrado avanzado [AES] de forma predeterminada) durante el intercambio de cambio de contraseña de Kerberos por RFC 4757. Contraseña de texto simple del usuario nunca se proporciona en el centro de distribución de claves (KDC) y, de forma predeterminada, los controladores de dominio de Active Directory no poseer una copia de contraseñas de texto simple para las cuentas. Si el controlador de dominio no es compatible con un tipo de cifrado de Kerberos, esa clave secreta no puede usarse para cambiar la contraseña. 

En los sistemas operativos de Windows designados en la lista se aplica al principio de este tema, hay tres maneras de bloquear la capacidad de cambiar las contraseñas mediante el uso de Kerberos con claves secretas de RC4:

- Configurar el usuario de cuenta para incluir la opción de cuenta de tarjeta inteligente es necesaria para el inicio de sesión interactivo. Esto limita el usuario solo inicio de sesión con una tarjeta inteligente válida para que se rechazan las solicitudes de servicio de autenticación de RC4 (AS-req.). Para establecer las opciones de cuenta en una cuenta, haga doble clic en la cuenta, haga clic en propiedades y haga clic en la ficha cuenta. 

- Deshabilitar RC4 la compatibilidad con Kerberos en todos los controladores de dominio. Esto requiere un mínimo de un nivel funcional del dominio de Windows Server 2008 y un entorno donde todos los clientes, servidores de aplicaciones y las relaciones de confianza a y desde el dominio de Kerberos deben admitir AES. Compatibilidad con AES se introdujo en Windows Server 2008 y Windows Vista.

    [!NOTE]
    Hay un problema conocido con deshabilitar RC4, lo que puede provocar el reinicio del sistema. Ver las revisiones siguientes:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Ninguna revisión está disponible en versiones anteriores de Windows Server

- Implementar el conjunto de dominios en el nivel funcional del dominio de Windows Server 2012 R2 o superior y configurar los usuarios como miembros del grupo de seguridad usuarios protegidos. Dado que esta característica interrumpa algo más que el uso de RC4 en el protocolo Kerberos, consulte los recursos en la siguiente [Vea también](#see-also) sección.

## <a name="see-also"></a>Vea también

- Para obtener información acerca de cómo evitar el uso del tipo de cifrado RC4 en dominios de Windows Server 2012 R2, consulte [Protected Users Security Group](/../credentials-protection-and-management/protected-users-security-group.md), y [How to Configure Protected Accounts](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Para obtener una explicación acerca de RFC 4120 y 4757 de RFC, consulte [documentos de IETF](http://tools.ietf.org/html/).
