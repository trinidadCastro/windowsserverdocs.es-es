---
title: Nombres de dominio kerberos
description: En este tema se proporciona información general sobre el uso de nombres de dominio Kerberos en el procesamiento de solicitudes de conexión del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7a855087647b86486eaf5358e0e713d6fab6dd02
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517880"
---
# <a name="realm-names"></a>Nombres de dominio kerberos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información general sobre el uso de nombres de dominio Kerberos en el procesamiento de solicitudes de conexión del servidor de directivas de redes.

El atributo RADIUS de nombre de usuario es una cadena de caracteres que normalmente contiene una ubicación de cuenta de usuario y un nombre de cuenta de usuario. La ubicación de cuenta de usuario también se denomina dominio kerberos o nombre de dominio kerberos, y es sinónimo del concepto de dominio, incluidos los dominios DNS, los dominios de Active Directory® y los dominios de Windows NT 4.0. Por ejemplo, si una cuenta de usuario está ubicada en la base de datos de cuentas de usuarios para un dominio llamado ejemplo.com, ejemplo.com es el nombre de dominio kerberos.

En otro ejemplo, si el atributo RADIUS de nombre de usuario contiene el nombre de usuario user1@example.com , user1 es el nombre de la cuenta de usuario y example.com es el nombre de dominio Kerberos. Los nombres de dominio kerberos se pueden presentar en el nombre de usuario como un prefijo o como un sufijo:

- **Example\user1**. En este ejemplo, el **ejemplo** de nombre de dominio Kerberos es un prefijo; Además, es el nombre de un dominio de &reg; AD DS de Active Directory Domain Services \( \) .

- <strong>user1@example.com</strong>. En este ejemplo, el nombre de dominio Kerberos **example.com** es un sufijo; y es un nombre de dominio DNS o el nombre de un dominio de AD DS.

Puede usar los nombres de dominio kerberos configurados en las directivas de solicitud de conexión mientras diseña e implementa la infraestructura de RADIUS para garantizar que las solicitudes de conexión se enrutan desde los clientes RADIUS, llamados también servidores de acceso a la red, a servidores RADIUS que pueden autenticar y autorizar la solicitud de conexión.

Cuando NPS se configura como un servidor RADIUS con la Directiva de solicitud de conexión predeterminada, NPS procesa las solicitudes de conexión para el dominio en el que el NPS es miembro y para dominios de confianza.

Para configurar NPS para que actúe como proxy RADIUS y reenvíe las solicitudes de conexión a dominios que no son de confianza, debe crear una directiva de solicitud de conexión nueva. En la nueva directiva de solicitud de conexión, debe configurar el atributo de nombre de usuario con el nombre de dominio kerberos que se incluirá en el atributo de nombre de usuario para las solicitudes de conexión que desea reenviar. Además, debe configurar la directiva de solicitud de conexión con un grupo de servidores RADIUS remoto. La directiva de solicitud de conexión permite a NPS calcular las solicitudes de conexión que se reenvían al grupo de servidores RADIUS remoto según la parte del dominio kerberos del atributo de nombre de usuario.

## <a name="acquiring-the-realm-name"></a>Adquisición del nombre de dominio kerberos

La parte del nombre de dominio kerberos incluida en el nombre de usuario se suministra cuando el usuario escribe las credenciales basadas en contraseña durante un intento de conexión o cuando se configura un perfil de Connection Manager (CM) en el equipo del usuario para proporcionar el nombre de dominio kerberos automáticamente.

Puede designar que los usuarios de la red suministren el nombre de dominio kerberos cuando escriban sus credenciales en los intentos de conexión a la red.

Por ejemplo, puede exigir que los usuarios escriban su nombre de usuario, incluido el nombre de la cuenta de usuario y el nombre de dominio Kerberos, en **nombre de usuario** , en el cuadro de diálogo **conectar** al crear una conexión de acceso telefónico o de red privada virtual (VPN).

