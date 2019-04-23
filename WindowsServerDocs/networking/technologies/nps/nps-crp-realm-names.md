---
title: Nombres de dominio kerberos
description: En este tema se proporciona información general del uso de nombres de dominio Kerberos en la solicitud de conexión de servidor de directivas de red de procesamiento en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0257c4d15db4fc54e55ef430f6f2eea9cea2ec4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882826"
---
# <a name="realm-names"></a>Nombres de dominio kerberos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016


Puede usar este tema para obtener información general del uso de nombres de dominio Kerberos en el procesamiento de solicitudes de conexión de servidor de directivas de red.

El atributo RADIUS de nombre de usuario es una cadena de caracteres que normalmente contiene una ubicación de la cuenta de usuario y un nombre de cuenta de usuario. La ubicación de la cuenta de usuario también se denomina dominio Kerberos o nombre de dominio Kerberos y es sinónimo del concepto de dominio, incluidos los dominios DNS, Active Directory® dominios y dominios de Windows NT 4.0. Por ejemplo, si una cuenta de usuario se encuentra en la base de datos de cuentas de usuario para un dominio llamado ejemplo.com, ejemplo.com es el nombre de dominio Kerberos.

En otro ejemplo, si el atributo RADIUS de nombre de usuario contiene el nombre de usuario user1@example.com, Usuario1 es el nombre de cuenta de usuario y ejemplo.com es el nombre de dominio Kerberos. Nombres de dominio Kerberos se pueden presentar en el nombre de usuario como un prefijo o sufijo:

- **Ejemplo\usuario1**. En este ejemplo, el nombre de dominio Kerberos **ejemplo** es un prefijo; y también es el nombre de Active Directory&reg; servicios de dominio \(AD DS\) dominio.

- **user1@example.com**. En este ejemplo, el nombre de dominio Kerberos **ejemplo.com** es un sufijo y es un nombre de dominio DNS o el nombre de un dominio de AD DS.

Puede usar nombres de dominio Kerberos configurados en las directivas de solicitud de conexión al diseñar e implementar la infraestructura de RADIUS para asegurarse de que se enrutan las solicitudes de conexión de clientes RADIUS, también denominados servidores de acceso de red, a servidores RADIUS que puede autenticar y autorizar la solicitud de conexión.

Cuando se configura NPS como servidor RADIUS con la directiva de solicitud de conexión de forma predeterminada, NPS procesa las solicitudes de conexión para el dominio en que el NPS es miembro y para dominios de confianza.

Para configurar NPS para que actúe como proxy RADIUS y reenviar solicitudes de conexión a dominios de confianza, debe crear una nueva directiva de solicitud de conexión. En la nueva directiva de solicitud de conexión, debe configurar el atributo de nombre de usuario con el nombre de dominio Kerberos que se incluirán en el atributo de nombre de usuario de las solicitudes de conexión que desea reenviar. También debe configurar la directiva de solicitud de conexión con un grupo de servidores RADIUS remotos. La directiva de solicitud de conexión permite a NPS calcular qué solicitudes de conexión para reenviar a los grupos de servidores RADIUS remotos basados en la parte del dominio Kerberos del atributo de nombre de usuario.

## <a name="acquiring-the-realm-name"></a>Al adquirir el nombre de dominio Kerberos

Se proporciona la parte del nombre de dominio Kerberos del nombre de usuario cuando intentan las credenciales del usuario tipos basado en contraseña durante una conexión o cuando se configura un perfil de Connection Manager (CM) en el equipo del usuario para proporcionar el nombre de dominio Kerberos automáticamente.

Puede designar los usuarios de la red que proporcionen su nombre de dominio Kerberos cuando escriban sus credenciales durante los intentos de conexión de red.

Por ejemplo, puede requerir a los usuarios escribir su nombre de usuario, incluidos el nombre de cuenta de usuario y el nombre de dominio Kerberos en **nombre de usuario** en el **Connect** cuadro de diálogo al realizar una red privada telefónico o virtual (VPN) conexión.

