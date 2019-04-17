---
title: Nombres de dominio Kerberos
description: Este tema proporciona una descripción general del uso de nombres de dominio Kerberos en la solicitud de conexión de servidor de directivas de red en Windows Server 2016 de procesamiento.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29a835c9965c2115d0aac1ef9b704e05f430db8b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="realm-names"></a>Nombres de dominio Kerberos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016


Puedes usar este tema para obtener información general del uso de nombres de dominio en el procesamiento de solicitudes de conexión de servidor de directivas de red.

El atributo RADIUS de nombre de usuario es una cadena de caracteres que suele incluir una ubicación de la cuenta de usuario y el nombre de cuenta de usuario. La ubicación de la cuenta de usuario también se denomina territorio o nombre de dominio Kerberos y es sinónimo del concepto de dominio, incluidos los dominios DNS, Active Directory® dominios y Windows NT 4.0. Por ejemplo, si una cuenta de usuario se encuentra en la base de datos de cuentas de usuario para un dominio denominado example.com, example.com es el nombre de dominio Kerberos.

En otro ejemplo, si el atributo de nombre de usuario RADIUS contiene el nombre de usuario user1@example.com, user1 es el nombre de cuenta de usuario y example.com es el nombre de dominio Kerberos. Los nombres de dominio Kerberos pueden presentarse en el nombre de usuario como un prefijo o como un sufijo:

- **Example\user1**. En este ejemplo, el nombre de dominio Kerberos **ejemplo** es un prefijo; y también es el nombre de Active Directory&reg; dominio \(AD DS\) de servicios de dominio.

- **user1@example.com**. En este ejemplo, el nombre de dominio Kerberos **example.com** es un sufijo; y es un nombre de dominio DNS o el nombre de un dominio de AD DS.

Puedes usar nombres de dominio Kerberos configurados en las directivas de la solicitud de conexión al diseñar e implementar la infraestructura de RADIUS para garantizar que las solicitudes de conexión se enrutan desde los clientes RADIUS, también denominados servidores de acceso de red, con los servidores RADIUS que pueden autenticar y autorizar la solicitud de conexión.

Cuando se configura NPS como un servidor RADIUS con la directiva predeterminada de solicitud de conexión, NPS procesa las solicitudes de conexión para que el servidor NPS es un miembro del dominio y para dominios de confianza.

Para configurar NPS para que actúe como un proxy RADIUS y solicitudes de conexión directa a los dominios de confianza, debes crear una nueva directiva de solicitud de conexión. En la nueva directiva de solicitud de conexión, debes configurar el atributo de nombre de usuario con el nombre de dominio que se incluirán en el atributo de nombre de usuario de las solicitudes de conexión que quieras reenviar. También debes configurar la directiva de solicitud de conexión con un grupo de servidores RADIUS remotos. La directiva de solicitud de conexión permite NPS calcular qué solicitudes de conexión para reenviar a los grupos de servidores RADIUS remotos en función de la parte de territorio del atributo de nombre de usuario.

## <a name="acquiring-the-realm-name"></a>Adquirir el nombre de dominio Kerberos

La parte del nombre de dominio del nombre de usuario se proporciona cuando intentan las credenciales del usuario tipos basada en contraseña durante una conexión o cuando se configura un perfil de Connection Manager (CM) en el equipo del usuario para proporcionar el nombre de dominio Kerberos automáticamente.

Puede designar a los usuarios de la red que proporcionen su nombre de dominio Kerberos al escribir sus credenciales durante los intentos de conexión de red.

Por ejemplo, pueden requerir que los usuarios escribir su nombre de usuario, incluidos el nombre de cuenta de usuario y el nombre de dominio Kerberos, en **nombre de usuario** en la **conectar** cuadro de diálogo al realizar una conexión de acceso telefónico o red privada virtual (VPN).

