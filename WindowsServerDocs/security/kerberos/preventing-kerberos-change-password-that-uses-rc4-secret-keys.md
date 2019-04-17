---
title: "Lo cual evitó que Kerberos cambiar la contraseña que usan claves secretas de RC4"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Lo cual evitó que Kerberos cambiar la contraseña que usa el cifrado RC4 claves secretas

>Se aplica a: Windows Server (Channel anual comas), Windows Server 2016, Windows Server 2008 R2 y Windows Server 2008

Este tema para profesionales de TI explica algunas limitaciones en el protocolo Kerberos que podría dar lugar a un usuario malintencionado que tome el control de una cuenta de usuario. Hay un límite en el estándar de servicio de autenticación de red (V5) de Kerberos (RFC 4120), que es conocido dentro de la industria, mediante el cual un atacante puede autenticarse como un usuario o cambiar la contraseña de usuario si el atacante sabe una clave secreta del usuario.

Durante el intercambio de cambio de contraseña de Kerberos por RFC 4757, se valida posesión de derivada de la contraseña claves secretas Kerberos un usuario (RC4 y Advanced Encryption Standard [AES] de manera predeterminada). La contraseña del usuario texto no cifrado nunca se proporciona en el centro de distribución de claves (KDC) y, de manera predeterminada, los controladores de dominio de Active Directory no cuentan con una copia de las contraseñas de texto no cifrado de cuentas. Si el controlador de dominio no admite un tipo de cifrado Kerberos, dicha clave secreta no puede usarse para cambiar la contraseña. 

En los sistemas de operativos de Windows que se indican en la lista se aplica al principio de este tema, hay tres maneras de bloquear la capacidad para cambiar las contraseñas por medio de Kerberos con claves de cifrado RC4 secretas:

- Configurar el usuario cuenta para incluir la opción de cuenta de tarjeta inteligente es necesaria para el inicio de sesión interactivo. Esto limita al usuario a solo inicio de sesión con una tarjeta inteligente válida para que se rechazan las solicitudes de servicio de autenticación (AS-requisitos) de RC4. Para establecer las opciones de la cuenta en una cuenta, haz clic en la cuenta, haga clic en propiedades y haz clic en la pestaña de la cuenta. 

- Deshabilitar la compatibilidad de RC4 para Kerberos en todos los controladores de dominio. Esto requiere un mínimo de un nivel funcional de dominio de Windows Server 2008 y un entorno donde todos los clientes, servidores de aplicaciones y las relaciones de confianza a y desde el dominio de Kerberos deben admitir AES. Soporte técnico para AES se presentó en Windows Server 2008 y Windows Vista.

    [!NOTE]
    Hay un problema conocido con deshabilitar el cifrado RC4, lo que puede hacer que reiniciar el sistema. Consulta las revisiones siguientes:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Ninguna revisión está disponible para versiones anteriores de Windows Server

- Implementar el conjunto de dominios al nivel funcional de dominio de Windows Server 2012 R2 o posterior y configura los usuarios como miembros del grupo de seguridad de los usuarios protegido. Dado que esta característica deteriore algo más que el cifrado RC4 uso en el protocolo Kerberos, consulta los recursos en el siguiente [Consulte también](#see-also) sección.

## <a name="see-also"></a>Consulta también

- Para obtener información sobre cómo evitar el uso del tipo de cifrado de RC4 en dominios de Windows Server 2012 R2, consulta [protegido el grupo de seguridad de los usuarios](/../credentials-protection-and-management/protected-users-security-group.md), y [cómo configurar cuentas protegido](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Para obtener una explicación acerca de RFC 4120 y RFC 4757, consulta [IETF documentos](http://tools.ietf.org/html/).