Además, si crea un paquete de marcado personalizado con el Kit de administración de Connection Manager (CMAK), puede ayudar a los usuarios al agregar el nombre de dominio Kerberos automáticamente al nombre de cuenta de usuario en los perfiles de CM que están instalados en equipos de los usuarios. Por ejemplo, puede especificar una sintaxis de nombre de usuario y el nombre de dominio Kerberos en el perfil de CM para que el usuario solo tiene que especificar el nombre de cuenta de usuario al escribir las credenciales. En este caso, el usuario no es necesario saber o recordar el dominio donde se encuentra su cuenta de usuario.

Durante el proceso de autenticación después de que los usuarios escriban sus credenciales basadas en contraseña, el nombre de usuario se pasa desde el cliente de acceso al servidor de acceso de red. El servidor de acceso de red crea una solicitud de conexión e incluye el nombre de dominio Kerberos dentro del atributo RADIUS de nombre de usuario en el mensaje de solicitud de acceso que se envía al servidor o proxy RADIUS.

Si el servidor RADIUS es un NPS, se evalúa el mensaje de solicitud de acceso con el conjunto de directivas de solicitud de conexión configuradas. Las condiciones en la directiva de solicitud de conexión pueden incluir la especificación del contenido del atributo de nombre de usuario.

Puede configurar un conjunto de directivas de solicitud de conexión que son específicas para el nombre de dominio Kerberos dentro del atributo de nombre de usuario de los mensajes entrantes. Esto le permite crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de dominio Kerberos específico a un conjunto específico de servidores RADIUS cuando NPS se usa como proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Reglas de manipulación de atributos

Antes de que el mensaje RADIUS se procese localmente (cuando se usa NPS como servidor RADIUS) o se reenvíen a otro servidor RADIUS (cuando se usa NPS como proxy RADIUS), el atributo de nombre de usuario en el mensaje puede modificarse mediante reglas de manipulación de atributos. Puede configurar reglas de manipulación de atributos para el atributo de nombre de usuario seleccionando **nombre de usuario** en el **condiciones** ficha en las propiedades de una directiva de solicitud de conexión. Reglas de manipulación de atributos NPS usan sintaxis de expresiones regulares.

Puede configurar reglas de manipulación de atributos para el atributo de nombre de usuario cambiar lo siguiente:

- Quitar el nombre de dominio Kerberos del nombre de usuario \(también se conoce como eliminación de dominio Kerberos\). Por ejemplo, el nombre de usuario user1@example.com cambia a usuario1.

- Cambiar el nombre de dominio Kerberos pero no su sintaxis. Por ejemplo, el nombre de usuario user1@example.com cambia a user1@wcoast.example.com.

- Cambiar la sintaxis del nombre de dominio Kerberos. Por ejemplo, el nombre de usuario ejemplo\usuario1 cambia a user1@example.com.

Después de modifica el atributo de nombre de usuario según las reglas de manipulación de atributos que se configuración, la configuración adicional de la primera directiva de solicitud de conexión coincidente se usa para determinar si:

- NPS procesa el mensaje de solicitud de acceso localmente (cuando se usa NPS como servidor RADIUS).

- NPS reenvía el mensaje a otro servidor RADIUS (cuando se usa NPS como proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configurar el nombre de dominio suministrado por NPS

Cuando el nombre de usuario no tiene un nombre de dominio, NPS suministra uno. De forma predeterminada, el nombre de dominio suministrado por NPS es el dominio del que el NPS es un miembro. Puede especificar el nombre de dominio suministrado por NPS a través de la configuración del registro siguiente:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>la modificación incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

Algunos servidores de acceso de red que no son de Microsoft se eliminación o modifique el nombre de dominio según lo especificado por el usuario. Como resultado, la solicitud de acceso de red se autentica en el dominio predeterminado, que puede no ser el dominio para la cuenta de usuario. Para resolver este problema, configure los servidores RADIUS para cambiar el nombre de usuario en el formato correcto con el nombre de dominio exacto.
