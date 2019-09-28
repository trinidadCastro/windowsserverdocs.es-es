---
title: Nombres de dominio kerberos
description: En este tema se proporciona información general sobre el uso de nombres de dominio Kerberos en el procesamiento de solicitudes de conexión del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f9c611b793df36c2e588b2fa099df4e5382194c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405462"
---
# <a name="realm-names"></a>Nombres de dominio kerberos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016


Puede usar este tema para obtener información general sobre el uso de nombres de dominio Kerberos en el procesamiento de solicitudes de conexión del servidor de directivas de redes.

El atributo RADIUS de nombre de usuario es una cadena de caracteres que normalmente contiene una ubicación de cuenta de usuario y un nombre de cuenta de usuario. La ubicación de la cuenta de usuario también se denomina nombre de dominio Kerberos o dominio Kerberos y es sinónimo del concepto de dominio, incluidos los dominios DNS, los dominios de Active Directory® y los dominios de Windows NT 4,0. Por ejemplo, si una cuenta de usuario se encuentra en la base de datos de cuentas de usuario de un dominio denominado example.com, example.com es el nombre de dominio Kerberos.

En otro ejemplo, si el atributo RADIUS de nombre de usuario contiene el nombre de usuario user1@example.com, user1 es el nombre de la cuenta de usuario y example.com es el nombre de dominio Kerberos. Los nombres de dominio Kerberos se pueden presentar en el nombre de usuario como prefijo o como sufijo:

- **Example\user1**. En este ejemplo, el **ejemplo** de nombre de dominio Kerberos es un prefijo; Además, es el nombre de un dominio Active Directory @ no__t-1 Domain Services \(AD DS @ no__t-3.

- <strong>user1@example.com</strong>. En este ejemplo, el nombre de dominio Kerberos **example.com** es un sufijo; y es un nombre de dominio DNS o el nombre de un dominio de AD DS.

Puede usar nombres de dominio Kerberos configurados en directivas de solicitud de conexión mientras diseña e implementa la infraestructura de RADIUS para asegurarse de que las solicitudes de conexión se enruten desde clientes RADIUS, también denominados servidores de acceso a la red, a servidores RADIUS que pueden autentique y autorice la solicitud de conexión.

Cuando NPS se configura como un servidor RADIUS con la Directiva de solicitud de conexión predeterminada, NPS procesa las solicitudes de conexión para el dominio en el que el NPS es miembro y para dominios de confianza.

Para configurar NPS para que actúe como proxy RADIUS y reenvíe las solicitudes de conexión a dominios que no son de confianza, debe crear una nueva Directiva de solicitud de conexión. En la nueva Directiva de solicitud de conexión, debe configurar el atributo de nombre de usuario con el nombre de dominio Kerberos que se incluirá en el atributo de nombre de usuario de las solicitudes de conexión que desee reenviar. También debe configurar la Directiva de solicitud de conexión con un grupo de servidores RADIUS remotos. La Directiva de solicitud de conexión permite a NPS calcular qué solicitudes de conexión se reenvían al grupo de servidores RADIUS remoto en función de la parte del dominio Kerberos del atributo de nombre de usuario.

## <a name="acquiring-the-realm-name"></a>Adquisición del nombre de dominio Kerberos

La parte del nombre de dominio Kerberos del nombre de usuario se proporciona cuando el usuario escribe credenciales basadas en contraseña durante un intento de conexión o cuando se configura un perfil de administrador de conexiones (CM) en el equipo del usuario para proporcionar el nombre de dominio Kerberos automáticamente.

Puede designar que los usuarios de la red proporcionen su nombre de dominio Kerberos al escribir sus credenciales durante los intentos de conexión de red.

Por ejemplo, puede exigir que los usuarios escriban su nombre de usuario, incluido el nombre de la cuenta de usuario y el nombre de dominio Kerberos, en **nombre de usuario** , en el cuadro de diálogo **conectar** al crear una conexión de acceso telefónico o de red privada virtual (VPN).

Además, si crea un paquete de marcado personalizado con el kit de administración de Connection Manager (CMAK), puede ayudar a los usuarios agregando el nombre de dominio Kerberos automáticamente al nombre de la cuenta de usuario en los perfiles de CM instalados en los equipos de los usuarios. Por ejemplo, puede especificar un nombre de dominio Kerberos y una sintaxis de nombre de usuario en el perfil de CM para que el usuario solo tenga que especificar el nombre de la cuenta de usuario al escribir las credenciales. En este caso, el usuario no necesita saber ni recordar el dominio en el que se encuentra su cuenta de usuario.

Durante el proceso de autenticación, después de que los usuarios escriban sus credenciales basadas en contraseña, el nombre de usuario se pasa desde el cliente de acceso al servidor de acceso a la red. El servidor de acceso a la red crea una solicitud de conexión e incluye el nombre de dominio Kerberos dentro del atributo RADIUS de nombre de usuario en el mensaje de solicitud de acceso que se envía al servidor o proxy RADIUS.

Si el servidor RADIUS es un NPS, el mensaje de solicitud de acceso se evalúa con el conjunto de directivas de solicitud de conexión configuradas. Las condiciones de la Directiva de solicitud de conexión pueden incluir la especificación del contenido del atributo de nombre de usuario.

Puede configurar un conjunto de directivas de solicitud de conexión que son específicas del nombre de dominio Kerberos dentro del atributo de nombre de usuario de los mensajes entrantes. Esto permite crear reglas de enrutamiento que reenvían mensajes RADIUS con un nombre de dominio Kerberos específico a un conjunto específico de servidores RADIUS cuando NPS se usa como proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Reglas de manipulación de atributos

Antes de que el mensaje RADIUS se procese localmente (cuando NPS se usa como un servidor RADIUS) o se reenvíe a otro servidor RADIUS (cuando NPS se usa como proxy RADIUS), el atributo de nombre de usuario del mensaje se puede modificar mediante reglas de manipulación de atributos. Para configurar las reglas de manipulación de atributos para el atributo de nombre de usuario, seleccione **nombre de usuario** en la pestaña **condiciones** en las propiedades de una directiva de solicitud de conexión. Las reglas de manipulación de atributos NPS usan la sintaxis de expresiones regulares.

Puede configurar reglas de manipulación de atributos para que el atributo de nombre de usuario cambie lo siguiente:

- Quite el nombre de dominio Kerberos del nombre de usuario \(also conocido como eliminación de dominio Kerberos \ no__t-1. Por ejemplo, el nombre de usuario user1@example.com cambia a user1.

- Cambiar el nombre de dominio Kerberos, pero no su sintaxis. Por ejemplo, el nombre de usuario user1@example.com cambia a user1@wcoast.example.com.

- Cambiar la sintaxis del nombre de dominio Kerberos. Por ejemplo, el nombre de usuario example\user1 se cambia a user1@example.com.

Una vez modificado el atributo de nombre de usuario de acuerdo con las reglas de manipulación de atributos que configure, se usará la configuración adicional de la primera Directiva de solicitud de conexión coincidente para determinar si:

- NPS procesa el mensaje de solicitud de acceso localmente (cuando se usa NPS como un servidor RADIUS).

- NPS reenvía el mensaje a otro servidor RADIUS (cuando se usa NPS como proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configuración del nombre de dominio proporcionado por NPS

Cuando el nombre de usuario no contiene un nombre de dominio, NPS proporciona uno. De forma predeterminada, el nombre de dominio proporcionado por NPS es el dominio del que el NPS es miembro. Puede especificar el nombre de dominio proporcionado por NPS mediante la siguiente configuración del registro:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>la modificación incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

Algunos servidores de acceso a la red que no son de Microsoft eliminan o modifican el nombre de dominio según lo especificado por el usuario. Como resultado, la solicitud de acceso a la red se autentica en el dominio predeterminado, que podría no ser el dominio de la cuenta del usuario. Para resolver este problema, configure los servidores RADIUS para cambiar el nombre de usuario al formato correcto con el nombre de dominio exacto.