Además, si crea un paquete personalizado de conexiones de acceso telefónico con el Kit de administración de Connection Manager (CMAK), puede ayudar a los usuarios al agregar el nombre de dominio kerberos automáticamente al nombre de la cuenta de usuario en los perfiles de CM que están instalados en equipos de usuarios. Por ejemplo, puede especificar una sintaxis de nombre de dominio kerberos y nombre de usuario en el perfil de CM de forma que el usuario sólo tenga que indicar el nombre de la cuenta de usuario al escribir las credenciales. En este caso, el usuario no necesita saber o recordar el dominio donde está ubicada la cuenta de usuario.

Durante el proceso de autenticación, después de que los usuarios escriban las credenciales basadas en contraseña, el nombre de usuario pasa del cliente de acceso al servidor de acceso a la red. El servidor de acceso a la red crea una solicitud de conexión e incluye el nombre de dominio kerberos dentro del atributo RADIUS de nombre de usuario en el mensaje de solicitud de acceso que se envía al servidor o al proxy RADIUS.

Si el servidor RADIUS es un NPS, el mensaje de solicitud de acceso se evalúa con el conjunto de directivas de solicitud de conexión configuradas. Las condiciones de la directiva de solicitud de conexión pueden incluir la especificación del contenido del atributo de nombre de usuario.

Puede configurar un conjunto de directivas de solicitud de conexión que sean específicas del nombre de dominio kerberos dentro del atributo de nombre de usuario de los mensajes entrantes. Esto permite crear reglas de enrutamiento que reenvían mensajes RADIUS con un nombre de dominio kerberos específico a un conjunto determinado de servidores RADIUS cuando se usa NPS como proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Reglas de manipulación de atributos

Antes de que el mensaje RADIUS se procese localmente (cuando se usa NPS como servidor RADIUS) o se reenvíe a otro servidor RADIUS (cuando se usa NPS como proxy RADIUS), el atributo de nombre de usuario incluido en el mensaje se puede modificar mediante reglas de manipulación de atributos. Para configurar las reglas de manipulación de atributos para el atributo de nombre de usuario, seleccione **nombre de usuario** en la pestaña **condiciones** en las propiedades de una directiva de solicitud de conexión. Las reglas de manipulación de atributos de NPS usan una sintaxis de expresión regular.

Puede configurar reglas de manipulación de atributos para el atributo de nombre de usuario y cambiar lo siguiente:

- Quite el nombre de dominio Kerberos del nombre de usuario \( , también conocido como eliminación de dominio Kerberos \) . Por ejemplo, el nombre de usuario user1@example.com se cambia a user1.

- Cambiar el nombre de dominio kerberos pero no la sintaxis. Por ejemplo, el nombre de usuario user1@example.com se cambia a user1@wcoast.example.com .

- Cambiar la sintaxis del nombre de dominio kerberos. Por ejemplo, el nombre de usuario example\user1 se cambia a user1@example.com .

Una vez que el atributo de nombre de usuario se modifica según las reglas de manipulación de atributos que ha configurado, la configuración adicional de la primera directiva de solicitud de conexión coincidente se usa para determinar si:

- NPS procesa el mensaje de solicitud de acceso localmente (cuando se usa NPS como un servidor RADIUS).

- NPS reenvía el mensaje a otro servidor RADIUS (cuando se usa NPS como proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configuración del nombre de dominio proporcionado por NPS

Cuando el nombre de usuario no contiene ningún nombre de dominio, NPS suministra uno. De forma predeterminada, el nombre de dominio proporcionado por NPS es el dominio del que el NPS es miembro. Puede especificar el nombre de dominio suministrado por NPS mediante la siguiente configuración del Registro:

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
```

> [!CAUTION]
> Una modificación incorrecta del Registro puede provocar daños graves en el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

Algunos servidores de acceso a la red que no son de Microsoft eliminan o modifican el nombre de dominio según especifica el usuario. Por eso, la solicitud de acceso a la red se autentica con el dominio predeterminado, que es posible que no sea el dominio para la cuenta de usuario. Para resolver este problema, configure los servidores RADIUS para cambiar el nombre de usuario al formato correcto con el nombre de dominio exacto.