Además, si creas un paquete de marcado personalizado con el Kit de administración de Connection Manager (CMAK), puede ayudar a los usuarios agregando el nombre de dominio Kerberos automáticamente en el nombre de cuenta de usuario en los perfiles de CM que están instalados en equipos de los usuarios. Por ejemplo, puedes especificar una sintaxis de nombre de usuario y el nombre de dominio Kerberos en el perfil CM para que el usuario solo tiene que especificar el nombre de cuenta al escribir credenciales. En este caso, el usuario no tiene que saber o recordar el dominio donde se encuentra su cuenta de usuario.

Durante el proceso de autenticación, después de que los usuarios escribir sus credenciales basada en contraseña, el nombre de usuario se pasa desde el cliente acceso al servidor de acceso de red. El servidor de acceso de red crea una solicitud de conexión e incluye el nombre de dominio dentro del atributo de radio de nombre de usuario en el mensaje de solicitud de acceso que se envía al servidor proxy RADIUS o.

Si el servidor RADIUS es un servidor NPS, el mensaje de solicitud de acceso se evalúa el conjunto de directivas de la solicitud de conexión configurada. Las condiciones en la directiva de solicitud de conexión pueden incluir la especificación del contenido del atributo de nombre de usuario.

Puedes configurar un conjunto de directivas de la solicitud de conexión que son específicas para el nombre de dominio en el atributo de nombre de usuario de los mensajes entrantes. Esto te permite crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de territorio específico a un conjunto específico de servidores RADIUS cuando NPS se usa como un proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Reglas de manipulación de atributo

Antes de que el mensaje RADIUS se procese localmente (cuando se usa como un servidor RADIUS NPS) o se reenvíe a otro servidor RADIUS (cuando NPS se usa como un proxy RADIUS), el atributo de nombre de usuario en el mensaje puede modificar las reglas de manipulación de atributo. Puedes configurar las reglas de manipulación de atributo para el atributo de nombre de usuario seleccionando **nombre de usuario** en la **condiciones** ficha en las propiedades de una directiva de solicitud de conexión. Reglas de manipulación de atributos NPS usan la sintaxis de expresión regular.

Puedes configurar las reglas de manipulación de atributo para el atributo de nombre de usuario cambiar las siguientes acciones:

- Quite el nombre de usuario en el nombre de dominio Kerberos \ (también conocido como territorio stripping\). Por ejemplo, el nombre de usuario user1@example.comha cambiado a user1.

- Cambiar el nombre de dominio, pero no su sintaxis. Por ejemplo, el nombre de usuario user1@example.comse cambia a user1@wcoast.example.com.

- Cambiar la sintaxis del nombre de dominio Kerberos. Por ejemplo, se cambia la example\user1 de nombre de usuario a user1@example.com.

Después de modifica según las reglas de manipulación de atributo que configura el atributo de nombre de usuario, configuración adicional de la primera coincidencia directiva de solicitud de conexión se usa para determinar si:

- El servidor NPS procesa el mensaje de solicitud de acceso localmente (cuando se usa como un servidor RADIUS NPS).

- El servidor NPS reenvía el mensaje a otro servidor RADIUS (cuando NPS se usa como un proxy de radio).

## <a name="configuring-the-the-nps-supplied-domain-name"></a>Configurar el el nombre de dominio proporcionado NPS

Cuando el nombre de usuario no contiene un nombre de dominio, NPS proporciona uno. De manera predeterminada, el nombre de dominio proporcionado NPS es el que el servidor NPS es un miembro de dominio. Puedes especificar el nombre de dominio proporcionado NPS mediante la siguiente configuración del registro:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>Edición incorrecta del registro puede dañar gravemente el sistema. Antes de realizar cambios en el registro, debe hacer una copia de los datos importantes en el equipo.

Algunos servidores de acceso de red no son de Microsoft eliminarán o modificar el nombre de dominio especificado por el usuario. Como resultado, la solicitud de acceso de red se autentica con el dominio predeterminado, que puede no ser el dominio de la cuenta del usuario. Para resolver este problema, configurar los servidores RADIUS para cambiar el nombre de usuario en el formato correcto con el nombre de dominio precisa.
