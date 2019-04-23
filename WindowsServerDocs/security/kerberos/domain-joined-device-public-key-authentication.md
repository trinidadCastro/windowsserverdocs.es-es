---
title: Autenticación de clave pública de dispositivo unido al dominio
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 80906e7cfe3740200938704a4b4eaf0759af303a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885036"
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticación de clave pública de dispositivo unido al dominio

>Se aplica a: Windows Server 2016, Windows 10

Kerberos agregó compatibilidad para dispositivos Unidos a dominio iniciar sesión con un certificado que comience con Windows Server 2012 y Windows 8. Este cambio permite 3rd proveedores crear soluciones para aprovisionar e inicializar los certificados para dispositivos Unidos a un dominio que se usará para la autenticación de dominio. 

## <a name="automatic-public-key-provisioning"></a>Automática aprovisionamiento de claves públicas

A partir de Windows 10 versión 1507 y Windows Server 2016, los dispositivos Unidos a dominio aprovisionan automáticamente una clave pública enlazada a un controlador de dominio (DC) de Windows Server 2016. Cuando se aprovisiona una clave, Windows pueden usar la autenticación de clave pública para el dominio.

### <a name="public-key-generation"></a>Generación de claves públicas
Si el dispositivo se está ejecutando a Credential Guard, se crea una clave pública protegida por Credential Guard. 

Si no está disponible de Credential Guard y es un TPM, se crea una clave pública protegida por el TPM. 

Si ninguno está disponible, no se genera una clave y el dispositivo solo puede autenticarse mediante contraseña.

### <a name="provisioning-computer-account-public-key"></a>Aprovisionamiento de la clave pública de la cuenta de equipo
Cuando Windows se inicia, comprueba si se aprovisiona una clave pública para su cuenta de equipo. Si no lo es, a continuación, genera una clave pública enlazada y lo configura para su cuenta de AD mediante un Windows Server 2016 o posterior DC. Si todos los controladores de dominio son inferiores, no se aprovisiona ninguna clave.

### <a name="configuring-device-to-only-use-public-key"></a>Usar sólo la clave pública al configurar el dispositivo
Si la configuración de directiva de grupo **compatibilidad con la autenticación de dispositivos con certificado** está establecido en **Force**, a continuación, el dispositivo debe encontrar un controlador de dominio que ejecuta Windows Server 2016 o posterior para autenticar. El valor es en plantillas administrativas > sistema > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configuración de dispositivo solo usar contraseña
Si la configuración de directiva de grupo **compatibilidad con la autenticación de dispositivos con certificado** está deshabilitado, siempre se utiliza la contraseña. El valor es en plantillas administrativas > sistema > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticación de dispositivos Unidos a un dominio mediante clave pública
Cuando Windows tiene un certificado para el dispositivo unido al dominio, Kerberos primero se autentica mediante el certificado y de reintentos de error con la contraseña. Esto permite que el dispositivo para autenticarse en los controladores de dominio de nivel inferior.

Dado que las claves públicas aprovisionadas automáticamente tienen un certificado autofirmado, se produce un error en la validación de certificados en los controladores de dominio que no admiten la asignación de cuenta de claves de confianza. De forma predeterminada, Windows vuelve a intentar la autenticación mediante contraseña de dominio del dispositivo.


