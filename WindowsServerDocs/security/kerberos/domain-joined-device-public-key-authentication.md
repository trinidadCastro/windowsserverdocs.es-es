---
title: "Autenticación de clave pública de dispositivo unido al dominio"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 8/18/2017
ms.openlocfilehash: 65f39f4900fcd99ef90b160d3db7099edb73bc1c
ms.sourcegitcommit: e94838df702ff0135ebe7c179b228aa67b772d35
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2017
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticación de clave pública de dispositivo unido al dominio

>Se aplica a: Windows Server 2016, Windows 10

Kerberos agregado compatibilidad para dispositivos Unidos al dominio iniciar sesión con Windows Server 2012 y Windows 8 en un principio de certificado. Este cambio permite a los proveedores de terceros 3 crear soluciones para aprovisionar e inicializar los certificados para dispositivos Unidos a un dominio que se usará para la autenticación de dominio. 

## <a name="automatic-public-key-provisioning"></a>Automático aprovisionamiento de clave pública

A partir de Windows 10 versión 1507 y Windows Server 2016, los dispositivos Unidos a un dominio aprovisionen automáticamente una clave pública enlazada a un controlador de dominio de Windows Server 2016 (corriente continua). Una vez que se aprovisione una clave, Windows puede usar la autenticación de clave pública para el dominio.

### <a name="public-key-generation"></a>Generación de claves públicas
Si el dispositivo está ejecutando a Credential Guard, se crea una clave pública protegida con Credential Guard. 

Si Credential Guard no está disponible y un TPM, se crea una clave pública está protegida por el TPM. 

Si no está disponible, no se genera una clave y el dispositivo solo puede autenticarse con contraseña.

### <a name="provisioning-computer-account-public-key"></a>Aprovisionamiento de clave pública de la cuenta de equipo
Cuando se inicia Windows, comprueba si se aprovisiona una clave pública para su cuenta de equipo. De lo contrario, a continuación, genera una clave pública enlazada y configura para su cuenta en AD con un Windows Server 2016 o DC superior. Si los controladores de dominio son bajo nivel, no se aprovisiona ninguna clave.

### <a name="configuring-device-to-only-use-public-key"></a>Configurar el dispositivo solo usar la clave pública
Si la configuración de directiva de grupo **soporte técnico para la autenticación de dispositivo con certificado** se establece en **fuerza**, a continuación, el dispositivo debe encontrar un controlador de dominio que se ejecuta Windows Server 2016 o posterior para autenticar. El valor es en plantillas administrativas > sistema > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configurar dispositivo usar solo la contraseña
Si la configuración de directiva de grupo **soporte técnico para la autenticación de dispositivo con certificado** está deshabilitada, y siempre se usa la contraseña. El valor es en plantillas administrativas > sistema > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticación de dispositivo unido al dominio mediante la clave pública
Cuando Windows tiene un certificado para el dispositivo unido al dominio, Kerberos primero autentica con el certificado y en los intentos de error con contraseña. Esto permite que el dispositivo autenticar a los controladores de dominio de nivel inferior.

Dado que las claves públicas automáticamente aprovisionadas tienen un certificado autofirmado, se produce un error en la validación de certificados en los controladores de dominio que no son compatibles con la clave de confianza de asignación de cuenta. De manera predeterminada, Windows vuelve a intentar la autenticación mediante la contraseña del dominio del dispositivo.


