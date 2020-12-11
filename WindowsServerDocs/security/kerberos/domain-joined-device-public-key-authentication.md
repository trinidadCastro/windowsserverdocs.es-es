---
description: 'Más información sobre: autenticación de clave pública de dispositivo unido a un dominio'
title: Autenticación de clave pública de dispositivo unido al dominio
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
author: justinha
ms.author: Justinha
ms.date: 08/18/2017
ms.openlocfilehash: fbf5db62d9526283a9d52f5edde5d89e7a5c8fd4
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049093"
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticación de clave pública de dispositivo unido al dominio

>Se aplica a: Windows Server 2016, Windows 10

Kerberos agregó compatibilidad para que los dispositivos Unidos a un dominio inicien sesión con un certificado a partir de Windows Server 2012 y Windows 8. Este cambio permite a los proveedores de terceros crear soluciones para aprovisionar e inicializar los certificados de los dispositivos Unidos a un dominio que se usarán para la autenticación de dominio.

## <a name="automatic-public-key-provisioning"></a>Aprovisionamiento automático de claves públicas

A partir de Windows 10, versión 1507 y Windows Server 2016, los dispositivos Unidos a un dominio aprovisionan automáticamente una clave pública enlazada a un controlador de dominio (DC) de Windows Server 2016. Una vez que se aprovisiona una clave, Windows puede usar la autenticación de clave pública en el dominio.

### <a name="key-generation"></a>Generación de claves
Si el dispositivo está ejecutando Credential Guard, se crea un par de claves pública y privada protegido por Credential Guard.

Si Credential Guard no está disponible y un TPM es, se crea un par de claves pública y privada protegido por el TPM.

Si ninguno está disponible, no se genera un par de claves y el dispositivo solo puede autenticarse mediante la contraseña.

### <a name="provisioning-computer-account-public-key"></a>Clave pública de la cuenta de equipo de aprovisionamiento
Cuando se inicia Windows, comprueba si se ha aprovisionado una clave pública para su cuenta de equipo. Si no es así, genera una clave pública enlazada y la configura para su cuenta en AD con un controlador de dominio de Windows Server 2016 o posterior. Si todos los controladores de DC son de nivel inferior, no se aprovisiona ninguna clave.

### <a name="configuring-device-to-only-use-public-key"></a>Configuración del dispositivo para usar solo la clave pública
Si la directiva de grupo configuración **de compatibilidad con la autenticación de dispositivos mediante el certificado** está establecida en **forzar**, el dispositivo debe encontrar un controlador de dominio que ejecute Windows Server 2016 o posterior para autenticarse. La configuración está en Plantillas administrativas sistema de > > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configuración del dispositivo para usar solo la contraseña
Si el valor directiva de grupo la compatibilidad con la **autenticación de dispositivos mediante el certificado** está deshabilitado, la contraseña siempre se usa. La configuración está en Plantillas administrativas sistema de > > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticación de dispositivos Unidos a un dominio mediante la clave pública
Cuando Windows tiene un certificado para el dispositivo unido a un dominio, Kerberos primero se autentica con el certificado y en los reintentos de error con contraseña. Esto permite que el dispositivo se autentique en los controladores de dispositivos de nivel inferior.

Puesto que las claves públicas aprovisionadas automáticamente tienen un certificado autofirmado, se produce un error en la validación de certificados en los controladores de dominio que no admiten la asignación de cuentas de confianza de clave. De forma predeterminada, Windows vuelve a intentar la autenticación con la contraseña de dominio del dispositivo.


